---
title: Azure yönetilen kimliklerini kullanarak kimlik doğrulama
titleSuffix: Azure App Configuration
description: Azure yönetilen kimliklerini kullanarak Azure Uygulama yapılandırmasında kimlik doğrulama
ms.service: azure-app-configuration
author: lisaguthrie
ms.topic: conceptual
ms.date: 2/25/2020
ms.author: lcozzens
ms.openlocfilehash: 957fef32702f35b4b509d829eba6a41914c4fc53
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78205866"
---
# <a name="integrate-with-azure-managed-identities"></a>Azure yönetilen kimliklerle tümleştirin

Azure Active Directory [Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md) , bulut uygulamanız için gizli dizi yönetimini basitleştirir. Yönetilen bir kimlikle, kodunuz üzerinde çalıştığı Azure hizmeti için oluşturulan hizmet sorumlusunu kullanabilir. Azure Key Vault veya yerel bağlantı dizesinde depolanan ayrı kimlik bilgileri yerine yönetilen bir kimlik kullanırsınız. 

Azure Uygulama yapılandırması ve .NET Core, .NET Framework ve Java Spring istemci kitaplıklarında yerleşik olarak bulunan yönetilen kimlik desteği vardır. Bunu kullanmanız gerekmese de, yönetilen kimlik gizli dizileri içeren bir erişim belirteci gereksinimini ortadan kaldırır. Kodunuz, yalnızca hizmet uç noktasını kullanarak uygulama yapılandırma deposuna erişebilir. Bu URL 'YI, herhangi bir gizli dizi olmadan doğrudan kodunuza katıştırabilirsiniz.

Bu makalede, uygulama yapılandırmasına erişmek için yönetilen kimliğin nasıl avantajlarından faydalanabilirsiniz. Hızlı başlangıçlarda tanıtılan web uygulamasında oluşturulur. Devam etmeden önce [uygulama yapılandırmasına sahip bir ASP.NET Core uygulaması oluşturun](./quickstart-aspnet-core-app.md) .

Bu makalede ayrıca yönetilen kimliği, uygulama yapılandırmasının Key Vault başvurularıyla birlikte nasıl kullanabileceğiniz gösterilmektedir. Tek bir yönetilen kimlik ile, uygulama yapılandırmasından Key Vault ve yapılandırma değerlerinden her iki sırda sorunsuzca erişebilirsiniz. Bu özelliği araştırmak isterseniz, önce [ASP.NET Core Key Vault başvuruları kullanın](./use-key-vault-references-dotnet-core.md) .

