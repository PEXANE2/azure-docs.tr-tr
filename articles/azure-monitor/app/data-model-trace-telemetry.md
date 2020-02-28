---
title: Azure Application Insights veri modeli-Izleme telemetrisi
description: İzleme telemetri için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 31958b26cdb8a7897cf0051af6600014c07949fd
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671962"
---
# <a name="trace-telemetry-application-insights-data-model"></a>İzleme telemetrisi: Application Insights veri modeli

İzleme telemetrisi ( [Application Insights](../../azure-monitor/app/app-insights-overview.md)), metin arama olan `printf` stili izleme deyimlerini temsil eder. `Log4Net`, `NLog`ve diğer metin tabanlı günlük dosyası girdileri bu türdeki örneklere çevrilir. İzlemenin bir genişletilebilirlik olarak ölçümleri yoktur.

## <a name="message"></a>İleti

Trace iletisi.

Maksimum Uzunluk: 32768 karakter

## <a name="severity-level"></a>Önem derecesi

Önem derecesi düzeyini izleyin. Değer `Verbose`, `Information`, `Warning`, `Error`, `Critical`olabilir.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights .NET izleme günlüklerini keşfet](../../azure-monitor/app/asp-net-trace-logs.md).
- [Application Insights Java izleme günlüklerini keşfet](../../azure-monitor/app/java-trace-logs.md).
- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- [Özel izleme telemetrisini yazma](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Application Insights tarafından desteklenen [platformları](../../azure-monitor/app/platforms.md) inceleyin.
