---
title: "Azure 'da birlikte çalışabilirlik: denetim düzlemi Analizi"
description: Bu makalede, Azure 'da ExpressRoute, siteden siteye VPN ve sanal ağ eşlemesi arasındaki birlikte çalışabilirliği çözümlemek için kullanabileceğiniz test kurulumunun denetim düzlemi Analizi sağlanmaktadır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 5e41bc86533815c394077bf5276d930fe958cd19
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80518281"
---
# <a name="interoperability-in-azure--control-plane-analysis"></a>Azure 'da birlikte çalışabilirlik: denetim düzlemi Analizi

Bu makalede, [Test kurulumunun][Setup]denetim düzlemi Analizi açıklanmaktadır. Test kurulumunun [test kurulumu yapılandırmasını][Configuration] ve [veri düzlemi analizini][Data-Analysis] de gözden geçirebilirsiniz.

Denetim düzlemi analizi, bir topoloji içindeki ağlar arasında değiştirilen yolların temelini inceler. Denetim düzlemi analizi, farklı ağların topolojiyi nasıl görüntüleyip anlamanıza yardımcı olabilir.

## <a name="hub-and-spoke-vnet-perspective"></a>Hub ve bağlı bileşen VNet perspektifi

Aşağıdaki şekilde, bir hub sanal ağı (VNet) ve bir bağlı olan VNet 'in perspektifinden ağ gösterilmektedir (mavi renkle vurgulanır). Bu şekilde farklı ağlar arasında değiştirilen farklı ağların ve yolların otonom sistem numarası (ASN) da gösterilmektedir: 

![1][1]

VNet 'in Azure ExpressRoute Gateway 'in ASN 'si Microsoft kurumsal sınır yönlendiricileri 'nin (MSEE) ASN 'den farklıdır. ExpressRoute ağ geçidi, özel bir ASN ( **65515**değeri) kullanır ve MSEE genel ASN 'yi ( **12076**değeri) Global olarak kullanır. ExpressRoute eşlemesini yapılandırdığınızda, MSEE eşdüzey olduğundan, eşdüzey ASN olarak **12076** kullanırsınız. Azure tarafında, MSEE ExpressRoute ağ geçidi ile eBGP eşlemesi belirler. MSEE 'ın her ExpressRoute eşlemesi için oluşturduğu çift eBGP eşlemesi, denetim düzlemi düzeyinde saydamdır. Bu nedenle, bir ExpressRoute yol tablosu görüntülediğinizde VNET 'in, VNET 'in ön ekleri için ExpressRoute ağ geçidi ASN 'yi görürsünüz. 

Aşağıdaki şekilde, örnek bir ExpressRoute yol tablosu gösterilmektedir: 

![5][5]

Azure 'da ASN yalnızca bir eşleme perspektifinden önemlidir. Azure VPN Gateway hem ExpressRoute ağ geçidi hem de VPN ağ geçidinin ASN 'si varsayılan olarak **65515**' dir.

## <a name="on-premises-location-1-and-the-remote-vnet-perspective-via-expressroute-1"></a>ExpressRoute 1 aracılığıyla şirket içi konum 1 ve uzak VNet perspektifi

Hem şirket içi konum 1 hem de uzak VNet, ExpressRoute 1 aracılığıyla hub VNet 'e bağlanır. Aşağıdaki diyagramda gösterildiği gibi topolojinin aynı perspektifi paylaşır:

![2][2]

## <a name="on-premises-location-1-and-the-branch-vnet-perspective-via-a-site-to-site-vpn"></a>Siteden siteye VPN aracılığıyla şirket içi konum 1 ve şube VNet perspektifi

Hem şirket içi konum 1 hem de şube sanal ağı, siteden siteye VPN bağlantısı aracılığıyla bir hub VNet 'in VPN ağ geçidine bağlanır. Aşağıdaki diyagramda gösterildiği gibi topolojinin aynı perspektifi paylaşır:

![3][3]

## <a name="on-premises-location-2-perspective"></a>Şirket içi konum 2 perspektif

Şirket içi konum 2, ExpressRoute 2 ' nin özel eşlemesi aracılığıyla bir hub VNet 'e bağlıdır: 

![4][4]

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Kademeli olarak ExpressRoute ve siteden siteye VPN bağlantısı

###  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

Şirket içi ağınız ve Azure sanal ağlarınız arasında özel olarak veri alışverişi yapmak için ExpressRoute Microsoft eşlemesi kullanarak siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırmayla, verileri gizlilik, özgünlük ve bütünlük ile değiş tokuş edebilirsiniz. Veri değişimi de yeniden oynama olur. ExpressRoute Microsoft eşlemesi kullanarak bir siteden siteye IPSec VPN 'yi tünel modunda yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute üzerinden siteden sıteye VPN Microsoft eşlemesi][S2S-Over-ExR]. 

Microsoft eşlemesi kullanan bir siteden siteye VPN yapılandırmanın birincil sınırlaması aktarım hızına sahiptir. IPSec tüneli üzerinden aktarım hızı VPN Gateway kapasitesinden sınırlıdır. VPN ağ geçidi verimlilik ExpressRoute aktarım hızına göre daha düşüktür. Bu senaryoda, yüksek oranda güvenli trafik için IPSec tüneli kullanılması ve diğer tüm trafik için özel eşleme kullanılması, ExpressRoute bant genişliği kullanımının iyileştirmenize yardımcı olur.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için güvenli bir yük devretme yolu olarak siteden siteye VPN

ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli bir devre çifti işlevi görür. Coğrafi olarak yedekli ExpressRoute bağlantısını, farklı Azure bölgelerinde yapılandırabilirsiniz. Ayrıca, test kurulumumuzda gösterildiği gibi, bir Azure bölgesi içinde, ExpressRoute bağlantınızın yük devretme yolunu oluşturmak için siteden siteye VPN kullanabilirsiniz. Aynı ön ekler hem ExpressRoute hem de siteden siteye VPN üzerinden tanıtıldığında Azure, ExpressRoute 'u önceliklendirir. ExpressRoute ve siteden siteye VPN arasındaki asimetrik yönlendirmeyi önlemek için, şirket içi ağ yapılandırması, siteden siteye VPN bağlantısı kullanmadan önce ExpressRoute bağlantısı kullanılarak da devrik olmalıdır.

ExpressRoute ve siteden siteye VPN için birlikte var olan bağlantıları yapılandırma hakkında daha fazla bilgi için bkz. [ExpressRoute ve siteden siteye birlikte bulunma][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantısını bağlı olan sanal ağlara ve dal konumlarına Genişlet

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşlemesi kullanarak bağlı olan VNet bağlantısı

Hub ve bağlı ağ VNet mimarisi yaygın olarak kullanılır. Hub, bağlı olan sanal ağlarınız ve şirket içi ağınız arasında merkezi bir bağlantı noktası görevi gören Azure 'da bulunan bir VNet ' dir. Bağlı bileşen, hub ile eş olan ve iş yüklerini yalıtmak için kullanabileceğiniz sanal ağlardır. ExpressRoute veya VPN bağlantısı aracılığıyla şirket içi veri merkezi ile hub arasındaki trafik akışları. Mimari hakkında daha fazla bilgi için bkz. [Azure 'da Merkez-uç ağ topolojisi uygulama][Hub-n-Spoke].

Bir bölgedeki VNet eşlemesi içinde, bağlı olan sanal ağlar, uzak ağlarla iletişim kurmak için hub VNet ağ geçitlerini (VPN ve ExpressRoute ağ geçitleri) kullanabilir.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak dal VNet bağlantısı

Farklı bölgelerde olan dal VNET 'leri ve şirket içi ağları bir hub VNet aracılığıyla birbirleriyle iletişim kurmak isteyebilirsiniz. Bu yapılandırma için yerel Azure çözümü, VPN kullanarak siteden siteye VPN bağlantısı olur. Alternatif, hub 'da yönlendirme için bir ağ sanal gereci (NVA) kullanmaktır.

Daha fazla bilgi için bkz. [ne VPN Gateway?][VPN] nedir ve [yüksek oranda kullanılabilir NVA dağıtma][Deploy-NVA].

## <a name="next-steps"></a>Sonraki adımlar

Test kurulumu ve Azure ağ izleme özelliği görünümlerinin [veri düzlemi Analizi][Data-Analysis] hakkında bilgi edinin.

Bkz. [ExpressRoute SSS][ExR-FAQ] :
-   ExpressRoute ağ geçidine kaç ExpressRoute bağlantı hattı bağlayabileceğinizi öğrenin.
-   Bir ExpressRoute devresine kaç ExpressRoute ağ geçidi bağlayabileceğinizi öğrenin.
-   ExpressRoute 'un diğer ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "Topolojinin hub ve bağlı uç VNet perspektifi"
[2]: ./media/backend-interoperability/Loc1ExRView.png "Konum 1 ve ExpressRoute 1 aracılığıyla topolojinin uzak VNet perspektifi"
[3]: ./media/backend-interoperability/Loc1VPNView.png "Konum 1 ve bir siteden siteye VPN aracılığıyla topolojinin sanal ağ perspektifi"
[4]: ./media/backend-interoperability/Loc2View.png "Konum 2 topolojinin perspektifi"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "ExpressRoute 1 yol tablosu"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


