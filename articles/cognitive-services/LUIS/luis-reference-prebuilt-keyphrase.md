---
title: Keyphrase önceden oluşturulmuş varlık-LUSıS
titleSuffix: Azure Cognitive Services
description: Bu makalede anahtar cümlesi içeren önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: d7ecb579305cb17525e220bc16be430c77d25080
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563419"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>LUSıS uygulaması için keyPhrase önceden oluşturulmuş varlık
anahtar cümlesi, anahtar ifadeleri çeşitli bir utterance ayıklar. Uygulamaya anahtar cümlesi içeren örnek Konuşma ekleme gerekmez. anahtar cümlesi varlık içerisinde desteklendiği [çok kültür](luis-language-support.md#languages-supported) parçası olarak [metin analizi](../text-analytics/overview.md) özellikleri. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Önceden oluşturulmuş bir anahtar cümlesi varlık için çözümleme

### <a name="api-version-2x"></a>API sürüm 2. x

Aşağıdaki örnek, çözünürlüğünü gösterir **builtin.keyPhrase** varlık.

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

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yüzdesi](luis-reference-prebuilt-percentage.md), [numarası](luis-reference-prebuilt-number.md), ve [yaş](luis-reference-prebuilt-age.md) varlıklar.
