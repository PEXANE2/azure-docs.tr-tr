---
title: Uygulama Öngörüleri ile Azure Hizmet Kumaş Olay Analizi
description: Azure Hizmet Kumaşı kümelerinin izlenmesi ve teşhisi için Uygulama Öngörüleri'ni kullanarak olayları görselleştirme ve analiz etme hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 5c80dc9f350fec30469a9bce2ed836c276ef3a7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464762"
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Uygulama Öngörüleri ile etkinlik analizi ve görselleştirme

Azure Monitor'un bir parçası olan Application Insights, uygulama izleme ve tanılama için genişletilebilir bir platformdur. Güçlü bir analiz ve sorgu aracı, özelleştirilebilir pano ve görselleştirmeler ve otomatik uyarı dahil olmak üzere diğer seçenekleri içerir. Application Insights'ın Service Fabric ile tümleştirilmesi, Visual Studio ve Azure portalı için araçlama deneyimlerinin yanı sıra Hizmet Kumaşı'na özgü ölçümleri de içerir ve kapsamlı bir kutudan çıkma günlüğü deneyimi sağlar. Application Insights ile birçok günlük otomatik olarak oluşturulup sizin için toplanmış olsa da, daha zengin bir tanılama deneyimi oluşturmak için uygulamalarınız için daha fazla özel günlüğe kaydetme eklemenizi öneririz.

Bu makale, aşağıdaki sık sorulan soruların ele alınmasına yardımcı olur:

* Başvuru ve hizmetlerimde neler olup bittiğini nasıl bilebilirim ve telemetri leri toplarım?
* Uygulamamı, özellikle de birbirleriyle iletişim kuran hizmetleri nasıl giderebilirim?
* Hizmetlerimin nasıl performans gösterdiğine ilişkin ölçümleri nasıl alabilirim, örneğin sayfa yükleme süresi, HTTP istekleri?

Bu makalenin amacı, Uygulama Öngörüleri içinden nasıl öngörüler ve sorun giderme elde edilebildiğini göstermektir. Hizmet Kumaşı ile Uygulama Öngörülerini nasıl ayarlayıp yapılandırabileceğinizi öğrenmek istiyorsanız, bu [öğreticiye](service-fabric-tutorial-monitoring-aspnet.md)göz atın.

## <a name="monitoring-in-application-insights"></a>Uygulama Öngörülerinde İzleme

Application Insights, Service Fabric kullanırken kutudan zengin bir deneyime sahiptir. Genel bakış sayfasında, Application Insights hizmetinizle ilgili yanıt süresi ve işlenen istek sayısı gibi önemli bilgiler sağlar. Üstteki 'Ara' düğmesini tıklatarak, uygulamanızda son gelen isteklerin listesini görebilirsiniz. Ayrıca, başarısız istekleri burada görebilir ve hangi hataların oluştuğunu tanılayabilirsiniz.

![Uygulama Öngörülerine Genel Bakış](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

Önceki resimdeki sağ panelde, listede iki ana giriş türü vardır: istekler ve olaylar. Bu durumda HTTP istekleri aracılığıyla uygulamanın API'sine yapılan çağrılardır ve olaylar, kodunuzun herhangi bir yerine ekleyebileceğiniz telemetri görevi gören özel olaylardır. Özel etkinlikler ve ölçümler için [Application Insights API'deki](../azure-monitor/app/api-custom-events-metrics.md)uygulamalarınızı daha iyi inceleyebilirsiniz. Bir isteğe tıkladığınızda, Uygulama Öngörüleri Hizmeti Kumaş nuget paketinde toplanan Service Fabric'e özgü veriler de dahil olmak üzere aşağıdaki resimde gösterildiği gibi daha fazla ayrıntı görüntülenir. Bu bilgi, sorun giderme ve uygulamanızın durumunun ne olduğunu bilmek için yararlıdır ve tüm bu bilgiler Application Insights içinde aranabilir

