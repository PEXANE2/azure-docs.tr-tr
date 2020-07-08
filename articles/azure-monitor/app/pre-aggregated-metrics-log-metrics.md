---
title: Azure Application Insights günlük tabanlı ve önceden toplanmış ölçümler | Microsoft Docs
description: Azure Application Insights günlük tabanlı ve önceden toplanmış ölçümleri kullanma
ms.topic: conceptual
author: vgorbenko
ms.author: vitalyg
ms.date: 09/18/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 30487eebed361e5b010df023a9b1a44f96590b14
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81271089"
---
# <a name="log-based-and-pre-aggregated-metrics-in-application-insights"></a>Application Insights’daki günlük tabanlı ve önceden toplanmış ölçümler

Bu makalede, günlükleri temel alan "geleneksel" Application Insights ölçümler ve şu anda genel önizleme aşamasında olan önceden toplanmış ölçümler arasındaki fark açıklanmaktadır. Her iki ölçüm türü de Application Insights kullanıcıları için kullanılabilir ve her biri, uygulama durumunu, tanılamayı ve analizlerini izlemek için benzersiz bir değer getirir. Uygulamaları seçen geliştiriciler, uygulamanın boyutuna, beklenen telemetri hacmine ve ölçüm duyarlılığı ve uyarı için iş gereksinimlerine bağlı olarak belirli bir senaryoya en uygun ölçüm türü olarak karar verebilir.

## <a name="log-based-metrics"></a>Günlük tabanlı ölçümler

Son olarak, Application Insights içindeki uygulama izleme telemetri verileri modeli, istekler, özel durumlar, bağımlılık çağrıları, sayfa görünümleri vb. gibi önceden tanımlanmış çok sayıda olay türünü temel alır. Geliştiriciler SDK 'Yı kullanarak bu olayları el ile (SDK 'Yı açıkça çağıran kodu yazarak) veya otomatik izleme 'den otomatik olay koleksiyonuna güvenebilirler. Her iki durumda da Application Insights arka ucu toplanan tüm olayları günlük olarak depolar ve Azure portal Application Insights dikey pencereleri, olay tabanlı verileri günlüklere görselleştirmede analitik ve Tanılama aracı olarak davranır.

Tüm olayların bir kümesini sürdürmek için günlüklerin kullanılması harika analitik ve tanılama değeri getirebilir. Örneğin, bu çağrıları yapan farklı kullanıcı sayısı ile belirli bir URL 'ye yönelik isteklerin tam sayımını yapabilirsiniz. Ya da herhangi bir Kullanıcı oturumu için özel durumlar ve bağımlılık çağrıları da dahil olmak üzere ayrıntılı tanılama izlemeleri edinebilirsiniz. Bu tür bilgilerin olması, uygulama durumunun ve kullanımının görünürlüğünü önemli ölçüde iyileştirebiliyor ve bir uygulamayla ilgili sorunları tanılamak için gereken süreyi kesmeye izin verir.

Aynı zamanda, büyük bir telemetri hacmi üreten uygulamalar için, tüm olayların bir kümesini toplamak pratik (veya hatta imkansız) olabilir. Olay hacmi çok yüksek olduğunda Application Insights, toplanan ve depolanan olayların sayısını azaltan [örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) ve [filtreleme](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling) gibi çeşitli telemetri hacmi azaltma tekniklerini uygular. Ne yazık ki, depolanan olayların sayısını azaltmak, arka planda saklanan olayların sorgu süresi toplamaları gerçekleştirmelidir.

> [!NOTE]
> Application Insights, günlüklerde depolanan olayların ve ölçümlerin sorgu süresi toplamasını temel alan ölçümler günlük tabanlı ölçümler olarak adlandırılır. Bu ölçümler genellikle olay özelliklerinden çok sayıda boyuta sahiptir ve bunları analiz için üst düzey yapar, ancak bu ölçümlerin doğruluğu örnekleme ve filtreleme tarafından olumsuz etkilenir.

## <a name="pre-aggregated-metrics"></a>Önceden toplanmış ölçümler

Günlük tabanlı ölçümlere ek olarak, geç 2018 ' de Application Insights ekibi, zaman serisi için iyileştirilmiş özel bir depoda depolanan ölçümlerin genel bir önizlemesini sevk ediyor. Yeni ölçümler artık çok sayıda özelliği olan ayrı olaylar olarak tutulmayacaktır. Bunun yerine, önceden toplanmış zaman serisi olarak ve yalnızca anahtar boyutlarıyla depolanır. Bu, sorgu sırasında yeni ölçümlerin üstün olmasını sağlar: verilerin alınması çok daha hızlı gerçekleşir ve daha az işlem gücü gerektirir. Bu nedenle, ölçüm boyutları, daha fazla yanıt veren [panolar](https://docs.microsoft.com/azure/azure-monitor/app/overview-dashboard)ve daha fazlası [hakkında neredeyse gerçek zamanlı uyarı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)gibi yeni senaryolar sağlanır.

> [!IMPORTANT]
> Hem günlük tabanlı hem de önceden toplanmış ölçümler Application Insights. İkisini de ayırt etmek için, Application Insights UX ön toplanmış ölçümler artık "standart ölçümler (Önizleme)" olarak adlandırılmaktadır, ancak olaylar için geleneksel ölçümler "günlük tabanlı ölçümler" olarak yeniden adlandırıldı.

Daha yeni SDK 'lar (.NET için[Application Insights 2,7](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.7.2) SDK veya üzeri), telemetri birimi azaltma teknikleri başlamadan önce koleksiyon sırasında ön toplama ölçümleri. Bu, yeni Ölçümlerin doğruluğu, en son Application Insights SDK 'Ları kullanılırken örnekleme ve filtreleme tarafından etkilenmediği anlamına gelir.

