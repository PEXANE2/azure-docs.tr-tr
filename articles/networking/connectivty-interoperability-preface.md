---
title: 'Azure arka uç bağlantı özelliklerinde birlikte çalışabilirlik: Test kurulumu | Microsoft Dokümanlar'
description: Bu makalede, siteden siteye VPN olan ExpressRoute ile Azure'da sanal ağ eşlemi arasında birlikte çalışabilirliği çözümlemek için kullanabileceğiniz bir test kurulumu açıklanmaktadır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 0cbd4b620a03ed26e95679cf7cb1abef277a9471
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873804"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-setup"></a>Azure arka uç bağlantı özelliklerinde birlikte çalışabilirlik: Test kurulumu

Bu makalede, Azure ağ hizmetlerinin denetim düzlemi düzeyinde ve veri düzlemi düzeyinde nasıl birlikte çalıştığını analiz etmek için kullanabileceğiniz bir test kurulumu açıklanmaktadır. Azure ağ bileşenlerine kısaca bakalım:

-   **Azure ExpressRoute**: Şirket içi ağınızdaki özel IP alanlarını Azure Sanal Ağ dağıtımlarınıza doğrudan bağlamak için Azure ExpressRoute'da özel bakışkullanın. ExpressRoute, daha yüksek bant genişliği ve özel bağlantı elde etmenize yardımcı olabilir. Birçok ExpressRoute eko iş ortağı, SLA'larla ExpressRoute bağlantısı sunar. ExpressRoute hakkında daha fazla bilgi edinmek ve ExpressRoute'u nasıl yapılandırıştırılabilirsiniz öğrenmek [için ExpressRoute'a Giriş 'e][ExpressRoute]bakın.
-   **Siteden siteye VPN**: Bir şirket içi ağı internet üzerinden Azure'a güvenli bir şekilde bağlamak veya ExpressRoute'u kullanarak azure VPN Ağ Geçidi'ni siteden siteye VPN olarak kullanabilirsiniz. Azure'a bağlanmak için siteden siteye VPN'i nasıl yapılandırılabildiğini öğrenmek için VPN [Ağ Geçidini Yapılandır'a][VPN]bakın.
-   **VNet peering**: Azure Sanal Ağ'daki VNet'ler arasında bağlantı kurmak için sanal ağ (VNet) bakışlarını kullanın. VNet eşleme hakkında daha fazla bilgi edinmek için [VNet'teki eğitime][VNet]bakın.

## <a name="test-setup"></a>Test kurulumu

Aşağıdaki şekil test kurulumünü göstermektedir:

![1][1]

Test kurulumunun merkezinde Azure Bölgesi 1'deki hub VNet yer almaktadır. Hub VNet aşağıdaki şekillerde farklı ağlara bağlanır:

-   Hub VNet VNet peering kullanarak kollu VNet bağlı. Konuşan VNet hub VNet her iki ağ geçidine uzaktan erişim vardır.
-   Hub VNet, siteden siteye VPN kullanarak VNet şubesine bağlanır. Bağlantı, rotaları değiştirmek için eBGP kullanır.
-   Hub VNet, ExpressRoute özel eşlemi birincil yol olarak kullanarak şirket içi Konum 1 ağına bağlanır. Yedekleme yolu olarak siteden siteye VPN bağlantısını kullanır. Bu makalenin geri kalanında, bu ExpressRoute devresine ExpressRoute 1 olarak atıfta bulunuyoruz. Varsayılan olarak, ExpressRoute devreleri yüksek kullanılabilirlik için gereksiz bağlantı sağlar. ExpressRoute 1'de, ikincil Microsoft Enterprise Edge Router (MSEE) ile karşı karşıya olan ikincil müşteri kenarı (CE) yönlendiricialt arabirimi devre dışı bırakılır. Önceki şekilde çift çizgili ok üzerinde kırmızı bir çizgi devre dışı bırakılan CE yönlendirici alt arabirimi temsil eder.
-   Hub VNet, başka bir ExpressRoute özel eşleme kullanarak şirket içi Konum 2 ağına bağlanır. Bu makalenin geri kalanında, bu ikinci ExpressRoute devresine ExpressRoute 2 olarak atıfta bulunuyoruz.
-   ExpressRoute 1 ayrıca hem hub VNet'i hem de şirket içi Konum 1 ağını Azure Bölgesi 2'deki uzak bir VNet'e bağlar.

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

Test topolojisi için [yapılandırma ayrıntıları][Configuration] hakkında bilgi edinin.

Test kurulumunun [kontrol düzlemi analizi][Control-Analysis] ve topolojideki farklı VNets veya V'lerin görünümleri hakkında bilgi edinin.

Test kurulumu ve Azure ağ izleme özellik görünümlerinin [veri düzlemi analizi][Data-Analysis] hakkında bilgi edinin.

Aşağıdakiler için [ExpressRoute SSS'sine][ExR-FAQ] bakın:
-   Bir ExpressRoute ağ geçidine kaç ExpressRoute devresi bağlanabileceğinizi öğrenin.
-   Bir ExpressRoute devresine kaç ExpressRoute ağ geçidi bağlanabileceğinizi öğrenin.
-   ExpressRoute'un diğer ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "Test topolojisi diyagramı"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: connectivty-interoperability-configuration.md
[Control-Analysis]: connectivty-interoperability-control-plane.md
[Data-Analysis]: connectivty-interoperability-data-plane.md
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha


