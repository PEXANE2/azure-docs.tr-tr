---
title: Application Insights 'de olay sayaçları | Microsoft Docs
description: Application Insights 'de sistem ve özel .NET/.NET Core EventCounters ' i izleyin.
services: application-insights
documentationcenter: ''
author: cithomas
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: cithomas
ms.openlocfilehash: fc9148d4f4c5920210b9218ca70f270bae3b663b
ms.sourcegitcommit: a6718e2b0251b50f1228b1e13a42bb65e7bf7ee2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71273935"
---
# <a name="eventcounters-introduction"></a>EventCounters giriş

`EventCounter`, sayaçlar veya istatistikler yayımlamak ve kullanmak için .NET/.NET Core mekanizmasıdır. [Bu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md) belge, bir genel bakış `EventCounters` ve bunların nasıl yayımlanalınacağını ve kullanılacağına dair örneklere sahip olmanızı sağlar. EventCounters tüm işletim sistemi platformlarında desteklenir-Windows, Linux ve macOS. Yalnızca Windows sistemlerinde desteklenen [PerformanceCounters](https://docs.microsoft.com/dotnet/api/system.diagnostics.performancecounter) için platformlar arası eşdeğer olarak düşünülebilir.

Kullanıcılar ihtiyaçlarını karşılamak için herhangi bir `EventCounters` özel yayım yaparken, .NET Core 3,0 çalışma zamanı varsayılan olarak bu sayaçların bir kümesini yayımlar. Belge, Azure Application Insights toplamak ve görüntülemek `EventCounters` için gerekli adımları (sistem tanımlı veya Kullanıcı tanımlı) adım adım gösterecektir.

## <a name="using-application-insights-to-collect-eventcounters"></a>EventCounters toplamak için Application Insights kullanma

Application Insights `EventCounterCollectionModule`, yeni `EventCounters` yayınlanan NuGet paketi [Microsoft. ApplicationInsights. eventcountercollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector)'ın bir parçası olan ile toplamayı destekler. `EventCounterCollectionModule`, [Aspnetcore](asp-net-core.md) veya [workerservice](worker-service.md)kullanılırken otomatik olarak etkinleştirilir. `EventCounterCollectionModule`yapılandırılabilir olmayan bir koleksiyon frekansı 60 saniye olan sayaçları toplar. EventCounters toplamak için gerekli özel izinler yoktur.

## <a name="default-counters-collected"></a>Toplanan varsayılan sayaçlar

.NET Core 3,0 ' de çalışan uygulamalar için aşağıdaki sayaçlar SDK tarafından otomatik olarak toplanır. Sayaçların adı "Kategori | biçiminde olacaktır. Counter ".

|Category | Sayaç|
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
> Microsoft. AspNetCore. Hosting kategorisi sayaçları yalnızca Asp.Net Core uygulamalarında eklenir.

## <a name="customizing-counters-to-be-collected"></a>Toplanacak sayaçları özelleştirme

Aşağıdaki örnek, sayaçların nasıl ekleneceğini/kaldırılacağını gösterir. Bu özelleştirme, `ConfigureServices` `AddApplicationInsightsTelemetry()` veya `AddApplicationInsightsWorkerService()`kullanarak Application Insights telemetri koleksiyonu etkinleştirildikten sonra uygulamanızın yönteminde yapılır. Aşağıda bir ASP.NET Core uygulamasından örnek bir kod verilmiştir. Diğer uygulama türleri için [Bu](worker-service.md#configuring-or-removing-default-telemetrymodules) belgeye başvurun.

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

[Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts)'Nde EventCounter ölçümlerini görüntülemek için Application Insights kaynak ' ı seçin ve ölçüm ad alanı olarak günlük tabanlı ölçümler ' i seçin. Daha sonra EventCounter ölçümleri PerformanceCounter kategorisi altında gösterilir.

> [!div class="mx-imgBorder"]
> ![Application Insights bildirilen olay sayaçları](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Analiz içindeki olay sayaçları

Ayrıca, **PerformanceCounters** tablosunda [analiz](../../azure-monitor/app/analytics.md)' de olay sayacı raporlarını arayabilir ve görüntüleyebilirsiniz.

Örneğin, hangi sayaçların toplandığını ve sorgu için kullanılabilir olduğunu görmek için aşağıdaki sorguyu çalıştırın:

```Kusto
performanceCounters | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Application Insights bildirilen olay sayaçları](./media/event-counters/analytics-event-counters.png)

Belirli bir sayacın (örneğin: `ThreadPool Completed Work Item Count`) bir grafiğini son döneme almak için aşağıdaki sorguyu çalıştırın.

```Kusto
performanceCounters 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Application Insights tek bir sayaca sohbet etme](./media/event-counters/analytics-completeditems-counters.png)

Diğer telemetri gibi, **PerformanceCounters** da uygulamanızın üzerinde çalıştığı `cloud_RoleInstance` ana bilgisayar sunucusu örneğinin kimliğini belirten bir sütun içerir. Yukarıdaki sorgu, örnek başına sayaç değerini gösterir ve farklı sunucu örneklerinin performansını karşılaştırmak için kullanılabilir.

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi, bir olay sayacı belirttiğiniz sınırın dışında kaldığında sizi uyarmak üzere [bir uyarı ayarlayabilirsiniz](../../azure-monitor/app/alerts.md) . Uyarılar bölmesini açın ve uyarı Ekle ' ye tıklayın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Canlı ölçümlerde EventCounters görebilir miyim?

Canlı ölçümler, bugün itibariyle EventCounters göstermez. Telemetriyi görmek için ölçüm Gezgini 'ni veya analizlerini kullanın.

### <a name="which-platforms-can-i-see-the-default-list-of-net-core-30-counters"></a>.NET Core 3,0 sayaçlarının varsayılan listesini hangi platformlar görebilir?

EventCounter herhangi bir özel izin gerektirmez ve tüm platformlarda desteklenir .NET Core 3,0 desteklenir. Buna aşağıdakiler dahildir:

* **İşletim sistemi**: Windows, Linux veya macOS.
* **Barındırma yöntemi**: İşlem veya işlem dışı.
* **Dağıtım yöntemi**: Çerçeveye bağımlı veya kendine dahil.
* **Web sunucusu**: IIS (Internet Information Server) veya Kestrel.
* **Barındırma platformu**: Azure App Service, Azure VM, Docker, Azure Kubernetes hizmeti (AKS) ve benzeri Web Apps özelliği.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Azure Web App Portal 'dan Application Insights etkinleştirdim. Ancak EventCounters göremiyorum.?

 ASP.NET Core için [Application Insights uzantısı](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps) henüz bu özelliği desteklemez. Bu özellik destekleniyorsa, bu belge güncelleştirilecektir.

## <a name="next"></a>Sonraki adımlar

* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)