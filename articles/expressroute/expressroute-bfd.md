---
title: ExpressRoute üzerinden BFD 'yi Yapılandırma-Azure | Microsoft Docs
description: Bu makale, bir ExpressRoute devresine ait özel eşleme üzerinde BFD (çift yönlü Iletim algılama) yapılandırma hakkında yönergeler sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 8/17/2018
ms.author: rambala
ms.custom: seodec18
ms.openlocfilehash: e33e90d988251afde630401bed165a4d3614d2cd
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881458"
---
# <a name="configure-bfd-over-expressroute"></a>ExpressRoute üzerinde BFD 'yi yapılandırma

ExpressRoute, özel eşleme üzerinden çift yönlü Iletme algılamasını (BFD) destekler. ExpressRoute üzerinde BFD 'yi etkinleştirerek, Microsoft Enterprise Edge (MSEE) cihazları ve ExpressRoute devresini (PE/CE) sonlandıran yönlendiriciler arasında bağlantı hatası algılamayı yönlendirebilirsiniz. ExpressRoute 'u, müşteri Edge yönlendirme cihazları veya Iş ortağı Edge yönlendirme cihazları üzerinden sonlandırabilirsiniz (yönetilen katman 3 bağlantı hizmeti ile karşılaşırsanız). Bu belge BFD gereksinimi boyunca size yol gösterir ve ExpressRoute üzerinde BFD 'yi nasıl etkinleştireceğinizi açıklar.

## <a name="need-for-bfd"></a>BFD gereksinimi

Aşağıdaki diyagramda, ExpressRoute bağlantı hattı üzerinde BFD 'yi etkinleştirme avantajı gösterilmektedir: [![1]][1]

ExpressRoute devresini katman 2 bağlantıları veya yönetilen katman 3 bağlantılarıyla etkinleştirebilirsiniz. Her iki durumda da ExpressRoute bağlantı yolunda bir veya daha fazla katman 2 cihaz varsa, yoldaki bağlantı hatalarının algılanmasının sorumluluğu, çok sayıda BGP ile sonuçlanır.

MSEE cihazlarında, BGP KeepAlive ve tutma zamanı genellikle sırasıyla 60 ve 180 saniye olarak yapılandırılır. Bu nedenle, bir bağlantı hatasının ardından bağlantı başarısızlığının algılanması ve trafiği alternatif bağlantıya geçirmek üç dakika sürer.

İstemci Edge eşleme cihazında daha düşük BGP canlı tutmayı ve bekleme süresini yapılandırarak BGP zamanlayıcılarını kontrol edebilirsiniz. BGP zamanlayıcıları iki eşleme cihazı arasında uyuşmdaysanız, eşler arasındaki BGP oturumu düşük Zamanlayıcı değerini kullanacaktır. BGP KeepAlive, en az üç saniye, en fazla saniye cinsinden bir bekleme süresi olarak ayarlanabilir. Ancak, protokol işlem yoğunluğu olduğundan BGP zamanlayıcıları daha az tercih edilebilir.

Bu senaryoda BFD yardımcı olabilir. BFD, alt/saniye bir zaman aralığında düşük ek yük hata algılaması sağlar. 


## <a name="enabling-bfd"></a>BFD 'yi etkinleştirme

BFD, tüm yeni oluşturulan ExpressRoute özel eşleme arabirimlerini MSEE altında varsayılan olarak yapılandırılır. Bu nedenle BFD 'yi etkinleştirmek için yalnızca PEs/CEs (hem birincil hem de ikincil cihazlarınızda) üzerinde BFD 'yi yapılandırmanız gerekir. BFD 'nin yapılandırması iki adımlı bir işlemdir: arabirimdeki BFD 'yi yapılandırmanız ve ardından BGP oturumuna bağlamanız gerekir.

Örnek bir PE/CE (Cisco IOS XE kullanılarak) yapılandırma aşağıda gösterilmiştir. 

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
>Zaten var olan bir özel eşleme altında BFD 'yi etkinleştirmek için; eşlemeyi sıfırlamanız gerekir. Bkz. [ExpressRoute eşayarlarını sıfırlama][ResetPeering]
>

## <a name="bfd-timer-negotiation"></a>BFD Zamanlayıcı anlaşması

BFD eşleri arasında iki eşin daha yavaş olması iletim hızını belirlemektir. MSEE BFD iletim/alma aralıkları 300 milisaniyeye ayarlanır. Belirli senaryolarda, Aralık 750 milisaniyelik daha yüksek bir değerde ayarlanabilir. Daha yüksek değerleri yapılandırarak, bu aralıkları daha uzun olacak şekilde zorlayabilirsiniz; Ancak, daha kısa değildir.

>[!NOTE]
>Coğrafi olarak yedekli ExpressRoute özel eşleme devreleri yapılandırdıysanız veya ExpressRoute özel eşlemesi için yedekleme olarak siteden siteye IPSec VPN bağlantısı kullanıyorsanız; özel eşleme üzerinde BFD 'nin etkinleştirilmesi, ExpressRoute bağlantı hatasından sonra hızlı bir şekilde yük devretmeye yardımcı olur. 
>

## <a name="next-steps"></a>Sonraki Adımlar

Daha fazla bilgi veya yardım için aşağıdaki bağlantıları inceleyin:

- [ExpressRoute bağlantı hattı oluşturma ve değiştirme][CreateCircuit]
- [ExpressRoute devresi için yönlendirme oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD bağlantı hatası kesinti süresi"

<!--Link References-->
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ResetPeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering






