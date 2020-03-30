---
title: Azure Uygulama Öngörüleri Telemetri Veri Modeli - Etkinlik Telemetrisi | Microsoft Dokümanlar
description: Olay telemetrisi için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: bd8b2581f7642f6825aaf0d1b51c8e94d4333d33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671894"
---
# <a name="event-telemetry-application-insights-data-model"></a>Olay telemetrisi: Application Insights veri modeli

Uygulamanızda meydana gelen bir olayı temsil etmek için olay telemetri öğeleri [(Uygulama Öngörüleri'nde)](../../azure-monitor/app/app-insights-overview.md)oluşturabilirsiniz. Genellikle düğme tıklama veya sipariş ödeme gibi bir kullanıcı etkileşimidir. Ayrıca, başlatma veya yapılandırma güncelleştirmesi gibi bir uygulama yaşam döngüsü olayı da olabilir. 

Semantically, olaylar olabilir veya istekleri ile ilişkili olmayabilir. Ancak, düzgün kullanıldığında, olay telemetri istekleri veya izleri daha önemlidir. Olaylar iş telemetrisini temsil ediyor ve ayrı, daha az agresif [örneklemeye](../../azure-monitor/app/api-filtering-sampling.md)konu olmalıdır.

## <a name="name"></a>Adı

Olay adı. Uygun gruplandırmaya ve yararlı ölçümlere izin vermek için, uygulamanızı az sayıda ayrı olay adı oluşturacak şekilde kısıtlayın. Örneğin, oluşturulan her olay örneği için ayrı bir ad kullanmayın.

Maksimum uzunluk: 512 karakter

## <a name="custom-properties"></a>Özel özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- [Özel olay telemetrisi yazın](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
