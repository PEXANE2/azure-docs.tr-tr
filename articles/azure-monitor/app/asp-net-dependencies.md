---
title: Azure Uygulama Öngörülerinde Bağımlılık İzleme | Microsoft Dokümanlar
description: Uygulama Öngörüleri ile şirket içi veya Microsoft Azure web uygulamanızdan gelen bağımlılık çağrılarını izleyin.
ms.topic: conceptual
ms.date: 03/26/2020
ms.openlocfilehash: 1e30d8036c1fc624d39f027f38e314c6c57360f6
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731503"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Azure Uygulama Öngörülerinde Bağımlılık İzleme 

*Bağımlılık,* uygulamanız tarafından çağrılan harici bir bileşendir. Genellikle HTTP, veritabanı veya dosya sistemi kullanılarak adlandırılan bir hizmettir. [Application Insights,](../../azure-monitor/app/app-insights-overview.md) bağımlılık adı ve benzeri ek bilgilerle birlikte, başarısız olsun veya olmasın, bağımlılık çağrılarının süresini ölçer. Belirli bağımlılık çağrılarını araştırabilir ve bunları istekler ve özel durumlar ile ilişkilendirebilirsiniz.

## <a name="automatically-tracked-dependencies"></a>Otomatik olarak izlenen bağımlılıklar

Bağımlılıkları otomatik olarak toplayan bir Telemetri Modülü `DependencyTrackingTelemetryModule` olan .NET ve .NET Core gemileri için Uygulama Öngörüleri SDK'ları. Bu bağımlılık koleksiyonu, bağlı resmi dokümanlara göre yapılandırıldığında, [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ve [ASP.NET Çekirdek](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) uygulamaları için otomatik olarak etkinleştirilir. `DependencyTrackingTelemetryModule` [bu](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) NuGet paketi olarak sevk edilir ve NuGet paketlerinden `Microsoft.ApplicationInsights.Web` birini `Microsoft.ApplicationInsights.AspNetCore`kullanırken otomatik olarak getirilir veya .

 `DependencyTrackingTelemetryModule`şu anda aşağıdaki bağımlılıkları otomatik olarak izler:

