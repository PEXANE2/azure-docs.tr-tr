---
title: Anahtar kelime önceden oluşturulmuş varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makalede, Dil Anlama (LUIS) anahtar tümcecik önceden oluşturulmuş varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 53be1b13f1e2744e143a4be0777e3a8e3135460e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270522"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>bir LUIS uygulaması için keyPhrase önceden oluşturulmuş varlık
Anahtar Cümlesi varlığı, bir sözcükten çeşitli anahtar tümcecikleri ayıklar. Uygulamaya keyPhrase içeren örnek ifadeler eklemeniz gerekmez. Anahtar Cümlesi varlığı, [metin analizi](../text-analytics/overview.md) özelliklerinin bir parçası olarak [birçok kültürde](luis-language-support.md#languages-supported) desteklenir.

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş anahtar Cümlesi varlığı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`where is the educational requirements form for the development and engineering group`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "keyPhrase": [
        "educational requirements",
        "development"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "educational requirements",
                "startIndex": 13,
                "length": 24,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "development",
                "startIndex": 51,
                "length": 11,
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

Aşağıdaki örnek, **builtin.keyPhrase** varlığının çözümünü gösterir.

```json
"entities": [
    {
        "entity": "development",
        "type": "builtin.keyPhrase",
        "startIndex": 51,
        "endIndex": 61
    },
    {
        "entity": "educational requirements",
        "type": "builtin.keyPhrase",
        "startIndex": 13,
        "endIndex": 36
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Yüzde,](luis-reference-prebuilt-percentage.md) [sayı](luis-reference-prebuilt-number.md)ve [yaş](luis-reference-prebuilt-age.md) varlıkları hakkında bilgi edinin.
