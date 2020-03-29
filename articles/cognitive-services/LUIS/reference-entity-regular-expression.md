---
title: Normal ifade varlık türü - LUIS
titleSuffix: Azure Cognitive Services
description: Düzenli bir ifade ham söyleyiş metni için en iyisidir. Bu durumda yok sayar ve kültürel varyant yok sayar.  Normal ifade eşleştirmesi, belirteç düzeyinde değil, karakter düzeyindeki yazım denetimi değişikliklerinden sonra uygulanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: b9da76a80183f353a74d43e667bf6c9219eb6c05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74841226"
---
# <a name="regular-expression-entity"></a>Normal ifade varlığı

Normal bir ifade varlığı, sağladığınız normal bir ifade desenine göre bir varlığı ayıklar.

Düzenli bir ifade ham söyleyiş metni için en iyisidir. Bu durumda yok sayar ve kültürel varyant yok sayar.  Normal ifade eşleştirmesi, belirteç düzeyinde değil, karakter düzeyindeki yazım denetimi değişikliklerinden sonra uygulanır. Normal ifade çok köşeli ayraç kullanmak gibi çok karmaşıksa, ifadeyi modele ekleyebilirsiniz. [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığın tamamını değil, bir kısmını kullanır.

**Varlık iyi bir uyum zaman:**

* Veriler sürekli olarak aynı zamanda tutarlı olan herhangi bir varyasyonla biçimlendirilir.
* Normal ifade iç içe geçme 2 düzeyden fazla gerekmez.

![Normal ifade varlığı](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Kullanım da göz önünde bulundurulması gerekenler

Normal ifadeler, beklediğiniz ifadelerden daha fazla eşleşebilir. Bunun bir örneği gibi sayısal kelime `one` eşleştirme `two`ve . Diğer sayılarla birlikte sayıyla `one` eşleşen aşağıdaki regex'e örnek olarak verilmiştir:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
```

Bu regex ifadesi aynı zamanda bu sayılarla `phone`biten sözcüklerle de eşleşir, örneğin. Bu gibi sorunları gidermek için, regex eşleşmelerinin sözcük sınırlarını dikkate aldığından emin olun. Bu örnek için sözcük sınırlarını kullanmak için regex aşağıdaki regex kullanılır:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Örnek JSON

Normal `kb[0-9]{6}`ifade varlık tanımı olarak kullanırken, aşağıdaki JSON yanıtı sorgu için döndürülen normal ifade varlıklarıyla örnek bir ifadedir:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmini uç nokta yanıtı](#tab/V2)

```JSON
"entities": [
  {
    "entity": "kb123456",
    "type": "KB number",
    "startIndex": 9,
    "endIndex": 16
  }
]
```


#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmini uç nokta yanıtı](#tab/V3)


Bu JSON sorgu `verbose=false` dizesinde ayarlanırsa:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Bu JSON sorgu `verbose=true` dizesinde ayarlanırsa:

```json
"entities": {
    "KB number": [
        "kb123456"
    ],
    "$instance": {
        "KB number": [
            {
                "type": "KB number",
                "text": "kb123456",
                "startIndex": 9,
                "length": 8,
                "modelTypeId": 8,
                "modelType": "Regex Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

Bu [öğreticide,](tutorial-regex-entity.md) **Düzenli İfade** varlığını kullanarak bir sözcükten sürekli biçimlendirilmiş verileri ayıklamak için bir uygulama oluşturun.