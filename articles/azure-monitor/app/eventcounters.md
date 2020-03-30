---
title: Uygulama Öngörüleri'ndeki etkinlik sayaçları | Microsoft Dokümanlar
description: Uygulama Öngörülerinde sistemi ve özel .NET/.NET Core EventCounters'ı izleyin.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2094c012e86131073fc66be4f2ac2fb2e81ef4c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663598"
---
# <a name="eventcounters-introduction"></a>EventCounters giriş

`EventCounter`sayaçları veya istatistikleri yayınlamak ve tüketmek için .NET/.NET Core mekanizmasıdır. [Bu belge,](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md) nasıl `EventCounters` yayımlanıp tüketileceğine ilişkin genel bir bakış ve örnekler verir. EventCounter'lar tüm işletim sistemi platformlarında desteklenir - Windows, Linux ve macOS. Yalnızca Windows sistemlerinde desteklenen [PerformanceCounter'lar](https://docs.microsoft.com/dotnet/api/system.diagnostics.performancecounter) için bir çapraz platform eşdeğeri olarak düşünülebilir.

Kullanıcılar gereksinimlerini karşılamak `EventCounters` için herhangi bir özel yayımlayabilirken, .NET Core 3.0 çalışma zamanı varsayılan olarak bu sayaçlardan oluşan bir küme yayımlar. Belge, Azure Uygulama Öngörüleri'nde `EventCounters` (sistem tanımlı veya kullanıcı tanımlı) toplamak ve görüntülemek için gereken adımlardan geçer.

## <a name="using-application-insights-to-collect-eventcounters"></a>EventCounter'ları toplamak için Uygulama Öngörülerini Kullanma

Application Insights, `EventCounters` yeni `EventCounterCollectionModule`yayımlanan nuget paketi [Microsoft.ApplicationInsights.EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector)bir parçasıdır ile toplama destekler. `EventCounterCollectionModule`[AspNetCore](asp-net-core.md) veya [WorkerService](worker-service.md)kullanırken otomatik olarak etkinleştirilir. `EventCounterCollectionModule`60 saniyelik yapılandırılamayan toplama frekansına sahip sayaçları toplar. EventCounters toplamak için gerekli özel izinler yoktur.

## <a name="default-counters-collected"></a>Toplanan varsayılan sayaçlar

.NET Core 3.0'da çalışan uygulamalar için aşağıdaki sayaçlar SDK tarafından otomatik olarak toplanır. Sayaçların adı "Kategori| Sayaç".

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
> Microsoft.AspNetCore.Hosting kategorisi sayaçları yalnızca ASP.NET Çekirdek Uygulamalarına eklenir.

## <a name="customizing-counters-to-be-collected"></a>Toplanacak sayaçları özelleştirme

Aşağıdaki örnek, sayaçların nasıl ekleyeceğini/kaldırılacaklarını gösterir. Bu özelleştirme, `ConfigureServices` Application Insights telemetri koleksiyonu ya da `AddApplicationInsightsTelemetry()` `AddApplicationInsightsWorkerService()`. Aşağıda, ASP.NET Core uygulamasından bir örnek kod verilmiştir. Diğer uygulama türü için [bu](worker-service.md#configuring-or-removing-default-telemetrymodules) belgeye bakın.

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

## <a name="event-counters-in-metric-explorer"></a>Metrik Gezgin'deki olay sayaçları

[Metrik](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)Gezgini'nde EventCounter ölçümlerini görüntülemek için Uygulama Öngörüleri kaynağını seçin ve metrik ad alanı olarak Günlük tabanlı ölçümleri seçin. Ardından EventCounter ölçümleri Özel kategori altında görüntülenir.

> [!div class="mx-imgBorder"]
> ![Uygulama Öngörüleri'nde bildirilen etkinlik sayaçları](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Analytics'te etkinlik sayaçları

Ayrıca [Analytics'te,](../../azure-monitor/app/analytics.md) **özelMetrics** tablosunda olay sayacı raporlarını arayabilir ve görüntüleyebilirsiniz.

Örneğin, hangi sayaçların toplandığını ve sorguiçin kullanılabilir olduğunu görmek için aşağıdaki sorguyu çalıştırın:

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Uygulama Öngörüleri'nde bildirilen etkinlik sayaçları](./media/event-counters/analytics-event-counters.png)

Son dönemde belirli bir sayacın `ThreadPool Completed Work Item Count`grafiğini (örneğin: ) almak için aşağıdaki sorguyu çalıştırın.

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Uygulama Öngörüleri'nde tek bir sayacın sohbeti](./media/event-counters/analytics-completeditems-counters.png)

Diğer telemetriler gibi **customMetrics'te** de uygulamanızın çalıştığı ana bilgisayar sunucusu örneğinin kimliğini gösteren bir sütun `cloud_RoleInstance` vardır. Yukarıdaki sorgu, örnek başına sayaç değerini gösterir ve farklı sunucu örneklerinin performansını karşılaştırmak için kullanılabilir.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi, bir olay sayacı belirttiğiniz sınırın dışına çıkarsa sizi uyarmak için [bir uyarı ayarlayabilirsiniz.](../../azure-monitor/app/alerts.md) Uyarılar bölmesini açın ve Uyarı Ekle'yi tıklatın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Canlı Ölçümlerde EventCounters'ı görebilir miyim?

Canlı Ölçümler bugün itibariyle EventCounters göstermez. Telemetriyi görmek için Metrik Explorer veya Analytics'i kullanın.

### <a name="which-platforms-can-i-see-the-default-list-of-net-core-30-counters"></a>.NET Core 3.0 sayaçlarının varsayılan listesini hangi platformlarda görebiliyorum?

EventCounter herhangi bir özel izin gerektirmez ve tüm platformlarda desteklenir .NET Core 3.0 desteklenir. Buna aşağıdakiler dahildir:

* **İşletim sistemi**: Windows, Linux veya macOS.
* **Barındırma yöntemi**: Süreç içinde veya süreç dışında.
* **Dağıtım yöntemi**: Çerçeveye bağımlı veya bağımsız.
* **Web sunucusu**: IIS (Internet Information Server) veya Kerkenez.
* **Barındırma platformu**: Azure Uygulama Hizmeti, Azure VM, Docker, Azure Kubernetes Hizmeti (AKS) vb. Web Apps özelliği.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Azure Web App Portalı'ndan Uygulama Öngörüleri'ni etkinleştirdim. Ama EventCounters göremiyorum.?

 ASP.NET Core için [Uygulama Öngörüleri uzantısı](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) henüz bu özelliği desteklemez. Bu özellik desteklendiğinde bu belge güncelleştirilir.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)
