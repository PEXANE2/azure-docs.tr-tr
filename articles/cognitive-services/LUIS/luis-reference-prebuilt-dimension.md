---
title: Boyut önceden oluşturulmuş varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makalede içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS) boyut.
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
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270643"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için boyut önceden oluşturulmuş varlık
Önceden oluşturulmuş boyut varlık LUIS uygulama kültürü ne olursa olsun, Boyutlar çeşitli türlerde algılar. Bu varlık zaten eğitildi çünkü uygulama ıntents boyutlar içeren örnek Konuşma ekleme gerekmez. Boyut varlığı [birçok kültürde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="types-of-dimension"></a>Boyut türü

Boyut, [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) GitHub deposundan yönetilir.

## <a name="resolution-for-dimension-entity"></a>Boyut varlık için çözümleme

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`10 1/2 miles of cable`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON, `verbose` parametresi `false`olarak ayarlanmıştır:

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
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)
Aşağıdaki JSON, `verbose` parametresi `true`olarak ayarlanmıştır:

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

Aşağıdaki örnek, **yerleşik. Dimension** varlığının çözünürlüğünü gösterir.

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

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[E-posta](luis-reference-prebuilt-email.md), [sayı](luis-reference-prebuilt-number.md)ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar hakkında bilgi edinin.
