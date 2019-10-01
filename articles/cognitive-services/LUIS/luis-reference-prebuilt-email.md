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
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 4a1bc9ae7ccf48b9dc8b47b57ea43b9259786d01
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677677"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için e-posta önceden oluşturulmuş varlık
E-posta ayıklama, bir söyleyden tüm e-posta adresini içerir. Bu varlık zaten eğitiltiğinden, uygulama hedefleri için e-posta içeren örnek bir değer eklemeniz gerekmez. E-posta varlığı yalnızca `en-us` kültür içinde desteklenir. 

## <a name="resolution-for-prebuilt-email"></a>Önceden oluşturulmuş e-posta için çözüm

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

Aşağıdaki örnekte, **yerleşik. e-posta** varlığının çözümlemesi gösterilmektedir.

```json
{
  "query": "please send the information to patti@contoso.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
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
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi `false` olarak ayarlanmıştır:

```json
{
    "query": "please send the information to patti@contoso.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti@contoso.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti@contoso.com"
            ]
        }
    }
}
```


Aşağıdaki JSON `verbose` parametresi `true` olarak ayarlanmıştır:

```json
{
    "query": "please send the information to patti@contoso.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti@contoso.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
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
                        "length": 25,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

* * * 

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

Sayı, sıra [sayısı](luis-reference-prebuilt-number.md)ve [](luis-reference-prebuilt-ordinal.md) [yüzde](luis-reference-prebuilt-percentage.md)bilgileri hakkında bilgi edinin. 
