---
title: Hızlı başlangıç-.NET için Azure Key Vault istemci kitaplığı (SDK v3)
description: .NET istemci kitaplığı 'nı (v3) kullanarak bir Azure Anahtar Kasası 'ndan gizli dizileri oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 11/05/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 2e5f3357baa774c9690e079a82b11d2c651ebe9b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078875"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v3"></a>Hızlı başlangıç: .NET için Azure Key Vault istemci kitaplığı (SDK v3)

.NET için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

> [!NOTE]
> Bu hızlı başlangıç, Microsoft. Azure. Keykasası istemci kitaplığının v 3.0.4 sürümünü kullanır. Key Vault istemci kitaplığının en güncel sürümünü kullanmak için bkz. [.NET için Azure Key Vault istemci kitaplığı (SDK v4)](quick-create-net.md). 

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. .NET için Key Vault istemci kitaplığı 'nı kullanın:

- Anahtarlar ve parolalar üzerinde güvenlik ve denetim düzeyini artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve içeri aktarın.
- Bulut ölçeği ve küresel yedeklilik ile gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirme ve otomatikleştirme.
- FIPS 140-2 düzey 2 doğrulanan HSM 'leri kullanın.

[API başvuru belgeleri](/dotnet/api/overview/azure/key-vault?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/)

> [!NOTE]
> Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. -Unique-keykasa-adı> <aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.


## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [.NET Core 3,1 SDK veya üzeri](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/)

Bu hızlı başlangıçta `dotnet` , bir Windows terminalinde ( [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6), [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-6)veya [Azure Cloud Shell](https://shell.azure.com/)) çalıştırdığınız, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest)ve Windows komutlarının bulunduğu varsayılmaktadır.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-new-net-console-app"></a>Yeni .NET konsol uygulaması oluşturma

Konsol penceresinde, `dotnet new` adıyla yeni bir .NET konsol uygulaması oluşturmak için komutunu kullanın `akv-dotnet` .


```console
dotnet new console -n akvdotnet
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir.

```console
Build succeeded.
 0 Warning(s)
 0 Error(s)
```

### <a name="install-the-package"></a>Paketi yükler

Konsol penceresinde, .NET için Azure Key Vault istemci Kitaplığı ' nı yükledikten sonra:

```console
dotnet add package Microsoft.Azure.KeyVault
```

Bu hızlı başlangıçta, aşağıdaki paketleri de yüklemeniz gerekecektir:

```console
dotnet add package System.Threading.Tasks
dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[!INCLUDE [Create a service principal](../../../includes/key-vault-sp-creation.md)]

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

[!INCLUDE [Give the service principal access to your key vault](../../../includes/key-vault-sp-kv-access.md)]

## <a name="object-model"></a>Nesne modeli

.NET için Azure Key Vault istemci kitaplığı, sertifikaları ve gizli dizileri gibi anahtarları ve ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örnekleri, gizli anahtar ayarlama ve gizli anahtar alma işlemlerinin nasıl yapılacağını gösterir.

Konsol uygulamasının tamamına adresinden ulaşılabilir https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet .

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri kodunuzun en üstüne ekleyin:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=directives)]

### <a name="authenticate-to-your-key-vault"></a>Anahtar kasanıza kimlik doğrulama

Bu .NET hızlı başlangıcı, kod içine yerleştirilmemelidir kimlik bilgilerini depolamak için ortam değişkenlerine bağımlıdır. 

Uygulamanızı derleyip çalıştırmadan önce,,, `setx` `akvClientId` `akvClientSecret` `akvTenantId` ve `akvSubscriptionId` ortam değişkenlerini yukarıda not ettiğiniz değerlere ayarlamak için komutunu kullanın.

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

**MacOS**

```bash
export akvClientId = "<your-clientID>"
export akvClientSecret = "<your-clientSecret>"
```

Bu ortam değişkenlerini kodunuzdaki dizelere atayın ve ardından bunları [Keyvaultclient sınıfına](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)geçirerek uygulamanızın kimliğini doğrulayın:

[!code-csharp[Authentication](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=authentication)]

### <a name="save-a-secret"></a>Gizli dizi Kaydet

Uygulamanızın kimliği doğrulandığına göre, [Setsecretasync metodunu](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) kullanarak keykasanıza gizli dizi ekleyebilirsiniz, bu, formda olan anahtar KASASıNıN URL 'sini gerektirir `https://<your-unique-keyvault-name>.vault.azure.net/secrets/` . Ayrıca gizli dizi için bir ad gerektirir; "mySecret" i kullanıyoruz. 

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=setsecret)]

Parolanın [az keykasası Secret Show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Gizli dizi alma

Artık, [Getsecretasync yöntemiyle](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync) önceden ayarlanan değeri alabilirsiniz

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=getsecret)]

Gizli anahtar artık olarak kaydedilir `keyvaultSecret.Value;` .

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, anahtar Kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLı veya Azure PowerShell kullanabilirsiniz.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, gizli dizi depolandı ve bu gizli dizi alındı. [GitHub 'da konsol uygulamasının tamamına](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet)bakın.

Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [.NET kullanarak Azure Key Vault Için hizmetten hizmete kimlik doğrulaması](../general/service-to-service-authentication.md) uygulama
- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Azure Key Vault geliştirici kılavuzuna](../general/developers-guide.md) bakın
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault
