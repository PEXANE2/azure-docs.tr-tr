---
title: Azure Uygulama Öngörüleri Veri Modeli - İzleme Telemetrisi
description: Trace telemetri için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 31958b26cdb8a7897cf0051af6600014c07949fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671962"
---
# <a name="trace-telemetry-application-insights-data-model"></a>İz telemetri: Uygulama Öngörüleri veri modeli

İzleme telemetrisi [(Uygulama Öngörüleri'nde)](../../azure-monitor/app/app-insights-overview.md)metin araması yapılan stil izleme deyimlerini temsil eder. `printf` `Log4Net`, `NLog`ve diğer metin tabanlı günlük dosya girişleri bu tür örneklere çevrilir. İz, genişletilebilirlik olarak ölçüm leri yoktur.

## <a name="message"></a>İleti

İletiyi takip et.

Maksimum uzunluk: 32768 karakter

## <a name="severity-level"></a>Önem derecesi

Önem düzeyinin izini sürükler. Değer , `Verbose` `Information`, `Warning` `Error`, `Critical`, .

## <a name="custom-properties"></a>Özel özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights'ta .NET izleme günlüklerini keşfedin.](../../azure-monitor/app/asp-net-trace-logs.md)
- [Application Insights'taki Java izleme günlüklerini keşfedin.](../../azure-monitor/app/java-trace-logs.md)
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- [Özel izleme telemetrisi yazın](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
