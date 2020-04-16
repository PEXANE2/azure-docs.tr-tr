---
title: Öğretici - .NET'te Azure web uygulamasıyla Azure Key Vault'u kullanın | Microsoft Dokümanlar
description: Bu öğreticide, anahtar kasanızdan bir sırrı okumak için ASP.NET bir çekirdek uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: bb907c809d411128ee799d5057379a2022144882
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422882"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Öğretici: Azure Anahtar Kasası'nı .NET'te Azure web uygulamasıyla kullanma

Azure Key Vault, API tuşları ve veritabanı bağlantı dizeleri gibi sırları korumanıza yardımcı olur. Uygulamalarınıza, hizmetlerinize ve BT kaynaklarına erişim sağlar.

Bu eğitimde, Azure anahtar kasasından bilgileri okuyabilen bir Azure web uygulaması oluşturmayı öğrenirsiniz. İşlem, Azure kaynakları için yönetilen kimlikleri kullanır. Azure web uygulamaları hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'ne](../../app-service/overview.md)bakın.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir sır ekle.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure web uygulaması oluşturun.
> * Web uygulaması için yönetilen bir kimliği etkinleştirin.
> * Web uygulaması için izin atayın.
> * Web uygulamasını Azure'da çalıştırın.

Başlamadan önce [Key Vault temel kavramlarını](basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Windows için: [.NET Core 2.1 SDK veya sonrası](https://www.microsoft.com/net/download/windows)
* Mac [için: Mac için Visual Studio](https://visualstudio.microsoft.com/vs/mac/)
* Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Azure CLI sürümü 2.0.4 veya daha sonra yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="about-managed-service-identity"></a>Yönetilen Hizmet Kimliği hakkında

Azure Key Vault kimlik bilgilerini güvenli bir şekilde saklar, böylece kodunuzda görüntülenmez. Ancak, anahtarlarınızı almak için Azure Key Vault'a kimlik doğrulamanız gerekir. Key Vault'a kimlik doğrulamak için bir kimlik bilgisine ihtiyacınız var. Klasik bir çizme ikilemi. Yönetilen Hizmet Kimliği (MSI), işlemi basitleştiren bir _bootstrap kimliği_ sağlayarak bu sorunu çözer.

Azure Sanal Makineleri, Azure Uygulama Hizmeti veya Azure Fonksiyonları gibi bir Azure hizmeti için MSI'ı etkinleştirdiğinizde, Azure bir [hizmet ilkesi](basic-concepts.md)oluşturur. MSI bunu Azure Etkin Dizin (Azure AD)'deki hizmet örneği için yapar ve bu örneğin içine hizmet temel kimlik bilgilerini enjekte eder.

![MSI diyagramı](../media/MSI.png)

Ardından, bir erişim jetonu almak için kodunuz Azure kaynağında bulunan yerel bir meta veri hizmetini çağırır. Kodunuz, yerel MSI bitiş noktasından bir Azure Anahtar Kasası hizmetine kimlik doğrulamasına kadar aldığı erişim belirtecikullanır.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun.

Ardından, bir kaynak grubu adı seçin ve yer tutucuyu doldurun. Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Bu kaynak grubunu bu öğretici boyunca kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kaynak grubunuzda önemli bir kasa oluşturmak için aşağıdaki bilgileri sağlayın:

* Anahtar kasa adı: yalnızca sayılar (0-9), harfler (a-z, A-Z) ve tire (-) içerebilen 3 ila 24 karakterden oluşan bir dize
* Kaynak grubu adı
* Yer: **Batı ABD**

Azure CLI'de aşağıdaki komutu girin:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada, Azure hesabınız bu yeni kasada işlem gerçekleştirme yetkisine sahip tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Şimdi bir sır ekleyebilirsiniz. Bir SQL bağlantı dizesi veya hem güvenli hem de uygulamanız için kullanılabilir tutmak için gereken diğer bilgiler olabilir.

**AppSecret**adlı anahtar tonozbir sır oluşturmak için, aşağıdaki komutu girin: 

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli değeri **MySecret**depolar .

Gizli metinde yer alan değeri düz metin olarak görüntülemek için aşağıdaki komutu girin:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Bu komut URI de dahil olmak üzere gizli bilgileri görüntüler. 

Bu adımları tamamladıktan sonra, bir anahtar kasasında gizli dizi URI'sine sahip olmanız gerekir. Bu öğreticide daha sonra kullanmak üzere bu bilgilere dikkat edin. 

## <a name="create-a-net-core-web-app"></a>Bir .NET Core web uygulaması oluşturma

Bir .NET Core web uygulaması oluşturmak ve Azure'da yayımlamak için [Azure'da ASP.NET Core web uygulaması oluşturma'daki](../../app-service/app-service-web-get-started-dotnet.md)yönergeleri izleyin. 

Ayrıca şu videoyu izleyebilirsiniz:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Çözümü açma ve düzenleme

1. **Sayfalar** > **About.cshtml.cs** dosyasına gidin.

1. Bu NuGet paketlerini yükleyin:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)

1. Aşağıdaki kodu *About.cshtml.cs* dosyasına aktarın:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

   AboutModel sınıfındaki kodunuz aşağıdaki gibi görünmelidir:

   ```csharp
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The next four lines of code show you how to use AppAuthentication library to fetch secrets from your key vault */
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;
            }
            /* If you have throttling errors see this tutorial https://docs.microsoft.com/azure/key-vault/tutorial-net-create-vault-azure-web-app */
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
            }
        }

        // This method implements exponential backoff if there are 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory, which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Visual Studio 2019'un ana menüsünde hata ayıklama lı veya hata ayıklamaolmadan **Hata Ayıklama** > **Başlat'ı**seçin. 
1. Tarayıcıda, **Hakkında** sayfasına gidin.  
    **AppSecret** değeri görüntülenir.

## <a name="enable-a-managed-identity"></a>Yönetilen bir kimliği etkinleştirme

Azure Key Vault kimlik bilgilerini ve diğer sırları güvenli bir şekilde depolamanın bir yolunu sağlar, ancak kodunuzun bunları almak için Key Vault'a kimlik doğrulaması gerekir. [Azure kaynaklarına genel bakış için yönetilen kimlikler,](../../active-directory/managed-identities-azure-resources/overview.md) Azure hizmetlerine Azure AD'de otomatik olarak yönetilen bir kimlik vererek bu sorunu çözmeye yardımcı olur. Bu kimliği, kodunuzda kimlik bilgilerini görüntülemek zorunda kalmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanabilirsiniz.

Azure CLI'de, bu uygulama için kimlik oluşturmak için, kimlik atama komutunu çalıştırın:

```azurecli
az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"
```

YourAppName>'yi Azure'da yayınlanan uygulamanın adıyla değiştirin. \<  
    Örneğin, yayınlanan uygulama adınız **MyAwesomeapp.azurewebsites.net** \<, YourAppName **>'ı MyAwesomeapp**ile değiştirin.

Uygulamayı Azure'da `PrincipalId` ne zaman yayımladığınızda n için not edin. Adım 1'deki komutun çıktısı aşağıdaki biçimde olmalıdır:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

>[!NOTE]
>Bu yordamdaki komut, [Azure portalına](https://portal.azure.com) gidip web uygulama **özelliklerinde** **Atanan Kimlik/Sistem** ayarı ile değiştirmeye eşdeğerdir.

## <a name="assign-permissions-to-your-app"></a>Uygulamanız için izinler atama

YourKeyVaultName> anahtar kasanızın adı ile \< \<değiştirin ve PrincipalId> aşağıdaki komuttaki **PrincipalId** değeriyle değiştirin:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Bu komut, uygulama hizmetinin kimliğini (MSI) anahtar kasanızda **alma** ve **liste** işlemlerini yapma izni verir.

## <a name="publish-the-web-app-to-azure"></a>Web uygulamasını Azure’da yayımlama

Canlı web uygulamanızın gizli değeri getirebileceğini doğrulamak için web uygulamanızı bir kez daha Azure'da yayınlayın.

1. Visual Studio'da **key-vault-dotnet-core-quickstart** projesini seçin.
2. **Başlat'ı Yayımla'yı** > **Start**seçin.
3. **Oluştur**’u seçin.

Uygulamayı çalıştırdığınızda, gizli değerinizi alabildiğinizi görmeniz gerekir.

Şimdi, .NET'te sırlarını anahtar kasanızdan saklayan ve getiren bir web uygulamasını başarıyla oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık ihtiyaç duyulmadığında, sanal makineyi ve anahtar kasanızı silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

>[!div class="nextstepaction"]
>[Azure Key Vault Geliştirici Kılavuzu](developers-guide.md)
