---
title: Azure Application Insights Telemetri veri modeli-olay telemetrisi | Microsoft Docs
description: Olay telemetrisi için veri modeli Application Insights
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: bd8b2581f7642f6825aaf0d1b51c8e94d4333d33
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671894"
---
# <a name="event-telemetry-application-insights-data-model"></a>Olay telemetrisi: Application Insights veri modeli

Uygulamanızda gerçekleşen bir olayı göstermek için olay telemetrisi öğeleri ( [Application Insights](../../azure-monitor/app/app-insights-overview.md)) oluşturabilirsiniz. Genellikle, düğme tıklama veya siparişi teslim alma gibi bir Kullanıcı etkileşimidir. Ayrıca, başlatma veya yapılandırma güncelleştirmesi gibi bir uygulama yaşam döngüsü olayı da olabilir. 

Anlamsal olarak olaylar, isteklerle bağıntılı olabilir veya olmayabilir. Ancak, düzgün şekilde kullanılırsa, olay telemetrisi isteklerden veya İzlemelerden daha önemlidir. Olaylar iş telemetrisini temsil eder ve ayrı, daha az agresif [örneklemeye](../../azure-monitor/app/api-filtering-sampling.md)tabi olmalıdır.

## <a name="name"></a>Adı

Olay adı. Doğru gruplandırılmasına ve yararlı ölçümlere izin vermek için uygulamanızı, az sayıda ayrı olay adı oluşturacak şekilde kısıtlayın. Örneğin, bir olayın oluşturulan her örneği için ayrı bir ad kullanmayın.

Maksimum Uzunluk: 512 karakter

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- [Özel olay telemetrisini yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- Application Insights tarafından desteklenen [platformları](../../azure-monitor/app/platforms.md) inceleyin.
