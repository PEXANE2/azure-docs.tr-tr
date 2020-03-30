---
title: İşçi Hizmeti uygulamaları için Uygulama Öngörüleri (HTTP dışı uygulamalar)
description: Azure Monitör Uygulama Öngörüleri ile .NET Core/.NET Framework olmayan uygulamaları izleme.
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: 34a64ffa67b1a43a77391e0d50ddf1bfad0f73ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501164"
---
# <a name="application-insights-for-worker-service-applications-non-http-applications"></a>İşçi Hizmeti uygulamaları için Uygulama Öngörüleri (HTTP dışı uygulamalar)

Application Insights, mesajlaşma, arka `Microsoft.ApplicationInsights.WorkerService`plan görevleri, konsol uygulamaları gibi HTTP dışı iş yükleri için en uygun olan yeni bir SDK'yı piyasaya salıyor. Bu tür uygulamalar geleneksel bir ASP.NET/ASP.NET Core Web Uygulaması gibi gelen bir HTTP isteği kavramına sahip değildir ve bu nedenle [ASP.NET](asp-net.md) veya [ASP.NET Çekirdek](asp-net-core.md) uygulamaları için Uygulama Öngörüleri paketleri nin kullanılması desteklenmez.

Yeni SDK tek başına herhangi bir telemetri koleksiyonu yapmaz. Bunun yerine, [DependencyCollector,](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) [PerfCounterCollector,](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) [ApplicationInsightsLoggingProvider](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) vb gibi diğer iyi bilinen Uygulama Insights otomatik toplayıcıları getiriyor Bu SDK, telemetri koleksiyonunu `IServiceCollection` etkinleştirmek ve yapılandırmak için uzantı yöntemlerini ortaya çıkarır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

[İşçi Hizmeti için Uygulama Öngörüleri SDK,](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) nerede ve nasıl çalıştıklarından bakılmaksızın, HTTP dışı uygulamalar için en uygun uygulamadır. Uygulamanız çalışıyorsa ve Azure'a ağ bağlantısı varsa, telemetri toplanabilir. Uygulama Öngörüleri izleme her yerde desteklenmektedir .NET Core desteklenmektedir. Bu paket yeni tanıtılan [.NET Core 3.0 İşçi Hizmeti,](https://devblogs.microsoft.com/aspnet/dotnet-core-workers-in-azure-container-instances) [Asp.Net Core 2.1/2.2](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-2.2)arka plan görevleri , Konsol uygulamaları (.NET Core/ .NET Framework), vb kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Geçerli bir Application Insights enstrümantasyon anahtarı. Bu anahtar, Uygulama Öngörüleri'ne herhangi bir telemetri göndermek için gereklidir. Enstrümantasyon anahtarı almak için yeni bir Application Insights [kaynağı](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource)oluşturmanız gerekiyorsa, bkz.

## <a name="using-application-insights-sdk-for-worker-services"></a>İşçi Hizmetleri için Uygulama Öngörüleri SDK'nın Kullanılması

1. [Microsoft.ApplicationInsights.WorkerService](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) paketini uygulamaya yükleyin.
   Aşağıdaki snippet, projenizin `.csproj` dosyasına eklenmesi gereken değişiklikleri gösterir.

```xml
    <ItemGroup>
        <PackageReference Include="Microsoft.ApplicationInsights.WorkerService" Version="2.13.1" />
    </ItemGroup>
```

1. Arama `AddApplicationInsightsTelemetryWorkerService(string instrumentationKey)` uzatma `IServiceCollection`yöntemi üzerinde , enstrümantasyon anahtarı sağlayan. Bu yöntem, uygulamanın başında çağrılmalıdır. Tam konumu uygulama türüne bağlıdır.

1. Bağımlılık `ILogger` Enjeksiyonu `TelemetryClient` (DI) kabından Constructor Injection'u arayarak `serviceProvider.GetRequiredService<TelemetryClient>();` veya kullanarak bir örnek veya örnek alın. Bu adım, otomatik `TelemetryConfiguration` toplama modüllerinin ayarlanmasını tetikler.

Her uygulama türü için özel yönergeler aşağıdaki bölümlerde açıklanmıştır.

