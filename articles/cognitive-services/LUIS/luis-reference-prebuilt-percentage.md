---
title: Yüzde Önceden Oluşturulmuş Varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makalede, Dil Anlama (LUIS) içinde yüzde önceden oluşturulmuş varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 31ea1c36139abcb1e102161ad76a203073ba4dfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270507"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için önceden oluşturulmuş varlığın yüzdesi
Yüzde sayıları kesirler `3 1/2`veya yüzde olarak `2%`görünebilir. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına yüzde içeren örnek sözcükeklemeler eklemeniz gerekmez. Yüzde varlık birçok [kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-percentage"></a>Yüzde türleri
Yüzde, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-percentage-entity"></a>Önceden oluşturulmuş yüzde varlık için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`set a trigger when my stock goes up 2%`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "percentage": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "percentage": [
        2
    ],
    "$instance": {
        "percentage": [
            {
                "type": "builtin.percentage",
                "text": "2%",
                "startIndex": 36,
                "length": 2,
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

Aşağıdaki örnek, **yerleşik yüzde** varlığının çözümünü gösterir.

```json
"entities": [
    {
        "entity": "2%",
        "type": "builtin.percentage",
        "startIndex": 36,
        "endIndex": 37,
        "resolution": {
        "value": "2%"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Ordinal,](luis-reference-prebuilt-ordinal.md) [sayı](luis-reference-prebuilt-number.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
