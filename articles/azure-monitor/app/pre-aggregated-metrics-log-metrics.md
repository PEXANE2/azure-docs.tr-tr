---
title: Azure Uygulama Öngörüleri'nde günlük tabanlı ve önceden toplanmış ölçümler | Microsoft Dokümanlar
description: Azure Application Insights'ta günlük tabanlı ve önceden toplanmış ölçümleri neden kullanmak için kullanılır?
ms.topic: conceptual
author: vgorbenko
ms.author: vitalyg
ms.date: 09/18/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 30487eebed361e5b010df023a9b1a44f96590b14
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81271089"
---
# <a name="log-based-and-pre-aggregated-metrics-in-application-insights"></a>Application Insights’daki günlük tabanlı ve önceden toplanmış ölçümler

Bu makalede, günlükleri temel alan "geleneksel" Uygulama Öngörüleri ölçümleri ile şu anda genel önizlemede bulunan önceden toplu ölçümler arasındaki fark açıklanmaktadır. Her iki ölçüm türü de Application Insights kullanıcıları tarafından kullanılabilir ve her biri uygulama durumu, tanılama ve analiz izlemede benzersiz bir değer getirir. Uygulamaları enstrümante eden geliştiriciler, uygulamanın boyutuna, beklenen telemetri hacmine ve ölçümler insa ve uyarı için iş gereksinimlerine bağlı olarak, belirli bir senaryoya en uygun metrik türüne karar verebilir.

## <a name="log-based-metrics"></a>Günlük Tabanlı Ölçümler

Yakın zamana kadar, Uygulama Öngörüleri'ndeki uygulama izleme telemetri veri modeli yalnızca istekler, özel durumlar, bağımlılık çağrıları, sayfa görünümleri gibi önceden tanımlanmış az sayıdaolay türüne dayanıyordu. Geliştiriciler, bu olayları el ile (Açıkça SDK'yı çağıran kod yazarak) veya otomatik enstrümantasyondan olayların otomatik olarak toplanmasına güvenmek için SDK'yı kullanabilirler. Her iki durumda da, Application Insights arka uçtan toplanan tüm olayları günlük olarak saklar ve Azure portalındaki Application Insights bıçakları günlüklerden olay tabanlı verileri görselleştirmek için analitik ve tanılama aracı görevi görehareket eder.

Tüm olaylar kümesini korumak için günlükleri kullanmak büyük analitik ve tanısal değer getirebilir. Örneğin, bu aramaları yapan farklı kullanıcı sayısıyla belirli bir URL'ye gelen isteklerin tam sayısını alabilirsiniz. Veya herhangi bir kullanıcı oturumu için özel durumlar ve bağımlılık çağrıları da dahil olmak üzere ayrıntılı tanılama izlemeleri alabilirsiniz. Bu tür bilgilere sahip olmak, uygulama nın sağlığı ve kullanımının görünürlüğünü önemli ölçüde artırarak, bir uygulamayla ilgili sorunları tanılamak için gereken süreyi azaltmaya olanak sağlar.

Aynı zamanda, büyük hacimli telemetri oluşturan uygulamalar için tam bir olay kümesinin toplanması pratik olmayabilir (hatta imkansız olabilir). Olayların hacminin çok yüksek olduğu durumlarda, Application Insights toplanan ve depolanan olayların sayısını azaltan [örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) ve [filtreleme](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) gibi çeşitli telemetri hacmi azaltma teknikleri uygular. Ne yazık ki, depolanan olayların sayısını düşürmek, sahne arkasında günlüklerde depolanan olayların sorgu zamanı toplamalarını gerçekleştirmesi gereken ölçümlerin doğruluğunu da düşürür.

> [!NOTE]
> Uygulama Öngörüleri'nde, günlüklerde depolanan olayların ve ölçümlerin sorgu zamanı toplamasını temel alan ölçümlere günlük tabanlı ölçümler denir. Bu ölçümler genellikle olay özelliklerinden birçok boyuta sahiptir, bu da onları analitik için üstün kılar, ancak bu ölçümlerin doğruluğu örnekleme ve filtrelemeden olumsuz etkilenir.

## <a name="pre-aggregated-metrics"></a>Önceden toplanmış ölçümler

Günlük tabanlı ölçümlere ek olarak, 2018'in sonlarında Application Insights ekibi, zaman serileri için optimize edilmiş özel bir depoda depolanan ölçümlerin genel önizlemesini gönderdi. Yeni ölçümler artık çok sayıda özelle tek tek olaylar olarak tutulmaz. Bunun yerine, önceden toplu zaman serisi olarak ve yalnızca önemli boyutlarda depolanır. Bu, yeni ölçümleri sorgu sırasında üstün kılar: veri alma çok daha hızlı gerçekleşir ve daha az işlem gücü gerektirir. Bu nedenle, [ölçümlerin boyutları,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)daha duyarlı panolar ve daha fazlası hakkında neredeyse gerçek zamanlı uyarı gibi yeni [senaryolar](https://docs.microsoft.com/azure/azure-monitor/app/overview-dashboard)sağlar.

> [!IMPORTANT]
> Her ikisi de, günlük tabanlı ve önceden toplu ölçümler Uygulama Öngörüleri'nde bir arada bulunur. İkisini ayırt etmek için, Application Insights UX'da önceden toplanmış ölçümler artık "Standart ölçümler (önizleme)" olarak adlandırılırken, olaylardaki geleneksel ölçümler "Günlük tabanlı ölçümler" olarak yeniden adlandırıldı.

