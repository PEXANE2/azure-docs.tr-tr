---
title: Normal ifade varlık türü-LUSıS
titleSuffix: Azure Cognitive Services
description: Normal ifade, ham söylenişi metin için en iyisidir. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: b9da76a80183f353a74d43e667bf6c9219eb6c05
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74841226"
---
# <a name="regular-expression-entity"></a>Normal ifade varlığı

Normal ifade varlığı, sağladığınız normal ifade düzenine göre bir varlığı ayıklar.

Normal ifade, ham söylenişi metin için en iyisidir. Küçük büyük harf duyarlı ve kültürel bir değişken yok sayar.  Normal ifadenin eşleştirilmesi, yazım denetimi değişiklikleri karakter düzeyinde belirteci düzeyinde değil sonra uygulanır. Normal ifade çok karmaşık ise (birçok köşeli ayraç kullanma gibi), bu ifadeyi modele ekleyemezsiniz. [.Net Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığının tümünü değil, bölümünü kullanır.

**Varlık, şu durumlarda iyi bir uyum:**

* Veriler tutarlı olarak tutarlı bir çeşitlerle biçimlendirilir.
* Normal ifadede 2 ' den fazla iç içe geçme düzeyi gerekmez.

![Normal ifade varlığı](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Kullanım konuları

Normal ifadeler, eşleştirmeye beklediğinizden daha fazla eşleşemez. Buna bir örnek `one` ve `two`gibi sayısal bir kelime eşleştirmedir. Aşağıdaki Regex, sayı `one` diğer sayılarla birlikte eşleşir:

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
```

Bu Regex ifadesi Ayrıca, `phone`gibi bu sayılarla biten sözcüklerle eşleşir. Bu gibi sorunları gidermek için, Regex eşleşmelerin, hesap sözcük sınırlarına uyduğundan emin olun. Bu örnek için sözcük sınırları kullanmanın Regex ifadesi aşağıdaki Regex içinde kullanılır:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Örnek JSON

`kb[0-9]{6}`kullanırken, normal ifade varlık tanımı olarak, aşağıdaki JSON yanıtı sorgu için döndürülen normal ifade varlıklarını içeren bir örnektir:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

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


#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)


Bu, sorgu dizesinde `verbose=false` ayarlandıysa JSON olur:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Bu, sorgu dizesinde `verbose=true` ayarlandıysa JSON olur:

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

Bu [öğreticide](tutorial-regex-entity.md), **normal ifade** varlığını kullanarak, düzenli olarak biçimlendirilen verileri bir noktadan ayıklamak için bir uygulama oluşturun.