---
title: Service Fabric kümesine giriş Kaynak Yöneticisi
description: Uygulamanızın hizmetlerinin düzenlemesini yönetmenin bir yolu olan Service Fabric kümesi Kaynak Yöneticisi hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: da9205f5d95eaf1b4dc655ee727ab8a4fe90893d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563335"
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>Service Fabric kümesi kaynak yöneticisi 'ne giriş
Geleneksel olarak BT sistemlerini veya çevrimiçi hizmetler, belirli fiziksel veya sanal makineleri bu hizmetlere veya sistemlere ayırabilirsiniz. Hizmetler, katman olarak tasarlanmıştır. "Web" katmanı ve "veri" veya "depolama" katmanı vardır. Uygulamalar, isteklerin akan ve giden isteklerinin yanı sıra önbelleğe alma için ayrılmış bir makine kümesi olur. Her katmana veya iş yükü türüne ayrılmış belirli makineler vardı: veritabanı, kendisine ayrılmış bir dizi makineye sahiptir ve Web sunucuları birkaç tane olur. Belirli bir iş yükü türü, üzerinde çalıştığı makinelere çok sıcak bir yol varsa, o katmana aynı yapılandırmaya sahip daha fazla makine eklediniz. Ancak, tüm iş yükleri daha kolay bir şekilde, özellikle de veri katmanıyla birlikte, makineleri daha büyük makinelere sahip olacak şekilde ölçeklenmez. Çok basit. Bir makine başarısız olursa, makinenin geri yükleneünceye kadar genel uygulamanın bu bölümü daha düşük kapasitede çalışır. Hala oldukça kolay (eğlenceli değilse).

Ancak, hizmet ve yazılım mimarisinin dünyası değişmiştir. Uygulamaların bir genişleme tasarımını benimsemesi daha yaygındır. Kapsayıcılar veya mikro hizmetler (ya da her ikisi) ile uygulama oluşturma yaygındır. Artık yalnızca birkaç makineniz olabilir, ancak iş yükünün yalnızca tek bir örneğini çalıştırmazlar. Aynı anda birden çok farklı iş yükü de çalıştırıyor olabilirler. Artık, bu hizmetlerin yüzlerce farklı örneği olan düzinelerce farklı hizmet türlerine (bir tam makinenin kaynak adına sahip değil) sahip olursunuz. Her bir adlandırılmış örnek, yüksek kullanılabilirlik (HA) için bir veya daha fazla örneğe veya çoğaltmaya sahiptir. Bu iş yüklerinin boyutlarına ve ne kadar meşgul olduğuna bağlı olarak, kendinizi yüzlerce veya binlerce makine ile bulabilirsiniz. 

