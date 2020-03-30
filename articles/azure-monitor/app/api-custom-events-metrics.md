---
title: Özel olaylar ve ölçümler için Uygulama Öngörüleri API ' si | Microsoft Dokümanlar
description: Kullanımı izlemek ve sorunları tanılamak için cihazınıza veya masaüstü uygulamanıza, web sayfanıza veya hizmetinize birkaç satır kod ekleyin.
ms.topic: conceptual
ms.date: 03/27/2019
ms.openlocfilehash: 4275d3ea3a340f0a4083ab929eb7f7872f3311e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295024"
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Özel olaylar ve ölçümler için Application Insights API

Kullanıcıların uygulamayla ne yaptığını öğrenmek veya sorunları tanılamaya yardımcı olmak için uygulamanıza birkaç satır kod ekleyin. Cihaz ve masaüstü uygulamalarından, web istemcilerinden ve web sunucularından telemetri gönderebilirsiniz. Özel olaylar ve ölçümler ve standart telemetrinin kendi sürümlerini göndermek için [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) temel telemetri API'sini kullanın. Bu API, standart Application Insights veri toplayıcılarının kullandığı API ile aynıdır.

## <a name="api-summary"></a>API özeti

Çekirdek API gibi birkaç varyasyondışında, tüm platformlarda `GetMetric`tek düze (.NET sadece).

