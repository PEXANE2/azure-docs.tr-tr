---
title: Yaş Önceden Oluşturulmuş Varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) yaş önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 1607b80276a9c35bf0ac2f0a00bd2226e2c07f97
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270788"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>BIR LUIS uygulaması için yaş önceden oluşturulmuş varlık
Önceden oluşturulmuş yaş varlığı hem sayısal olarak hem de gün, hafta, ay ve yıl açısından yaş değerini yakalar. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına yaş içeren örnek sözlerle eklemeniz gerekmez. Yaş varlık birçok [kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-age"></a>Yaş türleri
Yaş [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-age-entity"></a>Önceden oluşturulmuş yaş varlığı için çözüm



#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "age": [
        {
            "number": 90,
            "unit": "Day"
        }
    ],
    "$instance": {
        "age": [
            {
                "type": "builtin.age",
                "text": "90 day old",
                "startIndex": 2,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor"
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[V2 yanıtı](#tab/V2)

Aşağıdaki örnek, **yerleşik çağın** varlığının çözümünü gösterir.

```json
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Para birimi,](luis-reference-prebuilt-currency.md) [datetimeV2](luis-reference-prebuilt-datetimev2.md)ve [boyut](luis-reference-prebuilt-dimension.md) varlıkları hakkında bilgi edinin.
