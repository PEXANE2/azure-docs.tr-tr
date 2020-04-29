---
title: Personbadı önceden oluşturulmuş varlık-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUSıS) ' de personName önceden oluşturulmuş varlık bilgilerini içerir.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273428"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için sorumlu adı önceden oluşturulmuş varlık
Önceden oluşturulmuş Personbname varlığı kişi adlarını algılar. Bu varlık zaten eğitiltiğinden, uygulama hedefleri için personName 'i içeren örnek bir değer eklemeniz gerekmez. personName varlığı Ingilizce ve Çince [kültürlerde](luis-reference-prebuilt-entities.md)desteklenir.

## <a name="resolution-for-personname-entity"></a>PersonName varlığına yönelik çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`Is Jill Jones in Cairo?`


#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)


Aşağıdaki JSON `verbose` parametresi olarak `false`ayarlanmıştır:

```json
"entities": {
    "personName": [
        "Jill Jones"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)
Aşağıdaki JSON `verbose` parametresi olarak `true`ayarlanmıştır:

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

Aşağıdaki örnek, **yerleşik. personName** varlığının çözümlenme durumunu göstermektedir.

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

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[E-posta](luis-reference-prebuilt-email.md), [sayı](luis-reference-prebuilt-number.md)ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar hakkında bilgi edinin.
