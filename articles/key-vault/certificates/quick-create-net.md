---
title: Hızlı başlangıç-.NET için Azure Key Vault sertifikaları istemci kitaplığı (sürüm 4)
description: .NET istemci kitaplığı 'nı kullanarak bir Azure anahtar kasasından sertifika oluşturmayı, almayı ve silmeyi öğrenin (sürüm 4)
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/23/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: devx-track-csharp, devx-track-azurecli
ms.openlocfilehash: bbc90d0ba0bea0cd634602da2ebd4e07d6eadf0c
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96185173"
---
# <a name="quickstart-azure-key-vault-certificate-client-library-for-net-sdk-v4"></a>Hızlı başlangıç: .NET için Azure Key Vault sertifika istemci kitaplığı (SDK v4)

.NET için Azure Key Vault sertifika istemci kitaplığı ile çalışmaya başlayın. [Azure Key Vault](../general/overview.md) , sertifikalar için güvenli bir mağaza sağlayan bir bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer sertifikaları güvenli bir şekilde depolayabilirsiniz. Azure anahtar kasaları Azure portalı aracılığıyla oluşturulup yönetilebilir. Bu hızlı başlangıçta, .NET istemci kitaplığını kullanarak bir Azure anahtar kasasından sertifika oluşturmayı, almayı ve silmeyi öğreneceksiniz

Key Vault istemci kitaplığı kaynakları:

[API başvuru belgeleri](/dotnet/api/azure.security.keyvault.certificates)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.Security.KeyVault.Certificates/)

