---
title: Azure Site Kurtarma ile ExpressRoute kullanma hakkında
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Kurtarma hizmetiyle Azure ExpressRoute'un nasıl kullanılacağını açıklar.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/13/2019
ms.author: mayg
ms.openlocfilehash: e4525bdc6165e8e736db5f539c764d25250cb248
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258011"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Azure Site Kurtarma ile Azure ExpressRoute

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Bu makalede, olağanüstü durum kurtarma ve geçiş için Azure Site Kurtarma ile Azure ExpressRoute'u nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="expressroute-circuits"></a>ExpressRoute bağlantı hatları

ExpressRoute devresi, bir bağlantı sağlayıcısı aracılığıyla şirket içi altyapınızla Microsoft bulut hizmetleri arasındaki mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute devresi sipariş edebilirsiniz. Her devre aynı veya farklı bölgelerde olabilir ve farklı bağlantı sağlayıcıları aracılığıyla tesislerinize bağlanabilir. ExpressRoute devreleri hakkında daha fazla bilgi [için burada.](../expressroute/expressroute-circuit-peerings.md)

Bir ExpressRoute devresiyle ilişkili birden çok yönlendirme etki alanı vardır. [ExpressRoute](../expressroute/expressroute-circuit-peerings.md#peeringcompare)yönlendirme etki alanları hakkında daha fazla bilgi edinin ve karşılaştırın.

## <a name="on-premises-to-azure-replication-with-expressroute"></a>ExpressRoute ile Azure çoğaltma için şirket içi

Azure Site Kurtarma, şirket içi [Hyper-V sanal makineleri,](hyper-v-azure-architecture.md) [VMware sanal makineleri](vmware-azure-architecture.md)ve [fiziksel sunucular](physical-azure-architecture.md)için olağanüstü durum kurtarma ve Azure'a geçiş sağlar. Azure senaryolarının tüm şirket içi için çoğaltma verileri bir Azure Depolama hesabına gönderilir ve depolanır. Çoğaltma sırasında sanal makine ücreti ödemezsiniz. Azure'da bir hata yaptığınızda, Site Kurtarma otomatik olarak Azure IaaS sanal makineleri oluşturur.

Site Kurtarma, verileri ortak bir bitiş noktası üzerinden hedef Azure bölgesinde bir Azure Depolama hesabına veya yönetilen diski çoğaltmaya kopyalar. Site Kurtarma çoğaltma trafiği için ExpressRoute'u kullanmak için, [Microsoft'un eşlemesini](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) veya varolan bir [genel eşlemeden](../expressroute/about-public-peering.md) (yeni kreasyonlar için amortismana uygun) kullanabilirsiniz. Microsoft'un bakışları çoğaltma için önerilen yönlendirme etki alanıdır. Çoğaltmanın özel bakışlar üzerinden desteklenmediğini unutmayın.

