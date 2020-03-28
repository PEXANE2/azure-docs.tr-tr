---
title: Model sürümü
titleSuffix: Azure Cognitive Services
description: V3 uç noktalarında model sürümlerini belirtin
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 4cd8d0901ce23fb227bb6919dee18f5aa3d063ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77089043"
---
Text Analytics API sürümü 3, verileriniz için en güncel model sürümünü seçmenize olanak tanır. İstekleriniz `model-version` için istenen modelin sürümünü seçmek için isteğe bağlı parametreyi kullanın. Bu parametre belirtilmemişse, API `latest`varsayılan olarak , en son kararlı sürüm. En yeni model sürümünü herhangi bir istekte kullanabiliyor olsanız da, her sürümde yalnızca bazı özellikler güncelleştirilir. Aşağıdaki tabloda, her model sürümünde hangi özelliklerin güncelleştirildiği açıklanmaktadır:

| Model sürümü           | Güncelleştirilmiş özellikler         | En son sürüm:           |
|-------------------------|--------------------------|--------------------------|
| `2020-02-01`            | Varlık tanıma                      | Varlık tanıma                      |
| `2019-10-01`            | Varlık tanıma, Duyarlılık analizi  | Dil algılama, Anahtar kelime çıkarma, Duygu analizi|


v3 uç noktalarından gelen `model-version` her yanıt, kullanılan model sürümünü belirten bir alan içerir.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
Bu model sürümlerinin güncelleştirmeleri hakkında ayrıntılar için [yeniliklere](../whats-new.md) bakın.
