---
title: Hizmet Kumaş Küme Kaynak Yöneticisi Tanıtımı
description: Uygulamanızın hizmetlerinin düzenlenmesini yönetmenin bir yolu olan Service Fabric Cluster Resource Manager hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: da9205f5d95eaf1b4dc655ee727ab8a4fe90893d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75563335"
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Service Fabric küme kaynak yöneticisi tanıtımı
Geleneksel olarak BT sistemlerini veya çevrimiçi hizmetleri yönetmek, belirli fiziksel veya sanal makineleri belirli hizmetlere veya sistemlere adamak anlamına geliyordu. Hizmetler katman lar olarak tasarlandı. Bir "web" katmanı ve bir "veri" veya "depolama" katmanı olacaktır. Uygulamalar, isteklerin giriş ve çıkışyaptığı bir ileti katmanına ve önbelleğe almaya adanmış bir makine kümesine sahip olacaktır. Her katman veya iş yükü türüne adanmış belirli makineler vardı: veritabanı, web sunucuları birkaç adanmış bir çift makine var. Belirli bir iş yükü türü, üzerinde çalıştığı makinelerin çok sıcak çalışmasına neden olduysa, aynı yapılandırmaya sahip daha fazla makineyi bu katmana eklediniz. Ancak, tüm iş yükleri bu kadar kolay ölçeklendirilebilir - özellikle veri katmanı ile genellikle daha büyük makineler ile makineleri değiştirmek istiyorsunuz. Kolay. Bir makine başarısız olduysa, genel uygulamanın bu bölümü, makine geri yüklenene kadar daha düşük kapasitede çalıştırılan ayarı. Hala oldukça kolay (mutlaka eğlenceli değilse).

Şimdi ancak hizmet ve yazılım mimarisi nin dünya değişti. Uygulamaların ölçeklendirilmiş bir tasarımı benimsemesi daha yaygındır. Konteyner veya mikro hizmetler (veya her ikisi) ile bina uygulamaları yaygındır. Şimdi, hala sadece birkaç makine olabilir, onlar bir iş yükü sadece tek bir örnek çalışmıyor. Hatta aynı anda birden çok farklı iş yükü çalıştırıyor olabilirler. Şimdi hizmet düzinelerce var (hiçbiri tam bir makinenin kaynakları değerinde tüketen), bu hizmetlerin belki de yüzlerce farklı örnekleri. Adlandırılmış her örnek, Yüksek Kullanılabilirlik (HA) için bir veya daha fazla örneği veya yinelemesi vardır. Bu iş yüklerinin boyutlarına ve ne kadar meşgul olduklarına bağlı olarak, kendinizi yüzlerce veya binlerce makineyle bulabilirsiniz. 

