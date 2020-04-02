---
title: "Azure'da birlikte çalışabilirlik : Düzlem çözümlemesi denetimi"
description: Bu makalede, siteden siteye VPN olan ExpressRoute ile Azure'da sanal ağ eşlemesi arasında birlikte çalışabilirliği analiz etmek için kullanabileceğiniz test kurulumunun denetim düzlemi çözümlemesi sağlanmaktadır.
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
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518281"
---
# <a name="interoperability-in-azure--control-plane-analysis"></a>Azure'da birlikte çalışabilirlik : Düzlem çözümlemesi denetimi

Bu makalede, [test kurulumunun][Setup]denetim düzlemi çözümlemesi açıklanmaktadır. Ayrıca [test kurulumu yapılandırmasını][Configuration] ve test kurulumuveri [düzlemi çözümlemesi][Data-Analysis] gözden geçirebilirsiniz.

Kontrol düzlemi analizi esas olarak bir topoloji içinde ağlar arasında değiş tokuş edilen yolları inceler. Denetim düzlemi çözümlemesi, farklı ağların topolojiyi nasıl görüntülediğinizi anlamanıza yardımcı olabilir.

## <a name="hub-and-spoke-vnet-perspective"></a>Hub ve kollu VNet perspektifi

Aşağıdaki şekil, ağı bir hub sanal ağı (VNet) ve kollu VNet (mavi ile vurgulanan) perspektifinden göstermektedir. Bu rakam ayrıca, farklı ağlar arasında değiş tokuş edilen farklı ağların ve yolların özerk sistem numarasını (ASN) gösterir: 

![1][1]

VNet'in Azure ExpressRoute ağ geçidinin ASN'si, Microsoft Enterprise Edge Yönlendiricilerinin (MSEEs) ASN'sinden farklıdır. ExpressRoute ağ geçidi özel bir ASN **(65515**değeri) kullanır ve MSEEs genel ASN **(12076**değeri) genel olarak kullanır. ExpressRoute eşlemi yapılandırdığınızda, MSEE eş olduğundan, eş ASN olarak **12076'yı** kullanırsınız. Azure tarafında, MSEE ExpressRoute ağ geçidiile eBGP'yi kurar. MSEE'nin her ExpressRoute eşlemi için oluşturduğu çift eBGP eşleme, kontrol düzlemi düzeyinde saydamdır. Bu nedenle, bir ExpressRoute rota tablosunu görüntülediğinizde, VNet'in önekleri için VNet'in ExpressRoute ağ geçidi ASN'yi görürsünüz. 

Aşağıdaki şekil örnek bir ExpressRoute rota tablosunu gösterir: 

![5][5]

Azure'da ASN yalnızca bir bakış açısından önemlidir. Varsayılan olarak, Hem ExpressRoute ağ geçidinin hem de Azure VPN Ağ Geçidi'ndeki VPN ağ geçidinin ASN'si **65515'tir.**

## <a name="on-premises-location-1-and-the-remote-vnet-perspective-via-expressroute-1"></a>ExpressRoute 1 ile şirket içi Konum 1 ve uzak VNet perspektifi

Hem şirket içi Konum 1 hem de uzak VNet, ExpressRoute 1 üzerinden Hub VNet'e bağlanır. Aşağıdaki diyagramda gösterildiği gibi topolojinin aynı perspektifini paylaşırlar:

![2][2]

## <a name="on-premises-location-1-and-the-branch-vnet-perspective-via-a-site-to-site-vpn"></a>Site içi VPN üzerinden şirket içi Konum 1 ve şube VNet perspektifi

Hem şirket içi Konum 1 hem de VNet şubesi, siteden siteye VPN bağlantısı aracılığıyla vnet'in VPN ağ geçidine bağlanır. Aşağıdaki diyagramda gösterildiği gibi topolojinin aynı perspektifini paylaşırlar:

![3][3]

## <a name="on-premises-location-2-perspective"></a>Şirket içi Konum 2 perspektifi

Şirket içi Konum 2, ExpressRoute 2'nin özel bakışları aracılığıyla bir hub VNet'e bağlanır: 

![4][4]

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute ve siteden siteye VPN bağlantısı birlikte

###  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

ExpressRoute Microsoft'u kullanarak şirket içi ağınızla Azure VNet'leriniz arasında özel olarak veri alışverişi yaparak siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırma ile, gizlilik, özgünlük ve bütünlük ile veri alışverişi yapabilirsiniz. Veri alışverişi de anti-replay olduğunu. ExpressRoute Microsoft'u kullanarak tünel modunda siteden siteye IPsec VPN'i nasıl yapılandırılabilen hakkında daha fazla bilgi için [ExpressRoute Microsoft'un üzerinden Siteden siteye VPN'e][S2S-Over-ExR]bakın. 

