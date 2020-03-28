---
title: "Öğretici: ASP.NET Core'da Uygulama Yapılandırması dinamik yapılandırmayı kullanın"
titleSuffix: Azure App Configuration
description: Bu eğitimde, ASP.NET Core uygulamaları için yapılandırma verilerini dinamik olarak nasıl güncelleştirdiğinizi öğreneceksiniz
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: e9df6d2e7a8219d16e7b60f7c3b8d826a87e6110
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348846"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulamasında dinamik yapılandırmayı kullanın

ASP.NET Core çeşitli kaynaklardan yapılandırma verilerini okuyabilen takılabilir bir yapılandırma sistemine sahiptir. Bir uygulamanın yeniden başlatılmasına neden olmadan değişiklikleri dinamik olarak işleyebilir. ASP.NET Core, .NET sınıflarını güçlü bir şekilde yazmak için yapılandırma ayarlarının bağlanmasını destekler. Çeşitli `IOptions<T>` desenleri kullanarak kodunuza enjekte eder. Bu desenlerden biri, özellikle, `IOptionsSnapshot<T>`temel veriler değiştiğinde uygulamanın yapılandırmasını otomatik olarak yeniden yükler. Azure Uygulama `IOptionsSnapshot<T>` Yapılandırması'nda depolanan en son yapılandırmaya erişmek için uygulamanızdaki denetleyicilere enjekte edebilirsiniz.

Ayrıca, bir ara yazılım kullanarak bir dizi yapılandırma ayarını dinamik olarak yenilemek için App Configuration ASP.NET Core istemci kitaplığını da ayarlayabilirsiniz. Yapılandırma ayarları, web uygulaması istek aldığı sürece yapılandırma deposuyla her seferinde güncellenir.

Uygulama Yapılandırması, yapılandırma mağazasına çok fazla çağrı yapılmasını önlemek için her ayarı otomatik olarak önbelleğe ayarı razlar. Yenileme işlemi, yapılandırma deposundaki değeri değişse bile bu ayarı güncelleştirmek için bir ayarın önbelleğe alınmış değerinin süresi dolana kadar bekler. Varsayılan önbellek son kullanma süresi 30 saniyedir. Gerekirse bu son kullanma süresini geçersiz kılabilirsiniz.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmelerini nasıl uygulayabileceğinizi gösterir. Bu quickstarts tanıtılan web uygulaması üzerine oluşturur. Devam etmeden önce, [önce App Configuration ile ASP.NET Core uygulaması oluşturun'u](./quickstart-aspnet-core-app.md) tamamla.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarında kullanılabilen mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Uygulama yapılandırma mağazasındaki değişikliklere yanıt olarak yapılandırmasını güncelleştirecek şekilde uygulamanızı ayarlayın.
> * Uygulamanızın denetleyicilerine en son yapılandırmayı enjekte edin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyapmak için [,.NET Core SDK'yı](https://dotnet.microsoft.com/download)yükleyin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Devam etmeden önce, [önce App Configuration ile ASP.NET Core uygulaması oluşturun'u](./quickstart-aspnet-core-app.md) tamamla.

## <a name="add-a-sentinel-key"></a>Sentinel anahtarı ekleme

*Sentinel anahtarı,* yapılandırma değiştiğinde sinyal vermek için kullanılan özel bir anahtardır. Uygulamanız değişiklikler için sentinel anahtarını izler. Bir değişiklik algılandığında, tüm yapılandırma değerlerini yenilersiniz. Bu yaklaşım, uygulamanız tarafından Uygulama Yapılandırması'na yapılan toplam istek sayısını, değişiklikler için tüm anahtarları izlemekle karşılaştırıldığında azaltır.

1. Azure portalında Yapılandırma **Gezgini > > Anahtar Değeri Oluştur'u**seçin.

1. **Anahtar**için, *TestApp girin:Ayarlar:Sentinel*. **Değer**için , 1 girin. **Etiket** ve **İçerik türünü** boş bırakın.

1. **Uygula**’yı seçin.

## <a name="reload-data-from-app-configuration"></a>Uygulama Yapılandırmasından verileri yeniden yükleme

1. `Microsoft.Azure.AppConfiguration.AspNetCore` Aşağıdaki komutu çalıştırarak NuGet paketine bir başvuru ekleyin:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

1. *Program.cs*açın ve `CreateWebHostBuilder` yöntemi eklemek `config.AddAzureAppConfiguration()` için yöntemi güncelleştirin.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .ConfigureRefresh(refresh =>
                                {
                                    refresh.Register("TestApp:Settings:Sentinel", refreshAll: true)
                                           .SetCacheExpiration(new TimeSpan(0, 5, 0));
                                });
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
                    config.AddAzureAppConfiguration(options =>
                    {
                        options.Connect(settings["ConnectionStrings:AppConfig"])
                               .ConfigureRefresh(refresh =>
                                    {
                                        refresh.Register("TestApp:Settings:Sentinel", refreshAll: true)
                                               .SetCacheExpiration(new TimeSpan(0, 5, 0));
                                    });
                    });
                })
            .UseStartup<Startup>());
    ```
    ---

    Yöntem, `ConfigureRefresh` bir yenileme işlemi tetiklendiğinde, yapılandırma verilerini Uygulama Yapılandırma deposuyla güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. Yöntemin `refreshAll` `Register` parametresi, sentinel anahtarı değişirse tüm yapılandırma değerlerinin yenilenmesi gerektiğini gösterir.

    Ayrıca, `SetCacheExpiration` yöntem varsayılan önbellek son kullanma süresini geçersiz kakarak bunun yerine 5 dakikalık bir süre belirtir. Bu, Uygulama Yapılandırması'na yapılan istek sayısını azaltır.

    > [!NOTE]
    > Sınama amacıyla önbellek son kullanma süresini düşürmek isteyebilirsiniz.

    Bir yenileme işlemini gerçekten tetiklemek için, herhangi bir değişiklik olduğunda yapılandırma verilerini yenilemek için uygulama için bir yenileme ara yazılımını yapılandırmanız gerekir. Bunu daha sonraki bir adımda nasıl yapacağınızı göreceksiniz.

2. Yeni `Settings` bir sınıfı tanımlayan ve uygulayan *bir Settings.cs* dosyası ekleyin.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. *Startup.cs*açın ve `IServiceCollection.Configure<T>` yapılandırma `ConfigureServices` verilerini `Settings` sınıfa bağlamak için yöntemde kullanın.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));
        services.AddControllersWithViews();
    }
    ```
    ---

4. ASP.NET `Configure` Core web `UseAzureAppConfiguration` uygulaması istek almaya devam ederken yenileme için kayıtlı yapılandırma ayarlarının güncellenmesi için ara yazılımı ekleyerek yöntemi güncelleştirin.


    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        app.UseMvc();
    }
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }

            // Add the following line:
            app.UseAzureAppConfiguration();

            app.UseHttpsRedirection();
            
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");
            });
    }
    ```
    ---
    
    Ara yazılım, ASP.NET Core web `AddAzureAppConfiguration` uygulaması `Program.cs` tarafından alınan her istek için bir yenilemeyi tetiklemek için yöntemde belirtilen yenileme yapılandırmasını kullanır. Her istek için bir yenileme işlemi tetiklenir ve istemci kitaplığı, kayıtlı yapılandırma ayarı için önbelleğe alınmış değerin süresi dolup olmadığını denetler. Süresi dolmuşsa, yenilenir.

## <a name="use-the-latest-configuration-data"></a>En son yapılandırma verilerini kullanma

1. Denetleyiciler dizininde *HomeController.cs* açın ve `Microsoft.Extensions.Options` pakete bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Options;
    ```

2. Bağımlılık `HomeController` enjeksiyonu `Settings` yoluyla almak için sınıfı güncelleştirin ve değerlerinden yararlanın.

    #### <a name="net-core-2x"></a>[.NET Çekirdek 2.x](#tab/core2x)

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

    #### <a name="net-core-3x"></a>[.NET Çekirdek 3.x](#tab/core3x)

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        private readonly ILogger<HomeController> _logger;

        public HomeController(ILogger<HomeController> logger, IOptionsSnapshot<Settings> settings)
        {
            _logger = logger;
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }

        // ...
    }
    ```
    ---



3. Görünümler > Giriş dizininde *Index.cshtml'i* açın ve içeriğini aşağıdaki komut dosyasıyla değiştirin:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"]px;
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Uygulamayı yerel olarak oluşturma ve çalıştırma

1. .NET Core CLI kullanarak uygulamayı oluşturmak için komut kabuğunda aşağıdaki komutu çalıştırın:

        dotnet build

1. Yapı başarıyla tamamlandıktan sonra, web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run
1. Bir tarayıcı penceresi açın ve çıktıda `dotnet run` gösterilen URL'ye gidin.

    ![Hızlı başlatma uygulamasını yerel olarak başlatma](./media/quickstarts/aspnet-core-app-launch-local-before.png)

1. [Azure portalında](https://portal.azure.com)oturum açın. **Tüm kaynakları**seçin ve hızlı başlangıçta oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Configuration Explorer'ı**seçin ve aşağıdaki tuşların değerlerini güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Ayarlar:BackgroundColor | green |
    | TestApp:Ayarlar:FontColor | ışıkGri |
    | TestApp:Ayarlar:Mesaj | Azure Uygulama Yapılandırmasından veriler - şimdi canlı güncellemelerle! |
    | TestApp:Ayarlar:Sentinel | 2 |

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfasını yenileyin. Değişikliklerin yansıtılması için birden fazla kez yenilemeniz gerekebilir.

    ![Güncelleştirilmiş hızlı başlatma uygulamasını yerel olarak başlatma](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, ASP.NET Core web uygulamanızın Uygulama Yapılandırması'ndan yapılandırma ayarlarını dinamik olarak yenilemesini sağladınız. Uygulama Yapılandırması'na erişimi kolaylaştırmak için Azure tarafından yönetilen bir kimliği nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./howto-integrate-azure-managed-service-identity.md)
