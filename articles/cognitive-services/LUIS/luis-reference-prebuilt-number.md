---
title: Önceden oluşturulmuş varlık sayısı-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale Language Understanding (LUSıS) içinde önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: cb97cc5b0004442e00b970202dd01f76aa971a2a
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677584"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için önceden oluşturulmuş varlık sayısı
Bilgi parçalarını anlatmak, ifade etmek ve tanımlamak için sayısal değerlerin kullanıldığı birçok yol vardır. Bu makalede, olası örneklerden yalnızca bazıları ele alınmaktadır. LUO, Kullanıcı araslarının çeşitlemelerini Yorumlar ve tutarlı sayısal değerler döndürür. Bu varlık zaten eğitiltiğinden, uygulama amaçlarını sayı içeren örnek bir değer eklemeniz gerekmez. 

## <a name="types-of-number"></a>Sayı türleri
Numara [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) GitHub deposundan yönetiliyor

## <a name="examples-of-number-resolution"></a>Sayı çözümleme örnekleri

| İfade        | Kurum   | Çözünürlük |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


Luo, döndürdüğü JSON yanıtının `resolution` alanındaki **`builtin.number`** varlığının tanınan değerini içerir.

## <a name="resolution-for-prebuilt-number"></a>Önceden oluşturulmuş numara için çözüm


#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

Aşağıdaki örnek, "iki düzine" için 24 değerinin çözümlenme durumunu içeren LUSıS 'den bir JSON yanıtı gösterir.

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "subtype": "integer",
        "value": "24"
      }
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi `false` olarak ayarlanmıştır:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ]
        }
    }
}
```

Aşağıdaki JSON `verbose` parametresi `true` olarak ayarlanmıştır:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "two dozen",
                        "startIndex": 6,
                        "length": 9,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

* * * 

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Para birimi](luis-reference-prebuilt-currency.md), [sıra sayısı](luis-reference-prebuilt-ordinal.md)ve [yüzde](luis-reference-prebuilt-percentage.md)bilgileri hakkında bilgi edinin. 
