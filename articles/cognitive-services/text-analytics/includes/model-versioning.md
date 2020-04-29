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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77089043"
---
Metin Analizi API'si sürüm 3, verileriniz için en güncel model sürümünü seçmenizi sağlar. İstekleriniz için `model-version` istenen modelin sürümünü seçmek için isteğe bağlı parametresini kullanın. Bu parametre belirtilmemişse `latest`, API varsayılan olarak en son kararlı sürümdür. Herhangi bir istekte en yeni model sürümünü kullanabilirsiniz, ancak her sürümde yalnızca bazı özellikler güncelleştirilir. Aşağıdaki tabloda, her model sürümünde hangi özelliklerin güncelleştirildiği açıklanmaktadır:

| Model sürümü           | Güncelleştirilmiş özellikler         | İçin en son sürüm:           |
|-------------------------|--------------------------|--------------------------|
| `2020-02-01`            | Varlık tanıma                      | Varlık tanıma                      |
| `2019-10-01`            | Varlık tanıma, yaklaşım Analizi  | Dil algılama, anahtar tümceciği ayıklama, yaklaşım Analizi|


V3 uç noktalarından alınan her yanıt, kullanılan `model-version` model sürümünü belirten bir alan içerir.

```json
{
    "documents": […]
    "errors": []
    "model-version": "2019-10-01"
}
```
Bu model sürümlerinin güncelleştirmeleriyle ilgili ayrıntılar [için bkz. yenilikler.](../whats-new.md)
