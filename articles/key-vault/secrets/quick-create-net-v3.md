---
title: Quickstart - .NET için Azure Key Vault istemci kitaplığı (SDK v3)
description: .NET istemci kitaplığını (v3) kullanarak Azure anahtar kasasından nasıl sır oluşturup, alınve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 11/05/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: bdc686284711fb39e6cedb18aea4d1f5c9d1d318
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425010"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v3"></a>Hızlı başlangıç: .NET için Azure Key Vault istemci kitaplığı (SDK v3)

.NET için Azure Key Vault istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

> [!NOTE]
> Bu hızlı başlangıç, Microsoft.Azure.KeyVault istemci kitaplığı'nın v3.0.4 sürümünü kullanır. Key Vault istemci kitaplığı'nın en güncel sürümünü kullanmak [için .NET (SDK v4) için Azure Key Vault istemci kitaplığına](quick-create-net.md)bakın. 

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. .NET için Key Vault istemci kitaplığını kullanın:

- Anahtarlar ve parolalar üzerinde güvenliği ve denetimi artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve aktarın.
- Bulut ölçeği ve genel artıklık la gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirin ve otomatikleştirin.
- FIPS 140-2 Düzey 2 onaylı HSM'leri kullanın.

[API başvuru belgeleri](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/)

> [!NOTE]
> Her anahtar kasanın benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.


## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [.NET Core 2.1 SDK veya sonrası](https://dotnet.microsoft.com/download/dotnet-core/2.1).
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlatma, Bir `dotnet`Windows terminalinde [(PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6), [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-6)veya Azure Bulut [Shell](https://shell.azure.com/)gibi) Azure [CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)ve Windows komutları çalıştırdığınızı varsayar.

## <a name="setting-up"></a>Ayarlama

### <a name="create-new-net-console-app"></a>Yeni .NET konsol uygulaması oluşturun

Konsol penceresinde, adı `dotnet new` `akv-dotnet`olan yeni bir .NET konsol uygulaması oluşturmak için komutu kullanın.


```console
dotnet new console -n akvdotnet
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
dotnet add package Microsoft.Azure.KeyVault
```

Bu hızlı başlangıç için aşağıdaki paketleri de yüklemeniz gerekir:

```console
dotnet add package System.Threading.Tasks
dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve anahtar kasası oluşturma

Bu hızlı başlatma, önceden oluşturulmuş bir Azure anahtar kasası kullanır. [Azure CLI quickstart,](quick-create-cli.md) [Azure PowerShell quickstart](quick-create-powershell.md)veya Azure portalı [quickstart'taki](quick-create-portal.md)adımları izleyerek önemli bir kasa oluşturabilirsiniz. Alternatif olarak, aşağıdaki Azure CLI komutlarını çalıştırmanız yeterlidir.

> [!Important]
> Her anahtar kasanın benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bulut tabanlı bir .NET uygulamasının kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](../general/managed-identity.md) Ancak basitlik adına, bu hızlı başlatma bir .NET konsol uygulaması oluşturur. Azure ile bir masaüstü uygulamasının kimliğini doğrulamak için bir hizmet sorumlusu nun ve erişim denetim ilkesinin kullanılması nı gerektirir.

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

Aşağıdaki anahtar kasa adımiçin [Authenticate](#authenticate-to-your-key-vault) bunları kullanacağız gibi clientId ve clientSecret not alın.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Servis müdürüne anahtar kasanıza erişim hakkı verin

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Servis sorumlusuna hem anahtarlar hem de sırlar için izinleri al, listele ve ayarla.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

## <a name="object-model"></a>Nesne modeli

.NET için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri size nasıl bir sır ayarlayıp bir sır alacağınızı gösterecektir.

Tüm konsol uygulaması . https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=directives)]

### <a name="authenticate-to-your-key-vault"></a>Anahtar kasanıza kimlik doğrulaması

Bu .NET hızlı başlatma, koda konulmaması gereken kimlik bilgilerini depolamak için ortam değişkenlerine dayanır. 

Uygulamanızı oluşturmadan ve çalıştırmadan `setx` önce, `akvClientId`yukarıda `akvClientSecret` `akvTenantId`belirttiğiniz `akvSubscriptionId` değerlere , , ve çevre değişkenlerini ayarlamak için komutu kullanın.

**Windows**

```console
setx akvClientId "<your-clientID>"
setx akvClientSecret "<your-clientSecret>"
```

**Linux**

```bash
export akvClientId = "<your-clientID>"
export akvClientSecret = "<your-clientSecret>"
```

**Macos**

```bash
export akvClientId = "<your-clientID>"
export akvClientSecret = "<your-clientSecret>"
```

Bu ortam değişkenlerini kodunuzdaki dizeleri atayın ve ardından [bunları KeyVaultClient sınıfına](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)geçirerek uygulamanızın kimliğini doğrulayın:

[!code-csharp[Authentication](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=authentication)]

### <a name="save-a-secret"></a>Bir sırrı kaydet

Artık uygulamanızın kimlik doğrulaması olduğuna göre, [SetSecretAsync yöntemini](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) kullanarak keyvault'unuza bir sır koyabilirsiniz Bu, formda `https://<your-unique-keyvault-name>.vault.azure.net/secrets/`olan anahtar kasanızın URL'sini gerektirir. Ayrıca sır için bir isim gerektirir -- "mySecret"ı kullanıyoruz. 

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=setsecret)]

Gizlinin [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Bir sır alma

Artık [GetSecretAsync yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync) yle önceden ayarlanmış değeri alabilirsiniz

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=getsecret)]

Sırrın artık `keyvaultSecret.Value;`.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, anahtar kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLI veya Azure PowerShell'i kullanabilirsiniz.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sır sakladınız ve bu sırrı aldınız. [GitHub'daki konsol uygulamasının tamamını](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet)görün.

Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- [.NET'i kullanarak Azure Anahtar Kasası'na Hizmet-servis kimlik doğrulaması uygulama](../general/service-to-service-authentication.md)
- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](../general/developers-guide.md) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)
