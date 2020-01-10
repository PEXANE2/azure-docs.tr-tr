---
title: include dosyası
description: include dosyası
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: 3501ff3f92ae045019df2766bbcf7fc2c3fec5b0
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75768936"
---
## <a name="timeout"></a>İşlev uygulaması zaman aşımı süresi 

Bir işlev uygulamasının zaman aşımı süresi, [Host. JSON](../articles/azure-functions/functions-host-json.md#functiontimeout) proje dosyasındaki `functionTimeout` özelliği tarafından tanımlanır. Aşağıdaki tabloda, hem planlar hem de farklı çalışma zamanı sürümleri için dakika cinsinden varsayılan ve en büyük değerler gösterilmektedir:

| Plan | Çalışma zamanı sürümü | Varsayılan | Maksimum |
|------|---------|---------|---------|
| Tüketim | 'in | 5 | 10 |
| Tüketim | 2.x | 5 | 10 |
| Tüketim | 3.x | 5 | 10 |
| App Service | 'in | İş çalışma zamanında | İş çalışma zamanında |
| App Service | 2.x | 30 | İş çalışma zamanında |
| App Service | 3.x | 30 | İş çalışma zamanında |

> [!NOTE] 
> İşlev uygulaması zaman aşımı ayarından bağımsız olarak, 230 saniye bir HTTP tetiklenen işlevin bir isteğe yanıt vermek için gidebildiği en uzun süredir. Bunun nedeni, [Azure Load Balancer varsayılan boşta kalma zaman aşımı süresi](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Daha uzun işlem süreleri için [dayanıklı işlevler zaman uyumsuz model](../articles/azure-functions/durable/durable-functions-overview.md#async-http) kullanmayı veya [gerçek işi erteleyin ve anında yanıt döndürmenizi](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)düşünün.
