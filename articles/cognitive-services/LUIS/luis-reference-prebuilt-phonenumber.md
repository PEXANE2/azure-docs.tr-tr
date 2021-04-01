---
title: Telefon numarası önceden oluşturulmuş varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUSıS) içindeki telefon numarası önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.openlocfilehash: 598ecaddbab3b70297a460521c7ec3386b390a8d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "91535398"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için telefon numarası önceden oluşturulmuş varlık
`phonenumber`Varlık, ülke kodu dahil olmak üzere çeşitli telefon numaralarını ayıklar. Bu varlık zaten eğitiltiğinden, uygulamaya örnek eklememe eklemeniz gerekmez. `phonenumber`Varlık yalnızca kültür ' de desteklenir `en-us` .

## <a name="types-of-a-phone-number"></a>Telefon numarası türleri
`Phonenumber`[Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) GitHub deposundan yönetiliyor

## <a name="resolution-for-this-prebuilt-entity"></a>Bu önceden oluşturulmuş varlık için çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`my mobile is 1 (800) 642-7676`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `false` :

```json
"entities": {
    "phonenumber": [
        "1 (800) 642-7676"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `true` :

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

Aşağıdaki örnek, **yerleşik. PhoneNumber** varlığının çözünürlüğünü gösterir.

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

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Yüzde](luis-reference-prebuilt-percentage.md), [sayı](luis-reference-prebuilt-number.md)ve [sıcaklık](luis-reference-prebuilt-temperature.md) varlıkları hakkında bilgi edinin.
