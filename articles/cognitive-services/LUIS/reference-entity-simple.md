---
title: Basit varlık türü - LUIS
titleSuffix: Azure Cognitive Services
description: Basit bir varlık, makinetarafından öğrenilen bağlamdan tek bir kavramı açıklar. Sonuçları iyileştirmek için basit bir varlık kullanırken bir tümcecik listesi ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 8b92aa6057c81ec9442372c5b85918cb92196d61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74894754"
---
# <a name="simple-entity"></a>Basit varlık

Basit bir varlık, tek bir kavramı açıklayan ve makine tarafından öğrenilen bağlamdan öğrenilen genel bir varlıktır. Basit varlıklar genellikle şirket adları, ürün adları veya diğer ad kategorileri gibi adlar olduğundan, kullanılan adların sinyalini artırmak için basit bir varlık kullanırken [bir ifade listesi](luis-concept-feature.md) ekleyin.

**Varlık iyi bir uyum zaman:**

* Veriler sürekli biçimlendirilmemiş, ancak aynı şeyi gösterir.

![basit varlık](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Örnek JSON

`Bob Jones wants 3 meatball pho`

Önceki söyleyiş, `Bob Jones` basit `Customer` bir varlık olarak etiketlenir.

Bitiş noktasından döndürülen veriler varlık adını, söyleyişten keşfedilen metni, keşfedilen metnin konumunu ve skoru içerir:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

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

#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)

Bu JSON sorgu `verbose=false` dizesinde ayarlanırsa:

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
|Basit Varlık|`Customer`|`bob jones`|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desen sözdizimini öğrenin](reference-pattern-syntax.md)