## <a name="net-core-30-worker-service-application"></a>.NET Core 3.0 işçi hizmet uygulaması

Tam örnek [burada](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/WorkerServiceSampleWithApplicationInsights) paylaşılır

1. [.NET Core 3.0'ı](https://dotnet.microsoft.com/download/dotnet-core/3.0) indirin ve kurun
2. Visual Studio yeni proje şablonunu veya komut satırLarını kullanarak yeni bir İşçi Hizmeti projesi oluşturun`dotnet new worker`
3. [Microsoft.ApplicationInsights.WorkerService](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) paketini uygulamaya yükleyin.

4. Sınıfınızdaki `services.AddApplicationInsightsTelemetryWorkerService();` `Program.cs` `CreateHostBuilder()` yönteme, aşağıdaki örnekte olduğu gibi ekleyin:

```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
                services.AddHostedService<Worker>();
                services.AddApplicationInsightsTelemetryWorkerService();
            });
```

5. Aşağıdaki `Worker.cs` örnekte değişiklik yapın.

```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;

    public class Worker : BackgroundService
    {
        private readonly ILogger<Worker> _logger;
        private TelemetryClient _telemetryClient;
        private static HttpClient _httpClient = new HttpClient();

        public Worker(ILogger<Worker> logger, TelemetryClient tc)
        {
            _logger = logger;
            _telemetryClient = tc;
        }

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            while (!stoppingToken.IsCancellationRequested)
            {
                _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);

                using (_telemetryClient.StartOperation<RequestTelemetry>("operation"))
                {
                    _logger.LogWarning("A sample warning message. By default, logs with severity Warning or higher is captured by Application Insights");
                    _logger.LogInformation("Calling bing.com");
                    var res = await _httpClient.GetAsync("https://bing.com");
                    _logger.LogInformation("Calling bing completed with status:" + res.StatusCode);
                    _telemetryClient.TrackEvent("Bing call event completed");
                }

                await Task.Delay(1000, stoppingToken);
            }
        }
    }
```

6. Enstrümantasyon anahtarını ayarlayın.

    Enstrümantasyon anahtarını bağımsız değişken olarak `AddApplicationInsightsTelemetryWorkerService`sağlayabilseniz de, enstrümantasyon anahtarını yapılandırmada belirtmenizi öneririz. Aşağıdaki kod örneği, ''de `appsettings.json`bir enstrümantasyon anahtarının nasıl belirtilen Yayımlama `appsettings.json` sırasında uygulama kökü klasörüne kopyalandığından emin olun.

```json
    {
        "ApplicationInsights":
            {
            "InstrumentationKey": "putinstrumentationkeyhere"
            },
        "Logging":
        {
            "LogLevel":
            {
                "Default": "Warning"
            }
        }
    }
```

Alternatif olarak, aşağıdaki ortam değişkenlerinden birinde enstrümantasyon anahtarını belirtin.
`APPINSIGHTS_INSTRUMENTATIONKEY` veya `ApplicationInsights:InstrumentationKey`

Örneğin, `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`
Veya`SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

Genellikle, `APPINSIGHTS_INSTRUMENTATIONKEY` Web Apps'a Web İşleri olarak dağıtılan uygulamalar için enstrümantasyon anahtarını belirtir.

> [!NOTE]
> Kodda belirtilen bir enstrümantasyon anahtarı, diğer seçeneklerden daha fazla kazanan ortam değişkenini `APPINSIGHTS_INSTRUMENTATIONKEY`kazanır.

## <a name="aspnet-core-background-tasks-with-hosted-services"></a>barındırılan hizmetlerle ASP.NET Temel arka plan görevleri

[Bu belge,](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-2.2&tabs=visual-studio) ASP.NET Core 2.1/2.2 uygulamasında arka plan görevlerinin nasıl oluşturulacağını açıklar.

Tam örnek [burada](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/BackgroundTasksWithHostedService) paylaşılır

1. Microsoft.ApplicationInsights.WorkerService paketinihttps://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) uygulamaya yükleyin.
2. `services.AddApplicationInsightsTelemetryWorkerService();` Yönteme, `ConfigureServices()` bu örnekte olduğu gibi ekleyin:

```csharp
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .ConfigureAppConfiguration((hostContext, config) =>
            {
                config.AddJsonFile("appsettings.json", optional: true);
            })
            .ConfigureServices((hostContext, services) =>
            {
                services.AddLogging();
                services.AddHostedService<TimedHostedService>();

                // instrumentation key is read automatically from appsettings.json
                services.AddApplicationInsightsTelemetryWorkerService();
            })
            .UseConsoleLifetime()
            .Build();

        using (host)
        {
            // Start the host
            await host.StartAsync();

            // Wait for the host to shutdown
            await host.WaitForShutdownAsync();
        }
    }
```

Arka plan görev `TimedHostedService` mantığının bulunduğu kod aşağıda veda eder.

```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;

    public class TimedHostedService : IHostedService, IDisposable
    {
        private readonly ILogger _logger;
        private Timer _timer;
        private TelemetryClient _telemetryClient;
        private static HttpClient httpClient = new HttpClient();

        public TimedHostedService(ILogger<TimedHostedService> logger, TelemetryClient tc)
        {
            _logger = logger;
            this._telemetryClient = tc;
        }

        public Task StartAsync(CancellationToken cancellationToken)
        {
            _logger.LogInformation("Timed Background Service is starting.");

            _timer = new Timer(DoWork, null, TimeSpan.Zero,
                TimeSpan.FromSeconds(1));

            return Task.CompletedTask;
        }

        private void DoWork(object state)
        {
            _logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);

            using (_telemetryClient.StartOperation<RequestTelemetry>("operation"))
            {
                _logger.LogWarning("A sample warning message. By default, logs with severity Warning or higher is captured by Application Insights");
                _logger.LogInformation("Calling bing.com");
                var res = await httpClient.GetAsync("https://bing.com");
                _logger.LogInformation("Calling bing completed with status:" + res.StatusCode);
                telemetryClient.TrackEvent("Bing call event completed");
            }
        }
    }
```

3. Enstrümantasyon anahtarını ayarlayın.
   Yukarıdaki .NET Core 3.0 İşçi Hizmeti örneğinden aynı sıyrıkları `appsettings.json` kullanın.

## <a name="net-corenet-framework-console-application"></a>.NET Core/.NET Framework Console uygulaması

Bu makalenin başında belirtildiği gibi, yeni paket bile normal bir konsol uygulamasından Uygulama Insights Telemetri etkinleştirmek için kullanılabilir. Bu paket [`NetStandard2.0`](https://docs.microsoft.com/dotnet/standard/net-standard),.NET Core 2.0 veya üstü ve .NET Framework 4.7.2 veya daha yüksek konsol uygulamaları için kullanılabilir.

Tam örnek [burada](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/ConsoleAppWithApplicationInsights) paylaşılır

1. Microsoft.ApplicationInsights.WorkerService paketinihttps://www.nuget.org/packages/Microsoft.ApplicationInsights.WorkerService) uygulamaya yükleyin.

2. Aşağıdaki örnekte Program.cs değiştirin.

```csharp
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.Logging;
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;

    namespace WorkerSDKOnConsole
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                // Create the DI container.
                IServiceCollection services = new ServiceCollection();

                // Being a regular console app, there is no appsettings.json or configuration providers enabled by default.
                // Hence instrumentation key and any changes to default logging level must be specified here.
                services.AddLogging(loggingBuilder => loggingBuilder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>("Category", LogLevel.Information));
                services.AddApplicationInsightsTelemetryWorkerService("instrumentationkeyhere");

                // Build ServiceProvider.
                IServiceProvider serviceProvider = services.BuildServiceProvider();

                // Obtain logger instance from DI.
                ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

                // Obtain TelemetryClient instance from DI, for additional manual tracking or to flush.
                var telemetryClient = serviceProvider.GetRequiredService<TelemetryClient>();

                var httpClient = new HttpClient();

                while (true) // This app runs indefinitely. replace with actual application termination logic.
                {
                    logger.LogInformation("Worker running at: {time}", DateTimeOffset.Now);

                    // Replace with a name which makes sense for this operation.
                    using (telemetryClient.StartOperation<RequestTelemetry>("operation"))
                    {
                        logger.LogWarning("A sample warning message. By default, logs with severity Warning or higher is captured by Application Insights");
                        logger.LogInformation("Calling bing.com");                    
                        var res = await httpClient.GetAsync("https://bing.com");
                        logger.LogInformation("Calling bing completed with status:" + res.StatusCode);
                        telemetryClient.TrackEvent("Bing call event completed");
                    }

                    await Task.Delay(1000);
                }

                // Explicitly call Flush() followed by sleep is required in Console Apps.
                // This is to ensure that even if application terminates, telemetry is sent to the back-end.
                telemetryClient.Flush();
                Task.Delay(5000).Wait();
            }
        }
    }
