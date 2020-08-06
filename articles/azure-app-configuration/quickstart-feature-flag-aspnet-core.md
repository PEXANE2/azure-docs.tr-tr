---
title: ASP.NET Core özellik bayrakları eklemeye yönelik hızlı başlangıç
description: ASP.NET Core uygulamalarına özellik bayrakları ekleyin ve Azure Uygulama yapılandırması 'nı kullanarak bunları yönetin
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: lcozzens
ms.openlocfilehash: a25a40346d588f56028bf08294b070823b729e25
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87760150"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı başlangıç: ASP.NET Core uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, Azure Uygulama yapılandırması 'nı kullanarak bir ASP.NET Core uygulamasında Özellik yönetiminin uçtan uca bir uygulamasını oluşturacaksınız. Tüm özellik Bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için uygulama yapılandırma hizmeti kullanacaksınız. 

.NET Core Özellik Yönetimi kitaplıkları Framework 'ü kapsamlı özellik bayrağı desteğiyle genişletir. Bu kitaplıklar, .NET Core yapılandırma sisteminin üzerine kurulmuştur. Bunlar, .NET Core yapılandırma sağlayıcısı aracılığıyla uygulama yapılandırmasıyla sorunsuz bir şekilde tümleşir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download).

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Özellik **Yöneticisi**  >  **+ Ekle** ' yi seçerek Özellik bayrağı ekleyin `Beta` .

    > [!div class="mx-imgBorder"]
    > ![Beta adlı özellik bayrağını etkinleştir](media/add-beta-feature-flag.png)

    `label`Şimdilik tanımsız bırakın. Yeni özellik bayrağını kaydetmek için **Uygula** ' yı seçin.

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimi 'ni (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanın. Visual Studio yerine .NET Core CLI kullanmanın avantajı, .NET Core CLI Windows, macOS ve Linux platformları arasında kullanılabilir.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıç için *Testfeatureflags*olarak adlandırın.

1. Yeni klasörde, yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

   ```    
   dotnet new mvc --no-https
   ```

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Gizli dizi Yöneticisi 'ni kullanmak için `UserSecretsId` *. csproj* dosyanıza bir öğesi ekleyin.

1. *. Csproj* dosyasını açın.

1.  `UserSecretsId`Burada gösterildiği gibi bir öğe ekleyin. Aynı GUID 'i kullanabilir veya bu değeri kendi kendinizinkini kullanarak değiştirebilirsiniz.

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`, .NET Core 3,0 ' de yer alır.  Ortamınıza göre doğru söz dizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

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

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

        <PropertyGroup>
            <TargetFramework>netcoreapp3.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>

    </Project>
    ```
    ---

1. *. Csproj* dosyasını kaydedin.

Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

> [!TIP]
> Gizli yönetici hakkında daha fazla bilgi edinmek için lütfen [ASP.NET Core 'de geliştirme sırasında uygulama gizli dizileri Için güvenli depolama](https://docs.microsoft.com/aspnet/core/security/app-secrets)bölümüne bakın.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. `Microsoft.Azure.AppConfiguration.AspNetCore` `Microsoft.FeatureManagement.AspNetCore` Aşağıdaki komutları çalıştırarak ve NuGet paketlerine başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    dotnet add package Microsoft.FeatureManagement.AspNetCore
    ```

1. Projenizin paketlerini geri yüklemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet restore
    ```

1. Gizli dizi adlı **ConnectionString: AppConfig** adında bir gizli dizi ekleyin.

    Bu gizli anahtar, uygulama yapılandırma deponuza erişmek için bağlantı dizesini içerir. `<your_connection_string>`Aşağıdaki komutta bulunan değeri, uygulama yapılandırma deponuzdaki bağlantı dizesiyle değiştirin. Bağlantı dizesini Azure portal **erişim tuşları** altında bulabilirsiniz.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    Gizli dizi Yöneticisi yalnızca Web uygulamasını yerel olarak test etmek için kullanılır. Uygulamayı [Azure App Service](https://azure.microsoft.com/services/app-service)dağıtırken, örneğin, bağlantı dizesini depolamak Için gizli yönetici kullanmak yerine App Service **bağlantı dizeleri** adlı bir uygulama ayarı kullanırsınız.

    Bu gizli dizi ile App Configuration API 'sini kullanabilirsiniz. İki nokta (:) yapılandırma adında, desteklenen tüm platformlarda uygulama yapılandırma API 'SI ile birlikte kullanılır. Bkz. [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration).

1. *Program.cs*içinde `CreateWebHostBuilder` yöntemini çağırarak uygulama yapılandırmasını kullanmak üzere yöntemi güncelleştirin `config.AddAzureAppConfiguration()` .

    > [!IMPORTANT]
    > `CreateHostBuilder``CreateWebHostBuilder`, .NET Core 3,0 ' de yer alır.  Ortamınıza göre doğru söz dizimini seçin.

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)

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

    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)

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