Ön toplamayı uygulamayan SDK 'lar için (yani Application Insights SDK 'ların daha eski sürümleri veya tarayıcı araçları için) Application Insights arka ucu, Application Insights olay toplama uç noktası tarafından alınan olayları toplayarak yeni ölçümleri hala doldurur. Bu, kablo üzerinden iletilen azaltılan veri hacminin avantajlarından faydalanmadığında, önceden toplanmış ölçümleri kullanmaya devam edebilir ve koleksiyon sırasında ölçümleri önceden toplamayan SDK 'lar ile neredeyse gerçek zamanlı boyut uyarısı hakkında daha iyi performans ve destek elde edebilirsiniz.

Koleksiyon uç noktasının, uygulama ile birlikte kullandığınız SDK sürümünden bağımsız olarak önceden toplanmış ölçümlerin doğruluğunu hiçbir şekilde [etkilemeyeceği anlamına](https://docs.microsoft.com/azure/application-insights/app-insights-sampling) gelen, giriş bitiş noktası 'nın olayları önceden topladığından emin olmak önemlidir.  

## <a name="using-pre-aggregation-with-application-insights-custom-metrics"></a>Application Insights özel ölçümlerle ön toplamayı kullanma

Özel ölçümlerle ön toplamayı kullanabilirsiniz. İki ana avantaj, özel bir ölçümün boyutunu yapılandırma ve uyarma ve SDK 'dan Application Insights koleksiyon uç noktasına gönderilen veri hacmini azaltmaktır.

[APPLICATION INSIGHTS SDK 'dan özel ölçümler göndermenin birkaç yolu](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics)vardır. SDK sürümünüz [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) yöntemlerini sunuyorsa, bu durumda yalnızca Azure 'da depolanan veri hacminin azaltılmasından ve ayrıca sdk 'dan Application Insights ' ye aktarılan veri hacminin değil, özel ölçümler göndermek için tercih edilen yöntem budur. Aksi takdirde, veri alımı sırasında ölçüm olaylarını ön toplayan [trackmetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) yöntemini kullanın.

## <a name="custom-metrics-dimensions-and-pre-aggregation"></a>Özel Ölçüm boyutları ve ön toplama

[Trackmetric](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackmetric) veya [GetMetric ile trackvalue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric) API çağrılarını kullanarak göndereceğiniz tüm ölçümler otomatik olarak hem günlüklerde hem de ölçüm depolarında depolanır. Ancak, özel ölçümünüzün günlük tabanlı sürümü her zaman tüm boyutları koruurken, ölçümün önceden toplanmış sürümü, varsayılan olarak herhangi bir boyut olmadan saklanır. "Özel ölçüm boyutlarında uyarı etkinleştir" seçeneğini işaretleyerek [kullanım ve tahmini maliyet](https://docs.microsoft.com/azure/application-insights/app-insights-pricing) sekmesinde özel ölçümlerin boyut toplanmasını etkinleştirebilirsiniz: 

![Kullanım ve tahmini maliyet](./media/pre-aggregated-metrics-log-metrics/001-cost.png)

## <a name="why-is-collection-of-custom-metrics-dimensions-turned-off-by-default"></a>Özel Ölçüm boyutlarının toplanması neden varsayılan olarak kapalı?

Özel Ölçüm boyutlarının toplanması, daha sonra boyutlarla birlikte özel ölçümleri depolamanın Application Insights göre ayrı olarak faturalandırılacağından, boyutlu olmayan özel ölçümlerin depolanması boş kalır (bir kotasına kadar). Resmi [fiyatlandırma sayfamızda](https://azure.microsoft.com/pricing/details/monitor/)yaklaşan fiyatlandırma modeli değişiklikleri hakkında bilgi edinebilirsiniz.

## <a name="creating-charts-and-exploring-log-based-and-standard-pre-aggregated-metrics"></a>Grafik oluşturma ve günlük tabanlı ve standart önceden toplanmış ölçümleri keşfetme

Önceden toplanmış ve günlük tabanlı ölçülerden grafikler çizmek ve panoları grafiklerle yazmak için [Azure izleyici ölçüm Gezgini](../platform/metrics-getting-started.md) kullanın. İstenen Application Insights kaynağını seçtikten sonra standart (Önizleme) ve günlük tabanlı ölçümler arasında geçiş yapmak için ad alanı seçicisini kullanın veya özel bir ölçüm ad alanı seçin:

![Ölçüm ad alanı](./media/pre-aggregated-metrics-log-metrics/002-metric-namespace.png)

## <a name="pricing-models-for-application-insights-metrics"></a>Application Insights ölçümleri için fiyatlandırma modelleri

Günlük tabanlı veya ön toplanmış olan Application Insights ölçümleri, [burada](https://docs.microsoft.com/azure/azure-monitor/app/pricing#pricing-model)açıklandığı gibi, alınan verilerin boyutuna göre maliyetler üretir. Tüm boyutları da dahil olmak üzere özel ölçümleriniz her zaman Application Insights log Store 'da depolanır; Ayrıca, özel ölçümlerinizin önceden toplanmış bir sürümü (hiçbir boyut olmadan) varsayılan olarak ölçüm deposuna iletilir.

Önceden toplanmış ölçümlerin tüm boyutlarını ölçüm deposunda depolamak için [özel ölçüm boyutlarında uyarı etkinleştir](#custom-metrics-dimensions-and-pre-aggregation) seçeneğini belirlediğinizde, [özel ölçüm fiyatlandırmasına](https://azure.microsoft.com/pricing/details/monitor/)göre **ek** maliyetler oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Neredeyse gerçek zamanlı uyarı](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts)
* [GetMetric ve TrackValue](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#getmetric)