| Yöntem | Kullanıldığı yerler |
| --- | --- |
| [`TrackPageView`](#page-views) |Sayfalar, ekranlar, bıçaklar veya formlar. |
| [`TrackEvent`](#trackevent) |Kullanıcı eylemleri ve diğer olaylar. Kullanıcı davranışını izlemek veya performansı izlemek için kullanılır. |
| [`GetMetric`](#getmetric) |Sıfır ve çok boyutlu ölçümler, merkezi olarak yapılandırılmış toplama, yalnızca C# |
| [`TrackMetric`](#trackmetric) |Belirli olaylarla ilgili olmayan sıra uzunlukları gibi performans ölçümleri. |
| [`TrackException`](#trackexception) |Tanı için günlük özel durumları. Diğer olaylarla ilişkili olarak nerede oluştuklarını izleyin ve yığın izlerini inceleyin. |
| [`TrackRequest`](#trackrequest) |Performans analizi için sunucu isteklerinin sıklığını ve süresini günlüğe kaydetme. |
| [`TrackTrace`](#tracktrace) |Kaynak Tanılama günlük iletileri. Üçüncü taraf günlüklerini de yakalayabilirsiniz. |
| [`TrackDependency`](#trackdependency) |Uygulamanızın bağlı olduğu harici bileşenlere yapılan aramaların süresini ve sıklığını günlüğe kaydetme. |

Bu telemetri çağrılarının çoğuna [özellikler ve ölçümler ekleyebilirsiniz.](#properties)

## <a name="before-you-start"></a><a name="prep"></a>Başlamadan önce

Uygulama Öngörüleri SDK hakkında henüz bir referansınyoksa:

* Uygulama Öngörüleri SDK'yı projenize ekleyin:

  * [ASP.NET projesi](../../azure-monitor/app/asp-net.md)
  * [ASP.NET Çekirdek projesi](../../azure-monitor/app/asp-net-core.md)
  * [Java projesi](../../azure-monitor/app/java-get-started.md)
  * [Düğüm.js projesi](../../azure-monitor/app/nodejs.md)
  * [Her web sayfasında JavaScript](../../azure-monitor/app/javascript.md) 
* Cihazınızda veya web sunucu kodunuza şunları ekleyin:

    *C#:*`using Microsoft.ApplicationInsights;`

    *Görsel Temel:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`

    *Düğüm.js:*`var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>TelemetryClient örneği alın

Bir örneğini `TelemetryClient` alın (web sayfalarındajavaScript hariç):

ASP.NET [Core](asp-net-core.md#how-can-i-track-telemetry-thats-not-automatically-collected) uygulamaları ve [.NET/.NET Core uygulamaları için NON HTTP/Worker](worker-service.md#how-can-i-track-telemetry-thats-not-automatically-collected) `TelemetryClient` için, bağımlılık enjeksiyon kabının bir örneğini kendi belgelerinde açıklandığı şekilde almaları önerilir.

AzureFunctions v2+ veya Azure WebJobs v3+ kullanıyorsanız, aşağıdaki belgeyi izleyin:https://docs.microsoft.com/azure/azure-functions/functions-monitoring#version-2x-3

*C #*

```csharp
private TelemetryClient telemetry = new TelemetryClient();
```
Bu yöntemi gören herkes için eski mesajlar daha fazla bilgi için [lütfen microsoft/ApplicationInsights-dotnet#1152](https://github.com/microsoft/ApplicationInsights-dotnet/issues/1152) adresini ziyaret edin.

*Visual Basic*

```vb
Private Dim telemetry As New TelemetryClient
```

*Java*

```java
private TelemetryClient telemetry = new TelemetryClient();
``` 

*Node.js*

```javascript
var telemetry = applicationInsights.defaultClient;
```

TelemetryClient iş parçacığı için güvenlidir.

ASP.NET ve Java projeleri için gelen HTTP İstekleri otomatik olarak yakalanır. Uygulamanızın diğer modülü için TelemetryClient'un ek örneklerini oluşturmak isteyebilirsiniz. Örneğin, iş mantığı olaylarını bildirmek için ara yazılım sınıfınızda bir TelemetryClient örneğiniz olabilir. Makineyi tanımlamak için UserId ve DeviceId gibi özellikleri ayarlayabilirsiniz. Bu bilgiler, örneğin gönderdiği tüm olaylara eklenir.

*C #*

```csharp
TelemetryClient.Context.User.Id = "...";
TelemetryClient.Context.Device.Id = "...";
```

*Java*

```java
telemetry.getContext().getUser().setId("...");
telemetry.getContext().getDevice().setId("...");
```

Düğüm.js projelerinde, yeni `new applicationInsights.TelemetryClient(instrumentationKey?)` bir örnek oluşturmak için kullanabilirsiniz, ancak bu yalnızca singleton'dan `defaultClient`yalıtılmış yapılandırma gerektiren senaryolar için önerilir.

## <a name="trackevent"></a>TrackOlay

Uygulama Öngörüleri'nde özel bir *olay,* [Metrikler](../../azure-monitor/app/metrics-explorer.md) Gezgini'nde toplu sayım olarak ve [Tanılama Arama'da](../../azure-monitor/app/diagnostic-search.md) tek tek oluşumlar olarak görüntüleyebileceğiniz bir veri noktasıdır. (Bu MVC veya diğer çerçeve "olaylar ile ilgili değildir.")

Çeşitli `TrackEvent` olayları saymak için kodunuzda çağrılar ekleyin. Kullanıcıların belirli bir özelliği ne sıklıkta seçtikleri, belirli hedeflere ne sıklıkta ulaştıkları veya belirli hata türlerini ne sıklıkta yaptıkları.

Örneğin, bir oyun uygulamasında, bir kullanıcı oyunu kazandığında bir etkinlik gönderin:

*Javascript*

```javascript
appInsights.trackEvent({name:"WinGame"});
```

*C #*

```csharp
telemetry.TrackEvent("WinGame");
```

*Visual Basic*

```vb
telemetry.TrackEvent("WinGame")
```

*Java*

```java
telemetry.trackEvent("WinGame");
```

*Node.js*

```javascript
telemetry.trackEvent({name: "WinGame"});
```

### <a name="custom-events-in-analytics"></a>Analytics'te özel etkinlikler

Telemetri, Application `customEvents` [Insights Analytics'teki](analytics.md)tabloda mevcuttur. Her satır, uygulamanızda yapılan `trackEvent(..)` bir aramayı temsil eder.

[Örnekleme](../../azure-monitor/app/sampling.md) işlatılırsa, itemCount özelliği 1'den büyük bir değer gösterir. Örneğin itemCount==10, trackEvent() için yapılan 10 çağrıdan yalnızca birini iletildiği örnekleme işlemi anlamına gelir. Özel olayların doğru bir sayısını almak için, bu `customEvents | summarize sum(itemCount)`nedenle .

## <a name="getmetric"></a>GetMetric

### <a name="examples"></a>Örnekler

*C #*

```csharp
namespace User.Namespace.Example01
{
    using System;
    using Microsoft.ApplicationInsights;
    using Microsoft.ApplicationInsights.DataContracts;

    /// <summary>
    /// Most simple cases are one-liners.
    /// This is all possible without even importing an additional namespace.
    /// </summary>

    public class Sample01
    {
        /// <summary />
        public static void Exec()
        {
            // *** SENDING METRICS ***

            // Recall how you send custom telemetry with Application Insights in other cases, e.g. Events.
            // The following will result in an EventTelemetry object to be sent to the cloud right away.
            TelemetryClient client = new TelemetryClient();
            client.TrackEvent("SomethingInterestingHappened");

            // Metrics work very similar. However, the value is not sent right away.
            // It is aggregated with other values for the same metric, and the resulting summary (aka "aggregate" is sent automatically every minute.
            // To mark this difference, we use a pattern that is similar, but different from the established TrackXxx(..) pattern that sends telemetry right away:

            client.GetMetric("CowsSold").TrackValue(42);

            // *** MULTI-DIMENSIONAL METRICS ***

            // The above example shows a zero-dimensional metric.
            // Metrics can also be multi-dimensional.
            // In the initial version we are supporting up to 2 dimensions, and we will add support for more in the future as needed.
            // Here is an example for a one-dimensional metric:

            Metric animalsSold = client.GetMetric("AnimalsSold", "Species");

            animalsSold.TrackValue(42, "Pigs");
            animalsSold.TrackValue(24, "Horses");

            // The values for Pigs and Horses will be aggregated separately from each other and will result in two distinct aggregates.
            // You can control the maximum number of number data series per metric (and thus your resource usage and cost).
            // The default limits are no more than 1000 total data series per metric, and no more than 100 different values per dimension.
            // We discuss elsewhere how to change them.
            // We use a common .NET pattern: TryXxx(..) to make sure that the limits are observed.
            // If the limits are already reached, Metric.TrackValue(..) will return False and the value will not be tracked. Otherwise it will return True.
            // This is particularly useful if the data for a metric originates from user input, e.g. a file:

            Tuple<int, string> countAndSpecies = ReadSpeciesFromUserInput();
            int count = countAndSpecies.Item1;
            string species = countAndSpecies.Item2;

            if (!animalsSold.TrackValue(count, species))

            {
                client.TrackTrace($"Data series or dimension cap was reached for metric {animalsSold.Identifier.MetricId}.", SeverityLevel.Error);
            }

            // You can inspect a metric object to reason about its current state. For example:
            int currentNumberOfSpecies = animalsSold.GetDimensionValues(1).Count;
        }

        private static void ResetDataStructure()
        {
            // Do stuff
        }

        private static Tuple<int, string> ReadSpeciesFromUserInput()
        {
            return Tuple.Create(18, "Cows");
        }

        private static int AddItemsToDataStructure()
        {
            // Do stuff
            return 5;
        }
    }
}
```

## <a name="trackmetric"></a>TrackMetric

> [!NOTE]
> Microsoft.ApplicationInsights.TelemetryClient.TrackMetric ölçümleri göndermek için tercih edilen yöntem değildir. Ölçümler gönderilmeden önce her zaman bir zaman dilimi boyunca önceden toplanmalıdır. SDK ön toplama özelliklerine erişmek için metrik bir nesne almak için GetMetric(..) aşırı yüklerinden birini kullanın. Kendi ön toplama mantığınızı uyguluyorsanız, elde edilen toplamları göndermek için TrackMetric() yöntemini kullanabilirsiniz. Uygulamanız zaman içinde toplama olmadan her fırsatta ayrı bir telemetri öğesi göndermeyi gerektiriyorsa, büyük olasılıkla olay telemetrisi için bir kullanım örneğiniz vardır; bkz. TelemetryClient.TrackEvent (Microsoft.ApplicationInsights.DataContracts.EventTelemetry).

Uygulama Öngörüleri, belirli olaylara iliştirilmeyen ölçümleri grafikleyebilir. Örneğin, sıra uzunluğunu düzenli aralıklarla izleyebilirsiniz. Ölçümlerle, tek tek ölçümler varyasyonlardan ve eğilimlerden daha az ilgi çekicidir ve bu nedenle istatistiksel grafikler yararlıdır.

Uygulama Öngörüleri'ne ölçümler göndermek için API'yi `TrackMetric(..)` kullanabilirsiniz. Bir metrik göndermenin iki yolu vardır:

* Tek değer. Uygulamanızda her ölçüm yaptığınızda, ilgili değeri Application Insights'a gönderirsiniz. Örneğin, bir kapsayıcıdaki madde sayısını açıklayan bir metriğinolduğunu varsay. Belirli bir zaman dilimi içinde, önce kapsayıcıya üç öğe koyarsınız ve sonra iki öğeyi kaldırırsınız. Buna göre, iki `TrackMetric` kez aramak istiyorsunuz: ilk `3` `-2`değeri geçen ve daha sonra değer . Application Insights her iki değeri de sizin adınıza saklar.

* Toplama. Ölçümlerle çalışırken, her bir ölçüm nadiren ilgi çekicidir. Bunun yerine belirli bir zaman diliminde ne oldu bir özeti önemlidir. Böyle bir _özete toplama_denir. Yukarıdaki örnekte, o dönem için toplam `1` metrik toplamı ve metrik `2`değerlerin sayısı . Toplama yaklaşımını kullanırken, zaman diliminde `TrackMetric` yalnızca bir kez çağırır ve toplam değerleri gönderirsiniz. Bu, uygulama istatistiklerine daha az veri noktası göndererek ve ilgili tüm bilgileri toplamaya devam ederken maliyet ve performans ek yüküönemli ölçüde azaltabileceğinden önerilen yaklaşımdır.

### <a name="examples"></a>Örnekler

#### <a name="single-values"></a>Tek değerler

Tek bir metrik değer göndermek için:

*Javascript*

 ```javascript
appInsights.trackMetric("queueLength", 42.0);
 ```

*C #*

```csharp
var sample = new MetricTelemetry();
sample.Name = "metric name";
sample.Value = 42.3;
telemetryClient.TrackMetric(sample);
```

*Java*

```java
telemetry.trackMetric("queueLength", 42.0);
```

*Node.js*

 ```javascript
telemetry.trackMetric({name: "queueLength", value: 42.0});
 ```

### <a name="custom-metrics-in-analytics"></a>Analytics'te özel ölçümler

Telemetri, Application `customMetrics` [Insights Analytics'teki](analytics.md)tabloda mevcuttur. Her satır, uygulamanızda yapılan `trackMetric(..)` bir aramayı temsil eder.

* `valueSum`- Bu ölçümlerin toplamıdır. Ortalama değeri elde etmek `valueCount`için, bölün.
* `valueCount`- Bu `trackMetric(..)` çağrıya toplanan ölçümlerin sayısı.

## <a name="page-views"></a>Sayfa görünümleri

Bir aygıt veya web sayfası uygulamasında, her ekran veya sayfa yüklendiğinde varsayılan olarak sayfa görünümü telemetrisi gönderilir. Ancak bunu, ek veya farklı zamanlarda sayfa görünümlerini izlemek için değiştirebilirsiniz. Örneğin, sekmeleri veya bıçakları görüntüleyen bir uygulamada, kullanıcı yeni bir bıçak açtığında bir sayfayı izlemek isteyebilirsiniz.

Kullanıcı ve oturum verileri sayfa görünümleriyle birlikte özellik olarak gönderilir, böylece sayfa görünümü telemetrisi olduğunda kullanıcı ve oturum grafikleri canlanır.

### <a name="custom-page-views"></a>Özel sayfa görünümleri

*Javascript*

```javascript
appInsights.trackPageView("tab1");
```

*C #*

```csharp
telemetry.TrackPageView("GameReviewPage");
```

*Visual Basic*

```vb
telemetry.TrackPageView("GameReviewPage")
```

*Java*

```java
telemetry.trackPageView("GameReviewPage");
```

Farklı HTML sayfalarında birden fazla sekmeniz varsa, URL'yi de belirtebilirsiniz:

```javascript
appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");
```

### <a name="timing-page-views"></a>Sayfa görünümlerini zamanlama

Varsayılan olarak, Sayfa **görünümü yükleme süresi** olarak bildirilen süreler, tarayıcının sayfa yükleme olayı çağrılana kadar tarayıcının isteği gönderdiği zamandan itibaren ölçülür.

Bunun yerine, aşağıdakileri yapabilirsiniz:

* [TrackPageView](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/legacy/API.md#trackpageview) çağrısında açık bir süre `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`ayarlayın: .
* Sayfa görünümü zamanlama `startTrackPage` çağrıları `stopTrackPage`ve .

*Javascript*

```javascript
// To start timing a page:
appInsights.startTrackPage("Page1");

...

// To stop timing and log the page:
appInsights.stopTrackPage("Page1", url, properties, measurements);
```

İlk parametre olarak kullandığınız ad, çağrıları başlat ve durdurmayı ilişkilendirer. Geçerli sayfa adı varsayılan olarak.

Metrikler Gezgini'nde görüntülenen ortaya çıkan sayfa yükleme süreleri, başlatma ve durdurma çağrıları arasındaki aralıktan türetilir. Hangi aralıkta zaman alabildiğiniz size kalmış.

### <a name="page-telemetry-in-analytics"></a>Analitikte Sayfa telemetrisi

[Analytics'te](analytics.md) iki tablo tarayıcı işlemlerinden elde edilen verileri gösterir:

* Tablo, `pageViews` URL ve sayfa başlığı hakkında veri içerir
* Tablo, `browserTimings` gelen verileri işlemek için geçen süre gibi istemci performansı yla ilgili verileri içerir

Tarayıcının farklı sayfaları işlemesi ne kadar sürer bulmak için:

```kusto
browserTimings
| summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name
```

Farklı tarayıcıların popülerliklerini keşfetmek için:

```kusto
pageViews
| summarize count() by client_Browser
```

Sayfa görünümlerini AJAX çağrılarıyla ilişkilendirmek için bağımlılıklarla birleşin:

```kusto
pageViews
| join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest

Sunucu SDK, HTTP isteklerini günlüğe kaydetmek için TrackRequest'i kullanır.

Web hizmeti modülünün çalışmadığı bir bağlamda istekleri simüle etmek istiyorsanız, bunu kendiniz de arayabilirsiniz.

Ancak, istek telemetrigöndermek için önerilen yolu nerede istek bir <a href="#operation-context">işlem bağlamı</a>olarak hareket eder.

## <a name="operation-context"></a>Çalışma bağlamı

Telemetri öğelerini işlem bağlamıyla ilişkilendirerek ilişkilendirebilirsiniz. Standart istek izleme modülü, bir HTTP isteği işlenirken gönderilen özel durumlar ve diğer olaylar için bunu yapar. [Arama](../../azure-monitor/app/diagnostic-search.md) ve [Analiz'de,](analytics.md)işlem kimliğini kullanarak istekle ilişkili olayları kolayca bulabilirsiniz.

Korelasyon hakkında daha fazla bilgi için [Uygulama Öngörüleri'ndeki Telemetri korelasyonuna](../../azure-monitor/app/correlation.md) bakın.

Telemetriyi el ile takip ederken, bu deseni kullanarak telemetri korelasyonundan emin olmanın en kolay yolu:

*C #*

```csharp
// Establish an operation context and associated telemetry item:
using (var operation = telemetryClient.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use the same operation ID.
    ...
    telemetryClient.TrackTrace(...); // or other Track* calls
    ...

    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetryClient.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Bir işlem bağlamı `StartOperation` ayarlama ile birlikte, belirttiğiniz tür bir telemetri öğesi oluşturur. İşlemi elden çıkarırken veya açıkça ararsanız `StopOperation`telemetri öğesini gönderir. Telemetri `RequestTelemetry` türünü kullanırsanız, süresi başlangıç ve durdurma arasındaki zaman aralığına ayarlanır.

Operasyon kapsamında bildirilen telemetri öğeleri bu operasyonun 'çocukları' haline gelir. Çalışma bağlamları iç içe olabilir.

Arama'da, işlem bağlamı **İlgili Öğeler** listesini oluşturmak için kullanılır:

![İlgili öğeler](./media/api-custom-events-metrics/21.png)

Özel işlemler izleme hakkında daha fazla bilgi için [Uygulama Öngörüleri .NET SDK ile özel işlemleri izleyin.](../../azure-monitor/app/custom-operations-tracking.md)

### <a name="requests-in-analytics"></a>Analitikte İstekler

[Application Insights Analytics'te istekler](analytics.md)tabloda yer eder. `requests`

[Örnekleme](../../azure-monitor/app/sampling.md) işlatılırsa, itemCount özelliği 1'den büyük bir değer gösterir. Örneğin itemCount==10, trackRequest() için yapılan 10 çağrıdan yalnızca birini iletildiği örnekleme işlemi anlamına gelir. İsteklerin doğru sayısını ve istek adlarına göre bölümlere alınan ortalama süreyi elde etmek için aşağıdakiler gibi bir kod kullanın:

```kusto
requests
| summarize count = sum(itemCount), avgduration = avg(duration) by name
```

## <a name="trackexception"></a>TrackException

Uygulama Öngörüleri'ne özel durumlar gönderin:

* [Onları saymak](../../azure-monitor/app/metrics-explorer.md)için, bir sorunun sıklığının bir göstergesi olarak.
* [Tek tek oluşumları incelemek](../../azure-monitor/app/diagnostic-search.md)için.

Raporlar yığın izlerini içerir.

*C #*

```csharp
try
{
    ...
}
catch (Exception ex)
{
    telemetry.TrackException(ex);
}
```

*Java*

```java
try {
    ...
} catch (Exception ex) {
    telemetry.trackException(ex);
}
```

*Javascript*

```javascript
try
{
    ...
}
catch (ex)
{
    appInsights.trackException(ex);
}
```

*Node.js*

```javascript
try
{
    ...
}
catch (ex)
{
    telemetry.trackException({exception: ex});
}
```

SDK'lar birçok özel durumu otomatik olarak yakalar, böylece TrackException'ı her zaman açıkça aramanız gerekmez.

* ASP.NET: [Özel durumları yakalamak için kod yazın.](../../azure-monitor/app/asp-net-exceptions.md)
* Java EE: [İstisnalar otomatik olarak yakalanır.](../../azure-monitor/app/java-get-started.md#exceptions-and-request-failures)
* JavaScript: Özel durumlar otomatik olarak yakalanır. Otomatik koleksiyonu devre dışı atmak istiyorsanız, web sayfalarınıza eklediğiniz kod snippet'ine bir satır ekleyin:

```javascript
({
    instrumentationKey: "your key",
    disableExceptionTracking: true
})
```

### <a name="exceptions-in-analytics"></a>Analitikte İstisnalar

[Application Insights Analytics'te](analytics.md)özel durumlar `exceptions` tabloda yer eder.

[Örnekleme](../../azure-monitor/app/sampling.md) işlediyorsa, `itemCount` özellik 1'den büyük bir değer gösterir. Örneğin itemCount==10, izleme özel durum() için yapılan 10 çağrıdan yalnızca birini iletildiği anlamına gelir. Özel durum türüne göre bölümlenen özel durumların doğru bir sayısını almak için aşağıdakiler gibi kodu kullanın:

```kusto
exceptions
| summarize sum(itemCount) by type
```

Önemli yığın bilgilerinin çoğu zaten ayrı değişkenlere ayıklanır, ancak `details` daha fazla almak için yapıyı ayırabilirsiniz. Bu yapı dinamik olduğundan, sonucu beklediğiniz türe aktarmalısınız. Örnek:

```kusto
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

Özel durumları ilgili istekleriyle ilişkilendirmek için bir birleştirme kullanın:

```kusto
exceptions
| join (requests) on operation_Id
```

## <a name="tracktrace"></a>TrackTrace

Application Insights'a bir "kırıntı izi" göndererek sorunları tanılamaya yardımcı olmak için TrackTrace'i kullanın. Tanılama verisi yığınlarını gönderebilir ve [Bunları Tanıarama Arama'da](../../azure-monitor/app/diagnostic-search.md)inceleyebilirsiniz.

.NET [Log bağdaştırıcıları](../../azure-monitor/app/asp-net-trace-logs.md) portala üçüncü taraf günlükleri göndermek için bu API'yi kullanır.

[Log4J gibi Standart loggers](../../azure-monitor/app/java-trace-logs.md) için Java'da, Portala üçüncü taraf günlükleri göndermek için Uygulama Öngörüleri Log4j veya Logback Appenders kullanın.

*C #*

```csharp
telemetry.TrackTrace(message, SeverityLevel.Warning, properties);
```

*Java*

```java
telemetry.trackTrace(message, SeverityLevel.Warning, properties);
```

*Node.js*

```javascript
telemetry.trackTrace({
    message: message,
    severity: applicationInsights.Contracts.SeverityLevel.Warning,
    properties: properties
});
```

*İstemci/Tarayıcı tarafı JavaScript*

```javascript
trackTrace(message: string, properties?: {[string]:string}, severityLevel?: SeverityLevel)
```

Yöntem ekime girme veya bırakma gibi tanılama olayını günlüğe kaydedin.

 Parametre | Açıklama
---|---
`message` | Tanısal veriler. Bir isimden çok daha uzun olabilir.
`properties` | Dizeden dize haritası: Portaldaki [özel durumları filtrelemek](https://azure.microsoft.com/documentation/articles/app-insights-api-custom-events-metrics/#properties) için kullanılan ek veriler. Varsayılan olarak boşalır.
`severityLevel` | Desteklenen değerler: [Önem DerecesiLevel.ts](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/shared/AppInsightsCommon/src/Interfaces/Contracts/Generated/SeverityLevel.ts)

İleti içeriğinde arama yapabilirsiniz, ancak (özellik değerlerinden farklı olarak) iletiye filtre uygulayamazsınız.

Üzerindeki `message` boyut sınırı, özelliklerdeki sınırdan çok daha yüksektir.
TrackTrace'in bir avantajı, iletiye nispeten uzun veriler koyabiliyor olmasıdır. Örneğin, POST verilerini burada kodlayabilirsiniz.  

Ayrıca, iletinize önem düzeyi ekleyebilirsiniz. Ayrıca, diğer telemetriler gibi, farklı izleme kümelerini filtrelemenize veya aramanıza yardımcı olacak özellik değerleri ekleyebilirsiniz. Örnek:

*C #*

```csharp
var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
telemetry.TrackTrace("Slow database response",
                SeverityLevel.Warning,
                new Dictionary<string,string> { {"database", db.ID} });
```

*Java*

```java
Map<String, Integer> properties = new HashMap<>();
properties.put("Database", db.ID);
telemetry.trackTrace("Slow Database response", SeverityLevel.Warning, properties);
```

[Arama'da,](../../azure-monitor/app/diagnostic-search.md)belirli bir veritabanıyla ilgili belirli bir önem düzeyinin tüm iletilerini kolayca filtreleyebilirsiniz.

### <a name="traces-in-analytics"></a>Analitikte İzler

[Application Insights Analytics'te](analytics.md)TrackTrace çağrıları `traces` tabloda gösterilebilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlatılırsa, itemCount özelliği 1'den büyük bir değer gösterir. Örneğin `trackTrace()`itemCount==10, örnekleme işleminin yalnızca birini aktardığı 10 çağrıdan yalnızca birini ilettiği anlamına gelir. Doğru izleme çağrıları nın sayısını elde etmek için, `traces | summarize sum(itemCount)`bu nedenle ' nin .

## <a name="trackdependency"></a>TrackBağımlılık

Harici bir kod parçasına yapılan çağrıların yanıt sürelerini ve başarı oranlarını izlemek için TrackDependency çağrısını kullanın. Sonuçlar portaldaki bağımlılık grafiklerinde görünür. Aşağıdaki kod parçacığının bağımlılık çağrısının yapıldığı her yere eklenmesi gerekir.

*C #*

```csharp
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
catch(Exception ex) 
{
    success = false;
    telemetry.TrackException(ex);
    throw new Exception("Operation went wrong", ex);
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("DependencyType", "myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

*Java*

```java
boolean success = false;
Instant startTime = Instant.now();
try {
    success = dependency.call();
}
finally {
    Instant endTime = Instant.now();
    Duration delta = Duration.between(startTime, endTime);
    RemoteDependencyTelemetry dependencyTelemetry = new RemoteDependencyTelemetry("My Dependency", "myCall", delta, success);
    RemoteDependencyTelemetry.setTimeStamp(startTime);
    RemoteDependencyTelemetry.trackDependency(dependencyTelemetry);
}
```

*Node.js*

```javascript
var success = false;
var startTime = new Date().getTime();
try
{
    success = dependency.Call();
}
finally
{
    var elapsed = new Date() - startTime;
    telemetry.trackDependency({
        dependencyTypeName: "myDependency",
        name: "myCall",
        duration: elapsed,
        success: success
    });
}
```

Sunucu SDK'larının belirli bağımlılık çağrılarını (örneğin veritabanları ve REST API'leri) otomatik olarak keşfeden ve izleyen bir [bağımlılık modülü](../../azure-monitor/app/asp-net-dependencies.md) içerdiğini unutmayın. Modülün çalışması için sunucunuza bir aracı yüklemeniz gerekir. 

Java'da, belirli bağımlılık çağrıları [Java Agent](../../azure-monitor/app/java-agent.md)kullanılarak otomatik olarak izlenebilir.

Otomatik izlemenin yakalayamadığı aramaları izlemek istiyorsanız veya aracıyı yüklemek istemiyorsanız bu aramayı kullanırsınız.

C#'daki standart bağımlılık izleme modülünü kapatmak için [ApplicationInsights.config'i](../../azure-monitor/app/configuration-with-applicationinsights-config.md) `DependencyCollector.DependencyTrackingTelemetryModule`düzenle ve başvuruyu sil. Java'da, standart bağımlılıkları otomatik olarak toplamak istemiyorsanız lütfen java aracısını yüklemeyin.

### <a name="dependencies-in-analytics"></a>Analitikte Bağımlılıklar

[Application Insights Analytics'te](analytics.md)trackDependency çağrıları `dependencies` tabloda gösterilebilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlatılırsa, itemCount özelliği 1'den büyük bir değer gösterir. Örneğin itemCount==10, trackDependency() için yapılan 10 çağrıdan yalnızca birini iletildiği anlamına gelir. Hedef bileşene göre bölümlere ayrılmıştır bağımlılıkların doğru bir sayısını almak için şu gibi kod kullanın:

```kusto
dependencies
| summarize sum(itemCount) by target
```

Bağımlılıkları ilgili istekleriyle ilişkilendirmek için bir birleştirme kullanın:

```kusto
dependencies
| join (requests) on operation_Id
```

## <a name="flushing-data"></a>Verileri yıkama

Normalde, SDK verileri sabit aralıklarla (genellikle 30 saniye) veya arabellek tam olduğunda (genellikle 500 öğe) gönderir. Ancak, bazı durumlarda arabelleği sifonu çekmek isteyebilirsiniz,örneğin, sdk'yı kapanan bir uygulamada kullanıyorsanız.

*C #*

 ```csharp
telemetry.Flush();
// Allow some time for flushing before shutdown.
System.Threading.Thread.Sleep(5000);
```

*Java*

```java
telemetry.flush();
//Allow some time for flushing before shutting down
Thread.sleep(5000);
```

*Node.js*

```javascript
telemetry.flush();
```

İşlev [sunucu telemetri kanalı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/)için asynchronous olduğunu.

İdeal olarak, Uygulamanın kapatma etkinliğinde flush() yöntemi kullanılmalıdır.

## <a name="authenticated-users"></a>Kimliği doğrulanmış kullanıcılar

Bir web uygulamasında, kullanıcılar çerezler tarafından (varsayılan olarak) tanımlanır. Bir kullanıcı, uygulamanıza farklı bir makineden veya tarayıcıdan erişiğinde veya çerezleri silerse birden fazla kez sayılabilir.

Kullanıcılar uygulamanızda oturum açtıysa, tarayıcı kodunda kimlik doğrulaması yapılan kullanıcı kimliğini ayarlayarak daha doğru bir sayım elde edebilirsiniz:

*Javascript*

```javascript
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

Bir ASP.NET web MVC uygulamasında, örneğin:

*Razor*

```cshtml
@if (Request.IsAuthenticated)
{
    <script>
        appInsights.setAuthenticatedUserContext("@User.Identity.Name
            .Replace("\\", "\\\\")"
            .replace(/[,;=| ]+/g, "_"));
    </script>
}
```

Kullanıcının gerçek oturum açma adını kullanmanız gerekmez. Yalnızca o kullanıcıya özgü bir kimlik olmalıdır. Boşluklar veya karakterlerden herhangi birini `,;=|`içermemelidir.

Kullanıcı kimliği de bir oturum çerezayarlanır ve sunucuya gönderilir. Sunucu SDK yüklüyse, kimlik doğrulaması kullanıcı kimliği hem istemci hem de sunucu telemetrisinin bağlam özelliklerinin bir parçası olarak gönderilir. Daha sonra filtre uygulayabilir ve üzerinde arama yapabilirsiniz.

Uygulamanız kullanıcıları hesaplara gruplatırsa, hesap için bir tanımlayıcı da geçirebilirsiniz (aynı karakter kısıtlamalarıyla).

```javascript
appInsights.setAuthenticatedUserContext(validatedId, accountId);
```

[Metrics](../../azure-monitor/app/metrics-explorer.md)Explorer'da, **Kullanıcıları, Kimlik Doğrulaması**ve **Kullanıcı hesaplarını**sayan bir grafik oluşturabilirsiniz.

Ayrıca, belirli kullanıcı adları ve hesapları olan istemci veri noktalarını da [arayabilirsiniz.](../../azure-monitor/app/diagnostic-search.md)

## <a name="filtering-searching-and-segmenting-your-data-by-using-properties"></a><a name="properties"></a>Özellikleri kullanarak verilerinizi filtreleme, arama ve segmente etme

Özellikleri ve ölçümleri etkinliklerinize (ve ölçümlere, sayfa görünümlerine, özel durumlara ve diğer telemetri verilerine) ekleyebilirsiniz.

*Özellikler,* kullanım raporlarında telemetrinizi filtrelemek için kullanabileceğiniz dize değerleridir. Örneğin, uygulamanız birden fazla oyun sağlıyorsa, hangi oyunların daha popüler olduğunu görebilmeniz için her etkinliğe oyunun adını ekleyebilirsiniz.

Dize uzunluğunda 8192'lik bir sınır var. (Büyük veri yığınları göndermek istiyorsanız TrackTrace ileti parametresini kullanın.)

*Ölçümler,* grafik olarak sunulabilen sayısal değerlerdir. Örneğin, oyuncularınızın elde ettiği puanlarda kademeli bir artış olup olmadığını görmek isteyebilirsiniz. Grafikler, farklı oyunlar için ayrı veya yığılmış grafikler alabilmeniz için olayla birlikte gönderilen özelliklerle bölümlere ayrılabilir.

Metrik değerlerin doğru görüntülenemesi için, 0'dan büyük veya eşit olmalıdır.

Kullanabileceğiniz [özellik, özellik değerleri ve ölçümlersayısında](#limits) bazı sınırlar vardır.

*Javascript*

```javascript
appInsights.trackEvent
    ("WinGame",
        // String properties:
        {Game: currentGame.name, Difficulty: currentGame.difficulty},
        // Numeric metrics:
        {Score: currentGame.score, Opponents: currentGame.opponentCount}
        );

appInsights.trackPageView
    ("page name", "http://fabrikam.com/pageurl.html",
        // String properties:
        {Game: currentGame.name, Difficulty: currentGame.difficulty},
        // Numeric metrics:
        {Score: currentGame.score, Opponents: currentGame.opponentCount}
        );
```

*C #*

```csharp
// Set up some properties and metrics:
var properties = new Dictionary <string, string>
    {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
var metrics = new Dictionary <string, double>
    {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

// Send the event:
telemetry.TrackEvent("WinGame", properties, metrics);
```

*Node.js*

```javascript
// Set up some properties and metrics:
var properties = {"game": currentGame.Name, "difficulty": currentGame.Difficulty};
var metrics = {"Score": currentGame.Score, "Opponents": currentGame.OpponentCount};

// Send the event:
telemetry.trackEvent({name: "WinGame", properties: properties, measurements: metrics});
```

*Visual Basic*

```vb
' Set up some properties:
Dim properties = New Dictionary (Of String, String)
properties.Add("game", currentGame.Name)
properties.Add("difficulty", currentGame.Difficulty)

Dim metrics = New Dictionary (Of String, Double)
metrics.Add("Score", currentGame.Score)
metrics.Add("Opponents", currentGame.OpponentCount)

' Send the event:
telemetry.TrackEvent("WinGame", properties, metrics)
```

*Java*

```java
Map<String, String> properties = new HashMap<String, String>();
properties.put("game", currentGame.getName());
properties.put("difficulty", currentGame.getDifficulty());

Map<String, Double> metrics = new HashMap<String, Double>();
metrics.put("Score", currentGame.getScore());
metrics.put("Opponents", currentGame.getOpponentCount());

telemetry.trackEvent("WinGame", properties, metrics);
```

> [!NOTE]
> Özelliklerde kişisel olarak tanımlanabilir bilgileri kaydetmemeye özen.
>
>

### <a name="alternative-way-to-set-properties-and-metrics"></a>Özellikleri ve ölçümleri ayarlamak için alternatif yol

Daha uygunsa, bir olayın parametrelerini ayrı bir nesnede toplayabilirsiniz:

```csharp
var event = new EventTelemetry();

event.Name = "WinGame";
event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
event.Properties["game"] = currentGame.Name;
event.Properties["difficulty"] = currentGame.Difficulty;
event.Metrics["Score"] = currentGame.Score;
event.Metrics["Opponents"] = currentGame.Opponents.Length;

telemetry.TrackEvent(event);
```

> [!WARNING]
> Track*() adresini birden çok kez`event` aramak için aynı telemetri öğesi örneğini (bu örnekte) yeniden kullanmayın. Bu, telemetrinin yanlış yapılandırmayla gönderilmesine neden olabilir.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Analytics'te özel ölçümler ve özellikler

[Analytics'te,](analytics.md)özel ölçümler ve `customMeasurements` özellikler `customDimensions` her telemetri kaydının özniteliklerinde ve özniteliklerinde gösterir.

Örneğin, istek telemetrinize "oyun" adlı bir özellik eklediyseniz, bu sorgu "oyun"un farklı değerlerinin oluşumlarını sayar ve özel metrik "puan"ın ortalamasını gösterir:

```kusto
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game)
```

Şuna dikkat edin:

* ÖzelBoyutlar veya özel Ölçümler JSON bir değer ayıklamak, dinamik türü vardır `tostring` `todouble`ve bu yüzden döküm gerekir veya .
* [Örnekleme](../../azure-monitor/app/sampling.md)olasılığını dikkate almak için, `sum(itemCount)`kullanmalısınız, `count()`değil.

## <a name="timing-events"></a><a name="timed"></a>Zamanlama olayları

Bazen bir eylemi gerçekleştirmenin ne kadar sürdüğünü grafiklemek istersiniz. Örneğin, kullanıcıların bir oyunda seçimleri dikkate almaları ne kadar zaman alabilir diyebilirsiniz. Bunun için ölçüm parametresini kullanabilirsiniz.

*C #*

```csharp
var stopwatch = System.Diagnostics.Stopwatch.StartNew();

// ... perform the timed action ...

stopwatch.Stop();

var metrics = new Dictionary <string, double>
    {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

// Set up some properties:
var properties = new Dictionary <string, string>
    {{"signalSource", currentSignalSource.Name}};

// Send the event:
telemetry.TrackEvent("SignalProcessed", properties, metrics);
```

*Java*

```java
long startTime = System.currentTimeMillis();

// Perform timed action

long endTime = System.currentTimeMillis();
Map<String, Double> metrics = new HashMap<>();
metrics.put("ProcessingTime", (double)endTime-startTime);

// Setup some properties
Map<String, String> properties = new HashMap<>();
properties.put("signalSource", currentSignalSource.getName());

// Send the event
telemetry.trackEvent("SignalProcessed", properties, metrics);
```

## <a name="default-properties-for-custom-telemetry"></a><a name="defaults"></a>Özel telemetri için varsayılan özellikler

Yazdığınız bazı özel olaylar için varsayılan özellik değerlerini ayarlamak istiyorsanız, bunları Bir TelemetryClient örneğinde ayarlayabilirsiniz. Bu istemciden gönderilen her telemetri öğesine eklenirler.

*C #*

```csharp
using Microsoft.ApplicationInsights.DataContracts;

var gameTelemetry = new TelemetryClient();
gameTelemetry.Context.GlobalProperties["Game"] = currentGame.Name;
// Now all telemetry will automatically be sent with the context property:
gameTelemetry.TrackEvent("WinGame");
```

*Visual Basic*

```vb
Dim gameTelemetry = New TelemetryClient()
gameTelemetry.Context.GlobalProperties("Game") = currentGame.Name
' Now all telemetry will automatically be sent with the context property:
gameTelemetry.TrackEvent("WinGame")
```

*Java*

```java
import com.microsoft.applicationinsights.TelemetryClient;
import com.microsoft.applicationinsights.TelemetryContext;
...


TelemetryClient gameTelemetry = new TelemetryClient();
TelemetryContext context = gameTelemetry.getContext();
context.getProperties().put("Game", currentGame.Name);

gameTelemetry.TrackEvent("WinGame");
```

*Node.js*

```javascript
var gameTelemetry = new applicationInsights.TelemetryClient();
gameTelemetry.commonProperties["Game"] = currentGame.Name;

gameTelemetry.TrackEvent({name: "WinGame"});
```

Tek tek telemetri çağrıları, özellik sözlüklerindeki varsayılan değerleri geçersiz kılabilir.

*JavaScript web istemcileri için,* JavaScript telemetri başlatma kullanarak.

Standart toplama modüllerinden [ `ITelemetryInitializer` ](../../azure-monitor/app/api-filtering-sampling.md#add-properties)alınan veriler de dahil olmak üzere *tüm telemetrilere özellikler eklemek için.*

## <a name="sampling-filtering-and-processing-telemetry"></a>Telemetriörnekleme, filtreleme ve işleme

SDK'dan gönderilmeden önce telemetrinizi işlemek için kod yazabilirsiniz. İşleme, HTTP istek toplama ve bağımlılık toplama gibi standart telemetri modüllerinden gönderilen verileri içerir.

Uygulayarak `ITelemetryInitializer`telemetriye [özellikler ekleyin.](../../azure-monitor/app/api-filtering-sampling.md#add-properties) Örneğin, sürüm numaraları veya diğer özelliklerden hesaplanan değerler ekleyebilirsiniz.

[Filtreleme,](../../azure-monitor/app/api-filtering-sampling.md#filtering) stometreyi sdk'dan gönderilmeden önce uygulayarak `ITelemetryProcessor`telemetriyi değiştirebilir veya atabilir. Gönderilenveya atılanları denetlersiniz, ancak ölçümlerinizin üzerindeki etkiyi hesaba katabilirsiniz. Öğeleri nasıl attığınızbağlı olarak, ilgili öğeler arasında gezinme yeteneğini kaybedebilirsiniz.

[Örnekleme,](../../azure-monitor/app/api-filtering-sampling.md) uygulamanızdan portala gönderilen veri hacmini azaltmak için paketlenmiş bir çözümdür. Görüntülenen ölçümleri etkilemeden bunu yapar. Ve bunu, özel durumlar, istekler ve sayfa görünümleri gibi ilgili öğeler arasında gezinerek sorunları tanılama yeteneğinizi etkilemeden yapar.

[Daha fazla bilgi edinin](../../azure-monitor/app/api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetrinin devre dışı bırakılması

*Dinamik olarak durdurmak ve* telemetri toplama ve iletim başlatmak için:

*C #*

```csharp
using  Microsoft.ApplicationInsights.Extensibility;

TelemetryConfiguration.Active.DisableTelemetry = true;
```

*Java*

```java
telemetry.getConfiguration().setTrackingDisabled(true);
```

*Seçili standart toplayıcıları*(örneğin, performans sayaçları, HTTP istekleri veya bağımlılıklar) devre dışı kalmak için [ApplicationInsights.config'deki](../../azure-monitor/app/configuration-with-applicationinsights-config.md)ilgili satırları silmek veya yorumla. Örneğin, kendi TrackRequest verilerinizi göndermek istiyorsanız bunu yapabilirsiniz.

*Node.js*

```javascript
telemetry.config.disableAppInsights = true;
```

İlkbaşlatma zamanında *seçili standart toplayıcıları*(örneğin performans sayaçları, HTTP istekleri veya bağımlılıklar) devre dışı kalımsağlamak için, SDK başlatma kodunuza zincir yapılandırma yöntemleri:

```javascript
applicationInsights.setup()
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    .setAutoCollectDependencies(false)
    .setAutoCollectConsole(false)
    .start();
```

Bu toplayıcıları başlatmadan sonra devre dışı kullanabilirsiniz, Yapılandırma nesnesini kullanın:`applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="developer-mode"></a><a name="debug"></a>Geliştirici modu

Hata ayıklama sırasında, sonuçları hemen görebilmeniz için telemetrinizin boru hattında hızlandırılmasını sağlamak yararlıdır. Ayrıca, telemetri ile ilgili sorunları izlemenize yardımcı olacak ek iletiler de alırsınız. Uygulamada kapatın, çünkü uygulamanızı yavaşlatabilir.

*C #*

```csharp
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;
```

*Visual Basic*

```vb
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True
```

*Node.js*

Node.js için, dahili günlüğe `setInternalLogging` kaydetmeyi ve `maxBatchSize` 0'a ayarlayarak geliştirici modunu etkinleştirebilirsiniz, bu da telemetrinizin toplanır toplanmaz gönderilmesine neden olur.

```js
applicationInsights.setup("ikey")
  .setInternalLogging(true, true)
  .start()
applicationInsights.defaultClient.config.maxBatchSize = 0;
```

## <a name="setting-the-instrumentation-key-for-selected-custom-telemetry"></a><a name="ikey"></a>Seçili özel telemetri için enstrümantasyon anahtarını ayarlama

*C #*

```csharp
var telemetry = new TelemetryClient();
telemetry.InstrumentationKey = "---my key---";
// ...
```

## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dinamik enstrümantasyon anahtarı

Geliştirme, test ve üretim ortamlarından telemetriyi karıştırmayı önlemek için [ayrı Application Insights kaynakları oluşturabilir](../../azure-monitor/app/create-new-resource.md ) ve ortama bağlı olarak anahtarlarını değiştirebilirsiniz.

Enstrümantasyon anahtarını yapılandırma dosyasından almak yerine, kodunuza ayarlayabilirsiniz. Anahtarı, ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yönteminde ayarlayın:

*C #*

```csharp
protected void Application_Start()
{
    Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey =
        // - for example -
        WebConfigurationManager.Settings["ikey"];
    ...
}
```

*Javascript*

```javascript
appInsights.config.instrumentationKey = myKey;
```

Web sayfalarında, tam anlamıyla komut dosyasına kodlamak yerine web sunucusunun durumundan ayarlamak isteyebilirsiniz. Örneğin, bir ASP.NET uygulamasında oluşturulan bir web sayfasında:

*Razor içinde JavaScript*

```cshtml
<script type="text/javascript">
// Standard Application Insights webpage script:
var appInsights = window.appInsights || function(config){ ...
// Modify this part:
}({instrumentationKey:  
    // Generate from server property:
    @Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey;
}) // ...
```

```java
    String instrumentationKey = "00000000-0000-0000-0000-000000000000";

    if (instrumentationKey != null)
    {
        TelemetryConfiguration.getActive().setInstrumentationKey(instrumentationKey);
    }
```

## <a name="telemetrycontext"></a>TelemetriBağlam

TelemetriIstemci, tüm telemetri verileriyle birlikte gönderilen değerleri içeren bir Bağlam özelliğine sahiptir. Normalde standart telemetri modülleri tarafından ayarlanır, ancak bunları kendiniz de ayarlayabilirsiniz. Örnek:

```csharp
telemetry.Context.Operation.Name = "MyOperationName";
```

Bu değerlerden herhangi birini kendiniz ayarlarsanız, değerleriniz inip standart değerlerinkarışmaması için ilgili satırı [ApplicationInsights.config'den](../../azure-monitor/app/configuration-with-applicationinsights-config.md)kaldırmayı düşünün.

* **Bileşen**: Uygulama ve sürümü.
* **Aygıt**: Uygulamanın çalıştığı cihazla ilgili veriler. (Web uygulamalarında, bu telemetrinin gönderildiği sunucu veya istemci aygıtıdır.)
* **InstrumentationKey**: Telemetrinin göründüğü Azure'daki Application Insights kaynağı. Genellikle ApplicationInsights.config alınır.
* **Konum**: Cihazın coğrafi konumu.
* **Operasyon**: Web uygulamalarında, geçerli HTTP isteği. Diğer uygulama türlerinde, bunu olayları birlikte gruplamak için ayarlayabilirsiniz.
  * **ID**: Tanılama Arama'daki herhangi bir olayı incelediğinizde ilgili öğeleri bulabilmeniz için farklı olayları ilişkilendiren oluşturulan değerdir.
  * **Adı**: Bir tanımlayıcı, genellikle HTTP isteğinin URL'si.
  * **SyntheticSource**: Null veya boş değilse, isteğin kaynağının bir robot veya web testi olarak tanımlandığını gösteren bir dize. Varsayılan olarak, Ölçümler Gezgini'ndeki hesaplamaların dışında tutulur.
* **Özellikler**: Tüm telemetri verileriyle birlikte gönderilen özellikler. Tek tek Track* aramalarında geçersiz kılınabilir.
* **Oturum**: Kullanıcının oturumu. Kimlik, kullanıcı bir süredir etkin olmadığında değiştirilen oluşturulan bir değere ayarlanır.
* **Kullanıcı**: Kullanıcı bilgileri.

## <a name="limits"></a>Sınırlar

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

Veri hızı sınırına çarpmamak için [örnekleme](../../azure-monitor/app/sampling.md)kullanın.

Verilerin ne kadar süreyle tutulduğunu belirlemek için [bkz.](../../azure-monitor/app/data-retention-privacy.md)

## <a name="reference-docs"></a>Referans dokümanları

* [ASP.NET referans](https://docs.microsoft.com/dotnet/api/overview/azure/insights?view=azure-dotnet)
* [Java başvurusu](https://docs.microsoft.com/java/api/overview/azure/appinsights?view=azure-java-stable/)
* [JavaScript başvurusu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)


## <a name="sdk-code"></a>SDK kodu

* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [Windows Server paketleri](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [Node.js SDK'sı](https://github.com/Microsoft/ApplicationInsights-Node.js)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)


## <a name="questions"></a>Sorular

* *Track_() çağrıları hangi özel durumlar alababilir?*

    Yok. Onları try-catch yan tümcelerine sarmanız gerekmez. SDK sorunlarla karşılaşırsa, iletileri hata ayıklama konsolçıkışında ve iletiler geçerse Tanılama Arama'da günlüğe kaydeder.
* *Portaldan veri almak için bir REST API var mı?*

    Evet, [veri erişim API.](https://dev.applicationinsights.io/) Verileri ayıklamanın diğer yolları arasında [Analytics'ten Power BI'ye ihracat](../../azure-monitor/app/export-power-bi.md ) ve [sürekli dışa aktarma](../../azure-monitor/app/export-telemetry.md)yer almaktadır.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

* [Arama olayları ve günlükleri](../../azure-monitor/app/diagnostic-search.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
