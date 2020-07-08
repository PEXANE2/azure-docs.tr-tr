---
title: Azure Application Insights özel durum telemetrisi veri modeli
description: Özel durum telemetrisi için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: fa9bc4efd9549cbcb1d50439989c5dea79d9d60c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77671945"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Application Insights veri modeli

[Application Insights](../../azure-monitor/app/app-insights-overview.md)bir özel durum örneği, izlenen uygulamanın yürütülmesi sırasında oluşan işlenmiş veya işlenmemiş bir özel durumu temsil eder.

## <a name="problem-id"></a>Sorun kimliği

Kodda özel durumun oluşturulduğu tanımlayıcı. Özel durumlar gruplandırması için kullanılır. Genellikle, çağrı yığınından özel durum türü ve bir işlev birleşimi.

Maksimum Uzunluk: 1024 karakter

## <a name="severity-level"></a>Önem derecesi

Önem derecesi düzeyini izleyin. Değer,,,, vb `Verbose` `Information` . olabilir `Warning` `Error` `Critical` .

## <a name="exception-details"></a>Özel durum ayrıntıları

(Genişletilmek üzere)

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- [Web uygulamalarınızda Application Insights özel durumları tanılamayı](../../azure-monitor/app/asp-net-exceptions.md)öğrenin.
- Application Insights tarafından desteklenen [platformları](../../azure-monitor/app/platforms.md) inceleyin.
