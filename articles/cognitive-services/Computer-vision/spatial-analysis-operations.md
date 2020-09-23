---
title: Uzamsal analiz işlemleri
titleSuffix: Azure Cognitive Services
description: Uzamsal analiz işlemleri.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: aahi
ms.openlocfilehash: 5955bc35acfaf8f877e68db083871c353a3ce326
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90984920"
---
# <a name="spatial-analysis-operations"></a>Uzamsal analiz işlemleri

Uzamsal analiz, kamera cihazlarından gerçek zamanlı akış videosunun analizine izin vermez. Yapılandırdığınız her kamera cihazında, uzamsal analize yönelik işlemler, Azure IoT Hub örneğinizle gönderilen JSON iletilerinin çıktı akışını oluşturacaktır. 

Uzamsal analiz kapsayıcısı aşağıdaki işlemleri uygular:

| İşlem tanımlayıcısı| Açıklama|
|---------|---------|
| biliveservices. Vision. spatialanalysis-PersonCount | Kameranın görünüm alanındaki belirli bir bölgedeki kişileri sayar. <br> Sayı değiştiğinde ilk bir _personcountevent_ olayını ve sonra da _personcountevent_ olaylarını yayar.  |
| biliveservices. Vision. spatialanalysis-personcrossingline | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br>Kişi çizgiyi kesen ve yön bilgisi sağlayan bir _Personlineevent_ olayı yayar. 
| biliveservices. Vision. spatialanalysis-personcrossingçokgen | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br> Kişi bölgeyi kesen ve yönlü bilgi sağlayan bir _Personlineevent_ olayı yayar. |
| biliveservices. Vision. spatialanalysis-persondistance | İnsanlar bir uzaklık kuralını ihlal ediyor. <br> Her uzaklık ihlalinin konumuyla düzenli aralıklarla bir _personDistanceEvent_ yayar. |

Tüm yukarıdaki işlemler, `.debug` işlenmekte oldukları gibi video çerçevelerini görselleştirme özelliğine sahip olan sürümde de mevcuttur. `xhost +`Video çerçevelerinin ve olaylarının görselleştirilmesini sağlamak için ana bilgisayarda çalıştırmanız gerekir.

| İşlem tanımlayıcısı| Açıklama|
|---------|---------|
| biliveservices. Vision. spatialanalysis-PersonCount. Debug | Kameranın görünüm alanındaki belirli bir bölgedeki kişileri sayar. <br> Sayı değiştiğinde ilk bir _personcountevent_ olayını ve sonra da _personcountevent_ olaylarını yayar.  |
| biliveservices. Vision. spatialanalysis-personcrossingline. Debug | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br>Kişi çizgiyi kesen ve yön bilgisi sağlayan bir _Personlineevent_ olayı yayar. 
| biliveservices. Vision. spatialanalysis-personcrossingçokgen. Debug | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br> Kişi bölgeyi kesen ve yönlü bilgi sağlayan bir _Personlineevent_ olayı yayar. |
| biliveservices. Vision. spatialanalysis-persondistance. Debug | İnsanlar bir uzaklık kuralını ihlal ediyor. <br> Her uzaklık ihlalinin konumuyla düzenli aralıklarla bir _personDistanceEvent_ yayar. |

Uzamsal analiz, video AI modülü olarak [canlı video analizi](https://azure.microsoft.com/services/media-services/live-video-analytics/) ile de çalıştırılabilir. 

<!--more details on the setup can be found in the [LVA Setup page](LVA-Setup.md). Below is the list of the operations supported with Live Video Analytics. -->

| İşlem tanımlayıcısı| Açıklama|
|---------|---------|
| biliveservices. Vision. spatialanalysis-PersonCount. livevideoanalytics | Kameranın görünüm alanındaki belirli bir bölgedeki kişileri sayar. <br> Sayı değiştiğinde ilk bir _personcountevent_ olayını ve sonra da _personcountevent_ olaylarını yayar.  |
| biliveservices. Vision. spatialanalysis-personcrossingline. livevideoanalytics | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br>Kişi çizgiyi kesen ve yön bilgisi sağlayan bir _Personlineevent_ olayı yayar. 
| biliveservices. Vision. spatialanalysis-personcrossingçokgen. livevideoanalytics | Bir kişinin, kameranın görünüm alanındaki belirli bir satırı ne zaman kesişdiğini izler. <br> Kişi bölgeyi kesen ve yönlü bilgi sağlayan bir _Personlineevent_ olayı yayar. |
| biliveservices. Vision. spatialanalysis-persondistance. livevideoanalytics | İnsanlar bir uzaklık kuralını ihlal ediyor. <br> Her uzaklık ihlalinin konumuyla düzenli aralıklarla bir _personDistanceEvent_ yayar. |

Canlı video analizi işlemleri de `.debug` (örneğin, biliveservices. Vision. spatialanalysis-PersonCount. livevideoanalytics. Debug) sürümünde de mevcuttur. Bu, video çerçevelerini işlenen olarak görselleştirme özelliğine sahiptir. `xhost +`Video çerçevelerini ve olaylarını görselleştirmeyi etkinleştirmek için ana bilgisayarda çalıştırmanız gerekir

> [!IMPORTANT]
> Görüntü işleme AI modelleri, insan gövdenin etrafında bir sınırlayıcı kutu kullanarak video filmlerindeki ve çıkışındaki insan varlığını algılar ve bulur. AI modelleri yüzeyleri algılamaya veya kişilerin kimliklerini veya demograflarını bulmaya çalışmaz.

Bunlar, bu uzamsal analiz işlemlerinin her biri için gereken parametrelerdir.

| İşlem parametreleri| Açıklama|
|---------|---------|
| İşlem KIMLIĞI | Yukarıdaki tablodan Işlem tanımlayıcısı.|
| enabled | Boolean: true veya false|
| VIDEO_URL| Kamera cihazının RTSP URL 'si (örnek: `rtsp://username:password@url` ). Uzamsal analiz, RTSP, http veya MP4 aracılığıyla H. lanabilir kodlu akışı destekler |
| VIDEO_SOURCE_ID | Kamera cihazı veya video akışı için kolay bir ad. Bu, olay JSON çıkışıyla döndürülür.|
| VIDEO_IS_LIVE| Kamera aygıtları için doğru; Kayıtlı videolar için false.|
| VIDEO_DECODE_GPU_INDEX| Video çerçevesinin kodunu çözecek GPU. Varsayılan olarak 0 ' dır. `gpu_index`, Gibi diğer düğüm yapılandırması ile aynı olmalıdır `VICA_NODE_CONFIG` `DETECTOR_NODE_CONFIG` .|
| DETECTOR_NODE_CONFIG | Üzerinde algılayıcı düğümünün çalıştırılacağı GPU 'YU gösteren JSON. Aşağıdaki biçimde olmalıdır: `"{ \"gpu_index\": 0 }",`|
| SPACEANALYTICS_CONFIG | Aşağıda özetlenen bölge ve satır için JSON yapılandırması.|

### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-personcount"></a>Biliveservices. Vision. spatialanalysis-PersonCount için bölge yapılandırması

 Bu, bir bölgeyi yapılandıran SPACEANALYTICS_CONFIG parametresine yönelik JSON girişi örneğidir. Bu işlem için birden çok bölge yapılandırabilirsiniz.

```json
{
"zones":[{
    "name": "lobbycamera"
    "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
    "threshold": 50.00,
    "events":[{
        "type": "count",
        "config":{
            "trigger": "event",
            "output_frequency": 1
      }
    }]
}
```

| Ad | Tür| Açıklama|
|---------|---------|---------|
| `zones` | list| Bölgelerin listesi. |
| `name` | string| Bu bölge için kolay ad.|
| `polygon` | list| Her bir değer çifti, çokgenin köşeleri için x, y 'yi temsil eder. Çokgen, insanların izlenen veya sayılan olduğu alanı temsil eder ve çokgen noktaları, sol üst köşenin (0,0, 0,0) ve sağ alt köşenin (1,0, 1,0) olduğu normalleştirilmiş koordinatları (0-1) temel alır.   
| `threshold` | float| AI modellerinin güvenilirliği bu değere eşit veya ondan fazlaysa, olaylar yumurallardır. |
| `type` | string| **Biliveservices. Vision. spatialanalysis-PersonCount** için bu olmalıdır `count` .|
| `trigger` | string| Olay gönderme tetikleyicisinin türü. Desteklenen değerler, sayımdan `event` bağımsız olarak değişiklik yapılıp yapılmayacağını fark edildiğinde, sayı değiştiğinde ya da `interval` olayların düzenli aralıklarla gönderilmesi için olay göndermeye yöneliktir.
| `interval` | string| Bir olay tetiklenmadan önce kişi sayısının toplanması için gereken saniye cinsinden süre. İşlem, sahneyi sabit bir hızda çözümlemeye devam eder ve bu aralığa en sık kullanılan sayıyı döndürür. Toplama aralığı hem hem de için geçerlidir `event` `interval` .|

### <a name="line-configuration-for-cognitiveservicesvisionspatialanalysis-personcrossingline"></a>Biliveservices. Vision. spatialanalysis-personcrossingline için satır yapılandırması

Bu, bir satırı yapılandıran SPACEANALYTICS_CONFIG parametresi için bir JSON girişi örneğidir. Bu işlem için birden çok kesişen çizgi yapılandırabilirsiniz.

```json
{
"lines":[{
    "name": "doorcamera" 
    "line": {
        "start": {"x": 0, "y": 0.5},
        "end": {"x": 1, "y": 0.5}
            },
    "threshold": 50.00,
    "events":[{
        "type": "linecrossing",
        "config":{
            "trigger": "event"
            }
        }]
    }]
}
```

| Ad | Tür| Açıklama|
|---------|---------|---------|
| `lines` | list| Satır listesi.|
| `name` | string| Bu satır için kolay ad.|
| `line` | list| Çizginin tanımı. Bu bir yön çizgisi, "giriş" ve "çıkış" ile anlamanızı sağlar.|
| `start` | değer çifti| çizginin başlangıç noktası için x, y koordinatları. Float değerleri, üst, sol köşeye göre köşe konumunu temsil eder. Mutlak x, y değerlerini hesaplamak için, bu değerleri çerçeve boyutuyla çarpmanız gerekir. |
| `end` | değer çifti| çizginin bitiş noktası için x, y koordinatları. Float değerleri, üst, sol köşeye göre köşe konumunu temsil eder. Mutlak x, y değerlerini hesaplamak için, bu değerleri çerçeve boyutuyla çarpmanız gerekir. |
| `threshold` | float| AI modellerinin güvenilirliği bu değere eşit veya ondan fazlaysa, olaylar yumurallardır. |
| `type` | string| **Biliveservices. Vision. spatialanalysis-personcrossingline** için bu olmalıdır `linecrossing` .|
|`trigger`|string|Olay gönderme tetikleyicisinin türü.<br>Desteklenen değerler: "olay": bir kişi çizgiyi kesiştiğinde yangın.|

### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-personcrossingpolygon"></a>Biliveservices. Vision. spatialanalysis-personcrossingçokgen için bölge yapılandırması

Bu, bir bölgeyi yapılandıran SPACEANALYTICS_CONFIG parametresine yönelik JSON girişi örneğidir. Bu işlem için birden çok bölge yapılandırabilirsiniz.

 ```json
{
"zones":[{
    "name": "queuecamera"
    "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
    "threshold": 50.00,
    "events":[{
        "type": "zone_crossing",
        "config":{
            "trigger": "event"
            }
        }]
    }]
}
```

| Ad | Tür| Açıklama|
|---------|---------|---------|
| `zones` | list| Bölgelerin listesi. |
| `name` | string| Bu bölge için kolay ad.|
| `polygon` | list| Her bir değer çifti, poligonun köşeleri için x, y 'yi temsil eder. Çokgen, insanların izlenmekte veya dikkate alındığı alanı temsil eder. Float değerleri, üst, sol köşeye göre köşe konumunu temsil eder. Mutlak x, y değerlerini hesaplamak için, bu değerleri çerçeve boyutuyla çarpmanız gerekir. 
| `threshold` | float| AI modellerinin güvenilirliği bu değere eşit veya ondan fazlaysa, olaylar yumurallardır. |
| `type` | string| **Biliveservices. Vision. spatialanalysis-personcrossingçokgen** için bu veya olmalıdır `enter` `exit` .|
| `trigger`|string|Olay gönderme tetikleyicisinin türü<br>Desteklenen değerler: "olay": birisi bölgeden girdiğinde veya uygulamadan çıktığında yangın.|

### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-persondistance"></a>Biliveservices. Vision. spatialanalysis-persondistance için bölge yapılandırması

Bu, **biliveservices. Vision. spatialanalysis-persondistance**için bir bölgeyi yapılandıran SPACEANALYTICS_CONFIG parametresi IÇIN bir JSON girişi örneğidir. Bu işlem için birden çok bölge yapılandırabilirsiniz.

```json
{
"zones":[{
    "name": "lobbycamera",
    "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
    "threshold": 35.00,
    "events":[{
        "type": "persondistance",
        "config":{
            "trigger": "event",
            "output_frequency":1,
            "minimum_distance_threshold":6.0,
            "maximum_distance_threshold":35.0
            }
        }]
    }]
}
```

| Ad | Tür| Açıklama|
|---------|---------|---------|
| `zones` | list| Bölgelerin listesi. |
| `name` | string| Bu bölge için kolay ad.|
| `polygon` | list| Her bir değer çifti, poligonun köşeleri için x, y 'yi temsil eder. Çokgen, insanların sayıldığı ve kişiler arasındaki mesafe ölçülen alanı temsil eder. Float değerleri, üst, sol köşeye göre köşe konumunu temsil eder. Mutlak x, y değerlerini hesaplamak için, bu değerleri çerçeve boyutuyla çarpmanız gerekir. 
| `threshold` | float| AI modellerinin güvenilirliği bu değere eşit veya ondan fazlaysa, olaylar yumurallardır. |
| `type` | string| **Biliveservices. Vision. spatialanalysis-persondistance** için olmalıdır `people_distance` .|
| `trigger` | string| Olay gönderme tetikleyicisinin türü. Desteklenen değerler, sayımdan `event` bağımsız olarak değişiklik yapılıp yapılmayacağını fark edildiğinde, sayı değiştiğinde ya da `interval` olayların düzenli aralıklarla gönderilmesi için olay göndermeye yöneliktir.
| `interval` | string | Bir olay tetiklenmadan önce ihlallerin toplanması için gereken saniye cinsinden süre. Toplama aralığı hem hem de için geçerlidir `event` `interval` .|
| `output_frequency` | int | Olayların yumurdaki oranı. `output_frequency`= X olduğunda, her X olayı ise, EX olur. `output_frequency` = 2, diğer tüm olayların çıkış olduğu anlamına gelir. Output_frequency hem hem de için geçerlidir `event` `interval` .|
| `minimum_distance_threshold` | float| İnsanlar bu uzaklığa göre daha az olduğunda "Toockaybetme" olayını tetikleyen bir uzaklık.|
| `maximum_distance_threshold` | float| İnsanlar bu uzaklıktan daha büyük olduğunda bir "TooFar" olayı tetikleyen bir uzaklık.|

Bu, **biliveservices. Vision. spatialanalysis-persondistance** bölgesini yapılandıran DETECTOR_NODE_CONFIG parametresi IÇIN bir JSON girişi örneğidir.

```json
{ 
"gpu_index": 0, 
"do_calibration": true
}
```

| Ad | Tür| Description|
|---------|---------|---------|
| `gpu_index` | dize| Bu işlemin çalıştırılacağı GPU dizini.|
| `do_calibration` | string | Ayarlama özelliğinin açık olduğunu gösterir. `do_calibration`**biliveservices. Vision. spatialanalysis-persondistance** 'ın düzgün şekilde çalışması için true olması gerekir.|

Bölge ve hat yapılandırma hakkında bilgi edinmek için [kamera yerleştirme](spatial-analysis-camera-placement.md)  yönergelerine bakın.

## <a name="spatial-analysis-operation-output"></a>Uzamsal analiz Işlemi çıkışı

Her bir işlemin olayları, JSON biçiminde Azure IoT Hub için de kullanılır.

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcount-ai-insights"></a>Biliveservices. Vision. spatialanalysis-PersonCount AI Insights için JSON biçimi

Bu işlem tarafından bir olay çıkışı için örnek JSON.

```json
{
    "events": [
        {
            "id": "b013c2059577418caa826844223bb50b",
            "type": "personCountEvent",
            "detectionIds": [
                "bc796b0fc2534bc59f13138af3dd7027",
                "60add228e5274158897c135905b5a019"
            ],
            "properties": {
                "personCount": 2
            },
            "zone": "lobbycamera",
            "trigger": "event"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:06:57.224Z",
        "width": 608,
        "height": 342,
        "frameId": "1400",
        "cameraCalibrationInfo": {
            "status": "Complete",
            "cameraHeight": 10.306597709655762,
            "focalLength": 385.3199462890625,
            "tiltupAngle": 1.0969393253326416
        },
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "bc796b0fc2534bc59f13138af3dd7027",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.612683747944079,
                        "y": 0.25340268765276636
                    },
                    {
                        "x": 0.7185954043739721,
                        "y": 0.6425260577285499
                    }
                ]
            },
            "confidence": 0.9559211134910583,
            "centerGroundPoint": {
                "x": 0.0,
                "y": 0.0
            },
            "metadataType": ""
        },
        {
            "type": "person",
            "id": "60add228e5274158897c135905b5a019",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.22326200886776573,
                        "y": 0.17830915618361087
                    },
                    {
                        "x": 0.34922296122500773,
                        "y": 0.6297955429344847
                    }
                ]
            },
            "confidence": 0.9389744400978088,
            "centerGroundPoint": {
                "x": 0.0,
                "y": 0.0
            },
            "metadataType": ""
        }
    ],
    "schemaVersion": "1.0"
}
```

| Olay alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Olay Kimliği|
| `type` | string| Olay türü|
| `detectionsId` | array| Bu olayı tetikleyen kişinin benzersiz tanımlayıcısının 1 boyutlu dizisi|
| `properties` |  koleksiyonu| Değer koleksiyonu|
| `trackinId` | string| Algılanan kişinin benzersiz tanıtıcısı|
| `status` | string| ' Enter ' veya ' Exit '|
| `side` | int| Kişinin çapraz olduğu poligonun tarafının sayısı|
| `zone` | string | Çapraz olan bölgeyi temsil eden çokgenin "ad" alanı|
| `trigger` | string| Tetikleyici türü, SPACEANALYTICS_CONFIG değerine bağlı olarak ' Event ' veya ' Interval ' `trigger`|

| Algılama alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Algılama KIMLIĞI|
| `type` | string| Algılama türü|
| `region` |  koleksiyonu| Değer koleksiyonu|
| `type` | string| Bölgenin türü|
| `points` |  koleksiyonu| Bölge türü DIKDÖRTGEN olduğunda sol üst ve sağ alt noktaları |
| `confidence` | float| Algoritma güveni|

| SourceInfo alan adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Kamera KIMLIĞI|
| `timestamp` | tarih| JSON yükünün ne zaman yayıldığını UTC tarihi|
| `width` | int | Video çerçevesi genişliği|
| `height` | int | Video çerçeve yüksekliği|
| `frameId` | int | Çerçeve tanımlayıcısı|
| `cameraCallibrationInfo` |  koleksiyonu | Değer koleksiyonu|
| `status` | string | Kameranın taban düzlemi için "Tamam" olup olmadığını belirtir|
| `cameraHeight` | float | Kameranın, fit 'teki üstündeki yüksekliği. Bu, otomatik ayarlama tarafından algılanır. |
| `focalLength` | float | Kameranın piksel cinsinden odak uzunluğu. Bu, otomatik ayarlama tarafından algılanır. |
| `tiltUpAngle` | float | Kameranın eğim açısı Dikey. Bu, otomatik ayarlama tarafından algılanır.|

| SourceInfo alan adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Kamera KIMLIĞI|
| `timestamp` | tarih| JSON yükünün ne zaman yayıldığını UTC tarihi|
| `width` | int | Video çerçevesi genişliği|
| `height` | int | Video çerçeve yüksekliği|
| `frameId` | int | Çerçeve tanımlayıcısı|


### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcrossingline-ai-insights"></a>Biliveservices. Vision. spatialanalysis-personcrossingline AI öngörüleri için JSON biçimi

Bu işlem tarafından algılanan algılamalar için örnek JSON.

```json
{
    "events": [
        {
            "id": "3733eb36935e4d73800a9cf36185d5a2",
            "type": "personLineEvent",
            "detectionIds": [
                "90d55bfc64c54bfd98226697ad8445ca"
            ],
            "properties": {
                "trackingId": "90d55bfc64c54bfd98226697ad8445ca",
                "status": "CrossLeft"
            },
            "zone": "doorcamera"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:06:53.261Z",
        "width": 608,
        "height": 342,
        "frameId": "1340",
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "90d55bfc64c54bfd98226697ad8445ca",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.491627341822574,
                        "y": 0.2385801348769874
                    },
                    {
                        "x": 0.588894994635331,
                        "y": 0.6395559924387793
                    }
                ]
            },
            "confidence": 0.9005028605461121,
            "metadataType": ""
        }
    ],
    "schemaVersion": "1.0"
}
```
| Olay alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Olay Kimliği|
| `type` | string| Olay türü|
| `detectionsId` | array| Bu olayı tetikleyen kişinin benzersiz tanımlayıcısının 1 boyutlu dizisi|
| `properties` |  koleksiyonu| Değer koleksiyonu|
| `trackinId` | string| Algılanan kişinin benzersiz tanıtıcısı|
| `status` | string| Çizgi çapraz yönlerinin yönü, ' CrossLeft ' veya ' CrossRight '|
| `zone` | string | Çapraz çizginin "ad" alanı|

| Algılama alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Algılama KIMLIĞI|
| `type` | string| Algılama türü|
| `region` |  koleksiyonu| Değer koleksiyonu|
| `type` | string| Bölgenin türü|
| `points` |  koleksiyonu| Bölge türü DIKDÖRTGEN olduğunda sol üst ve sağ alt noktaları |
| `confidence` | float| Algoritma güveni|

| SourceInfo alan adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Kamera KIMLIĞI|
| `timestamp` | tarih| JSON yükünün ne zaman yayıldığını UTC tarihi|
| `width` | int | Video çerçevesi genişliği|
| `height` | int | Video çerçeve yüksekliği|
| `frameId` | int | Çerçeve tanımlayıcısı|


> [!IMPORTANT]
> AI modeli, kişinin kameraya veya uzaklaşmaya bağlı olup olmadığına bakılmaksızın bir kişi algılar. AI modeli yüz algılamayı veya tanımayı çalıştırmaz ve herhangi bir biyometrik bilgi oluşturmaz. 

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcrossingpolygon-ai-insights"></a>Biliveservices. Vision. spatialanalysis-personcrossingçokgen AI öngörüleri için JSON biçimi

Bu işlem tarafından algılanan algılamalar için örnek JSON.

```json
{
    "events": [
        {
            "id": "f095d6fe8cfb4ffaa8c934882fb257a5",
            "type": "personZoneEvent",
            "detectionIds": [
                "afcc2e2a32a6480288e24381f9c5d00e"
            ],
            "properties": {
                "trackingId": "afcc2e2a32a6480288e24381f9c5d00e",
                "status": "Enter",
                "side": ""
            },
            "zone": "queuecamera"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:15:09.680Z",
        "width": 608,
        "height": 342,
        "frameId": "428",
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "afcc2e2a32a6480288e24381f9c5d00e",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.8135572734631991,
                        "y": 0.6653949670624315
                    },
                    {
                        "x": 0.9937645761590255,
                        "y": 0.9925406829655519
                    }
                ]
            },
            "confidence": 0.6267998814582825,
            "metadataType": ""
        }
    ],
    "schemaVersion": "1.0"
}
```

| Olay alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Olay Kimliği|
| `type` | string| Olay türü|
| `detectionsId` | array| Bu olayı tetikleyen kişinin benzersiz tanımlayıcısının 1 boyutlu dizisi|
| `properties` |  koleksiyonu| Değer koleksiyonu|
| `trackinId` | string| Algılanan kişinin benzersiz tanıtıcısı|
| `status` | string| Çokgen çapraz yönlerinin yönü, ' Enter ' veya ' Exit '|
| `zone` | string | Çapraz olan bölgeyi temsil eden çokgenin "ad" alanı|

| Algılama alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Algılama KIMLIĞI|
| `type` | string| Algılama türü|
| `region` |  koleksiyonu| Değer koleksiyonu|
| `type` | string| Bölgenin türü|
| `points` |  koleksiyonu| Bölge türü DIKDÖRTGEN olduğunda sol üst ve sağ alt noktaları |
| `confidence` | float| Algoritma güveni|

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-persondistance-ai-insights"></a>Biliveservices. Vision. spatialanalysis-persondistance AI öngörüleri için JSON biçimi

Bu işlem tarafından algılanan algılamalar için örnek JSON.

```json
{
    "events": [
        {
            "id": "9c15619926ef417aa93c1faf00717d36",
            "type": "personDistanceEvent",
            "detectionIds": [
                "9037c65fa3b74070869ee5110fcd23ca",
                "7ad7f43fd1a64971ae1a30dbeeffc38a"
            ],
            "properties": {
                "personCount": 5,
                "averageDistance": 20.807043981552123,
                "minimumDistanceThreshold": 6.0,
                "maximumDistanceThreshold": "Infinity",
                "eventName": "TooClose",
                "distanceViolationPersonCount": 2
            },
            "zone": "lobbycamera",
            "trigger": "event"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:17:25.309Z",
        "width": 608,
        "height": 342,
        "frameId": "1199",
        "cameraCalibrationInfo": {
            "status": "Complete",
            "cameraHeight": 12.9940824508667,
            "focalLength": 401.2800598144531,
            "tiltupAngle": 1.057669997215271
        },
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "9037c65fa3b74070869ee5110fcd23ca",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.39988183975219727,
                        "y": 0.2719132942065858
                    },
                    {
                        "x": 0.5051516984638414,
                        "y": 0.6488402517218339
                    }
                ]
            },
            "confidence": 0.948630690574646,
            "centerGroundPoint": {
                "x": -1.4638760089874268,
                "y": 18.29732322692871
            },
            "metadataType": ""
        },
        {
            "type": "person",
            "id": "7ad7f43fd1a64971ae1a30dbeeffc38a",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.5200299714740954,
                        "y": 0.2875368218672903
                    },
                    {
                        "x": 0.6457497446160567,
                        "y": 0.6183311060855263
                    }
                ]
            },
            "confidence": 0.8235412240028381,
            "centerGroundPoint": {
                "x": 2.6310102939605713,
                "y": 18.635927200317383
            },
            "metadataType": ""
        }
    ],
    "schemaVersion": "1.0"
}
```

| Olay alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Olay Kimliği|
| `type` | string| Olay türü|
| `detectionsId` | array| Bu olayı tetikleyen kişinin benzersiz tanımlayıcısının 1 boyutlu dizisi|
| `properties` |  koleksiyonu| Değer koleksiyonu|
| `personCount` | int| Olay yayınlandığında algılanan kişilerin sayısı|
| `averageDistance` | float| Tüm algılanan kişilerin feet içinde ortalama uzaklığı|
| `minimumDistanceThreshold` | float| İnsanlar bu uzaklıktan daha az olduğunda bir "Toockaybetme" olayı tetiklenecek mesafe uzaklığı.|
| `maximumDistanceThreshold` | float| İnsanlar birbirinden uzaklıktan daha büyük olduğunda bir "TooFar" olayı tetiklenecek mesafe uzaklığı.|
| `eventName` | string| Olay adı `TooClose` `minimumDistanceThreshold` ihlal edildi, `TooFar` ne zaman ihlal edildiğinde `maximumDistanceThreshold` veya `unknown` otomatik ayarlama tamamlanmamışsa|
| `distanceViolationPersonCount` | int| Veya ihlaline göre algılanan kişilerin sayısı `minimumDistanceThreshold``maximumDistanceThreshold`|
| `zone` | string | Kişiler arasında distancing için izlenen bölgeyi temsil eden çokgenin "ad" alanı|
| `trigger` | string| Tetikleyici türü, SPACEANALYTICS_CONFIG değerine bağlı olarak ' Event ' veya ' Interval ' `trigger`|

| Algılama alanı adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Algılama KIMLIĞI|
| `type` | string| Algılama türü|
| `region` |  koleksiyonu| Değer koleksiyonu|
| `type` | string| Bölgenin türü|
| `points` |  koleksiyonu| Bölge türü DIKDÖRTGEN olduğunda sol üst ve sağ alt noktaları |
| `confidence` | float| Algoritma güveni|
| `centerGroundPoint` | 2 float değeri| `x`, `y` kişinin yüz içinde çıkarılan konumunun koordinatlarıyla birlikte değerleri. `x` , kameradan, fit 'te bulunan kamera görüntü düzlemi ile dikey mesafeden uzaklığı. `y` , kameradan, fit 'teki zemin içinde yansıtılan görüntü düzlemine paralel olarak uzaklığı.|

| SourceInfo alan adı | Tür| Description|
|---------|---------|---------|
| `id` | dize| Kamera KIMLIĞI|
| `timestamp` | tarih| JSON yükünün ne zaman yayıldığını UTC tarihi|
| `width` | int | Video çerçevesi genişliği|
| `height` | int | Video çerçeve yüksekliği|
| `frameId` | int | Çerçeve tanımlayıcısı|
| `cameraCallibrationInfo` |  koleksiyonu | Değer koleksiyonu|
| `status` | string | Kameranın taban düzlemi için "Tamam" olup olmadığını belirtir|
| `cameraHeight` | float | Kameranın, fit 'teki üstündeki yüksekliği. Bu, otomatik ayarlama tarafından algılanır. |
| `focalLength` | float | Kameranın piksel cinsinden odak uzunluğu. Bu, otomatik ayarlama tarafından algılanır. |
| `tiltUpAngle` | float | Kameranın eğim açısı Dikey. Bu, otomatik ayarlama tarafından algılanır.|


## <a name="use-the-output-generated-by-the-container"></a>Kapsayıcı tarafından oluşturulan çıktıyı kullanın

Uzamsal analiz algılama veya olaylarını uygulamanızla tümleştirmek isteyebilirsiniz. Göz önünde bulundurmanız gereken birkaç yaklaşım aşağıda verilmiştir: 

* Azure IoT Hub uç noktasına bağlanmak ve olayları almak için seçtiğiniz programlama diline yönelik Azure Event hub SDK 'sını kullanın. Daha fazla bilgi için bkz. [yerleşik uç noktadan cihazdan buluta Iletileri okuma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin) . 
* Olayları diğer uç noktalara göndermek veya olayları veri depolamaya kaydetmek için Azure IoT Hub **Ileti yönlendirmeyi** ayarlayın. Daha fazla bilgi için bkz. [IoT Hub Ileti yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) . 
* Olayları geldikçe ve görselleştirmeler oluştururken gerçek zamanlı olarak işlemek için bir Azure Stream Analytics işi ayarlayın. 

## <a name="deploying-spatial-analysis-operations-at-scale-multiple-cameras"></a>Uzamsal analiz işlemlerini ölçekte dağıtma (birden çok kamera)

GPU 'ların en iyi performansını ve kullanımını sağlamak için, grafik örnekleri kullanarak birden fazla kamera üzerinde herhangi bir uzamsal analiz işlemini dağıtabilirsiniz. Aşağıda, beş (5) kamerada biliveservices. Vision. spatialanalysis-PersonCount işlemini çalıştırmaya yönelik bir örnek verilmiştir.

```json
 "properties.desired": {
      "globalSettings": {
          "PlatformTelemetryEnabled": false,
          "CustomerTelemetryEnabled": true
      },
      "graphs": {
          "personcount": {
              "operationId": "cognitiveservices.vision.spatialanalysis-personcount",
              "version": 1,
              "enabled": true,
              "sharedNodes": {
                  "shared_detector1": {
                      "node": "PersonCountGraph.detector",
                      "parameters": {
                          "DETECTOR_NODE_CONFIG": "{ \"gpu_index\": 0, \"batch_size\": 5}",
                      }
                  }
              },
              "parameters": {
                  "VIDEO_DECODE_GPU_INDEX": 0,
                  "VIDEO_IS_LIVE": true
              },
              "instances": {
                  "1": {
                      "sharedNodeMap": {
                          "PersonCountGraph/detector": "shared_detector1"
                      },
                      "parameters": {
                          "VIDEO_URL": "<Replace RTSP URL for camera 1>",
                          "VIDEO_SOURCE_ID": "camera 1",
                          "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"zone5\",\"polygon\":[[0,0],[1,0],[0,1],[1,1],[0,0]],\"threshold\":50.0, \"events\":[{\"type\":\"count\", \"output_frequency\": 1}]}]}"
                      }
                  },
                  "2": {
                      "sharedNodeMap": {
                          "PersonCountGraph/detector": "shared_detector1"
                      },
                      "parameters": {
                          "VIDEO_URL": "<Replace RTSP URL for camera 2>",
                          "VIDEO_SOURCE_ID": "camera 2",
                          "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                      }
                  },
                  "3": {
                      "sharedNodeMap": {
                          "PersonCountGraph/detector": "shared_detector1"
                      },
                      "parameters": {
                          "VIDEO_URL": "<Replace RTSP URL for camera 3>",
                          "VIDEO_SOURCE_ID": "camera 3",
                          "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                      }
                  },
                  "4": {
                      "sharedNodeMap": {
                          "PersonCountGraph/detector": "shared_detector1"
                      },
                      "parameters": {
                          "VIDEO_URL": "<Replace RTSP URL for camera 4>",
                          "VIDEO_SOURCE_ID": "camera 4",
                          "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                      }
                  },
                  "5": {
                      "sharedNodeMap": {
                          "PersonCountGraph/detector": "shared_detector1"
                      },
                      "parameters": {
                          "VIDEO_URL": "<Replace RTSP URL for camera 5>",
                          "VIDEO_SOURCE_ID": "camera 5",
                          "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                      }
                  }
              }
          }
      }
  }
  ```
| Ad | Tür| Açıklama|
|---------|---------|---------|
| `batch_size` | int | Bu işlemde kullanılacak kamera sayısını belirtir.|

## <a name="next-steps"></a>Sonraki adımlar

* [Web uygulaması sayımını yapan bir kişi dağıtın](spatial-analysis-web-app.md)
* [Günlüğe kaydetme ve sorun giderme](spatial-analysis-logging.md)
* [Kamera Yerleştirme Kılavuzu](spatial-analysis-camera-placement.md)
* [Bölge ve satır yerleştirme Kılavuzu](spatial-analysis-zone-line-placement.md)
