---
title: Azure Monitör Uygulama Öngörüleri Bağımlılık Veri Modeli
description: Bağımlılık telemetrisi için Uygulama Öngörüleri veri modeli
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: ba0d848904d1ba885dc53e2941953d8dfb4864cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671928"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık Telemetrisi [(Uygulama Öngörüleri'nde),](../../azure-monitor/app/app-insights-overview.md)izlenen bileşenin SQL veya HTTP bitiş noktası gibi uzak bir bileşenle etkileşimini temsil eder.

## <a name="name"></a>Adı

Bu bağımlılık çağrısıyla başlatılan komutun adı. Düşük önem düzeyi. Örnekler yordam adı ve URL yol şablonu saklanır.

## <a name="id"></a>Kimlik

Bağımlılık arama örneğinin tanımlayıcısı. Bu bağımlılık çağrısına karşılık gelen istek telemetri öğesi ile korelasyon için kullanılır. Daha fazla bilgi için [korelasyon](../../azure-monitor/app/correlation.md) sayfasına bakın.

## <a name="data"></a>Veri

Bu bağımlılık çağrısı tarafından başlatılan komut. Örnekler tüm sorgu parametreleri ile SQL deyimi ve HTTP URL vardır.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıkların mantıksal gruplandırması ve commandName ve resultCode gibi diğer alanların yorumlanması için düşük önem düzeyi. Örnekler SQL, Azure tablosu ve HTTP'dir.

## <a name="target"></a>Hedef

Bağımlılık çağrısının hedef sitesi. Örnekler sunucu adı, ana bilgisayar adresidir. Daha fazla bilgi için [korelasyon](../../azure-monitor/app/correlation.md) sayfasına bakın.

## <a name="duration"></a>Süre

Formatta istek `DD.HH:MM:SS.MMMMMM`süresi: . Birkaç günden `1000` az olmalı.

## <a name="result-code"></a>Sonuç kodu

Bağımlılık çağrısının sonuç kodu. Örnekler SQL hata kodu ve HTTP durum koduvardır.

## <a name="success"></a>Başarılı

Başarılı veya başarısız aramanın göstergesi.

## <a name="custom-properties"></a>Özel özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [.NET](../../azure-monitor/app/asp-net-dependencies.md)için bağımlılık izleme ayarlama.
- [Java](../../azure-monitor/app/java-agent.md)için bağımlılık izleme ayarlayın.
- [Özel bağımlılık telemetrisi yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