```

Bu konsol uygulaması da `TelemetryConfiguration`aynı varsayılan kullanır ve önceki bölümde örnekler olarak aynı şekilde özelleştirilebilir.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

Başvurunuzu çalıştırın. Yukarıdaki tüm örnek işçiler bing.com için her saniye bir http arama yapar ve aynı zamanda ILogger kullanarak birkaç günlükleri yakar. Bu `StartOperation` `TelemetryClient`satırlar, bir işlem oluşturmak için kullanılan (bu örnekte `RequestTelemetry` "işlem" olarak adlandırılır) çağrı içinde sarılır. Uygulama Öngörüleri bu ILogger günlüklerini (uyarı veya varsayılan olarak yukarıda) ve bağımlılıkları `RequestTelemetry` toplar ve üst-alt ilişkisiyle ilişkilendirilir. Korelasyon aynı zamanda çapraz işlem/ağ sınırı da çalışır. Örneğin, arama başka bir izlenen bileşene yapıldıysa, bu üst öğeyle de ilişkilendirilir.

Bu özel `RequestTelemetry` işlem, tipik bir Web Uygulamasında gelen web isteğine eşdeğer olarak düşünülebilir. Bir İşlem kullanmak gerekli olmasa da, en iyi [Uygulama Öngörüleri korelasyon veri modeline](https://docs.microsoft.com/azure/azure-monitor/app/correlation) uyar - ana işlem olarak hareket eden `RequestTelemetry` ve işçi yinelemesi içinde oluşturulan her telemetri mantıksal olarak aynı işleme ait olarak kabul edilir. Bu yaklaşım aynı zamanda oluşturulan tüm telemetri (otomatik ve `operation_id`manuel) aynı olmasını sağlar. Örnekleme dayandığı `operation_id`gibi, örnekleme algoritması tek bir yinelemeden tüm telemetriyi tutar veya düşürür.

Aşağıda, Application Insights tarafından otomatik olarak toplanan tam telemetri listelenir.

### <a name="live-metrics"></a>Canlı Ölçümler

[Canlı Ölçümler,](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) Uygulama Öngörüleri izlemenin doğru yapılandırıp yapılandırılmadı sını hızlı bir şekilde doğrulamak için kullanılabilir. Telemetrinin portalda ve analitikte görünmeye başlaması birkaç dakika sürebilir, ancak Canlı Ölçümler, çalışan işlemin CPU kullanımını neredeyse gerçek zamanlı olarak gösterir. Ayrıca İstekler, Bağımlılıklar, İzler vb. gibi diğer telemetrileri de gösterebilir.

### <a name="ilogger-logs"></a>ILogger günlükleri

Önem derecesi `Warning` veya `ILogger` daha büyük bir şekilde yayılan günlükler otomatik olarak yakalanır. Uygulama Öngörüleri tarafından hangi günlük düzeylerinin yakalandığını özelleştirmek için [ILogger dokümanlarını](ilogger.md#control-logging-level) izleyin.

### <a name="dependencies"></a>Bağımlılıklar

Bağımlılık toplama varsayılan olarak etkinleştirilir. [Bu](asp-net-dependencies.md#automatically-tracked-dependencies) makalede, otomatik olarak toplanan bağımlılıkları açıklar ve el ile izleme yapmak için adımlar da içerir.

### <a name="eventcounter"></a>EventCounter

`EventCounterCollectionModule`varsayılan olarak etkinleştirilir ve .NET Core 3.0 uygulamalarından varsayılan bir sayaç kümesi toplar. [EventCounter](eventcounters.md) öğreticisi toplanan varsayılan sayaç kümesini listeler. Ayrıca liste özelleştirme talimatları vardır.

### <a name="manually-tracking-additional-telemetry"></a>Ek telemetriyi el ile izleme

SDK yukarıda açıklandığı gibi otomatik olarak telemetri toplarken, çoğu durumda kullanıcının Application Insights hizmetine ek telemetri göndermesi gerekir. Ek telemetriyi izlemenin önerilen yolu, Bağımlılık `TelemetryClient` Enjeksiyonu'ndan bir örnek almak ve `TrackXXX()` ardından desteklenen [API](api-custom-events-metrics.md) yöntemlerinden birini çağırmaktır. Başka bir tipik kullanım örneği [operasyonların özel izleme](custom-operations-tracking.md)olduğunu. Bu yaklaşım yukarıdaki İşçi örneklerinde gösterilmiştir.

## <a name="configure-the-application-insights-sdk"></a>Uygulama Öngörülerini Yapılandırma SDK

İşçi `TelemetryConfiguration` hizmeti SDK tarafından kullanılan varsayılan bir ASP.NET veya ASP.NET Core uygulamasında kullanılan otomatik yapılandırma benzer, eksi TelemetryInitializers telemetri zenginleştirmek için kullanılır `HttpContext`.

Varsayılan yapılandırmayı değiştirmek için İşçi Hizmeti için Uygulama Öngörüleri SDK'sını özelleştirebilirsiniz. Core SDK'ASP.NET Uygulama Öngörüleri kullanıcıları, ASP.NET Core yerleşik bağımlılık [enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)kullanarak yapılandırmayı değiştirmeye aşina olabilirler. İşçi Hizmeti SDK de benzer ilkelere dayanmaktadır. Aşağıda açıklandığı gibi, `ConfigureServices()` uygun yöntemleri `IServiceCollection`arayarak bölümdeki hemen hemen tüm yapılandırma değişikliklerini yapın.

> [!NOTE]
> Bu SDK'yı kullanırken, `TelemetryConfiguration.Active` değiştirerek yapılandırmayı değiştirmek desteklenmez ve değişiklikler yansıtılmayacaktır.

### <a name="using-applicationinsightsserviceoptions"></a>ApplicationInsightsServiceOptions kullanma

Bu örnekte olduğu `ApplicationInsightsServiceOptions` `AddApplicationInsightsTelemetryWorkerService`gibi, birkaç ortak ayarı , geçerek değiştirebilirsiniz:

```csharp
    using Microsoft.ApplicationInsights.WorkerService;

    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.WorkerService.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.WorkerService.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetryWorkerService(aiOptions);
    }
```

Bu `ApplicationInsightsServiceOptions` SDK'nın ASP.NET Core `Microsoft.ApplicationInsights.WorkerService` SDK'nın aksine `Microsoft.ApplicationInsights.AspNetCore.Extensions` isim alanında olduğunu unutmayın.

Sık kullanılan ayarlar`ApplicationInsightsServiceOptions`

|Ayar | Açıklama | Varsayılan
|---------------|-------|-------
|EnableQuickPulseMetricStream | LiveMetrics özelliğini etkinleştirme/devre dışı | true
|AdaptiveSampling'ı Etkinleştir | Uyarlamalı Örneklemeyi Etkinleştirme/Devre Dışı | true
|Sinyal Atışını Etkinleştirme | Düzenli aralıklarla (15 dakika varsayılan) .NET Sürümü, Azure Ortamı bilgileri gibi çalışma zamanı hakkında bilgi içeren 'HeartBeatState' adlı özel bir metrik gönderen Kalp Atışlarını Etkinleştir/Devre Dışı Kaltın özelliği, örneğin. | true
|AddAutoCollectedMetricExtractor | Örnekleme gerçekleşmeden önce İstekler/Bağımlılıklar hakkında önceden toplanmış ölçümler gönderen bir Telemetri İşlemcisi olan Otomatik Toplanan Metrikleri etkinleştirme/devre dışı etme. | true

En güncel liste için [yapılandırılabilir `ApplicationInsightsServiceOptions` ayarlara](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/NETCORE/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs) bakın.

### <a name="sampling"></a>Örnekleme

İşçi Hizmeti için Uygulama Öngörüleri SDK hem sabit oranlı hem de uyarlanabilir örneklemeyi destekler. Uyarlanabilir örnekleme varsayılan olarak etkinleştirilir. İşçi Hizmeti için örnekleme [yapılandırma, ASP.NET Temel Uygulamalar](https://docs.microsoft.com/azure/azure-monitor/app/sampling#configuring-adaptive-sampling-for-aspnet-core-applications)da aynı şekilde yapılır.

### <a name="adding-telemetryinitializers"></a>Telemetri EklemeInitializers

Tüm telemetri ile gönderilen özellikleri tanımlamak istediğinizde [telemetri başharflerini](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#addmodify-properties-itelemetryinitializer) kullanın.

Kapsayıcıya `TelemetryInitializer` herhangi bir yeni ekleyin ve SDK otomatik olarak bunları ekleyecektir . `TelemetryConfiguration` `DependencyInjection`

```csharp
    using Microsoft.ApplicationInsights.Extensibility;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
        services.AddApplicationInsightsTelemetryWorkerService();
    }
```

### <a name="removing-telemetryinitializers"></a>TelemetriBaşlangıçlarını Kaldırma

Telemetri başharfleri varsayılan olarak mevcuttur. Tüm veya belirli telemetri başharflerini kaldırmak için, aşağıdaki `AddApplicationInsightsTelemetryWorkerService()`örnek *kodu* çağırın.

```csharp
   public void ConfigureServices(IServiceCollection services)
   {
        services.AddApplicationInsightsTelemetryWorkerService();
        // Remove a specific built-in telemetry initializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                            (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
   }
```

### <a name="adding-telemetry-processors"></a>Telemetri işlemcileri ekleme

'deki `IServiceCollection`uzantı yöntemini `AddApplicationInsightsTelemetryProcessor` kullanarak `TelemetryConfiguration` özel telemetri işlemcileri ekleyebilirsiniz. Uygulama Öngörüleri hizmetine gönderdiğiniz telemetrinin dahil edilmesi veya hariç olması üzerinde daha doğrudan denetim sağlamak için [gelişmiş filtreleme senaryolarında](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#itelemetryprocessor-and-itelemetryinitializer) telemetri işlemcileri kullanırsınız. Aşağıdaki örneği kullanın.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetryWorkerService();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();
        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Varsayılan Telemetri Modüllerini yapılandırma veya kaldırma

Application Insights, manuel izleme gerektirmeden belirli iş yükleri hakkında telemetriyi otomatik olarak toplamak için telemetri modüllerini kullanır.

Aşağıdaki otomatik toplama modülleri varsayılan olarak etkinleştirilir. Bu modüller otomatik olarak telemetri toplamakiçin sorumludur. Varsayılan davranışlarını değiştirmek için devre dışı bırakabilirsiniz veya bunları yapılandırabilirsiniz.

* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Herhangi bir varsayılan `TelemetryModule`yapılandırmak için, `IServiceCollection`aşağıdaki örnekte gösterildiği gibi, uzantı yöntemini `ConfigureTelemetryModule<T>` kullanın.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
    using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetryWorkerService();

            // The following configures QuickPulseTelemetryModule.
            // Similarly, any other default modules can be configured.
            services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) =>
            {
                module.AuthenticationApiKey = "keyhere";
            });

            // The following removes PerformanceCollectorModule to disable perf-counter collection.
            // Similarly, any other default modules can be removed.
            var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>
                                        (t => t.ImplementationType == typeof(PerformanceCollectorModule));
            if (performanceCounterService != null)
            {
                services.Remove(performanceCounterService);
            }
    }
```

### <a name="configuring-telemetry-channel"></a>Telemetri kanalının yapılandırılması

Varsayılan `ServerTelemetryChannel`kanal. Aşağıdaki örnekte görüldüğü gibi geçersiz kılabilirsiniz.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetryWorkerService();
    }
```

### <a name="disable-telemetry-dynamically"></a>Telemetriyi dinamik olarak devre dışı

Telemetriyi koşullu ve dinamik olarak devre dışı kılmış olmak istiyorsanız, kodunuzun herhangi bir yerinde ASP.NET Çekirdek bağımlılık enjeksiyon konteyneri ile örneği çözebilir `TelemetryConfiguration` ve üzerine bayrak ayarlayabilirsiniz. `DisableTelemetry`

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetryWorkerService();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Otomatik olarak toplanmayan telemetriyi nasıl izleyebilirim?

Yapıcı enjeksiyon `TelemetryClient` kullanarak bir örnek alın ve `TrackXXX()` üzerinde gerekli yöntemi arayın. Yeni `TelemetryClient` örnekler oluşturmanızı önermiyoruz. Tek tonlu `TelemetryClient` bir örnek zaten `DependencyInjection` telemetri `TelemetryConfiguration` geri kalanı ile paylaşan konteyner, kayıtlıdır. Yeni `TelemetryClient` bir örnek oluşturma, yalnızca telemetrinin geri kalanından ayrı bir yapılandırmaya ihtiyaç duyduğunda önerilir.

### <a name="can-i-use-visual-studio-ide-to-onboard-application-insights-to-a-worker-service-project"></a>Visual Studio IDE'yi Bir İşçi Hizmeti projesine dahil etmek için Kullanabilir miyim?

Visual Studio IDE onboarding şu anda sadece ASP.NET/ASP.NET Çekirdek Uygulamaları için desteklenir. Bu belge, Visual Studio, onboarding Worker hizmet uygulamaları için destek verdiğinde güncellenecektir.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Durum Monitörü gibi araçları kullanarak Uygulama Öngörüleri izlemeyi etkinleştirebilir miyim?

Hayır. [Durum İzle](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) mi ve [Durum İzleyicisi v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) şu anda yalnızca ASP.NET 4.x'i destekliyor.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Uygulamamı Linux'ta çalıştırıyorumsa, tüm özellikler desteklenir mi?

Evet. Bu SDK için özellik desteği aşağıdaki istisnalar dışında tüm platformlarda aynıdır:

* Performans sayaçları, Canlı Ölçümler'de gösterilen İşlem CPU/Bellek dışında yalnızca Windows'da desteklenir.
* Varsayılan `ServerTelemetryChannel` olarak etkin olsa da, uygulama Linux veya MacOS'ta çalışıyorsa, kanal ağ sorunları olduğunda telemetriyi geçici olarak tutmak için otomatik olarak yerel bir depolama klasörü oluşturmaz. Bu sınırlama nedeniyle, geçici ağ veya sunucu sorunları olduğunda telemetri kaybolur. Bu sorunu çözmek için kanal için yerel bir klasör yapılandırır:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetryWorkerService();
    }
```

## <a name="sample-applications"></a>Örnek uygulamalar

[.NET Core Konsol Uygulaması](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/ConsoleAppWithApplicationInsights) .NET Core (2.0 veya üzeri) veya .NET Framework (4.7.2 veya üzeri) ile yazılmış bir Konsol Uygulaması kullanıyorsanız bu örneği kullanın

[HostedServices ile ASP .NET Çekirdek arka plan görevleri](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/BackgroundTasksWithHostedService) Core 2.1/2.2 Asp.Net ve burada resmi [kılavuza](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-2.2) göre arka plan görevleri oluşturuyorsanız bu örneği kullanın

[.NET Çekirdek 3.0 İşçi Hizmeti](https://github.com/microsoft/ApplicationInsights-Home/tree/master/Samples/WorkerServiceSDK/WorkerServiceSampleWithApplicationInsights) [Burada](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services?view=aspnetcore-3.0&tabs=visual-studio#worker-service-template) resmi rehberlik uyarınca bir .NET Core 3.0 İşçi Hizmeti uygulamanız varsa bu örneği kullanın

## <a name="open-source-sdk"></a>Açık kaynak SDK

[Kodu okuyun ve katkıda bulunun.](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanızın performansı nın ve kullanımının ayrıntılı bir görünümü için kendi etkinliklerinizi ve ölçümlerinizi göndermek için [API'yi kullanın.](../../azure-monitor/app/api-custom-events-metrics.md)
* [Otomatik olarak izlenmeyen ek bağımlılıkları izleyin.](../../azure-monitor/app/auto-collect-dependencies.md)
* [Zenginleştirin veya Filtre otomatik toplanan telemetri](../../azure-monitor/app/api-filtering-sampling.md).
* [ASP.NET Çekirdek bağımlılık enjeksiyon](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).
