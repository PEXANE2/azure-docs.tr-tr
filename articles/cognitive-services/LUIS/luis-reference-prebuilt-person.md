---
title: Personbadı önceden oluşturulmuş varlık-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makalede personName içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a9da5089c9309ec06f29fd3e0868102cfa538049
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933451"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Bir LUSıS uygulaması için sorumlu adı önceden oluşturulmuş varlık
Önceden oluşturulmuş personName varlık kişi adlarını algılar. Bu varlık zaten eğitildi çünkü uygulama hedefleri için personName içeren örnek Konuşma ekleme gerekmez. İngilizce ve Çince personName varlık desteklenen [kültürler](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>PersonName varlık için çözümleme

### <a name="api-version-2x"></a>API sürüm 2. x

Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.personName** varlık.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  },
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Preview API sürüm 3. x

Aşağıdaki JSON `verbose` parametresi olarak `false`ayarlanmıştır:

```json
{
    "query": "Is Jill Jones in Cairo?",
    "prediction": {
        "normalizedQuery": "is jill jones in cairo?",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6544678
            }
        },
        "entities": {
            "personName": [
                "Jill Jones"
            ]
        }
    }
}
```

Aşağıdaki JSON `verbose` parametresi olarak `true`ayarlanmıştır:

```json
{
    "query": "Is Jill Jones in Cairo?",
    "prediction": {
        "normalizedQuery": "is jill jones in cairo?",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.6544678
            }
        },
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
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [e-posta](luis-reference-prebuilt-email.md), [numarası](luis-reference-prebuilt-number.md), ve [sıralı](luis-reference-prebuilt-ordinal.md) varlıklar. 