Microsoft'un eşlemasını kullanan siteden siteye VPN yapılandırmanın birincil sınırlaması iş bölümüdür. IPsec tüneli üzerindeki iş çıkışı VPN ağ geçidi kapasitesiyle sınırlıdır. VPN ağ geçidi iş girişi ExpressRoute iş lerinden daha düşüktür. Bu senaryoda, son derece güvenli trafik için IPsec tünelini kullanmak ve diğer tüm trafik için özel bakışkullanmak ExpressRoute bant genişliği kullanımını optimize etmesine yardımcı olur.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için güvenli bir arıza yolu olarak siteden siteye VPN

ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli bir devre çifti olarak hizmet vermektedir. Coğrafi yedekli ExpressRoute bağlantısını farklı Azure bölgelerinde yapılandırabilirsiniz. Ayrıca, bir Azure bölgesinde test kurulumumuzda gösterildiği gibi, ExpressRoute bağlantınız için bir arıza yolu oluşturmak için siteden siteye VPN kullanabilirsiniz. Aynı önekler hem ExpressRoute hem de siteden siteye VPN üzerinden duyurulduğunda, Azure ExpressRoute'a öncelik verir. ExpressRoute ile siteden siteye VPN arasında asimetrik yönlendirmeyi önlemek için, şirket içi ağ yapılandırması, siteden siteye VPN bağlantısını kullanmadan önce ExpressRoute bağlantısını kullanarak da karşılık vermelidir.

ExpressRoute ve siteden siteye VPN için birlikte varolan bağlantıları niçin yapılandırılabilen ler hakkında daha fazla bilgi için [ExpressRoute ve siteden siteye birlikte yaşama][ExR-S2S-CoEx]bilgi sini görün.

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantısını konuşan VNet'lere ve şube konumlarına genişletin

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşleme kullanarak Kollu VNet bağlantısı

Hub ve kollu VNet mimarisi yaygın olarak kullanılır. Hub, Azure'da, konuştuğunuz VNet'leriniz ve şirket içi ağınız arasında merkezi bir bağlantı noktası görevi gören bir VNet'tir. Sözcüler, hub'a göre eşleyen ve iş yüklerini yalıtmak için kullanabileceğiniz VNet'lerdir. Bir ExpressRoute veya VPN bağlantısı üzerinden şirket içi veri merkezi ve hub arasında trafik akışı. Mimari hakkında daha fazla bilgi için [bkz.][Hub-n-Spoke]

VNet'in bir bölge içinde ki bakışlarında, kollu VNet'ler uzak ağlarla iletişim kurmak için hub VNet ağ geçitlerini (hem VPN hem de ExpressRoute ağ geçitleri) kullanabilir.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak Şube VNet bağlantısı

Farklı bölgelerde bulunan şube VNet'lerinin ve şirket içi ağların bir hub VNet üzerinden birbirleriyle iletişim kurmasını isteyebilirsiniz. Bu yapılandırma için yerel Azure çözümü, VPN kullanarak siteden siteye VPN bağlantısıdır. Bir alternatif hub yönlendirme için bir ağ sanal cihaz (NVA) kullanmaktır.

Daha fazla bilgi için VPN Ağ [Deploy a highly available NVA][Deploy-NVA] [Geçidi nedir?][VPN]

## <a name="next-steps"></a>Sonraki adımlar

Test kurulumu ve Azure ağ izleme özellik görünümlerinin [veri düzlemi analizi][Data-Analysis] hakkında bilgi edinin.

Aşağıdakiler için [ExpressRoute SSS'sine][ExR-FAQ] bakın:
-   Bir ExpressRoute ağ geçidine kaç ExpressRoute devresi bağlanabileceğinizi öğrenin.
-   Bir ExpressRoute devresine kaç ExpressRoute ağ geçidi bağlanabileceğinizi öğrenin.
-   ExpressRoute'un diğer ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "Hub ve topoloji VNet perspektif konuştu"
[2]: ./media/backend-interoperability/Loc1ExRView.png "ExpressRoute 1 ile topolojinin konum 1 ve uzak VNet perspektifi"
[3]: ./media/backend-interoperability/Loc1VPNView.png "Siteden siteye VPN aracılığıyla topolojinin konum 1 ve şube VNet perspektifi"
[4]: ./media/backend-interoperability/Loc2View.png "Topolojinin konum 2 perspektifi"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "ExpressRoute 1 rota tablosu"

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


