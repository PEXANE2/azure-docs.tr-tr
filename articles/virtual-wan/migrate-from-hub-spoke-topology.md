---
title: "Mimari: Azure Sanal WAN'a geçirin"
description: Azure Virtual WAN'a nasıl geçiş yapılacağını öğrenin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 02/06/2020
ms.author: cherylmc
ms.openlocfilehash: 8aa4fe143c78d2053ce8c48e4866a5522057aa0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063030"
---
# <a name="migrate-to-azure-virtual-wan"></a>Azure Sanal WAN'a geçirin

Azure Virtual WAN, şirketlerin Microsoft küresel ağının ölçeğinden yararlanmak için küresel bağlantılarını basitleştirmelerine olanak tanır. Bu makalede, varolan müşteri tarafından yönetilen hub ve kollu topolojiden Microsoft tarafından yönetilen Virtual WAN hub'larından yararlanan bir tasarıma geçiş yapmak isteyen şirketler için teknik ayrıntılar sağlar.

Azure Virtual WAN'ın bulut merkezli modern kurumsal küresel ağı benimseyen işletmeler için sağladığı avantajlar hakkında bilgi için [Global transit ağı mimarisi ve Virtual WAN](virtual-wan-global-transit-network-architecture.md)bölümüne bakın.

![hub ve](./media/migrate-from-hub-spoke-topology/hub-spoke.png)
kollu**Şekil: Azure Virtual WAN**

Azure Sanal Veri Merkezi (VDC) hub ve kollu bağlantı modeli, basit ve ölçeklenebilir bulut ağları oluşturmak için Azure Ağı'nın varsayılan geçişli yönlendirme davranışından yararlanmak için binlerce müşterimiz tarafından benimsenmiştir. Azure Virtual WAN bu kavramlar üzerine inşa eder ve yalnızca şirket içi konumlar ve Azure arasında değil, aynı zamanda müşterilerin microsoft ağının ölçeğinden yararlanarak kendi ağlarını genişletmelerine olanak tanıyan küresel bağlantı topolojilerine olanak tanıyan yeni özellikler sunar mevcut küresel ağlar.

Bu makalede, varolan bir karma ortamın Sanal WAN'a nasıl geçirilen nasıl geçirilen gösterir.

## <a name="scenario"></a>Senaryo

Contoso, hem Avrupa'da hem de Asya'da ofisleri bulunan küresel bir finans kuruluşudur. Mevcut uygulamalarını şirket içi bir veri merkezinden Azure'a taşımayı planlıyorlar ve hibrit bağlantı için bölgesel müşteri tarafından yönetilen hub sanal ağları da dahil olmak üzere VDC mimarisine dayalı bir temel tasarımı oluşturmuşlardır. Bulut tabanlı teknolojilere geçişin bir parçası olarak, ağ ekibi, bağlantılarının işletmenin ilerlemesi için optimize edilmesini sağlamakla görevlendirildi.

Aşağıdaki şekil, birden çok Azure bölgesine bağlantı da dahil olmak üzere varolan küresel ağın üst düzey bir görünümünü gösterir.

![Contoso mevcut ağ](./media/migrate-from-hub-spoke-topology/contoso-pre-migration.png)
**topolojisi Şekil: Contoso mevcut ağ topolojisi**

Aşağıdaki noktalar mevcut ağ topolojisinden anlaşılabilir:

- Hub ve spoke topolojisi, ortak bir özel WAN'a bağlantı sağlamak için ExpressRoute Premium devreleri de dahil olmak üzere birden çok bölgede kullanılır.

- Bu sitelerin bazıları, Microsoft bulutu içinde barındırılan uygulamalara ulaşmak için doğrudan Azure'a VPN tünellerine sahiptir.

## <a name="requirements"></a>Gereksinimler

Ağ ekibi, Buluta Contoso geçişini destekleyebilir ve maliyet, ölçek ve performans alanlarında optimize etmesi gereken küresel bir ağ modeli sunmakla görevlendirildi. Özetle, aşağıdaki gereksinimlerin karşılanması gerekir:

- Bulut barındırılan uygulamalara en iyi şekilde giden yol ile hem genel merkez (HQ) hem de şube ofisleri sağlayın.
- Aşağıdaki bağlantı yollarını korurken VPN sonlandırması için varolan şirket içi veri merkezlerine (DC) olan güveni kaldırın:
  - **Şube -to- VNet**: VPN'e bağlı ofisler, yerel Azure bölgesinde buluta geçirilen uygulamalara erişebilmeli.
  - **Şube -to- Hub -to- Hub -to- VNet**: VPN'e bağlı ofisler, uzak Azure bölgesinde buluta geçirilen uygulamalara erişebilmeli.
  - **Şube -to- şube**: Bölgesel VPN bağlantılı ofisler birbirleriyle iletişim kurabilmeli ve ExpressRoute bağlantılı HQ/DC siteleri ile iletişim kurabilmelidir.
  - **Şube -to- Hub -to- Hub -to- şube**: Küresel olarak ayrılmış VPN bağlantılı ofisler birbirleriyle ve ExpressRoute'a bağlı HQ/DC siteleri ile iletişim kurabilmeli.
  - **Şube -to- Internet**: Bağlı siteler internet ile iletişim kurabilmeli. Bu trafik filtrelenmeli ve günlüğe kaydedilmelidir.
  - **VNet -to- VNet**: Aynı bölgede konuşan sanal ağlar birbirleriyle iletişim kurabilmeli.
  - **VNet -to- Hub -to- Hub -to- VNet**: Farklı bölgelerdeki konuşan sanal ağlar birbirleriyle iletişim kurabilmeli.
- Contoso'nun dolaşım kullanıcılarının (dizüstü bilgisayar ve telefon) şirket ağında değil, şirket kaynaklarına erişme olanağı sağlayın.

## <a name="azure-virtual-wan-architecture"></a><a name="architecture"></a>Azure Virtual WAN mimarisi

Aşağıdaki şekil, önceki bölümde ayrıntılı olarak açıklanan gereksinimleri karşılamak için Azure Virtual WAN kullanarak güncelleştirilmiş hedef topolojisinin üst düzey görünümünü gösterir.

![Contoso sanal](./media/migrate-from-hub-spoke-topology/vwan-architecture.png)
WAN mimarisi**Şekil: Azure Virtual WAN mimarisi**

Özet:

- Avrupa'daki karargah ExpressRoute'a bağlı olmaya devam ediyor, Avrupa şirket içi DC tamamen Azure'a geçiriliyor ve şu anda kullanımdan kaldırıldı.
- Asia DC ve HQ Özel WAN'a bağlı kalır. Azure Virtual WAN artık yerel operatör ağını genişletmek ve küresel bağlantı sağlamak için kullanıldı.
- ExpressRoute ve VPN'e bağlı aygıtlar için bağlantı hub'ı sağlamak için hem Batı Avrupa hem de Güney Doğu Asya Azure bölgelerinde dağıtılan Azure Sanal WAN hub'ları.
- Hub'lar ayrıca, openvpn bağlantısını kullanarak birden çok istemci türünde dolaşım kullanıcıları için VPN sonlandırma sı sağlayarak yalnızca Azure'a geçirilen uygulamalara değil, şirket içinde kalan kaynaklara da erişim sağlar.
- Azure Virtual WAN tarafından sağlanan sanal ağdaki kaynaklar için Internet bağlantısı.

Azure Virtual WAN tarafından sağlanan uzak siteler için Internet bağlantısı. Office 365 gibi SaaS hizmetlerine en iyi şekilde erişim için ortak tümleştirmesi yoluyla desteklenen yerel internet koparması.

## <a name="migrate-to-virtual-wan"></a>Sanal WAN'a geçme

Bu bölümde, Azure Sanal WAN'a geçiş için çeşitli adımlar gösterilmektedir.

### <a name="step-1-vdc-hub-and-spoke-single-region"></a>Adım 1: VDC hub-ve-spoke tek bölge

Mimariyi gözden geçirin. Aşağıdaki şekil, Azure Virtual WAN'ın piyasaya sürülmesinden önce Contoso için tek bir bölge topolojisini gösterir:

![Tek bölge](./media/migrate-from-hub-spoke-topology/figure1.png)
topolojisi**Şekil 1: VDC hub-ve-spoke tek bölge**

Sanal Veri Merkezi (VDC) yaklaşımına uygun olarak, müşteri tarafından yönetilen hub sanal ağ birkaç işlev bloğu içerir:

- Paylaşılan hizmetler (birden çok sözcü tarafından gerekli olan ortak işlev). Örnek: Contoso, Hizmet Olarak Altyapı (IaaS) sanal makinelerinde Windows Server etki alanı denetleyicilerini kullanır.
- IP/Yönlendirme güvenlik duvarı hizmetleri, konuşan katman-3 IP yönlendirmesine olanak tanıyan bir üçüncü taraf ağ sanal cihazı tarafından sağlanır.
- Gelen HTTPS istekleri için Azure Uygulama Ağ Geçidi ve internet kaynaklarına filtrelenmiş giden erişim için sanal makinelerde çalışan üçüncü taraf proxy hizmetleri dahil olmak üzere Internet giriş/çıkış hizmetleri.
- Şirket içi ağlara bağlantı için ExpressRoute ve VPN sanal ağ ağ geçidi.

### <a name="step-2-deploy-virtual-wan-hubs"></a>Adım 2: Sanal WAN hub'larını dağıtma

Her bölgeye bir Sanal WAN hub'ı dağıtın. Aşağıdaki makalelerde açıklandığı gibi VPN Ağ Geçidi ve ExpressRoute Ağ Geçidi ile Sanal WAN hub'ını ayarlayın:

- [Öğretici: Azure Sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)
- [Öğretici: Azure Virtual WAN'ı kullanarak ExpressRoute ilişkilendirme oluşturma](virtual-wan-expressroute-portal.md)

> [!NOTE]
> Azure Virtual WAN, bu makalede gösterilen bazı trafik yollarını etkinleştirmek için Standart SKU'yu kullanıyor olmalıdır.

![Sanal WAN hub'larını](./media/migrate-from-hub-spoke-topology/figure2.png)
dağıtma Şekil**2: Sanal WAN geçişiiçin VDC hub ve spoke**

### <a name="step-3-connect-remote-sites-expressroute-and-vpn-to-virtual-wan"></a>Adım 3: Uzak siteleri (ExpressRoute ve VPN) Sanal WAN'a bağlayın

Sanal WAN hub'ını mevcut ExpressRoute devrelerine bağlayın ve Internet üzerinden siteden siteye VPN'leri herhangi bir uzak şubeye ayarlayın.

> [!NOTE]
> Ekspres Rota Devreleri, Virtual WAN hub'ına bağlanmak için Premium SKU türüne yükseltilmelidir.

![Uzak siteleri Virtual](./media/migrate-from-hub-spoke-topology/figure3.png)
WAN**Şekil 3'e bağlayın: Sanal WAN geçişine VDC hub ve spoke**

Bu noktada, şirket içi ağ donanımı, Sanal WAN tarafından yönetilen hub VNet'e atanan IP adres alanını yansıtan rotalar almaya başlar. Bu aşamada VPN'e bağlı uzak dallar, konuşan sanal ağlardaki mevcut uygulamalara giden iki yol görür. Bu aygıtlar, geçiş aşamasında simetrik yönlendirme sağlamak için tüneli VDC hub'ına kullanmaya devam edecek şekilde yapılandırılmalıdır.

### <a name="step-4-test-hybrid-connectivity-via-virtual-wan"></a>Adım 4: Sanal WAN üzerinden test hibrid bağlantı

Üretim bağlantısı için yönetilen Virtual WAN hub'ını kullanmadan önce, test konuşan bir sanal ağ ve Virtual WAN VNet bağlantısı kurmanızı öneririz. Sonraki adımlara devam etmeden önce expressroute ve Site'den Siteye VPN üzerinden bu test ortamına bağlantıların çalıştığını doğrulayın.

![Sanal WAN](./media/migrate-from-hub-spoke-topology/figure4.png)
**Şekil 4: VDC hub-and-spoke Sanal WAN geçiş** ile test hibrid bağlantı

### <a name="step-5-transition-connectivity-to-virtual-wan-hub"></a>Adım 5: Sanal WAN hub'ına geçiş bağlantısı

![Sanal WAN hub'ına](./media/migrate-from-hub-spoke-topology/figure5.png)
geçiş bağlantısı Şekil**5: VDC hub ve kollu Sanal WAN geçişi**

**bir**. Varolan bakan bağlantıları Spoke sanal ağlarından eski VDC hub'ına silin. A-c adımları tamamlanana kadar konuşan sanal ağlardaki uygulamalara erişim kullanılamaz.

**b**. VNet bağlantıları aracılığıyla konuşan sanal ağları Sanal WAN hub'ına bağlayın.

**c**. Daha önce konuşan sanal ağlarda kullanılan kullanıcı tanımlı yolları (UDR) spoke-to-spoke iletişimiçin kaldırın. Bu yol artık Virtual WAN hub'ında bulunan dinamik yönlendirme ile etkinleştirilir.

**d**. VDC hub'ındaki mevcut ExpressRoute ve VPN Ağ Geçitleri, bir sonraki adıma (e) izin vermek üzere kullanımdan kaldırıldı.

**e**. Eski VDC hub'ını (hub sanal ağ) yeni bir VNet bağlantısı yla Virtual WAN hub'ına bağlayın.

### <a name="step-6-old-hub-becomes-shared-services-spoke"></a>Adım 6: Eski hub paylaşılan hizmetler konuştu olur

Azure ağımızı, Virtual WAN merkezini yeni topolojimizin merkezi noktası haline getirecek şekilde yeniden tasarladık.

![Eski hub Paylaşılan](./media/migrate-from-hub-spoke-topology/figure6.png)
Hizmetler Şekil 6 konuştu**olur: VDC hub-ve-Sanal WAN geçiş konuştu**

Sanal WAN hub'ı yönetilen bir varlık olduğundan ve sanal makineler gibi özel kaynakların dağıtımına izin vermedığından, paylaşılan hizmetler bloğu artık kollu sanal ağ olarak var ve Azure Application Gateway üzerinden internet girişi veya ağ sanallaştırılmış cihaz. Paylaşılan hizmetler ortamı ile arka uç sanal makineleri arasındaki trafik artık Virtual WAN tarafından yönetilen hub'dan geçmektedir.

### <a name="step-7-optimize-on-premises-connectivity-to-fully-utilize-virtual-wan"></a>Adım 7: Virtual WAN'ı tam olarak kullanmak için şirket içi bağlantıyı optimize edin

Bu aşamada Contoso, şirket içi DC'de yalnızca birkaç eski uygulama kalarak, çoğunlukla Microsoft Cloud'daki iş uygulamaları geçişlerini tamamlamıştır.

![Sanal WAN](./media/migrate-from-hub-spoke-topology/figure7.png)
Şekil 7'yi tam olarak kullanmak için şirket içi bağlantıyı optimize edin:**Sanal WAN geçişiyle VDC hub ve spoke**

Contoso, Azure Virtual WAN'ın tüm işlevselliğini sağlamak için eski şirket içi VPN bağlantılarını devre dışı bırakmaya karar verir. HQ veya DC ağlarına erişmeye devam eden tüm şubeler, Azure Virtual WAN'ın yerleşik geçiş yönlendirmesini kullanarak Microsoft global ağına geçiş yapabiliyor.

> [!NOTE]
> ExpressRoute Global Reach, microsoft omurgasını mevcut özel WA'larını tamamlamak için yararlanmak isteyen müşteriler için alternatif bir seçimdir.

## <a name="end-state-architecture-and-traffic-paths"></a>Son durum mimarisi ve trafik yolları

![Son durum mimarisi ve](./media/migrate-from-hub-spoke-topology/figure8.png)
trafik yolları**Şekil: Çift bölge Sanal WAN**

Bu bölümde, bazı örnek trafik akışlarına bakarak bu topolojinin özgün gereksinimleri nasıl karşıladığının bir özetini sağlar.

### <a name="path-1"></a>Yol 1

Yol 1, Asya'daki S2S VPN bağlantılı bir şubeden Güney Doğu Asya bölgesindeki bir Azure VNet'e giden trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Asya şubesi, Güney Doğu Asya Sanal WAN hub'ına esnek S2S BGP özellikli tüneller aracılığıyla bağlanır.

- Asia Virtual WAN hub'ı trafiği bağlı VNet'e yerel olarak yönlendirir.

![Akış 1](./media/migrate-from-hub-spoke-topology/flow1.png)

### <a name="path-2"></a>Yol 2

Yol 2, ExpressRoute'a bağlı Avrupa Karargahı'ndan Güney Doğu Asya bölgesindeki bir Azure VNet'e giden trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Avrupa HQ Batı Avrupa Virtual WAN hub içine premium ExpressRoute devre üzerinden bağlanır.

- Sanal WAN hub'dan hub'a global bağlantı, uzak bölgede bağlı VNet'e trafik geçişine olanak tanır.

![Akış 2](./media/migrate-from-hub-spoke-topology/flow2.png)

### <a name="path-3"></a>Yol 3

Yol 3, Private WAN'a bağlı Asya'daki DC'den Avrupa S2S bağlantılı bir Şubeye gelen trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Asia DC yerel Private WAN taşıyıcısına bağlıdır.

- ExpressRoute devresi, Özel WAN'da Güney Doğu Asya Sanal WAN hub'ına bağlanır.

- Sanal WAN hub'dan hub'a küresel bağlantı, trafik geçişine olanak tanır.

![Akış 3](./media/migrate-from-hub-spoke-topology/flow3.png)

### <a name="path-4"></a>Yol 4

Yol 4, Güney Doğu Asya bölgesindeki bir Azure VNet'ten Batı Avrupa bölgesindeki bir Azure VNet'e giden trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Sanal WAN hub'dan hub'a global bağlantı, daha fazla kullanıcı config olmadan bağlı tüm Azure VNet'lerin yerel geçişine olanak tanır.

![Akış 4](./media/migrate-from-hub-spoke-topology/flow4.png)

### <a name="path-5"></a>Yol 5

Yol 5, dolaşım VPN (P2S) kullanıcılarından Batı Avrupa bölgesindeki bir Azure VNet'e giden trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Dizüstü bilgisayar ve mobil cihaz kullanıcıları, Batı Avrupa'daki P2S VPN ağ geçidine saydam bağlantı sağlamak için OpenVPN istemcisini kullanır.

- Batı Avrupa Sanal WAN hub'ı trafiği bağlı VNet'e yerel olarak yönlendirir.

![Akış 5](./media/migrate-from-hub-spoke-topology/flow5.png)

## <a name="security-and-policy-control-via-azure-firewall"></a>Azure Güvenlik Duvarı ile güvenlik ve ilke denetimi

Contoso şimdi bu makalede daha önce tartışılan gereksinimleri doğrultusunda tüm dalları ve VNets arasındaki bağlantıyı doğrulamıştır. Güvenlik denetimi ve ağ yalıtımı gereksinimlerini karşılamak için, hub ağı üzerinden trafiği ayırmaya ve günlüğe kaydetmeye devam etmeleri gerekir. Daha önce bu işlev bir ağ sanal cihaz (NVA) tarafından gerçekleştirildi. Contoso ayrıca mevcut proxy hizmetlerini devre dışı bırakmak ve giden Internet filtresi için yerel Azure hizmetlerini kullanmak istiyor.

![Azure Güvenlik Duvarı](./media/migrate-from-hub-spoke-topology/security-policy.png)
Şekli ile güvenlik ve ilke**denetimi: Sanal WAN'da Azure Güvenlik Duvarı (Güvenli Sanal hub)**

Birleştirilmiş bir ilke denetimi noktası sağlamak için Azure Güvenlik Duvarı'nı Sanal WAN hub'larına tanıtmak için aşağıdaki üst düzey adımlar gereklidir. Bu işlem ve Güvenli Sanal Hub kavramı hakkında daha fazla bilgi için [Azure Güvenlik Duvarı Yöneticisi'ne](../firewall-manager/index.yml)bakın.

1. Azure Güvenlik Duvarı ilkesi oluşturun.
2. Güvenlik duvarı ilkesini Azure Virtual WAN hub'ına bağla. Bu adım, varolan Sanal WAN hub'ın güvenli bir sanal hub olarak çalışmasına olanak tanır ve gerekli Azure Güvenlik Duvarı kaynaklarını dağıtMaktadır.

> [!NOTE]
> Azure Güvenlik Duvarı Standart Sanal WAN hub'ında (SKU : Standart) dağıtılırsa: V2V, B2V, V2I ve B2I FW ilkeleri yalnızca Azure FW'nin dağıtıldığı belirli hub'a bağlı VNet ve Dallardan kaynaklanan trafikte uygulanır (Güvenli Hub). Uzak Şubeler ve VNet Sanal WAN hub üzerinden hub bağlantılarına birbirine bağlı olsa bile, aynı Sanal WAN hub'larına bağlı uzak VNet ve Dallardan kaynaklanan trafik "güvenlik duvarı" olmaz. Hub'lar arası güvenlik duvarı desteği Azure Sanal WAN ve Güvenlik Duvarı Yöneticisi yol haritasında yer almaktadır.

Aşağıdaki yollar, Azure güvenli sanal hub'ları kullanarak etkinleştirilen bağlantı yollarını gösterir:

### <a name="path-6"></a>Yol 6

Yol 6, aynı bölgedeki VNet'ler arasındaki güvenli trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Aynı Güvenli Sanal Hub'a bağlı Sanal Ağlar artık trafiği Azure Güvenlik Duvarı üzerinden yönlendirin.

- Azure Güvenlik Duvarı bu akışlara ilke uygulayabilir.

![Akış 6](./media/migrate-from-hub-spoke-topology/flow6.png)

### <a name="path-7"></a>Yol 7

Yol 7, Bir Azure VNet'ten Internet'e veya üçüncü taraf Güvenlik Hizmetine giden trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Güvenli Sanal Hub'a bağlı Sanal Ağlar, Internet erişiminin merkezi bir noktası olarak Güvenli Hub'ı kullanarak trafiği herkese açık, Internet'teki hedeflere gönderebilir.

- Bu trafik, Azure Güvenlik Duvarı FQDN kuralları kullanılarak yerel olarak filtrelenebilir veya denetim için üçüncü taraf bir güvenlik hizmetine gönderilebilir.

![Akış 7](./media/migrate-from-hub-spoke-topology/flow7.png)

### <a name="path-8"></a>Yol 8

Yol 8, şubeden Internet'e veya üçüncü taraf Güvenlik Hizmeti'nden gelen trafik akışını gösterir.

Trafik aşağıdaki gibi yönlendirilir:

- Güvenli Sanal Hub'a bağlı şubeler, Güvenli Hub'ı Internet erişiminin merkezi bir noktası olarak kullanarak Internet'teki ortak hedeflere trafik gönderebilir.

- Bu trafik, Azure Güvenlik Duvarı FQDN kuralları kullanılarak yerel olarak filtrelenebilir veya denetim için üçüncü taraf bir güvenlik hizmetine gönderilebilir.

![Akış 8](./media/migrate-from-hub-spoke-topology/flow8.png) 

## <a name="next-steps"></a>Sonraki adımlar

[Azure Virtual WAN](virtual-wan-about.md) hakkında daha fazla bilgi edinin
