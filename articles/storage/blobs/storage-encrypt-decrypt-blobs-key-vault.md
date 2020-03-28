---
title: Öğretici - Azure Key Vault kullanarak blobları şifreleyin ve şifresini çöz
titleSuffix: Azure Storage
description: Azure Key Vault ile istemci tarafı şifrelemesini kullanarak bir blob'u nasıl şifreleyip şifrenizi çözeriz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: c83e56a47f4b212a5612cb9e6965ce8e73228dcb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74892898"
---
# <a name="tutorial---encrypt-and-decrypt-blobs-using-azure-key-vault"></a>Öğretici - Azure Key Vault kullanarak blobları şifreleyin ve şifresini çöz

Bu öğretici, Azure Key Vault ile istemci tarafı depolama şifrelemesinin nasıl kullanılacağını kapsar. Bu teknolojileri kullanarak bir konsol uygulamasında bir blob şifrelemek ve şifresini nasıl size yol.

**Tahmini tamamlanma süresi:** 20 dakika

Azure Anahtar Kasası hakkında genel bilgi [için](../../key-vault/key-vault-overview.md)bkz.

Azure Depolama için istemci tarafı şifreleme hakkında genel bilgi için Bkz. [Microsoft Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası.](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakilere sahip olmanız gerekir:

* Azure Depolama hesabı
* Visual Studio 2013 veya sonrası
* Azure PowerShell

## <a name="overview-of-client-side-encryption"></a>İstemci tarafı şifrelemeye genel bakış

Azure Depolama için istemci tarafı şifrelemeye genel bakış için, [Microsoft Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası'na](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) bakın

İstemci tarafı şifrelemesinin nasıl çalıştığına ilgili kısa bir açıklama aşağıda veda edebilirsiniz:

1. Azure Depolama istemcisi SDK, tek kullanımlık simetrik anahtar olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Müşteri verileri bu CEK kullanılarak şifrelenir.
3. CEK daha sonra anahtar şifreleme anahtarı (KEK) kullanılarak sarılır (şifrelenir). KEK, önemli bir tanımlayıcı tarafından tanımlanır ve asimetrik anahtar çifti veya simetrik anahtar olabilir ve yerel olarak yönetilebilir veya Azure Key Vault'ta depolanabilir. Depolama istemcisinin kendisi KEK'e hiçbir zaman erişememiştir. Sadece Key Vault tarafından sağlanan anahtar sarma algoritması çağırır. Müşteriler, isterlerse anahtar kaydırma/açma için özel sağlayıcılar kullanmayı seçebilirler.
4. Şifrelenmiş veriler daha sonra Azure Depolama hizmetine yüklenir.

## <a name="set-up-your-azure-key-vault"></a>Azure Anahtar Kasanızı Ayarlama

Bu öğreticiye devam etmek için Quickstart: Set'te özetlenen aşağıdaki adımları yapmanız ve [bir .NET web uygulamasını kullanarak Azure Key Vault'tan bir sır almanız ve ayarlamanız](../../key-vault/quick-create-net.md)gerekir:

* Bir anahtar kasası oluşturma.
* Anahtar kasasına bir anahtar veya sır ekleyin.
* Azure Active Directory ile bir uygulama kaydedin.
* Anahtarı veya sırrı kullanmak için başvuruyun yetkilendirin.

Azure Active Directory ile bir uygulama kaydederken oluşturulan ClientID ve ClientSecret'ı not edin.

Anahtar kasasında her iki anahtarı da oluşturun. Biz aşağıdaki isimleri kullandık öğretici geri kalanı için varsayalım: ContosoKeyVault ve TestRSAKey1.

## <a name="create-a-console-application-with-packages-and-appsettings"></a>Paketler ve AppSettings ile bir konsol uygulaması oluşturma

Visual Studio'da yeni bir konsol uygulaması oluşturun.

Paket Yöneticisi Konsoluna gerekli nuget paketlerini ekleyin.

```powershell
Install-Package Microsoft.Azure.ConfigurationManager
Install-Package Microsoft.Azure.Storage.Common
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

Install-Package Microsoft.Azure.KeyVault
Install-Package Microsoft.Azure.KeyVault.Extensions
```

App.Config'e AppSettings ekleyin.

```xml
<appSettings>
    <add key="accountName" value="myaccount"/>
    <add key="accountKey" value="theaccountkey"/>
    <add key="clientId" value="theclientid"/>
    <add key="clientSecret" value="theclientsecret"/>
    <add key="container" value="stuff"/>
</appSettings>
```

Aşağıdaki `using` yönergeleri ekleyin ve projeye System.Configuration'a bir başvuru eklediğinizden emin olun.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Configuration;
using Microsoft.Azure;
using Microsoft.Azure.Storage;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
using Microsoft.Azure.KeyVault;
using System.Threading;
using System.IO;
```

## <a name="add-a-method-to-get-a-token-to-your-console-application"></a>Konsol uygulamanıza bir belirteç almak için bir yöntem ekleme

Aşağıdaki yöntem, anahtar kasanıza erişmek için kimlik doğrulaması gereken Key Vault sınıfları tarafından kullanılır.

```csharp
private async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);
    ClientCredential clientCred = new ClientCredential(
        CloudConfigurationManager.GetSetting("clientId"),
        CloudConfigurationManager.GetSetting("clientSecret"));
    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)
        throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

## <a name="access-azure-storage-and-key-vault-in-your-program"></a>Programınızda Azure Depolama ve Anahtar Kasası'na erişin

Main() yönteminde aşağıdaki kodu ekleyin.

```csharp
// This is standard code to interact with Blob storage.
StorageCredentials creds = new StorageCredentials(
    CloudConfigurationManager.GetSetting("accountName"),
    CloudConfigurationManager.GetSetting("accountKey");
CloudStorageAccount account = new CloudStorageAccount(creds, useHttps: true);
CloudBlobClient client = account.CreateCloudBlobClient();
CloudBlobContainer contain = client.GetContainerReference(CloudConfigurationManager.GetSetting("container"));
contain.CreateIfNotExists();

// The Resolver object is used to interact with Key Vault for Azure Storage.
// This is where the GetToken method from above is used.
KeyVaultKeyResolver cloudResolver = new KeyVaultKeyResolver(GetToken);
```

> [!NOTE]
> Anahtar Kasa Nesne Modelleri
> 
> Aslında farkında olmak için iki Key Vault nesne modelleri olduğunu anlamak önemlidir: bir REST API (KeyVault ad alanı) ve diğer istemci tarafı şifreleme için bir uzantısıdır.
> 
> Key Vault İstemci REST API ile etkileşim ve Anahtar Vault bulunan şeyler iki tür için JSON Web Tuşları ve sırlarını anlar.
> 
> Anahtar Vault Uzantıları, Azure Depolama'da istemci tarafı şifrelemesi için özel olarak oluşturulmuş gibi görünen sınıflardır. Anahtarlar (IKey) ve Anahtar Çözümleyici kavramına dayalı sınıflar için bir arabirim içerirler. IKey bilmeniz gereken iki uygulama vardır: RSAKey ve SymmetricKey. Şimdi bir Key Vault bulunan şeyler ile çakışıyor, ama bu noktada bağımsız sınıflar (bu yüzden Anahtar Vault İstemci tarafından alınan Anahtar ve Gizli IKey uygulamaz).
> 
> 

## <a name="encrypt-blob-and-upload"></a>Blob'u şifreleme ve yükleme

Bir lekeyi şifrelemek ve Azure depolama hesabınıza yüklemek için aşağıdaki kodu ekleyin. Kullanılan **ResolveKeyAsync** yöntemi bir IKey döndürür.

```csharp
// Retrieve the key that you created previously.
// The IKey that is returned here is an RsaKey.
var rsa = cloudResolver.ResolveKeyAsync(
            "https://contosokeyvault.vault.azure.net/keys/TestRSAKey1", 
            CancellationToken.None).GetAwaiter().GetResult();

// Now you simply use the RSA key to encrypt by setting it in the BlobEncryptionPolicy.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(rsa, null);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

// Reference a block blob.
CloudBlockBlob blob = contain.GetBlockBlobReference("MyFile.txt");

// Upload using the UploadFromStream method.
using (var stream = System.IO.File.OpenRead(@"C:\Temp\MyFile.txt"))
    blob.UploadFromStream(stream, stream.Length, null, options, null);
```

> [!NOTE]
> BlobEncryptionPolicy oluşturucuya bakarsanız, bir anahtarı ve/veya çözümleyiciyi kabul edebileceğini görürsünüz. Şu anda varsayılan anahtarı desteklemediği için şifreleme için çözümleyici kullanamayacağınızı unutmayın.