Key Vault ve sertifikaları hakkında daha fazla bilgi için bkz.:
- [Key Vault genel bakış](../general/overview.md)
- [Sertifikalara genel bakış](about-certificates.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/dotnet)
* [.NET Core 3,1 SDK veya üzeri](https://dotnet.microsoft.com/download/dotnet-core)
* [Azure CLI](/cli/azure/install-azure-cli)
* Key Vault- [Azure Portal](../general/quick-create-portal.md), [Azure CLI](../general/quick-create-cli.md)veya [Azure PowerShell](../general/quick-create-powershell.md)kullanarak bir tane oluşturabilirsiniz.

Bu hızlı başlangıç, `dotnet` Azure CLI 'yi kullanıyor

## <a name="setup"></a>Kurulum

Bu hızlı başlangıç, Azure 'da kullanıcının kimliğini doğrulamak için Azure CLı ile Azure Identity Library kullanıyor. Geliştiriciler, aramalarını doğrulamak için Visual Studio veya Visual Studio Code de kullanabilir. daha fazla bilgi için bkz. [Azure Identity istemci kitaplığı ile Istemci kimlik doğrulaması](/dotnet/api/overview/azure/identity-readme?#authenticate-the-client&preserve-view=true).

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

1. `login` komutunu çalıştırın.

    ```azurecli-interactive
    az login
    ```

    CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler.

    Aksi takdirde, konumunda bir tarayıcı sayfası açın [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ve terminalinizde görünen yetkilendirme kodunu girin.

2. Tarayıcıda hesabınızın kimlik bilgileriyle oturum açın.


### <a name="create-new-net-console-app"></a>Yeni .NET konsol uygulaması oluşturma

1. Bir komut kabuğunda, aşağıdaki komutu çalıştırarak adında bir proje oluşturun `key-vault-console-app` :

    ```dotnetcli
    dotnet new console --name key-vault-console-app
    ```

1. Yeni oluşturulan *Anahtar Kasası-konsol-uygulama* dizinine geçin ve projeyi derlemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

    Derleme çıktısı hiçbir uyarı veya hata içermemelidir.
    
    ```console
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ```

### <a name="install-the-packages"></a>Paketleri yükler

Komut kabuğundan, .NET için Azure Key Vault sertifikası istemci kitaplığı 'nı yükledikten sonra:

```dotnetcli
dotnet add package Azure.Security.KeyVault.Certificates
```

Bu hızlı başlangıçta Azure Identity için Azure SDK istemci kitaplığı 'nı yüklemeniz gerekir:

```dotnetcli
dotnet add package Azure.Identity
```

#### <a name="grant-access-to-your-key-vault"></a>Anahtar kasanıza erişim izni verin

Anahtar kasanız için Kullanıcı hesabınıza Sertifika izni veren bir erişim ilkesi oluşturun

```console
az keyvault set-policy --name <your-key-vault-name> --upn user@domain.com --certificate-permissions delete get list create purge
```

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Bu uygulama, Anahtar Kasası adını adında bir ortam değişkeni olarak kullanıyor `KEY_VAULT_NAME` .

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME=<your-key-vault-name>
```

macOS veya Linux
```bash
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Nesne modeli

.NET için Azure Key Vault sertifikası istemci kitaplığı, sertifikaları yönetmenizi sağlar. [Kod örnekleri](#code-examples) bölümünde, bir istemci oluşturma, sertifika ayarlama, sertifika alma ve sertifikayı silme işlemlerinin nasıl yapılacağı gösterilir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri *program.cs*' nin en üstüne ekleyin:

```csharp
using System;
using Azure.Identity;
using Azure.Security.KeyVault.Certificates;
```

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Bu hızlı başlangıçta oturum açan kullanıcı, yerel geliştirme için tercih edilen yöntem olan Anahtar Kasası kimlik doğrulaması için kullanılır. Azure 'a dağıtılan uygulamalar için, yönetilen kimlik App Service veya sanal makineye atanmalıdır, daha fazla bilgi için bkz. [yönetilen kimliğe genel bakış](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

Aşağıdaki örnekte, anahtar kasanızın adı, "https://. vault.azure.net" biçiminde Anahtar Kasası URI 'sine genişletilir \<your-key-vault-name\> . Bu örnek, kimlik sağlamak için farklı seçeneklere sahip farklı ortamlarda aynı kodun kullanılmasına izin veren  [' DefaultAzureCredential () '](/dotnet/api/azure.identity.defaultazurecredential) sınıfını kullanıyor. Anahtar Kasası kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Geliştirici Kılavuzu](https://docs.microsoft.com/azure/key-vault/general/developers-guide#authenticate-to-key-vault-in-code).

```csharp
string keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
var kvUri = "https://" + keyVaultName + ".vault.azure.net";

var client = new CertificateClient(new Uri(kvUri), new DefaultAzureCredential());
```

### <a name="save-a-certificate"></a>Sertifika kaydetme

Bu örnekte, kolaylık sağlaması için varsayılan verme ilkesiyle otomatik olarak imzalanan sertifika kullanabilirsiniz. Bu görev için [Startcreatecertificateasync](/dotnet/api/azure.security.keyvault.certificates.certificateclient.startcreatecertificateasync) yöntemini kullanın. Yöntemin ilk parametresi bir sertifika adı ve sertifika ilkesi[sertifika ilkesi](https://docs.microsoft.com/dotnet/api/azure.security.keyvault.certificates.certificatepolicy)kabul eder.

```csharp
CertificateOperation operation = await client.StartCreateCertificateAsync("MyCertificate", CertificatePolicy.Default);
var certificate = await operation.WaitForCompletionAsync();
```

> [!NOTE]
> Sertifika adı varsa, yukarıdaki kod bu sertifikanın yeni bir sürümünü oluşturur.

### <a name="retrieve-a-certificate"></a>Sertifika alma

Artık, önceden oluşturulan sertifikayı [Getcertificateasync](/dotnet/api/azure.security.keyvault.certificates.certificateclient.getcertificateasync) yöntemiyle alabilirsiniz.

```csharp
var certificate = await client.GetCertificateAsync("myCertificate");
```

### <a name="delete-a-certificate"></a>Sertifikayı silme

Son olarak, [Startdeletecertificateasync](/dotnet/api/azure.security.keyvault.certificates.certificateclient.startdeletecertificateasync) ve [PurgeDeletedCertificateAsync](/dotnet/api/azure.security.keyvault.certificates.certificateclient.purgedeletedcertificateasync)  yöntemleriyle anahtar kasaınızdan sertifikayı silip temizlim.

```csharp
var operation = await client.StartDeleteCertificateAsync("MyCertificate");

// You only need to wait for completion if you want to purge or recover the certificate.
await operation.WaitForCompletionAsync();

var certificate = operation.Value;
await client.PurgeDeletedCertificateAsync(certificate.Name);
```

## <a name="sample-code"></a>Örnek kod

Aşağıdaki adımları tamamlayarak, .NET Core konsol uygulamasını Key Vault etkileşimde bulunmak üzere değiştirin:

1. *Program.cs* içindeki kodu aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Azure.Identity;
    using Azure.Security.KeyVault.Certificates;
    
    namespace key_vault_console_app
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                const string certificateName = "myCertificate";
                var keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
                var kvUri = $"https://{keyVaultName}.vault.azure.net";
    
                var client = new CertificateClient(new Uri(kvUri), new DefaultAzureCredential());
    
                Console.Write($"Creating a certificate in {keyVaultName} called '{certificateName}' ...");
                CertificateOperation operation = await client.StartCreateCertificateAsync(certificateName, CertificatePolicy.Default);
                await operation.WaitForCompletionAsync();
                Console.WriteLine(" done.");
    
                Console.WriteLine($"Retrieving your certificate from {keyVaultName}.");
                var certificate = await client.GetCertificateAsync(certificateName);
                Console.WriteLine($"Your certificate value is '{certificate.Value.Properties.Version}'.");
    
                Console.Write($"Deleting your certificate from {keyVaultName} ...");
                DeleteCertificateOperation deleteOperation = await client.StartDeleteCertificateAsync(certificateName);
                // You only need to wait for completion if you want to purge or recover the certificate.
                await deleteOperation.WaitForCompletionAsync();
                Console.WriteLine(" done.");

                Console.Write($"Purging your certificate from {keyVaultName} ...");
                await client.PurgeDeletedCertificateAsync(certificateName);
                Console.WriteLine(" done.");
            }
        }
    }
    ```
### <a name="test-and-verify"></a>Test ve doğrulama

1.  Projeyi derlemek için aşağıdaki komutu yürütün

    ```dotnetcli
    dotnet build
    ```

1. Uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

    ```dotnetcli
    dotnet run
    ```

1. İstendiğinde, gizli bir değer girin. Örneğin, mySecretPassword.

    Aşağıdaki çıkışın bir varyasyonu görüntülenir:

    ```console
    Creating a certificate in mykeyvault called 'myCertificate' ... done.
    Retrieving your certificate from mykeyvault.
    Your certificate version is '8532359bced24e4bb2525f2d2050738a'.
    Deleting your certificate from jl-kv ... done
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, anahtar Kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLı veya Azure PowerShell kullanabilirsiniz.

### <a name="delete-a-key-vault"></a>Key Vault silme

```azurecli
az keyvault delete --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name>
```

### <a name="purge-a-key-vault"></a>Key Vault Temizleme

```azurecli
az keyvault purge --location eastus --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name> -InRemovedState -Location eastus
```

### <a name="delete-a-resource-group"></a>Kaynak grubunu silme

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Anahtar Kasası oluşturdunuz, bir sertifikayı depolı, ve bu sertifikayı almıştır. 

Key Vault ve bunu uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Sertifikalara genel bakış](about-certificates.md) konusunu okuyun
- [App Service uygulama öğreticisindeki bir erişim Key Vault](../general/tutorial-net-create-vault-azure-web-app.md) görün
- [Sanal makine öğreticiden bir erişim Key Vault](../general/tutorial-net-virtual-machine.md) görün
- [Azure Key Vault geliştirici kılavuzuna](../general/developers-guide.md) bakın
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault