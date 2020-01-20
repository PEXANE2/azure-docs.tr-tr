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
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279413"
---
[Application Insights ölçümleri hesaplanırken](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator)kaç tane işlev etkinleştirmeleri toplanmış olduğunu belirtir. 

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
|batchSize|1000|Toplanacak en fazla istek sayısı.| 
|flushTimeout|00:00:30|Toplanacak en uzun süre.| 

İşlev etkinleştirmeleri, ilk iki sınıra ulaşıldığında toplanır.
