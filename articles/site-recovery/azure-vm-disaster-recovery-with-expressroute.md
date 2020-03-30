---
title: Azure ExpressRoute Azure VM olağanüstü durum kurtarmayı Azure Site Kurtarma ile tümleştirin
description: Azure Site Kurtarma ve Azure ExpressRoute kullanarak Azure VM'leri için olağanüstü durum kurtarma nın nasıl ayarlanır?
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: bf12a5b7850a56d945e1082be6c522c31738669c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73954094"
---
# <a name="integrate-expressroute-with-disaster-recovery-for-azure-vms"></a>Azure VM'leri için Olağanüstü Durum Kurtarma ile ExpressRoute'u tümleştir


Bu makalede, Azure VM'leri için olağanüstü durum kurtarmayı ikincil bir Azure bölgesine ayarladığınızda Azure ExpressRoute ile Azure [Site Kurtarma](site-recovery-overview.md)ile nasıl tümleştirilen açıklanmaktadır.

Site Kurtarma, Azure VM verilerini Azure'da kopyalayarak Azure VM'lerinin olağanüstü kurtarmasını sağlar.

- Azure Sanal Taşıtları [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md)kullanıyorsa, VM verileri ikincil bölgede çoğaltılmış yönetilen bir diske çoğaltılır.
- Azure Sanal Taşıtları yönetilen diskleri kullanmıyorsa, VM verileri bir Azure depolama hesabına çoğaltılır.
- Çoğaltma bitiş noktaları herkese açıktır, ancak Azure VM'lerinin çoğaltma trafiği Internet'ten geçmez.

ExpressRoute, bir bağlantı sağlayıcısı tarafından kolaylaştırılan özel bir bağlantı üzerinden şirket içi ağları Microsoft Azure bulutuna genişletmenize olanak tanır. ExpressRoute yapılandırıldıysanız, Site Kurtarma ile aşağıdaki gibi tümleştirir:

- **Azure bölgeleri arasında çoğaltma sırasında**: Azure VM olağanüstü durum kurtarma için çoğaltma trafiği yalnızca Azure'dadır ve ExpressRoute'a çoğaltma için gerekli veya kullanılmaz. Ancak, bir şirket içi siteden birincil Azure sitesindeki Azure VM'lerine bağlantı kuruyorsanız, bu Azure VM'leri için olağanüstü durum kurtarma ayarlarken dikkat edilmesi gereken bazı sorunlar vardır.
- **Azure bölgeleri arasında başarısız olmak**: Kesintiler oluştuğunda, birincilden ikincil Azure bölgesine kadar Azure VM'leri üzerinde başarısız olursunuz. İkincil bir bölgeye ulaşamadıktan sonra, ExpressRoute'u kullanarak ikincil bölgedeki Azure VM'lere erişmek için atılması gereken birkaç adım vardır.


## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce aşağıdaki kavramları anladığınızdan emin olun:

- ExpressRoute [devreleri](../expressroute/expressroute-circuit-peerings.md)
- ExpressRoute [yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#routingdomains)
- ExpressRoute [konumları](../expressroute/expressroute-locations.md).
- Azure VM [çoğaltma mimarisi](azure-to-azure-architecture.md)
- Azure VM'ler için [çoğaltma nasıl ayarlayın.](azure-to-azure-tutorial-enable-replication.md)
- Azure VM'ler [üzerinde nasıl başarısız olunulur?](azure-to-azure-tutorial-failover-failback.md)


## <a name="general-recommendations"></a>Genel öneriler

En iyi uygulama ve olağanüstü durum kurtarma için verimli Kurtarma Süresi Hedefleri (RTO' lar) sağlamak için, ExpressRoute ile tümleştirmek üzere Site Kurtarma'yı ayarlarken aşağıdakileri yapmanızı öneririz:

- İkincil bir bölgeye devrilmeden önce ağ bileşenlerini sağlama:
    - Azure VM'leri için çoğaltmayı etkinleştirdiğinizde, Site Kurtarma, kaynak ağ ayarlarını temel alan hedef Azure bölgesinde ağlar, alt ağlar ve ağ geçitleri gibi ağ kaynaklarını otomatik olarak dağıtabilir.
    - Site Kurtarma, VNet ağ geçitleri gibi ağ kaynaklarını otomatik olarak ayarlayamamaktadır.
    - Bu ek ağ kaynaklarını başarısız olmadan önce sağlamanızı öneririz. Küçük bir kapalı kalma süresi bu dağıtımla ilişkilidir ve dağıtım planlaması sırasında bunu hesaba katmadıysanız, genel kurtarma süresini etkileyebilir.
- Düzenli felaket kurtarma matkapları çalıştırın:
    - Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. RTO'u olumsuz etkileyebilen son dakika yapılandırma sorunlarının önlenmesine yardımcı olur.
    - Alıştırma için bir test başarısızlığı çalıştırdığınızda, çoğaltmayı etkinleştirdiğinizde ayarlanan varsayılan ağ yerine ayrı bir Azure VM ağı kullanmanızı öneririz.
- Tek bir ExpressRoute devreniz varsa farklı IP adresi alanları kullanın.
    - Hedef sanal ağ için farklı bir IP adresi alanı kullanmanızı öneririz. Bu, bölgesel kesintiler sırasında bağlantı kurarken sorunları önler.
    - Ayrı bir adres alanı kullanamıyorsanız, olağanüstü durum kurtarma sondaj testini farklı IP adreslerine sahip ayrı bir test ağında çalıştırdığınızdan emin olun. Aynı ExpressRoute devresine çakışan IP adresi alanına sahip iki VNet'i bağlayamadığınız bir şekilde bağlayabilirsiniz.

## <a name="replicate-azure-vms-when-using-expressroute"></a>ExpressRoute kullanırken Azure VM'lerini çoğaltma


Birincil bir sitede Azure VM'leri için çoğaltma ayarlamak istiyorsanız ve ExpressRoute üzerinden şirket içi sitenizden bu VM'lere bağlanıyorsanız, yapmanız gerekenler şunlardır:

1. Her Azure VM için [çoğaltmayı etkinleştirin.](azure-to-azure-tutorial-enable-replication.md)
2. İsteğe bağlı olarak Site Kurtarma ağ kurmasına izin:
    - Çoğaltmayı yapılandırDığınızda ve etkinleştirdiğinizde, Site Kurtarma, hedef Azure bölgesindeki ağları, alt ağları ve ağ geçidi alt ağlarını kaynak bölgedekilerle eşleşecek şekilde ayarlar. Site Kurtarma da kaynak ve hedef sanal ağlar arasında haritalar.
    - Site Kurtarma'nın bunu otomatik olarak yapmasını istemiyorsanız, çoğaltmayı etkinleştirmeden önce hedef tarafındaki ağ kaynaklarını oluşturun.
3. Diğer ağ öğeleri oluşturma:
    - Site Kurtarma, ikincil bölgedeki rota tabloları, VNet ağ geçitleri, VNet ağ geçidi bağlantıları, VNet eşleme veya diğer ağ kaynakları ve bağlantıları oluşturmaz.
    - Birincil bölgeden bir başarısızlık çalıştırmadan önce, ikincil bölgede bu ek ağ öğelerini oluşturmanız gerekir.
    - Bu ağ kaynaklarını ayarlamak ve bağlamak için [kurtarma planlarını](site-recovery-create-recovery-plans.md) ve otomasyon komut dosyalarını kullanabilirsiniz.
1. Ağ trafiğinin akışını denetlemek için dağıtılan bir ağ sanal cihazınız (NVA) varsa, aşağıdakileri unutmayın:
    - Azure VM çoğaltma için Azure varsayılan sistem rotası 0.0.0.0/0'dır.
    - Genellikle, NVA dağıtımları, giden Internet trafiğini NVA'dan akmaya zorlayan varsayılan bir rota (0.0.0.0/0) da tanımlar. Varsayılan rota, başka belirli bir rota yapılandırması bulunamadığında kullanılır.
    - Bu durumda, tüm çoğaltma trafiği NVA'dan geçerse NVA aşırı yüklenmiş olabilir.
    - Aynı sınırlama, tüm Azure VM trafiğini şirket içi dağıtımlara yönlendirmek için varsayılan yolları kullanırken de geçerlidir.
    - Bu senaryoda, çoğaltma trafiğinin Azure sınırını terk etmemesi için Microsoft.Storage hizmeti için sanal ağınızda [bir ağ hizmeti bitiş noktası oluşturmanızı](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) öneririz.

## <a name="replication-example"></a>Çoğaltma örneği

Genellikle kurumsal dağıtımların iş yükleri birden çok Azure VNet'e bölünür ve internete ve şirket içi sitelere harici bağlantı için merkezi bir bağlantı hub'ı vardır. Bir hub ve kollu topoloji genellikle ExpressRoute ile birlikte kullanılır.

![ExpressRoute ile şirket içinde azure'a geçilmeden önce](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

- **Bölge**. Uygulamalar Azure Doğu Asya bölgesinde dağıtılır.
- **Konuştu vNets**. Uygulamalar iki kollu vNets dağıtılır:
    - **Kaynak vNet1**: 10.1.0.0/24.
    - **Kaynak vNet2**: 10.2.0.0/24.
    - Konuşan her sanal ağ **Hub vNet'e**bağlıdır.
- **Hub vNet**. Bir hub vNet **Kaynak Hub vNet**var: 10.10.10.0/24.
  - Bu hub vNet kapı bekçisi olarak görev eder.
  - Alt ağlardaki tüm iletişimler bu merkezden geçer.
    - **Hub vNet alt ağları**. Hub vNet'in iki alt ağı vardır:
    - **NVA alt net**: 10.10.10.0/25. Bu alt ağ bir NVA içerir (10.10.10.10).
    - **Ağ geçidi alt ağı**: 10.10.10.128/25. Bu alt ağ, özel bir yönlendirme etki alanı üzerinden şirket içi siteye yönlendiren bir ExpressRoute bağlantısına bağlı bir ExpressRoute ağ geçidi içerir.
- Şirket içi veri merkezi, Hong Kong'daki bir iş ortağı kenarından ExpressRoute devre bağlantısına sahiptir.
- Tüm yönlendirme, Azure rota tabloları (UDR) aracılığıyla denetlenir.
- vNet'ler veya şirket içi veri merkezine giden tüm giden trafik NVA üzerinden yönlendirilir.

### <a name="hub-and-spoke-peering-settings"></a>Hub ve spoke eşleme ayarları

#### <a name="spoke-to-hub"></a>Uçtan merkeze

**Yön** | **Ayar** | **Durum**
--- | --- | ---
Uçtan merkeze | Sanal ağ adresine izin ver | Etkin
Uçtan merkeze | İlileli trafiğe izin ver | Etkin
Uçtan merkeze | Ağ geçidi geçişine izin ver | Devre dışı
Uçtan merkeze | Ağ geçitlerini kaldırma'yı kullanma | Etkin

 ![Hub eşleme yapılandırmasına spoke](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

#### <a name="hub-to-spoke"></a>Merkezden uca

**Yön** | **Ayar** | **Durum**
--- | --- | ---
Merkezden uca | Sanal ağ adresine izin ver | Etkin
Merkezden uca | İlileli trafiğe izin ver | Etkin
Merkezden uca | Ağ geçidi geçişine izin ver | Etkin
Merkezden uca | Ağ geçitlerini kaldırma'yı kullanma | Devre dışı

 ![Hub'dan konuşan eşleme yapılandırmasına](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="example-steps"></a>Örnek adımlar

Örneğimizde, kaynak ağdaki Azure VM'leri için çoğaltma etkinleştirirken aşağıdakiler gerçekleştirilmelidir:

1. Bir VM için [çoğaltmayı etkinleştirin.](azure-to-azure-tutorial-enable-replication.md)
2. Site Kurtarma, hedef bölgede çoğaltma vNet'leri, alt ağları ve ağ geçidi alt ağları oluşturur.
3. Site Kurtarma, kaynak ağlar ve oluşturduğu yineleme hedef ağları arasında eşlemeler oluşturur.
4. Sanal ağ ağ geçitlerini, sanal ağ ağ ağ geçidi bağlantılarını, sanal ağ eşlemini veya diğer ağ kaynaklarını veya bağlantılarını el ile oluşturursunuz.


## <a name="fail-over-azure-vms-when-using-expressroute"></a>ExpressRoute kullanırken Azure VM'ler üzerinde başarısız olun

Azure VM'leri Site Kurtarma'yı kullanarak hedef Azure bölgesine geçemedikten sonra, ExpressRoute [özel eşlemesini](../expressroute/expressroute-circuit-peerings.md#privatepeering)kullanarak bunlara erişebilirsiniz.

- ExpressRoute'u yeni bir bağlantıyla hedef vNet'e bağlamanız gerekir. Varolan ExpressRoute bağlantısı otomatik olarak aktarılmaz.
- ExpressRoute bağlantınızı hedef vNet'e kurma şekliniz ExpressRoute topolojinize bağlıdır.


### <a name="access-with-two-circuits"></a>İki devre ile erişim

#### <a name="two-circuits-with-two-peering-locations"></a>İki eşleme konumuna sahip iki devre

Bu yapılandırma, ExpressRoute devrelerinin bölgesel felaketlere karşı korunmasına yardımcı olur. Birincil eşleme konumunuz aşağı iniyorsa, bağlantılar diğer konumdan devam edebilir.

- Üretim ortamına bağlı devre genellikle birincildir. İkincil devre genellikle bir felaket oluşursa artırılabilir düşük bant genişliği vardır.
- Başarısız olduktan sonra, ikincil ExpressRoute devresinden hedef vNet'e bağlantılar kurabilirsiniz. Alternatif olarak, genel kurtarma süresini azaltmak için, felaket durumunda bağlantılar kurulabilir ve hazır olabilirsiniz.
- Hem birincil hem de hedef vNet'lere eşzamanlı bağlantılarla, şirket içi yönlendirmenizin yalnızca devre dışı kalmadan sonra ikincil devreyi ve bağlantıyı kullandığından emin olun.
- Kaynak ve hedef vNet'ler başarısız olduktan sonra yeni IP adresleri alabilir veya aynı adresleri tutabilir. Her iki durumda da, ikincil bağlantılar başarısız olmadan önce kurulabilir.


#### <a name="two-circuits-with-single-peering-location"></a>Tek bir eşleme konumuna sahip iki devre

Bu yapılandırma birincil ExpressRoute devresinin arızalanmasına karşı korumaya yardımcı olur, ancak tek ExpressRoute eşleme konumu aşağı iner ve her iki devreyi de etkilemez.

- Birincil devre ile kaynak vNEt ve ikincil devre ile hedef vNet için şirket içi veri merkezinden eşzamanlı bağlantılar alabilirsiniz.
- Birincil ve hedefe eşzamanlı bağlantılarla, şirket içi yönlendirmenin yalnızca devre dışı kalmadan sonra ikincil devreyi ve bağlantıyı kullandığından emin olun.
-   Devreler aynı eşleme konumunda oluşturulduğunda her iki devreyi de aynı vNet'e bağlayamazsınız.

### <a name="access-with-a-single-circuit"></a>Tek bir devre ile erişim

Bu yapılandırmada sadece bir Expressroute devresi vardır. Devrenin iniş durumunda gereksiz bir bağlantısı olsa da, tek bir rota devresi, bakan bölgeniz inerse esneklik sağlamaz. Şunlara dikkat edin:

- Azure VM'lerini [aynı coğrafi konumdaki](azure-to-azure-support-matrix.md#region-support)herhangi bir Azure bölgesine çoğaltabilirsiniz. Hedef Azure bölgesi kaynakla aynı konumda değilse, tek bir ExpressRoute devresi kullanıyorsanız ExpressRoute Premium'u etkinleştirmeniz gerekir. [ExpressRoute konumları](../expressroute/expressroute-locations.md) ve [ExpressRoute fiyatlandırması](https://azure.microsoft.com/pricing/details/expressroute/)hakkında bilgi edinin.
- Hedef bölgede aynı IP adresi alanı kullanılıyorsa, kaynak ve hedef vNet'leri aynı anda devreye bağlayamaz. Bu senaryoda:    
    -  Kaynak yan bağlantıyı kesip hedef yan bağlantıyı kurun. Bu bağlantı değişikliği, Site Kurtarma kurtarma planının bir parçası olarak komut dosyası olabilir. Şunlara dikkat edin:
        - Bölgesel bir hatada, birincil bölgeye erişilemiyorsa, kesme işlemi başarısız olabilir. Bu, hedef bölgeye bağlantı oluşturmayı etkileyebilir.
        - Bağlantıyı hedef bölgede oluşturduysanız ve birincil bölge daha sonra kurtarırsa, aynı adres alanına bağlanmaya çalışan iki eşzamanlı bağlantı varsa paket düşüşleri yaşayabilirsiniz.
        - Bunu önlemek için birincil bağlantıyı hemen sonlandırın.
        - VM birincil bölgeye geri döndükten sonra, ikincil bağlantıyı kestikten sonra birincil bağlantı yeniden kurulabilir.
-   Hedef vNet'te farklı bir adres alanı kullanılıyorsa, aynı ExpressRoute devresinden aynı anda kaynağa ve hedef vNet'lere bağlanabilirsiniz.


## <a name="failover-example"></a>Failover örneği

Örneğimizde, aşağıdaki topolojiyi kullanıyoruz:

- İki farklı bakış noktasında iki farklı ExpressRoute devresi.
- Azure VM'leri için özel IP adreslerini başarısız olduktan sonra saksayın.
- Hedef kurtarma bölgesi Azure Güneydoğu Asya'dır.
- Singapur'da bir ortak kenarı üzerinden ikincil bir ExpressRoute devre bağlantısı kurulur.

Tek bir ExpressRoute devresi kullanan basit bir topoloji için, başarısız olduktan sonra aynı IP adresi ile, [bu makaleyi gözden geçirin.](site-recovery-retain-ip-azure-vm-failover.md#hybrid-resources-full-failover)

### <a name="example-steps"></a>Örnek adımlar
Bu örnekte kurtarmayı otomatikleştirmek için yapmanız gerekenler şunlardır:

1. Çoğaltmayı ayarlamak için adımları izleyin.
2. [Azure VM'ler üzerinde](azure-to-azure-tutorial-failover-failback.md)başarısız olun , bu ek adımlar sırasında veya sonrasında başarısız olun.

    a. Hedef bölge hub'ı VNet'te Azure ExpressRoute Ağ Geçidi'ni oluşturun. Bu expressroute devresine hedef hub vNet bağlamak gerekir.

    b. Hedef hub vNet'ten hedef ExpressRoute devresine bağlantı oluşturun.

    c. VNet eşlemelerini hedef bölgenin hub'ı ile konuşan sanal ağlar arasında ayarlayın. Hedef bölgedeki görüntü özellikleri, kaynak bölgedeki özelliklerle aynı olacaktır.

    d. Hub VNet'te ÜT'leri ayarlayın ve ikisi VNets'i konuştu.

    - Hedef yan ÜTR'lerin özellikleri, aynı IP adreslerini kullanırken kaynak taraftakilerle aynıdır.
    - Farklı hedef IP adreslerinde, ÜVER'ler buna göre değiştirilmelidir.


Yukarıdaki adımlar bir [kurtarma planının](site-recovery-create-recovery-plans.md)bir parçası olarak komut dosyası olabilir. Uygulama bağlantısı ve kurtarma süresi gereksinimlerine bağlı olarak, yukarıdaki adımlar da başarısız başlatmadan önce tamamlanabilir.

#### <a name="after-recovery"></a>İyileşme den sonra

VM'leri geri kazanıp bağlantıyı tamamladıktan sonra kurtarma ortamı aşağıdaki gibidir.

![Failover sonrası ExpressRoute ile şirket içi Azure](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)



## <a name="next-steps"></a>Sonraki adımlar

Uygulama başarısızlığını otomatikleştirmek için [kurtarma planlarını](site-recovery-create-recovery-plans.md) kullanma hakkında daha fazla bilgi edinin.
