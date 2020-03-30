---
title: Azure Site Kurtarma ile Ağ Güvenlik Grupları | Microsoft Dokümanlar
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Kurtarma ile Ağ Güvenlik Gruplarının nasıl kullanılacağını açıklar
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: eb5ba99133f5726c44164b0ba45b7ab5d94e292f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292371"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Azure Site Kurtarma ile Ağ Güvenlik Grupları

Ağ Güvenlik Grupları, ağ trafiğini sanal ağdaki kaynaklarla sınırlamak için kullanılır. [Ağ Güvenlik Grubu (NSG),](../virtual-network/security-overview.md#network-security-groups) kaynak veya hedef IP adresi, bağlantı noktası ve protokole dayalı gelen veya giden ağ trafiğine izin veren veya reddeden güvenlik kurallarının bir listesini içerir.

Kaynak Yöneticisi dağıtım modeli altında, NSG'ler alt ağlarla veya tek tek ağ arabirimleriyle ilişkilendirilebilir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Trafik, bir NSG'yi zaten ilişkili bir NSG'ye sahip bir alt ağ içindeki tek ağ arabirimleriyle ilişkilendirerek de sınırlandırılabilir.

Bu makalede, Azure Site Kurtarma ile Ağ Güvenlik Grupları'nı nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="using-network-security-groups"></a>Ağ Güvenlik Gruplarını Kullanma

Tek bir alt ağ sıfır veya bir ilişkili NSG olabilir. Tek bir ağ arabirimi de sıfır veya bir, ilişkili NSG olabilir. Yani, bir NSG'yi önce bir alt ağa, sonra da VM'nin ağ arabirimine başka bir NSG'yi ilişkilendirerek sanal bir makine için çift trafik kısıtlamasına sahip olabilirsiniz. Bu durumda NSG kurallarının uygulanması trafiğin yönü ve uygulanan güvenlik kurallarının önceliğine bağlıdır.

Aşağıdaki gibi bir sanal makine ile basit bir örnek düşünün:
-    Sanal makine **Contoso Subnet**içine yerleştirilir.
-    **Contoso Subnet** **Subnet NSG**ile ilişkilidir.
-    VM ağ arabirimi ayrıca **VM NSG**ile ilişkilidir.

![Site Kurtarma ile NSG](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

Bu örnekte, gelen trafik için önce Subnet NSG değerlendirilir. Subnet NSG ile izin verilen herhangi bir trafik daha sonra VM NSG tarafından değerlendirilir. Ters giden trafik için geçerlidir, vm NSG ilk değerlendirilmektedir. VM NSG ile izin verilen herhangi bir trafik daha sonra Subnet NSG tarafından değerlendirilir.

Bu, tanecikli güvenlik kuralı uygulamasına izin verir. Örneğin, bir alt ağ altında birkaç uygulama VM'sine (ön uç VM'leri gibi) gelen internet erişimine izin vermek, ancak gelen internet erişimini diğer VM'lere (veritabanı ve diğer arka uç VM'ler gibi) kısıtlamak isteyebilirsiniz. Bu durumda, Internet trafiğine izin veren Subnet NSG üzerinde daha yumuşak bir kural alabilirsiniz ve VM NSG'ye erişimi reddederek belirli VM'lere erişimi kısıtlayabilirsiniz. Aynı giden trafik için de uygulanabilir.

