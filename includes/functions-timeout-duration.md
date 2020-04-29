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
ms.openlocfilehash: eca2d3359614875e5bff0c9bb67f006f0a8cdba1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77198335"
---
## <a name="function-app-timeout-duration"></a><a name="timeout"></a>İşlev uygulaması zaman aşımı süresi 

Bir işlev uygulamasının zaman aşımı süresi, [Host. JSON](../articles/azure-functions/functions-host-json.md#functiontimeout) proje `functionTimeout` dosyasındaki özelliği tarafından tanımlanır. Aşağıdaki tabloda, hem planlar hem de farklı çalışma zamanı sürümleri için dakika cinsinden varsayılan ve en büyük değerler gösterilmektedir:

| Planlama | Çalışma zamanı sürümü | Varsayılan | Maksimum |
|------|---------|---------|---------|
| Tüketim | 'in | 5 | 10 |
| Tüketim | 2.x | 5 | 10 |
| Tüketim | 3.x | 5 | 10 |
| Premium | 'in | 30 | Sınırsız |
| Premium | 2.x | 30 | Sınırsız |
| Premium | 3.x | 30 | Sınırsız |
| App Service | 'in | Sınırsız | Sınırsız |
| App Service | 2.x | 30 | Sınırsız |
| App Service | 3.x | 30 | Sınırsız |

> [!NOTE] 
> İşlev uygulaması zaman aşımı ayarından bağımsız olarak, 230 saniye bir HTTP tetiklenen işlevin bir isteğe yanıt vermek için gidebildiği en uzun süredir. Bunun nedeni, [Azure Load Balancer varsayılan boşta kalma zaman aşımı süresi](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Daha uzun işlem süreleri için [dayanıklı işlevler zaman uyumsuz model](../articles/azure-functions/durable/durable-functions-overview.md#async-http) kullanmayı veya [gerçek işi erteleyin ve anında yanıt döndürmenizi](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)düşünün.