Aniden çevrenizi yönetmek, tek tür iş yüklerine adanmış birkaç makineyi yönetmek kadar kolay değildir. Sunucularınız sanaldır ve artık isimleri yoktur (tüm sonra [evcil hayvanlardan sığırlara](https://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) zihniyetler değiştirdiniz). Yapılandırma makinelerhakkında daha az ve hizmetlerin kendileri hakkında daha fazla. İş yükünün tek bir örneğine adanmış donanım büyük ölçüde geçmişte kaldı. Hizmetlerin kendileri, birden fazla küçük emtia donanımını kapsayan küçük dağıtılmış sistemler haline gelmiştir.

Uygulamanız artık çeşitli katmanlara yayılmış bir monolit serisi olmadığından, artık uğraşmanız gereken çok daha fazla kombinasyon var. Hangi iş yük türlerinin hangi donanımda veya kaç taneden çalıştırılayabildiğine kim karar verir? Hangi iş yükleri aynı donanımda iyi çalışır ve hangi çakışma? Bir makine battığında o makinede ne olduğunu nereden biliyorsun? İş yükünün yeniden çalışmaya başladığından kim sorumludur? (Sanal?) makinenin geri gelmesini mi bekliyorsunuz yoksa iş yüklerinizin otomatik olarak diğer makinelere akıp çalışmaya devam etmesini mi istiyorsunuz? İnsan müdahalesi gerekli midir? Peki ya bu ortamdaki yükseltmeler?

Bu ortamda işlem yapan geliştiriciler ve operatörler olarak, bu karmaşıklığı yönetmek için yardım isteyeceğiz. Bir işe alem ve insanlarla karmaşıklığı gizlemek için çalışıyor muhtemelen doğru cevap değil, bu yüzden ne yapacağız?

## <a name="introducing-orchestrators"></a>Orkestratörleri tanıtın
"Orkestratör" yöneticilerin bu tür ortamları yönetmesine yardımcı olan bir yazılım parçası için genel bir terimdir. Orkestratörler, "Bu hizmetin çevremde çalışan beş kopyasını istiyorum" gibi isteklerde bulunan bileşenlerdir. Ne olursa olsun ortamın istenilen duruma uymasını sağlamaya çalışırlar.

Orkestratörler (insanlar değil), bir makine arızalandığında veya iş yükü beklenmedik bir nedenle sona erdiğinde harekete geçer. Çoğu orkestracı başarısızlıkla uğraşmaktan fazlasını yapar. Sahip oldukları diğer özellikler, yeni dağıtımları yönetmek, yükseltmeleri işlemek ve kaynak tüketimi ve yönetimle uğraşmaktır. Tüm orkestratörler temelde çevrede bazı istenilen yapılandırma durumunu korumak la ilgilidir. Bir orkestracıya ne istediğini söyleyebilmek ve ağır kaldırmayı yapmak istiyorsun. Mesos, Docker Datacenter/Docker Swarm, Kubernetes ve Service Fabric'in tepesindeki Aurora orkestratörlerin örnekleridir. Bu orkestratörler, üretim ortamlarında gerçek iş yüklerinin ihtiyaçlarını karşılamak için aktif olarak geliştirilmektedir. 

## <a name="orchestration-as-a-service"></a>Bir hizmet olarak orkestrasyon
Küme Kaynak Yöneticisi, Service Fabric'te orkestrasyon işleyen sistem bileşenidir. Küme Kaynak Yöneticisi'nin işi üç bölüme ayrılmıştır:

1. Kuralları Uygulama
2. Ortamınızı Optimize Etme
3. Diğer Süreçlere Yardımcı Olmak

### <a name="what-it-isnt"></a>Ne değil ki
Geleneksel N katmanı uygulamalarında, yük [dengeleyicisi](https://en.wikipedia.org/wiki/Load_balancing_(computing))her zaman vardır. Genellikle bu ağ yığınında oturdu nerede bağlı olarak bir Ağ Yük Dengeleyici (NLB) veya Uygulama Yük Dengeleyici (ALB) oldu. Bazı yük dengeleyicileri F5'in BigIP teklifi gibi Donanım tabanlıdır, diğerleri Microsoft'un NLB'si gibi yazılım tabanlıdır. Diğer ortamlarda, bu rolde HAProxy, nginx, Istio veya Envoy gibi bir şey görebilirsiniz. Bu mimarilerde, yük dengelemenin görevi, devletsiz iş yüklerinin (kabaca) aynı miktarda iş almasını sağlamaktır. Yükü dengeleme stratejileri çeşitlidir. Bazı bakiyeciler her farklı aramayı farklı bir sunucuya gönderir. Diğerleri oturum sabitleme / yapışkanlık sağladı. Daha gelişmiş dengeleyiciler, beklenen maliyeti ve geçerli makine yükünü temel alan bir aramayı yönlendirmek için gerçek yük tahmini veya raporlama kullanır.

Ağ dengeleyicileri veya ileti yönlendiricileri web/çalışan katmanının kabaca dengeli kalmasını sağlamaya çalıştı. Veri katmanını dengeleme stratejileri farklıydı ve veri depolama mekanizmasına bağlıydı. Veri katmanını dengeleme, veri parçalama, önbelleğe alma, yönetilen görünümler, depolanan yordamlar ve depoya özgü diğer mekanizmalara dayanıyordu.

Bu stratejilerden bazıları ilginç olsa da, Hizmet Kumaş Küme Kaynak Yöneticisi ağ yük dengeleyicisi veya önbellek gibi bir şey değildir. Ağ Yük Dengeleyicisi, trafiği ön uçlara yayarak ön uçları dengeler. Service Fabric Cluster Kaynak Yöneticisi'nin farklı bir stratejisi vardır. Temelde, Service Fabric *hizmetleri* en mantıklı oldukları yere taşır, trafik veya yükün takip etmesini bekleyiş. Örneğin, hizmetleri şu anda soğuk olan düğümlere taşıyabilir, çünkü var olan hizmetler çok fazla iş yapmıyor. Mevcut hizmetler silindiğinden veya başka bir yere taşındığından düğümler soğuk olabilir. Başka bir örnek olarak, Küme Kaynak Yöneticisi de bir hizmeti makineden uzağa taşıyabilir. Belki de makine yükseltilmek üzere, ya da üzerinde çalışan hizmetler tarafından tüketimde bir ani artış nedeniyle aşırı yüklenir. Alternatif olarak, hizmetin kaynak gereksinimleri artmış olabilir. Sonuç olarak, bu makinede çalıştırmaya devam etmek için yeterli kaynak yoktur. 

Küme Kaynak Yöneticisi hizmetleri taşımaktan sorumlu olduğundan, ağ yük dengeleyicisinde bulabileceğinizle karşılaştırıldığında farklı bir özellik kümesi içerir. Bunun nedeni, ağ yük dengeleyicilerinin, bu konumun hizmeti çalıştırmak için ideal olmasa bile, hizmetlerin zaten bulunduğu yerlere ağ trafiği sağlamasıdır. Service Fabric Cluster Kaynak Yöneticisi, kümedeki kaynakların verimli bir şekilde kullanılmasını sağlamak için temelde farklı stratejiler kullanır.

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi içindeki mimari ve bilgi akışı hakkında bilgi için [bu makaleye](service-fabric-cluster-resource-manager-architecture.md) göz atın
- Küme Kaynak Yöneticisi kümeyi açıklamak için birçok seçenek vardır. Ölçümler hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş ı kümesini tanımlama](service-fabric-cluster-resource-manager-cluster-description.md) yla ilgili bu makaleye göz atın
- Hizmetleri yapılandırma hakkında daha fazla bilgi [için, Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Ölçümler, Service Fabric Cluster Resource Manger'ın kümedeki tüketimi ve kapasiteyi nasıl yönettiğidir. Ölçümler hakkında daha fazla bilgi edinmek ve bunları nasıl yapılandıracağınız hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın
- Küme Kaynak Yöneticisi, Service Fabric'in yönetim yetenekleriyle çalışır. Bu tümleştirme hakkında daha fazla bilgi edinmek için [bu makaleyi](service-fabric-cluster-resource-manager-management-integration.md) okuyun
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
