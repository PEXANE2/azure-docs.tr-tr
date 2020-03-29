---
title: Dimension Prebuilt varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) boyutu önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 3c7bebb38d0331c76eeaaa1d9d6464c568a61b83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270643"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için boyut önceden oluşturulmuş varlık
Önceden oluşturulmuş boyut varlığı, LUIS uygulama kültüründen bağımsız olarak çeşitli boyut türlerini algılar. Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına boyutlar içeren örnek sözcükler eklemeniz gerekmez. Boyut varlık birçok [kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-dimension"></a>Boyut türleri

Boyut, [Tanıyanlar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) GitHub deposundan yönetilir.

## <a name="resolution-for-dimension-entity"></a>Boyut varlığı için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`10 1/2 miles of cable`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ],
    "$instance": {
        "dimension": [
            {
                "type": "builtin.dimension",
                "text": "10 1/2 miles",
                "startIndex": 0,
                "length": 12,
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

Aşağıdaki örnek, **yerleşik boyut** varlığının çözümünü gösterir.

```json
{
    "entity": "10 1/2 miles",
    "type": "builtin.dimension",
    "startIndex": 0,
    "endIndex": 11,
    "resolution": {
    "unit": "Mile",
    "value": "10.5"
    }
}
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[E-posta,](luis-reference-prebuilt-email.md) [numara](luis-reference-prebuilt-number.md)ve [ordinal](luis-reference-prebuilt-ordinal.md) varlıklar hakkında bilgi edinin.
