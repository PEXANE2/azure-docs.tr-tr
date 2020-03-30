---
title: Azure Hizmet Kumaş İzleme ve Tanılama Genel Bakış
description: Azure Hizmet Kumaşı kümeleri, uygulamaları ve hizmetleri için izleme ve tanılama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: ef77810adfab213845c7824740effc3416d85407
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282490"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Azure Hizmet Kumaşı için izleme ve tanılama

Bu makalede, Azure Hizmet Kumaşı için izleme ve tanılama genel bir bakış sağlar. İzleme ve tanılama, iş yüklerinin herhangi bir bulut ortamında geliştirilmesi, test edilmesi ve dağıtılması açısından önemlidir. Örneğin, uygulamalarınızın nasıl kullanıldığını, Service Fabric platformu tarafından gerçekleştirilen eylemleri, performans sayaçlarıyla kaynak kullanımınızı ve kümenizin genel durumunu izleyebilirsiniz. Bu bilgileri sorunları tanılamak ve düzeltmek ve gelecekte oluşmasını önlemek için kullanabilirsiniz. Sonraki birkaç bölüm, üretim iş yüklerini göz önünde bulundurmak üzere Hizmet Kumaşı izlemenin her alanını kısaca açıklayacaktır. 

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="application-monitoring"></a>Uygulama izleme
Uygulama izleme, uygulamanızın özelliklerinin ve bileşenlerinin nasıl kullanıldığını izler. Kullanıcıları etkileyen sorunların yakalandığından emin olmak için uygulamalarınızı izlemek istiyorsunuz. Uygulama izlemenin sorumluluğu, uygulamanızın iş mantığına özgü olduğu için bir uygulamayı ve hizmetlerini geliştiren kullanıcılara bağlıdır. Uygulamalarınızı izlemek aşağıdaki senaryolarda yararlı olabilir:
* Uygulamamne ne kadar trafik yaşanıyor? - Hizmetlerinizi kullanıcı taleplerini karşılamak veya uygulamanızdaki olası bir darboğaza çözüm bulmak için ölçeklendirmeniz mi gerekiyor?
* Servis çağrılarına olan hizmetim başarılı ve izleniyor mu?
* Uygulamamın kullanıcıları tarafından hangi işlemler yapılır? - Telemetri nin toplanması gelecekteki özellik geliştirme ve uygulama hataları için daha iyi tanılama kılavuzu olabilir
* Uygulamam işlenmemiş özel durumlar mı? 
* Konteynerlerimin içinde çalışan hizmetlerde neler oluyor?

Uygulama izleme hakkında çok şey geliştiriciler uygulamanızın bağlamında yaşıyor bu yana istedikleri araçları ve çerçeveyi kullanabilirsiniz! Azure Monitor ile uygulama izleme için Azure çözümü hakkında daha fazla bilgi edinebilirsiniz - [Uygulama Öngörüleri ile Etkinlik analizinde Uygulama Öngörüleri.](service-fabric-diagnostics-event-analysis-appinsights.md)
Ayrıca [.NET Applications için bu kurmak için](service-fabric-tutorial-monitoring-aspnet.md)nasıl bir öğretici var. Bu öğretici, doğru araçları nasıl yükleyip, uygulamanızda özel telemetri yazmak için bir örnek ve Azure portalında uygulama tanılama ve telemetriyi görüntüleme yi geçer. 


## <a name="platform-cluster-monitoring"></a>Platform (Küme) izleme
Bir kullanıcı kodu kendisi yazdığından, uygulamadan hangi telemetrinin geldiğini kontrol altında tutuyor, ancak Service Fabric platformundaki tanılamalar ne olacak? Service Fabric'in hedeflerinden biri, uygulamaları donanım hatalarına karşı esnek tutmaktır. Bu hedefe, platformun sistem hizmetlerinin altyapı sorunlarını algılama ve kümedeki diğer düğümlere iş yüklerinin hızla başarısız olması sayesinde ulaşılır. Ama bu özel durumda, ya sistem hizmetleri kendilerini sorunları varsa? Veya bir iş yükünü dağıtmaya veya taşımaya çalışırken, hizmetlerin yerleşimine yönelik kurallar ihlal edilir mi? Service Fabric, kümenizde gerçekleşen etkinlikler hakkında bilgilendirilmenizi sağlamak için bunlar ve daha fazlası için tanılama sağlar. Küme izleme için bazı örnek senaryolar şunlardır:

Service Fabric kutunun dışında kapsamlı bir olaylar kümesi sağlar. Bu [Hizmet Kumaşı olaylarına](service-fabric-diagnostics-events.md) EventStore veya operasyonel kanal (platform tarafından açıklanan olay kanalı) üzerinden erişilebilir. 

* Service Fabric etkinlik kanalları - Windows'da, Service Fabric etkinlikleri, Operasyonel ve `logLevelKeywordFilters` Veri & Mesajlaşma kanalları arasında seçim yapmak için kullanılan bir dizi alakalı tek bir ETW sağlayıcısından edinilebilir - bu, giden Hizmet Kumaşı olaylarını gerektiğinde filtrelenecek şekilde ayırma şeklimizdir. Linux'ta, Service Fabric olayları LTTng aracılığıyla gelir ve gerektiğinde filtrelenebildikleri tek bir Depolama tablosuna konur. Bu kanallar, kümenizin durumunu daha iyi anlamak için kullanılabilecek seçilmiş, yapılandırılmış olaylar içerir. Tanılama varsayılan olarak küme oluşturma zamanında etkinleştirilir ve bu kanallardan gelen olayların gelecekte sorgulanmanız için gönderildiği bir Azure Depolama tablosu oluşturulur. 

* EventStore - EventStore, Service Fabric Explorer'da ve REST API'de sunulan Service Fabric platform etkinliklerini sağlayan platform tarafından sunulan bir özelliktir. Kümenizde her bir varlık için düğüm, hizmet, uygulama ve sorgu gibi olayın zamanına bağlı olarak neler olup bittiğinin anlık görüntüsünü görebilirsiniz. Ayrıca EventStore Hakkında Daha Fazla Bilgi Için [EventStore'a Genel Bakış'](service-fabric-diagnostics-eventstore.md)ı da edinebilirsiniz.    

![EventStore](media/service-fabric-diagnostics-overview/eventstore.png)

Sağlanan tanılama kutunun dışında olaylar kapsamlı bir dizi şeklindedir. Bu [Hizmet Kumaşı olayları,](service-fabric-diagnostics-events.md) düğümler, Uygulamalar, Hizmetler, Bölümler vb. gibi farklı varlıklarda platform tarafından yapılan eylemleri gösterir. Yukarıdaki son senaryoda, bir düğüm aşağı gitmek olsaydı, platform bir `NodeDown` olay yayar ve seçtiğiniz izleme aracı tarafından hemen haberdar edilebilir. Diğer yaygın `ApplicationUpgradeRollbackStarted` örnekler `PartitionReconfigured` arasında veya bir başarısızlık sırasında yer almaktadır. **Aynı olaylar hem Windows hem de Linux kümelerinde kullanılabilir.**

Olaylar hem Windows hem de Linux'taki standart kanallar aracılığıyla gönderilir ve bunları destekleyen herhangi bir izleme aracı tarafından okunabilir. Azure Monitor çözümü Azure Monitor günlükleridir. Kümeniz için özel bir çalışma panosu ve uyarılar oluşturabileceğiniz bazı örnek sorgular içeren [Azure Monitor günlükleri tümleştirmemiz](service-fabric-diagnostics-event-analysis-oms.md) hakkında daha fazla bilgi almaktan çekinmeyin. Platform düzeyinde etkinlik ve [günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md)da daha fazla küme izleme konsepti mevcuttur.

### <a name="health-monitoring"></a>Sistem durumu izleme
Service Fabric platformu, kümedeki varlıkların durumu için genişletilebilir sağlık raporlaması sağlayan bir sistem durumu modeli içerir. Her düğüm, uygulama, hizmet, bölüm, yineleme veya örnek, sürekli güncellenebilir bir sistem durumu vardır. Sistem durumu "Tamam", "Uyarı" veya "Hata" olabilir. Service Fabric olaylarını kümenin çeşitli varlıklara yaptığı fiiller ve her varlık için bir sıfat olarak düşünün. Belirli bir varlığın sağlığı her geçişte, bir olay da yayılır. Bu şekilde, diğer olaylar gibi seçtiğiniz izleme aracınızda sağlık olayları için sorgular ve uyarılar ayarlayabilirsiniz. 

