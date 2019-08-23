---
title: .NET (Önizleme) ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturma-Azure depolama
description: .NET istemci kitaplığını kullanarak Azure Storage 'da Azure Active Directory kimlik bilgilerini kullanarak bir Kullanıcı temsili SAS oluşturmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/12/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 98ab93bbec8da17dde93c9c343703838b0279994
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900440"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-net-preview"></a>.NET ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturma (Önizleme)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, [.net Için Azure depolama istemci kitaplığı](https://www.nuget.org/packages/Azure.Storage.Blobs)ile bir kapsayıcı veya blob için Kullanıcı temsili SAS oluşturmak üzere Azure Active Directory (Azure AD) kimlik bilgilerinin nasıl kullanılacağı gösterilmektedir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-preview-packages"></a>Önizleme paketlerini yükler

Bu makaledeki örneklerde, BLOB depolama için Azure Storage istemci kitaplığı 'nın en son önizleme sürümü kullanılmaktadır. Önizleme paketini yüklemek için, NuGet paket yöneticisi konsolundan aşağıdaki komutu çalıştırın:

```
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

Bu makaledeki örneklerde, Azure AD kimlik bilgileriyle kimlik doğrulaması yapmak için [.net Için Azure Identity istemci kitaplığı](https://www.nuget.org/packages/Azure.Identity/) 'nın en son önizleme sürümü de kullanılır. Azure Identity istemci kitaplığı bir güvenlik sorumlusunun kimliğini doğrular. Kimliği doğrulanmış güvenlik sorumlusu, daha sonra Kullanıcı temsili SAS oluşturabilir. Azure Identity istemci kitaplığı hakkında daha fazla bilgi için bkz. [.net Için Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

```
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Azure Identity istemci kitaplığı aracılığıyla Azure AD kimlik bilgileriyle kimlik doğrulaması yapmak için, kodunuzun çalıştığı yere bağlı olarak, güvenlik sorumlusu olarak bir hizmet sorumlusu ya da yönetilen kimlik kullanın. Kodunuz bir geliştirme ortamında çalışıyorsa, sınama amacıyla bir hizmet sorumlusu kullanın. Kodunuz Azure 'da çalışıyorsa, yönetilen bir kimlik kullanın. Bu makalede, geliştirme ortamından kod çalıştırdığınız ve Kullanıcı temsilciliğini oluşturmak için hizmet sorumlusunun nasıl kullanılacağı gösterilmektedir.

Azure CLı ile bir hizmet sorumlusu oluşturmak ve RBAC rolü atamak için, [az ad SP Create-for-RBAC](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) komutunu çağırın. Yeni hizmet sorumlusuna atamak için bir Azure depolama veri erişim rolü sağlayın. Rol, **Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içermelidir. Azure depolama için sunulan yerleşik roller hakkında daha fazla bilgi için bkz. [Azure kaynakları Için yerleşik roller](../../role-based-access-control/built-in-roles.md).

Ayrıca, rol atamasının kapsamını belirtin. Hizmet sorumlusu, depolama hesabı düzeyinde gerçekleştirilen bir işlem olan Kullanıcı temsili anahtarını oluşturur, bu nedenle rol atamasının depolama hesabı, kaynak grubu veya abonelik düzeyinde kapsamı belirlenmiş olması gerekir. Kullanıcı temsilciliğini oluşturmaya yönelik RBAC izinleri hakkında daha fazla bilgi için, [Kullanıcı temsilcileri oluşturma SAS (REST API)](/rest/api/storageservices/create-user-delegation-sas)konusunun **RBAC ile izin atama** bölümüne bakın.

Hizmet sorumlusuna rol atamak için yeterli izniniz yoksa, hesap sahibine veya yöneticiden rol atamasını gerçekleştirmesini isteyebilirsiniz.

Aşağıdaki örnek, yeni bir hizmet sorumlusu oluşturmak ve **Depolama Blobu veri okuyucusu** rolünü hesap kapsamı ile atamak IÇIN Azure CLI 'yi kullanır

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Komut `az ad sp create-for-rbac` , JSON biçiminde hizmet sorumlusu özelliklerinin bir listesini döndürür. Bir sonraki adımda gerekli ortam değişkenlerini oluşturmak için bunları kullanabilmeniz için bu değerleri kopyalayın.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```

> [!IMPORTANT]
> RBAC rol atamalarının yayılması birkaç dakika sürebilir.

## <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Azure Identity istemci kitaplığı, hizmet sorumlusunun kimliğini doğrulamak için çalışma zamanında üç ortam değişkeninden değerleri okur. Aşağıdaki tabloda her ortam değişkeni için ayarlanacak değer açıklanmaktadır.

|Ortam değişkeni|Value
|-|-
|`AZURE_CLIENT_ID`|Hizmet sorumlusu için uygulama KIMLIĞI
|`AZURE_TENANT_ID`|Hizmet sorumlusunun Azure AD kiracı KIMLIĞI
|`AZURE_CLIENT_SECRET`|Hizmet sorumlusu için oluşturulan parola

> [!IMPORTANT]
> Ortam değişkenlerini ayarladıktan sonra konsol pencerenizi kapatıp yeniden açın. Visual Studio veya başka bir geliştirme ortamı kullanıyorsanız, yeni ortam değişkenlerini kaydedebilmesi için geliştirme ortamını yeniden başlatmanız gerekebilir.

## <a name="add-using-directives"></a>Using yönergeleri ekleme

Azure Identity ve `using` Azure Storage istemci kitaplıklarının önizleme sürümlerini kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

## <a name="authenticate-the-service-principal"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusunun kimliğini doğrulamak için [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) sınıfının bir örneğini oluşturun. `DefaultAzureCredential` Oluşturucu, daha önce oluşturduğunuz ortam değişkenlerini okur.

Aşağıdaki kod parçacığı kimliği doğrulanmış kimlik bilgisinin nasıl alınacağını ve BLOB depolaması için bir hizmet istemcisi oluşturmak üzere nasıl kullanılacağını gösterir

```csharp
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Kullanıcı temsili anahtarını al

Her SAS bir anahtarla imzalanır. Bir Kullanıcı temsili SAS oluşturmak için, önce SAS imzalamak için kullanılan bir Kullanıcı temsili anahtarı istemeniz gerekir. Kullanıcı temsili anahtarı, Azure AD kimlik bilgilerinizi temel aldığından, bir hizmet SAS 'sini veya bir hesap SAS 'sini imzalamak için kullanılan hesap anahtarına benzer. İstemci bir OAuth 2,0 belirteci kullanarak bir Kullanıcı temsili anahtarı istediğinde, Azure depolama Kullanıcı adına Kullanıcı temsili anahtarını döndürür.

Kullanıcı temsilciliğini aldıktan sonra, anahtarın ömrü boyunca herhangi bir sayıda kullanıcı temsili paylaşılan erişim imzası oluşturmak için bu anahtarı kullanabilirsiniz. Kullanıcı temsili anahtarı, bunu elde etmek için kullanılan OAuth 2,0 belirtecinden bağımsızdır, bu nedenle anahtar hala geçerli olduğu sürece belirtecin yenilenmesi gerekmez. Anahtarın 7 güne kadar bir dönem için geçerli olduğunu belirtebilirsiniz.

Kullanıcı temsili anahtarını istemek için aşağıdaki yöntemlerden birini kullanın:

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

Aşağıdaki kod parçacığı, Kullanıcı temsili anahtarını alır ve özelliklerini Yazar:

```csharp
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStart);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="create-the-sas-token"></a>SAS belirteci oluşturma

Aşağıdaki kod parçacığı, yeni bir [Blobsasbuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) oluşturma ve Kullanıcı temsili SAS için parametreler sağlama gösterilmektedir. Kod parçacığı daha sonra SAS belirteç dizesini almak için [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) çağırır. Son olarak, kod kaynak adresi ve SAS belirteci dahil olmak üzere tüm URI 'yi oluşturur.

```csharp
BlobSasBuilder builder = new BlobSasBuilder()
{
    ContainerName = containerName,
    BlobName = blobName,
    Permissions = "r",
    Resource = "b",
    StartTime = DateTimeOffset.UtcNow,
    ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
};

string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

UriBuilder fullUri = new UriBuilder()
{
    Scheme = "https",
    Host = string.Format("{0}.blob.core.windows.net", accountName),
    Path = string.Format("{0}/{1}", containerName, blobName),
    Query = sasToken
};
```

## <a name="example-get-a-user-delegation-sas"></a>Örnek: Kullanıcı temsilciliğini al SAS

Aşağıdaki örnek yöntemi, güvenlik sorumlusunun kimliğini doğrulamak ve Kullanıcı temsili SAS oluşturmak için tüm kodu gösterir:

```csharp
async static Task<Uri> GetUserDelegationSasBlob(string accountName, string containerName, string blobName)
{
    // Construct the blob endpoint from the account name.
    string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

    // Create a new Blob service client with Azure AD credentials.  
    BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint), 
                                                            new DefaultAzureCredential());

    // Get a user delegation key for the Blob service that's valid for seven days.
    // Use the key to generate any number of shared access signatures over the lifetime of the key.
    UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                       DateTimeOffset.UtcNow.AddDays(7));

    // Read the key's properties.
    Console.WriteLine("User delegation key properties:");
    Console.WriteLine("Key signed start: {0}", key.SignedStart);
    Console.WriteLine("Key signed expiry: {0}", key.SignedExpiry);
    Console.WriteLine("Key signed object ID: {0}", key.SignedOid);
    Console.WriteLine("Key signed tenant ID: {0}", key.SignedTid);
    Console.WriteLine("Key signed service: {0}", key.SignedService);
    Console.WriteLine("Key signed version: {0}", key.SignedVersion);

    // Create a SAS token that's valid a short interval.
    BlobSasBuilder sasBuilder = new BlobSasBuilder()
    {
        ContainerName = containerName,
        BlobName = blobName,
        Permissions = "r",
        Resource = "b",
        StartTime = DateTimeOffset.UtcNow,
        ExpiryTime = DateTimeOffset.UtcNow.AddMinutes(5)
    };

    // Use the key to get the SAS token.
    string sasToken = sasBuilder.ToSasQueryParameters(key, accountName).ToString();

    // Construct the full URI, including the SAS token.
    UriBuilder fullUri = new UriBuilder()
    {
        Scheme = "https",
        Host = string.Format("{0}.blob.core.windows.net", accountName),
        Path = string.Format("{0}/{1}", containerName, blobName),
        Query = sasToken
    };

    Console.WriteLine("User delegation SAS URI: {0}", fullUri);
    return fullUri.Uri;
}
```

## <a name="example-read-a-blob-with-a-user-delegation-sas"></a>Örnek: Kullanıcı temsilciliğini içeren bir blobu okuma

Aşağıdaki örnek, bir önceki örnekte oluşturulan kullanıcı temsili SAS 'ı sanal bir istemci uygulamasından sınar. SAS geçerliyse, istemci uygulama Blobun içeriğini okuyabilir. SAS geçersizse, örneğin süresi dolmuşsa Azure Storage 403 (yasak) hata kodunu döndürür.

```csharp
private static async Task ReadBlobWithSasAsync(Uri sasUri)
{
    // Try performing blob operations using the SAS provided.

    // Create a blob client object for blob operations.
    BlobClient blobClient = new BlobClient(sasUri, null);

    // Download and read the contents of the blob.
    try
    {
        // Download blob contents to a stream and read the stream.
        BlobDownloadInfo blobDownloadInfo = await blobClient.DownloadAsync();
        using (StreamReader reader = new StreamReader(blobDownloadInfo.Content, true))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }

        Console.WriteLine();
        Console.WriteLine("Read operation succeeded for SAS {0}", sasUri);
        Console.WriteLine();
    }
    catch (StorageRequestFailedException e)
    {
        // Check for a 403 (Forbidden) error. If the SAS is invalid, 
        // Azure Storage returns this error.
        if (e.Status == 403)
        {
            Console.WriteLine("Read operation failed for SAS {0}", sasUri);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Kullanıcı temsilciyi anahtar işlemi al](/rest/api/storageservices/get-user-delegation-key)
- [Kullanıcı temsilciliğini oluşturma SAS (REST API)](/rest/api/storageservices/create-user-delegation-sas)
