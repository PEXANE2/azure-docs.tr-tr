---
title: Model sürümü oluşturma
titleSuffix: Azure Cognitive Services
description: V3 uç noktalarında model sürümlerini belirtme
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 4cd8d0901ce23fb227bb6919dee18f5aa3d063ed
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089043"
---
Metin Analizi API'si sürüm 3, verileriniz için en güncel model sürümünü seçmenizi sağlar. İstekleriniz için istenen modelin sürümünü seçmek için isteğe bağlı `model-version` parametresini kullanın. Bu parametre belirtilmemişse, API varsayılan olarak en son kararlı sürümü `latest`. Herhangi bir istekte en yeni model sürümünü kullanabilirsiniz, ancak her sürümde yalnızca bazı özellikler güncelleştirilir. Aşağıdaki tabloda, her model sürümünde hangi özelliklerin güncelleştirildiği açıklanmaktadır:

| Model sürümü           | Güncelleştirilmiş özellikler         | İçin en son sürüm:           |
|-------------------------|--------------------------|--------------------------|
| `2020-02-01`            | Varlık tanıma                      | Varlık tanıma                      |
| `2019-10-01`            | Varlık tanıma, yaklaşım Analizi  | Dil algılama, anahtar tümceciği ayıklama, yaklaşım Analizi|


V3 uç noktalarından alınan her yanıt, kullanılan model sürümünü belirten bir `model-version` alanı içerir.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
Bu model sürümlerinin güncelleştirmeleriyle ilgili ayrıntılar [için bkz. yenilikler.](../whats-new.md)
