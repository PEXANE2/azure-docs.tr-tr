---
title: Azure Uygulama Öngörüleri Özel Durum Telemetri Veri modeli
description: Özel durum telemetrisi için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: fa9bc4efd9549cbcb1d50439989c5dea79d9d60c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671945"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Uygulama Öngörüleri veri modeli

[Uygulama Öngörüleri'nde,](../../azure-monitor/app/app-insights-overview.md)Özel Durum örneği, izlenen uygulamanın yürütülmesi sırasında oluşan işlenmiş veya işlenmemiş bir özel durumu temsil eder.

## <a name="problem-id"></a>Sorun Kimliği

Özel durum kodda atıldığı yerin tanımlayıcısı. Özel durumlar gruplandırma için kullanılır. Genellikle özel durum türü ve çağrı yığınından bir işlev in birleşimi.

Maksimum uzunluk: 1024 karakter

## <a name="severity-level"></a>Önem derecesi

Önem düzeyinin izini sürükler. Değer , `Verbose` `Information`, `Warning` `Error`, `Critical`, .

## <a name="exception-details"></a>Özel durum ayrıntıları

(Uzatılacak)

## <a name="custom-properties"></a>Özel özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- [Uygulama Öngörüleri ile web uygulamalarınızdaki özel durumları nasıl tanılayılacağını](../../azure-monitor/app/asp-net-exceptions.md)öğrenin.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
