---
title: Azure Uygulama Öngörüleri Veri Modeli | Microsoft Dokümanlar
description: JSON'da sürekli dışa aktarılan ve filtre olarak kullanılan özellikleri açıklar.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 9891bea1d52c61197fa32fa5c0764df5450b563c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536854"
---
# <a name="application-insights-export-data-model"></a>Uygulama Öngörüleri İhracat Veri Modeli
Bu tablo, [Uygulama Öngörüleri](../../azure-monitor/app/app-insights-overview.md) SDK'larından portala gönderilen telemetrinin özelliklerini listeler.
Bu özellikleri [Sürekli Dışa Aktarma'dan](export-telemetry.md)veri çıktısında görürsünüz.
Ayrıca [Metrik Gezginve](../../azure-monitor/platform/metrics-charts.md) [Tanılama Arama'daki](../../azure-monitor/app/diagnostic-search.md)özellik filtrelerinde de görünürler.

Dikkat edilmesi gereken noktalar:

* `[0]`bu tablolarda bir dizin eklemeniz gereken yolda bir nokta gösterir; ama her zaman 0 değildir.
* Zaman süreleri bir mikrosaniyenin onda birindedir, bu yüzden 10000000 == 1 saniye.
* Tarihler ve saatler UTC'dir ve ISO formatında verilir`yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Örnek
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Bağlam
Her türlü telemetriye bağlam bölümü eşlik ediyor. Bu alanların tümü her veri noktasıyla birlikte iletilir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| context.custom.dimensions [0] |nesne [ ] |Özel özellikler parametresi tarafından ayarlanan anahtar değeri dize çiftleri. Anahtar max uzunluğu 100, değerleri max uzunluğu 1024. 100'den fazla benzersiz değerler, özellik aranabilir ancak segmentasyon için kullanılamaz. Ikey başına Max 200 tuşları. |
| context.custom.metrics [0] |nesne [ ] |Özel ölçümler parametresi ve TrackMetrics tarafından ayarlanan anahtar değer çiftleri. Anahtar max uzunluğu 100, değerleri sayısal olabilir. |
| context.data.eventTime |string |UTC |
| context.data.isSentetik |boole |İstek bir bot veya web testi nden gelmiş gibi görünür. |
| context.data.samplingRate |number |Portala gönderilen SDK tarafından oluşturulan telemetri yüzdesi. Aralık 0.0-100.0. |
| context.device |object |İstemci aygıtı |
| context.device.browser |string |Yani, Krom, ... |
| context.device.browserVersion |string |Krom 48.0, ... |
| context.device.deviceModel |string | |
| context.device.deviceName |string | |
| context.device.id |string | |
| context.device.locale |string |en-GB, de-DE, ... |
| context.device.network |string | |
| context.device.oemName |string | |
| context.device.os |string | |
| context.device.osVersion |string |Konak işletim sistemi |
| context.device.roleInstance |string |Sunucu ana bilgisayarkimliği |
| context.device.roleName |string | |
| context.device.screenResolution |string | |
| context.device.type |string |PC, Tarayıcı, ... |
| bağlam.location |object |`clientip`Türetilmiş. |
| context.location.city |string |Türetilmiş `clientip`, biliniyorsa |
| context.location.clientip |string |Son sekizgen 0'a anonimdir. |
| context.location.continent |string | |
| context.location.country |string | |
| context.location.province |string |Eyalet veya il |
| context.operation.id |string |Aynı `operation id` olan öğeler portaldaki İlgili Öğeler olarak gösterilir. `request id`Genellikle. |
| context.operation.name |string |url veya istek adı |
| context.operation.parentId |string |İç içe ilgili öğelere izin verir. |
| context.session.id |string |`Id`aynı kaynaktan bir operasyon grubu. İşlem siz 30 dakikalık bir süre oturumun sona ermesinyali. |
| context.session.isFirst |boole | |
| context.user.accountAcquisitionDate |string | |
| context.user.accountId |string | |
| context.user.anonAcquisitionDate |string | |
| context.user.anonId |string | |
| context.user.authAcquisitionDate |string |[Kimlik Doğrulaması Kullanıcı](../../azure-monitor/app/api-custom-events-metrics.md#authenticated-users) |
| context.user.authId |string | |
| context.user.isAuthenticated |boole | |
| context.user.storeRegion |string | |
| internal.data.documentVersion |string | |
| internal.data.id |string | `Unique id`bir öğe Uygulama Öngörüleri'ne yutulduğunda atanan |

## <a name="events"></a>Olaylar
[TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)tarafından oluşturulan özel olaylar.

| Yol | Tür | Notlar |
| --- | --- | --- |
| olay [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin 4&gt; = %25. |
| olay [0] adı |string |Olay adı.  Maksimum uzunluk 250. |
| olay [0] url |string | |
| olay [0] urlData.base |string | |
| olay [0] urlData.host |string | |

## <a name="exceptions"></a>Özel durumlar
Sunucuda ve tarayıcıda [özel durumları](../../azure-monitor/app/asp-net-exceptions.md) bildirir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| basicException [0] derleme |string | |
| basicException [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin 4&gt; = %25. |
| basicException [0] exceptionGroup |string | |
| basicException [0] özel durumTürü |string | |
| basicException [0] failedUserCodeMethod |string | |
| basicException [0] başarısızUserCodeAssembly |string | |
| basicException [0] handledAt |string | |
| basicException [0] hasFullStack |boole | |
| basicException [0]`id` |string | |
| basicException [0] yöntemi |string | |
| basicException [0] iletisi |string |Özel durum iletisi. Maksimum uzunluk 10k. |
| basicException [0] dışExceptionMessage |string | |
| basicException [0] dışExceptionThrownAtAssembly |string | |
| basicException [0] dışExceptionThrownAtMethod |string | |
| basicException [0] dışÖzel Durum Türü |string | |
| basicException [0] outerId |string | |
| basicException [0] parsedStack [0] derleme |string | |
| basicException [0] parsedStack [0] fileName |string | |
| basicException [0] parsedStack [0] düzeyi |integer | |
| basicException [0] parsedStack [0] hattı |integer | |
| basicException [0] parsedStack [0] yöntemi |string | |
| basicException [0] yığını |string |Maksimum uzunluk 10k |
| basicException [0] typeName |string | |

## <a name="trace-messages"></a>İletileri İzleme
[TrackTrace](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)tarafından gönderilen ve [günlük adaptörleri](../../azure-monitor/app/asp-net-trace-logs.md)tarafından .

| Yol | Tür | Notlar |
| --- | --- | --- |
| mesaj [0] loggerName |string | |
| mesaj [0] parametreleri |string | |
| mesaj [0] ham |string |Günlük iletisi, maksimum uzunluk 10k. |
| ileti [0] önem düzeyi |string | |

## <a name="remote-dependency"></a>Uzak bağımlılık
TrackDependency tarafından gönderildi. Sunucudaki [bağımlılıklara yapılan çağrıların](../../azure-monitor/app/asp-net-dependencies.md) performansını ve kullanımını ve tarayıcıdaki AJAX çağrılarını bildirmek için kullanılır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| remoteDependency [0] async |boole | |
| remoteDependency [0] baseName |string | |
| remoteDependency [0] commandName |string |Örneğin "ev/dizin" |
| remoteDependency [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin 4&gt; = %25. |
| remoteDependency [0] bağımlılıkTypeName |string |HTTP, SQL, ... |
| remoteDependency [0] durationMetric.value |number |Bağımlılık tarafından yanıt Tamamlama çağrıdan zaman |
| uzaktan Bağımlılık [0]`id` |string | |
| remoteDependency [0] adı |string |Url. Maksimum uzunluk 250. |
| remoteDependency [0] resultCode |string |HTTP bağımlılığından |
| remoteDependency [0] başarı |boole | |
| remoteDependency [0] türü |string |Http, Sql,... |
| remoteDependency [0] url |string |Maksimum uzunluk 2000 |
| remoteDependency [0] urlData.base |string |Maksimum uzunluk 2000 |
| remoteDependency [0] urlData.hashTag |string | |
| remoteDependency [0] urlData.host |string |Maksimum uzunluk 200 |

## <a name="requests"></a>İstekler
[TrackRequest](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)tarafından gönderildi. Standart modüller, sunucuda ölçülen sunucu yanıt süresini raporlar için bunu kullanır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| istek [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin: 4&gt; = 25%. |
| istek [0] durationMetric.value |number |Yanıta gelen istekten itibaren zaman. 1e7 == 1'ler |
| istek [0]`id` |string |`Operation id` |
| istek [0] adı |string |GET/POST + url tabanı.  Maksimum uzunluk 250 |
| request [0] responseCode |integer |Istemciye gönderilen HTTP yanıtı |
| istek [0] başarı |boole |Varsayılan == (yanıt Kodu &lt; 400) |
| istek [0] url |string |Ev sahibi dahil değil |
| istek [0] urlData.base |string | |
| istek [0] urlData.hashTag |string | |
| istek [0] urlData.host |string | |

## <a name="page-view-performance"></a>Sayfa Görünümü Performansı
Tarayıcı tarafından gönderildi. Bir sayfayı işlemek için kullanıcının tam görüntüleme isteğini başlatmasından (async AJAX çağrıları hariç) zamanı ölçer.

Bağlam değerleri istemci işletim sistemi ve tarayıcı sürümünü gösterir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| clientPerformance [0] clientProcess.value |integer |HTML'nin alınmasından sayfayı görüntülemeye kadar olan süre. |
| clientPerformance [0] adı |string | |
| istemciPerformance [0] networkConnection.value |integer |Bir ağ bağlantısı kurmak için zaman alınır. |
| clientPerformance [0] receiveRequest.value |integer |İsteğin gönderilmesinin sonundan yanıtolarak HTML'yi almaya kadar olan süre. |
| clientPerformance [0] sendRequest.value |integer |HTTP isteğini göndermek için alınan süre. |
| clientPerformance [0] total.value |integer |Sayfayı görüntülemek için isteği göndermeye başlama saati. |
| clientPerformance [0] url |string |Bu isteğin URL'si |
| istemciPerformans [0] urlData.base |string | |
| clientPerformance [0] urlData.hashTag |string | |
| istemciPerformans [0] urlData.host |string | |
| istemciPerformans [0] urlData.protocol |string | |

## <a name="page-views"></a>Sayfa Görüntülemeleri
TrackPageView() veya [stopTrackPage](../../azure-monitor/app/api-custom-events-metrics.md#page-views) tarafından gönderildi

| Yol | Tür | Notlar |
| --- | --- | --- |
| görünüm [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin 4&gt; = %25. |
| görünüm [0] süresiMetric.value |integer |Değer isteğe bağlı olarak trackPageView() veya startTrackPage() - stopTrackPage() tarafından ayarlanır. IstemciPerformans değerleri yle aynı değildir. |
| görünüm [0] adı |string |Sayfa başlığı.  Maksimum uzunluk 250 |
| görünüm [0] url |string | |
| görünüm [0] urlData.base |string | |
| görünüm [0] urlData.hashTag |string | |
| görünüm [0] urlData.host |string | |

## <a name="availability"></a>Kullanılabilirlik
[Kullanılabilirlik web testlerini](../../azure-monitor/app/monitor-web-app-availability.md)raporlar.

| Yol | Tür | Notlar |
| --- | --- | --- |
| kullanılabilirlik [0] availabilityMetric.name |string |availability |
| kullanılabilirlik [0] availabilityMetric.value |number |1.0 veya 0.0 |
| kullanılabilirlik [0] sayısı |integer |100/(örnekleme oranı).[sampling](../../azure-monitor/app/sampling.md) Örneğin 4&gt; = %25. |
| kullanılabilirlik [0] dataSizeMetric.name |string | |
| kullanılabilirlik [0] dataSizeMetric.value |integer | |
| kullanılabilirlik [0] durationMetric.name |string | |
| kullanılabilirlik [0] süresiMetric.value |number |Test süresi. 1e7==1'ler |
| kullanılabilirlik [0] iletisi |string |Hata tanılama |
| kullanılabilirlik [0] sonucu |string |Başarılı/Başarısız |
| kullanılabilirlik [0] runLocation |string |Coğrafi kaynak http req |
| kullanılabilirlik [0] testName |string | |
| durumu [0] testRunId |string | |
| kullanılabilirlik [0] testTimestamp |string | |

## <a name="metrics"></a>Ölçümler
TrackMetric() tarafından oluşturulur.

Metrik değer context.custom.metrics[0] bulunur.

Örneğin:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Metrik değerler hakkında
Metrik değerler, hem metrik raporlarda hem de başka yerlerde, standart bir nesne yapısıyla bildirilir. Örneğin:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Şu anda - bu gelecekte değişebilir rağmen - standart SDK `count==1` modülleri `name` bildirilen `value` tüm değerlerde ve sadece ve alanları yararlıdır. Farklı olabilecekleri tek durum, diğer parametreleri ayarladığınız kendi TrackMetric aramalarınızı yazmanız olacaktır.

Diğer alanların amacı, portaltrafiğini azaltmak için ölçümlerin SDK'da biraraya alınmasına izin vermektir. Örneğin, her metrik raporu göndermeden önce birbirini izleyen birkaç okumanın ortalamasına göre olabilirsiniz. Daha sonra min, max, standart sapma ve toplam değeri (toplam veya ortalama) hesaplar ve rapor tarafından temsil edilen okuma sayısına sayım ayarlayın.

Yukarıdaki tablolarda, nadiren kullanılan alan sayısı, min, max, stdDev ve sampledValue atladık.

Ölçümleri önceden toplamak yerine, telemetri hacmini azaltmanız gerekiyorsa [örneklemeyi](../../azure-monitor/app/sampling.md) kullanabilirsiniz.

### <a name="durations"></a>Süre
Aksi belirtilmedikçe, süreler bir mikrosaniyenin onda birinde gösterilir, böylece 100000000.0 1 saniye anlamına gelir.

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama Bilgileri](../../azure-monitor/app/app-insights-overview.md)
* [Sürekli İhracat](export-telemetry.md)
* [Kod örnekleri](export-telemetry.md#code-samples)
