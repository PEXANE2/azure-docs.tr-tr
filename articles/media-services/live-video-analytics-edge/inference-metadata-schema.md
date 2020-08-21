---
title: Çıkarım meta verileri şeması-Azure
description: Bu makalede, çıkarım meta verileri şeması hakkında bilgi edineceksiniz.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 6239713fc92b75b8ed026a8f04953e92a24c4596
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691950"
---
# <a name="inference-metadata-schema"></a>Çıkarım meta verileri şeması 

HTTP tabanlı sözleşmenin veya gRPC tabanlı sözleşmenin kullanılmasıyla bağımsız olarak her çıkarım nesnesi, bu konuda açıklanan nesne modelini izler.

## <a name="object-model"></a>Nesne modeli

![Nesne modeli](./media/inference-metadata-schema/object-model.png)
 
|Tür Tanımı|Açıklama|
|---|---|
|Etiket|Sonuçla ilişkili etiket veya etiket. Etiketlemeyle, etiketle ilişkilendirilmiş güvenirlik değerini de alırsınız.|
|Öznitelik|Sonuçla ilişkili ek öznitelikler. Güvenimme altyapısından ve güvenirlik değeriyle aldığınız yeni öznitelikleri ekleyebilirsiniz.|
|Öznitelik Listesi|Isteğe bağlı özniteliklerin listesi.|
|Dikdörtgen|Resmin sol üst köşesine göre dikdörtgen bölge. Gerekli Özellikler "uzunluk", "Genişlik", yükseklik "ve" kaynaktan üst kenar uzaklığı "olacaktır.|
|Sınıflandırma|Genellikle tüm örnek için geçerli olan sınıflandırıcının etiketi. "Tag" yardımı ile sonucu sınıflandırabilirsiniz.|
|Varlık|Örnekteki varlık algılandı veya tanımlandı. Bir varlık, ındısel konumlandırma altyapısı tarafından algılandığında, "Tag", algılanan ek öznitelikler ve bulunan bulunan varlık etrafında dikdörtgen bir kutunun ortak Ordinatları döndürülür.|
|Olay|Örnekte olay algılandı. Örnekte bir olay algılandığında, olayın adı ve olaya özgü özellikler döndürülür.|
|Film|Örnekte hareket algılandı. Örnekte hareket algılandığında, hareketin algılandığı dikdörtgen bir sınırlayıcı kutusunun birlikte bulunması döndürülür.|
|Metin|Metnin başlangıç ve bitiş zaman damgasıyla birlikte örnekle ilişkili metin döndürülür.|
|Diğer|Diğer genel yük bilgilerini döndürür.|

Aşağıdaki örnekte, bazı desteklenen çıkarım türlerine sahip tek bir olay yer almaktadır:

```
[ 
  // Light Detection 
  { 
    "type": "classification", 
    "subtype": "lightDetection", 
    "classification": { 
      "tag": { "value": "daylight", "confidence": 0.86 }, 
      "attributes": [ 
          { "name": "isBlackAndWhite", "value": "false", "confidence": 0.71 } 
      ] 
    } 
  }, 
 
  // Motion Detection 
  { 
    "type": "motion", 
    "subtype": "motionDetection", 
    "motion": 
    { 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Yolo V3 
  { 
    "type": "entity", 
    "subtype": "objectDetection",     
    "entity": 
    { 
      "tag": { "value": "dog", "confidence": 0.97 }, 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // Vehicle Identification 
  { 
    "type": "entity", 
    "subtype": "vehicleIdentification",     
    "entity": 
    { 
      "tag": { "value": "007-SPY", "confidence": 0.82 }, 
      "attributes":[   
        { "name": "color", "value": "black", "confidence": 0.90 }, 
        { "name": "body", "value": "coupe", "confidence": 0.87 }, 
        { "name": "make", "value": "Aston Martin", "confidence": 0.35 }, 
        { "name": "model", "value": "DBS V12", "confidence": 0.33 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    } 
  }, 
 
  // People Identification 
  { 
    "type": "entity", 
    "subtype": "peopleIdentification",     
    "entity": 
    { 
      "tag": { "value":"Erwin Schrödinger", "confidence": 0.50 }, 
      "attributes":[   
        { "name": "age", "value": "73", "confidence": 0.87 }, 
        { "name": "glasses", "value": "yes", "confidence": 0.94 } 
      ], 
      "box": { "l": 0.0, "t": 0.0, "w": 0.0, "h": 0.0 } 
    }, 
 
    // Open type coming from the gRPC Map 
    "extensions":  
    { 
      "vector": "e1xkaXNwbGF5c3R5bGUgaVxoYmFyIHtcZnJhYyB7ZH17ZHR9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSA9e1xoYXQge0h9fVx2ZXJ0IFxQc2kgKHQpXHJhbmdsZSB9KQ==", 
      "skeleton": "p1,p2,p3,p4" 
    } 
  }, 
 
  // Captions 
  {     
    "type": "text", 
    "subtype": "speechToText",   
    "text": 
    { 
      "value": "Humor 75%. Confirmed. Self-destruct sequence in T minus 10… 9…", 
      "language": "en-US", 
      "startTimestamp": 12000, 
      "endTimestamp": 13000 
    } 
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [gRPC veri sözleşmesi](grpc-data-contract.md)
- [HTTP veri sözleşmesi](http-data-contract.md)
