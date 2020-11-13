---
title: ASP.NET Core özellik bayrakları eklemeye yönelik hızlı başlangıç
description: ASP.NET Core uygulamalarına özellik bayrakları ekleyin ve Azure Uygulama yapılandırması 'nı kullanarak bunları yönetin
author: lisaguthrie
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 09/28/2020
ms.author: lcozzens
ms.openlocfilehash: 88481346f22176b8e307b53774b42d753838f90b
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94554832"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı başlangıç: ASP.NET Core uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, Azure Uygulama yapılandırması 'nı kullanarak bir ASP.NET Core uygulamasında Özellik yönetiminin uçtan uca bir uygulamasını oluşturacaksınız. Tüm özellik Bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için uygulama yapılandırma hizmetini kullanacaksınız. 

.NET Core Özellik Yönetimi kitaplıkları Framework 'ü kapsamlı özellik bayrağı desteğiyle genişletir. Bu kitaplıklar, .NET Core yapılandırma sisteminin üzerine kurulmuştur. Bunlar, .NET Core yapılandırma sağlayıcısı aracılığıyla uygulama yapılandırmasıyla sorunsuz bir şekilde tümleşir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/dotnet)
* [.NET Core SDK](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE[Azure App Configuration resource creation steps](../../includes/azure-app-configuration-create.md)]

8. **Operations**  >  **Feature Manager**  >  **Ekle** ' yi seçerek *Beta* adlı bir özellik bayrağı ekleyin.

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştir](media/add-beta-feature-flag.png)

    **Etiketi** şimdilik boş bırakın. Yeni özellik bayrağını kaydetmek için **Uygula** ' yı seçin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC projesi oluşturmak için [.NET Core komut satırı arabirimi 'ni (CLI)](/dotnet/core/tools) kullanın. Visual Studio yerine .NET Core CLI kullanmanın avantajı, .NET Core CLI Windows, macOS ve Linux platformları arasında kullanılabilir.

Yeni bir *Testfeatureflags* KLASÖRÜNDE ASP.NET Core MVC projesi oluşturmak için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet new mvc --no-https --output TestFeatureFlags
```

[!INCLUDE[Add Secret Manager support to an ASP.NET Core project](../../includes/azure-app-configuration-add-secret-manager.md)]

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Aşağıdaki komutları çalıştırarak [Microsoft. Azure. AppConfiguration. aspnetcore](https://www.nuget.org/packages/Microsoft.Azure.AppConfiguration.AspNetCore) ve [Microsoft. Featuremanagement. aspnetcore](https://www.nuget.org/packages/Microsoft.FeatureManagement.AspNetCore) NuGet paketlerini yükler:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

    ```dotnetcli
    dotnet add package Microsoft.FeatureManagement.AspNetCore
    ```

1. Aşağıdaki komutu *. csproj* dosyasıyla aynı dizinde çalıştırın. Komutu `ConnectionStrings:AppConfig` , uygulama yapılandırma deponuzu için bağlantı dizesini depolayan adlı bir gizli dizi depolamak Için gizli bir yönetici kullanır. `<your_connection_string>`Yer tutucusunu uygulama yapılandırma deponuzdaki bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portal **erişim tuşları** altında bulabilirsiniz.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig "<your_connection_string>"
    ```

    Gizli dizi Yöneticisi yalnızca Web uygulamasını yerel olarak test etmek için kullanılır. Uygulama [Azure App Service](https://azure.microsoft.com/services/app-service/web)dağıtıldığında, bağlantı dizesini depolamak Için gizli yönetici yerine App Service **bağlantı dizeleri** uygulama ayarını kullanın.

    .NET Core Yapılandırma API 'sini kullanarak bu gizli dizi ile erişin. İki nokta üst üste ( `:` ) yapılandırma adında desteklenen tüm platformlarda Yapılandırma API 'si ile birlikte kullanılır. Daha fazla bilgi için bkz. [yapılandırma anahtarları ve değerleri](/aspnet/core/fundamentals/configuration#configuration-keys-and-values).

1. *Program.cs* içinde `CreateWebHostBuilder` yöntemini çağırarak uygulama yapılandırmasını kullanmak üzere yöntemi güncelleştirin `AddAzureAppConfiguration` .

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`.NET Core 3. x içindeki yerini alır. Ortamınıza göre doğru söz dizimini seçin.

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
                webBuilder.ConfigureAppConfiguration(config =>
                {
                    var settings = config.Build();
                    var connection = settings.GetConnectionString("AppConfig");
                    config.AddAzureAppConfiguration(options =>
                        options.Connect(connection).UseFeatureFlags());
                }).UseStartup<Startup>());
    ```

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration(config =>
               {
                   var settings = config.Build();
                   var connection = settings.GetConnectionString("AppConfig");
                   config.AddAzureAppConfiguration(options =>
                       options.Connect(connection).UseFeatureFlags());
               }).UseStartup<Startup>();
    ```

    ---

    Önceki değişiklikten sonra, [uygulama yapılandırması için yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664) .NET Core Yapılandırma API 'sine kaydedilir.

1. *Startup.cs* ' de .NET Core Özellik Yöneticisi 'ne bir başvuru ekleyin:

    ```csharp
    using Microsoft.FeatureManagement;
    ```

1. Yöntemini `Startup.ConfigureServices` çağırarak özellik bayrağı desteği eklemek için yöntemini güncelleştirin `AddFeatureManagement` . İsteğe bağlı olarak, özelliği çağırarak özellik bayraklarıyla kullanılacak herhangi bir filtre ekleyebilirsiniz `AddFeatureFilter<FilterType>()` :

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

    ```csharp    
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        services.AddFeatureManagement();
    }
    ```

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc()
            .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
        services.AddFeatureManagement();
    }
    ```

    ---

1. Kök proje dizinine aşağıdaki kodla bir *MyFeatureFlags.cs* dosyası ekleyin:

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

1. *Controllers* dizinine aşağıdaki kodla bir *BetaController.cs* dosyası ekleyin:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement;
    using Microsoft.FeatureManagement.Mvc;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager) =>
                _featureManager = featureManager;

            [FeatureGate(MyFeatureFlags.Beta)]
            public IActionResult Index() => View();
        }
    }
    ```

1. *Görünümler/_ViewImports. cshtml* 'de, bir yönergesi kullanarak Feature Manager etiketi yardımcısını kaydedin `@addTagHelper` :

    ```cshtml
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

    Yukarıdaki kod, `<feature>` etiket Yardımcısı 'nın projenin *. cshtml* dosyalarında kullanılmasına izin verir.

