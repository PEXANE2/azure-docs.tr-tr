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
ms.openlocfilehash: fded43bb655cefda508b82eca94522730ab6da00
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74941752"
---
## <a name="timeout"></a>İşlev uygulaması zaman aşımı süresi 

Bir işlev uygulamasının zaman aşımı süresi, [Host. JSON](../articles/azure-functions/functions-host-json.md#functiontimeout) proje dosyasındaki functiontimeout özelliği tarafından tanımlanır. Aşağıdaki tabloda, hem planlar hem de çalışma zamanı sürümlerinde, varsayılan ve en büyük değerler dakika cinsinden gösterilmektedir:

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
