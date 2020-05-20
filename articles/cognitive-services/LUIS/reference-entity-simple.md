---
title: Basit varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Basit bir varlık, makine öğrenimi bağlamından tek bir kavramı açıklar. Sonuçları geliştirmek için basit bir varlık kullanırken bir tümcecik listesi ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 1b5754be3c9941101a53f332841ace93caf9acdd
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684564"
---
# <a name="simple-entity"></a>Basit varlık

Basit bir varlık, tek bir kavramı açıklayan ve makine öğrenimi bağlamından öğrenilmiş genel bir varlıktır. Basit varlıklar genellikle şirket adları, ürün adları veya diğer ad kategorileri gibi adlar olduğundan, kullanılan adların sinyalini artırmak için basit bir varlık kullanırken bir [tümcecik listesi](luis-concept-feature.md) ekleyin.

**Varlık, şu durumlarda iyi bir uyum:**

* Veriler sürekli olarak biçimlendirilmemiştir ancak aynı şeyi gösterir.

![Basit varlık](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Örnek JSON

`Bob Jones wants 3 meatball pho`

Önceki deymede `Bob Jones` basit bir varlık olarak etiketlendi `Customer` .

Uç noktadan döndürülen veriler varlık adını, utterance 'teki bulunan metni, bulunan metnin konumunu ve puanı içerir:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

Bu, `verbose=false` sorgu dizesinde AYARLANDıYSA JSON 'dir:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Veri nesnesi|Varlık adı|Değer|
|--|--|--|
|Basit varlık|`Customer`|`bob jones`|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Model sözdizimini öğrenin](reference-pattern-syntax.md)