---
title: LUIS Önceden oluşturulmuş varlıklar e-posta başvurusu
titleSuffix: Azure Cognitive Services
description: Bu makale, Dil Anlama (LUIS) e-posta önceden oluşturulmuş varlık bilgileri içerir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273476"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>LUIS uygulaması için önceden oluşturulmuş varlığı e-postayla gönder
E-posta çıkarma bir söyleyiş tüm e-posta adresi içerir. Bu varlık zaten eğitimli olduğundan, uygulama amaçlarına e-posta içeren örnek sözcükler eklemeniz gerekmez. E-posta varlığı `en-us` yalnızca kültür de desteklenir.

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş e-posta için çözünürlük

Aşağıdaki varlık nesneleri sorgu için döndürülür:

`please send the information to patti@contoso.com`

#### <a name="v3-response"></a>[V3 yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametre ile `false`ayarlanır:

```json
"entities": {
    "email": [
        "patti@contoso.com"
    ]
}
```
#### <a name="v3-verbose-response"></a>[V3 verbose yanıtı](#tab/V3-verbose)

Aşağıdaki JSON `verbose` parametre ile `true`ayarlanır:

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

Aşağıdaki örnek, **builtin.email** kuruluşunun çözünürlüğünü gösterir.

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

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

[Sayı](luis-reference-prebuilt-number.md), [ordinal](luis-reference-prebuilt-ordinal.md), ve [yüzdesi](luis-reference-prebuilt-percentage.md)hakkında bilgi edinin.
