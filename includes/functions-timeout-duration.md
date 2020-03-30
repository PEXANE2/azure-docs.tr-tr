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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77198335"
---
## <a name="function-app-timeout-duration"></a><a name="timeout"></a>Fonksiyon uygulaması zaman ödeme süresi 

Bir işlev uygulamasının zaman adabı, `functionTimeout` [ana bilgisayar.json](../articles/azure-functions/functions-host-json.md#functiontimeout) proje dosyasındaki özellik tarafından tanımlanır. Aşağıdaki tablo, hem planlar hem de farklı çalışma zamanı sürümleri için dakika içinde varsayılan ve maksimum değerleri gösterir:

| Planlama | Çalışma Zamanı Sürümü | Varsayılan | Maksimum |
|------|---------|---------|---------|
| Tüketim | 1.x | 5 | 10 |
| Tüketim | 2.x | 5 | 10 |
| Tüketim | 3.x | 5 | 10 |
| Premium | 1.x | 30 | Sınırsız |
| Premium | 2.x | 30 | Sınırsız |
| Premium | 3.x | 30 | Sınırsız |
| App Service | 1.x | Sınırsız | Sınırsız |
| App Service | 2.x | 30 | Sınırsız |
| App Service | 3.x | 30 | Sınırsız |

> [!NOTE] 
> İşlev uygulaması zaman ayarı ne olursa olsun, 230 saniye, bir HTTP tetiklenen işlevin bir isteğe yanıt vermek için kullanabileceği maksimum süredir. Bunun nedeni [Azure Yük Dengeleyicisinin varsayılan boşta zaman ahalisi.](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds) Daha uzun işlem süreleri için, [Dayanıklı Fonksiyonlar async deseni](../articles/azure-functions/durable/durable-functions-overview.md#async-http) kullanmayı düşünün veya [fiili çalışmayı ertelayın ve hemen yanıt döndürün.](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)