1. *Startup.cs*'i açın ve .NET Core Feature Manager 'a başvurular ekleyin:

    ```csharp
    using Microsoft.FeatureManagement;
    ```

1. Yöntemini `ConfigureServices` çağırarak özellik bayrağı desteği eklemek için yöntemini güncelleştirin `services.AddFeatureManagement()` . İsteğe bağlı olarak, özelliği çağırarak özellik bayraklarıyla kullanılacak herhangi bir filtre ekleyebilirsiniz `services.AddFeatureFilter<FilterType>()` :

    #### <a name="net-core-2x"></a>[.NET Core 2. x](#tab/core2x)
    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);        
        services.AddFeatureManagement();
    }
    ```
    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)
    ```csharp    
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        services.AddSingleton(Configuration).AddFeatureManagement();
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
    #### <a name="net-core-3x"></a>[.NET Core 3. x](#tab/core3x)
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

1. *Controllers* dizinine *BetaController.cs* ekleyin:

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

1. *Görünümler* dizininde *_ViewImports. cshtml* dosyasını açın ve Feature Manager etiketi yardımcısını ekleyin:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

1. *Görünümler*paylaşılan dizininde *_Layout. cshtml* dosyasını açın \\ *Shared* ve `<nav>` altındaki barkodu `<body>`  >  `<header>` aşağıdaki kodla değiştirin:

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

1. *Görünümler* altında bir *Beta* dizini oluşturun ve buna *Index. cshtml* ekleyin:

    ```html
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>
        This is the beta website.
    </h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak derleyin ve çalıştırın

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğu 'nda aşağıdaki komutu çalıştırın:

    ```
    dotnet build
    ```

1. Oluşturma başarıyla tamamlandıktan sonra, Web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```
    dotnet run
    ```

1. Bir tarayıcı penceresi açın ve `https://localhost:5000` yerel olarak barındırılan Web uygulaması için varsayılan URL olan öğesine gidin.
    Azure Cloud Shell çalışıyorsanız, *Web önizleme* düğmesini ve ardından *Yapılandır*' ı seçin.  İstendiğinde, 5000 numaralı bağlantı noktasını seçin.

    ![Web önizlemesi düğmesini bulun](./media/quickstarts/cloud-shell-web-preview.png)

    Tarayıcınızın aşağıdaki görüntüye benzer bir sayfa görüntülemesi gerekir.
    ![Hızlı başlangıç uygulaması başlatma yerel](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Özellik Yöneticisi**' ni seçin ve **Beta** anahtarının durumunu **Açık**olarak değiştirin.

1. Komut istemine geri dönün ve ' e basarak çalışan `dotnet` işlemi iptal edin `Ctrl-C` .  Kullanarak uygulamanızı yeniden başlatın `dotnet run` .

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfasını yenileyin.

    ![Hızlı başlangıç uygulaması başlatma yerel](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yeni bir uygulama yapılandırma deposu oluşturdunuz ve bunu, [Özellik Yönetimi kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664)aracılığıyla bir ASP.NET Core Web uygulamasındaki özellikleri yönetmek için kullandınız.

- [Özellik yönetimi](./concept-feature-management.md)hakkında daha fazla bilgi edinin.
- [Özellik bayraklarını yönetin](./manage-feature-flags.md).
- [ASP.NET Core uygulamasında Özellik bayraklarını kullanın](./use-feature-flags-dotnet-core.md).
- [ASP.NET Core uygulamasında dinamik yapılandırma kullanma](./enable-dynamic-configuration-aspnet-core.md)
