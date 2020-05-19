---
title: Normal ifade varlık türü-LUSıS
description: Normal ifade, ham söylenişi metin için en iyisidir. Büyük/küçük harf durumunu yoksayar ve kültürel türevini yoksayar.  Normal ifade eşleştirme, belirteç düzeyi değil, karakter düzeyinde yazım denetimi değişiklikleri yapıldıktan sonra uygulanır.
ms.topic: reference
ms.date: 04/14/2020
ms.openlocfilehash: 90260fca10fc087225f6b1286e9fa2dd6d17c836
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585615"
---
# <a name="regular-expression-entity"></a>Normal ifade varlığı

Normal ifade varlığı, sağladığınız normal ifade düzenine göre bir varlığı ayıklar.

Normal ifade, ham söylenişi metin için en iyisidir. Büyük/küçük harf durumunu yoksayar ve kültürel türevini yoksayar.  Normal ifade eşleştirme, belirteç düzeyi değil, karakter düzeyinde yazım denetimi değişiklikleri yapıldıktan sonra uygulanır. Normal ifade çok karmaşık ise (birçok köşeli ayraç kullanma gibi), bu ifadeyi modele ekleyemezsiniz. [.Net Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) kitaplığının tümünü değil, bölümünü kullanır.

**Varlık, şu durumlarda iyi bir uyum:**

* Veriler tutarlı olarak tutarlı bir çeşitlerle biçimlendirilir.
* Normal ifadede 2 ' den fazla iç içe geçme düzeyi gerekmez.

![Normal ifade varlığı](./media/luis-concept-entities/regex-entity.png)

## <a name="usage-considerations"></a>Kullanım konuları

Normal ifadeler, eşleştirmeye beklediğinizden daha fazla eşleşemez. Buna bir örnek, ve gibi sayısal kelime eşleştirmedir `one` `two` . Aşağıdaki Regex, diğer sayılarla birlikte sayıyla eşleşen aşağıdaki normal ifade örneğidir `one` :

```javascript
(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*
```

Bu Regex ifadesi, gibi bu sayılarla biten sözcüklerle da eşleşir `phone` . Bu gibi sorunları gidermek için, Regex eşleşmelerin, hesap sözcük sınırlarına uyduğundan emin olun. Bu örnek için sözcük sınırları kullanmanın Regex ifadesi aşağıdaki Regex içinde kullanılır:

```javascript
\b(plus )?(zero|one|two|three|four|five|six|seven|eight|nine)(\s+(zero|one|two|three|four|five|six|seven|eight|nine))*\b
```

### <a name="example-json"></a>Örnek JSON

Kullanırken `kb[0-9]{6}` , normal ifade varlık tanımı olarak AŞAĞıDAKI JSON yanıtı, sorgu için döndürülen normal ifade varlıklarını içeren bir örnektir:

`When was kb123456 published?`:

#### <a name="v2-prediction-endpoint-response"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

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


#### <a name="v3-prediction-endpoint-response"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)


Bu, `verbose=false` sorgu dizesinde AYARLANDıYSA JSON 'dir:

```json
"entities": {
    "KB number": [
        "kb123456"
    ]
}
```

Bu, `verbose=true` sorgu dizesinde AYARLANDıYSA JSON 'dir:

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

Varlıklar hakkında daha fazla bilgi edinin:

* [Kavramlar](luis-concept-entity-types.md)
* [Oluşturma](luis-how-to-add-entities.md)