Ortamınızı yönetmek, tek bir iş yükü türüne adanmış birkaç makineyi yönetmek kadar basit değildir. Sunucularınız sanal ve artık adlara sahip değil (mindsets 'leri, Evcil hayvanlar 'dan sonra [katlarından](https://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) geçirtiniz). Yapılandırma makineler ve hizmetlerin kendileri hakkında daha azdır. Bir iş yükünün tek bir örneğine adanmış donanımlar büyük ölçüde geçmiş bir şeydir. Hizmetlerin kendisi, daha küçük bir ticari donanım parçasını kapsayan küçük dağıtılmış sistemler haline gelir.

Uygulamanız artık birkaç katmanda tek bir dizi yayılmaya sahip olmadığından, daha fazla bilgi almak için artık daha fazla bileşim vardır. Hangi tür iş yüklerinin hangi donanım üzerinde çalıştırılabileceklerini veya kaç tane olduğuna karar veriyor? Hangi iş yükleri aynı donanımda çalışır ve ne de çakışıyor? Bir makine bu makinede ne çalıştığını nasıl anlarım? İş yükünün yeniden çalışmaya başlamasını kim ediyor? (Sanal?) makinenin geri dönmesini veya iş yüklerinizi otomatik olarak diğer makinelere devreder ve çalışmaya devam edecek şekilde beklesin mi? İnsan müdahalesi gerekli mi? Bu ortamdaki yükseltmeler nelerdir?

Bu ortamda geliştiriciler ve operatörler olarak bu karmaşıklığın yönetilmesine yardımcı olmak istiyoruz. Bir işe alım ve insanlarla karmaşıklığın gizlenme olasılığı büyük olasılıkla doğru cevap değildir, bu nedenle ne yapmalıyım?

## <a name="introducing-orchestrators"></a>Düzenleyicilerine giriş
"Orchestrator", yöneticilerin bu tür ortamları yönetmesine yardımcı olan bir yazılım parçası için genel bir terimdir. Düzenlemeler, "ortamda bu hizmetin beş kopyasını çalıştırmak istiyorum" gibi istekleri alma bileşenleridir. Ne olursa olsun, ortamı istenen durumla eşleştirmeye çalışır.

Yöneticiler, bir makine başarısız olduğunda veya bir iş yükü beklenmeyen bir nedenden dolayı sonlandırıldığında yapılacak işlem (insanlara değil) için gerekli değildir. Çoğu düzenleyen yalnızca hatayla başa çıkmakla kalmaz. Sahip oldukları diğer özellikler yeni dağıtımları yönetiyor, yükseltmeleri işliyor ve kaynak tüketimine ve idare ile ilgilenmekte. Tüm düzenlemeler, ortamda belirli bir yapılandırma durumunun korunmasıyla ilgilidir. Bir Orchestrator 'a istediğiniz şeyi söyleyebilir ve ağır bir kaldırma yapabilmesini istiyorsunuz. Mesos, Docker Datacenter/Docker Sısınma, Kubernetes ve Service Fabric en üstünde Aurora, düzenleyicilerinin her örneğidir. Bu düzenleyiciler, üretim ortamlarında gerçek iş yüklerinin ihtiyaçlarını karşılamak için etkin bir şekilde geliştirilmiştir. 

## <a name="orchestration-as-a-service"></a>Hizmet olarak düzenleme
Küme Kaynak Yöneticisi, Service Fabric düzenleme işleyen sistem bileşenidir. Küme Kaynak Yöneticisi işi üç parçaya ayrılmıştır:

1. Kuralları zorlama
2. Ortamınızı iyileştirme
3. Diğer Işlemlerle yardımcı olma

### <a name="what-it-isnt"></a>Ne değildir?
Geleneksel N katmanı uygulamalarında her zaman bir [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing))vardır. Genellikle bu bir ağ Load Balancer (NLB) veya ağ yığınında nereden gönderildiği yere bağlı olarak bir uygulama Load Balancer (ALB) idi. Bazı yük dengeleyiciler donanım tabanlı F5's BigIP teklifi gibi, diğerleri ise Microsoft 'un NLB gibi yazılım tabanlıdır. Diğer ortamlarda, bu roldeki HAProxy, NGINX, Istio veya Envoy gibi bir şey görebilirsiniz. Bu mimarilerde, Yük Dengeleme işi durum bilgisiz iş yükünün (kabaca) aynı iş miktarını almasını sağlamaktır. Yük farklılaştırmalarının dengelenmesi stratejileri. Bazı dengeleyiciler her farklı çağrıyı farklı bir sunucuya gönderir. Diğer kullanıcılar oturum sabitleme/süreklilik sağladı. Daha gelişmiş dengeleyiciler, gerçek yük tahmini veya raporlamayı, beklenen maliyeti ve geçerli makine yüküne bağlı olarak yönlendirmek üzere kullanır.

Ağ dengeleyiciler veya ileti yönlendiricileri, web/çalışan katmanının kabaca dengeli şekilde kaldığını sağlamaya çalıştı. Veri katmanını dengelemek için stratejiler, veri depolama mekanizmasına göre farklılık görmıştı. Veri katmanını veri parçalama, önbelleğe alma, yönetilen görünümler, saklı yordamlar ve diğer mağazaya özgü mekanizmalar üzerinde bir şekilde dengeleyin.

Bu stratejilerin bazıları ilginç olsa da, Service Fabric Küme Kaynak Yöneticisi ağ yük dengeleyici veya önbellek gibi hiçbir şey değildir. Bir ağ Load Balancer ön uçları, ön uçları kapsayan trafiği yayarak dengeler. Service Fabric kümesi Kaynak Yöneticisi farklı bir stratejiye sahiptir. Temel olarak, Service Fabric *Hizmetleri* en anlamlı oldukları yere taşıdıkça trafik ya da yükün izlenmesi bekleniyor. Örneğin, var olan hizmetler çok fazla iş yapmadığı için Hizmetleri şu anda soğuk olan düğümlere taşıyabilir. Mevcut hizmetler başka bir yerde silindiğinden veya taşındığı için düğümler soğuk olabilir. Diğer bir örnek olarak, Küme Kaynak Yöneticisi bir hizmeti bir makineden uzakta de taşıyabilir. Büyük olasılıkla makine Yükseltilmek üzere veya üzerinde çalışan hizmetler tarafından tüketilene bir ani artış nedeniyle aşırı yüklendi. Alternatif olarak, hizmetin kaynak gereksinimleri artmış olabilir. Sonuç olarak, bu makinede çalıştırmaya devam etmek için yeterli kaynak yok. 

Küme Kaynak Yöneticisi, hizmetlerin etrafında taşınmasından sorumlu olduğundan, ağ yükü dengeleyicisinde buldıklarla karşılaştırıldığında farklı bir özellik kümesi içerir. Bunun nedeni, ağ yükü dengeleyiciler hizmetin kendisini çalıştırmak için ideal olmasa bile, hizmetlerin zaten bulunduğu konuma ağ trafiği sunabilmesidir. Service Fabric Küme Kaynak Yöneticisi, kümedeki kaynakların verimli bir şekilde kullanılmasını sağlamak için temelde farklı stratejiler kullanır.

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi mimari ve bilgi akışı hakkında daha fazla bilgi için [Bu makaleye](service-fabric-cluster-resource-manager-architecture.md) göz atın
- Küme Kaynak Yöneticisi, kümeyi açıklamak için birçok seçenek içerir. Ölçümler hakkında daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Ölçümler, Service Fabric küme kaynağı Yöneticisi 'nin kümedeki tüketimi ve kapasiteyi nasıl yönettiğini açıklamaktadır. Ölçümler ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [Bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın
- Küme Kaynak Yöneticisi Service Fabric yönetim özellikleri ile birlikte kullanılabilir. Bu tümleştirme hakkında daha fazla bilgi edinmek için [Bu makaleyi](service-fabric-cluster-resource-manager-management-integration.md) okuyun
- Kaynak Yöneticisi kümenin, kümedeki yükü nasıl yönettiğini ve dengelemediğini öğrenmek için [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md) sayfasındaki makaleye göz atın
