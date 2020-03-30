---
title: Azure Application Insights günlük tabanlı ölçümler | Microsoft Dokümanlar
description: Bu makalede, desteklenen toplamalar ve boyutlarla Azure Uygulama Öngörüleri ölçümleri listelenir. Günlük tabanlı ölçümlerle ilgili ayrıntılar, temel Kusto sorgu ekstrelerini içerir.
author: vgorbenko
services: azure-monitor
ms.topic: reference
ms.date: 07/03/2019
ms.author: vitalyg
ms.subservice: application-insights
ms.openlocfilehash: 12bc51e800ef5ccd4ad3c72d3860fb22bac5b749
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664924"
---
# <a name="application-insights-log-based-metrics"></a>Uygulama Öngörüleri günlük tabanlı ölçümler

Application Insights günlük tabanlı ölçümler, izlenen uygulamalarınızın durumunu analiz edeyim, güçlü panolar oluşturmanıza ve uyarıları yapılandırmanıza sağlar. İki tür ölçüm vardır:

* Sahnenin arkasındaki [günlük tabanlı ölçümler,](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#log-based-metrics) depolanan olaylardaki [Kusto sorgularına](https://docs.microsoft.com/azure/kusto/query/) çevrilir.
* [Standart ölçümler](../../azure-monitor/app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics) önceden toplanmış zaman serisi olarak depolanır.

Standart ölçümler toplama sırasında önceden toplanmış olduğundan, sorgu sırasında daha iyi performansa *sahiptirler.* Bu onları pano ve gerçek zamanlı uyarı için daha iyi bir seçim yapar. *Günlük tabanlı ölçümler* daha fazla boyuta sahiptir ve bu da onları veri analizi ve geçici tanılama için üstün seçenek yapar. Ölçümler gezgininde günlük tabanlı ve standart ölçümler arasında geçiş yapmak için [ad alanı seçicisini](metrics-getting-started.md#create-your-first-metric-chart) kullanın. [metrics explorer](metrics-getting-started.md)

## <a name="interpret-and-use-queries-from-this-article"></a>Bu makaledeki sorguları yorumlama ve kullanma

Bu makalede, desteklenen toplamave boyutları ile ölçümleri listeler. Günlük tabanlı ölçümlerle ilgili ayrıntılar, temel Kusto sorgu ekstrelerini içerir. Kolaylık sağlamak için, her sorgu, zaman parçalı, grafik türü ve bazen de değişiklik gerekmeden Log Analytics'te sorguyu kullanarak basitleştiren boyut bölme için varsayılanları kullanır.

[Ölçümler explorer'da](metrics-getting-started.md)aynı ölçüt çizdiğinizde, varsayılan yoktur - sorgu grafik ayarlarınıza göre dinamik olarak ayarlanır:

- Seçili **Zaman aralığı,** yalnızca seçilen zaman aralığından olayları seçmek için ek bir *zaman damgası...* yan tümcesine çevrilir. Örneğin, en son 24 saat için veri gösteren bir grafik, sorgu *| nerede zaman damgası > önce (24 saat)* içerir.

- Seçilen **Zaman parçalı son** özetlemek içine konur *... bin(zaman damgası, [zaman tanesi])* maddesi ile.

- Seçili **Filtre** boyutları ek *yan* tümcelere çevrilir.

- Seçili **Bölünmüş grafik** boyutu, fazladan özetlenebilir özelliğe çevrilir. Örneğin, grafiğinizi *konuma*göre bölerseniz ve 5 dakikalık bir zaman ayırıcısını kullanarak çizim ilerlerseniz, *özetlenen* yan tümce özetlenir... * bin (zaman damgası, 5 m), yer*.

> [!NOTE]
> Kusto sorgu dilinde yeniyseniz, kusto ifadelerini kopyalayıp Log Analytics sorgu bölmesine herhangi bir değişiklik yapmadan yapıştırarak başlarsınız. Temel grafiği görmek için **Çalıştır'ı** tıklatın. Sorgu dilinin sözdizimini anlamaya başladığınızda, küçük değişiklikler yapmaya başlayabilir ve değişikliğinizin etkisini görebilirsiniz. Kendi verilerinizi [keşfetmek, Log Analytics](../../azure-monitor/log-query/get-started-portal.md) ve [Azure Monitor'un](../../azure-monitor/overview.md)tüm gücünü gerçekleştirmeye başlamak için harika bir yoldur.

## <a name="availability-metrics"></a>Kullanılabilirlik ölçümleri

Kullanılabilirlik kategorisindeki ölçümler, web uygulamanızın durumunu dünyanın dört bir yanındaki noktalardan gözlendiği gibi görmenizi sağlar. [Kullanılabilirlik testlerini](../../azure-monitor/app/monitor-web-app-availability.md) bu kategorideki ölçümleri kullanmaya başlayacak şekilde yapılandırın.

### <a name="availability-availabilityresultsavailabilitypercentage"></a>Kullanılabilirlik (availabilitySonuçlar/kullanılabilirlikPercentage)
*Kullanılabilirlik* ölçümü, web test çalıştırmalarının herhangi bir sorun algılamamış yüzdesini gösterir. Mümkün olan en düşük değer 0'dır ve bu da tüm web test çalıştırmalarının başarısız olduğunu gösterir. 100 değeri, tüm web test çalıştırmalarının doğrulama ölçütlerini geçtiği anlamına gelir.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|---|---|---|
|Yüzde|Ortalama|Çalışma konumu, Test adı|

```Kusto
availabilityResults 
| summarize sum(todouble(success == 1) * 100) / count() by bin(timestamp, 5m), location
| render timechart
```

### <a name="availability-test-duration-availabilityresultsduration"></a>Kullanılabilirlik test süresi (availabilityResults/duration)

*Kullanılabilirlik test süresi* ölçümü, web testinin çalışması için ne kadar zaman aldığını gösterir. Çok [adımlı web testleri](../../azure-monitor/app/availability-multistep.md)için metrik, tüm adımların toplam yürütme süresini yansıtır.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|---|---|---|
|Milisaniye|Ortalama, Min, Max|Çalışma yeri, Test adı, Test sonucu

```Kusto
availabilityResults
| where notempty(duration)
| extend availabilityResult_duration = iif(itemType == 'availabilityResult', duration, todouble(''))
| summarize sum(availabilityResult_duration)/sum(itemCount) by bin(timestamp, 5m), location
| render timechart
```

### <a name="availability-tests-availabilityresultscount"></a>Kullanılabilirlik testleri (availabilityResults/count)

*Kullanılabilirlik testleri* ölçümü, Azure Monitor tarafından çalıştırılan web testlerinin sayısını yansıtır.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|---|---|---|
|Sayı|Sayı|Çalışma yeri, Test adı, Test sonucu|

```Kusto
availabilityResults
| summarize sum(itemCount) by bin(timestamp, 5m)
| render timechart
```

## <a name="browser-metrics"></a>Tarayıcı ölçümleri

Tarayıcı ölçümleri, Application Insights JavaScript SDK tarafından gerçek son kullanıcı tarayıcılarından toplanır. Kullanıcılarınızın web uygulamanızla ilgili deneyimleri hakkında harika bilgiler sağlarlar. Tarayıcı ölçümleri genellikle örneklenmemiştir, bu da örnekleme yoluyla çarpıtılabilecek sunucu tarafındaki ölçümlere kıyasla kullanım sayılarında daha yüksek hassasiyet sağladıkları anlamına gelir.

> [!NOTE]
> Tarayıcı ölçümlerini toplamak için uygulamanızın Application [Insights JavaScript SDK](../../azure-monitor/app/javascript.md)ile birlikte çalınması gerekir.

### <a name="browser-page-load-time-browsertimingstotalduration"></a>Tarayıcı sayfası yükleme süresi (browserTimings/totalDuration)

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|
|---|---|---|
|Milisaniye|Ortalama, Min, Max|None|

```Kusto
browserTimings
| where notempty(totalDuration)
| extend _sum = totalDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="client-processing-time-browsertimingprocessingduration"></a>İstemci işlem süresi (browserTiming/processingDuration)

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|
|---|---|---|
|Milisaniye|Ortalama, Min, Max|None|

```Kusto
browserTimings
| where notempty(processingDuration)
| extend _sum = processingDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum)/sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="page-load-network-connect-time-browsertimingsnetworkduration"></a>Sayfa yükleme ağı bağlanma süresi (browserTimings/networkDuration)

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|
|---|---|---|
|Milisaniye|Ortalama, Min, Max|None|

```Kusto
browserTimings
| where notempty(networkDuration)
| extend _sum = networkDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="receiving-response-time-browsertimingsreceiveduration"></a>Yanıt süresi alma (browserTimings/receiveDuration)

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|
|---|---|---|
|Milisaniye|Ortalama, Min, Max|None|

```Kusto
browserTimings
| where notempty(receiveDuration)
| extend _sum = receiveDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

### <a name="send-request-time-browsertimingssendduration"></a>İstek süresi gönderme (browserTimings/sendDuration)

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|
|---|---|---|
|Milisaniye|Ortalama, Min, Max|None|

```Kusto
browserTimings
| where notempty(sendDuration)
| extend _sum = sendDuration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render timechart
```

## <a name="failure-metrics"></a>Hata ölçümleri

**Hatalar'daki** ölçümler, işleme istekleri, bağımlılık çağrıları ve atılan özel durumlarla ilgili sorunları gösterir.

### <a name="browser-exceptions-exceptionsbrowser"></a>Tarayıcı özel durumları (özel durumlar/tarayıcı)

Bu metrik, tarayıcıda çalışan uygulama kodundan atılan özel durumların sayısını yansıtır. Yalnızca ```trackException()``` Uygulama Öngörüleri API çağrısıyla izlenen özel durumlar metrikte yer alıyor.

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|Notlar|
|---|---|---|---|
|Sayı|Sayı|None|Günlük tabanlı **sürüm, Toplam** toplama yı kullanır|

```Kusto
exceptions
| where notempty(client_Browser)
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="dependency-call-failures-dependenciesfailed"></a>Bağımlılık çağrı hataları (bağımlılıklar/başarısız)

Başarısız bağımlılık çağrılarının sayısı.

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|Notlar|
|---|---|---|---|
|Sayı|Sayı|None|Günlük tabanlı **sürüm, Toplam** toplama yı kullanır|

```Kusto
dependencies
| where success == 'False'
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="exceptions-exceptionscount"></a>Özel durumlar (özel durumlar/sayım)

Uygulama Öngörüleri için bir özel durum günlüğe her zaman, SDK'nın [trackException() yöntemine](../../azure-monitor/app/api-custom-events-metrics.md#trackexception) bir çağrı vardır. Özel Durumlar ölçümü günlüğe kaydedilmiş özel durumlar sayısını gösterir.

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|Notlar|
|---|---|---|---|
|Sayı|Sayı|Bulut rol adı, Bulut rol örneği, Aygıt türü|Günlük tabanlı **sürüm, Toplam** toplama yı kullanır|

```Kusto
exceptions
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="failed-requests-requestsfailed"></a>Başarısız istekler (istekler/başarısız)

*Başarısız*olarak işaretlenmiş izlenen sunucu isteklerinin sayısı. Varsayılan olarak, Application Insights SDK, HTTP yanıt kodu 5xx veya 4xx'i başarısız bir istek olarak döndüren her sunucu isteğini otomatik olarak işaretler. Bu mantığı, istek telemetri öğesinin *başarı* özelliğini özel bir [telemetri başharfinde](../../azure-monitor/app/api-filtering-sampling.md#addmodify-properties-itelemetryinitializer)değiştirerek özelleştirebilirsiniz.

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|Notlar|
|---|---|---|---|
|Sayı|Sayı|Bulut rolü örneği, Bulut rol adı, Gerçek veya sentetik trafik, İstek performansı, Yanıt kodu|Günlük tabanlı **sürüm, Toplam** toplama yı kullanır|

```Kusto
requests
| where success == 'False'
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="server-exceptions-exceptionsserver"></a>Sunucu özel durumları (özel durumlar/sunucu)

Bu metrik, sunucu özel durumlarının sayısını gösterir.

|Ölçü birimi|Desteklenen toplamalar|Önceden birleştirilmiş boyutlar|Notlar|
|---|---|---|---|
|Sayı|Sayı|Bulut rol adı, Bulut rol örneği|Günlük tabanlı **sürüm, Toplam** toplama yı kullanır|

```Kusto
exceptions
| where isempty(client_Browser)
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

## <a name="performance-counters"></a>Performans sayaçları

[Application Insights tarafından toplanan sistem performans sayaçlarına](../../azure-monitor/app/performance-counters.md)erişmek için **Performans sayaçları** kategorisindeki ölçümleri kullanın.

### <a name="available-memory-performancecountersavailablememory"></a>Kullanılabilir bellek (performanceCounters/availableMemory)

```Kusto
performanceCounters
| where ((category == "Memory" and counter == "Available Bytes") or name == "availableMemory")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="exception-rate-performancecountersexceptionrate"></a>Özel durum oranı (performanceCounters/exceptionRate)

```Kusto
performanceCounters
| where ((category == ".NET CLR Exceptions" and counter == "# of Exceps Thrown / sec") or name == "exceptionRate")
| extend performanceCounter_value = iif(itemType == 'performanceCounter',value,todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-request-execution-time-performancecountersrequestexecutiontime"></a>HTTP isteği yürütme süresi (performanceCounters/requestExecutionTime)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Request Execution Time") or name == "requestExecutionTime")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-request-rate-performancecountersrequestspersecond"></a>HTTP istek oranı (performanceCounters/requestsPerSecond)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Requests/Sec") or name == "requestsPerSecond")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="http-requests-in-application-queue-performancecountersrequestsinqueue"></a>Uygulama kuyruğundaki HTTP istekleri (performanceCounters/requestsInQueue)

```Kusto
performanceCounters
| where ((category == "ASP.NET Applications" and counter == "Requests In Application Queue") or name == "requestsInQueue")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-cpu-performancecountersprocesscpupercentage"></a>İşlem CPU 'su (performanceSayaçlar/prosesCpuPercentage)

Metrik, izlenen uygulamanızı barındıran işlem tarafından toplam işlemci kapasitesinin ne kadarının tüketilir olduğunu gösterir.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|
|Yüzde|Ortalama, Min, Max|Bulut rolü örneği

```Kusto
performanceCounters
| where ((category == "Process" and counter == "% Processor Time Normalized") or name == "processCpuPercentage")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-io-rate-performancecountersprocessiobytespersecond"></a>Proses IO oranı (performanceCounters/processIOBytesPerSecond)

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|
|Saniyede bayt|Ortalama, Min, Max|Bulut rolü örneği

```Kusto
performanceCounters
| where ((category == "Process" and counter == "IO Data Bytes/sec") or name == "processIOBytesPerSecond")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="process-private-bytes-performancecountersprocessprivatebytes"></a>Özel baytişlemleri işleme (performanceCounters/processPrivateBytes)

İzlenen işlemin verileri için ayırdığı paylaşılmayan bellek miktarı.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|
|Bayt|Ortalama, Min, Max|Bulut rolü örneği

```Kusto
performanceCounters
| where ((category == "Process" and counter == "Private Bytes") or name == "processPrivateBytes")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

### <a name="processor-time-performancecountersprocessorcpupercentage"></a>İşlemci süresi (performanceCounters/processorCpuPercentage)

Izlenen sunucu örneğinde çalışan *tüm* işlemler tarafından CPU tüketimi.

|Ölçü birimi|Desteklenen toplamalar|Desteklenen boyutlar|
|---|---|---|
|Yüzde|Ortalama, Min, Max|Bulut rolü örneği

>[!NOTE]
> İşlemci zaman ölçümü Azure Uygulama Hizmetleri'nde barındırılan uygulamalar için kullanılamaz. Uygulama Hizmetlerinde barındırılan web uygulamalarının CPU kullanımını izlemek için [İşlem CPU](#process-cpu-performancecountersprocesscpupercentage) metrik'ini kullanın.

```Kusto
performanceCounters
| where ((category == "Processor" and counter == "% Processor Time") or name == "processorCpuPercentage")
| extend performanceCounter_value = iif(itemType == "performanceCounter", value, todouble(''))
| summarize sum(performanceCounter_value) / count() by bin(timestamp, 1m)
| render timechart
```

## <a name="server-metrics"></a>Sunucu ölçümleri

### <a name="dependency-calls-dependenciescount"></a>Bağımlılık çağrıları (bağımlılıklar/sayım)

Bu metrik bağımlılık çağrılarının sayısı ile ilgilidir.

```Kusto
dependencies
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="dependency-duration-dependenciesduration"></a>Bağımlılık süresi (bağımlılıklar/süre)

Bu metrik bağımlılık çağrılarının süresini ifade eder.

```Kusto
dependencies
| where notempty(duration)
| extend dependency_duration = iif(itemType == 'dependency',duration,todouble(''))
| extend _sum = dependency_duration
| extend _count = itemCount
| extend _sum = _sum*_count
| summarize sum(_sum)/sum(_count) by bin(timestamp, 1m)
| render timechart
```

### <a name="server-requests-requestscount"></a>Sunucu istekleri (istekler/sayım)

Bu metrik, web uygulamanız tarafından alınan gelen sunucu isteklerinin sayısını yansıtır.

```Kusto
requests
| summarize sum(itemCount) by bin(timestamp, 5m)
| render barchart
```

### <a name="server-response-time-requestsduration"></a>Sunucu yanıt süresi (istekler/süre)

Bu metrik, sunucuların gelen istekleri işlemesi için geçen süreyi yansıtır.

```Kusto
requests
| where notempty(duration)
| extend request_duration = iif(itemType == 'request', duration, todouble(''))
| extend _sum = request_duration
| extend _count = itemCount
| extend _sum = _sum*_count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 1m)
| render timechart
```

## <a name="usage-metrics"></a>Ölçümleri kullanma

### <a name="page-view-load-time-pageviewsduration"></a>Sayfa görünümü yükleme süresi (sayfa Görünümleri/süresi)

Bu metrik, PageView olaylarının yüklenmesi için geçen süreyi ifade eder.

```Kusto
pageViews
| where notempty(duration)
| extend pageView_duration = iif(itemType == 'pageView', duration, todouble(''))
| extend _sum = pageView_duration
| extend _count = itemCount
| extend _sum = _sum * _count
| summarize sum(_sum) / sum(_count) by bin(timestamp, 5m)
| render barchart
```

### <a name="page-views-pageviewscount"></a>Sayfa görünümleri (sayfa Görünümleri/sayısı)

TrackPageView() Application Insights API ile günlüğe kaydedilen PageView olaylarının sayısı.

```Kusto
pageViews
| summarize sum(itemCount) by bin(timestamp, 1h)
| render barchart
```

### <a name="sessions-sessionscount"></a>Oturumlar (oturumlar/sayım)

Bu metrik, farklı oturum kimliklerinin sayısını ifade eder.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(session_Id)
| summarize dcount(session_Id) by bin(timestamp, 1h)
| render barchart
```

### <a name="traces-tracescount"></a>İzler (izlemeler/sayım)

TrackTrace() Application Insights API çağrısıyla günlüğe kaydedilen izleme ifadelerinin sayısı.

```Kusto
traces
| summarize sum(itemCount) by bin(timestamp, 1h)
| render barchart
```

### <a name="users-userscount"></a>Kullanıcılar (kullanıcılar/sayım)

Uygulamanıza erişen farklı kullanıcı sayısı. Bu ölçümün doğruluğu, telemetri örneklemesi ve filtreleme kullanılarak önemli ölçüde etkilenebilir.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(user_Id)
| summarize dcount(user_Id) by bin(timestamp, 1h)
| render barchart
```

### <a name="users-authenticated-usersauthenticated"></a>Kullanıcılar, Kimlik Doğrulaması (kullanıcılar/kimlik doğrulaması)

Uygulamanızda kimlik doğrulaması yapılan farklı kullanıcı sayısı.

```Kusto
union traces, requests, pageViews, dependencies, customEvents, availabilityResults, exceptions, customMetrics, browserTimings
| where notempty(user_AuthenticatedId)
| summarize dcount(user_AuthenticatedId) by bin(timestamp, 1h)
| render barchart
```
