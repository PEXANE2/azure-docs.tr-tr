---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 32a04518d3cd097a02ec3045da891237fc0e405a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334685"
---
Uygulama başına ölçüm ve olay sayısında, yani enstrümantasyon anahtarı başına bazı sınırlar vardır. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| Kaynak | Sınır | Not
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla veriye ihtiyacınız varsa, portaldaki sınırı 1.000 GB'a kadar artırabilirsiniz. 1.000 GB'dan büyük kapasiteler AIDataCap@microsoft.comiçin e-posta gönderin.
| Azaltma | 32.000 etkinlik/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) ve [Ölçüm Gezgini](../articles/azure-monitor/app/metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/azure-monitor/app/availability-multistep.md) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| Maksimum olay boyutu | 64,000,000 bayt |
| Özellik ve ölçüm adı uzunluğu | 150 | [Bkz. tür şemaları](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Özellik değeri dize uzunluğu | 8,192 | [Bkz. tür şemaları](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| İzleme ve özel durum iletisi uzunluğu | 32,768  | [Bkz. tür şemaları](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond/).
| Uygulama başına [kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md) sayısı | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) veri saklama | 5 gün |
| Günlük gönderilen [profil oluşturucu](../articles/azure-monitor/app/profiler.md) verileri | 10 GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/azure-monitor/app/pricing.md).