Telemetri hacim azaltma teknikleri devreye girmeden önce toplama sırasında yeni[SDK'lar (Application Insights 2.7](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.7.2) SDK veya daha sonra .NET için) önceden toplu ölçümler devreye sokuldu. Bu, yeni ölçümlerin doğruluğunun en son Application Insights SDK'larını kullanırken örnekleme ve filtrelemeden etkilenmeyeceğimanlamına gelir.

Ön toplama yı (diğer bir şekilde Uygulama Öngörüleri SDK'larının eski sürümleri veya tarayıcı araçları için) uygulamayan SDK'lar için Application Insights arka ucu, Application Insights olay toplama bitiş noktası tarafından alınan olayları toplayarak yeni ölçümleri doldurmaya devam eder. Bu, kablo üzerinden iletilen veri hacminin azalmasından yararlanmasanız da, önceden toplanmış ölçümleri kullanmaya devam edebileceğiniz ve toplama sırasında önceden toplamlaştırma yapmayan SDK'larla yakın gerçek zamanlı boyutsal uyarının daha iyi performans ve destek le karşılaşabileceğiniz anlamına gelir.

Toplama bitiş noktasının, yutma örneklemesinden önce olayları önceden biraraya getiren, yani [yutma örneklemesinin](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) uygulamanızla birlikte kullandığınız SDK sürümünden bağımsız olarak önceden toplanmış ölçümlerin doğruluğunu asla etkilemeyeceğini belirtmekte yarar vardır.  

## <a name="using-pre-aggregation-with-application-insights-custom-metrics"></a>Uygulama Öngörüleri özel ölçümleri ile ön toplama kullanma

Özel ölçümlerle ön toplamayı kullanabilirsiniz. İki ana avantajı, özel bir metrik boyutu üzerinde yapılandırma ve uyarı ve SDK'dan Application Insights toplama bitiş noktasına gönderilen veri hacmini azaltmaktır.

[Uygulama Öngörüleri SDK'dan özel ölçümler göndermenin](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics)çeşitli yolları vardır. SDK sürümünüz [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) yöntemlerini sunuyorsa, bu durumda ön toplama Yalnızca Azure'da depolanan veri hacmini değil, Aynı zamanda SDK'dan Application Insights'a aktarılan veri hacmini de azaltmakla birlikte, SDK'nın içinde olduğundan, özel ölçümler göndermenin tercih edilen yoludur. Aksi takdirde, veri alımı sırasında metrik olayları önceden toplayacak [trackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) yöntemini kullanın.

## <a name="custom-metrics-dimensions-and-pre-aggregation"></a>Özel ölçümler boyutları ve ön toplama

[TrackMetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) veya [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) API çağrılarını kullanarak gönderdiğiniz tüm ölçümler hem günlükhem de ölçüm mağazalarında otomatik olarak depolanır. Ancak, özel metnizin günlük tabanlı sürümü her zaman tüm boyutları korurken, ölçümün önceden toplanmış sürümü varsayılan olarak boyutsuz olarak depolanır. "Özel metrik boyutlarda uyarıyı etkinleştir"i işaretleyerek [kullanımda özel](https://docs.microsoft.com/azure/application-insights/app-insights-pricing) ölçümlerin boyutlarının ve tahmini maliyet sekmesinin toplanmasını açabilirsiniz: 

![Kullanım ve tahmini maliyet](./media/pre-aggregated-metrics-log-metrics/001-cost.png)

## <a name="why-is-collection-of-custom-metrics-dimensions-turned-off-by-default"></a>Özel ölçümlerin toplanması boyutları varsayılan olarak neden kapatılır?

Gelecekte özel ölçümleri boyutlarla depolamak Application Insights'tan ayrı olarak faturalandırılırken, boyutsal olmayan özel ölçümlerin depolanması (kotaya kadar) serbest kalacaktır. Yaklaşan fiyatlandırma modeli değişiklikleri hakkında resmi [fiyatlandırma sayfamızdan](https://azure.microsoft.com/pricing/details/monitor/)bilgi alabilirsiniz.

## <a name="creating-charts-and-exploring-log-based-and-standard-pre-aggregated-metrics"></a>Grafik oluşturma ve günlük tabanlı ve standart önceden toplanmış ölçümleri keşfetme

Önceden toplanmış ve günlük tabanlı ölçümlerden grafikler çizmek ve grafiklerle panolar oluşturmak için [Azure Monitor Metrics Explorer'ı](../platform/metrics-getting-started.md) kullanın. İstenilen Application Insights kaynağını seçtikten sonra, standart (önizleme) ve günlük tabanlı ölçümler arasında geçiş yapmak için ad alanı seçiciyi kullanın veya özel bir metrik ad alanı seçin:

![Metrik ad alanı](./media/pre-aggregated-metrics-log-metrics/002-metric-namespace.png)

## <a name="pricing-models-for-application-insights-metrics"></a>Uygulama Öngörüleri ölçümleri için fiyatlandırma modelleri

Günlük tabanlı veya önceden toplanmış olsun, Uygulama Öngörüleri'ne ölçümler inme, [burada](https://docs.microsoft.com/azure/azure-monitor/app/pricing#pricing-model)açıklandığı gibi, yutulan verilerin boyutuna göre maliyetler oluşturur. Tüm boyutları da dahil olmak üzere özel ölçümleriniz her zaman Application Insights günlük deposunda depolanır; ayrıca, özel ölçümlerinizin önceden toplanmış bir sürümü (boyutsuz) varsayılan olarak ölçüm deposuna iletilir.

Önceden toplanmış ölçümlerin tüm boyutlarını metrik mağazada depolamak için [özel metrik boyutlarda uyarı etkinleştir](#custom-metrics-dimensions-and-pre-aggregation) seçeneğini seçmek, Özel Ölçümler [fiyatlandırmasına](https://azure.microsoft.com/pricing/details/monitor/)dayalı **ek** maliyetler oluşturabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Neredeyse gerçek zamanlı uyarı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)
* [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric)