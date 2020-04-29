---
title: Azure Site Recovery ile ağ güvenlik grupları | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ağ güvenlik gruplarının nasıl kullanılacağını açıklar
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: eb5ba99133f5726c44164b0ba45b7ab5d94e292f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80292371"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Azure Site Recovery ile ağ güvenlik grupları

Ağ güvenlik grupları, ağ trafiğini bir sanal ağ içindeki kaynaklarla sınırlamak için kullanılır. Bir [ağ güvenlik grubu (NSG)](../virtual-network/security-overview.md#network-security-groups) , kaynak veya hedef IP adresi, bağlantı noktası ve protokole göre gelen veya giden ağ trafiğine izin veren veya reddeden güvenlik kurallarının bir listesini içerir.

Kaynak Yöneticisi dağıtım modeli altında, NSG 'ler alt ağlarla veya tek tek ağ arabirimleriyle ilişkilendirilebilir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur. Ayrıca, bir NSG 'yi zaten ilişkili bir NSG 'ye sahip bir alt ağ içindeki bireysel ağ arabirimleriyle ilişkilendirerek, trafik daha da kısıtlanabilir.

Bu makalede, Azure Site Recovery ile ağ güvenlik gruplarını nasıl kullanabileceğiniz açıklanır.

## <a name="using-network-security-groups"></a>Ağ güvenlik gruplarını kullanma

Tek bir alt ağda sıfır veya ilişkili bir NSG olabilir. Tek bir ağ arabirimi sıfır veya ilişkili bir NSG de olabilir. Bu nedenle, önce bir NSG 'yi bir alt ağ ile ilişkilendirerek bir sanal makine için iki trafik kısıtlamasına sahip olabilirsiniz ve ardından VM 'nin ağ arabirimine başka bir NSG ekleyebilirsiniz. Bu durumda NSG kurallarının uygulanması, uygulanan güvenlik kurallarının trafik ve önceliğinin yönlerine bağlıdır.

Bir sanal makineyle aşağıdaki gibi basit bir örnek düşünün:
-    Sanal makine **contoso alt ağının**içine yerleştirilir.
-    **Contoso alt** ağı, **alt ağ NSG**ile ilişkilendirilir.
-    VM ağ arabirimi Ayrıca **VM NSG**ile ilişkilendirilir.

![Site Recovery ile NSG](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

Bu örnekte, gelen trafik için NSG alt ağı ilk olarak değerlendirilir. Alt ağ NSG aracılığıyla izin verilen tüm trafik daha sonra VM NSG tarafından değerlendirilir. Ters, önce VM NSG 'nin değerlendirilme giden trafik için geçerlidir. VM NSG aracılığıyla izin verilen tüm trafik daha sonra alt ağ NSG tarafından değerlendirilir.

Bu, ayrıntılı güvenlik kuralı uygulaması için izin verir. Örneğin, bir alt ağ altındaki birkaç uygulama VM 'lerine (ön uç VM 'Ler gibi) gelen internet erişimine izin vermek, ancak gelen internet erişimini diğer VM 'lere (veritabanı ve diğer arka uç VM 'Ler gibi) kısıtlamak isteyebilirsiniz. Bu durumda, alt ağ NSG 'de daha fazla bir kuralınız olabilir, internet trafiğine izin verebilir ve VM NSG 'ye erişimi reddeterek belirli VM 'lere erişimi kısıtlayabilirsiniz. Bu, giden trafik için de uygulanabilir.

Bu tür NSG yapılandırmalarının kurulması sırasında, [güvenlik kurallarına](../virtual-network/security-overview.md#security-rules)doğru önceliklerin uygulandığından emin olun. Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.

Her zaman ağ güvenlik gruplarının hem bir ağ arabirimine hem de alt ağa uygulandığının farkında olmayabilirsiniz. Bir ağ arabirimine yönelik [etkin güvenlik kurallarını](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) görüntüleyerek, bir ağ arabirimine uygulanan Toplam kuralları doğrulayabilirsiniz. Ayrıca, bir ağ arabiriminden veya bu arabirimden iletişime izin verilip verilmediğini öğrenmek için [Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) 'nde [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) özelliğini de kullanabilirsiniz. Bu araç iletişime izin verilip verilmediğini ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

## <a name="on-premises-to-azure-replication-with-nsg"></a>NSG ile şirket içi Azure 'a çoğaltma

Azure Site Recovery, şirket içi [Hyper-V sanal makineleri](hyper-v-azure-architecture.md), [VMware sanal makineleri](vmware-azure-architecture.md)ve [fiziksel sunucular](physical-azure-architecture.md)için olağanüstü durum kurtarma ve Azure 'a geçiş sağlar. Tüm şirket içi Azure senaryolarında çoğaltma verileri bir Azure depolama hesabına gönderilir ve depolanır. Çoğaltma sırasında, herhangi bir sanal makine ücreti ödemezsiniz. Azure 'a yük devretme işlemi çalıştırdığınızda, Site Recovery otomatik olarak Azure IaaS sanal makineleri oluşturur.

Azure 'a yük devretmeden sonra sanal makineler oluşturulduktan sonra, ağ trafiğini sanal ağ ve VM 'Lerle sınırlandırmak için NSG 'Ler kullanılabilir. Site Recovery yük devretme işleminin bir parçası olarak NSG 'ler oluşturmaz. Yük devretmeyi başlatmadan önce gerekli Azure NSG 'ler oluşturmanızı öneririz. Daha sonra, Site Recovery güçlü [Kurtarma planlarıyla](site-recovery-create-recovery-plans.md)Otomasyon betikleri kullanarak yük devretme sırasında NSG 'leri otomatik olarak yük devreder ile ilişkilendirebilirsiniz.

Örneğin, yük devretme sonrası VM yapılandırması yukarıda açıklanan [Örnek senaryoya](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) benzerdir:
-    Hedef Azure bölgesinde DR planlamasının parçası olarak **contoso VNET** ve **contoso alt ağı** oluşturabilirsiniz.
-    Aynı DR planlamasının parçası olarak hem **alt ağ NSG** hem de **VM NSG 'yi** oluşturup yapılandırabilirsiniz.
-    Hem NSG hem de alt ağ zaten kullanılabilir olduğundan **alt ağ NSG** , **contoso alt ağıyla**hemen ilişkilendirilebilir.
-    **VM NSG** , kurtarma planları kullanılarak yük devretme sırasında VM 'lerle ilişkilendirilebilir.

NSG 'ler oluşturulup yapılandırıldıktan sonra, betikte NSG ilişkilendirmelerini ve yük devretme sonrası VM bağlantısını doğrulamak için bir [Yük devretme testi](site-recovery-test-failover-to-azure.md) çalıştırmayı öneririz.

## <a name="azure-to-azure-replication-with-nsg"></a>NSG ile Azure 'dan Azure 'a çoğaltma

Azure Site Recovery, [Azure sanal makinelerinin](azure-to-azure-architecture.md)olağanüstü durum kurtarmaya izin verebilir. Azure VM 'Ler için çoğaltmayı etkinleştirirken, Site Recovery hedef bölgede çoğaltma sanal ağlarını (alt ağlar ve ağ geçidi alt ağları dahil) oluşturabilir ve kaynak ile hedef sanal ağlar arasında gerekli eşlemeleri oluşturabilir. Ayrıca hedef tarafı ağları ve alt ağları önceden oluşturabilir ve çoğaltmayı etkinleştirirken aynı şekilde kullanabilirsiniz. Site Recovery [Yük devretmeden](azure-to-azure-tutorial-failover-failback.md)önce hedef Azure bölgesinde hiçbir VM oluşturmaz.

Azure VM çoğaltma için, kaynak Azure bölgesindeki NSG kurallarının çoğaltma trafiği için [giden bağlantıya](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) izin verildiğinden emin olun. Ayrıca, bu gerekli kuralları test edebilir ve bu [örnek NSG yapılandırması](azure-to-azure-about-networking.md#example-nsg-configuration)aracılığıyla doğrulayabilirsiniz.

Site Recovery yük devretme işleminin bir parçası olarak NSG 'ler oluşturmaz veya çoğaltmaz. Yük devretmeyi başlatmadan önce, hedef Azure bölgesinde gerekli NSG 'ler oluşturmanızı öneririz. Daha sonra, Site Recovery güçlü [Kurtarma planlarıyla](site-recovery-create-recovery-plans.md)Otomasyon betikleri kullanarak yük devretme sırasında NSG 'leri otomatik olarak yük devreder ile ilişkilendirebilirsiniz.

Daha önce açıklanan [Örnek senaryoya](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) göz önünde bulundurularak:
-    Site Recovery, VM için çoğaltma etkinleştirildiğinde, hedef Azure bölgesinde **contoso VNET** ve **contoso alt ağı** çoğaltmaları oluşturabilir.
-    Hedef bölgede gerekli olan ek kurallara izin vererek, **alt ağ NSG** ve **VM NSG** (sırasıyla hedef **alt ağ NSG** ve hedef **VM NSG**) için istenen çoğaltmaları oluşturabilirsiniz.
-    **Hedef alt ağ NSG** 'si daha sonra hem NSG hem de alt ağ zaten kullanılabilir olduğundan hedef bölge alt ağıyla hemen ilişkilendirilebilir.
-    **Hedef VM NSG** , kurtarma planları kullanılarak yük devretme sırasında VM 'lerle ilişkilendirilebilir.

NSG 'ler oluşturulup yapılandırıldıktan sonra, betikte NSG ilişkilendirmelerini ve yük devretme sonrası VM bağlantısını doğrulamak için bir [Yük devretme testi](azure-to-azure-tutorial-dr-drill.md) çalıştırmayı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
-    [Ağ güvenlik grupları](../virtual-network/security-overview.md#network-security-groups)hakkında daha fazla bilgi edinin.
-    NSG [güvenlik kuralları](../virtual-network/security-overview.md#security-rules)hakkında daha fazla bilgi edinin.
-    NSG için [geçerli güvenlik kuralları](../virtual-network/diagnose-network-traffic-filter-problem.md) hakkında daha fazla bilgi edinin.
-    Uygulama yük devretmesini otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.
