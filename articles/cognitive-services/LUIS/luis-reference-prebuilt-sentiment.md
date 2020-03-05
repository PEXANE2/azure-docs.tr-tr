---
title: Yaklaşım Analizi-LUSıS
titleSuffix: Azure Cognitive Services
description: Yaklaşım analizi yapılandırılmışsa, yaklaşım analizi LUIS json yanıtı içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/22/2019
ms.author: diberry
ms.openlocfilehash: dee07859da97f76d9a2db3ccbb71ae74d8ff4846
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270415"
---
# <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi yapılandırılmışsa, yaklaşım analizi LUIS json yanıtı içerir. [Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgelerindeki yaklaşım analizi hakkında daha fazla bilgi edinin.


## <a name="resolution-for-sentiment"></a>Yaklaşım için çözüm

Yaklaşım verilerdir pozitif gösteren 0 ile 1 arasındaki bir puan (1 yakın) veya (0 yakın) negatif yaklaşım veri.

#### <a name="english-language"></a>[İngilizce dili](#tab/english)

Kültür `en-us`olduğunda yanıt şu şekilde olur:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Diğer diller](#tab/other-languages)

Diğer tüm kültürler için yanıt şöyledir:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin uç noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