![Uygulama Öngörüleri İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

Application Insights, gelen tüm verilere karşı sorgulama için özel bir görünüme sahiptir. Uygulama Öngörüleri portalına gitmek için Genel Bakış sayfasının üst kısmındaki "Ölçümler Gezgini"ni tıklatın. Burada, Kusto sorgu dilini kullanarak daha önce bahsedilen özel olaylara, isteklere, özel durumlara, performans sayaçlarına ve diğer ölçümlere karşı sorguları çalıştırabilirsiniz. Aşağıdaki örnekte, son 1 saat içinde tüm istekleri gösterir.

![Uygulama Öngörüleri İstek Ayrıntıları](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Application Insights portalının yeteneklerini daha fazla keşfetmek için [Application Insights portal belgelerine](../azure-monitor/app/app-insights-dashboards.md)gidin.

### <a name="configuring-application-insights-with-eventflow"></a>Uygulama Öngörülerini EventFlow ile Yapılandırma

Olayları toplamak için EventFlow kullanıyorsanız, NuGet `Microsoft.Diagnostics.EventFlow.Outputs.ApplicationInsights`paketini içe aktardığınızdan emin olun. *EventFlowConfig.json* *çıktıları* bölümünde aşağıdaki kod gereklidir:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Filtrelerinizde gerekli değişiklikleri yaptığınızdan ve diğer girdileri (ilgili NuGet paketleriyle birlikte) eklediğinden emin olun.

## <a name="application-insights-sdk"></a>Uygulama Öngörüleri SDK

EventFlow ve WAD'nin toplama çözümleri olarak kullanılması önerilir, çünkü tanılama ve izleme için daha modüler bir yaklaşıma izin verirler, örneğin, EventFlow'dan çıktılarınızı değiştirmek istiyorsanız, gerçek enstrümantasyonunuzda herhangi bir değişiklik gerektirmez, sadece config dosyanızda basit bir değişiklik. Ancak, Application Insights'ı kullanmaya yatırım yapmaya karar verirseniz ve farklı bir platforma geçiş olasılığı yoksa, olayları bir araya getirmek ve bunları Application Insights'a göndermek için Application Insights'ın yeni SDK'sını kullanmayı araştırmanız gerekir. Bu, verilerinizi Application Insights'a göndermek için Artık EventFlow'u yapılandırmanız gerekmeyecek, bunun yerine ApplicationInsight'ın Hizmet Kumaşı NuGet paketini yükleyeceğiniz anlamına gelir. Paket le ilgili ayrıntılara [buradan](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)ulabilirsiniz.

[Microservices and Containers için Uygulama Öngörüleri desteği,](https://azure.microsoft.com/blog/app-insights-microservices/) üzerinde çalışılan bazı yeni özellikleri (şu anda hala beta sürümünde) gösterir ve uygulama istatistikleri ile daha zengin out-of-the-box izleme seçeneklerine sahip olmanızı sağlar. Bunlar arasında bağımlılık izleme (bir kümedeki tüm hizmetlerinizin ve uygulamalarınızın bir AppMap'ini oluştururken kullanılan ve bunlar arasındaki iletişim) ve hizmetlerinizden gelen izlemelerin daha iyi korelasyonu bulunmaktadır (iş akışındaki bir sorunu daha iyi belirlemeye yardımcı olur bir uygulama veya hizmet).

.NET'te geliştirme yapıyorsanız ve büyük olasılıkla Service Fabric'in programlama modellerinden bazılarını kullanacaksanız ve etkinlik ve günlük verilerini görselleştirmek ve analiz etmek için Uygulama Öngörülerini platform olarak kullanmak istiyorsanız, Uygulama Öngörüleri üzerinden gitmenizi öneririz İzleme ve tanılama iş akışınız olarak SDK rotası. Günlüklerinizi toplamak ve görüntülemek için Application Insights'ı kullanmaya başlamak için [bunu](../azure-monitor/app/asp-net-more.md) ve [bunu](../azure-monitor/app/asp-net-trace-logs.md) okuyun.

## <a name="navigating-the-application-insights-resource-in-azure-portal"></a>Azure portalında Uygulama Öngörüleri kaynağında gezinme

Uygulama Öngörülerini olaylarınve günlükleriniz için bir çıktı olarak yapılandırıldıktan sonra, bilgiler birkaç dakika içinde Application Insights kaynağınızda görünmeye başlamalıdır. Sizi Application Insights kaynak panosuna götürecek olan Application Insights kaynağına gidin. Aldığı en son izleri görmek ve bunların içinden filtre uygulayabilmek için Uygulama Öngörüleri görev çubuğunda **Arama'yı** tıklatın.

*Metrics Explorer,* uygulamalarınızın, hizmetlerinizin ve kümenizin bildirdiği ölçümlere dayalı özel panolar oluşturmak için kullanışlı bir araçtır. Topladığınız verilere dayanarak kendiniz için birkaç grafik ayarlamak için [Uygulama Öngörülerinde Ölçümleri Keşfetme'ye](../azure-monitor/app/metrics-explorer.md) bakın.

**Analytics'i** tıklatmak sizi, olayları ve izleri daha kapsamlı ve isteğe bağlı olarak sorgulayabileceğiniz Application Insights Analytics portalına götürür. [Analytics in Application Insights'ta](../azure-monitor/app/analytics.md)bu konuda daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

* Performans veya kullanımdaki değişikliklerden haberdar olmak için [AI'deki Uyarıları ayarlama](../azure-monitor/app/alerts.md)
* [Uygulama Öngörülerinde Akıllı Algılama,](../azure-monitor/app/proactive-diagnostics.md) olası performans sorunları konusunda sizi uyarmak için Application Insights'a gönderilen telemetrinin proaktif bir analizini gerçekleştirir