Bu öğreticideki adımları uygulamak için herhangi bir kod düzenleyicisi kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) , Windows, MacOS ve Linux platformlarında kullanılabilen harika bir seçenektir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama yapılandırmasına yönetilen bir kimlik erişimi verin.
> * Uygulamanızı, uygulama yapılandırmasına bağlandığınızda yönetilen bir kimlik kullanacak şekilde yapılandırın.
> * İsteğe bağlı olarak, uygulama yapılandırma Key Vault başvurusu aracılığıyla Key Vault bağlandığınızda Uygulamanızı yönetilen bir kimlik kullanacak şekilde yapılandırın.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* [.NET Core SDK](https://www.microsoft.com/net/download/windows).
* [Azure Cloud Shell yapılandırıldı](https://docs.microsoft.com/azure/cloud-shell/quickstart).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="add-a-managed-identity"></a>Yönetilen kimlik ekleme

Portalda yönetilen bir kimlik ayarlamak için, önce bir uygulama oluşturun ve sonra özelliği etkinleştirin.

1. [Azure Portal](https://portal.azure.com) , normalde yaptığınız gibi bir App Services örneği oluşturun. Portalda buna gidin.

1. Sol bölmedeki **Ayarlar** grubuna aşağı kaydırın ve **kimlik**' i seçin.

1. **Sistem atandı** sekmesinde **durumu** **Açık** olarak değiştirin ve **Kaydet**' i seçin.

1. Sistem tarafından atanan yönetilen kimliği etkinleştirmek isteyip istemediğiniz sorulduğunda **Evet** yanıtını verin.

    ![App Service yönetilen kimliği ayarlama](./media/set-managed-identity-app-service.png)

## <a name="grant-access-to-app-configuration"></a>Uygulama yapılandırmasına erişim izni verme

1. [Azure Portal](https://portal.azure.com), **tüm kaynaklar** ' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposunu seçin.

1. **Erişim denetimi (IAM)** öğesini seçin.

1. **Erişim denetimi** sekmesinde, **rol atama** kartı Kullanıcı arabiriminde **Ekle** ' yi seçin.

1. **Rol**altında **uygulama yapılandırma veri okuyucusu**' nu seçin. **Erişim ata**' nın altında, **sistem tarafından atanan yönetilen kimlik**altında **App Service** ' yi seçin.

1. **Abonelik**altında Azure aboneliğinizi seçin. Uygulamanız için App Service kaynağını seçin.

1. **Kaydet**’i seçin.

    ![Yönetilen kimlik ekleme](./media/add-managed-identity.png)

1. İsteğe bağlı: Key Vault erişim vermek istiyorsanız, [yönetilen bir kimlikle Key Vault kimlik doğrulaması sağlama](https://docs.microsoft.com/azure/key-vault/managed-identity)bölümündeki yönergeleri izleyin.

## <a name="use-a-managed-identity"></a>Yönetilen kimlik kullanma

1. *Azure. Identity* paketine bir başvuru ekleyin:

    ```cli
    dotnet add package Azure.Identity
    ```

1. Uygulama yapılandırma deponuzda uç noktayı bulun. Bu URL, Azure portal mağazanın **erişim tuşları** sekmesinde listelenir.

1. *AppSettings. JSON*dosyasını açın ve aşağıdaki betiği ekleyin. Parantez dahil *\<service_endpoint >* yerine, uygulama yapılandırma deponuzu URL 'siyle değiştirin. 

    ```json
    "AppConfig": {
        "Endpoint": "<service_endpoint>"
    }
    ```

1. *Program.cs*'i açın ve `Azure.Identity` ve `Microsoft.Azure.Services.AppAuthentication` ad alanlarına bir başvuru ekleyin:

    ```csharp-interactive
    using Azure.Identity;
    ```

1. Yalnızca uygulama yapılandırmasında doğrudan depolanan değerlere erişmek istiyorsanız, `config.AddAzureAppConfiguration()` metodunu değiştirerek `CreateWebHostBuilder` yöntemini güncelleştirin.

    > [!IMPORTANT]
    > `CreateHostBuilder` `CreateWebHostBuilder` .NET Core 3,0 ' de yer alır.  Ortamınıza göre doğru söz dizimini seçin.

    ### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

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

    ### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

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

1. Hem uygulama yapılandırma değerlerini hem de Key Vault başvurularını kullanmak için, aşağıda gösterildiği gibi *program.cs* güncelleştirin. Bu kod, bir `AzureServiceTokenProvider` kullanarak yeni bir `KeyVaultClient` oluşturur ve bu başvuruyu `UseAzureKeyVault` yöntemine yapılan çağrıya geçirir.

    ### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

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

    ### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

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

    Artık diğer uygulama yapılandırma anahtarı gibi Key Vault başvurularına erişebilirsiniz. Yapılandırma sağlayıcısı, Key Vault için kimlik doğrulamak üzere yapılandırdığınız `KeyVaultClient` kullanır ve değeri alır.

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="deploy-from-local-git"></a>Yerel Git’ten dağıtım

Kudu yapı sunucusuyla uygulamanız için yerel git dağıtımını etkinleştirmenin en kolay yolu [Azure Cloud Shell](https://shell.azure.com)kullanmaktır.

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Kudu ile yerel git 'i etkinleştirme
Uygulamanız için yerel bir git deponuz yoksa, bir tane açmanız gerekir. Yerel bir Git deposunu başlatmak için uygulamanızın proje dizininden aşağıdaki komutları çalıştırın:

```cmd
git init
git add .
git commit -m "Initial version"
```

Kudu yapı sunucusuyla uygulamanız için yerel git dağıtımını etkinleştirmek üzere Cloud Shell [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) çalıştırın.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Bu komut, aşağıdaki çıktıya benzer bir işlem sağlar:

```json
{
  "url": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
}
```

### <a name="deploy-your-project"></a>Projenizi dağıtın

_Yerel Terminal penceresinde_yerel git deponuza bir Azure uzak ekleyin. _\<URL 'si >_ , [Kudu ile yerel git 'i etkinleştirdiğinizden](#enable-local-git-with-kudu)ALDıĞıNıZ git uzak 'nin URL 'siyle değiştirin.

```bash
git remote add azure <url>
```

Aşağıdaki komutla uygulamanızı dağıtmak için Azure uzak deposuna gönderin. Bir parola istendiğinde, [dağıtım kullanıcısı yapılandırma](#configure-a-deployment-user)bölümünde oluşturduğunuz parolayı girin. Azure portal oturum açmak için kullandığınız parolayı kullanmayın.

```bash
git push azure master
```

Çıkışta ASP.NET için MSBuild, Node. js için `npm install` ve Python için `pip install` gibi çalışma zamanına özgü Otomasyon görebilirsiniz.

### <a name="browse-to-the-azure-web-app"></a>Azure web uygulamasına göz atma

İçeriğin dağıtıldığını doğrulamak için tarayıcı kullanarak Web uygulamanıza gidin.

```bash
http://<app_name>.azurewebsites.net
```

![App Service 'de çalışan uygulama](../app-service/media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

## <a name="use-managed-identity-in-other-languages"></a>Yönetilen kimliği diğer dillerde kullanma

.NET Framework ve Java Spring uygulama yapılandırma sağlayıcılarının yönetilen kimlik için yerleşik desteği de vardır. Bu sağlayıcılardan birini yapılandırdığınızda, deponun URL uç noktasını tam bağlantı dizesi yerine kullanabilirsiniz. 

Örneğin, aşağıdaki ayarları *app. config* dosyasında belirtmek için hızlı başlangıçta oluşturulan .NET Framework konsol uygulamasını güncelleştirebilirsiniz:

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
Bu öğreticide, uygulama yapılandırmasına erişimi kolaylaştırmak ve uygulamanız için kimlik bilgisi yönetimini geliştirmek üzere bir Azure yönetilen kimliği ekledik. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için Azure CLı örneklerine devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
