---
title: LUSıS önceden oluşturulmuş varlıklar e-posta başvurusu
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUSıS) ' de e-posta önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: f7aa2a341a82d30f7022af49e626a3cd358854b7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273476"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için e-posta önceden oluşturulmuş varlık
E-posta ayıklama, bir söyleyden tüm e-posta adresini içerir. Bu varlık zaten eğitiltiğinden, uygulama hedefleri için e-posta içeren örnek bir değer eklemeniz gerekmez. E-posta varlığı yalnızca `en-us` kültür ' de desteklenir.

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş e-posta için çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`please send the information to patti@contoso.com`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi olarak `false`ayarlanmıştır:

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametresi olarak `true`ayarlanmıştır:

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ],
    "$instance": {
        "email": [
            {
                "type": "builtin.email",
                "text": "patti@contoso.com",
                "startIndex": 31,
                "length": 17,
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

Aşağıdaki örnekte, **yerleşik. e-posta** varlığının çözümlemesi gösterilmektedir.

```json
"entities": [
    {
        "entity": "patti@contoso.com",
        "type": "builtin.email",
        "startIndex": 31,
        "endIndex": 55,
        "resolution": {
        "value": "patti@contoso.com"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

Sayı, sıra [sayısı](luis-reference-prebuilt-number.md)ve [ordinal](luis-reference-prebuilt-ordinal.md) [yüzde](luis-reference-prebuilt-percentage.md)bilgileri hakkında bilgi edinin.
