---
title: Duygusallık analizi - LUIS
titleSuffix: Azure Cognitive Services
description: Duygusallık analizi yapılandırılırsa, LUIS json yanıtı duygusallık analizini içerir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270415"
---
# <a name="sentiment-analysis"></a>Yaklaşım analizi
Duygusallık analizi yapılandırılırsa, LUIS json yanıtı duygusallık analizini içerir. [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) belgelerinde duygu analizi hakkında daha fazla bilgi edinin.


## <a name="resolution-for-sentiment"></a>Duyarlılık için çözüm

Duyarlılık verileri, verilerin pozitif (1'e yakın) veya negatif (0'a yakın) duyarlılığını gösteren 1 ile 0 arasında bir puandır.

#### <a name="english-language"></a>[Ingilizce](#tab/english)

Kültür olduğunda, `en-us`yanıt:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

#### <a name="other-languages"></a>[Diğer diller](#tab/other-languages)

Diğer tüm kültürler için, yanıt:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```
* * *

## <a name="next-steps"></a>Sonraki adımlar

[V3 tahmin bitiş noktası](luis-migration-api-v3.md)hakkında daha fazla bilgi edinin.

