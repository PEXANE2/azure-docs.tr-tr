---
title: include dosyası
description: include dosyası
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 08/06/2019
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 14e2bd4af2616e9dd33fe8267de132ab6c0f1cfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69982629"
---
Uygulama başına ölçüm ve olay sayısında, yani enstrümantasyon anahtarı başına bazı sınırlar vardır. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| Kaynak | Varsayılan limit | Not
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla veriye ihtiyacınız varsa, portaldaki sınırı 1.000 GB'a kadar artırabilirsiniz. 1.000 GB'dan büyük kapasiteler AIDataCap@microsoft.comiçin e-posta gönderin.
| Azaltma | 32.000 etkinlik/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | [30 - 730 gün arası](https://docs.microsoft.com/azure/azure-monitor/app/pricing#change-the-data-retention-period)  | Bu kaynak [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) ve [Ölçüm Gezgini](../articles/azure-monitor/app/metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/azure-monitor/app/availability-multistep.md) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| Maksimum telemetri madde boyutu | 64 kB |
| Toplu iş başına maksimum telemetri öğeleri | 64 K |
| Özellik ve ölçüm adı uzunluğu | 150 | [Bkz. tür şemaları](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Özellik değeri dize uzunluğu | 8,192  | [Bkz. tür şemaları](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| İzleme ve özel durum iletisi uzunluğu | 32,768  | [Bkz. tür şemaları](https://github.com/microsoft/ApplicationInsights-Home/tree/master/EndpointSpecs/Schemas/Bond).
| Uygulama başına [kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md) sayısı | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) veri saklama | 5 gün |
| Günlük gönderilen [profil oluşturucu](../articles/azure-monitor/app/profiler.md) verileri | 10 GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/azure-monitor/app/pricing.md).