Ayrıca, kullanıcıların varlıklar için sistem durumunu geçersiz kılmasına bile izin veririz. Uygulamanız bir yükseltmeden geçiyorsa ve doğrulama testlerinde başarısız oluyorsanız, Uygulamanızın artık sağlıklı olmadığını belirtmek için Sağlık API'sini kullanarak Service Fabric Health'e yazabilirsiniz ve Service Fabric yükseltmeyi otomatik olarak geri alacaktır! Sağlık modeli hakkında daha fazla şey için [Service Fabric sağlık izlemesine girişe](service-fabric-health-introduction.md) göz atın

![SFX sistem durumu panosu](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


### <a name="watchdogs"></a>Watchdogs
Genellikle, bir izleme örgütü, sağlık ve hizmetler arasında yük, ping uç noktaları ve kümedeki herhangi bir şey için sağlık raporu ayrı bir hizmettir. Bu, tek bir hizmetin görünümüne dayalı olarak algılanmayacak hataları önlemeye yardımcı olabilir. Watchdogs da kullanıcı etkileşimi olmadan iyileştirici eylemler gerçekleştiren kod barındırmak için iyi bir yerdir (örneğin, belirli zaman aralıklarında depolama günlük dosyaları temizleme). [Burada](https://github.com/Azure-Samples/service-fabric-watchdog-service)bir örnek watchdog hizmet uygulaması bulabilirsiniz.

## <a name="infrastructure-performance-monitoring"></a>Altyapı (performans) izleme
Başvurunuzdaki ve platformdaki tanılamaları ele aldığımıza göre, donanımın beklendiği gibi çalıştığını nasıl bileceğiz? Temel altyapınızı izlemek, kümenizin durumunu ve kaynak kullanımınızı anlamanın önemli bir parçasıdır. Sistem performansını ölçmek, iş yüklerinize bağlı olarak öznel olabilecek birçok etkene bağlıdır. Bu faktörler genellikle performans sayaçları ile ölçülür. Bu performans sayaçları işletim sistemi, .NET çerçevesi veya Service Fabric platformunun kendisi gibi çeşitli kaynaklardan gelebilir. Yararlı olacakları bazı senaryolar

* Donanımımı verimli bir şekilde mi kullanıyorum? Donanımınızı %90 CPU veya %10 CPU'da kullanmak istiyor musunuz? Bu, kümenizi ölçeklerken veya uygulamanızın işlemlerini optimize ederken kullanışlıdır.
* Altyapı sorunlarını proaktif olarak tahmin edebilir miyim? - Birçok sorun performansan ani değişiklikler (düşüşler) öncesinde, bu nedenle proaktif sorunları tahmin etmek ve tanılamak için ağ G/Ç ve CPU kullanımı gibi performans sayaçları kullanabilirsiniz.

Altyapı düzeyinde toplanması gereken performans sayaçlarının listesi [Performans ölçümlerinde](service-fabric-diagnostics-event-generation-perf.md)bulunabilir. 

Service Fabric ayrıca Güvenilir Hizmetler ve Aktörler programlama modelleri için bir dizi performans sayacı da sağlar. Bu modellerden birini kullanıyorsanız, bu performans sayaçları, aktörlerinizin doğru şekilde yukarı ve aşağı döndüklerinden veya güvenilir hizmet isteklerinizin yeterince hızlı ele alındığını sağlamak için bilgi verebilir. Daha fazla bilgi [için, Güvenilir Aktörler için Güvenilir Hizmet Remoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) ve [Performans izleme](service-fabric-reliable-actors-diagnostics.md#performance-counters)için Izleme bakın. 

Bunları toplamak için Azure Monitor çözümü, platform düzeyinde izleme gibi Azure Monitor günlükleridir. Uygun performans sayaçlarını toplamak ve Azure Monitor günlüklerinde görüntülemek için [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) kullanmalısınız.

## <a name="recommended-setup"></a>Önerilen Kurulum
Artık her izleme ve örnek senaryo alanını ele geçirdiğimiz için, Azure izleme araçlarının bir özetini ve yukarıdaki tüm alanları izlemek için gerekli olan kurulumu burada bulabilirsiniz. 

* [Uygulama Öngörüleri](service-fabric-tutorial-monitoring-aspnet.md) ile Uygulama İzleme
* [Diagnostics Agent](service-fabric-diagnostics-event-aggregation-wad.md) ve [Azure Monitor günlükleriyle](service-fabric-diagnostics-oms-setup.md) küme izleme
* Azure Monitor [günlükleri](service-fabric-diagnostics-oms-agent.md) ile altyapı izleme

Ayrıca, gerekli tüm kaynakların ve aracıların dağıtımını otomatikleştirmek için [burada](service-fabric-diagnostics-oms-setup.md#deploy-azure-monitor-logs-with-azure-resource-manager) bulunan örnek ARM şablonunu kullanabilir ve değiştirebilirsiniz. 

## <a name="other-logging-solutions"></a>Diğer günlük çözümleri

Önerdiğimiz iki çözüm, [Azure Monitor günlükleri](service-fabric-diagnostics-event-analysis-oms.md) ve Uygulama [Bilgileri](service-fabric-diagnostics-event-analysis-appinsights.md) Service Fabric ile tümleştirme de oluşturmuş olsa da, birçok etkinlik ETW sağlayıcıları aracılığıyla yazılır ve diğer günlük çözümleriyle genişletilebilir. Ayrıca [Elastik Yığın](https://www.elastic.co/products) (özellikle çevrimdışı bir ortamda bir küme çalıştırmayı düşünüyorsanız), [Dynatrace](https://www.dynatrace.com/)veya tercih ettiğiniz başka bir platformiçine bakmak gerekir. [Burada](service-fabric-diagnostics-partners.md)entegre ortakların bir listesi mevcuttur.

Seçtiğiniz herhangi bir platform için önemli noktalar, kullanıcı arabirimi, sorgulama yetenekleri, özel görselleştirmeler ve mevcut panolar ve izleme deneyiminizi geliştirmek için sağladıkları ek araçlar la ne kadar rahat olduğunuzu içermelidir. 

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı enstrümantize etmeye başlamak için [Uygulama düzeyi etkinliğine ve günlük oluşturma olayına](service-fabric-diagnostics-event-generation-app.md)bakın.
* Monitor ile uygulamanız için Uygulama Öngörüleri'ni ayarlamak [ve Service Fabric'te ASP.NET Çekirdek uygulamasını tanılamak](service-fabric-tutorial-monitoring-aspnet.md)için gereken adımları gözden geçirin.
* [Platform düzeyinde etkinlik](service-fabric-diagnostics-event-generation-infra.md)ve günlük oluşturma da Hizmet Kumaşı'nın size sunduğu platformu ve etkinlikleri izleme hakkında daha fazla bilgi edinin.
* Bir [küme için Azure Monitor günlüklerini Ayarlama'da](service-fabric-diagnostics-oms-setup.md) Hizmet Dokusu ile Azure Monitör günlükleri tümleştirmesini yapılandırın
* Kapsayıcıları izlemek için Azure Monitor günlüklerini nasıl ayarlayizleyeceğinizi öğrenin - [Azure Hizmet Dokusunda Windows Kapsayıcıları için İzleme ve Tanılama](service-fabric-tutorial-monitoring-wincontainers.md).
* [Ortak senaryoları tanılamada](service-fabric-diagnostics-common-scenarios.md) Service Fabric ile örnek tanılama sorunlarına ve çözümlerine bakın
* [Service Fabric diagnostik ortaklarında](service-fabric-diagnostics-partners.md) Servis Kumaşı ile entegre olan diğer tanılama ürünlerine göz atın
* Azure kaynakları için genel izleme önerileri hakkında bilgi edinin - [En İyi Uygulamalar - İzleme ve tanılama.](https://docs.microsoft.com/azure/architecture/best-practices/monitoring) 