|Bağımlılıklar |Ayrıntılar|
|---------------|-------|
|Http/Https | Yerel veya Uzak http/https aramaları |
|WCF aramaları| Yalnızca Http tabanlı bağlamalar kullanılıyorsa otomatik olarak izlenir.|
|SQL | Yapılan aramalar `SqlClient`. SQL sorgusuyakalama için [buna](#advanced-sql-tracking-to-get-full-sql-query) bakın.  |
|[Azure depolama (Blob, Tablo, Sıra)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure Depolama İstemci ile yapılan aramalar. |
|[EventHub İstemci SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Sürüm 1.1.0 ve üzeri. |
|[ServiceBus Müşteri SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve üzeri. |
|Azure Cosmos DB | Yalnızca HTTP/HTTPS kullanılırsa otomatik olarak izlenir. TCP modu Application Insights tarafından yakalanmaz. |

Bir bağımlılık eksikveya farklı bir SDK kullanıyorsanız otomatik [toplanan bağımlılıklar](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies)listesinde olduğundan emin olun. Bağımlılık otomatik olarak toplanmıyorsa, [bir parça bağımlılık çağrısıyla](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency)el ile takip edebilirsiniz.

## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Konsol Uygulamalarında otomatik bağımlılık izleme kurulumu

.NET konsol uygulamalarından bağımlılıkları otomatik olarak izlemek `Microsoft.ApplicationInsights.DependencyCollector`için Nuget `DependencyTrackingTelemetryModule` paketini yükleyin ve aşağıdaki gibi başlatılmasını sağlar:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

.NET Core konsol uygulamaları için TelemetryConfiguration.Active eskidir. [İşçi hizmet belgelerindeki](https://docs.microsoft.com/azure/azure-monitor/app/worker-service) kılavuza ve [ASP.NET Çekirdek izleme belgelerine](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) bakın

### <a name="how-automatic-dependency-monitoring-works"></a>Otomatik bağımlılık izleme nasıl çalışır?

Bağımlılıklar, aşağıdaki tekniklerden biri kullanılarak otomatik olarak toplanır:

* Seçili yöntemler etrafında bayt kodu enstrümantasyonunu kullanma. (StatusMonitor veya Azure Web App Uzantısı'ndan InstrumentationEngine)
* EventSource geri aramaları
* DiagnosticSource geri aramaları (en son .NET/.NET Core SDK'larda)

## <a name="manually-tracking-dependencies"></a>Bağımlılıkları el ile izleme

Aşağıda, otomatik olarak toplanmayan ve dolayısıyla el ile izleme gerektiren bazı bağımlılık örnekleri verilmiştir.

* Azure Cosmos DB, [yalnızca HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) kullanılırsa otomatik olarak izlenir. TCP modu Application Insights tarafından yakalanmaz.
* Redis

SDK tarafından otomatik olarak toplanmayan bu bağımlılıklar için, standart otomatik toplama modülleri tarafından kullanılan [TrackDependency API'sini](api-custom-events-metrics.md#trackdependency) kullanarak bunları el ile izleyebilirsiniz.

Örneğin, kodunuzu kendi yazdığınız bir derlemeyle oluşturursanız, yanıt sürenize ne gibi bir katkı sağladığını öğrenmek için tüm aramaları zamanlayabilirsiniz. Bu verilerin Uygulama Öngörüleri'ndeki bağımlılık grafiklerinde görüntülenmesi `TrackDependency`için , ' yi kullanarak gönderin.

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

Alternatif olarak, `TelemetryClient` uzatma `StartOperation` `StopOperation` yöntemleri sağlar ve [burada](custom-operations-tracking.md#outgoing-dependencies-tracking) gösterildiği gibi bağımlılıkları el ile izlemek için kullanılabilir

Standart bağımlılık izleme modüllerini kapatmak istiyorsanız, [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) for ASP.NET uygulamalardaki DependencyTrackingTelemetryModule başvurusunu kaldırın. ASP.NET Core uygulamaları [için, buradaki](asp-net-core.md#configuring-or-removing-default-telemetrymodules)talimatları takip edin.

## <a name="tracking-ajax-calls-from-web-pages"></a>AJAX aramalarını Web Sayfalarından izleme

Web sayfaları için, Application Insights JavaScript SDK, AJAX çağrılarını bağımlılık olarak otomatik olarak toplar.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Tam SQL Sorgusu almak için gelişmiş SQL izleme

SQL aramaları için, sunucu ve veritabanının adı her zaman toplanır ve `DependencyTelemetry`toplanan adı olarak saklanır. Tam SQL sorgu metnini içerebilen 'veri' adı verilen ek bir alan vardır.

ASP.NET Core uygulamaları için, tam SQL Sorgusu almak için ek bir adım gerekmez.

ASP.NET uygulamalar için, tam SQL sorgusu enstrümantasyon motoru gerektiren bayt kodu enstrümantasyonu yardımıyla toplanır. Aşağıda açıklandığı gibi platforma özgü ek adımlar gereklidir.

| Platform | Adım(lar) Tam SQL Sorgusu almak için gerekli |
| --- | --- |
| Azure Web App |Web uygulama kontrol [panelinizde, Application Insights bıçaklarını açın](../../azure-monitor/app/azure-web-apps.md) ve .NET altında SQL Komutları etkinleştirin |
| IIS Server (Azure VM, on-prem vb.) | [Instrumentation Engine'i yüklemek](../../azure-monitor/app/status-monitor-v2-api-reference.md) ve IIS'yi yeniden başlatmak için Durum Monitörü PowerShell Modülünü kullanın. |
| Azure Cloud Service | [StatusMonitor'u yüklemek için başlangıç görevi ekleme](../../azure-monitor/app/cloudservices.md#set-up-status-monitor-to-collect-full-sql-queries-optional) <br> [Uygulamanız, ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) veya ASP.NET Core uygulamaları için NuGet [paketlerini](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) yükleyerek uygulamanız ApplicationInsights SDK'ya dahil edilmelidir. |
| IIS Ekspresi | Desteklenmiyor

Yukarıdaki durumlarda, enstrümantasyon motorunun doğru şekilde yüklenmesini doğrulamanın doğru yolu, toplanan `DependencyTelemetry` SDK sürümünün 'rddp' olduğunu doğrulamaktır. 'rdddsd' veya 'rddf' bağımlılıkların DiagnosticSource veya EventSource geri aramaları aracılığıyla toplandığını ve dolayısıyla tam SQL sorgusunun yakalanmayacağını gösterir.

## <a name="where-to-find-dependency-data"></a>Bağımlılık verileri nerede bulabilirim

* [Uygulama Haritası,](app-map.md) uygulamanızla komşu bileşenler arasındaki bağımlılıkları görselleştirir.
* [İşlem Tanılama](transaction-diagnostics.md) birleşik, ilişkili sunucu verilerini gösterir.
* [Tarayıcılar sekmesi,](javascript.md) kullanıcılarınızın tarayıcılarından gelen AJAX çağrılarını gösterir.
* Bağımlılık çağrılarını denetlemek için yavaş veya başarısız isteklerden gelenleri tıklatın.
* [Analitik](#logs-analytics) bağımlılık verilerini sorgulamak için kullanılabilir.

## <a name="diagnose-slow-requests"></a><a name="diagnosis"></a>Yavaş istekleri tanılama

Her istek olayı, uygulamanız isteği işlerken izlenen bağımlılık çağrıları, özel durumlar ve diğer olaylarla ilişkilidir. Bu nedenle, bazı istekler kötü gidiyorsa, bunun bir bağımlılıktan gelen yavaş yanıtlardan kaynaklanıp kaynaklanmadığını öğrenebilirsiniz.

### <a name="tracing-from-requests-to-dependencies"></a>İsteklerden bağımlılıklara izleme

**Performans** sekmesini açın ve işlemlerin yanındaki üstteki **Bağımlılıklar** sekmesine gidin.

Genel olarak bağımlılık **adını** tıklatın. Bir bağımlılık seçtikten sonra, bu bağımlılığın süre dağılımının grafiği sağda gösterilecek.

![Performans sekmesinde, grafikte bir Bağımlılık adı sonra üstteki Bağımlılık sekmesine tıklayın](./media/asp-net-dependencies/2-perf-dependencies.png)

Sağ alttaki mavi **Örnekler** düğmesine ve ardından uçuça işlem ayrıntılarını görmek için bir örnekte tıklayın.

![Uçuca işlem ayrıntılarını görmek için bir örneğe tıklayın](./media/asp-net-dependencies/3-end-to-end.png)

### <a name="profile-your-live-site"></a>Canlı sitenizin profilini

Zamanın nereye gittiğine dair bir fikrin yok mu? [Application Insights profilleyicisi](../../azure-monitor/app/profiler.md) HTTP çağrılarını canlı sitenize izler ve kodunuzda en uzun süren işlevleri gösterir.

## <a name="failed-requests"></a>Başarısız istekler

Başarısız istekler, bağımlılıklara yapılan başarısız çağrılarla da ilişkilendirilebilir.

Soldaki **Hatalar** sekmesine gidebilir ve üstteki **bağımlılıklar** sekmesine tıklayabiliriz.

![Başarısız istekler grafiğini tıklatın](./media/asp-net-dependencies/4-fail.png)

Burada başarısız bağımlılık sayısını görebilirsiniz. Alt tablodaki bağımlılık adını tıklatmaya çalışırken başarısız bir oluşum hakkında daha fazla bilgi almak için. Uçuça işlem ayrıntılarını almak için sağ alttaki mavi **Bağımlılıklar** düğmesine tıklayabilirsiniz.

## <a name="logs-analytics"></a>Günlükler (Analytics)

[Kusto sorgu dilindeki](/azure/kusto/query/)bağımlılıkları izleyebilirsiniz. Bazı örnekler aşağıda verilmiştir.

* Başarısız bağımlılık çağrıları bulma:

``` Kusto

    dependencies | where success != "True" | take 10
```

* AJAX aramalarını bulun:

``` Kusto

    dependencies | where client_Type == "Browser" | take 10
```

* İsteklerle ilişkili bağımlılık çağrılarını bulma:

``` Kusto

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Sayfa görünümleriyle ilişkili AJAX çağrılarını bulun:

``` Kusto 

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*Otomatik bağımlılık toplayıcısı bağımlılıkçağrıları nasıl başarısız olur?*

* Başarısız bağımlılık çağrıları False olarak ayarlanmış 'başarı' alanı olacaktır. `DependencyTrackingTelemetryModule`bildirmez. `ExceptionTelemetry` Bağımlılık için tam veri modeli [burada](data-model-dependency-telemetry.md)açıklanmıştır.

## <a name="open-source-sdk"></a>Açık kaynak SDK
Her Application Insights SDK gibi bağımlılık toplama modülü de açık kaynak kodludur. Kodu okuyun ve koda katkıda bulunun veya [resmi GitHub repo'sundaki sorunları bildirin.](https://github.com/Microsoft/ApplicationInsights-dotnet-server)

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Durumlar](../../azure-monitor/app/asp-net-exceptions.md)
* [Kullanıcı & sayfa verileri](../../azure-monitor/app/javascript.md)
* [Kullanılabilirlik](../../azure-monitor/app/monitor-web-app-availability.md)
