---
title: ASP.NET Core özellik bayrakları eklemeye yönelik hızlı başlangıç | Microsoft Docs
description: ASP.NET Core uygulamalara özellik bayrakları eklemeye ve bunları Azure Uygulama yapılandırmasında yönetmeye yönelik bir hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 6f9094a52ff3558fa8d1f2fee1d80ed8eb09a416
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076335"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı Başlangıç: ASP.NET Core uygulamasına özellik bayrakları ekleme

Bu hızlı başlangıçta, özellik yönetiminin uçtan uca bir uygulamasını oluşturmak için Azure uygulama yapılandırmasını bir ASP.NET Core Web uygulamasına katabilirsiniz. Tüm özellik Bayraklarınızı merkezi olarak depolamak ve durumlarını denetlemek için uygulama yapılandırma hizmetini kullanabilirsiniz. 

.NET Core Özellik Yönetimi kitaplıkları Framework 'ü kapsamlı özellik bayrağı desteğiyle genişletir. Bu kitaplıklar, .NET Core yapılandırma sisteminin üzerine kurulmuştur. Bunlar, .NET Core yapılandırma sağlayıcısı aracılığıyla uygulama yapılandırmasıyla sorunsuz bir şekilde tümleşir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [.NET Core SDK](https://dotnet.microsoft.com/download).

## <a name="create-an-app-configuration-store"></a>Uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Aşağıdaki özellik bayraklarını eklemek için **Özellik Yöneticisi** >  **+ Ekle** ' yi seçin:

    | Anahtar | State |
    |---|---|
    | Beta | Kapalı |

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için [.NET Core komut satırı arabirimini (CLI)](https://docs.microsoft.com/dotnet/core/tools/) kullanabilirsiniz. Visual Studio yerine .NET Core CLI kullanmanın avantajı, .NET Core CLI Windows, macOS ve Linux platformları arasında kullanılabilir.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıç için *Testfeatureflags*olarak adlandırın.

1. Yeni klasörde, yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

   ```    
   dotnet new mvc
   ```

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Gizli dizi [Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) projenize ekleyin. Gizli dizi Yöneticisi Aracı, geliştirme çalışması için hassas verileri proje ağacınızdaki dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

1. *. Csproj* dosyasını açın.
1. Aşağıdaki örnekte `UserSecretsId` gösterildiği gibi bir öğe ekleyin ve değerini, genellikle GUID olan kendi değeri ile değiştirin:

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

1. Dosyayı kaydedin.

## <a name="connect-to-an-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanma

1. Aşağıdaki komutları çalıştırarak `Microsoft.Azure.AppConfiguration.AspNetCore` `Microsoft.FeatureManagement.AspNetCore` ve NuGet paketlerine başvuru ekleyin:

    ```
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-009470001-12
    dotnet add package Microsoft.FeatureManagement.AspNetCore --version 1.0.0-preview-009000001-1251
    ```

1. Projenizin paketlerini geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    dotnet restore
    ```

1. Gizli dizi adlı **ConnectionString: AppConfig** adında bir gizli dizi ekleyin.

    Bu gizli anahtar, uygulama yapılandırma deponuza erişmek için bağlantı dizesini içerir. Aşağıdaki komutta bulunan değeri, uygulama yapılandırma deponuzdaki bağlantı dizesiyle değiştirin. `<your_connection_string>`

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    Gizli dizi Yöneticisi yalnızca Web uygulamasını yerel olarak test etmek için kullanılır. Uygulamayı [Azure App Service](https://azure.microsoft.com/services/app-service)dağıtırken, örneğin, bağlantı dizesini depolamak Için gizli yönetici kullanmak yerine App Service **bağlantı dizeleri** adlı bir uygulama ayarı kullanırsınız.

    Bu gizli dizi ile App Configuration API 'sini kullanabilirsiniz. İki nokta (:) yapılandırma adında, desteklenen tüm platformlarda uygulama yapılandırma API 'SI ile birlikte kullanılır. Bkz. [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration).

1. *Program.cs*'i açın ve .NET Core uygulama yapılandırma sağlayıcısına bir başvuru ekleyin:

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Yöntemini çağırarak uygulama yapılandırmasını kullanmak için yöntemigüncelleştirin.`CreateWebHostBuilder` `config.AddAzureAppConfiguration()`

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

1. *Startup.cs*'i açın ve .NET Core Feature Manager 'a başvurular ekleyin:

    ```csharp
    using Microsoft.FeatureManagement;
    ```

1. Yöntemini çağırarak özellik bayrağı desteği eklemek için yönteminigüncelleştirin.`ConfigureServices` `services.AddFeatureManagement()` İsteğe bağlı olarak, özelliği çağırarak `services.AddFeatureFilter<FilterType>()`Özellik bayraklarıyla kullanılacak herhangi bir filtre ekleyebilirsiniz:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

1. ASP.NET Core Web uygulaması istekleri almaya devam ederken, özellik bayrağı değerlerinin yinelenen bir aralıkta yenilenmesini sağlamak üzere bir ara yazılım eklemek için yöntemigüncelleştirin.`Configure`

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();
        app.UseMvc();
    }
    ```

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

1. *Görünümler* dizininde *_Viewwimports. cshtml* dosyasını açın ve Feature Manager etiketi yardımcısını ekleyin:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

1. *Görünümler* `<body>` `<nav>`  >  `<header>` paylaşılandizininde_Layout.cshtmldosyasınıaçınvealtındakibarkoduaşağıdakikodladeğiştirin:\\

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

1. Bir tarayıcı penceresi açın ve yerel olarak barındırılan `https://localhost:5001`Web uygulaması için varsayılan URL olan öğesine gidin.

    ![Hızlı başlangıç uygulaması başlatma yerel](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tüm kaynaklar**' ı seçin ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneğini seçin.

1. **Özellik Yöneticisi**' ni seçin ve **Beta** anahtarının durumunu **Açık**olarak değiştirin:

    | Anahtar | State |
    |---|---|
    | Beta | Açık |

1. Komut istemine geri dönüp çalışan `Ctrl-C` `dotnet` işlemi iptal etmek için tuşuna basarak ve sonra yeniden çalıştırarak `dotnet run`uygulamanızı yeniden başlatın.

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
