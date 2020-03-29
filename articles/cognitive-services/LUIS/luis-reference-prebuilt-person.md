---
title: PersonName önceden oluşturulmuş varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) kişi Adı önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 768c719211e8a8f2133d3798343d076e795a3da0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273428"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Bir LUIS uygulaması için PersonName önceden oluşturulmuş varlık
Önceden oluşturulmuş kişi Adı varlığı kişi adlarını algılar. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına kişi adı içeren örnek sözcükler eklemeniz gerekmez. personName varlık İngilizce ve Çin [kültürlerinde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="resolution-for-personname-entity"></a>PersonName entity için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`Is Jill Jones in Cairo?`


#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)


Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ],
    "$instance": {
        "personName": [
            {
                "type": "builtin.personName",
                "text": "Jill Jones",
                "startIndex": 3,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
    }
}
```
#### <a name="v2-response"></a>[V2 yanıtı](#tab/V2)

Aşağıdaki örnek, **builtin.personName** varlığının çözümünü gösterir.

```json
"entities": [
{
    "entity": "Jill Jones",
    "type": "builtin.personName",
    "startIndex": 3,
    "endIndex": 12
}
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[E-posta,](luis-reference-prebuilt-email.md) [numara](luis-reference-prebuilt-number.md)ve [ordinal](luis-reference-prebuilt-ordinal.md) varlıklar hakkında bilgi edinin.
