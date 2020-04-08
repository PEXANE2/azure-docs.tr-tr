---
title: ASP.NET Core'a özellik bayrakları eklemek için hızlı başlatma
description: ASP.NET Core uygulamalarına özellik bayrakları ekleyin ve Azure Uygulama Yapılandırmasını kullanarak bunları yönetin
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: lcozzens
ms.openlocfilehash: b3579d12981e2b0add916a280bac7b4f9392d8ba
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80803152"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı başlatma: ASP.NET Core uygulamasına özellik bayrakları ekleme

Bu hızlı başlatmada, Azure Uygulama Yapılandırmasını kullanarak ASP.NET Core uygulamasında özellik yönetiminin uçtan uca bir uygulamasını oluşturursunuz. Tüm özellik bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için Uygulama Yapılandırma hizmetini kullanırsınız. 

.NET Core Özellik Yönetimi kitaplıkları, kapsamlı özellik bayrak desteği yle çerçeveyi genişletir. Bu kitaplıklar .NET Core yapılandırma sisteminin üzerine inşa edilmiştir. .NET Core yapılandırma sağlayıcısı aracılığıyla App Configuration ile sorunsuz bir şekilde entegre olurlar.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
- [.NET Çekirdek SDK](https://dotnet.microsoft.com/download).

## <a name="create-an-app-configuration-store"></a>Uygulama Yapılandırma mağazası oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. **Özellik Yöneticisi** > **+Ekle** adlı bir `Beta`özellik bayrağı eklemek için seçin.

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştirme](media/add-beta-feature-flag.png)

    Şimdilik `label` tanımsız bırak. Yeni özellik bayrağını kaydetmek için **Uygula'yı** seçin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanın. Visual Studio yerine .NET Core CLI'yi kullanmanın avantajı ,NET Core CLI'nin Windows, macOS ve Linux platformlarında kullanılabilmesidir.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıç için, *testfeatureFlags*adını.

1. Yeni klasörde, yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

   ```    
   dotnet new mvc --no-https
   ```

## <a name="add-secret-manager"></a>Gizli Yönetici Ekle

Gizli Yönetici'yi kullanmak `UserSecretsId` için *.csproj* dosyanıza bir öğe ekleyin.

1. *.csproj* dosyasını açın.

1.  Burada `UserSecretsId` gösterildiği gibi bir öğe ekleyin. Aynı GUID'i kullanabilir veya bu değeri kendi GUID'inizle değiştirebilirsiniz.

    > [!IMPORTANT]
    > `CreateHostBuilder`.NET `CreateWebHostBuilder` Core 3.0'ın yerini alır.  Ortamınıza göre doğru sözdizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>

        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>

    </Project>
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

        <PropertyGroup>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>

    </Project>
    ```
    ---

1. *.csproj* dosyasını kaydedin.

Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

> [!TIP]
> Secret Manager hakkında daha fazla bilgi edinmek için lütfen [ASP.NET Core'da uygulama sırlarının güvenli bir şekilde saklanması bölümüne](https://docs.microsoft.com/aspnet/core/security/app-secrets)bakın.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama Yapılandırma mağazasına bağlanma

1. Aşağıdaki komutları `Microsoft.Azure.AppConfiguration.AspNetCore` çalıştırarak ve `Microsoft.FeatureManagement.AspNetCore` NuGet paketlerine referans ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    dotnet add package Microsoft.FeatureManagement.AspNetCore
    ```

1. Projeniz için paketleri geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

