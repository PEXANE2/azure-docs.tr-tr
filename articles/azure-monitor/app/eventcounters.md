---
title: Application Insights 'de olay sayaçları | Microsoft Docs
description: Application Insights 'de sistem ve özel .NET/.NET Core EventCounters ' i izleyin.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 37d0e1e741548986788be78860830f36add1f5a8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83700442"
---
# <a name="eventcounters-introduction"></a>EventCounters giriş

`EventCounter`, sayaçlar veya istatistikler yayımlamak ve kullanmak için .NET/.NET Core mekanizmasıdır. [Bu](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md) belge, bir genel bakış `EventCounters` ve bunların nasıl yayımlanalınacağını ve kullanılacağına dair örneklere sahip olmanızı sağlar. EventCounters tüm işletim sistemi platformlarında desteklenir-Windows, Linux ve macOS. Yalnızca Windows sistemlerinde desteklenen [PerformanceCounters](https://docs.microsoft.com/dotnet/api/system.diagnostics.performancecounter) için platformlar arası eşdeğer olarak düşünülebilir.

Kullanıcılar ihtiyaçlarını karşılamak için herhangi bir özel yayım yaparken `EventCounters` , .NET Core 3,0 çalışma zamanı varsayılan olarak bu sayaçların bir kümesini yayımlar. Belge, Azure Application Insights toplamak ve görüntülemek için gerekli adımları `EventCounters` (sistem tanımlı veya Kullanıcı tanımlı) adım adım gösterecektir.

## <a name="using-application-insights-to-collect-eventcounters"></a>EventCounters toplamak için Application Insights kullanma

Application Insights `EventCounters` `EventCounterCollectionModule` , yeni yayınlanan NuGet paketi [Microsoft. ApplicationInsights. eventcountercollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector)'ın bir parçası olan ile toplamayı destekler. `EventCounterCollectionModule`, [Aspnetcore](asp-net-core.md) veya [workerservice](worker-service.md)kullanılırken otomatik olarak etkinleştirilir. `EventCounterCollectionModule`yapılandırılabilir olmayan bir koleksiyon frekansı 60 saniye olan sayaçları toplar. EventCounters toplamak için gerekli özel izinler yoktur.

## <a name="default-counters-collected"></a>Toplanan varsayılan sayaçlar

.NET Core 3,0 ' de çalışan uygulamalar için aşağıdaki sayaçlar SDK tarafından otomatik olarak toplanır. Sayaçların adı "Kategori | biçiminde olacaktır. Counter ".

|Kategori | Sayaç|
|---------------|-------|
|`System.Runtime` | `cpu-usage` |
|`System.Runtime` | `working-set` |
|`System.Runtime` | `gc-heap-size` |
|`System.Runtime` | `gen-0-gc-count` |
|`System.Runtime` | `gen-1-gc-count` |
|`System.Runtime` | `gen-2-gc-count` |
|`System.Runtime` | `time-in-gc` |
|`System.Runtime` | `gen-0-size` |
|`System.Runtime` | `gen-1-size` |
|`System.Runtime` | `gen-2-size` |
|`System.Runtime` | `loh-size` |
|`System.Runtime` | `alloc-rate` |
|`System.Runtime` | `assembly-count` |
|`System.Runtime` | `exception-count` |
|`System.Runtime` | `threadpool-thread-count` |
|`System.Runtime` | `monitor-lock-contention-count` |
|`System.Runtime` | `threadpool-queue-length` |
|`System.Runtime` | `threadpool-completed-items-count` |
|`System.Runtime` | `active-timer-count` |
|`Microsoft.AspNetCore.Hosting` | `requests-per-second` |
|`Microsoft.AspNetCore.Hosting` | `total-requests` |
|`Microsoft.AspNetCore.Hosting` | `current-requests` |
|`Microsoft.AspNetCore.Hosting` | `failed-requests` |

> [!NOTE]
> Microsoft. AspNetCore. Hosting kategorisi sayaçları yalnızca ASP.NET Core uygulamalarında eklenir.

## <a name="customizing-counters-to-be-collected"></a>Toplanacak sayaçları özelleştirme

Aşağıdaki örnek, sayaçların nasıl ekleneceğini/kaldırılacağını gösterir. Bu özelleştirme, `ConfigureServices` veya kullanarak Application Insights telemetri koleksiyonu etkinleştirildikten sonra uygulamanızın yönteminde yapılır `AddApplicationInsightsTelemetry()` `AddApplicationInsightsWorkerService()` . Aşağıda bir ASP.NET Core uygulamasından örnek bir kod verilmiştir. Diğer uygulama türleri için [Bu](worker-service.md#configuring-or-removing-default-telemetrymodules) belgeye başvurun.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows several customizations done to EventCounterCollectionModule.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );

        // The following code removes EventCounterCollectionModule to disable the module completely.
        var eventCounterModule = services.FirstOrDefault<ServiceDescriptor>
                    (t => t.ImplementationType == typeof(EventCounterCollectionModule));
        if (eventCounterModule != null)
        {
            services.Remove(eventCounterModule);
        }
    }
```

## <a name="event-counters-in-metric-explorer"></a>Ölçüm Gezgini 'nde olay sayaçları

[Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)'Nde EventCounter ölçümlerini görüntülemek için Application Insights kaynak ' ı seçin ve ölçüm ad alanı olarak günlük tabanlı ölçümler ' i seçin. Daha sonra EventCounter ölçümleri özel kategori altında gösterilir.

> [!div class="mx-imgBorder"]
> ![Application Insights bildirilen olay sayaçları](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Analiz içindeki olay sayaçları

Ayrıca, **Customölçümler** tablosunda [analiz](../../azure-monitor/app/analytics.md)' de olay sayacı raporlarını arayabilir ve görüntüleyebilirsiniz.

Örneğin, hangi sayaçların toplandığını ve sorgu için kullanılabilir olduğunu görmek için aşağıdaki sorguyu çalıştırın:

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Application Insights bildirilen olay sayaçları](./media/event-counters/analytics-event-counters.png)

Belirli bir sayacın (örneğin:) bir grafiğini `ThreadPool Completed Work Item Count` son döneme almak için aşağıdaki sorguyu çalıştırın.

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Application Insights tek bir sayaca sohbet etme](./media/event-counters/analytics-completeditems-counters.png)

Diğer telemetri gibi, **Customölçümleri** de `cloud_RoleInstance` uygulamanızın çalıştırıldığı ana bilgisayar sunucusu örneğinin kimliğini belirten bir sütun içerir. Yukarıdaki sorgu, örnek başına sayaç değerini gösterir ve farklı sunucu örneklerinin performansını karşılaştırmak için kullanılabilir.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi, bir olay sayacı belirttiğiniz sınırın dışında kaldığında sizi uyarmak üzere [bir uyarı ayarlayabilirsiniz](../../azure-monitor/platform/alerts-log.md) . Uyarılar bölmesini açın ve uyarı Ekle ' ye tıklayın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Canlı ölçümlerde EventCounters görebilir miyim?

Canlı ölçümler, bugün itibariyle EventCounters göstermez. Telemetriyi görmek için ölçüm Gezgini 'ni veya analizlerini kullanın.

### <a name="which-platforms-can-i-see-the-default-list-of-net-core-30-counters"></a>.NET Core 3,0 sayaçlarının varsayılan listesini hangi platformlar görebilir?

EventCounter herhangi bir özel izin gerektirmez ve tüm platformlarda desteklenir .NET Core 3,0 desteklenir. Buna aşağıdakiler dahildir:

* **İşletim sistemi**: Windows, Linux veya MacOS.
* **Barındırma yöntemi**: işlemde veya işlem dışı.
* **Dağıtım yöntemi**: çerçeveye bağımlı veya kendine dahil.
* **Web sunucusu**: IIS (Internet Information Server) veya Kestrel.
* **Barındırma platformu**: Azure App Service, Azure VM, Docker, Azure Kubernetes hizmeti (aks) ve benzeri Web Apps özelliği.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Azure Web App Portal 'dan Application Insights etkinleştirdim. Ancak EventCounters göremiyorum.?

 ASP.NET Core için [Application Insights uzantısı](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) henüz bu özelliği desteklemez. Bu özellik destekleniyorsa, bu belge güncelleştirilecektir.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)
