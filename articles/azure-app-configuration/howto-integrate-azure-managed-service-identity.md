---
title: Azure yönetilen kimlikleri kullanarak kimlik doğrulaması
titleSuffix: Azure App Configuration
description: Azure yönetilen kimlikleri kullanarak Azure Uygulama Yapılandırmasına kimlik doğrulama
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 2/25/2020
ms.openlocfilehash: bf97a1eae758778efc8d800666af4a5fcb574429
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80056833"
---
# <a name="integrate-with-azure-managed-identities"></a>Azure Yönetilen Kimliklerle tümleştirme

Azure Active Directory [yönetilen kimlikler,](../active-directory/managed-identities-azure-resources/overview.md) bulut uygulamanız için sırlar yönetimini basitleştirir. Yönetilen bir kimlikle kodunuz, çalıştığı Azure hizmeti için oluşturulan hizmet ilkesini kullanabilir. Azure Anahtar Atlama'da veya yerel bir bağlantı dizesinde depolanan ayrı bir kimlik bilgisi yerine yönetilen bir kimlik kullanırsınız. 

Azure Uygulama Yapılandırması ve .NET Core, .NET Framework ve Java Spring istemci kitaplıkları, kendilerinde yerleşik kimlik desteğini yönetmektedir. Kullanmanız gerekmese de, yönetilen kimlik sırları içeren bir erişim belirteci gereksinimini ortadan kaldırır. Kodunuz, yalnızca hizmet bitiş noktasını kullanarak Uygulama Yapılandırma deposuna erişebilir. Bu URL'yi herhangi bir sırrı açığa çıkarmadan doğrudan kodunuza gömebilirsiniz.

Bu makalede, Uygulama Yapılandırmasına erişmek için yönetilen kimlikten nasıl yararlanabileceğiniz gösterilmektedir. Bu quickstarts tanıtılan web uygulaması üzerine oluşturur. Devam etmeden önce, [önce App Configuration ile ASP.NET Core uygulaması oluşturun.](./quickstart-aspnet-core-app.md)