Configuration Server için [Ağ Gereksinimleri'nin](vmware-azure-configuration-server-requirements.md#network-requirements) de karşılandığından emin olun. Site Kurtarma çoğaltmasının düzenlenmesi için Configuration Server tarafından belirli URL'lere bağlantı gereklidir. ExpressRoute bu bağlantı için kullanılamaz. 

Proxy'yi şirket içinde kullanıyorsanız ve çoğaltma trafiği için ExpressRoute'u kullanmak istiyorsanız, Configuration Server ve Process Servers'daki Proxy bypass listesini yapılandırmanız gerekir. Aşağıdaki adımları izleyin:

- Sistem kullanıcı bağlamına erişmek için psExec aracını [buradan](https://aka.ms/PsExec) indirin.
- Aşağıdaki komut satırı psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe" çalıştırarak sistem kullanıcı bağlamında Açık Internet Explorer
- IE'de proxy ayarları ekleme
- Baypas listesinde Azure depolama URL'sini ekleyin *.blob.core.windows.net

Bu, iletişim proxy üzerinden geçebiliyorken yalnızca çoğaltma trafiğinin ExpressRoute üzerinden akmasını sağlar.

Sanal makineler veya sunucular bir Azure sanal ağına geçemedikten sonra, [bunlara özel bir bakış kullanarak](../expressroute/expressroute-circuit-peerings.md#privatepeering)erişebilirsiniz. 

Birleştirilmiş senaryo aşağıdaki diyagramda gösterilmiştir: ![ExpressRoute ile şirket içinde Azure'a](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>ExpressRoute ile Azure'dan Azure'a çoğaltma

Azure Site Kurtarma, [Azure sanal makinelerin](azure-to-azure-architecture.md)olağanüstü kurtarma sını sağlar. Azure sanal makinelerinizin [Azure Yönetilen Diskler](../virtual-machines/windows/managed-disks-overview.md)kullanıp kullanmadığına bağlı olarak, çoğaltma verileri hedeflenen Azure bölgesinde bir Azure Depolama hesabına veya çoğaltma Yönetilen Disk'e gönderilir. Çoğaltma bitiş noktaları herkese açık olsa da, varsayılan olarak Azure VM çoğaltma çoğaltma trafiği, kaynak sanal ağın hangi Azure bölgesinde bulunduğuna bakılmaksızın Internet'te geçiş yapmaz. Azure'un varsayılan sistem rotasını özel bir [rotayla](../virtual-network/virtual-networks-udr-overview.md#custom-routes) 0.0.0.0/0 adres öneki için geçersiz kılabilir ve VM trafiğini şirket içi bir ağ sanal cihazına (NVA) yönlendirebilirsiniz, ancak bu yapılandırma Site Kurtarma çoğaltmaiçin önerilmez. Özel rotalar kullanıyorsanız, çoğaltma trafiğinin Azure sınırını terk etmesin diye sanal ağınızda "Depolama" için [bir sanal ağ hizmeti bitiş noktası oluşturmanız](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) gerekir.

Azure VM olağanüstü durum kurtarma için varsayılan olarak, ExpressRoute çoğaltma için gerekli değildir. Sanal makineler hedef Azure bölgesine geçemedikten sonra, [bunlara özel bir bakış](../expressroute/expressroute-circuit-peerings.md#privatepeering)kullanarak erişebilirsiniz. Veri aktarım fiyatlarının Azure bölgelerindeki veri çoğaltma moduna bakılmaksızın geçerli olduğunu unutmayın.

Şirket içi veri merkezinizden kaynak bölgedeki Azure Sanal Taşıtlarına bağlanmak için ExpressRoute'u zaten kullanıyorsanız, failover hedef bölgesinde ExpressRoute bağlantısını yeniden kurmayı planlayabilirsiniz. Yeni bir sanal ağ bağlantısı aracılığıyla hedef bölgeye bağlanmak için aynı ExpressRoute devresini kullanabilir veya olağanüstü durum kurtarma için ayrı bir ExpressRoute devresi ve bağlantısını kullanabilirsiniz. Farklı olası senaryolar [burada](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute)açıklanmıştır.

Azure sanal [makinelerini, burada](../site-recovery/azure-to-azure-support-matrix.md#region-support)ayrıntılı olarak açıklandığı şekilde, aynı coğrafi kümedeki herhangi bir Azure bölgesine çoğaltabilirsiniz. Seçilen hedef Azure bölgesi kaynakla aynı jeopolitik bölgede değilse, ExpressRoute Premium'u etkinleştirmeniz gerekebilir. Daha fazla bilgi için [ExpressRoute konumlarını](../expressroute/expressroute-locations.md) ve [ExpressRoute fiyatlandırmasını](https://azure.microsoft.com/pricing/details/expressroute/)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
- [ExpressRoute devreleri](../expressroute/expressroute-circuit-peerings.md)hakkında daha fazla bilgi edinin.
- [ExpressRoute yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#peeringcompare)hakkında daha fazla bilgi edinin.
- [ExpressRoute konumları](../expressroute/expressroute-locations.md)hakkında daha fazla bilgi edinin.
- [ExpressRoute ile Azure sanal makinelerinin](azure-vm-disaster-recovery-with-expressroute.md)olağanüstü durum kurtarma hakkında daha fazla bilgi edinin.