1. *Görünümler* paylaşılan dizininde *_Layout. cshtml* dosyasını açın \\ *Shared* . `<nav>`Altındaki barkodu bulun `<body>`  >  `<header>` . `<feature>`Aşağıdaki Vurgulanan satırlarda gösterildiği gibi, *giriş* ve *Gizlilik* gezinti çubuğu öğeleri arasına yeni bir etiket ekleyin.

    :::code language="html" source="../../includes/azure-app-configuration-navbar.md" range="15-38" highlight="13-17":::

1. Aşağıdaki biçimlendirmeyi içeren bir *views/Beta* dizini ve *Index. cshtml* dosyası oluşturun:

    ```cshtml
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>This is the beta website.</h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğu 'nda aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

1. Oluşturma başarıyla tamamlandıktan sonra, Web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet run
    ```

1. Bir tarayıcı penceresi açın ve `http://localhost:5000` yerel olarak barındırılan Web uygulaması için varsayılan URL olan öğesine gidin. Azure Cloud Shell çalışıyorsanız, **Web önizleme** düğmesini ve ardından **Yapılandır** ' ı seçin. İstendiğinde, 5000 numaralı bağlantı noktasını seçin.

    ![Web önizlemesi düğmesini bulun](./media/quickstarts/cloud-shell-web-preview.png)

    Tarayıcınızın aşağıdaki görüntüye benzer bir sayfa görüntülemesi gerekir.

    :::image type="content" source="media/quickstarts/aspnet-core-feature-flag-local-before.png" alt-text="Değişiklikten önce yerel hızlı başlangıç uygulaması" border="true":::

1. [Azure portalında](https://portal.azure.com) oturum açın. **Tüm kaynaklar** ' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Özellik Yöneticisi** ' ni seçin. 

1. **Etkin** ' in altındaki onay kutusunu seçerek *Beta* bayrağını etkinleştirin.

1. Komut kabuğuna geri dönün. `dotnet` <kbd>CTRL + C</kbd>tuşlarına basarak çalışan işlemi iptal edin. Uygulamasını kullanarak uygulamanızı yeniden başlatın `dotnet run` .

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfasını yenileyin.

    :::image type="content" source="media/quickstarts/aspnet-core-feature-flag-local-after.png" alt-text="Değişiklikten sonra yerel hızlı başlangıç uygulaması" border="true":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE[Azure App Configuration cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu, [Özellik Yönetimi kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir ASP.NET Core Web uygulamasındaki özellikleri yönetmek için kullandınız.

* [Özellik yönetimi](./concept-feature-management.md)hakkında daha fazla bilgi edinin.
* [Özellik bayraklarını yönetin](./manage-feature-flags.md).
* [ASP.NET Core uygulamasında Özellik bayraklarını kullanın](./use-feature-flags-dotnet-core.md).
* [ASP.NET Core uygulamasında dinamik yapılandırma kullanma](./enable-dynamic-configuration-aspnet-core.md)
