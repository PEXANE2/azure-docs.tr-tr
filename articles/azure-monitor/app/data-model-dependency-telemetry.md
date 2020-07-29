---
title: Azure Izleyici Application Insights bağımlılık veri modeli
description: Bağımlılık telemetri için Application Insights veri modeli
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 0f9fc96479569c3411024068ed614d422035ab17
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87315980"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık telemetrisi ( [Application Insights](./app-insights-overview.md)), IZLENEN bileşenin SQL veya HTTP uç noktası gibi uzak bir bileşenle bir etkileşimini temsil eder.

## <a name="name"></a>Ad

Bu bağımlılık çağrısıyla başlatılan komutun adı. Düşük kardinalite değeri. Örnekler, saklı yordam adı ve URL yolu şablonudur.

## <a name="id"></a>ID

Bağımlılık çağrı örneğinin tanımlayıcısı. Bu bağımlılık çağrısına karşılık gelen istek telemetrisi öğesiyle bağıntı için kullanılır. Daha fazla bilgi için bkz. [bağıntı](./correlation.md) sayfası.

## <a name="data"></a>Veriler

Bu bağımlılık çağrısıyla başlatılan komut. Örnekler, tüm sorgu parametrelerine sahip SQL deyimleridir ve HTTP URL 'sidir.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıkların mantıksal gruplandırması ve commandName ve resultCode gibi diğer alanların yorumlanması için düşük kardinalite değeri. SQL, Azure tablosu ve HTTP örnekleri aşağıda verilmiştir.

## <a name="target"></a>Hedef

Bağımlılık çağrısının hedef sitesi. Örnek olarak sunucu adı, ana bilgisayar adresidir. Daha fazla bilgi için bkz. [bağıntı](./correlation.md) sayfası.

## <a name="duration"></a>Süre

Şu biçimdeki istek süresi: `DD.HH:MM:SS.MMMMMM` . Gün sayısından az olmalıdır `1000` .

## <a name="result-code"></a>Sonuç kodu

Bağımlılık çağrısının sonuç kodu. SQL hata kodu ve HTTP durum kodu örnekleri aşağıda verilmiştir.

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [.Net](./asp-net-dependencies.md)için bağımlılık izlemeyi ayarlayın.
- [Java](./java-agent.md)için bağımlılık izlemeyi ayarlayın.
- [Özel bağımlılık telemetrisi yazma](./api-custom-events-metrics.md#trackdependency)
- Application Insights türleri ve veri modeli için [veri modeli](data-model.md) 'ne bakın.
- Application Insights tarafından desteklenen [platformları](./platforms.md) inceleyin.

