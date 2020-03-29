---
title: Sayı Önceden Oluşturulmuş Varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) içinde sayı önceden oluşturulmuş varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 85e3589d7467691e2b9a11879510ab980bbd875a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273459"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için önceden oluşturulmuş varlığın sayısı
Sayısal değerlerin bilgi parçalarını ölçmek, ifade etmek ve açıklamak için kullanıldığı birçok yol vardır. Bu makalede, yalnızca bazı olası örnekler kapsar. LUIS, kullanıcı söyleyişlerinin varyasyonlarını yorumlar ve tutarlı sayısal değerler döndürür. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına numara içeren örnek sözcükeklemeler eklemeniz gerekmez.

## <a name="types-of-number"></a>Sayı türleri
Numara, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) GitHub deposundan yönetilir

## <a name="examples-of-number-resolution"></a>Sayı çözümleme örnekleri

| İfade        | Varlık   | Çözüm |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      |
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS, döndürdüğü JSON yanıtı **`builtin.number`** `resolution` alanında bir varlığın tanınmışdeğerini içerir.

## <a name="resolution-for-prebuilt-number"></a>Önceden oluşturulmuş sayı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`order two dozen eggs`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "number": [
        24
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
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
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 yanıtı](#tab/V2)

Aşağıdaki örnek, LUIS'in "iki düzine" ifadesi için 24 değerinin çözümünü içeren bir JSON yanıtını gösterir.

```json
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
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Para birimi](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md), ve [yüzdesi](luis-reference-prebuilt-percentage.md)hakkında bilgi edinin.
