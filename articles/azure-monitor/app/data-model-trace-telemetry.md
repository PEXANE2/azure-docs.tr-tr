---
title: Azure Application Insights veri modeli-Izleme telemetrisi
description: İzleme telemetri için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 8fea4bbf590816b2ef168a2ed16f197389ee282e
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320553"
---
# <a name="trace-telemetry-application-insights-data-model"></a>İzleme telemetrisi: Application Insights veri modeli

İzleme telemetrisi ( [Application Insights](./app-insights-overview.md)) `printf` , metin arama olan stil izleme deyimlerini temsil eder. `Log4Net`, `NLog` ve diğer metin tabanlı günlük dosyası girdileri bu türdeki örneklere çevrilir. İzlemenin bir genişletilebilirlik olarak ölçümleri yoktur.

## <a name="message"></a>İleti

Trace iletisi.

Maksimum Uzunluk: 32768 karakter

## <a name="severity-level"></a>Önem derecesi

Önem derecesi düzeyini izleyin. Değer,,,, vb `Verbose` `Information` . olabilir `Warning` `Error` `Critical` .

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application Insights .NET izleme günlüklerini keşfet](./asp-net-trace-logs.md).
- [Application Insights Java izleme günlüklerini keşfet](./java-trace-logs.md).
- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- [Özel izleme telemetrisini yazma](./api-custom-events-metrics.md#tracktrace)
- Application Insights tarafından desteklenen [platformları](./platforms.md) inceleyin.

