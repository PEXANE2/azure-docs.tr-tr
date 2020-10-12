---
title: Azure Application Insights Telemetri veri modeli-olay telemetrisi | Microsoft Docs
description: Olay telemetrisi için veri modeli Application Insights
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 69685afa14352a22b58bccbea342038e4273696e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87320621"
---
# <a name="event-telemetry-application-insights-data-model"></a>Olay telemetrisi: Application Insights veri modeli

Uygulamanızda gerçekleşen bir olayı göstermek için olay telemetrisi öğeleri ( [Application Insights](./app-insights-overview.md)) oluşturabilirsiniz. Genellikle, düğme tıklama veya siparişi teslim alma gibi bir Kullanıcı etkileşimidir. Ayrıca, başlatma veya yapılandırma güncelleştirmesi gibi bir uygulama yaşam döngüsü olayı da olabilir. 

Anlamsal olarak olaylar, isteklerle bağıntılı olabilir veya olmayabilir. Ancak, düzgün şekilde kullanılırsa, olay telemetrisi isteklerden veya İzlemelerden daha önemlidir. Olaylar iş telemetrisini temsil eder ve ayrı, daha az agresif [örneklemeye](./api-filtering-sampling.md)tabi olmalıdır.

## <a name="name"></a>Adı

Olay adı. Doğru gruplandırılmasına ve yararlı ölçümlere izin vermek için uygulamanızı, az sayıda ayrı olay adı oluşturacak şekilde kısıtlayın. Örneğin, bir olayın oluşturulan her örneği için ayrı bir ad kullanmayın.

Maksimum Uzunluk: 512 karakter

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- [Özel olay telemetrisini yazma](./api-custom-events-metrics.md#trackevent)
- Application Insights tarafından desteklenen [platformları](./platforms.md) inceleyin.

