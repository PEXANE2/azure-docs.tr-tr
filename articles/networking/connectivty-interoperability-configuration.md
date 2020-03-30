---
title: 'Azure arka uç bağlantı özelliklerinde birlikte çalışabilirlik: Yapılandırma ayrıntıları | Microsoft Dokümanlar'
description: Bu makalede, siteden siteye VPN olan ExpressRoute ile Azure'da sanal ağ eşlemi arasında birlikte çalışabilirliği çözümlemek için kullanabileceğiniz test kurulumu için yapılandırma ayrıntıları açıklanmaktadır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 9c4a57111566248d3537cab0d9d85c0c3be874a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68335936"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-test-configuration-details"></a>Azure arka uç bağlantı özelliklerinde birlikte çalışabilirlik: Yapılandırma ayrıntılarını test edin

Bu makalede, [test kurulumuyapılandırma][Setup]ayrıntıları açıklanır. Test kurulumu, Azure ağ hizmetlerinin denetim düzlemi düzeyinde ve veri düzlemi düzeyinde nasıl birlikte çalıştığını analiz etmenize yardımcı olur.

## <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşleme kullanarak Kollu VNet bağlantısı

Aşağıdaki şekilde, azure sanal ağı konuşan bir sanal ağın (VNet) ayrıntıları nı gösterir. İki VNet arasında eşleme kurmayı öğrenmek için [VNet'i yönet'e][VNet-Config]bakın. Konuşan VNet'in vnet hub'ına bağlı ağ geçitlerini kullanmasını istiyorsanız, **uzak ağ geçitlerini kullan'ı**seçin.

[![1]][1]

Aşağıdaki şekil, VNet hub'ının VNet eşleme ayrıntılarını gösterir. VNet hub'ın, kullanılan VNet'in hub'ın ağ geçitlerini kullanmasına izin vermesini istiyorsanız, **ağ geçidi geçişine izin ver'i**seçin.

[![2]][2]

## <a name="branch-vnet-connectivity-by-using-a-site-to-site-vpn"></a>Siteden siteye VPN kullanarak Şube VNet bağlantısı

Azure VPN Ağ Geçidi'nde VPN ağ geçitlerini kullanarak hub ve şube VNets arasında siteden siteye VPN bağlantısı ayarlayın. Varsayılan olarak, VPN ağ geçitleri ve Azure ExpressRoute ağ geçitleri **65515**değerinde özel bir özerk sistem numarası (ASN) kullanır. VPN Ağ Geçidi'nde ASN değerini değiştirebilirsiniz. Test kurulumunda, vnet şubesinin ASN değeri, hub ve şube VNets arasındaki eBGP yönlendirmesini desteklemek için **65516** olarak değiştirilir.


[![3]][3]


## <a name="on-premises-location-1-connectivity-by-using-expressroute-and-a-site-to-site-vpn"></a>ExpressRoute ve siteden siteye VPN kullanarak şirket içi Konum 1 bağlantısı

### <a name="expressroute-1-configuration-details"></a>ExpressRoute 1 yapılandırma ayrıntıları

Aşağıdaki şekilde, Azure Bölgesi 1 ExpressRoute devre yapılandırması şirket içi Konum 1 müşteri kenarı (CE) yönlendiricilerini gösterir:

[![4]][4]

Aşağıdaki şekilde ExpressRoute 1 devresi ile vnet hub'ı arasındaki bağlantı yapılandırması gösterilmektedir:

[![5]][5]

Aşağıdaki liste, ExpressRoute özel eşleme bağlantısı için birincil CE yönlendirici yapılandırmasını gösterir. (Cisco ASR1000 yönlendiricileri test kurulumunda CE yönlendiriciolarak kullanılır.) Siteden siteye VPN ve ExpressRoute devreleri şirket içi bir ağı Azure'a bağlamak için paralel olarak yapılandırıldığında, Azure varsayılan olarak ExpressRoute devresine öncelik verir. Asimetrik yönlendirmeyi önlemek için, şirket içi ağ da siteden siteye VPN bağlantısına göre ExpressRoute bağlantısına öncelik vermelidir. Aşağıdaki yapılandırma, BGP **yerel tercih** özniteliğini kullanarak öncelik belirlemeyi kurar:

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

### <a name="site-to-site-vpn-configuration-details"></a>Siteden siteye VPN yapılandırma ayrıntıları

Aşağıdaki liste, siteden siteye VPN bağlantısı için birincil CE yönlendirici yapılandırmasını gösterir:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

## <a name="on-premises-location-2-connectivity-by-using-expressroute"></a>ExpressRoute'u kullanarak şirket içi Konum 2 bağlantısı

Şirket içi Konum 2'ye daha yakın olan ikinci bir ExpressRoute devresi, şirket içi Konum 2'yi vNet hub'ına bağlar. Aşağıdaki şekil ikinci ExpressRoute yapılandırmasını gösterir:

[![6]][6]

Aşağıdaki şekilde ikinci ExpressRoute devresi ile vnet hub'ı arasındaki bağlantı yapılandırması gösterilmektedir:

[![7]][7]

ExpressRoute 1, hem hub VNet'i hem de şirket içi Konum 1'i farklı bir Azure bölgesindeki uzak bir VNet'e bağlar:

[![8]][8]

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

Test kurulumunun [kontrol düzlemi analizi][Control-Analysis] ve topolojideki farklı VNets veya V'lerin görünümleri hakkında bilgi edinin.

Test kurulumu ve Azure ağ izleme özellik görünümlerinin [veri düzlemi analizi][Data-Analysis] hakkında bilgi edinin.

Aşağıdakiler için [ExpressRoute SSS'sine][ExR-FAQ] bakın:
-   Bir ExpressRoute ağ geçidine kaç ExpressRoute devresi bağlanabileceğinizi öğrenin.
-   Bir ExpressRoute devresine kaç ExpressRoute ağ geçidi bağlanabileceğinizi öğrenin.
-   ExpressRoute'un diğer ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "Spoke VNet's VNet akran"
[2]: ./media/backend-interoperability/HubVNet-peering.png "Hub VNet'in VNet eşlemi"
[Bir]: ./media/backend-interoperability/BranchVNet-VPNGW.png "şube VNet" 3 VPN Ağ Geçidi yapılandırma
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute 1 yapılandırması"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "ExpressRoute 1'in hub VNet ExR ağ geçidine bağlantı yapılandırması"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute 2 yapılandırması"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "ExpressRoute 2'nin hub VNet ExR ağ geçidine bağlantı yapılandırması"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "ExpressRoute 2'nin uzak bir VNet ExR ağ geçidine bağlantı yapılandırması"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
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