## <a name="decrypt-blob-and-download"></a>Blob şifresini çözmek ve indir

Şifre çözme gerçekten çözümleyici sınıfları kullanarak mantıklı olmasıdır. Şifreleme için kullanılan anahtarın kimliği meta verilerindeki blob ile ilişkilidir, bu nedenle anahtarı almanız ve anahtar ile blob arasındaki ilişkiyi hatırlamanız için bir neden yoktur. Sadece anahtarın Key Vault'ta kaldığından emin olmalısın.   

RSA Anahtarının özel anahtarı Key Vault'ta kalır, bu nedenle şifre çözmenin gerçekleşmesi için CEK içeren blob meta verilerinden Şifrelenmiş Anahtar şifre çözme için Key Vault'a gönderilir.

Az önce yüklediğiniz blobun şifresini çözmek için aşağıdakileri ekleyin.

```csharp
// In this case, we will not pass a key and only pass the resolver because
// this policy will only be used for downloading / decrypting.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(null, cloudResolver);
BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

using (var np = File.Open(@"C:\data\MyFileDecrypted.txt", FileMode.Create))
    blob.DownloadToStream(np, null, options, null);
```

> [!NOTE]
> Anahtar yönetimini kolaylaştırmak için birkaç çözümleyici türü vardır: AggregateKeyResolver ve CachingKeyResolver.

## <a name="use-key-vault-secrets"></a>Key Vault sırlarını kullanma

Bir sırrı istemci tarafı şifrelemeyle kullanmanın yolu SymmetricKey sınıfı üzerindendir, çünkü bir sır aslında simetrik bir anahtardır. Ancak, yukarıda belirtildiği gibi, Key Vault bir sır tam olarak bir SymmetricKey eş değildir. Anlamak için birkaç şey vardır:

* SymmetricKey'deki anahtar sabit uzunlukta olmalıdır: 128, 192, 256, 384 veya 512 bit.
* SymmetricKey'deki anahtar Base64 kodlanmış olmalıdır.
* SymmetricKey olarak kullanılacak bir Key Vault sırrının Key Vault'ta "uygulama/sekizli akış" İçerik Türüne sahip olması gerekir.

Burada Bir Simetrik Anahtar olarak kullanılabilir Key Vault bir sır oluşturma PowerShell bir örnektir.
$key olan sabit kodlu değerin yalnızca gösteri amaçlı olduğunu lütfen unutmayın. Kendi kodunuzda bu anahtarı oluşturmak isteyeceksiniz.

```csharp
// Here we are making a 128-bit key so we have 16 characters.
//     The characters are in the ASCII range of UTF8 so they are
//    each 1 byte. 16 x 8 = 128.
$key = "qwertyuiopasdfgh"
$b = [System.Text.Encoding]::UTF8.GetBytes($key)
$enc = [System.Convert]::ToBase64String($b)
$secretvalue = ConvertTo-SecureString $enc -AsPlainText -Force

// Substitute the VaultName and Name in this command.
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'TestSecret2' -SecretValue $secretvalue -ContentType "application/octet-stream"
```

Konsol uygulamanızda, bu sırrı SymmetricKey olarak almak için öncekiyle aynı aramayı kullanabilirsiniz.

```csharp
SymmetricKey sec = (SymmetricKey) cloudResolver.ResolveKeyAsync(
    "https://contosokeyvault.vault.azure.net/secrets/TestSecret2/",
    CancellationToken.None).GetAwaiter().GetResult();
```

İşte bu kadar. Keyfini çıkarın!

## <a name="next-steps"></a>Sonraki adımlar

C# ile Microsoft Azure Depolama Alanı'nı kullanma hakkında daha fazla bilgi [için .NET için Microsoft Azure Depolama İstemci Kitaplığı'na](https://msdn.microsoft.com/library/azure/dn261237.aspx)bakın.

Blob REST API hakkında daha fazla bilgi için [Blob Service REST API'ye](https://msdn.microsoft.com/library/azure/dd135733.aspx)bakın.

Microsoft Azure Depolama ile ilgili en son bilgiler için [Microsoft Azure Depolama Ekibi Blog'una](https://blogs.msdn.com/b/windowsazurestorage/)gidin.
