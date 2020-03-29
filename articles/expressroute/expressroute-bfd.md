---
title: "Azure ExpressRoute: BFD'yi yapılandırın"
description: Bu makalede, bir ExpressRoute devresinin özel olarak bakması üzerinde BFD (Çift Yönlü Yönlendirme Algılaması) nasıl yapılandırılabilmek için talimatlar verilmektedir.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 11/1/2018
ms.author: rambala
ms.openlocfilehash: 378b639e89ffd46f6b32d7004f934104dd4b5407
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064849"
---
# <a name="configure-bfd-over-expressroute"></a>BFD'yi ExpressRoute üzerinden yapılandır

ExpressRoute, hem özel hem de Microsoft'un bakışları üzerinden Çift Yönlü Yönlendirme Algılamasını (BFD) destekler. BFD'yi ExpressRoute üzerinden etkinleştirerek, Microsoft Enterprise edge (MSEE) aygıtları ile ExpressRoute devresini (CE/PE) sonlandırdığınızdaki yönlendiriciler arasındaki bağlantı hatası algılamasını hızlandırabilirsiniz. ExpressRoute'u Müşteri Kenarı yönlendirme aygıtları veya İş Ortağı Kenarı yönlendirme aygıtları üzerinden sonlandırabilirsiniz (yönetilen Katman 3 bağlantı hizmetiyle gittiyseniz). Bu belge, BFD gereksinimi ve ExpressRoute üzerinden BFD'nin nasıl etkinleştirilen konusunda size yol göstersin.

## <a name="need-for-bfd"></a>BFD ihtiyacı

Aşağıdaki diyagram, BfD'yi ExpressRoute devresi üzerinden etkinleştirme avantajını gösterir: [![1]][1]

ExpressRoute devresini Katman 2 bağlantıları veya yönetilen Katman 3 bağlantıları yla etkinleştirebilirsiniz. Her iki durumda da, ExpressRoute bağlantı yolunda bir veya daha fazla Layer-2 aygıtı varsa, yoldaki bağlantı hatalarını algılama sorumluluğu üstteki BGP'ye ait.

MSEE cihazlarında, BGP canlı tutar ve bekleme süresi genellikle sırasıyla 60 ve 180 saniye olarak yapılandırılır. Bu nedenle, bir bağlantı arızası sonrasında herhangi bir bağlantı hatası algılamak ve alternatif bağlantı trafiği geçmek için üç dakika kadar sürer.

Daha düşük BGP tutma ve bekleme süresini müşteri kenarı eşleme aygıtında yapılandırarak BGP zamanlayıcılarını denetleyebilirsiniz. BGP zamanlayıcıları iki eşleme aygıtı arasında uyumsuzsa, eşler arasındaki BGP oturumu daha düşük zamanlayıcı değerini kullanır. BGP canlı tutma üç saniye, bekleme süresi ise onlarca saniye sırasına göre ayarlanabilir. Ancak, protokol işlem yoğun olduğundan, BGP zamanlayıcılarını agresif bir şekilde ayarlamak daha az tercih edilir.

Bu senaryoda, BFD yardımcı olabilir. BFD, ikinci dönem altı zaman aralığında düşük havai bağlantı hatası algılaması sağlar. 


## <a name="enabling-bfd"></a>BFD'yi etkinleştirme

BFD, MSEE'lerde yeni oluşturulan tüm ExpressRoute özel eşleme arabirimleri altında varsayılan olarak yapılandırılır. Bu nedenle, BFD'yi etkinleştirmek için, YALNıZCA CEs/P'lerinizde (hem birincil hem de ikincil aygıtlarınızda) BFD'yi yapılandırmanız gerekir. BFD yapılandırma iki adımlı bir işlemdir: BFD'yi arabirimüzerinde yapılandırmanız ve ardından BGP oturumuna bağlamanız gerekir.

Aşağıda bir ÖRNEK CE/PE (Cisco IOS XE kullanarak) yapılandırması gösterilmiştir. 

    interface TenGigabitEthernet2/0/0.150
       description private peering to Azure
       encapsulation dot1Q 15 second-dot1q 150
       ip vrf forwarding 15
       ip address 192.168.15.17 255.255.255.252
       bfd interval 300 min_rx 300 multiplier 3


    router bgp 65020
       address-family ipv4 vrf 15
          network 10.1.15.0 mask 255.255.255.128
          neighbor 192.168.15.18 remote-as 12076
          neighbor 192.168.15.18 fall-over bfd
          neighbor 192.168.15.18 activate
          neighbor 192.168.15.18 soft-reconfiguration inbound
       exit-address-family

>[!NOTE]
>BFD'yi zaten var olan özel bir bakış altında etkinleştirmek için; eğer akran sıfırlamak gerekir. Bkz. [ExpressRoute eşlemelerini sıfırla][ResetPeering]
>

## <a name="bfd-timer-negotiation"></a>BFD Zamanlayıcı Müzakeresi

BFD eşler arasında, iletim hızını iki eşin daha yavaş belirler. MSEEs BFD iletim/alma aralıkları 300 milisaniye olarak ayarlanır. Belirli senaryolarda, aralık 750 milisaniye daha yüksek bir değerde ayarlanabilir. Daha yüksek değerleri yapılandırarak, bu aralıkları daha uzun olmaya zorlayabilirsiniz; ama, daha kısa değil.

>[!NOTE]
>Geo-gereksiz ExpressRoute devrelerini yapılandırmışsanız veya Siteden Siteye IPSec VPN bağlantısını yedekleme olarak kullanıyorsanız; BFD'yi etkinleştirmek, ExpressRoute bağlantı hatası sonrasında daha hızlı başarısız lığa yardımcı olur. 
>

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla bilgi veya yardım için aşağıdaki bağlantılara göz atın:

- [ExpressRoute bağlantı hattı oluşturma ve değiştirme][CreateCircuit]
- [Bir ExpressRoute bağlantı hattı için yönlendirmeyi oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD bağlantı hatası kesinti süresini hızlandırır"

<!--Link References-->
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ResetPeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering






