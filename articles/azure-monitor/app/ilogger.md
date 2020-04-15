---
title: ILogger - Azure Application Insights ile .NET izleme günlüklerini keşfedin
description: ASP.NET Core ve Console uygulamalarına sahip Azure Application Insights ILogger sağlayıcısını kullanma örnekleri.
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 33effe9cfec6d766d573617ff03b58564e5b34d1
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313653"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>.NET Core ILogger günlükleri için ApplicationInsightsLoggerProvider

ASP.NET Core, farklı türde yerleşik ve üçüncü taraf günlük sağlayıcılarıyla çalışan bir günlük API'sini destekler. Günlük, **Log()** veya *ILogger* örneklerinde bunun bir varyantını arayarak yapılır. Bu makalede, ILogger konsol ve ASP.NET Core uygulamaları günlükleri yakalamak için *ApplicationInsightsLoggerProvider* nasıl kullanılacağını göstermektedir. Bu makalede ayrıca ApplicationInsightsLoggerProvider'ın diğer Application Insights telemetrisiyle nasıl entegre olduğu açıklanmaktadır.
Daha fazla bilgi için [ASP.NET Core'da Oturum Açma'ya](https://docs.microsoft.com/aspnet/core/fundamentals/logging)bakın.

## <a name="aspnet-core-applications"></a>ASP.NET Çekirdek uygulamaları

ApplicationInsightsLoggerProvider, [microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.1 (ve daha sonra) her iki yöntem üzerinden normal Application Insights izlemeyi açtığınızda varsayılan olarak etkinleştirilir:

- IWebHostBuilder (Şimdi eski) **üzerinde UseApplicationInsights** uzantısı yöntemi arayarak
- IServiceCollection'da **AddApplicationInsightsTelemetry** uzantı yöntemini arayarak

ILogger, ApplicationInsightsLoggerProvider'ın yakaladığı şeyin, toplanan diğer telemetrilerle aynı yapılandırmaya tabi olduğunu kaydeder. Onlar TelemetryInitializers ve Telemetriİşlemciler aynı seti var, aynı TelemetryChannel kullanmak, ve ilişkili ve diğer telemetri gibi aynı şekilde örneklenir. Sürüm 2.7.1 veya daha yeni sürüm kullanıyorsanız, ILogger günlüklerini yakalamak için herhangi bir işlem yapmanız gerekmez.

Yalnızca *Uyarı* veya daha yüksek ILogger günlükleri (tüm [kategorilerden)](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-3.1#log-category)varsayılan olarak Uygulama Öngörüleri'ne gönderilir. Ancak [bu davranışı değiştirmek için filtreler uygulayabilirsiniz.](#control-logging-level) **Program.cs** veya **Startup.cs**ILogger günlüklerini yakalamak için ek adımlar gereklidir. (Bkz. [ASP.NET Core uygulamalarında Startup.cs ve Program.cs iLogger günlüklerini yakalama](#capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps).)

Microsoft.ApplicationInsights.AspNet SDK'nın önceki bir sürümünü kullanıyorsanız veya applicationinsightsLoggerProvider'ı başka bir Application Insights izlemeden kullanmak istiyorsanız, aşağıdaki yordamı kullanın:

1. NuGet paketini yükleyin:

   ```xml
       <ItemGroup>
         <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
       </ItemGroup>
   ```

1. Burada gösterildiği gibi **Program.cs** değiştirin:

   ```csharp
   using Microsoft.AspNetCore;
   using Microsoft.AspNetCore.Hosting;
   using Microsoft.Extensions.Logging;

   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
         WebHost.CreateDefaultBuilder(args)
           .UseStartup<Startup>()
         .ConfigureLogging(
               builder =>
               {
                   // Providing an instrumentation key here is required if you're using
                   // standalone package Microsoft.Extensions.Logging.ApplicationInsights
                   // or if you want to capture logs from early in the application startup
                   // pipeline from Startup.cs or Program.cs itself.
                   builder.AddApplicationInsights("ikey");

                   // Optional: Apply filters to control what logs are sent to Application Insights.
                   // The following configures LogLevel Information or above to be sent to
                   // Application Insights for all categories.
                   builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                    ("", LogLevel.Information);
               }
           );
   }
   ```

Adım 2'deki kod `ApplicationInsightsLoggerProvider`yapılandırır. Aşağıdaki kod, günlükleri göndermek için `ILogger` kullanan bir örnek Denetleyici sınıfını gösterir. Günlükler Application Insights tarafından yakalanır.

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all of them will have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
                _logger.LogWarning("An example of a Warning trace..");
                _logger.LogError("An example of an Error level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

### <a name="capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps"></a>ASP.NET Core uygulamalarında Startup.cs ve Program.cs iLogger günlüklerini yakalama

> [!NOTE]
> Core 3.0 ve daha sonra ASP.NET, artık `ILogger` Startup.cs ve Program.cs enjekte etmek mümkün değildir. Daha https://github.com/aspnet/Announcements/issues/353 fazla bilgi için bkz.

Yeni ApplicationInsightsLoggerProvider, uygulama başlatma ardışık hattının başlarından itibaren günlükleri yakalayabilir. ApplicationInsightsLoggerProvider, Application Insights'ta (sürüm 2.7.1'den başlayarak) otomatik olarak etkinleştirilse de, daha sonra boru hattında ayarlanmış bir enstrümantasyon anahtarı yoktur. Bu nedenle, yalnızca **Denetleyici/diğer**sınıflardan gelen günlükler yakalanır. **Program.cs** ve **kendisi Startup.cs** başlayan her günlüğü yakalamak için ApplicationInsightsLoggerProvider için bir enstrümantasyon anahtarını açıkça etkinleştirmeniz gerekir. Ayrıca, *telemetryConfiguration* tam olarak **Program.cs** veya **Startup.cs** kendisi oturum açma değil. Yani bu günlükleri InMemoryChannel, hiçbir örnekleme ve standart telemetri initializatörleri veya işlemciler kullanan bir minimum yapılandırma olacaktır.

Aşağıdaki **örnekler, Program.cs** ve **Startup.cs**ile bu yeteneği göstermektedir.

#### <a name="example-programcs"></a>Örnek Program.cs

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        // This will be picked up by AI
        logger.LogInformation("From Program. Running the host now..");
        host.Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
        builder =>
            {
            // Providing an instrumentation key here is required if you're using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Program).FullName, LogLevel.Trace);

            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Startup).FullName, LogLevel.Trace);
            }
        );
}
```

#### <a name="example-startupcs"></a>Örnek Startup.cs

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following will be picked up by Application Insights.
        _logger.LogInformation("Logging from ConfigureServices.");
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

## <a name="migrate-from-the-old-applicationinsightsloggerprovider"></a>Eski ApplicationInsightsLoggerProvider'dan geçiş yap

Microsoft.ApplicationInsights.AspNet SDK sürümleri önce 2.7.1 artık eski bir günlük sağlayıcısı destekledi. Bu sağlayıcı, ILoggerFactory'nin **AddApplicationInsights()** uzantı yöntemi yle etkinleştirildi. İki adım içeren yeni sağlayıcıya geçiş yapmanızı öneririz:

1. Çift günlüğe kaydetmeyi önlemek için **Başlangıç.Configure()** yönteminden *ILoggerFactory.AddApplicationInsights()* çağrısını kaldırın.
2. Yeni sağlayıcı tarafından saygı göreilmeyeceklerinden, koddaki tüm filtreleme kurallarını yeniden uygulayın. *ILoggerFactory.AddApplicationInsights()* overloads minimum LogLevel veya filtre işlevleri aldı. Yeni sağlayıcıile, filtreleme günlük çerçevesinin bir parçasıdır. Application Insights sağlayıcısı tarafından yapılmaz. Yani *iLoggerFactory.AddApplicationInsights()* aşırı yükleri üzerinden sağlanan herhangi bir filtre kaldırılmalıdır. Ve filtreleme kuralları [Denetim günlük düzeyi](#control-logging-level) yönergeleriizden izleyerek sağlanmalıdır. Günlüğe kaydetmeyi filtrelemek için *appsettings.json* kullanıyorsanız, her ikisi de aynı sağlayıcı takma adı olan *ApplicationInsights'ı*kullandığından, yeni sağlayıcıyla çalışmaya devam edecektir.

Eski sağlayıcıyı kullanmaya devam edebilirsiniz. (Yalnızca ana sürüm değişikliğiyle 3 olarak kaldırılır. *xx*.) Ancak aşağıdaki nedenlerden dolayı yeni sağlayıcıya geçiş yapmanızı öneririz:

- Önceki sağlayıcı [günlük kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes)için destek yoksun. Yeni sağlayıcıda, kapsamdaki özellikler otomatik olarak toplanan telemetriye özel özellikler olarak eklenir.
- Günlükler artık uygulama başlangıç ardışık bölümünde çok daha erken yakalanabilir. **Program** ve **Başlangıç** sınıflarından günlükler artık yakalanabilir.
- Yeni sağlayıcı ile filtreleme çerçeve düzeyinde yapılır. Kullanıcı bilgileri sağlayıcısına, Konsol, Hata Ayıklama gibi yerleşik sağlayıcılar da dahil olmak üzere diğer sağlayıcılarla aynı şekilde filtre uygulayabilirsiniz. Aynı filtreleri birden çok sağlayıcıya da uygulayabilirsiniz.
- ASP.NET Core (2.0 ve sonrası) olarak, [günlük sağlayıcıları etkinleştirmek](https://github.com/aspnet/Announcements/issues/255) için önerilen yolu kendi içinde iLoggingBuilder uzantısı yöntemleri kullanarak **Program.cs.**

> [!Note]
> Yeni sağlayıcı, NETSTANDARD2.0 veya daha yenisini hedefleyen uygulamalar için kullanılabilir. [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.14.0'dan itibaren ,.NET Framework NET461 veya sonraki uygulamaları hedefleyen uygulamalar için yeni sağlayıcı da kullanılabilir. Uygulamanız .NET Core 1.1 gibi eski .NET Core sürümlerini hedefliyorsa veya .NET Framework'u NET46'dan daha az hedefliyorsa, eski sağlayıcıyı kullanmaya devam edin.

## <a name="console-application"></a>Konsol uygulaması

> [!NOTE]
> Herhangi bir Konsol Uygulamaları için Uygulama Öngörüleri (ILogger ve diğer Uygulama İstatistikleri telemetri) etkinleştirmek için kullanılabilecek [Microsoft.ApplicationInsights.WorkerService](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) adlı yeni bir Uygulama Öngörüleri SDK vardır. Bu paketi ve ilişkili talimatları [buradan](../../azure-monitor/app/worker-service.md)kullanmanız önerilir.

Aşağıdaki kod, ILogger izlemelerini Application Insights'a göndermek üzere yapılandırılan örnek bir konsol uygulamasını gösterir.

Yüklenen paketler:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />  
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create the DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(builder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // Application Insights for all categories.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("", LogLevel.Trace);
            builder.AddApplicationInsights("--YourAIKeyHere--");
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

Bu örnekte bağımsız `Microsoft.Extensions.Logging.ApplicationInsights`paket kullanır. Varsayılan olarak, bu yapılandırma Uygulama Öngörüleri veri göndermek için "çıplak minimum" TelemetriYapılandırma kullanır. Minimum, InMemoryChannel'ın kullanılan kanal olduğu anlamına gelir. Örnekleme ve standart TelemetriInitializers yok. Aşağıdaki örnekte görüldüğü gibi, bu davranış bir konsol uygulaması için geçersiz kılınabilir.

Bu ek paketi yükleyin:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Aşağıdaki bölümde, hizmetleri kullanarak varsayılan Telemetri Yapılandırması nasıl geçersiz kılını **gösterilmektedir. TelemetryConfiguration>() yöntemini yapılandırın.\<** Bu örnek, `ServerTelemetryChannel` örnekleme yi ve örneklemeyi ayarlar. TelemetriConfiguration'a özel bir ITelemetryInitializer ekler.

```csharp
    // Create the DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding Application Insights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly calling Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if the application terminates, telemetry is sent to the back end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Günlük düzeyini denetleme

Core *ILogger* infra ASP.NET [günlük filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering)uygulamak için yerleşik bir mekanizma vardır. Bu, Application Insights sağlayıcısı da dahil olmak üzere her kayıtlı sağlayıcıya gönderilen günlükleri denetlemenize olanak tanır. Filtreleme yapılandırmada (genellikle bir *appsettings.json* dosyası kullanılarak) veya kod olarak yapılabilir. Bu tesis çerçevenin kendisi tarafından sağlanmaktadır. Uygulama Öngörüleri sağlayıcısına özgü değildir.

Aşağıdaki örnekler ApplicationInsightsLoggerProvider için filtre kuralları uygular.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>appsettings.json ile yapılandırmada filtre kuralları oluşturma

ApplicationInsightsLoggerProvider için sağlayıcı diğer `ApplicationInsights`adı . *appsettings.json'un* aşağıdaki bölümü, günlük sağlayıcılarına genellikle *Uyarı* düzeyinde ve yukarıda oturum açmalarını bildirir. Daha sonra düzeyinde `ApplicationInsightsLoggerProvider` *Hata* ve yukarıda "Microsoft" ile başlayan günlük kategorileri geçersiz kılar.

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    },
    "ApplicationInsights": {
      "LogLevel": {
        "Microsoft": "Error"
      }
    }
  }
}
```

### <a name="create-filter-rules-in-code"></a>Kodda filtre kuralları oluşturma

Aşağıdaki kod snippet uyarı *ve* tüm kategorilerden yukarıda ve *Hata* ve yukarıda için "Microsoft" ile `ApplicationInsightsLoggerProvider`başlayacak kategorilerden günlükleri yapılandırır. Bu yapılandırma *appsettings.json*önceki bölümde aynıdır.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-are-the-old-and-new-versions-of-applicationinsightsloggerprovider"></a>ApplicationInsightsLoggerProvider'ın eski ve yeni sürümleri nelerdir?

[Microsoft.ApplicationInsights.AspNet SDK,](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) **ILoggerFactory** uzantı yöntemleri yle etkinleştirilen yerleşik bir ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider) dahil etti. Bu sağlayıcı sürüm 2.7.1'den eski olarak işaretlenmiştir. Bir sonraki ana sürüm değişikliğinde tamamen kaldırılacaktır. [Microsoft.ApplicationInsights.AspNetCore 2.6.1](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) paketinin kendisi eski değildir. İsteklerin, bağımlılıkların ve benzeri taleplerin izlenmesini sağlamak için gereklidir.

Önerilen alternatif yeni bağımsız paket [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), geliştirilmiş bir ApplicationInsightsLoggerProvider (Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) ve bunu etkinleştirmek için ILoggerBuilder uzantı yöntemleri içerir.

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.1 yeni pakete bağımlı dır ve ILogger'ın otomatik olarak yakalanmasını sağlar.

### <a name="why-are-some-ilogger-logs-shown-twice-in-application-insights"></a>Uygulama Bilgileri'nde neden bazı ILogger günlükleri iki kez gösteriliyor?

ApplicationInsightsLoggerProvider'ın `AddApplicationInsights` eski (şimdi eski) sürümüne sahipseniz, yineleme `ILoggerFactory`oluşabilir. Yapılandırma yönteminizde aşağıdakileri olup olmadığını kontrol **edin** ve kaldırın:

```csharp
 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
 {
     loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
     // ..other code.
 }
```

Visual Studio'dan hata ayıklama yaparken çift `EnableDebugLogger` oturum açma yla karşılaşırsanız, Aşağıdaki gibi Uygulama Öngörüleri sağlayan kodda *yanlış* olarak ayarlayın. Bu yineleme ve düzeltme yalnızca uygulamayı hata ayıklarken alakalıdır.

```csharp
 public void ConfigureServices(IServiceCollection services)
 {
     ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
     options.EnableDebugLogger = false;
     services.AddApplicationInsightsTelemetry(options);
     // ..other code.
 }
```

### <a name="i-updated-to-microsoftapplicationinsightsaspnet-sdk-version-271-and-logs-from-ilogger-are-captured-automatically-how-do-i-turn-off-this-feature-completely"></a>[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.1'e güncelledim ve ILogger'daki günlükler otomatik olarak yakalanır. Bu özelliği tamamen nasıl kapatacağım?

Günlüklerin genel olarak nasıl filtre uygulandığını görmek için [Denetim günlüğü düzeyi](../../azure-monitor/app/ilogger.md#control-logging-level) bölümüne bakın. Kapatma ApplicationInsightsLoggerProvider için: `LogLevel.None`

**Kod:**

```csharp
    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                      ("", LogLevel.None);
```

**Config olarak:**

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "None"
      }
}
```

### <a name="why-do-some-ilogger-logs-not-have-the-same-properties-as-others"></a>Neden bazı ILogger günlükleri diğerleri ile aynı özelliklere sahip değil mi?

Application Insights, diğer tüm telemetriler için kullanılan aynı Telemetri Yapılandırmasını kullanarak ILogger günlüklerini yakalar ve gönderir. Ama bir istisna var. Varsayılan olarak, **Program.cs** veya **Startup.cs**oturum açtığınızda TelemetryConfiguration tam olarak ayarlanmaz. Bu yerlerden gelen günlükler varsayılan yapılandırmaya sahip olmayacaktır, bu nedenle tüm TelemetryInitializers ve TelemetryProcessors çalıştırılamayacak.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-i-want-to-log-some-additional-custom-telemetry-manually-how-should-i-do-that"></a>Ben bağımsız paket Microsoft.Extensions.Logging.ApplicationInsights kullanıyorum ve bazı ek özel telemetri el ile günlüğe kaydetmek istiyorum. Bunu nasıl yapmalıyım?

Bağımsız paketi kullandığınızda, `TelemetryClient` DI kapsayıcısına enjekte edilmez, bu nedenle logger sağlayıcısının kullandığı yapılandırmanın yeni bir örneğini `TelemetryClient` oluşturmanız ve aşağıdaki kodun gösterdiği gibi aynı yapılandırmayı kullanmanız gerekir. Bu, aynı yapılandırmanın tüm özel telemetrilerin yanı sıra ILogger'ın telemetrileri için kullanılmasını sağlar.

```csharp
public class MyController : ApiController
{
   // This telemetryclient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Microsoft.ApplicationInsights.AspNetCore paketini Application Insights'ı etkinleştirmek için `TelemetryClient` kullanıyorsanız, doğrudan oluşturucuya ulaşmak için bu kodu değiştirin. Örneğin, bu [SSS'ye](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core#frequently-asked-questions)bakın.


### <a name="what-application-insights-telemetry-type-is-produced-from-ilogger-logs-or-where-can-i-see-ilogger-logs-in-application-insights"></a>ILogger günlüklerinden hangi Uygulama Öngörüleri telemetri türü üretilir? Veya Uygulama İstatistikleri'nde ILogger günlüklerini nerede görebiliyorum?

ApplicationInsightsLoggerProvider ILogger günlüklerini yakalar ve onlardan TraceTelemetry oluşturur. ILogger'da **Log()** yöntemine bir Özel Durum nesnesi aktarılırsa, TraceTelemetry yerine *ExceptionTelemetry* oluşturulur. Bu telemetri öğeleri, portal, analitik veya Visual Studio yerel hata ayıklayıcı dahil olmak üzere Uygulama Öngörüleri için diğer TraceTelemetry veya ExceptionTelemetry ile aynı yerlerde bulunabilir.

Her zaman TraceTelemetry göndermek isterseniz, bu snippet kullanın:```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```

### <a name="i-dont-have-the-sdk-installed-and-i-use-the-azure-web-apps-extension-to-enable-application-insights-for-my-aspnet-core-applications-how-do-i-use-the-new-provider"></a>SDK yüklü değil ve ASP.NET Çekirdek uygulamalarım için Uygulama Öngörüleri'ni etkinleştirmek için Azure Web Uygulamaları uzantısını kullanıyorum. Yeni sağlayıcıyı nasıl kullanırım? 

Azure Web Apps'taki Uygulama Öngörüleri uzantısı yeni sağlayıcıyı kullanır. Uygulamanız için *appsettings.json* dosyasındaki filtreleme kurallarını değiştirebilirsiniz.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-enabling-application-insights-provider-by-calling-builderaddapplicationinsightsikey-is-there-an-option-to-get-an-instrumentation-key-from-configuration"></a>Microsoft.Extensions.Logging.ApplicationInsights paketini kullanıyorum ve oluşturucuyu arayarak Application Insights sağlayıcısını **etkinleştiriyorum. AddApplicationInsights("ikey")**. Yapılandırmadan bir enstrümantasyon anahtarı almak için bir seçenek var mı?


Program.cs ve appsettings.json'u aşağıdaki gibi değiştirin:

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .UseStartup<Startup>()
               .ConfigureLogging((hostingContext, logging) =>
               {
                   // hostingContext.HostingEnvironment can be used to determine environments as well.
                   var appInsightKey = hostingContext.Configuration["myikeyfromconfig"];
                   logging.AddApplicationInsights(appInsightKey);
               });
   }
   ```

   İlgili `appsettings.json`bölüm:

   ```json
   {
     "myikeyfromconfig": "putrealikeyhere"
   }
   ```

Bu kod yalnızca bağımsız bir günlük sağlayıcısı kullandığınızda gereklidir. Düzenli Application Insights izleme için, enstrümantasyon anahtarı yapılandırma yolundan otomatik olarak yüklenir *ApplicationInsights: Instrumentationkey*. Appsettings.json şu na benzemelidir:

   ```json
   {
     "ApplicationInsights":
       {
           "Instrumentationkey":"putrealikeyhere"
       }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

* [ASP.NET Core'da Oturum Açma](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [.NET, Uygulama Öngörüleri'ndeki izleme günlükleri](../../azure-monitor/app/asp-net-trace-logs.md)
