---
title: Özel olaylar ve ölçümler için API Application Insights | Microsoft Docs
description: Kullanımı izlemek ve sorunları tanılamak için cihazınıza veya masaüstü uygulamanıza, Web sayfasına veya hizmete birkaç satır kod ekleyin.
ms.topic: conceptual
ms.date: 03/27/2019
ms.openlocfilehash: 152bd117ec0ae76c2c85ead26ba5278aa71d582f
ms.sourcegitcommit: eaec2e7482fc05f0cac8597665bfceb94f7e390f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82509296"
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Özel olaylar ve ölçümler için Application Insights API

Kullanıcılara neler yaptığını öğrenmek veya sorunları tanılamaya yardımcı olmak için uygulamanıza birkaç satır kod ekleyin. Cihaz ve Masaüstü uygulamalarından, Web istemcilerinden ve Web sunucularından telemetri gönderebilirsiniz. Özel olaylar ve ölçümler ve kendi standart telemetri sürümleriniz göndermek için [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) çekırdek telemetri API 'sini kullanın. Bu API, standart Application Insights veri toplayıcılarının kullandığı API 'dir.

## <a name="api-summary"></a>API özeti

Çekirdek API 'SI, (yalnızca .net) gibi birkaç çeşitten farklı olarak tüm `GetMetric`platformlarda tek bir şekilde yayılır.

| Yöntem | Kullanıldığı yerler |
| --- | --- |
| [`TrackPageView`](#page-views) |Sayfalar, ekranlar, Blade veya formlar. |
| [`TrackEvent`](#trackevent) |Kullanıcı eylemleri ve diğer olaylar. Kullanıcı davranışını izlemek veya performansı izlemek için kullanılır. |
| [`GetMetric`](#getmetric) |Sıfır ve çok boyutlu ölçümler, merkezi olarak yapılandırılmış toplama, yalnızca C#. |
| [`TrackMetric`](#trackmetric) |Belirli olaylarla ilgili değildir sıra uzunlukları gibi performans ölçümleri. |
| [`TrackException`](#trackexception) |Tanılama için özel durumlar günlüğe kaydediliyor. Diğer olaylarla ilgili olarak nerede olduğunu izleyin ve yığın izlemelerini inceleyin. |
| [`TrackRequest`](#trackrequest) |Performans analizi için sunucu isteklerinin sıklığını ve süresini günlüğe kaydetme. |
| [`TrackTrace`](#tracktrace) |Kaynak tanılama günlük iletileri. Ayrıca, üçüncü taraf günlüklerini yakalayabilirsiniz. |
| [`TrackDependency`](#trackdependency) |Uygulamanızın bağımlı olduğu dış bileşenlere yapılan çağrıların süresini ve sıklığını günlüğe kaydetme. |

Bu telemetri çağrılarının çoğuna [Özellikler ve ölçümler](#properties) ekleyebilirsiniz.

## <a name="before-you-start"></a><a name="prep"></a>Başlamadan önce

Application Insights SDK 'da henüz bir başvurunuz yoksa:

* Application Insights SDK 'sını projenize ekleyin:

  * [ASP.NET projesi](../../azure-monitor/app/asp-net.md)
  * [ASP.NET Core projesi](../../azure-monitor/app/asp-net-core.md)
  * [Java projesi](../../azure-monitor/app/java-get-started.md)
  * [Node. js projesi](../../azure-monitor/app/nodejs.md)
  * [Her Web sayfasında JavaScript](../../azure-monitor/app/javascript.md) 
* Cihazınızda veya Web sunucusu kodunuzda şunları dahil edin:

    *C#:*`using Microsoft.ApplicationInsights;`

    *Visual Basic:*`Imports Microsoft.ApplicationInsights`

    *Java:*`import com.microsoft.applicationinsights.TelemetryClient;`

    *Node. js:*`var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>Bir TelemetryClient örneği alın

Bir örneğini al `TelemetryClient` (Web sayfalarındaki JavaScript hariç):

[.Net/.NET Core uygulamaları için ASP.NET Core uygulamalar ve http olmayan/çalışan](worker-service.md#how-can-i-track-telemetry-thats-not-automatically-collected) için, ilgili belgelerde açıklandığı gibi bağımlılık ekleme `TelemetryClient` kapsayıcısından bir örnek almanız önerilir. [ASP.NET Core](asp-net-core.md#how-can-i-track-telemetry-thats-not-automatically-collected)

AzureFunctions v2 + veya Azure WebJobs v3 + kullanıyorsanız, bu belgeyi izleyin:https://docs.microsoft.com/azure/azure-functions/functions-monitoring#version-2x-and-higher

*, #*

```csharp
private TelemetryClient telemetry = new TelemetryClient();
```
Bu yöntemi artık kullanılmayan iletilerde, daha fazla ayrıntı için lütfen [Microsoft/ApplicationInsights-DotNet # 1152](https://github.com/microsoft/ApplicationInsights-dotnet/issues/1152) adresini ziyaret edin.

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

TelemetryClient iş parçacığı güvenlidir.

ASP.NET ve Java projeleri için gelen HTTP Istekleri otomatik olarak yakalanır. Uygulamanızın diğer modülü için ek TelemetryClient örnekleri oluşturmak isteyebilirsiniz. Örneğin, iş mantığı olaylarını raporlamak için, ara yazılım sınıfınızdaki bir TelemetryClient örneğiniz olabilir. Makineyi tanımlamak için UserID ve DeviceID gibi özellikleri ayarlayabilirsiniz. Bu bilgiler, örneğin gönderdiği tüm olaylara iliştirilir.

*, #*

```csharp
TelemetryClient.Context.User.Id = "...";
TelemetryClient.Context.Device.Id = "...";
```

*Java*

```java
telemetry.getContext().getUser().setId("...");
telemetry.getContext().getDevice().setId("...");
```

Node. js projelerinde, yeni bir örnek oluşturmak `new applicationInsights.TelemetryClient(instrumentationKey?)` için kullanabilirsiniz, ancak bu yalnızca singleton `defaultClient`'ten yalıtılmış yapılandırma gerektiren senaryolar için önerilir.

## <a name="trackevent"></a>TrackEvent

Application Insights, özel bir *olay* , toplanan bir sayı olarak [Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md) görüntülenebilecek ve tek tek oluşum olarak [Tanılama aramasında](../../azure-monitor/app/diagnostic-search.md) kullanabileceğiniz bir veri noktasıdır. (MVC veya diğer Framework "olayları" ile ilgili değildir.)

Çeşitli `TrackEvent` olayları saymak için kodunuzda çağrı ekleyin. Kullanıcıların belirli bir özelliği ne sıklıkta seçtikleri, belirli hedeflere ne sıklıkta elde ettikleri veya ne sıklıkta belirli türde hatalar gerçekleştirdikleri.

Örneğin, bir oyun uygulamasında, Kullanıcı oyunu her kullandığında bir olay gönderin:

*JavaScript*

```javascript
appInsights.trackEvent({name:"WinGame"});
```

*, #*

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

### <a name="custom-events-in-analytics"></a>Analytics 'te özel olaylar

Telemetri, `customEvents` [Application Insights Analytics](analytics.md)'teki tabloda kullanılabilir. Her satır uygulamanızdaki bir çağrıyı `trackEvent(..)` temsil eder.

[Örnekleme](../../azure-monitor/app/sampling.md) işlemi içinde Ise, ItemCount özelliği 1 ' den büyük bir değeri gösterir. Örneğin ItemCount = = 10, trackEvent () için 10 çağrının (), örnekleme işleminin yalnızca birini aktardığı anlamına gelir. Özel olayların doğru sayısını almak için, bu nedenle gibi `customEvents | summarize sum(itemCount)`bir kod kullanmanız gerekir.

## <a name="getmetric"></a>GetMetric

.NET ve .NET Core uygulamaları için yerel olarak önceden toplanmış ölçümleri yakalamak üzere GetMetric () çağrısını etkin bir şekilde nasıl kullanacağınızı öğrenmek için [GetMetric](../../azure-monitor/app/get-metric.md) belgelerini ziyaret edin.

## <a name="trackmetric"></a>TrackMetric

> [!NOTE]
> Microsoft. ApplicationInsights. TelemetryClient. TrackMetric ölçüm göndermek için tercih edilen yöntem değildir. Ölçümler gönderilmeden önce her zaman bir süre içinde önceden toplanmalıdır. SDK ön toplama özelliklerine erişim için bir ölçüm nesnesi almak üzere GetMetric (..) aşırı yüklerini kullanın. Kendi ön toplama mantığınızı uygulamadıysanız, elde edilen toplamaları göndermek için TrackMetric () yöntemini kullanabilirsiniz. Uygulamanız zaman içinde toplanmadan her gün ayrı bir telemetri öğesi gönderilmesini gerektiriyorsa, büyük olasılıkla olay telemetrisi için bir kullanım örneğine sahip olursunuz; bkz. TelemetryClient. TrackEvent (Microsoft. ApplicationInsights. DataContracts. Eventtelemetri).

Application Insights, belirli olaylara eklenmemiş ölçümleri grafik olarak oluşturabilir. Örneğin, düzenli aralıklarla bir sıra uzunluğu izleyebilirsiniz. Ölçümler sayesinde, bireysel ölçümler Çeşitlemeler ve eğilimleri daha az ilgi çekici olduğundan istatistiksel grafikler yararlı olur.

Application Insights ölçümleri göndermek için `TrackMetric(..)` API 'yi kullanabilirsiniz. Ölçüm göndermenin iki yolu vardır:

* Tek değer. Uygulamanızda bir ölçüm gerçekleştirdiğinizde, karşılık gelen değeri Application Insights gönderirsiniz. Örneğin, bir kapsayıcıdaki öğelerin sayısını açıklayan bir ölçümünüzün olduğunu varsayalım. Belirli bir süre boyunca, önce kapsayıcıya üç öğe yerleştirip iki öğeyi kaldırırsınız. Buna uygun olarak, iki `TrackMetric` kez çağrı yapmanız gerekir: önce `3` değeri geçirerek değeri `-2`. Application Insights her iki değeri de sizin adınıza depolar.

* Toplama. Ölçümler ile çalışırken her tek ölçüm çok nadir olur. Bunun yerine, belirli bir süre boyunca ne olduğunu bir Özet önem taşır. Bu tür bir Özet _toplama_olarak adlandırılır. Yukarıdaki örnekte, bu dönemin toplam ölçüm toplamı `1` ve ölçüm değerlerinin sayısı. `2` Toplama yaklaşımını kullanırken, zaman aralığı için yalnızca bir `TrackMetric` kez çağırır ve toplama değerlerini gönderirsiniz. Bu, Application Insights ' ye daha az veri noktası göndererek maliyet ve performans yükünü önemli ölçüde azaltacağından ve ilgili tüm bilgileri almaya devam ederken önerilen yaklaşımdır.

### <a name="examples"></a>Örnekler

#### <a name="single-values"></a>Tek değerler

Tek bir ölçüm değeri göndermek için:

*JavaScript*

 ```javascript
appInsights.trackMetric("queueLength", 42.0);
 ```

*, #*

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

### <a name="custom-metrics-in-analytics"></a>Analytics 'te özel ölçümler

Telemetri, `customMetrics` [Application Insights Analytics](analytics.md)'teki tabloda kullanılabilir. Her satır uygulamanızdaki bir çağrıyı `trackMetric(..)` temsil eder.

* `valueSum`-Bu, ölçümlerin toplamıdır. Ortalama değeri almak için, öğesini ayırın `valueCount`.
* `valueCount`-Bu `trackMetric(..)` çağrıda toplanmış ölçüm sayısı.

## <a name="page-views"></a>Sayfa görünümleri

Bir cihaz veya Web sayfası uygulamasında, her ekran veya sayfa yüklendiğinde varsayılan olarak sayfa görünümü telemetrisi gönderilir. Ancak sayfa görünümlerini ek veya farklı zamanlarda izlemek için bunu değiştirebilirsiniz. Örneğin, sekmeler veya blade görüntüleyen bir uygulamada, Kullanıcı yeni bir dikey pencere açtığında bir sayfayı izlemek isteyebilirsiniz.

Kullanıcı ve oturum verileri sayfa görünümleriyle birlikte özellikler olarak gönderilir; bu nedenle, sayfa görüntüleme telemetrisi olduğunda Kullanıcı ve oturum grafikleri etkin olur.

### <a name="custom-page-views"></a>Özel sayfa görünümleri

*JavaScript*

```javascript
appInsights.trackPageView("tab1");
```

*, #*

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

Farklı HTML sayfaları içinde birkaç sekmeye sahipseniz URL 'YI de belirtebilirsiniz:

```javascript
appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");
```

### <a name="timing-page-views"></a>Zamanlama sayfası görünümleri

Varsayılan olarak, **sayfa görünümü yükleme süresi** olarak bildirilen süreler, tarayıcının sayfa yükleme olayı çağrılana kadar, tarayıcı isteği gönderdiğinde ölçülür.

Bunun yerine şunlardan birini yapabilirsiniz:

* [TrackPageview](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/legacy/API.md#trackpageview) çağrısında açık bir süre ayarlayın: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Sayfa görüntüleme zamanlaması çağrılarını `startTrackPage` ve `stopTrackPage`öğesini kullanın.

*JavaScript*

```javascript
// To start timing a page:
appInsights.startTrackPage("Page1");

...

// To stop timing and log the page:
appInsights.stopTrackPage("Page1", url, properties, measurements);
```

İlk parametre olarak kullandığınız ad, başlangıç ve durdurma çağrılarını ilişkilendirir. Geçerli sayfa adını varsayılan olarak alır.

Ölçüm Gezgini görüntülenen sonuç sayfası yükleme süreleri, başlangıç ve durdurma çağrıları arasındaki aralıktan türetilir. Size gerçekten zaman aralığı kadar zaman dilimi kadar.

### <a name="page-telemetry-in-analytics"></a>Analytics 'te sayfa telemetrisi

[Analiz](analytics.md) ' de iki tabloda, tarayıcı işlemlerinden verileri göster:

* `pageViews` Tablo, URL ve sayfa başlığı hakkındaki verileri içerir
* `browserTimings` Tablo, gelen verileri işlemek için geçen süre gibi istemci performansı hakkındaki verileri içerir

Tarayıcının farklı sayfaları işlemeye ne kadar süreceği hakkında bilgi edinmek için:

```kusto
browserTimings
| summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name
```

Farklı tarayıcıların popululuğunu bulma:

```kusto
pageViews
| summarize count() by client_Browser
```

Sayfa görünümlerini AJAX çağrılarına ilişkilendirmek için bağımlılıklarla birleştirin:

```kusto
pageViews
| join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest

Sunucu SDK 'Sı HTTP isteklerini günlüğe kaydetmek için TrackRequest kullanır.

Ayrıca, Web hizmeti modülünün çalıştırıldığı bağlamdaki isteklerin benzetimini yapmak istiyorsanız kendiniz de çağırabilirsiniz.

Ancak, istek Telemetriyi göndermek için önerilen yol, isteğin bir <a href="#operation-context">işlem bağlamı</a>işlevi görür.

## <a name="operation-context"></a>İşlem bağlamı

Telemetri öğelerini işlem bağlamıyla ilişkilendirerek ilişkilendirilebilir. Standart istek izleme modülü, bir HTTP isteği işlenirken gönderilen özel durumlar ve diğer olaylar için bunu yapar. [Arama](../../azure-monitor/app/diagnostic-search.md) ve [analiz](analytics.md)' de, istekle ILIŞKILI tüm olayları işlem kimliğini kullanarak kolayca bulabilirsiniz.

Bağıntı hakkında daha fazla bilgi için [Application Insights telemetri bağıntısını](../../azure-monitor/app/correlation.md) inceleyin.

Telemetriyi el ile izlerken, bu kalıbı kullanarak telemetri bağıntısını sağlamanın en kolay yolu:

*, #*

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

Bir işlem bağlamını ayarlamaya birlikte, `StartOperation` belirttiğiniz türden bir telemetri öğesi oluşturur. İşlemi çıkardığınızda telemetri öğesini gönderir veya açıkça çağırdıysanız `StopOperation`. Telemetri türü olarak `RequestTelemetry` kullanıyorsanız, süresi başlangıç ve durdurma arasındaki zaman aralığı olarak ayarlanır.

İşlem kapsamı içinde bildirilen telemetri öğeleri, bu işlemin ' alt öğeleri ' olur. İşlem bağlamları iç içe olabilir.

Aramada, işlem bağlamı **Ilgili öğeler** listesini oluşturmak için kullanılır:

![İlgili öğeler](./media/api-custom-events-metrics/21.png)

Özel işlemleri izleme hakkında daha fazla bilgi için bkz. [.NET SDK ile özel Işlemleri izleme Application Insights](../../azure-monitor/app/custom-operations-tracking.md) .

### <a name="requests-in-analytics"></a>Analytics istekleri

[Application Insights Analytics](analytics.md)'te, istekler `requests` tabloda gösterilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlemi içinde Ise, ItemCount özelliği 1 ' den büyük bir değer gösterir. Örneğin ItemCount = = 10, trackRequest () için 10 çağrının, örnekleme işleminin yalnızca birini aktardığı anlamına gelir. İstek adlarına göre doğru istek sayısını ve ortalama süreyi almak için, şöyle bir kod kullanın:

```kusto
requests
| summarize count = sum(itemCount), avgduration = avg(duration) by name
```

## <a name="trackexception"></a>TrackException

Application Insights özel durum gönder:

* Bunları, bir sorun sıklığının göstergesi olarak [saymak](../../azure-monitor/platform/metrics-charts.md)için.
* [Tek tek oluşumları incelemek](../../azure-monitor/app/diagnostic-search.md)için.

Raporlar yığın izlemelerini içerir.

*, #*

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

*JavaScript*

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

SDK 'lar birçok özel durumu otomatik olarak yakalar, bu nedenle her zaman TrackException 'ı açıkça çağırmanız gerekmez.

* ASP.NET: [özel durumları yakalamak için kod yazın](../../azure-monitor/app/asp-net-exceptions.md).
* Java EE: [özel durumlar otomatik olarak yakalanır](../../azure-monitor/app/java-get-started.md#exceptions-and-request-failures).
* JavaScript: özel durumlar otomatik olarak yakalanır. Otomatik toplamayı devre dışı bırakmak istiyorsanız, Web sayfalarınızı eklediğiniz kod parçacığına bir satır ekleyin:

```javascript
({
    instrumentationKey: "your key",
    disableExceptionTracking: true
})
```

### <a name="exceptions-in-analytics"></a>Analytics 'te özel durumlar

[Application Insights Analytics](analytics.md)'te özel durumlar `exceptions` tabloda gösterilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlemi ise, `itemCount` özelliği 1 ' den büyük bir değeri gösterir. Örneğin ItemCount = = 10, trackException () için 10 çağrının, örnekleme işleminin yalnızca birini aktardığı anlamına gelir. Özel durum türüne göre bölünmüş özel durumların doğru sayısını almak için, şu gibi bir kod kullanın:

```kusto
exceptions
| summarize sum(itemCount) by type
```

Önemli yığın bilgilerinin çoğu farklı değişkenlere zaten ayıklandı, ancak daha fazla bilgi edinmek için `details` yapıyı ayırabilirsiniz. Bu yapı dinamik olduğundan, sonucu istediğiniz türe atamalısınız. Örneğin:

```kusto
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

Özel durumları ilgili isteklerle ilişkilendirmek için bir JOIN kullanın:

```kusto
exceptions
| join (requests) on operation_Id
```

## <a name="tracktrace"></a>TrackTrace

Application Insights için bir "içerik haritası izi" göndererek sorunları tanılamaya yardımcı olması için TrackTrace kullanın. Tanılama verileri parçalarını gönderebilir ve bunları [Tanılama aramasında](../../azure-monitor/app/diagnostic-search.md)inceleyebilirsiniz.

.NET [günlük bağdaştırıcılarında](../../azure-monitor/app/asp-net-trace-logs.md) , portala üçüncü taraf Günlükler göndermek IÇIN bu API 'yi kullanır.

Java 'da [Log4J gibi standart Günlükçüler için, logback](../../azure-monitor/app/java-trace-logs.md) Application Insights Log4J veya Logback uygulamalarını kullanarak portala üçüncü taraf Günlükler gönderebilir.

*, #*

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

*İstemci/tarayıcı tarafı JavaScript*

```javascript
trackTrace(message: string, properties?: {[string]:string}, severityLevel?: SeverityLevel)
```

Yöntemi girme veya bir yönteme ayrılma gibi bir tanılama olayını günlüğe kaydetme.

 Parametre | Açıklama
---|---
`message` | Tanılama verileri. Bir adından çok daha uzun olabilir.
`properties` | Dizenin dize eşlemesi: portalda [özel durumları filtrelemek](https://azure.microsoft.com/documentation/articles/app-insights-api-custom-events-metrics/#properties) Için kullanılan ek veriler. Varsayılan olarak boştur.
`severityLevel` | Desteklenen değerler: [Severitylevel. TS](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/shared/AppInsightsCommon/src/Interfaces/Contracts/Generated/SeverityLevel.ts)

İleti içeriğinde arama yapabilirsiniz, ancak (Özellik değerlerinden farklı olarak) üzerinde filtreleme yapamazsınız.

Boyut sınırı `message` , özellikleri sınırından çok daha yüksek.
TrackTrace 'in avantajı, oldukça uzun verileri iletiye koyacağınızdır. Örneğin, veri Gönder ' i burada bulabilirsiniz.  

Ayrıca, iletinize önem düzeyi ekleyebilirsiniz. Diğer telemetri gibi, farklı izleme kümelerini filtrelemenize veya aramanıza yardımcı olacak özellik değerleri ekleyebilirsiniz. Örneğin:

*, #*

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

[Arama](../../azure-monitor/app/diagnostic-search.md)' da, belirli bir veritabanı ile ilgili belirli bir önem derecesindeki tüm iletileri kolayca filtreleyebilirsiniz.

### <a name="traces-in-analytics"></a>Analizler

[Application Insights Analytics](analytics.md)'te, `traces` tabloda tracktrace çağrıları gösterilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlemi içinde Ise, ItemCount özelliği 1 ' den büyük bir değeri gösterir. Örneğin ItemCount = = 10, için `trackTrace()`10 ' un, örnekleme işleminin yalnızca birini aktardığı anlamına gelir. İzleme çağrılarının doğru sayısını almak için, bu kodu gibi bir kod kullanmanız gerekir `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency

Bir dış kod parçasına yapılan çağrıların yanıt sürelerini ve başarı oranlarını izlemek için TrackDependency çağrısını kullanın. Sonuçlar, portaldaki bağımlılık grafiklerinde görüntülenir. Aşağıdaki kod parçacığının bir bağımlılık çağrısının yapıldığı her yerde eklenmesi gerekir.

*, #*

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

Sunucu SDK 'Larının, belirli bağımlılık çağrılarını tespit eden ve izleyen bir [bağımlılık modülünü](../../azure-monitor/app/asp-net-dependencies.md) (örneğin, VERITABANLARıNA ve REST API 'lerine) dahil edileceğini unutmayın. Modülün çalışmasını sağlamak için sunucunuza bir aracı yüklemelisiniz. 

Java 'da, bazı bağımlılık çağrıları [Java Aracısı](../../azure-monitor/app/java-agent.md)kullanılarak otomatik olarak izlenebilir.

Otomatik izlemenin yakalayamediği çağrıları izlemek isterseniz veya aracıyı yüklemek istemiyorsanız bu çağrıyı kullanırsınız.

C# dilinde standart bağımlılık izleme modülünü devre dışı bırakmak için [ApplicationInsights. config dosyasını](../../azure-monitor/app/configuration-with-applicationinsights-config.md) düzenleyin ve başvurusunu silin `DependencyCollector.DependencyTrackingTelemetryModule`. Java 'da, standart bağımlılıkları otomatik olarak toplamak istemiyorsanız, lütfen Java aracısını yüklemeyin.

### <a name="dependencies-in-analytics"></a>Analytics 'teki bağımlılıklar

[Application Insights Analytics](analytics.md)'te, trackdependency çağrıları `dependencies` tabloda gösterilir.

[Örnekleme](../../azure-monitor/app/sampling.md) işlemi içinde Ise, ItemCount özelliği 1 ' den büyük bir değeri gösterir. Örneğin ItemCount = = 10, trackDependency () için 10 çağrının olduğu anlamına gelir ve örnekleme işlemi bunlardan yalnızca birini iletilir. Hedef bileşene göre bölünmüş bağımlılıkların doğru sayısını almak için, şu gibi bir kod kullanın:

```kusto
dependencies
| summarize sum(itemCount) by target
```

Bağımlılıkları ilgili isteklerle ilişkilendirmek için bir JOIN kullanın:

```kusto
dependencies
| join (requests) on operation_Id
```

## <a name="flushing-data"></a>Verileri temizleme

Normalde, SDK verileri sabit aralıklarla (genellikle 30 saniye) veya arabellek dolu olduğunda (genellikle 500 öğe) gönderir. Ancak, bazı durumlarda, örneğin, SDK 'yi kapatan bir uygulamada kullanıyorsanız, arabelleği temizlemek isteyebilirsiniz.

*, #*

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

İşlev, [sunucu telemetri kanalı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/)için zaman uyumsuzdur.

İdeal olarak, Temizleme () yöntemi uygulamanın kapatılma etkinliğinde kullanılmalıdır.

## <a name="authenticated-users"></a>Kimliği doğrulanmış kullanıcılar

Bir Web uygulamasında kullanıcılar, tanımlama bilgileri tarafından tanımlanan (varsayılan olarak). Bir Kullanıcı, uygulamanıza farklı bir makineden veya tarayıcıdan erişebiliyorlarsa veya tanımlama bilgilerini sildiklerinde birden çok kez sayılır.

Kullanıcılar uygulamanızda oturum açtığında, tarayıcı kodunda kimliği doğrulanmış kullanıcı KIMLIĞINI ayarlayarak daha doğru bir sayı alabilirsiniz:

*JavaScript*

```javascript
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

Bir ASP.NET Web MVC uygulamasında, örneğin:

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

Kullanıcının gerçek oturum açma adını kullanmak gerekli değildir. Yalnızca bu kullanıcı için benzersiz olan bir KIMLIK olmalıdır. Boşluk veya karakterleri `,;=|`içermemelidir.

Kullanıcı KIMLIĞI bir oturum tanımlama bilgisinde de ayarlanır ve sunucusuna gönderilir. Sunucu SDK 'Sı yüklüyse, kimliği doğrulanmış kullanıcı KIMLIĞI hem istemci hem de sunucu telemetrinin bağlam özelliklerinin bir parçası olarak gönderilir. Daha sonra filtreleyebilir ve üzerinde arama yapabilirsiniz.

Uygulamanız kullanıcıları hesaplara gruplamadıysanız, hesap için bir tanımlayıcı da geçirebilirsiniz (aynı karakter kısıtlamalarına sahip).

```javascript
appInsights.setAuthenticatedUserContext(validatedId, accountId);
```

[Ölçüm Gezgini](../../azure-monitor/platform/metrics-charts.md), **kullanıcıları, kimliği doğrulanmış**ve **Kullanıcı hesaplarını**sayan bir grafik oluşturabilirsiniz.

Ayrıca, belirli kullanıcı adları ve hesapları ile istemci veri noktalarını [arayabilirsiniz](../../azure-monitor/app/diagnostic-search.md) .

## <a name="filtering-searching-and-segmenting-your-data-by-using-properties"></a><a name="properties"></a>Özellikleri kullanarak verilerinizi filtreleme, arama ve parçalara ayırma

Olaylarınıza Özellikler ve ölçümler ekleyebilirsiniz (Ayrıca ölçümler, sayfa görünümleri, özel durumlar ve diğer telemetri verileri için).

*Özellikler* , kullanım raporlarında Telemetriyi filtrelemek için kullanabileceğiniz dize değerleridir. Örneğin, uygulamanız çeşitli oyunlar sağlıyorsa, hangi oyunların daha popüler olduğunu görebilmeniz için her bir olaya oyunun adını ekleyebilirsiniz.

Dize uzunluğu üzerinde 8192 sınırı vardır. (Büyük veri öbeklerini göndermek istiyorsanız, TrackTrace 'in ileti parametresini kullanın.)

*Ölçümler* , grafiksel olarak sunulabilen sayısal değerlerdir. Örneğin, oyun görmeniz gereken puanlar üzerinde aşamalı bir artış olup olmadığını görmek isteyebilirsiniz. Grafikler, farklı oyunlarda ayrı veya yığılmış grafikler alabilmeniz için olayla birlikte gönderilen özelliklerle ayrılabilir.

Ölçüm değerlerinin doğru görüntülenmesi için, 0 ' dan büyük veya buna eşit olmalıdır.

Kullanabileceğiniz özellik [sayısı, özellik değerleri ve ölçümler için bazı sınırlar](#limits) vardır.

*JavaScript*

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

*, #*

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
> Özelliklerde kişisel olarak tanımlanabilen bilgileri günlüğe kaydetmek için dikkatli olmanız gerekmez.
>
>

### <a name="alternative-way-to-set-properties-and-metrics"></a>Özellikleri ve ölçümleri ayarlamak için alternatif yol

Daha uygun ise, bir olayın parametrelerini ayrı bir nesnede toplayabilirsiniz:

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
> Izlemeyi * () birden çok kez çağırmak`event` için aynı telemetri öğesi örneğini (Bu örnekte) yeniden kullanmayın. Bu, telemetrinin yanlış yapılandırmayla gönderilmesine neden olabilir.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Analiz içindeki özel ölçümler ve Özellikler

[Analytics](analytics.md)'te özel ölçümler ve özellikler, `customMeasurements` her telemetri kaydının ve `customDimensions` özniteliklerinde gösterilir.

Örneğin, istek telemetrinize "oyun" adlı bir özellik eklediyseniz, bu sorgu farklı "oyun" değerlerinin oluşumlarını sayar ve "Score" özel ölçüsünün ortalamasını gösterir:

```kusto
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game)
```

Dikkat edin:

* CustomDimensions veya Customölçüler JSON öğesinden bir değeri ayıkladığınızda, dinamik türe sahiptir ve bu `tostring` nedenle veya `todouble`' i atamalısınız.
* [Örnekleme](../../azure-monitor/app/sampling.md)olasılığa yönelik bir hesap almak için, kullanmanız `sum(itemCount)`gerekir. `count()`

## <a name="timing-events"></a><a name="timed"></a>Zamanlama olayları

Bazen bir eylem gerçekleştirmek için ne kadar sürdüğünü grafik yapmak isteyebilirsiniz. Örneğin, kullanıcıların bir oyunun seçeneklerini düşünmek için ne kadar süreceğine öğrenmek isteyebilirsiniz. Bunun için ölçüm parametresini kullanabilirsiniz.

*, #*

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

Yazdığınız bazı özel olaylar için varsayılan özellik değerlerini ayarlamak istiyorsanız, bunları bir TelemetryClient örneğinde ayarlayabilirsiniz. Bu istemciler, bu istemciden gönderilen her telemetri öğesine eklenir.

*, #*

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

*JavaScript Web Istemcileri Için*JavaScript telemetri başlatıcıları ' nı kullanın.

Standart koleksiyon modüllerindeki veriler de dahil olmak üzere *Tüm telemetrisine özellikler eklemek için*, uygulamasını [uygulayın `ITelemetryInitializer` ](../../azure-monitor/app/api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Telemetriyi örnekleme, filtreleme ve işleme

SDK 'dan gönderilmeden önce Telemetriyi işlemek için kod yazabilirsiniz. İşleme, HTTP istek koleksiyonu ve bağımlılık koleksiyonu gibi standart telemetri modüllerden gönderilen verileri içerir.

Uygulayarak `ITelemetryInitializer`telemetrisine [Özellikler ekleyin](../../azure-monitor/app/api-filtering-sampling.md#add-properties) . Örneğin, diğer özelliklerden hesaplanan sürüm numaralarını veya değerleri ekleyebilirsiniz.

[Filtreleme](../../azure-monitor/app/api-filtering-sampling.md#filtering) , ' yı uygulayarak `ITelemetryProcessor`SDK 'dan gönderilmeden önce Telemetriyi değiştirebilir veya atabilir. Ne gönderildiğini ve atılacağını denetlersiniz, ancak ölçümleriniz üzerindeki etkiyi hesaba eklemek zorunda olursunuz. Öğeleri nasıl atdığınıza bağlı olarak, ilgili öğeler arasında gezinme özelliğini kaybedebilirsiniz.

[Örnekleme](../../azure-monitor/app/api-filtering-sampling.md) , uygulamanızdan portala gönderilen veri hacmini azaltmak için paketlenmiş bir çözümdür. Bu, görünen ölçümleri etkilemeden bunu yapar. Ayrıca, özel durumlar, istekler ve sayfa görünümleri gibi ilgili öğeler arasında gezinerek sorunları tanılama yeteneğinizi etkilemeden bunu yapar.

[Daha fazla bilgi edinin](../../azure-monitor/app/api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetri devre dışı bırakılıyor

Telemetri toplamayı ve iletimini *dinamik olarak durdurmak ve başlatmak* için:

*, #*

```csharp
using  Microsoft.ApplicationInsights.Extensibility;

TelemetryConfiguration.Active.DisableTelemetry = true;
```

*Java*

```java
telemetry.getConfiguration().setTrackingDisabled(true);
```

*Seçili standart toplayıcıları devre dışı bırakmak*için (örneğin, performans SAYAÇLARı, http istekleri veya bağımlılıklar), [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)dosyasında ilgili satırları silin veya not alın. Örneğin, kendi TrackRequest verilerinizi göndermek istiyorsanız bunu yapabilirsiniz.

*Node.js*

```javascript
telemetry.config.disableAppInsights = true;
```

*Seçili standart toplayıcıları devre dışı bırakmak*için--örneğin, performans SAYAÇLARı, http istekleri veya bağımlılıklar--başlatma SıRASıNDA, SDK başlatma kodunuzda zincir yapılandırma yöntemleri:

```javascript
applicationInsights.setup()
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    .setAutoCollectDependencies(false)
    .setAutoCollectConsole(false)
    .start();
```

Başlatma sonrasında bu toplayıcıları devre dışı bırakmak için yapılandırma nesnesini kullanın:`applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="developer-mode"></a><a name="debug"></a>Geliştirici modu

Hata ayıklama sırasında, sonuçları hemen görebilmeniz için Telemetriyi ardışık düzen aracılığıyla elde etmeniz yararlı olur. Telemetriyle ilgili sorunları izlemenize yardımcı olan ek iletiler de alırsınız. Uygulamanızı yavaşlatabileceğinden, üretimde devre dışı bırakın.

*, #*

```csharp
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;
```

*Visual Basic*

```vb
TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True
```

*Node.js*

Node. js için, ile iç günlük kaydını etkinleştirerek `setInternalLogging` ve ayarını `maxBatchSize` 0 olarak etkinleştirerek Geliştirici modunu etkinleştirebilirsiniz. Bu, Telemetriyi toplandıktan hemen sonra gönderilmesine neden olur.

```js
applicationInsights.setup("ikey")
  .setInternalLogging(true, true)
  .start()
applicationInsights.defaultClient.config.maxBatchSize = 0;
```

## <a name="setting-the-instrumentation-key-for-selected-custom-telemetry"></a><a name="ikey"></a>Seçili özel telemetri için izleme anahtarı ayarlanıyor

*, #*

```csharp
var telemetry = new TelemetryClient();
telemetry.InstrumentationKey = "---my key---";
// ...
```

## <a name="dynamic-instrumentation-key"></a><a name="dynamic-ikey"></a>Dinamik izleme anahtarı

Geliştirme, test ve üretim ortamlarından Telemetriyi karışmamak için, ortama bağlı olarak [ayrı Application Insights kaynakları oluşturabilir](../../azure-monitor/app/create-new-resource.md ) ve anahtarlarını değiştirebilirsiniz.

Yapılandırma dosyasından izleme anahtarını almak yerine kodunuzda bu ayarı yapabilirsiniz. Bir ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yönteminde anahtarı ayarlayın:

*, #*

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

*JavaScript*

```javascript
appInsights.config.instrumentationKey = myKey;
```

Web sayfalarında, betik içine yazmak yerine Web sunucusunun durumundan bir şekilde ayarlamak isteyebilirsiniz. Örneğin, bir ASP.NET uygulamasında oluşturulan bir Web sayfasında:

*Razor 'de JavaScript*

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

## <a name="telemetrycontext"></a>TelemetryContext

TelemetryClient, tüm telemetri verileriyle birlikte gönderilen değerleri içeren bir Context özelliğine sahiptir. Bunlar normalde standart telemetri modülleri tarafından ayarlanır, ancak bunları kendiniz de ayarlayabilirsiniz. Örneğin:

```csharp
telemetry.Context.Operation.Name = "MyOperationName";
```

Bu değerlerden herhangi birini kendiniz ayarlarsanız [ApplicationInsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md)dosyasından ilgili satırı kaldırmayı düşünün; böylece değerleriniz ve standart değerler karıştırılmamalıdır.

* **Bileşen**: uygulama ve sürümü.
* **Cihaz**: uygulamanın çalıştığı cihazla ilgili veriler. (Web Apps 'te, telemetrinin gönderildiği sunucu veya istemci aygıtıdır.)
* **Instrumentationkey**: Azure 'da Telemetriyi göründüğü Application Insights kaynak. Genellikle ApplicationInsights. config dosyasından alınır.
* **Konum**: cihazın coğrafi konumu.
* **İşlem**: Web Apps 'TE geçerli http isteği. Diğer uygulama türlerinde, bunu olayları gruplamak için de ayarlayabilirsiniz.
  * **ID**: farklı olayları ilişkilendiren oluşturulmuş bir değer, böylece tanılama aramasında herhangi bir olayı inceleyebileceğiniz ilgili öğeleri bulabilirsiniz.
  * **Ad**: bir tanımlayıcı, genellikle http isteğinin URL 'si.
  * **Syntheticsource**: null veya boş değilse, isteğin kaynağının bir robot veya Web testi olarak tanımlandığını gösteren bir dize. Varsayılan olarak, Ölçüm Gezgini hesaplamalarında çıkarılır.
* **Özellikler**: tüm telemetri verileriyle gönderilen Özellikler. Tek tek Izleme * çağrılarında geçersiz kılınabilir.
* **Oturum**: kullanıcının oturumu. KIMLIĞI, Kullanıcı bir süredir etkin olmadığında değiştirilen bir üretilen değere ayarlanır.
* **Kullanıcı**: Kullanıcı bilgileri.

## <a name="limits"></a>Sınırlar

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

Veri hızı sınırına ulaşmaktan kaçınmak için [örnekleme](../../azure-monitor/app/sampling.md)kullanın.

Verilerin ne kadar süreyle tutulacağını öğrenmek için bkz. [veri saklama ve gizlilik](../../azure-monitor/app/data-retention-privacy.md).

## <a name="reference-docs"></a>Başvuru belgeleri

* [ASP.NET başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/insights?view=azure-dotnet)
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

* *Hangi özel durumlar Track_ () çağrı throw?*

    Yok. Bunları try-catch yan tümcelerinde sarmalısınız. SDK sorunlarla karşılaşırsa, hata ayıklama konsolu çıkışında iletileri günlüğe kaydeder ve iletiler tanılama araması ' nda ile alıyorsa.
* *Portaldan veri almak için bir REST API var mı?*

    Evet, [veri erişimi API 'si](https://dev.applicationinsights.io/). Verilerin ayıklanmasına yönelik diğer yollar [analiz 'ten Power BI](../../azure-monitor/app/export-power-bi.md ) ve [sürekli dışarı aktarmaya](../../azure-monitor/app/export-telemetry.md)aktarma içerir.

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

* [Olayları ve günlükleri ara](../../azure-monitor/app/diagnostic-search.md)
* [Sorun giderme](../../azure-monitor/app/troubleshoot-faq.md)
