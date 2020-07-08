---
title: Seçtiğiniz AI ile canlı videoyu çözümleyin-Azure
description: Bu makalede, seçtiğiniz bir bilgisayar görüntü modeli kullanarak canlı videoyu çözümlemek için IoT Edge üzerinde canlı video analiziyle tümleştirilebilen bir IoT Edge modülü oluşturmayı öğreneceksiniz.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: 0ac2af280eefd5ce293a8be422551d5ee6f6d3f3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84261264"
---
# <a name="analyze-live-video-with-ai-of-your-choice"></a>Seçtiğiniz yapay zeka ile canlı video analiz etme

Bu makalede, seçtiğiniz bir bilgisayar görüntü modeli kullanarak canlı videoyu çözümlemek için IoT Edge üzerinde canlı video analiziyle tümleştirilebilen bir IoT Edge modülü oluşturmayı öğreneceksiniz. 

## <a name="pre-reading"></a>Önceden okuma

[Medya grafiği kavramı](media-graph-concept.md)

## <a name="media-graph-extension"></a>Medya grafiği uzantısı

IoT Edge üzerindeki canlı video analizi, medya grafiği işleme yeteneklerini bir grafik uzantısı aracılığıyla kendi Medya Analizi bileşenlerinizi genişletmenizi sağlayan bir genişletilebilirlik modeli tanımlar. Analiz bileşeniniz geleneksel görüntü işleme tekniklerinden veya bilgisayar Vision AI modellerinden kullanılabilir. Grafik uzantıları, [http uzantısı işlemci](media-graph-concept.md#http-extension-processor) düğümü bir medya grafiğine eklenerek etkinleştirilir. HTTP uzantısı işlemcisi, video çerçevelerini sizin için belirtilen bir HTTP uç noktasına geçirebilir ve bunun aracılığıyla bileşeninizin arabirimi olarak görev yapar. Bağlantı, yerel veya uzak bir uç noktaya yapılabilir ve gerekirse, kimlik doğrulama ve TLS şifrelemesi ile güvenliği sağlanmış olabilir. Ayrıca, işlemci, video çerçevelerinin ileri geçmeden önce isteğe bağlı ölçeklendirilmesine ve kodlamasına olanak sağlar.

Seçtiğiniz çıkarım modelini, ONNX, TensorFlow, PyTorch gibi kullanılabilir bir çıkarım çalışma zamanına veya kendi Docker kapsayıcısında çalıştırmayı tercih edebilirsiniz. Ayrıca, kendi kapsayıcıda bir HTTP sunucusu olsa da, görüntü Inse yeteneklerini göstermek için tercih ettiğiniz programlama dilini ve kitaplıklarını da kullanabilirsiniz. Inse kapsayıcı kapsayıcısı, en iyi performans için canlı video analizi Edge modülü ile birlikte dağıtılmalıdır ve daha sonra grafik topolojinize dahil olmak üzere HTTP uzantısı işlemcisi aracılığıyla çağrılacaktır.
Ayrıca, özel modülünüzü yapılan çağrıların sıklığı isteğe bağlı olarak bir [hareket algılayıcısı işlemcisi](media-graph-concept.md#motion-detection-processor) ve http uzantısı işlemcisine bir [kare hızı filtresi işlemcisi](media-graph-concept.md#frame-rate-filter-processor) yukarı akış eklenerek kısıtlanabilir.

Aşağıdaki diyagramda üst düzey veri akışı gösterilmektedir:

![Edge cihazı](./media/analyze-live-video-with-ai-your-choice-how-to/edge-device.png)

Bu, benzersiz iş gereksinimlerinizi karşılamak üzere seçtiğiniz AI modellerini kullanarak videoyu analiz etmenizi sağlar. AI modelleri, açık kaynaklı topluluktan veya kendi veri biliminizden veya özel bir AI sağlayıcısından olabilir.

## <a name="media-graph-http-extension-contract-definitions"></a>Medya grafiği HTTP uzantısı sözleşme tanımları

Bu bölüm, veri akışını tanımlayan HTTP sözleşmesini tanımlar.

### <a name="http-extensibility-protocol"></a>HTTP genişletilebilirlik Protokolü  

HTTP sözleşmesi aşağıdaki gibi tanımlanır:

* Modülünüzün HTTP sunucusu işlevi görür.
* IoT Edge modülündeki canlı video analizi HTTP istemcisi olarak davranır.

### <a name="request"></a>İstek

Canlı video analizi modülünden modülize gönderilen istekler aşağıdaki gibi olacaktır:

|||
|---|---|
|POST| `https://hostname/optional-path?optional-query`|
|Kabul Et|Uygulama/JSON,*/*|
|Yetkilendirme| Temel, Özet, taşıyıcı (özel üst bilgi desteği aracılığıyla)|
|İçerik Türü|image/jpeg<br/>image/png<br/>image/bmp<br/>resim/x-ham|
|İçerik Uzunluğu|Gövde uzunluğu, bayt cinsinden|
|User-Agent|Azure Media Services|
|Gövde|Desteklenen içerik türlerinden birinde ikili kodlanmış görüntü baytları.|

#### <a name="example"></a>Örnek

```
POST http://localhost:8080/inference HTTP/1.1
Host: localhost:8080
x-ms-client-request-id: d6050cd4-c9f2-42d3-9adc-53ba7e440f17
Content-Type: image/bmp
Content-Length: 519222

(Image Binary Content)
```

### <a name="response"></a>Yanıt

Modülünüzün canlı video analizi modülüne olan yanıtları şu şekilde olmalıdır

|||
|---|---|
|Durum Kodları|200 Tamam-çıkarım sonuçları bulundu<br/>204 Içerik yok-AI tarafından içerik bulunamadı<br/>400 Hatalı Istek-beklenmiyordu<br/>500 iç sunucu hatası-beklenmiyordu<br/>503 sunucu meşgul-"yeniden deneme" başlığına göre veya durum üst bilgisinde ön ayarlı olmayan varsayılan bir süre temelinde, AMS yeniden kapatılacak.|
|İçerik Türü|uygulama/json|
|İçerik Uzunluğu|    Gövde uzunluğu, bayt cinsinden|
|Gövde|Tek "ının" özelliğine sahip JSON nesnesi.|

#### <a name="example"></a>Örnek

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 468
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 17 Apr 2020 04:44:01 GMT

{
  "inferences": [
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.9048132 },
        "box": { "l": 0.42681578, "t": 0.47660735, "w": 0.019501392, "h": 0.020954132 }
      }
    },
    {
      "type": "entity",
      "entity": {
        "tag": { "value": "car", "confidence": 0.8953932 },
        "box": { "l": 0.55083525, "t": 0.4843858, "w": 0.046550274, "h": 0.046502113 }
      }
    }    
  ]
}
```

Yanıtların, aşağıda tanımlanan önceden oluşturulan şemadan sonra geçerli JSON belgeleri kullanılarak döndürülmesi önemle önerilir. Bu, diğer bileşenlerle birlikte çalışabilirliğini ve canlı video analizi modülüne eklenen olası gelecekteki özellikleri daha iyi sağlar.

Modülünüzün içerik türü "Application/JSON" olmayan bir yanıt döndürürse, canlı video analizi iletiyi temel 64 içeriği olarak kodlayıp donuk bir JSON yükü olarak seri hale getirir.

Modülünüzün içerik türü "Application/JSON" olarak bir yanıt döndürürse, ancak JSON şeması [aşağıda özetlenen çıkarım meta veri şemasını](#inference-metadata-schema)izlemediği için ileti yükü işlem hattı üzerinden iletilir, ancak birlikte çalışabilirlik azaltılır.

> [!NOTE]
> Modülünüzün herhangi bir sonuç oluşturmuyorsa, boş bir yanıt gövdesi ile HTTP 204 durum kodu (Içerik yok) döndürmelidir. Canlı video analizi bunu boş bir sonuç olarak anlayacak ve olayı ardışık düzen boyunca iletmeyecektir.

### <a name="inference-metadata-schema"></a>Çıkarım meta verileri şeması

Her çıkarım nesnesi bu üst küme şemasını izler:

```
{
  "type": "[classification|motion|entity|text|other]",
  "subtype": "",              // Free form subtype id

  // Object has *at most one of* the following
  "classification":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ]
  },

  "motion":
  {
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
    // "regionId": ""
  },

  "entity":
  {
    "tag": 
    {
      "value": "",            // Tag value
      "confidence": 0.0       // Tag confidence
    },
    "attributes":[            // Optional attributes
      {
        "name": "",           
        "value": "",          
        "confidence": 0.0     
      }
    ],
    "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
  },

  "text":
  {
    "value": "",              // Text value
    "language": "",           // Optional BCP47 Language Code (https://tools.ietf.org/html/bcp47)
    "startTimestamp": 0,      // Optional start timestamp
    "endTimestamp": 0         // Optional end timestamp
  },

  // Plus every object can have zero or more extensions
  "extensions":
  {
    "name1": "value1",
    "name2": "value2"
    // ...
  }
}
```
 
### <a name="data-contracts---class-hierarchy"></a>Veri sözleşmeleri-sınıf hiyerarşisi

![Veri sözleşmeleri-sınıf hiyerarşisi](./media/analyze-live-video-with-ai-your-choice-how-to/data-contracts.png)

### <a name="examples"></a>Örnekler  

Aşağıdaki örnekte, tüm desteklenen çıkarım türlerine sahip tek bir olay yer almaktadır:

```
{
  "inferences": [
    // Light detection
    {
      "type": "classification",
      "subtype": "lightDetection",
      "classification": {
        "tag": { "value": "daylight", "confidence": 0.86 },
        "attributes": [
            { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 }
        ]
      }
    },

    // Motion detection
    {
      "type": "motion",
      "subtype": "motionDetection",
      "motion":
      {
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Object detection
    {
      "type": "entity",
      "subtype": "objectDetection",    
      "entity":
      {
        "tag": { "value": "dog", "confidence": 0.97 },
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // Vehicle identification
    {
      "type": "entity",
      "subtype": "vehicleIdentification",    
      "entity":
      {
        "tag": { "value": "007-SPY", "confidence": 0.82 },
        "attributes":[  
          { "name": "color", "value": "black", "confidence": 0.90 },
          { "name": "body", "value": "coupe", "confidence": 0.87 },
          { "name": "make", "value": "Aston Martin", "confidence": 0.35 },
          { "name": "model", "value": "DBS V12", "confidence": 0.33 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      }
    },

    // People identification
    {
      "type": "entity",
      "subtype": "peopleIdentification",    
      "entity":
      {
        "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 },
        "attributes":[  
          { "name": "age", "value": "73", "confidence": 0.87 },
          { "name": "glasses", "value": "yes", "confidence": 0.94 }
        ],
        "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 }
      },
    }
  ]
}
```
 
## <a name="sample-http-extension-modules"></a>Örnek HTTP uzantı modülleri

[Canlı video analizi GitHub](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis)deposunda bırkaç örnek http uzantı modülü bulunabilir. Bu [video analizi örneklerinden](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) biri, nesne algılaması için bir IoT Edge modülü oluşturmak üzere [Yolov3](https://pjreddie.com/darknet/yolo/) [onnx](http://onnx.ai/) modelinin nasıl kullanılacağını gösterir. Seçtiğiniz bir AI modeliyle kendi modülünüzü oluşturmak için aynı yaklaşımı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme](troubleshoot-how-to.md)
