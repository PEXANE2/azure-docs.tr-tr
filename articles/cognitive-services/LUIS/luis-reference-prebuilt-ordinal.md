---
title: Ordinal Prebuilt entity - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) içinde ordinal önceden oluşturulmuş varlık bilgileri içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: bb3bb27db48255f534e873ed4e93ac62f07016af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273437"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için ordinal önceden oluşturulmuş varlık
Ordinal sayı, bir nesnenin bir küme içindeki `first` `second`sayısal `third`gösterimidir: , . Bu varlık zaten eğitilmiş olduğundan, uygulama amaçlarına yönetmelik içeren örnek sözcükler eklemeniz gerekmez. Ordinal varlık birçok [kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-ordinal"></a>Ordinal türleri
Ordinal [Recognizers-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Önceden oluşturulmuş ordinal varlık için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`Order the second option`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "ordinal": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "ordinal": [
        2
    ],
    "$instance": {
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "second",
                "startIndex": 10,
                "length": 6,
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

Aşağıdaki örnek, **yerleşik ordinal** varlığın çözünürlüğünü gösterir.

```json
"entities": [
  {
    "entity": "second",
    "type": "builtin.ordinal",
    "startIndex": 10,
    "endIndex": 15,
    "resolution": {
      "value": "2"
    }
  }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[OrdinalV2,](luis-reference-prebuilt-ordinal-v2.md)telefon [numarası](luis-reference-prebuilt-phonenumber.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