Bu makalede, yönetilen kimliği App Configuration'ın Key Vault başvurularıyla birlikte nasıl kullanabileceğiniz de gösterilmektedir. Tek bir yönetilen kimlikle, Key Vault'tan hem de Uygulama Yapılandırması'ndan yapılandırma değerlerinden her iki sıra sorunsuz bir şekilde erişebilirsiniz. Bu yeteneği keşfetmek istiyorsanız, önce [ASP.NET Core ile Anahtar Kasa Referanslarını Kullanın'ı](./use-key-vault-references-dotnet-core.md) tamamla.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarında mükemmel bir seçenektir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * App Configuration'a yönetilen bir kimlik erişimi sağlayın.
> * Uygulama Yapılandırması'na bağlandığınızda uygulamanızı yönetilen bir kimliği kullanacak şekilde yapılandırın.
> * İsteğe bağlı olarak, Bir Uygulama Yapılandırma Anahtar Kasası başvurusu yla Key Vault'a bağlandığınızda uygulamanızı yönetilen bir kimlik kullanacak şekilde yapılandırın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* [.NET Çekirdek SDK](https://www.microsoft.com/net/download/windows).
* [Azure Bulut Kabuğu yapılandırıldı.](https://docs.microsoft.com/azure/cloud-shell/quickstart)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Yönetilen kimlik ekleme

Portalda yönetilen bir kimlik ayarlamak için önce bir uygulama oluşturun ve ardından özelliği etkinleştirin.

1. [Azure portalında](https://portal.azure.com) normalde yaptığınız gibi bir Uygulama Hizmetleri örneği oluşturun. Portalda git.

1. Sol bölmedeki **Ayarlar** grubuna gidin ve **Kimlik'i**seçin.

1. Sistem **atanan** sekmede, **Durum** **Açık'a** geçin ve **Kaydet'i**seçin.

1. Sistem yönetilen kimlik atanmış etkinleştirmek için istendiğinde **Evet** yanıtı.

    ![Uygulama Hizmetinde yönetilen kimliği ayarlama](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Uygulama Yapılandırmasına erişim izni verme

1. Azure [portalında](https://portal.azure.com) **Tüm kaynakları** seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma deposunu seçin.

1. **Erişim denetimi (IAM) seçeneğini**belirleyin.

1. **Erişim** sekmesinde rol atama kartı **Ekle'yi** seçin. **Add role assignment**

1. **Rolü**altında, **App Configuration Data Reader'ı**seçin. **Access'e erişim atama**altında, **Yönetilen kimlik atanan Sistem**altında App **Service'i** seçin.

1. **Abonelik**altında Azure aboneliğinizi seçin. Uygulamanız için Uygulama Hizmeti kaynağını seçin.

1. **Kaydet'i**seçin.

    ![Yönetilen kimlik ekleme](./media/add-managed-identity.png)

1. İsteğe bağlı: Key Vault'a da erişim izni vermek istiyorsanız, yönetilen bir kimlikle Anahtar Kasa kimlik [doğrulaması sağlayın'daki](https://docs.microsoft.com/azure/key-vault/managed-identity)yönergeleri izleyin.

## <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

1. *Azure.Identity* paketine bir başvuru ekleyin:

    ```cli
    dotnet add package Azure.Identity
    ```

1. App Configuration mağazanızın bitiş noktasını bulun. Bu URL, Azure portalındaki mağazanın **Erişim tuşları** sekmesinde listelenir.

1. *appsettings.json'ı*açın ve aşağıdaki komut dosyasını ekleyin. Parantezler de dahil olmak üzere * \<service_endpoint>* uygulama yapılandırma mağazanızın URL'si ile değiştirin. 

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

1. *Program.cs*açın ve `Azure.Identity` `Microsoft.Azure.Services.AppAuthentication` ad boşluklarına bir başvuru ekleyin:

    ```csharp-interactive
    using Azure.Identity;
    ```

1. Yalnızca Doğrudan Uygulama Yapılandırması'nda depolanan değerlere `CreateWebHostBuilder` erişmek istiyorsanız, `config.AddAzureAppConfiguration()` yöntemi değiştirerek yöntemi güncelleştirin.

    > [!IMPORTANT]
    > `CreateHostBuilder`.NET `CreateWebHostBuilder` Core 3.0'ın yerini alır.  Ortamınıza göre doğru sözdizimini seçin.

    ### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((hostingContext, config) =>
                {
                    var settings = config.Build();
                    config.AddAzureAppConfiguration(options =>
                        options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential()));
                })
                .UseStartup<Startup>();
    ```

    ### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                    config.AddAzureAppConfiguration(options =>
                        options.Connect(new Uri(settings["AppConfig:Endpoint"]), new ManagedIdentityCredential()));
                })
                .UseStartup<Startup>());
    ```
    ---

1. Hem Uygulama Yapılandırma değerlerini hem de Key Vault başvurularını kullanmak için, aşağıda gösterildiği gibi *Program.cs* güncelleyin. Bu kod, an `KeyVaultClient` `AzureServiceTokenProvider` kullanarak yeni bir yöntem oluşturur `UseAzureKeyVault` ve bu başvuruyu yönteme bir çağrıya geçirir.

    ### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
            public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
                WebHost.CreateDefaultBuilder(args)
                    .ConfigureAppConfiguration((hostingContext, config) =>
                    {
                        var settings = config.Build();
                        var credentials = new ManagedIdentityCredential();

                        config.AddAzureAppConfiguration(options =>
                        {
                            options.Connect(new Uri(settings["AppConfig:Endpoint"]), credentials)
                                    .ConfigureKeyVault(kv =>
                                    {
                                        kv.SetCredential(credentials);
                                    });
                        });
                    })
                    .UseStartup<Startup>();
    ```

    ### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
                    {
                        var settings = config.Build();
                        var credentials = new ManagedIdentityCredential();

                        config.AddAzureAppConfiguration(options =>
                        {
                            options.Connect(new Uri(settings["AppConfig:Endpoint"]), credentials)
                                    .ConfigureKeyVault(kv =>
                                    {
                                        kv.SetCredential(credentials);
                                    });
                        });
                    })
                    .UseStartup<Startup>());
    ```
    ---

    Artık diğer Uygulama Yapılandırması tuşu gibi Key Vault referansları erişebilirsiniz. Config sağlayıcısı, Key `KeyVaultClient` Vault'a kimlik doğrulamak ve değeri almak için yapılandırdığınız değeri kullanır.

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="deploy-from-local-git"></a>Yerel Git’ten dağıtım

Kudu build sunucusuyla uygulamanız için yerel Git dağıtımını etkinleştirmenin en kolay yolu [Azure Bulut BulutU'yu](https://shell.azure.com)kullanmaktır.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Kudu ile yerel Git'i etkinleştirin
Uygulamanız için yerel bir git deposu yoksa, uygulamanın başlatılması gerekir. Yerel bir git deposunu başlatmayı sağlamak için uygulamanızın proje dizininden aşağıdaki komutları çalıştırın:

```cmd
git init
git add .
git commit -m "Initial version"
```

Kudu build sunucusuyla uygulamanız için yerel Git [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) dağıtımını etkinleştirmek için Cloud Shell'de çalıştırın.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Bu komut, aşağıdaki çıktıya benzer bir şey verir:

```json
{
  "url": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
}
```

### <a name="deploy-your-project"></a>Projenizi dağıtma

Yerel _terminal penceresinde,_ yerel Git deponuza bir Azure uzaktan kumandası ekleyin. _ \<Url>,_ Kudu ile yerel Git'i etkinleştir'den aldığınız Git uzaktan kumandasının URL'si [ile değiştirin.](#enable-local-git-with-kudu)

```bash
git remote add azure <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Parola istendiğinde, [dağıtım kullanıcısını yapılandır'da](#configure-a-deployment-user)oluşturduğunuz parolayı girin. Azure portalında oturum açarken kullandığınız parolayı kullanmayın.

```bash
git push azure master
```

Çıkışta, ASP.NET için MSBuild, `npm install` Node.js ve `pip install` Python için çalışma süresine özgü otomasyon u görebilirsiniz.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

İçeriğin dağıtılmış olduğunu doğrulamak için bir tarayıcı kullanarak web uygulamanıza göz atın.

```bash
http://<app_name>.azurewebsites.net
```

## <a name="use-managed-identity-in-other-languages"></a>Yönetilen kimliği diğer dillerde kullanma

.NET Framework ve Java Spring için Uygulama Yapılandırma sağlayıcıları da yönetilen kimlik için yerleşik desteğe sahiptir. Bu sağlayıcılardan birini yapılandırırken mağazanızın tam bağlantı dizesi yerine URL bitiş noktasını kullanabilirsiniz. 

Örneğin, *App.config* dosyasında aşağıdaki ayarları belirtmek için quickstart'ta oluşturulan .NET Framework konsolu uygulamasını güncelleştirebilirsiniz:

```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" endpoint="${Endpoint}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="Endpoint" value ="Set via an environment variable - for example, dev, test, staging, or production endpoint." />
    </appSettings>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Uygulama Yapılandırması'na erişimi kolaylaştırmak ve uygulamanızın kimlik bilgilerini iyileştirmek için Azure yönetilen bir kimlik eklediniz. Uygulama Yapılandırmasını nasıl kullanacağınız hakkında daha fazla bilgi edinmek için Azure CLI örneklerine devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
