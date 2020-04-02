---
title: Azure Kubernetes Hizmetinde (AKS) yüksek kullanılabilirlik ve olağanüstü durum kurtarma
description: Azure Kubernetes Hizmetinde (AKS) yüksek kullanılabilirlik sağlayarak ve olağanüstü durum kurtarma için hazırlık sağlayarak uygulamalarınız için maksimum çalışma süresi elde etmek için bir küme operatörünün en iyi uygulamalarını öğrenin.
services: container-service
author: lastcoolnameleft
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: thfalgou
ms.custom: fasttrack-edit
ms.openlocfilehash: 3c1f0bb715b3c3bf9b3a3350ab11e26834aa84c8
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528654"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) iş sürekliliği ve olağanüstü durum kurtarma için en iyi uygulamalar

Azure Kubernetes Hizmeti'nde (AKS) kümeleri yönetirken, uygulama çalışma süresi önemli hale gelir. AKS, bir kullanılabilirlik kümesinde birden çok düğüm kullanarak yüksek kullanılabilirlik sağlar. Ancak bu birden çok düğüm sisteminizi bir bölge hatasından korumaz. Çalışma sürenizi en üst düzeye çıkarmak için, iş sürekliliğini korumak ve olağanüstü durum kurtarma için hazırlanmak için önceden plan layın.

Bu makalede, AKS iş sürekliliği ve felaket kurtarma için planlamak için nasıl odaklanır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Birden çok bölgede AKS kümeleri için planlayın.
> * Azure Trafik Yöneticisi'ni kullanarak trafiği birden çok küme arasında yönlendirin.
> * Kapsayıcı görüntü kayıt defterleriniz için coğrafi çoğaltma kullanın.
> * Birden çok küme arasında uygulama durumu için planlayın.
> * Depolamayı birden çok bölgede çoğaltın.

## <a name="plan-for-multiregion-deployment"></a>Çok bölgeli dağıtım planı

**En iyi yöntem**: Birden çok AKS kümesi dağıtırken, AKS'nin kullanılabildiği bölgeleri seçin ve eşleştirilmiş bölgeleri kullanın.

Bir AKS kümesi tek bir bölgeye dağıtılır. Sisteminizi bölge hatasından korumak için, uygulamanızı farklı bölgelerdeki birden çok AKS kümesine dağıtın. AKS kümenizi nereye dağıtacağınızı planlarken şunları göz önünde bulundurun:

* [**AKS bölge kullanılabilirliği**](https://docs.microsoft.com/azure/aks/quotas-skus-regions#region-availability): Kullanıcılarınıza yakın bölgeleri seçin. AKS sürekli olarak yeni bölgelere doğru genişlemektedir.
* [**Azure eşleştirilmiş bölgeler**](https://docs.microsoft.com/azure/best-practices-availability-paired-regions): Coğrafi bölgeniz için, birbiriyle eşleştirilmiş iki bölge seçin. Eşli bölgeler platform güncelleştirmelerini koordine eder ve gerektiğinde kurtarma çabalarına öncelik verir.
* **Servis durumu**: Eşleştirilmiş bölgelerinizin sıcak/sıcak mı, sıcak/sıcak mı yoksa sıcak/soğuk mu olacağına karar verin. Her iki bölgeyi de aynı anda çalıştırmak ve bir bölge trafiğe hizmet vermeye *hazır* mı? Ya da bir bölgenin trafiğe hazır olmak için zamanı olmasını mı istiyorsunuz?

AKS bölgesinin kullanılabilirliği ve eşleştirilmiş bölgeler ortak bir husustur. AKS kümelerinizi bölge olağanüstü durum kurtarmayı birlikte yönetmek üzere tasarlanmış eşleştirilmiş bölgelere dağıtın. Örneğin, AKS Doğu ABD ve Batı ABD'de kullanılabilir. Bu bölgeler eşlenmiştir. Aks BC/DR stratejisi oluştururken bu iki bölgeyi seçin.

Uygulamanızı dağıttığınızda, bu birden çok AKS kümelerine dağıtmak için CI/CD ardışık sisteminize bir adım daha ekleyin. Dağıtım ardışık durumlarınızı güncelleştirmezseniz, uygulamalar bölgenizden ve AKS kümelerinizden yalnızca birine dağıtılabilir. İkinci lüksbir bölgeye yönelik müşteri trafiği en son kod güncelleştirmelerini almaz.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Trafiği yönlendirmek için Azure Trafik Yöneticisi'ni kullanın

**En iyi yöntem**: Azure Trafik Yöneticisi, müşterileri en yakın AKS kümelerine ve uygulama örneklerine yönlendirebilir. En iyi performans ve artıklık için, AKS kümenize gitmeden önce tüm uygulama trafiğini Trafik Yöneticisi'ne yönlendirin.

Farklı bölgelerde birden çok AKS kümeniz varsa, her kümede çalışan uygulamalara trafik akışını denetlemek için Trafik Yöneticisi'ni kullanın. [Azure Trafik Yöneticisi,](https://docs.microsoft.com/azure/traffic-manager/) ağ trafiğini bölgeler arasında dağıtabilen DNS tabanlı bir trafik yük dengeleyicisidir. Kullanıcıları küme yanıt süresine veya coğrafyaya göre yönlendirmek için Trafik Yöneticisi'ni kullanın.

![Trafik Müdürü ile AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Tek bir AKS kümesine sahip müşteriler genellikle belirli bir uygulamanın hizmet IP veya DNS adına bağlanır. Çok kümeli dağıtımda, müşterilerin her AKS kümesindeki hizmetlere işaret eden bir Trafik Yöneticisi DNS adına bağlanması gerekir. Trafik Yöneticisi uç noktalarını kullanarak bu hizmetleri tanımlayın. Her uç nokta *hizmet yük dengeleyici*IP'dir. Bir bölgedeki Trafik Yöneticisi bitiş noktasından farklı bir bölgedeki bitiş noktasına ağ trafiğini yönlendirmek için bu yapılandırmayı kullanın.

![Trafik Yöneticisi aracılığıyla coğrafi yönlendirme](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Trafik Yöneticisi DNS aramalarını gerçekleştirir ve kullanıcının en uygun bitiş noktasını döndürür. İç içe profiller birincil konuma öncelik verebilir. Örneğin, kullanıcılar genellikle en yakın coğrafi bölgeye bağlanmalıdır. Bu bölgede bir sorun varsa, Trafik Yöneticisi bunun yerine kullanıcıları ikincil bir bölgeye yönlendirir. Bu yaklaşım, müşterilerin en yakın coğrafi bölgeleri kullanılamamış olsa bile bir uygulama örneğine bağlanabilmesini sağlar.

Uç noktaların ve yönlendirmenin nasıl ayarlanış ı hakkında bilgi [için](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method)bkz.

### <a name="layer-7-application-routing-with-azure-front-door-service"></a>Azure Ön Kapı Hizmeti ile Katman 7 uygulama yönlendirmesi

Trafik Yöneticisi trafiği şekillendirmek için DNS (katman 3) kullanır. [Azure Ön Kapı Hizmeti,](https://docs.microsoft.com/azure/frontdoor/front-door-overview) http/HTTPS (katman 7) yönlendirme seçeneği sağlar. Azure Ön Kapı Hizmetinin ek özellikleri arasında TLS sonlandırma, özel alan adı, web uygulama güvenlik duvarı, URL Yeniden Yazma ve oturum yakınlığı yer almaktadır. Hangi çözümün en uygun olduğunu anlamak için uygulama trafiğinizin gereksinimlerini gözden geçirin.

### <a name="interconnect-regions-with-global-virtual-network-peering"></a>Küresel sanal ağ eşlemeli bölgeleri birbirine bağlama

Kümelerin birbirleriyle konuşması gerekiyorsa, her iki sanal ağı birbirine bağlamak [sanal ağ eşleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)yoluyla elde edilebilir. Bu teknoloji, farklı coğrafi bölgelerde bile Microsoft'un omurga ağında yüksek bant genişliği sağlayarak sanal ağları birbirine bağlar.

AKS kümelerinin çalıştığı sanal ağlara bakmanın ön koşulu, AkS kümenizdeki standart Yük Dengeleyicisini kullanmaktır, böylece Kubernetes hizmetlerine sanal ağ eşlemesinde ulaşılabilir.

## <a name="enable-geo-replication-for-container-images"></a>Konteyner görüntüleri için coğrafi çoğaltmayı etkinleştirme

**En iyi yöntem**: Kapsayıcı resimlerinizi Azure Kapsayıcı Kayıt Defteri'nde saklayın ve kayıt defterini her AKS bölgesine coğrafi olarak çoğaltın.

Uygulamalarınızı AKS'de dağıtmak ve çalıştırmak için kapsayıcı görüntülerini depolamanın ve çekmenin bir yolunu bulabilmeniz gerekir. Kapsayıcı Kayıt Defteri AKS ile entegre olur, böylece kapsayıcı resimlerinizi veya Miğfer grafiklerinizi güvenli bir şekilde saklayabilir. Kapsayıcı Kayıt Defteri, resimlerinizi dünya çapındaki Azure bölgelerine otomatik olarak çoğaltmak için çok kullanışlı coğrafi çoğaltmayı destekler. 

Performansı ve kullanılabilirliği artırmak için, AKS kümenizin bulunduğu her bölgede bir kayıt defteri oluşturmak için Konteyner Kayıt Defteri coğrafi çoğaltmasını kullanın. Her AKS kümesi daha sonra aynı bölgedeki yerel kapsayıcı kayıt defterinden kapsayıcı görüntülerini çeker:

![Konteyner görüntüleri için Konteyner Kayıt Defteri coğrafi çoğaltma](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Aynı bölgeden görüntüleri çekmek için Konteyner Kayıt Defteri coğrafi çoğaltma kullandığınızda, sonuçlar şunlardır:

* **Daha Hızlı**: Aynı Azure bölgesindeki yüksek hızlı, düşük gecikmeli ağ bağlantılarından görüntüler çekersiniz.
* **Daha güvenilir**: Bir bölge kullanılamıyorsa, AKS kümeniz görüntüleri kullanılabilir bir kapsayıcı kayıt defterinden çeker.
* **Daha ucuz**: Veri merkezleri arasında ağ çıkış ücreti yoktur.

Coğrafi çoğaltma *Premium* SKU konteyner kayıtlarının bir özelliğidir. Coğrafi çoğaltmanın nasıl yapılandırılabildiğini öğrenmek için [Bkz. Konteyner Kayıt Defteri coğrafi çoğaltma.](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

## <a name="remove-service-state-from-inside-containers"></a>Servis durumunu iç kapsayıcılardan kaldırma

**En iyi uygulama**: Mümkün olduğunda servis durumunu konteynerin içinde saklamayın. Bunun yerine, çok bölgeli çoğaltmayı destekleyen bir hizmet (PaaS) olarak bir Azure platformu kullanın.

*Hizmet durumu,* bir hizmetin çalışması için gereken bellek veya disk içi verileri ifade eder. Durum, hizmetin okuduğu ve yazdığı veri yapılarını ve üye değişkenleri içerir. Hizmetin nasıl tasarlanacağına bağlı olarak, durum diskte depolanan dosyalar veya diğer kaynaklar da içerebilir. Örneğin, durum, veritabanının veri ve işlem günlüklerini depolamak için kullandığı dosyaları içerebilir.

Durum dışsallaştırılabilir veya durumu manipüle eden kodla birlikte kullanılabilir. Genellikle, ağ üzerinden farklı makinelerde çalışan veya aynı makinede işlem biten bir veritabanı veya başka bir veri deposu kullanarak durumu dışa aktarAbilirsiniz.

Kapsayıcılar ve mikro hizmetler, içlerinde çalışan işlemler durumu korumadığında en esnek tir. Uygulamalar hemen hemen her zaman bazı durumları içerdiğinden, MySQL için Azure Veritabanı, PostgreSQL için Azure Veritabanı veya Azure SQL Veritabanı gibi bir PaaS çözümü kullanın.

Taşınabilir uygulamalar oluşturmak için aşağıdaki yönergelere bakın:

* [12 faktörlü uygulama metodolojisi](https://12factor.net/)
* [Birden çok Azure bölgesinde bir web uygulaması çalıştırma](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Depolama geçiş planı oluşturma

**En iyi yöntem**: Azure Depolama kullanıyorsanız, depolama alanınızı birincil bölgeden yedekleme bölgesine nasıl geçirilen leri hazırlayın ve sınayın.

Uygulamalarınız verileri için Azure Depolama'yı kullanabilir. Uygulamalarınız farklı bölgelerdeki birden çok AKS kümesine yayıldığı için depolama alanını eşitlemeniz gerekir. Depolamayı çoğaltmanın iki yaygın yolu şunlardır:

* Altyapı tabanlı eşzamanlı çoğaltma
* Uygulama tabanlı asynchronous çoğaltma

### <a name="infrastructure-based-asynchronous-replication"></a>Altyapı tabanlı eşzamanlı çoğaltma

Uygulamalarınız, bir bölme silindikten sonra bile kalıcı depolama gerektirebilir. Kubernetes'te, veri depolamayı sürdürmek için kalıcı birimler kullanabilirsiniz. Kalıcı birimler bir düğüm VM'ye monte edilir ve daha sonra bölmelere maruz kalır. Bölmeler aynı küme içinde farklı bir düğüme taşınsa bile kalıcı birimler bölmeleri izler.

Kullandığınız çoğaltma stratejisi depolama çözümüne bağlıdır. [Gluster,](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/) [Ceph,](https://docs.ceph.com/docs/master/cephfs/disaster-recovery/) [Rook](https://rook.io/docs/rook/v1.2/ceph-disaster-recovery.html)ve [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) gibi ortak depolama çözümleri felaket kurtarma ve çoğaltma hakkında kendi rehberlik sağlar.

Tipik strateji, uygulamaların verilerini yazabileceği ortak bir depolama noktası sağlamaktır. Bu veriler daha sonra bölgeler arasında çoğaltılır ve yerel olarak erişilir.

![Altyapı tabanlı eşzamanlı çoğaltma](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Azure Yönetilen Diskler kullanıyorsanız, çoğaltma ve dr çözümlerini şu gibi seçebilirsiniz:

* [Azure'da Velero](https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Uygulama tabanlı asynchronous çoğaltma

Kubernetes şu anda uygulama tabanlı eşzamanlı çoğaltma için yerel bir uygulama sağlar. Kapsayıcılar ve Kubernetes gevşek birleştiğinden, herhangi bir geleneksel uygulama veya dil yaklaşımı çalışması gerekir. Genellikle, uygulamaların kendileri, her kümenin temel veri depolamasına yazılan depolama isteklerini çoğaltır.

![Uygulama tabanlı asynchronous çoğaltma](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS kümeleri için iş sürekliliği ve olağanüstü durum kurtarma hususları üzerinde duruluyor. AKS'deki küme işlemleri hakkında daha fazla bilgi için en iyi uygulamalar la ilgili şu makalelere bakın:

* [Çok luluk ve küme yalıtımı][aks-best-practices-cluster-isolation]
* [Temel Kubernetes zamanlayıcı özellikleri][aks-best-practices-scheduler]

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
