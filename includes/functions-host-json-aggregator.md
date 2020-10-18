---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: c63cee1c451918569e9b7aa2e76209e8069d86ac
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167670"
---
[Application Insights ölçümleri hesaplanırken](../articles/azure-functions/configure-monitoring.md#configure-the-aggregator)kaç tane işlev etkinleştirmeleri toplanmış olduğunu belirtir. 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik |Varsayılan  | Description |
|---------|---------|---------| 
|batchSize|1000|Toplanacak en fazla istek sayısı.| 
|flushTimeout|00:00:30|Toplanacak en uzun süre.| 

İşlev etkinleştirmeleri, ilk iki sınıra ulaşıldığında toplanır.
