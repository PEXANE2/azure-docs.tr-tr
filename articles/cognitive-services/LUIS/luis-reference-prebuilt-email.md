---
title: LUSıS önceden oluşturulmuş varlıklar e-posta başvurusu
titleSuffix: Azure Cognitive Services
description: Bu makale, Language Understanding (LUSıS) ' de e-posta önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.openlocfilehash: 806357670bead54658f0b501ca20473293275d58
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91533375"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için e-posta önceden oluşturulmuş varlık
E-posta ayıklama, bir söyleyden tüm e-posta adresini içerir. Bu varlık zaten eğitiltiğinden, uygulama hedefleri için e-posta içeren örnek bir değer eklemeniz gerekmez. E-posta varlığı yalnızca kültür ' de desteklenir `en-us` .

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş e-posta için çözüm

Sorgu için aşağıdaki varlık nesneleri döndürülür:

`please send the information to patti@contoso.com`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `false` :

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 ayrıntılı yanıt](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametresi olarak ayarlanmıştır `true` :

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
