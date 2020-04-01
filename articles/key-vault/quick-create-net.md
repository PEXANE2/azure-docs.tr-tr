---
title: Quickstart - .NET için Azure Key Vault istemci kitaplığı (v4)
description: .NET istemci kitaplığını (v4) kullanarak Azure anahtar kasasından nasıl sır oluşturup, alınve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 03/12/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: a94717c7bed3ba25a4682896053672fe100dc43a
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398391"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v4"></a>Hızlı başlangıç: .NET için Azure Key Vault istemci kitaplığı (SDK v4)

.NET için Azure Key Vault istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. .NET için Key Vault istemci kitaplığını kullanın:

- Anahtarlar ve parolalar üzerinde güvenliği ve denetimi artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve aktarın.
- Bulut ölçeği ve genel artıklık la gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirin ve otomatikleştirin.
- FIPS 140-2 Düzey 2 onaylı HSM'leri kullanın.

[API başvuru belgeleri](/dotnet/api/azure.security.keyvault.secrets?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paketi (NuGet)](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [.NET Core 2.1 SDK veya sonrası](https://dotnet.microsoft.com/download/dotnet-core/2.1).
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlatma, Bir `dotnet`Windows terminalinde [(PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6), [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-6)veya Azure Bulut [Shell](https://shell.azure.com/)gibi) Azure [CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)ve Windows komutları çalıştırdığınızı varsayar.

## <a name="setting-up"></a>Ayarlama

### <a name="create-new-net-console-app"></a>Yeni .NET konsol uygulaması oluşturun

Konsol penceresinde, adı `dotnet new` `key-vault-console-app`olan yeni bir .NET konsol uygulaması oluşturmak için komutu kullanın.

```console
dotnet new console -n key-vault-console-app
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```console
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir.

```console
Build succeeded.
 0 Warning(s)
 0 Error(s)
```

### <a name="install-the-package"></a>Paketi yükleyin

Konsol penceresinden .NET için Azure Key Vault istemci kitaplığını yükleyin:

```console
dotnet add package Azure.Security.KeyVault.Secrets
```

Bu hızlı başlangıç için aşağıdaki paketleri de yüklemeniz gerekir:

```console
dotnet add package Azure.Identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve anahtar kasası oluşturma

Bu hızlı başlatma, önceden oluşturulmuş bir Azure anahtar kasası kullanır. [Azure CLI quickstart,](quick-create-cli.md) [Azure PowerShell quickstart](quick-create-powershell.md)veya Azure portalı [quickstart'taki](quick-create-portal.md)adımları izleyerek önemli bir kasa oluşturabilirsiniz. Alternatif olarak, aşağıdaki Azure CLI komutlarını çalıştırmanız yeterlidir.

> [!Important]
> Her anahtar kasanın benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

```azurepowershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS

New-AzKeyVault -Name <your-unique-keyvault-name> -ResourceGroupName myResourceGroup -Location EastUS
```

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bulut tabanlı bir .NET uygulamasının kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](managed-identity.md) Ancak basitlik adına, bu hızlı başlatma bir .NET konsol uygulaması oluşturur. Azure ile bir masaüstü uygulamasının kimliğini doğrulamak için bir hizmet sorumlusu nun ve erişim denetim ilkesinin kullanılması nı gerektirir.

Azure CLI az reklam [sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet ilkesi oluşturun:

```azurecli
az ad sp create-for-rbac -n "http://mySP" --sdk-auth
```

Bu işlem bir dizi anahtar / değer çifti döndürecektir. 

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Azure PowerShell [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal) komutunu kullanarak bir hizmet sorumlusu oluşturun:

```azurepowershell
# Create a new service principal
$spn = New-AzADServicePrincipal -DisplayName "http://mySP"

# Get the tenant ID and subscription ID of the service principal
$tenantId = (Get-AzContext).Tenant.Id
$subscriptionId = (Get-AzContext).Subscription.Id

# Get the client ID
$clientId = $spn.ApplicationId

# Get the client Secret
$bstr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($spn.Secret)
$clientSecret = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($bstr)
```

Azure PowerShell ile hizmet sorumlusu hakkında daha fazla bilgi için Azure PowerShell ile [bir Azure hizmet sorumlusu oluştur'a](/powershell/azure/create-azure-service-principal-azureps)bakın.

Aşağıdaki adımlarda kullanacağımız için clientId, clientSecret ve tenantId'e dikkat edin.


#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Servis müdürüne anahtar kasanıza erişim hakkı verin

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Servis sorumlusuna hem anahtarlar hem de sırlar için izinleri al, listele ve ayarla.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions list get set delete purge
```

```azurepowershell
Set-AzKeyVaultAccessPolicy -VaultName <your-unique-keyvault-name> -ServicePrincipalName <clientId-of-your-service-principal> -PermissionsToSecrets list,get,set,delete,purge
```

#### <a name="set-environmental-variables"></a>Çevresel değişkenleri ayarlama

Uygulamamızdaki Varsayılan AzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`ve `AZURE_TENANT_ID`. bu değişkenleri yukarıda [bir hizmet anası](#create-a-service-principal) oluştur adımında belirttiğiniz clientId, clientSecret ve tenantId değerlerine ayarlayın.

Ayrıca anahtar kasa adınızı bir ortam değişkeni `KEY_VAULT_NAME`olarak kaydetmeniz gerekir;

```console
setx AZURE_CLIENT_ID <your-clientID>

setx AZURE_CLIENT_SECRET <your-clientSecret>

setx AZURE_TENANT_ID <your-tenantId>

setx KEY_VAULT_NAME <your-key-vault-name>
````

Her aradığınızda `setx`"SUCCESS: Belirtilen değer kaydedildi" yanıtını almalısınız.

```shell
AZURE_CLIENT_ID=<your-clientID>

AZURE_CLIENT_SECRET=<your-clientSecret>

AZURE_TENANT_ID=<your-tenantId>

KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Nesne modeli

.NET için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturmak, bir sır ayarlamak, bir sır almak ve bir sırrı silmek nasıl gösterecektir.

Tüm konsol uygulaması . https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=directives)]

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adımındaki çevresel değişkenlere bağlıdır. Anahtar kasanızın adı, "https://-key-vault-name\<\>.vault.azure.net" biçiminde, anahtar kasası URI'ye genişletilir.

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=authenticate)]

### <a name="save-a-secret"></a>Bir sırrı kaydet

Başvurunuzun kimliği doğrulanabildiği için, istemciyi kullanarak keyvault'unuza bir sır [koyabilirsiniz. SetSecret yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) Bu sır için bir ad gerektirir -- bu örnekte "mySecret" kullanıyoruz.  

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=setsecret)]

Gizlinin [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

### <a name="retrieve-a-secret"></a>Bir sır alma

Şimdi istemci ile daha önce ayarlanmış değeri [alabilirsiniz. GetSecret yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync).

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=getsecret)]

Sırrın artık `secret.Value`.

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, istemci ile anahtar kasasından sırrı [silelim. DeleteSecret yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync).

[!code-csharp[Delete secret](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=deletesecret)]

Gizli [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutu ile gitti doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, anahtar kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLI veya Azure PowerShell'i kullanabilirsiniz.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Örnek kod

```csharp
using System;
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

namespace key_vault_console_app
{
    class Program
    {
        static void Main(string[] args)
        {
            string secretName = "mySecret";

            string keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
            var kvUri = "https://" + keyVaultName + ".vault.azure.net";

            var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());

            Console.Write("Input the value of your secret > ");
            string secretValue = Console.ReadLine();

            Console.Write("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");

            client.SetSecret(secretName, secretValue);

            Console.WriteLine(" done.");

            Console.WriteLine("Forgetting your secret.");
            secretValue = "";
            Console.WriteLine("Your secret is '" + secretValue + "'.");

            Console.WriteLine("Retrieving your secret from " + keyVaultName + ".");

            KeyVaultSecret secret = client.GetSecret(secretName);

            Console.WriteLine("Your secret is '" + secret.Value + "'.");

            Console.Write("Deleting your secret from " + keyVaultName + " ...");

            client.StartDeleteSecret(secretName);

            System.Threading.Thread.Sleep(5000);
            Console.WriteLine(" done.");

        }
    }
}
```


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sır sakladınız ve bu sırrı aldınız. [GitHub'daki konsol uygulamasının tamamını](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app)görün.

Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- [.NET'i kullanarak Azure Anahtar Kasası'na Hizmet-servis kimlik doğrulaması uygulama](service-to-service-authentication.md)
- Azure [Anahtar Kasasına Genel Bakış](key-vault-overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](key-vault-developers-guide.md) bakın
- [Anahtarlar, sırlar ve sertifikalar](about-keys-secrets-and-certificates.md) hakkında bilgi edinin
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](key-vault-best-practices.md)
