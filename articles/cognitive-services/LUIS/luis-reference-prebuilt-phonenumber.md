---
title: Telefon numarası Önceden oluşturulmuş varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) içinde önceden oluşturulmuş varlık bilgilerini telefon numarası içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 4e2c8e27c6d4195252c6a5b423fa98b2a4247182
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270463"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için önceden oluşturulmuş bir varlık telefon numarası
Kuruluş, `phonenumber` ülke kodu da dahil olmak üzere çeşitli telefon numaralarını ayıklar. Bu varlık zaten eğitilmiş olduğundan, uygulamaya örnek sözeklemeniz gerekmez. Varlık `phonenumber` yalnızca kültürde `en-us` desteklenir.

## <a name="types-of-a-phone-number"></a>Telefon numarası türleri
`Phonenumber`[Recognizers-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub deposundan yönetilir

## <a name="resolution-for-this-prebuilt-entity"></a>Bu önceden oluşturulmuş varlık için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`my mobile is 1 (800) 642-7676`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "phonenumber": [
        "1 (800) 642-7676"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

```json
"entities": {
    "phonenumber": [
        "1 (800) 642-7676"
    ],
    "$instance": {

        "phonenumber": [
            {
                "type": "builtin.phonenumber",
                "text": "1 (800) 642-7676",
                "startIndex": 13,
                "length": 16,
                "score": 1.0,
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

Aşağıdaki örnek, **builtin.phonenumber** varlığının çözünürlüğünü gösterir.

```json
"entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Yüzde,](luis-reference-prebuilt-percentage.md) [sayı](luis-reference-prebuilt-number.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
