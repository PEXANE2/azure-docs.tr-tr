---
title: ASP.NET Core uygulamalar için Azure Application Insights | Microsoft Docs
description: Kullanılabilirlik, performans ve kullanım için ASP.NET Core Web uygulamalarını izleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mbullwin
ms.openlocfilehash: 9e14a9f3f2f27112a591f14e9a93580f66aadef7
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169556"
---
# <a name="application-insights-for-aspnet-core-applications"></a>ASP.NET Core uygulamalar için Application Insights

Bu makalede, [ASP.NET Core](https://docs.microsoft.com/aspnet/core) bir uygulama için Application Insights nasıl etkinleştirileceği açıklanır. Bu makaledeki yönergeleri tamamladığınızda Application Insights, ASP.NET Core uygulamanızdan istekleri, bağımlılıkları, özel durumları, performans sayaçlarını, sinyalleri ve günlükleri toplar.

Burada kullanacağınız örnek, ' i hedefleyen `netcoreapp2.2`bir [MVC uygulamasıdır](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) . Bu yönergeleri tüm ASP.NET Core uygulamalarına uygulayabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

[ASP.NET Core için APPLICATION INSIGHTS SDK](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) , uygulamalarınızı nerede veya nasıl çalıştırdıklarından bağımsız olarak izleyebilir. Uygulamanız çalışıyorsa ve Azure ile ağ bağlantısı varsa telemetri toplanabilir. Application Insights izleme .NET Core 'un desteklendiği her yerde desteklenir. Destek şunları içerir:
* **İşletim sistemi**: Windows, Linux veya Mac.
* **Barındırma yöntemi**: İşlem veya işlem dışı.
* **Dağıtım yöntemi**: Çerçeveye bağımlı veya kendine dahil.
* **Web sunucusu**: IIS (Internet Information Server) veya Kestrel.
* **Barındırma platformu**: Azure App Service, Azure VM, Docker, Azure Kubernetes hizmeti (AKS) ve benzeri Web Apps özelliği.
* **IDE**: Visual Studio, VS Code veya komut satırı.

> [!NOTE]
> Application Insights ile birlikte ASP.NET Core 3,0-Preview kullanıyorsanız, lütfen [2.8.0-Beta3](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/2.8.0-beta3) sürümünü veya üstünü kullanın. Bu, ASP.NET Core 3,0 ile iyi çalışmak için bilinen tek sürümdür. Ayrıca, Visual Studio tabanlı ekleme ASP.NET Core 3,0 uygulamaları için henüz desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar

- Çalışan bir ASP.NET Core uygulaması. Bir ASP.NET Core uygulaması oluşturmanız gerekiyorsa, bu [ASP.NET Core öğreticisini](https://docs.microsoft.com/aspnet/core/getting-started/)izleyin.
- Geçerli bir Application Insights izleme anahtarı. Bu anahtar, Application Insights telemetri göndermek için gereklidir. Bir izleme anahtarı almak için yeni bir Application Insights kaynağı oluşturmanız gerekiyorsa, bkz. [Application Insights kaynağı oluşturma](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Sunucu tarafı telemetrisini Application Insights etkinleştirme (Visual Studio)

1. Projenizi Visual Studio 'da açın.

    > [!TIP]
    > İsterseniz, Application Insights yapacağı tüm değişiklikleri izleyebilmek için projeniz için kaynak denetimi ayarlayabilirsiniz. Kaynak denetimini etkinleştirmek için **Dosya** > **kaynak denetimine Ekle**' yi seçin.

2. **Proje** > **Ekle Application Insights telemetri**seçin.

3. **Kullanmaya**başlayın ' ı seçin. Bu seçimin metni, Visual Studio sürümünüze bağlı olarak farklılık gösterebilir. Bazı önceki sürümler bunun yerine **ücretsiz Başlat** düğmesini kullanır.

4. Aboneliğinizi seçin. Sonra **kaynak** > **kaydı**' nı seçin.

5. Projenize Application Insights ekledikten sonra, SDK 'nın en son kararlı sürümünü kullandığınızı onaylamak için denetleyin.  > Project > 'e git**Microsoft. ApplicationInsights. aspnetcore** **NuGet Paketlerini Yönet**. Gerekirse **Güncelleştir**' i seçin.

     ![Güncelleştirme için Application Insights paketinin nerede seçileceğini gösteren ekran görüntüsü](./media/asp-net-core/update-nuget-package.png)

6. İsteğe bağlı ipucunu izlediyseniz ve projenizi kaynak denetimine eklediyseniz,**Takım Gezgini** > **değişiklikleri** **görüntüle** > ' ye gidin. Sonra Application Insights telemetri tarafından yapılan değişikliklerin fark görünümünü görmek için her bir dosyayı seçin.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Sunucu tarafı telemetrisini Application Insights etkinleştirme (Visual Studio yok)

1. [ASP.NET Core için APPLICATION INSIGHTS SDK NuGet paketini](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore)yükler. Her zaman en son kararlı sürümü kullanmanızı öneririz. [Açık kaynaklı GitHub](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases)deposunda SDK için tam sürüm notları bulun.

    Aşağıdaki kod örneğinde, projenizin `.csproj` dosyasına eklenecek değişiklikler gösterilmektedir.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.7.0" />
        </ItemGroup>
    ```

2. Aşağıdaki örnekte olduğu gibi `services.AddApplicationInsightsTelemetry();` sınıfınıza `Startup` yöntemine ekleyin: `ConfigureServices()`

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. İzleme anahtarını ayarlayın.

    İzleme anahtarını için `AddApplicationInsightsTelemetry`bir bağımsız değişken olarak sağlayabilseniz de, yapılandırmada izleme anahtarını belirtmenizi öneririz. Aşağıdaki kod örneği, içinde `appsettings.json`bir izleme anahtarının nasıl ekleneceğini gösterir. Yayımlama sırasında `appsettings.json` uygulama kök klasörüne kopyalandığından emin olun.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Alternatif olarak, aşağıdaki ortam değişkenlerinden birinde izleme anahtarını belirtin:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Örneğin:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Genellikle, `APPINSIGHTS_INSTRUMENTATIONKEY` Web Apps dağıtılan uygulamalar için izleme anahtarını belirtir.

    > [!NOTE]
    > Diğer seçenekler üzerinde WINS olan ortam değişkeni `APPINSIGHTS_INSTRUMENTATIONKEY`üzerinden WINS kodunda belirtilen bir izleme anahtarı.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

Uygulamanızı çalıştırın ve ona istek yapın. Telemetri artık Application Insights akışı sağlamalıdır. Application Insights SDK aşağıdaki Telemetriyi otomatik olarak toplar.

|İstekler/bağımlılıklar |Ayrıntılar|
|---------------|-------|
|İstekler | Uygulamanıza gelen Web istekleri. |
|HTTP veya HTTPS | İle `HttpClient`yapılan çağrılar. |
|SQL | İle `SqlClient`yapılan çağrılar. |
|[Azure Depolama](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure Storage istemcisiyle yapılan çağrılar. |
|[EventHubs istemci SDK 'Sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Sürüm 1.1.0 ve üzeri. |
|[ServiceBus istemci SDK 'Sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve üzeri. |
|Azure Cosmos DB | Yalnızca HTTP/HTTPS kullanılıyorsa otomatik olarak izlenir. Application Insights TCP modunu yakalamaz. |

### <a name="performance-counters"></a>Performans sayaçları

ASP.NET Core [performans sayaçları](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) için destek sınırlıdır:

* SDK sürümleri 2.4.1 ve üzeri, uygulama Azure Web Apps (Windows) üzerinde çalışıyorsa performans sayaçlarını toplar.
* SDK sürümleri 2.7.1 ve üzeri, uygulama Windows ve hedefler `NETSTANDARD2.0` veya sonraki sürümlerde çalışıyorsa performans sayaçlarını toplar.
* .NET Framework hedefleyen uygulamalar için SDK 'nın tüm sürümleri performans sayaçlarını destekler.
* 2\.8.0-Beta3 ve üzeri SDK sürümleri, Linux 'ta CPU/bellek sayacını destekler. Linux 'ta başka bir sayaç desteklenmez. Linux 'ta (ve diğer Windows dışı ortamlarda) sistem sayaçlarını almanın önerilen yolu [Eventcounters](#eventcounter) kullanmaktır

### <a name="eventcounter"></a>EventCounter

[EventCounter](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md), .NET/.NET Core 'da sayaçları yayımlamak ve kullanmak için platformlar arası bir yöntemdir. Bu özellik daha önce var olsa da, bu sayaçları yayımlayan yerleşik sağlayıcı yoktu. .NET Core 3,0 ile başlayarak, birkaç sayaç CLR sayaçları, ASP.NET Core sayaçları vb. gibi kutudan dışarı yayımlanır.

SDK sürümleri 2.8.0-Beta3 ve üzeri, EventCounters koleksiyonunu destekler. Varsayılan olarak SDK aşağıdaki sayaçları toplar ve bu sayaçlar, Ölçüm Gezgini veya PerformanceCounter tablosundaki analiz sorgusu kullanılarak sorgulanabilir. Sayaçların adı "Kategori | biçiminde olacaktır. Counter ".

|Category | Sayaç|
|---------------|-------|
|System. Runtime | CPU kullanımı |
|System. Runtime | çalışma kümesi |
|System. Runtime | GC-yığın boyutu |
|System. Runtime | Gen-0-GC-Count |
|System. Runtime | Gen-1-GC-sayısı |
|System. Runtime | Gen-2-GC-sayım |
|System. Runtime | GC zamanı |
|System. Runtime | Gen-0-boyut |
|System. Runtime | Gen-1-boyut |
|System. Runtime | Gen-2 boyutu |
|System. Runtime | Loh-boyut |
|System. Runtime | ayırma oranı |
|System. Runtime | derleme sayısı |
|System. Runtime | özel durum-sayı |
|System. Runtime | ThreadPool-thread-Count |
|System. Runtime | Monitor-Lock-çekişme-Count |
|System. Runtime | ThreadPool-kuyruk uzunluğu |
|System. Runtime | ThreadPool-tamamlanan-öğe sayısı |
|System. Runtime | etkin-zamanlayıcı-sayısı |
|Microsoft. AspNetCore. Hosting | saniye başına istek sayısı |
|Microsoft. AspNetCore. Hosting | toplam istek sayısı |
|Microsoft. AspNetCore. Hosting | geçerli istekler |
|Microsoft. AspNetCore. Hosting | başarısız-istekler |

### <a name="ilogger-logs"></a>ILogger günlükleri

[](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) 2.7.0-Beta3 ve üzeri SDK `Warning` sürümlerinde otomatik olarak önem veya daha fazla önem düzeyi günlükleri yakalanır.

### <a name="live-metrics"></a>Canlı Ölçümler

Telemetrinin portalda görünmesi birkaç dakika sürebilir. Her şeyin çalıştığından hızlı bir şekilde emin olmak için, çalışan uygulamaya istek yaptığınızda [canlı ölçümleri](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) kullanmak en iyisidir.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Web uygulamaları için istemci tarafı telemetrisini etkinleştir

Yukarıdaki adımlar, sunucu tarafı Telemetriyi toplamaya başlamanıza yardımcı olacak kadar yeterlidir. Uygulamanızda istemci tarafı bileşenleri varsa, [kullanım telemetrisini](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview)toplamaya başlamak için sonraki adımları izleyin.

1. İçinde `_ViewImports.cshtml`ekleme ekle:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. İçinde `_Layout.cshtml` `HtmlHelper` , bölümünün`<head>` sonuna, ancak diğer herhangi bir betikten önce ekleyin. Sayfadan herhangi bir özel JavaScript telemetrisini raporlamak istiyorsanız, bu kod parçacığına sonra ekleme:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

Daha önce başvurulan dosya adları varsayılan bir MVC uygulama şablonundan alınır. `.cshtml` Sonuç olarak, uygulamanız için istemci tarafı izlemeyi doğru şekilde etkinleştirmek istiyorsanız, uygulamanızın izlemek istediğiniz her sayfasının `<head>` bölümünde JavaScript kod parçacığı görünmelidir. Bu uygulama şablonu için JavaScript kod parçacığını `_Layout.cshtml`ekleyerek bu hedefi gerçekleştirebilirsiniz. 

Projeniz içermiyorsa `_Layout.cshtml`, [istemci tarafı izleme](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring)eklemeye devam edebilirsiniz. Bu, JavaScript kod parçacığını uygulamanızdaki tüm sayfaları denetleyen `<head>` eşdeğer bir dosyaya ekleyerek yapabilirsiniz. Ya da kod parçacığını birden çok sayfaya ekleyebilirsiniz, ancak bu çözümün korunması zordur ve genellikle bunu önermiyoruz.

## <a name="configure-the-application-insights-sdk"></a>Application Insights SDK 'sını yapılandırma

Varsayılan yapılandırmayı değiştirmek için ASP.NET Core Application Insights SDK 'sını özelleştirebilirsiniz. Application Insights ASP.NET SDK kullanıcıları, kullanarak `ApplicationInsights.config` veya değiştirerek `TelemetryConfiguration.Active`yapılandırma değiştirme konusunda bilgi sahibi olabilir. Yapılandırmayı ASP.NET Core için farklı şekilde değiştirirsiniz. ASP.NET Core SDK 'sını uygulamaya ekleyin ve ASP.NET Core yerleşik [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)eklentisini kullanarak yapılandırın. Başka türlü yönlendirilmediğiniz takdirde, `ConfigureServices()` `Startup.cs` sınıfınızın yönteminde neredeyse tüm yapılandırma değişikliklerini yapın. Aşağıdaki bölümler daha fazla bilgi sunar.

> [!NOTE]
> ASP.NET Core uygulamalarda, değiştirme `TelemetryConfiguration.Active` ile yapılandırmayı değiştirme desteklenmez.

### <a name="using-applicationinsightsserviceoptions"></a>Applicationınsightsserviceoptions kullanma

Aşağıdaki örnekte olduğu gibi `ApplicationInsightsServiceOptions` `AddApplicationInsightsTelemetry`, ' a geçirerek birkaç ortak ayarı değiştirebilirsiniz:

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

İçindeki ayarların tam listesi`ApplicationInsightsServiceOptions`

|Ayar | Açıklama | Varsayılan
|---------------|-------|-------
|EnableQuickPulseMetricStream | Liveölçümlerini etkinleştir/devre dışı bırak özelliği | true
|Enableadaptiveörnekleme | Uyarlamalı örneklemeyi etkinleştir/devre dışı bırak | true
|Enablesinyal | Düzenli aralıklarla (15 dakikalık varsayılan), .NET sürümü, Azure ortam bilgileri, varsa vb. gibi çalışma zamanı hakkında bilgi içeren "HeartBeatState" adlı özel bir ölçüm göndererek, sinyalleri etkinleştir/devre dışı bırak özelliği. | true
|Addadutocollectedmetricextractor | Örnekleme gerçekleşmeden önce Istekler/bağımlılıklar hakkında önceden toplanmış ölçümler gönderen bir TelemetryProcessor olan otomatik Collectedölçümler ayıklayıcısı 'nı etkinleştirin/devre dışı bırakın. | true
|RequestCollectionOptions. TrackExceptions | Istek koleksiyonu modülü tarafından işlenmeyen özel durum izlemenin raporlamasını etkinleştirin/devre dışı bırakın. | NETSTANDARD 2.0'da false (özel durumlar Applicationınsightsloggerprovider ile izleniyor), aksi takdirde true.

En güncel listesi için [içindeki `ApplicationInsightsServiceOptions` yapılandırılabilir ayarlar '](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Shared/Extensions/ApplicationInsightsServiceOptions.cs) a bakın.

### <a name="sampling"></a>Örnekleme

ASP.NET Core için Application Insights SDK hem sabit hızlı hem de Uyarlamalı örneklemeyi destekler. Uyarlamalı örnekleme varsayılan olarak etkindir. 

Daha fazla bilgi için bkz. [ASP.NET Core uygulamalar için uyarlamalı örnekleme yapılandırma](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers ekleme

Tüm telemetriyle gönderilen genel özellikleri tanımlamak istediğinizde [telemetri başlatıcıları](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) 'nı kullanın.

Aşağıdaki kodda gösterildiği `TelemetryInitializer` gibi `DependencyInjection` kapsayıcıya yenilerini ekleyin. SDK otomatik olarak `DependencyInjection` kapsayıcıya eklenen her `TelemetryInitializer` birini seçer.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>TelemetryInitializers kaldırılıyor

Telemetri başlatıcıları varsayılan olarak mevcuttur. Tüm veya belirli telemetri başlatıcıları 'nı kaldırmak için, çağrısından `AddApplicationInsightsTelemetry()` *sonra* aşağıdaki örnek kodu kullanın.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

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

### <a name="adding-telemetry-processors"></a>Telemetri işlemcisi ekleme

Üzerinde genişletme yöntemi `TelemetryConfiguration` `AddApplicationInsightsTelemetryProcessor` kullanarak ' `IServiceCollection`ye özel telemetri işlemcileri ekleyebilirsiniz. Application Insights hizmetine göndereceğiniz telemetride nelerin dahil edildiğini veya dışlandığından daha doğrudan denetim sağlamak için, [Gelişmiş filtreleme senaryolarında](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) telemetri işlemcileri kullanırsınız. Aşağıdaki örneği kullanın.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Varsayılan TelemetryModules yapılandırma veya kaldırma

Application Insights, Kullanıcı tarafından el ile izlemeye gerek duymadan belirli iş yükleri hakkında faydalı telemetri otomatik olarak toplamak için telemetri modüllerini kullanır.

Aşağıdaki otomatik koleksiyon modülleri varsayılan olarak etkinleştirilmiştir. Bu modüller, otomatik olarak telemetri toplamanın sorumluluğundadır. Varsayılan davranışlarını değiştirecek şekilde devre dışı bırakabilir veya yapılandırabilirsiniz.

* `RequestTrackingTelemetryModule`-Gelen web isteklerinden Requesttelemetri toplar.
* `DependencyTrackingTelemetryModule`-Giden http çağrılarından ve SQL çağrılarından Dependencytelemetri toplar.
* `PerformanceCollectorModule`-Windows PerformanceCounters sayaçlarını toplar.
* `QuickPulseTelemetryModule`-Canlı ölçüm portalında göstermek için telemetri toplar.
* `AppServicesHeartbeatTelemetryModule`-Uygulamanın barındırıldığı Azure App Service ortamı hakkında, sinyal (özel ölçümler olarak gönder) hakkında bilgi toplar.
* `AzureInstanceMetadataTelemetryModule`-Uygulamanın barındırıldığı Azure VM ortamı hakkında (özel ölçümler olarak gönderilir) kalpler toplar.
* `EventCounterCollectionModule`- [Eventcounters toplar.](#eventcounter). Bu modül yeni bir özelliktir ve SDK sürümü 2.8.0-Beta3 ve üzeri sürümlerde kullanılabilir.

Herhangi bir varsayılanı `TelemetryModule`yapılandırmak için, aşağıdaki örnekte gösterildiği gibi `IServiceCollection`genişletme yöntemini `ConfigureTelemetryModule<T>` üzerinde kullanın.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes all default counters from EventCounterCollectionModule, and adds a single one.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
                            (module, o) =>
                            {
                                module.Counters.Clear();
                                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
                            }
                        );

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-a-telemetry-channel"></a>Telemetri kanalını yapılandırma

Varsayılan kanal `ServerTelemetryChannel`. Aşağıdaki örnekte gösterildiği gibi, geçersiz kılabilirsiniz.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Telemetrisi dinamik olarak devre dışı bırak

Telemetriyi koşullu ve dinamik olarak devre dışı bırakmak istiyorsanız, kodunuzu `TelemetryConfiguration` kodunuzda herhangi bir yere ASP.NET Core bağımlılık ekleme kapsayıcısı ile çözümleyebilir ve üzerinde `DisableTelemetry` bayrak ayarlayabilirsiniz.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

Yukarıdaki bir otomatik koleksiyon modüllerinin telemetri toplamasını önleyemediğini unutmayın. Yalnızca Application Insights telemetri gönderilmesi yukarıdaki yaklaşımla devre dışı bırakılır. Belirli bir otomatik toplama modülü istenmiyorsa [telemetri modülünü kaldırmak](#configuring-or-removing-default-telemetrymodules) için en iyisi

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Otomatik olarak toplanmayan telemetrileri nasıl izleyebilirim?

Oluşturucu Ekleme kullanarak bir `TelemetryClient` örneğini alın ve üzerinde gerekli `TrackXXX()` yöntemi çağırın. ASP.NET Core uygulamasında yeni `TelemetryClient` örnekler oluşturmanız önerilmez. Tek bir örneği `TelemetryClient` `DependencyInjection` kapsayıcıda zaten kayıtlı olduğundan telemetri geri kalanıyla birlikte paylaşır `TelemetryConfiguration` . Yeni `TelemetryClient` bir örnek oluşturmak yalnızca telemetri geri kalanından ayrı bir yapılandırmaya ihtiyaç duyduğunda önerilir. 

Aşağıdaki örnek, bir denetleyiciden ek Telemetriyi nasıl izleyeceğinizi gösterir.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Application Insights özel veri raporlama hakkında daha fazla bilgi için bkz. [Application Insights özel ölçümler API başvurusu](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Bazı Visual Studio şablonları, Application Insights etkinleştirmek için ıwebhostbuilder üzerinde Useapplicationınsights () genişletme yöntemini kullandı. Bu kullanım hala geçerli mi?

Evet, bu yöntemle Application Insights etkinleştirilmesi geçerlidir. Bu teknik, Visual Studio ekleme ve Web Apps uzantılarında kullanılır. Ancak, bazı yapılandırmayı denetlemek `services.AddApplicationInsightsTelemetry()` için aşırı yüklemeler sağladığından kullanmanızı öneririz. Her iki yöntem de dahili olarak aynı şeyi yapar, bu nedenle özel yapılandırma uygulamanız gerekmiyorsa, her iki yöntemi de çağırabilirsiniz.

`IWebHostBuilder`, ASP.NET Core 3,0 `IHostBuilder` ' de ile değiştirilmiştir ve karışıklık oluşmasını önlemek için, Application Insights Version 2.8.0-Beta3 onler useapplicationınsights () metodunu eski olarak işaretliyor ve bir sonraki ana sürümde kaldırılacak.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>ASP.NET Core uygulamamı Web Apps 'ye dağıttım. Web Apps Application Insights uzantısını yine de etkinleştirmem gerekir mi?

SDK, bu makalede gösterildiği gibi derleme zamanında yüklendiyse, App Service portalından [Application Insights uzantısını](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) etkinleştirmeniz gerekmez. Uzantı yüklü olsa bile, SDK 'nın uygulamaya zaten eklendiğini algıladığında, bu geri açılır. Uzantıyı Application Insights etkinleştirirseniz SDK 'yı yükleyip güncelleştirmeniz gerekmez. Ancak bu makaledeki yönergeleri izleyerek Application Insights etkinleştirirseniz şu nedenle daha fazla esneklik elde edersiniz:

   * Application Insights telemetri şu şekilde çalışmaya devam edecektir:
       * Windows, Linux ve Mac gibi tüm işletim sistemleri.
       * Kendi içinde veya çerçevesine bağımlı olan tüm yayımlama modları.
       * Tam .NET Framework dahil olmak üzere tüm hedef çerçeveler.
       * Web Apps, VM 'Ler, Linux, kapsayıcılar, Azure Kubernetes hizmeti ve Azure olmayan barındırma dahil olmak üzere tüm barındırma seçenekleri.
       * Önizleme sürümleri dahil olmak üzere tüm .NET Core sürümleri.
   * Visual Studio 'da hata ayıklarken telemetri ' i yerel olarak görebilirsiniz.
   * `TrackXXX()` API kullanarak ek özel telemetri izleyebilirsiniz.
   * Yapılandırma üzerinde tam denetim sahibi olursunuz.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Durum İzleyicisi gibi araçları kullanarak Application Insights izlemeyi etkinleştirebilir miyim?

Hayır. [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) ve [durum İzleyicisi v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) Şu anda yalnızca ASP.NET 4. x desteğine sahiptir.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Application Insights ASP.NET Core 2,0 uygulaması için otomatik olarak etkinleştirildi mi?

`Microsoft.AspNetCore.All` 2,0 metapackage, Application Insights SDK 'sını (sürüm 2.1.0) içerir. Uygulamayı Visual Studio hata ayıklayıcısı altında çalıştırırsanız, Visual Studio Application Insights etkinleştirilir ve doğrudan IDE 'de telemetri gösterir. Bir izleme anahtarı belirtilmediği takdirde telemetri Application Insights hizmetine gönderilmedi. 2,0 uygulamaları için bile Application Insights etkinleştirmek üzere bu makaledeki yönergeleri takip etmenizi öneririz.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Uygulamamı Linux 'ta çalıştırdım, tüm özellikler destekleniyor mu?

Evet. SDK için özellik desteği, aşağıdaki özel durumlarla birlikte tüm platformlarda aynıdır:

* Performans sayaçları yalnızca Windows 'ta desteklenir.
* `ServerTelemetryChannel` Varsayılan olarak etkin olsa da, uygulama Linux veya MacOS 'ta çalışıyorsa, ağ sorunları varsa Telemetriyi geçici tutmak için kanal otomatik olarak yerel bir depolama klasörü oluşturmaz. Bu sınırlama nedeniyle, geçici ağ veya sunucu sorunları olduğunda telemetri kaybedilir. Bu sorunu geçici olarak çözmek için, kanal için yerel bir klasör yapılandırın:

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
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>Açık kaynaklı SDK

[Kodu okuyun ve koda katkıda bulunun](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- [.NET Core ve Visual Studio 'yu sıfırdan Application Insights yapılandırmak](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) için bu dış adım adım videoya göz atın.
- [.NET Core ve sıfırdan Visual Studio Code Application Insights yapılandırmak](https://youtu.be/ygGt84GDync) için bu dış adım adım videoya göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* Kullanıcıların uygulamanızda nasıl gezintiğini anlamak için [Kullanıcı akışlarını araştırma](../../azure-monitor/app/usage-flows.md) .
* Bir özel durum oluştuğunda kaynak kodu ve değişkenlerin durumunu görmek için [bir anlık görüntü koleksiyonu yapılandırın](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) .
* Uygulamanızın performans ve kullanımının ayrıntılı bir görünümü için kendi olay ve ölçümlerinizi göndermek üzere [API 'Yi kullanın](../../azure-monitor/app/api-custom-events-metrics.md) .
* Uygulamanızı dünyanın her yerindeki sürekli denetlemek için [kullanılabilirlik testlerini](../../azure-monitor/app/monitor-web-app-availability.md) kullanın.
* [ASP.NET Core bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)
