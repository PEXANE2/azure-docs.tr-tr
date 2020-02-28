---
title: Azure Izleyici Application Insights bağımlılık veri modeli
description: Bağımlılık telemetri için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: ba0d848904d1ba885dc53e2941953d8dfb4864cf
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671928"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık telemetrisi ( [Application Insights](../../azure-monitor/app/app-insights-overview.md)), IZLENEN bileşenin SQL veya HTTP uç noktası gibi uzak bir bileşenle bir etkileşimini temsil eder.

## <a name="name"></a>Adı

Bu bağımlılık çağrısıyla başlatılan komutun adı. Düşük kardinalite değeri. Örnekler, saklı yordam adı ve URL yolu şablonudur.

## <a name="id"></a>Kimlik

Bağımlılık çağrı örneğinin tanımlayıcısı. Bu bağımlılık çağrısına karşılık gelen istek telemetrisi öğesiyle bağıntı için kullanılır. Daha fazla bilgi için bkz. [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

## <a name="data"></a>Veriler

Bu bağımlılık çağrısıyla başlatılan komut. Örnekler, tüm sorgu parametrelerine sahip SQL deyimleridir ve HTTP URL 'sidir.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıkların mantıksal gruplandırması ve commandName ve resultCode gibi diğer alanların yorumlanması için düşük kardinalite değeri. SQL, Azure tablosu ve HTTP örnekleri aşağıda verilmiştir.

## <a name="target"></a>Hedef

Bağımlılık çağrısının hedef sitesi. Örnek olarak sunucu adı, ana bilgisayar adresidir. Daha fazla bilgi için bkz. [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

## <a name="duration"></a>Süre

Şu biçimdeki istek süresi: `DD.HH:MM:SS.MMMMMM`. `1000` günden az olmalıdır.

## <a name="result-code"></a>Sonuç kodu

Bağımlılık çağrısının sonuç kodu. SQL hata kodu ve HTTP durum kodu örnekleri aşağıda verilmiştir.

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [.Net](../../azure-monitor/app/asp-net-dependencies.md)için bağımlılık izlemeyi ayarlayın.
- [Java](../../azure-monitor/app/java-agent.md)için bağımlılık izlemeyi ayarlayın.
- [Özel bağımlılık telemetrisi yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- Application Insights tarafından desteklenen [platformları](../../azure-monitor/app/platforms.md) inceleyin.
