---
title: Sıralı önceden oluşturulmuş varlık-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale Language Understanding (LUSıS) içindeki sıralı önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 1fa86c8960ea9f32163ebd7991260a19ef7a5d79
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91535466"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için sıralı önceden oluşturulmuş varlık
Sıra numarası, kümesi içindeki bir nesnenin sayısal bir gösterimidir: `first` , `second` , `third` . Bu varlık zaten eğitiltiğinden, uygulama hedefleri için sıra içeren örnek bir değer eklemeniz gerekmez. Sıralı varlık [birçok kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-ordinal"></a>Sıra türleri
Sıra, [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) GitHub deposundan yönetilir

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Önceden oluşturulmuş sıralı varlık için çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`Order the second option`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `false` :

```json
"entities": {
    "ordinal": [
        2
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `true` :

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

Aşağıdaki örnek, **yerleşik. Ordinal** varlığının çözünürlüğünü gösterir.

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

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[OrdinalV2](luis-reference-prebuilt-ordinal-v2.md), [telefon numarası](luis-reference-prebuilt-phonenumber.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
