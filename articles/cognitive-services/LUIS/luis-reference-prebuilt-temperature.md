---
title: Sıcaklık Önceden Oluşturulmuş Varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) sıcaklık önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 7e2b48c6353f56ab2269a8718146cb765797adba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270360"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için sıcaklık önceden oluşturulmuş varlık
Sıcaklık çeşitli sıcaklık türlerini ayıklar. Bu varlık zaten eğitilmiş olduğundan, uygulamaya sıcaklık içeren örnek sözcükler eklemeniz gerekmez. Sıcaklık varlığı birçok [kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-temperature"></a>Sıcaklık türleri
Sıcaklık, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-temperature-entity"></a>Önceden oluşturulmuş sıcaklık varlığı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`set the temperature to 30 degrees`


#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "temperature": [
        {
            "number": 30,
            "units": "Degree"
        }
    ],
    "$instance": {
        "temperature": [
            {
                "type": "builtin.temperature",
                "text": "30 degrees",
                "startIndex": 23,
                "length": 10,
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

Aşağıdaki örnek, **yerleşik sıcaklık** varlığının çözünürlüğünü gösterir.

```json
"entities": [
    {
        "entity": "30 degrees",
        "type": "builtin.temperature",
        "startIndex": 23,
        "endIndex": 32,
        "resolution": {
        "unit": "Degree",
        "value": "30"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Yüzde,](luis-reference-prebuilt-percentage.md) [sayı](luis-reference-prebuilt-number.md)ve [yaş](luis-reference-prebuilt-age.md) varlıkları hakkında bilgi edinin.
