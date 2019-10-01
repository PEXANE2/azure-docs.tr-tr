---
title: Keyphrase önceden oluşturulmuş varlık-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makale Language Understanding (LUSıS) içindeki keyphrase önceden oluşturulmuş varlık bilgilerini içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: e55c0453c117c51e5a8e4986631516d3e61ed10b
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677588"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>LUSıS uygulaması için keyPhrase önceden oluşturulmuş varlık
KeyPhrase varlığı, çeşitli anahtar tümceciklerini utterance 'ten ayıklar. Uygulamaya keyPhrase içeren örnek eklemelerine ihtiyacınız yoktur. KeyPhrase varlığı, [metin analizi](../text-analytics/overview.md) özelliklerinin bir parçası olarak [birçok kültürde](luis-language-support.md#languages-supported) desteklenir. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş keyPhrase varlığı için çözüm

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[V2 tahmin uç noktası yanıtı](#tab/V2)

Aşağıdaki örnek, **yerleşik. keyPhrase** varlığının çözünürlüğünü gösterir.

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```
#### <a name="v3-prediction-endpoint-responsetabv3"></a>[V3 tahmin uç noktası yanıtı](#tab/V3)

Aşağıdaki JSON `verbose` parametresi `false` olarak ayarlanmıştır:

```json
{
    "query": "where is the educational requirements form for the development and engineering group",
    "prediction": {
        "normalizedQuery": "where is the educational requirements form for the development and engineering group",
        "topIntent": "GetJobInformation",
        "intents": {
            "GetJobInformation": {
                "score": 0.157861546
            }
        },
        "entities": {
            "keyPhrase": [
                "educational requirements",
                "development"
            ]
        }
    }
}
```

Aşağıdaki JSON `verbose` parametresi `true` olarak ayarlanmıştır:

```json
{
    "query": "where is the educational requirements form for the development and engineering group",
    "prediction": {
        "normalizedQuery": "where is the educational requirements form for the development and engineering group",
        "topIntent": "GetJobInformation",
        "intents": {
            "GetJobInformation": {
                "score": 0.157861546
            }
        },
        "entities": {
            "keyPhrase": [
                "educational requirements",
                "development"
            ],
            "$instance": {
                "keyPhrase": [
                    {
                        "type": "builtin.keyPhrase",
                        "text": "educational requirements",
                        "startIndex": 13,
                        "length": 24,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    },
                    {
                        "type": "builtin.keyPhrase",
                        "text": "development",
                        "startIndex": 51,
                        "length": 11,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
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

[Yüzde](luis-reference-prebuilt-percentage.md), [sayı](luis-reference-prebuilt-number.md)ve [yaş](luis-reference-prebuilt-age.md) varlıkları hakkında bilgi edinin.
