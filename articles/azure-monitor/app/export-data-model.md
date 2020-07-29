---
title: Azure Application Insights veri modeli | Microsoft Docs
description: JSON 'da sürekli dışarı aktarma işleminden dışarı aktarılmış özellikleri açıklar ve filtre olarak kullanılır.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 29ad999c307d1c11e7a584b61d85ed73b9448cb4
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87324395"
---
# <a name="application-insights-export-data-model"></a>Application Insights veri modelini dışarı aktarma
Bu tabloda, [Application Insights](./app-insights-overview.md) SDK 'lardan portala gönderilen telemetrinin özellikleri listelenmektedir.
Bu özellikleri [sürekli dışarı aktarmanın](export-telemetry.md)veri çıkışında görürsünüz.
Ayrıca, [Ölçüm Gezgini](../platform/metrics-charts.md) ve [Tanılama aramasında](./diagnostic-search.md)Özellik filtrelerinde görünürler.

Şunlara işaret eder:

* `[0]`Bu tablolarda, dizin eklemeniz gereken yoldaki bir noktayı belirtir; Ancak her zaman 0 değildir.
* Süreler, mikro saniyenin onda biri olan süre, 10000000 = = 1 saniye.
* Tarihler ve saatler UTC biçimindedir ve ISO biçiminde verilir`yyyy-MM-DDThh:mm:ss.sssZ`

## <a name="example"></a>Örnek

```json
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
```

## <a name="context"></a>Bağlam
Tüm telemetri türlerine bir bağlam bölümü eşlik eder. Bu alanların hepsi her veri noktasıyla aktarılmaz.

