---
title: Ordinal V2 önceden oluşturulmuş varlık - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) ordinal V2 önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 5e852313db75e598da647ea0f985e2ee18af16de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270494"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Luis uygulaması için Ordinal V2 önceden oluşturulmuş varlık
Ordinal V2 numarası Gibi göreli referanslar sağlamak `next` `last`için `previous` [Ordinal](luis-reference-prebuilt-ordinal.md) genişletir , , ve . Bunlar, önceden oluşturulmuş bir varlık kullanılarak çıkarılmaz.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Önceden oluşturulmuş ordinal V2 varlığı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`what is the second to last choice in the list`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
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

Aşağıdaki örnek, **builtin.ordinalV2** varlığının çözünürlüğünü gösterir.

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Yüzde,](luis-reference-prebuilt-percentage.md)telefon [numarası](luis-reference-prebuilt-phonenumber.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