1. Secret Manager'a **ConnectionStrings:AppConfig** adlı bir sır ekleyin.

    Bu gizli, Uygulama Yapılandırma mağazanıza erişmek için bağlantı dizesini içerir. Aşağıdaki `<your_connection_string>` komuttaki değeri Uygulama Yapılandırma mağazanızın bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portalında **Access Keys** altında bulabilirsiniz.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    Gizli Yönetici'yi yalnızca web uygulamasını yerel olarak test etmek için kullanırsınız. Uygulamayı Azure Uygulama [Hizmeti'ne](https://azure.microsoft.com/services/app-service)dağıttığınızda (örneğin), bağlantı dizesini depolamak için Gizli Yönetici'yi kullanmak yerine Uygulama Hizmetinde **Bağlantı Dizeleri** adlı bir uygulama ayarı kullanırsınız.

    Bu gizliye Uygulama Yapılandırma API'si ile erişebilirsiniz. Bir kolon (:) desteklenen tüm platformlarda App Configuration API ile yapılandırma adında çalışır. [Ortama göre Yapılandırma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/configuration)bakın.

1. *Program.cs,* yöntemi `CreateWebHostBuilder` arayarak Uygulama Yapılandırması'nı kullanma yöntemini güncelleştirin. `config.AddAzureAppConfiguration()`

    > [!IMPORTANT]
    > `CreateHostBuilder`.NET `CreateWebHostBuilder` Core 3.0'ın yerini alır.  Ortamınıza göre doğru sözdizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                        .UseFeatureFlags();
                });
            })
            .UseStartup<Startup>();
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        webBuilder.ConfigureAppConfiguration((hostingContext, config) =>
        {
            var settings = config.Build();
            config.AddAzureAppConfiguration(options => {
                options.Connect(settings["ConnectionStrings:AppConfig"])
                    .UseFeatureFlags();
            });
        })
        .UseStartup<Startup>());
    ```
    ---

1. *Startup.cs*açın ve .NET Core özellik yöneticisine başvurular ekleyin:

    ```csharp
    using Microsoft.FeatureManagement;
    ```

1. `ConfigureServices` Yöntemi çağırarak özellik bayrağı desteği `services.AddFeatureManagement()` eklemek için yöntemi güncelleştirin. İsteğe bağlı olarak, çağırarak özellik bayrakları ile `services.AddFeatureFilter<FilterType>()`kullanılacak herhangi bir filtre ekleyebilirsiniz:

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)
    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);        
        services.AddFeatureManagement();
    }
    ```
    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)
    ```csharp    
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        services.AddFeatureManagement();
    }

    ---

1. Update the `Configure` method to add a middleware to allow the feature flag values to be refreshed at a recurring interval while the ASP.NET Core web app continues to receive requests.

    #### [.NET Core 2.x](#tab/core2x)
    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
            }

            app.UseStaticFiles();
            app.UseCookiePolicy();
            app.UseAzureAppConfiguration();
            app.UseMvc(routes =>
            {
                routes.MapRoute(
                    name: "default",
                    template: "{controller=Home}/{action=Index}/{id?}");
            });
    }
    ```
    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)
    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
            }
            app.UseStaticFiles();
            app.UseRouting();
            app.UseAuthorization();
            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");
            });
            app.UseAzureAppConfiguration();
    }
    ```
    ---

1. *MyFeatureFlags.cs* dosyası ekleyin:

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

1. *Denetleyiciler* dizinine *BetaController.cs* ekleyin:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement;
    using Microsoft.FeatureManagement.Mvc;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [FeatureGate(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

1. *Görünümler* dizininde *_ViewImports.cshtml'i* açın ve özellik yöneticisi etiketi yardımcısını ekleyin:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

1. *Paylaşılan Görünümler*\\dizininde *_Layout.cshtml'i* `<nav>` açın ve `<body>`  >  `<header>` barkodun altında aşağıdaki kodla değiştirin:*Shared*

    ```html
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">TestFeatureFlags</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
            aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                <ul class="navbar-nav flex-grow-1">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                    </li>
                    <feature name="Beta">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Beta" asp-action="Index">Beta</a>
                    </li>
                    </feature>
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    ```

1. *Görünümler* altında bir *Beta* dizini oluşturun ve buna *Index.cshtml* ekleyin:

    ```html
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>
        This is the beta website.
    </h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. .NET Core CLI kullanarak uygulamayı oluşturmak için komut kabuğunda aşağıdaki komutu çalıştırın:

    ```
    dotnet build
    ```

1. Yapı başarıyla tamamlandıktan sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```
    dotnet run
    ```

1. Bir tarayıcı penceresi açın `https://localhost:5000`ve yerel olarak barındırılan web uygulamasının varsayılan URL'si olan adrese gidin.
    Azure Bulut Kabuğu'nda çalışıyorsanız, Web *Önizleme* düğmesini ve ardından *Yapılandırma'yı*seçin.  İstendiğinde, bağlantı noktası 5000'i seçin.

    ![Web Önizleme düğmesini bulma](./media/quickstarts/cloud-shell-web-preview.png)

    Tarayıcınız aşağıdaki resme benzer bir sayfa görüntülemelidir.
    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynakları**seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Özellik Yöneticisi'ni**seçin ve **Beta** tuşu'nun durumunu **On**olarak değiştirin.

1. Komut istemine dönün ve `dotnet` 'ye `Ctrl-C`basarak çalıştırma işlemini iptal edin.  Uygulamanızı kullanarak `dotnet run`yeniden başlatın.

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfasını yenileyin.

    ![Quickstart uygulaması yerel başlatmak](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Uygulama Yapılandırma mağazası oluşturdunuz ve [Özellik Yönetimi kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla ASP.NET Core web uygulamasındaki özellikleri yönetmek için kullandınız.

- [Özellik yönetimi](./concept-feature-management.md)hakkında daha fazla bilgi edinin.
- [Özellik bayraklarını yönetin.](./manage-feature-flags.md)
- [ASP.NET Core uygulamasında özellik bayraklarını kullanın.](./use-feature-flags-dotnet-core.md)
- [ASP.NET Core uygulamasında dinamik yapılandırma yı kullanma](./enable-dynamic-configuration-aspnet-core.md)
