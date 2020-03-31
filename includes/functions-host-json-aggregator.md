---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9c51ce726545d1c64d69c86c36fc69ea43c3b882
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279413"
---
[Uygulama Öngörüleri ölçümleri hesaplanırken](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator)kaç işlev çağrısının toplanmış olduğunu belirtir. 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik |Varsayılan  | Açıklama |
|---------|---------|---------| 
|batchSize|1000|Toplam istek sayısı maksimum.| 
|flushTimeout|00:00:30|Toplama maksimum süre.| 

İki sınırdan ilkine ulaşıldığında işlev çağrıları toplanır.
