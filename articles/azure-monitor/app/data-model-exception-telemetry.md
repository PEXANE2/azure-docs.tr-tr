---
title: Azure Application Insights özel durum telemetrisi veri modeli
description: Özel durum telemetrisi için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 6732fb3dde79abd5c69e2b0a91710bdd2356d6aa
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320604"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Application Insights veri modeli

[Application Insights](./app-insights-overview.md)bir özel durum örneği, izlenen uygulamanın yürütülmesi sırasında oluşan işlenmiş veya işlenmemiş bir özel durumu temsil eder.

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
- [Web uygulamalarınızda Application Insights özel durumları tanılamayı](./asp-net-exceptions.md)öğrenin.
- Application Insights tarafından desteklenen [platformları](./platforms.md) inceleyin.