Bu tür NSG yapılandırmaları ayarlarken, doğru önceliklerin [güvenlik kurallarına](../virtual-network/security-overview.md#security-rules)uygulandığından emin olun. Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.

Her zaman ağ güvenlik gruplarının hem bir ağ arabirimine hem de alt ağa uygulandığının farkında olmayabilirsiniz. Ağ arabiriminin [etkili güvenlik kurallarını](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) görüntüleyerek ağ arabirimine uygulanan toplu kuralları doğrulayabilirsiniz. Azure Ağ İzleyicisi'ndeki [Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) IP [akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) özelliğini, bir ağ arabirimine veya ağ arabiriminden iletişime izin verilip verilmediğini belirlemek için de kullanabilirsiniz. Bu araç iletişime izin verilip verilmediğini ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

## <a name="on-premises-to-azure-replication-with-nsg"></a>NSG ile Azure çoğaltma için şirket içi

Azure Site Kurtarma, şirket içi [Hyper-V sanal makineleri,](hyper-v-azure-architecture.md) [VMware sanal makineleri](vmware-azure-architecture.md)ve [fiziksel sunucular](physical-azure-architecture.md)için olağanüstü durum kurtarma ve Azure'a geçiş sağlar. Azure senaryolarının tüm şirket içi için çoğaltma verileri bir Azure Depolama hesabına gönderilir ve depolanır. Çoğaltma sırasında sanal makine ücreti ödemezsiniz. Azure'da bir hata yaptığınızda, Site Kurtarma otomatik olarak Azure IaaS sanal makineleri oluşturur.

Sanal M'ler Azure'a geçildikten sonra oluşturulduktan sonra, NSG'ler ağ trafiğini sanal ağ ve SANAL'larla sınırlamak için kullanılabilir. Site Kurtarma, başarısız işlemin bir parçası olarak NSG'ler oluşturmaz. Başarısız olmadan önce gerekli Azure NSG'lerini oluşturmanızı öneririz. Daha sonra, Otomasyon komut dosyalarını Site Kurtarma'nın güçlü [kurtarma planlarıyla](site-recovery-create-recovery-plans.md)kullanarak NSG'leri başarısız olurken, başarısız olduğu için NSG'leri otomatik olarak başarısız lığa bağlayabilirsiniz.

Örneğin, başarısız sonrası VM yapılandırması yukarıda açıklanan [örnek senaryoya](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) benziyorsa:
-    **Contoso VNet** ve **Contoso Subnet'i** hedef Azure bölgesinde DR planlamasının bir parçası olarak oluşturabilirsiniz.
-    Ayrıca, aynı DR planlamasının bir parçası olarak hem **Subnet NSG** hem de **VM NSG** oluşturabilir ve yapılandırabilirsiniz.
-    **Subnet NSG** sonra hemen **Contoso Subnet**ile ilişkili olabilir , Hem NSG ve alt ağ zaten mevcuttur gibi.
-    **VM NSG** kurtarma planları kullanarak başarısız sırasında VM ile ilişkili olabilir.

NSG'ler oluşturulduktan ve yapılandırıldıktan sonra, komut dosyası yla yazılmış NSG ilişkilendirmelerini ve başarısız lık sonrası VM bağlantısını doğrulamak için bir [test başarısızlığı](site-recovery-test-failover-to-azure.md) çalıştırmanızı öneririz.

## <a name="azure-to-azure-replication-with-nsg"></a>NSG ile Azure'dan Azure'a çoğaltma

Azure Site Kurtarma, [Azure sanal makinelerin](azure-to-azure-architecture.md)olağanüstü kurtarma sını sağlar. Azure Sanal M'leri için çoğaltmayı etkinleştirirken, Site Kurtarma hedef bölgede yineleme sanal ağları (alt ağlar ve ağ geçidi alt ağları dahil) oluşturabilir ve kaynak ve hedef sanal ağlar arasında gerekli eşlemeleri oluşturabilir. Ayrıca, hedef yan ağları ve alt ağları önceden oluşturabilir ve çoğaltmayı etkinleştirirken aynı şeyi kullanabilirsiniz. Site [Kurtarma, başarısız](azure-to-azure-tutorial-failover-failback.md)olmadan önce hedef Azure bölgesinde herhangi bir VM oluşturmaz.

Azure VM çoğaltma için, kaynak Azure bölgesindeki NSG kurallarının çoğaltma trafiği için [giden bağlantıya](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) izin verdiğinden emin olun. Ayrıca, bu [örnek NSG yapılandırması](azure-to-azure-about-networking.md#example-nsg-configuration)aracılığıyla bu gerekli kuralları sınayabilir ve doğrulayabilirsiniz.

Site Kurtarma, başarısız işlemin bir parçası olarak NSG'ler oluşturmaz veya çoğaltmaz. Başarısız olmadan önce hedef Azure bölgesinde gerekli NSG'leri oluşturmanızı öneririz. Daha sonra, Otomasyon komut dosyalarını Site Kurtarma'nın güçlü [kurtarma planlarıyla](site-recovery-create-recovery-plans.md)kullanarak NSG'leri başarısız olurken, başarısız olduğu için NSG'leri otomatik olarak başarısız lığa bağlayabilirsiniz.

Daha önce açıklanan [örnek senaryo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) göz önüne alındığında:
-    Site Kurtarma, VM için çoğaltma etkinleştirildiğinde hedef Azure bölgesinde **Contoso VNet** ve **Contoso Subnet'in** kopyalarını oluşturabilir.
-    Hedef Azure bölgesinde, hedef Azure bölgesinde **Subnet NSG** ve **VM NSG'nin** (sırasıyla **Hedef Altağ NSG** ve **Hedef VM NSG**olarak adlandırılır) istenilen kopyalarını oluşturarak hedef bölgede gereken ek kurallara izin verebilirsiniz.
-    **Hedef Subnet NSG,** hem NSG hem de alt ağ zaten mevcut olduğundan, hedef bölge alt ağıyla hemen ilişkilendirilebilir.
-    **Hedef VM NSG** kurtarma planları kullanarak başarısız sırasında VM ile ilişkili olabilir.

NSG'ler oluşturulduktan ve yapılandırıldıktan sonra, komut dosyası yla yazılmış NSG ilişkilendirmelerini ve başarısız lık sonrası VM bağlantısını doğrulamak için bir [test başarısızlığı](azure-to-azure-tutorial-dr-drill.md) çalıştırmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
-    [Ağ Güvenlik Grupları](../virtual-network/security-overview.md#network-security-groups)hakkında daha fazla bilgi edinin.
-    NSG güvenlik [kuralları](../virtual-network/security-overview.md#security-rules)hakkında daha fazla bilgi edinin.
-    Bir NSG için [etkili güvenlik kuralları](../virtual-network/diagnose-network-traffic-filter-problem.md) hakkında daha fazla bilgi edinin.
-    Uygulama başarısızlığını otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.
