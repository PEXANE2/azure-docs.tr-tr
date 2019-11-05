---
title: Model sürümü oluşturma
titleSuffix: Azure Cognitive Services
description: V3 uç noktalarında model sürümlerini belirtme
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/25/2019
ms.author: aahi
ms.openlocfilehash: 5a06e26e5f1640024e343c714db3df134422115c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488629"
---
Metin Analizi API'si sürüm 3, verilerinizde kullanılan Metin Analizi modelini seçmenizi sağlar. İstekinizdeki modelin bir sürümünü seçmek için isteğe bağlı `model-version` parametresini kullanın. Bu parametre belirtilmemişse, API varsayılan olarak en son kararlı model sürümüne `latest`.

Kullanılabilir model sürümleri:
* `2019-10-01` (`latest`)

V3 uç noktalarından alınan her yanıt, kullanılan model sürümünü belirten bir `model-version` alanı içerir.

```json
{
    “documents”: […]
    “errors”: []
    “model-version”: “2019-10-01”
}
```