| Yol | Tür | Notlar |
| --- | --- | --- |
| Context. Custom. Dimensions [0] |Object [] |Özel özellikler parametresi tarafından ayarlanan anahtar-değer dize çiftleri. Anahtar en fazla uzunluğu 100, değer en fazla uzunluk 1024. 100 ' den fazla benzersiz değer, özellik aranabilir ancak kesimde kullanılamaz. Ikey başına en fazla 200 anahtar. |
| Context. Custom. ölçümler [0] |Object [] |Özel ölçümler parametresi ve Trackölçümler tarafından ayarlanan anahtar-değer çiftleri. Anahtar en fazla uzunluğu 100, değerler sayısal olabilir. |
| Context. Data. eventTime |dize |UTC |
| Context. Data. ısyapay |boolean |İstek bir bot veya Web testinde geliyor gibi görünüyor. |
| Context. Data. samplingRate |sayı |Portala gönderilen SDK tarafından oluşturulan telemetri yüzdesi. Aralık 0.0-100.0. |
| Context. Device |object |İstemci cihazı |
| Context. Device. Browser |dize |IE, Chrome,... |
| Context. Device. browserVersion |dize |Chrome 48,0,... |
| Context. Device. deviceModel |dize | |
| Context. Device. aygıtadı |dize | |
| context.device.id |dize | |
| Context. Device. locale |dize |en-GB, de-DE,... |
| Context. Device. Network |dize | |
| Context. Device. oemName |dize | |
| Context. Device. OS |dize | |
| Context. Device. osVersion |dize |Konak işletim sistemi |
| Context. Device. Roleınstance |dize |Sunucu konağının KIMLIĞI |
| Context. Device. roleName |dize | |
| Context. Device. screenResolution |dize | |
| Context. Device. Type |dize |BILGISAYAR, tarayıcı,... |
| Context. Location |object |Öğesinden türetilir `clientip` . |
| Context. Location. City |dize |Biliniyorsa türetilmiş `clientip` |
| Context. Location. clienentip |dize |Son sekizgen, anonim olarak 0 ' dır. |
| Context. Location. kıent |dize | |
| Context. Location. Country |dize | |
| Context. Location. eyalet |dize |Eyalet veya il |
| context.operation.id |dize |Aynı olan öğeler `operation id` portalda Ilgili öğeler olarak gösterilir. Genellikle `request id` . |
| context.operation.name |dize |URL veya istek adı |
| Context. Operation. parentID |dize |İç içe ilişkili öğelere izin verir. |
| context.session.id |dize |`Id`aynı kaynaktan gelen bir işlem grubu. İşlem olmadan 30 dakikalık bir süre, bir oturumun sonuna işaret eder. |
| Context. Session. IsFirst |boolean | |
| Context. User. accountAcquisitionDate |dize | |
| Context. User. AccountID |dize | |
| Context. User. anonAcquisitionDate |dize | |
| Context. User. anonId |dize | |
| Context. User. Authtanışisitiondate |dize |[Kimliği doğrulanmış Kullanıcı](./api-custom-events-metrics.md#authenticated-users) |
| Context. User. Authıd |dize | |
| Context. User. IsAuthenticated |boolean | |
| Context. User. storeRegion |dize | |
| internal.data.documentVersion |dize | |
| internal.data.id |dize | `Unique id`Bu, bir öğe Application Insights yapıldığında atanır |

## <a name="events"></a>Olaylar
[Trackevent ()](./api-custom-events-metrics.md#trackevent)tarafından oluşturulan özel olaylar.

| Yol | Tür | Notlar |
| --- | --- | --- |
| olay [0] sayısı |integer |100/([örnekleme](./sampling.md) hızı). Örneğin 4 = &gt; %25. |
| olay [0] adı |dize |Olay adı.  Maksimum uzunluk 250. |
| olay [0] URL 'si |dize | |
| olay [0] urlData. Base |dize | |
| olay [0] urlData. Host |dize | |

## <a name="exceptions"></a>Özel durumlar
Sunucudaki ve tarayıcıdaki [özel durumları](./asp-net-exceptions.md) raporlar.

| Yol | Tür | Notlar |
| --- | --- | --- |
| basicException [0] derlemesi |dize | |
| basicException [0] sayısı |integer |100/([örnekleme](./sampling.md) hızı). Örneğin 4 = &gt; %25. |
| basicException [0] exceptionGroup |dize | |
| basicException [0] exceptionType |dize | |
| basicException [0] failedUserCodeMethod |dize | |
| basicException [0] failedUserCodeAssembly |dize | |
| basicException [0] handledAt |dize | |
| basicException [0] hasFullStack |boolean | |
| basicException [0]`id` |dize | |
| basicException [0] yöntemi |dize | |
| basicException [0] iletisi |dize |Özel durum iletisi. En fazla uzunluk 10.000. |
| basicException [0] outerExceptionMessage |dize | |
| basicException [0] outerExceptionThrownAtAssembly |dize | |
| basicException [0] outerExceptionThrownAtMethod |dize | |
| basicException [0] outerExceptionType |dize | |
| basicException [0] outerId |dize | |
| basicException [0] parsedStack [0] derlemesi |dize | |
| basicException [0] parsedStack [0] dosya adı |dize | |
| basicException [0] parsedStack [0] düzeyi |integer | |
| basicException [0] parsedStack [0] satırı |integer | |
| basicException [0] parsedStack [0] yöntemi |dize | |
| basicException [0] yığını |dize |En fazla uzunluk 10.000 |
| basicException [0] typeName |dize | |

## <a name="trace-messages"></a>İzleme Iletileri
[Tracktrace](./api-custom-events-metrics.md#tracktrace)tarafından ve [günlük bağdaştırıcıları](./asp-net-trace-logs.md)tarafından gönderilir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| ileti [0] loggerName |dize | |
| ileti [0] Parametreler |dize | |
| ileti [0] RAW |dize |Günlük iletisi, en fazla uzunluk 10.000. |
| ileti [0] severityLevel |dize | |

## <a name="remote-dependency"></a>Uzak bağımlılık
TrackDependency tarafından gönderildi. Sunucudaki [bağımlılıklara yapılan çağrıların](./asp-net-dependencies.md) performansını ve kullanımını ve tarayıcıdaki Ajax çağrılarını raporlamak için kullanılır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| remoteDependency bağımlılığı [0] zaman uyumsuz |boolean | |
| remoteDependency bağımlılığı [0] baseName |dize | |
| remoteDependency bağımlılığı [0] commandName |dize |Örneğin, "giriş/Dizin" |
| remoteDependency bağımlılığı [0] sayısı |integer |100/([örnekleme](./sampling.md) hızı). Örneğin 4 = &gt; %25. |
| remoteDependency bağımlılığı [0] dependencyTypeName |dize |HTTP, SQL,... |
| remoteDependency bağımlılığı [0] durationMetric. Value |sayı |Çağrıya göre yanıtın tamamlanmasına yönelik çağrıdan geçen süre |
| remoteDependency bağımlılığı [0]`id` |dize | |
| remoteDependency bağımlılığı [0] adı |dize |'Deki. Maksimum uzunluk 250. |
| remoteDependency bağımlılığı [0] resultCode |dize |HTTP bağımlılığıyla |
| remoteDependency bağımlılığı [0] başarılı |boolean | |
| remoteDependency bağımlılığı [0] tür |dize |Http, SQL,... |
| remoteDependency bağımlılığı [0] URL |dize |Maksimum uzunluk 2000 |
| remoteDependency bağımlılığı [0] urlData. Base |dize |Maksimum uzunluk 2000 |
| remoteDependency bağımlılığı [0] urlData. hashTag |dize | |
| remoteDependency bağımlılığı [0] urlData. Host |dize |Maksimum uzunluk 200 |

## <a name="requests"></a>İstekler
[Trackrequest](./api-custom-events-metrics.md#trackrequest)tarafından gönderildi. Standart modüller bunu, sunucuda ölçülen sunucu yanıt süresini raporlar olarak kullanır.

| Yol | Tür | Notlar |
| --- | --- | --- |
| istek [0] sayısı |integer |100/([örnekleme](./sampling.md) hızı). Örneğin: 4 = &gt; %25. |
| istek [0] durationMetric. Value |sayı |İsteğin yanıt gelme süresi. 1E7 = = 1s |
| istek [0]`id` |dize |`Operation id` |
| istek [0] adı |dize |Al/postala + URL tabanı.  Maksimum uzunluk 250 |
| istek [0] yanıt kodu |integer |İstemciye gönderilen HTTP yanıtı |
| istek [0] başarılı |boolean |Varsayılan = = (responseCode &lt; 400) |
| istek [0] URL 'si |dize |Konak dahil değildir |
| istek [0] urlData. Base |dize | |
| istek [0] urlData. hashTag |dize | |
| istek [0] urlData. Host |dize | |

## <a name="page-view-performance"></a>Sayfa görünümü performansı
Tarayıcı tarafından gönderilir. Bir sayfayı işleme süresini ölçer, kullanıcıdan isteği başlatan (zaman uyumsuz AJAX çağrıları hariç).

Bağlam değerleri istemci işletim sistemini ve tarayıcı sürümünü gösterir.

| Yol | Tür | Notlar |
| --- | --- | --- |
| clientPerformance [0] clientProcess. Value |integer |Sayfayı görüntülemek için HTML alma sonundan itibaren geçen süre. |
| clientPerformance [0] adı |dize | |
| clientPerformance [0] Ağbağlantısı. değer |integer |Ağ bağlantısı kurmak için geçen süre. |
| clientPerformance [0] receiveRequest. Value |integer |İsteğin, yanıt olarak HTML almaya yönelik gönderilme bitiş zamanı. |
| clientPerformance [0] sendRequest. Value |integer |HTTP isteğini göndermek için geçen süre. |
| clientPerformance [0] toplam. değer |integer |Sayfayı görüntülemek için isteğin gönderilmesi için başlangıç zamanı. |
| clientPerformance [0] URL 'si |dize |Bu isteğin URL 'SI |
| clientPerformance [0] urlData. Base |dize | |
| clientPerformance [0] urlData. diyez etiketi |dize | |
| clientPerformance [0] urlData. Host |dize | |
| clientPerformance [0] urlData. Protocol |dize | |

## <a name="page-views"></a>Sayfa Görüntülemeleri
TrackPageView () veya [Stoptrackpage](./api-custom-events-metrics.md#page-views) tarafından gönderildi

| Yol | Tür | Notlar |
| --- | --- | --- |
| [0] sayısını görüntüle |integer |100/([örnekleme](./sampling.md) hızı). Örneğin 4 = &gt; %25. |
| [0] durationMetric. Value görüntüle |integer |Değer, isteğe bağlı olarak trackPageView () veya startTrackPage ()-stopTrackPage () tarafından ayarlanır. ClientPerformance değerleriyle aynı değildir. |
| [0] adını görüntüle |dize |Sayfa başlığı.  Maksimum uzunluk 250 |
| [0] URL 'sini görüntüle |dize | |
| [0] urlData. Base öğesini görüntüle |dize | |
| [0] urlData. diyez etiketini görüntüle |dize | |
| [0] urlData. Host öğesini görüntüle |dize | |

## <a name="availability"></a>Kullanılabilirlik
[Kullanılabilirlik Web testlerini](./monitor-web-app-availability.md)raporlar.

| Yol | Tür | Notlar |
| --- | --- | --- |
| kullanılabilirlik [0] availabilityMetric.name |dize |availability |
| kullanılabilirlik [0] Kullanılabilirbilitymetric. değer |sayı |1,0 veya 0,0 |
| kullanılabilirlik [0] sayısı |integer |100/([örnekleme](./sampling.md) hızı). Örneğin 4 = &gt; %25. |
| kullanılabilirlik [0] dataSizeMetric.name |dize | |
| kullanılabilirlik [0] dataSizeMetric. değer |integer | |
| kullanılabilirlik [0] durationMetric.name |dize | |
| kullanılabilirlik [0] durationMetric. Value |sayı |Test süresi. 1E7 = = 1s |
| kullanılabilirlik [0] ileti |dize |Hata tanılama |
| kullanılabilirlik [0] Sonuç |dize |Başarılı/Başarısız |
| kullanılabilirlik [0] runLocation |dize |Http REQ coğrafi kaynağı |
| kullanılabilirlik [0] testName |dize | |
| kullanılabilirlik [0] TestRunId |dize | |
| kullanılabilirlik [0] test zaman damgası |dize | |

## <a name="metrics"></a>Ölçümler
TrackMetric () tarafından oluşturuldu.

Ölçüm değeri Context. Custom. ölçümler [0] içinde bulunur

Örnek:

```json
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
      ]  
    }
  }
}
```

## <a name="about-metric-values"></a>Ölçüm değerleri hakkında
Ölçüm değerleri, hem ölçüm raporlarında hem de başka bir yerde, standart bir nesne yapısıyla raporlanır. Örnek:

```json
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
```

Şu anda bu durum, standart SDK modülleriyle raporlanan tüm değerlerin gelecekte değişebilir `count==1` ve yalnızca `name` ve `value` alanları yararlı olur. Diğer parametreleri ayarladığınız kendi TrackMetric çağrılarınızı yazarsanız, farklı oldukları durumlar tek durumdur.

Diğer alanların amacı, portala gelen trafiği azaltmak için ölçümlerin SDK 'da toplanmasına izin vermaktır. Örneğin, her ölçüm raporunu göndermeden önce birkaç ardışık readusing ortalamasını alabilirsiniz. Daha sonra Min, Max, standart sapma ve toplam değeri (Sum veya Average) hesaplar ve sayısı, rapor tarafından temsil edilen okuma sayısı olarak ayarlanır.

Yukarıdaki tablolarda, nadiren kullanılan alanları say, min, Max, stdDev ve sampledValue değerlerini attık.

Ölçümleri önceden toplamak yerine, telemetri hacmini azaltmanız gerekiyorsa [örnekleme](./sampling.md) kullanabilirsiniz.

### <a name="durations"></a>Sürelerde
Aksi belirtilmedikçe, süreler bir mikro saniyenin onda temsil edildiği sürece 10000000,0, 1 saniye anlamına gelir.

## <a name="see-also"></a>Ayrıca bkz.
* [Uygulama Bilgileri](./app-insights-overview.md)
* [Sürekli dışarı aktarma](export-telemetry.md)
* [Kod örnekleri](export-telemetry.md#code-samples)

