---
title: Uygulama Öngörülerinde Performans Sayaçları | Microsoft Dokümanlar
description: Uygulama Öngörüleri'nde sistemi ve özel .NET performans sayaçlarını izleyin.
ms.topic: conceptual
ms.date: 12/13/2018
ms.openlocfilehash: 94d2520c17867f6d70caffd002a76365a425986f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669888"
---
# <a name="system-performance-counters-in-application-insights"></a>Uygulama Öngörüleri'nde sistem performans sayaçları

Windows tarafından CPU doluluğu, bellek, disk ve ağ kullanımı gibi şeylere yönelik birçok çeşit [performans sayacı](https://docs.microsoft.com/windows/desktop/PerfCtrs/about-performance-counters) sunulur. Ayrıca kendi performans sayaçlarınızı da tanımlayabilirsiniz. Performans sayaçları koleksiyonu, uygulamanız IIS altında şirket içi bir ana bilgisayarda veya yönetim erişimine sahip olduğunuz sanal makinede çalıştırıldığı sürece desteklenir. Azure Web Apps olarak çalışan uygulamaların performans sayaçlarına doğrudan erişimi olmasa da, kullanılabilir sayaçların bir alt kümesi Application Insights tarafından toplanır.

## <a name="view-counters"></a>Sayaçları görüntüleme

Ölçümler bölmesi varsayılan performans sayaçları kümesini gösterir.

![Uygulama Öngörüleri'nde bildirilen performans sayaçları](./media/performance-counters/performance-counters.png)

Core web uygulamaları ASP.NET/ASP.NET için toplanacak şekilde yapılandırılan geçerli varsayılan sayaçlar şunlardır:
- %\\İşlem İşlemci Süresi
- %\\İşlem İşlemci Süresi Normalleştirilmiş
- Bellek\\Kullanılabilir Baytlar
- ASP.NET İstekleri/Sn
- .NET CLR Özel Durumlar Atılmış / sn
- ASP.NET BaşvurularıYürütme Süresi İsten
- İşlem\\Özel Baytlar
- Proses\\IO Veri Baytları/sn
- ASP.NET Uygulama\\Kuyruğundaki Uygulama İstekleri
- İşlemci(_Total)\\% İşlemci Süresi

## <a name="add-counters"></a>Sayaç ekleme

İstediğiniz performans sayacı ölçümler listesine dahil değilse, ekleyebilirsiniz.

1. Yerel sunucudaki bu PowerShell komutunu kullanarak sunucunuzda hangi sayaçların mevcut olduğunu öğrenin:

    `Get-Counter -ListSet *`

    (Bkz. [`Get-Counter`](https://technet.microsoft.com/library/hh849685.aspx).)
2. Açık ApplicationInsights.config.

   * Geliştirme sırasında uygulamanız için Application Insights eklediyseniz, Project'inizde ApplicationInsights.config'i edin ve sunucularınıza yeniden dağıtın.
3. Performans toplayıcı yönergesini edin:

    ```XML

        <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
          <Counters>
            <Add PerformanceCounter="\Objects\Processes"/>
            <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
          </Counters>
        </Add>
    ```

> [!NOTE]
> ASP.NET Çekirdek uygulamaları yoktur `ApplicationInsights.config`ve bu nedenle yukarıdaki yöntem ASP.NET Temel Uygulamalar için geçerli değildir.

Hem standart sayaçları hem de kendi uyguladığınız sayaçları yakalayabilirsiniz. `\Objects\Processes`tüm Windows sistemlerinde kullanılabilen standart bir sayaç örneğidir. `\Sales(photo)\# Items Sold`bir web hizmetinde uygulanabilecek özel bir sayaç örneğidir.

`\Category(instance)\Counter"`Biçimi, ya da örnekleri olmayan kategoriler için, `\Category\Counter`sadece .

`ReportAs`eşleşmeyan `[a-zA-Z()/-_ \.]+` sayaç adları için gereklidir - yani, aşağıdaki kümelerde olmayan karakterler içerir: harfler, yuvarlak köşeli ayraçlar, ileri eğik çizgi, tire, alt çizgi, boşluk, nokta.

Bir örnek belirtirseniz, bildirilen ölçümün "CounterInstanceName" boyutu olarak toplanır.

### <a name="collecting-performance-counters-in-code-for-aspnet-web-applications-or-netnet-core-console-applications"></a>ASP.NET Web Uygulamaları veya .NET/.NET Core Console Uygulamaları için kodda performans sayaçlarının toplanması
Sistem performans sayaçlarını toplamak ve Bunları Application Insights'a göndermek için aşağıdaki parçacığı uyarlayabilirsiniz:


```csharp
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Process([replace-with-application-process-name])\Page Faults/sec", "PageFaultsPerfSec")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

Veya oluşturduğunuz özel ölçümlerle aynı şeyi yapabilirsiniz:

```csharp
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="collecting-performance-counters-in-code-for-aspnet-core-web-applications"></a>ASP.NET Çekirdek Web Uygulamaları için kodda performans sayaçları toplama

Sınıfınızdaki yöntemi `Startup.cs` aşağıdaki gibi değiştirin. `ConfigureServices`

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures PerformanceCollectorModule.
  services.ConfigureTelemetryModule<PerformanceCollectorModule>((module, o) =>
            {
                // the application process name could be "dotnet" for ASP.NET Core self-hosted applications.
                module.Counters.Add(new PerformanceCounterCollectionRequest(
    @"\Process([replace-with-application-process-name])\Page Faults/sec", "DotnetPageFaultsPerfSec"));
            });
    }
```

## <a name="performance-counters-in-analytics"></a>Analytics'te performans sayaçları
[Analytics'te](../../azure-monitor/app/analytics.md)performans sayacı raporlarını arayabilir ve görüntüleyebilirsiniz.

**PerformanceCounters** şema, `category` `counter` her performans sayacının adını ve `instance` adını ortaya çıkarır.  Her uygulama için telemetride, yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, hangi sayaçların kullanılabileni görmek için: 

![Application Insights analizinde performans sayaçları](./media/performance-counters/analytics-performance-counters.png)

('Örnek' burada performans sayacı örneğini ifade eder, rolü değil, sunucu makinesi örneğini ifade eder. Performans sayacı örnek adı genellikle işlem veya uygulamanın adına işlemci zamanı gibi sayaçları parçalar.)

Son dönemde kullanılabilir belleğin grafiğini almak için: 

![Uygulama Öngörüleri analizinde bellek zaman çizelgesi](./media/performance-counters/analytics-available-memory.png)

Diğer telemetriler gibi **performanceCounters'ın** da uygulamanızın çalıştığı ana bilgisayar sunucusu örneğinin kimliğini gösteren bir sütunu `cloud_RoleInstance` vardır. Örneğin, uygulamanızın farklı makinelerdeki performansını karşılaştırmak için: 

![Uygulama Öngörüleri analizinde rol örneğine göre bölümlenen performans](./media/performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET ve Uygulama Öngörüleri sayar

*Özel Durum oranı ve Özel Durumlar ölçümleri arasındaki fark nedir?*

* *Özel durum oranı* bir sistem performans sayacıdır. CLR, atılan tüm işlenmiş ve işlenmemiş özel durumları sayar ve toplamı bir örnekleme aralığındaki aralığın uzunluğuna böler. Application Insights SDK bu sonucu toplar ve portala gönderir.

* *Özel durumlar,* portal tarafından grafiğin örnekleme aralığında alınan TrackException raporlarının sayısıdır. Yalnızca kodunuzda TrackException çağrıları yazdığınız ve [işlenmemiş](../../azure-monitor/app/asp-net-exceptions.md)tüm özel durumları içermediği işlenmemiş özel durumları içerir. 

## <a name="performance-counters-for-applications-running-in-azure-web-apps"></a>Azure Web Apps'ta çalışan uygulamalar için performans sayaçları

Azure Web Apps'a dağıtılan hem ASP.NET hem de ASP.NET Core uygulamaları özel bir kum havuzu ortamında çalıştırılır. Bu ortam, sistem performans sayaçlarına doğrudan erişime izin vermez. Ancak, sayaçların sınırlı bir alt kümesi [burada](https://github.com/projectkudu/kudu/wiki/Perf-Counters-exposed-as-environment-variables)açıklandığı gibi çevre değişkenleri olarak ortaya çıkar. ASP.NET ve ASP.NET Core için Application Insights SDK, azure web apps'tan bu özel ortam değişkenlerinden performans sayaçları toplar. Bu ortamda yalnızca bir sayaç alt kümesi mevcuttur ve tam liste [burada bulunabilir.](https://github.com/microsoft/ApplicationInsights-dotnet-server/blob/develop/WEB/Src/PerformanceCollector/Perf.Shared/Implementation/WebAppPerformanceCollector/CounterFactory.cs)

## <a name="performance-counters-in-aspnet-core-applications"></a>ASP.NET Core uygulamalarında performans sayaçları

ASP.NET Core'daki performans sayaçları desteği sınırlıdır:

* [SDK](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm2.4.1 ve daha sonra uygulama Azure Web Apps (Windows) üzerinde çalışıyorsa performans sayaçları toplayın.
* SDK sürümleri 2.7.1 ve daha sonra uygulama Windows ve `NETSTANDARD2.0` hedefleri veya daha sonra çalışıyorsa performans sayaçları toplamak.
* .NET Framework'ü hedefleyen uygulamalarda, SDK'nın tüm sürümleri performans sayaçlarını destekler.
* SDK Sürümleri 2.8.0 ve daha sonra Linux cpu / bellek sayacı destekler. Linux'ta başka hiçbir sayaç desteklenmez. Linux'ta (ve Diğer Windows dışındaki ortamlarda) sistem sayaçları almak için önerilen yol [EventCounters](eventcounters.md) kullanarak

## <a name="alerts"></a>Uyarılar
Diğer ölçümler gibi, bir performans sayacı belirttiğiniz sınırın dışına çıkarsa sizi uyarmak için [bir uyarı ayarlayabilirsiniz.](../../azure-monitor/app/alerts.md) Uyarılar bölmesini açın ve Uyarı Ekle'yi tıklatın.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

* [Bağımlılık izleme](../../azure-monitor/app/asp-net-dependencies.md)
* [Özel durum izleme](../../azure-monitor/app/asp-net-exceptions.md)

