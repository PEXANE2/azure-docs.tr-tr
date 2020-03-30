---
title: Azure'u genel bulutlara bağlama | Microsoft Dokümanlar
description: Azure'u diğer genel bulutlara bağlamanın çeşitli yollarını açıklayın
services: expressroute
author: osamazia
ms.service: expressroute
ms.topic: article
ms.date: 07/24/2019
ms.author: osamaz
ms.openlocfilehash: b8a454c2a104dfe8545cf734bf0b020b8f749bb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889637"
---
# <a name="connecting-azure-with-public-clouds"></a>Azure'u genel bulutlarla bağlama

Birçok işletme, iş ve teknik hedefler nedeniyle çok bulutlu bir strateji izlememektedir. Bunlar arasında maliyet, esneklik, özellik kullanılabilirliği, fazlalık, veri egemenliği vb. yer alır. Bu strateji, her iki bulutun da en iyi lerinden yararlanmalarına yardımcı olur. 

Bu yaklaşım aynı zamanda ağ ve uygulama mimarisi açısından kuruluş için zorluklar teşkil etmektedir. Bu zorluklardan bazıları gecikme ve veri aktarımıdır. Bu zorlukları gidermek için müşteriler doğrudan birden çok buluta bağlanmak istiyor. Bazı hizmet sağlayıcıları, müşteriler için birden çok bulut sağlayıcısını bağlamak için bir çözüm sağlar. Diğer durumlarda, müşteri birden çok genel bulutu bağlamak için kendi yönlendiricisini dağıtabilir.
## <a name="connectivity-via-expressroute"></a>ExpressRoute ile bağlantı
ExpressRoute, müşterilerin bir bağlantı sağlayıcısı tarafından kolaylaştırılan özel bir bağlantı üzerinden şirket içi ağlarını Microsoft bulutuna genişletmelerine olanak tanır. ExpressRoute ile müşteriler Microsoft bulut hizmetlerine bağlantılar kurabilir.

ExpressRoute üzerinden bağlanmanın üç yolu vardır.

1. Layer3 sağlayıcısı
2. Layer2 sağlayıcısı
3. Doğrudan bağlantı

### <a name="layer3-provider"></a>Layer3 Sağlayıcı

Layer3 sağlayıcıları genellikle IP VPN veya MPLS VPN sağlayıcıları olarak bilinir. Müşteriler, veri merkezleri, şubeleri ve bulut arasında çok noktalı bağlantı için bu sağlayıcılardan yararlanır. Müşteriler L3 sağlayıcısına BGP üzerinden veya statik varsayılan rota üzerinden bağlanır. Hizmet sağlayıcısı, müşteri siteleri, veri merkezleri ve genel bulut arasındaki yolların reklamını yapmaktadır. 
 
Katman3 sağlayıcısı üzerinden bağlanırken, Microsoft bgp üzerinden hizmet sağlayıcısına müşteri VNET yollarının reklamını yapacaktır. Sağlayıcının iki farklı uygulaması olabilir.

![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l3.png)

Tüm bulut sağlayıcılarının trafiği müşteri yönlendiricisine ulaşacaksa, sağlayıcı her bulut sağlayıcısını ayrı bir VRF'ye indiriyor olabilir. Müşteri BGP'yi hizmet sağlayıcısıyla çalıştırıyorsa, bu yollar varsayılan olarak diğer bulut sağlayıcılarına yeniden duyurulur. 

Hizmet sağlayıcısı tüm bulut sağlayıcılarını aynı VRF'ye indiriyorsa, rotalar doğrudan hizmet sağlayıcısından diğer bulut sağlayıcılarına duyurulur. Bu, eBGP yollarının varsayılan olarak diğer eBGP komşularına duyurulduğu standart BGP işlemi dir.

Her genel bulutun farklı önek sınırı vardır, bu nedenle güzergahları dağıtırken servis sağlayıcı yolların dağıtılmasında dikkatli olmalıdır.

### <a name="layer2-provider-and-direct-connection"></a>Layer2 Sağlayıcı ve Doğrudan bağlantı

Her iki modelde de fiziksel bağlantı farklı olsa da, katman3'te BGP doğrudan MSEE ve müşteri yönlendiricisi arasında kurulur. ExpressRoute Direct için müşteri doğrudan MSEE'ye bağlanır. Layer2 durumunda, servis sağlayıcı VLAN'ı müşteri tesislerinden buluta genişletir. Müşteriler, DC'lerini buluta bağlamak için Layer2 ağının üstünde BGP çalıştırır.
![](media/expressroute-connect-azure-to-public-cloud/azure-to-public-clouds-l2.png)
Her iki durumda da, müşteri ortak bulutların her biriyle noktadan noktaya bağlantılara sahip olacaktır. Müşteri, her genel buluta ayrı Bir BGP bağlantısı kurar. Bir bulut sağlayıcısı tarafından alınan rotalar varsayılan olarak diğer bulut sağlayıcısına duyurulacaktır. Her bulut sağlayıcısının farklı önek sınırı vardır, bu nedenle rotaların reklamını yaparken müşterinin bu sınırlara dikkat etmesi gerekir. Müşteri, diğer genel bulutlardan gelen rotaları reklamlarken Microsoft ile her zamanki BGP düğümlerini kullanabilir.

## <a name="direct-connection-with-expressroute"></a>ExpressRoute ile doğrudan bağlantı

Müşteriler ExpressRoute'u doğrudan bulut sağlayıcısının doğrudan bağlantı teklifine bağlamayı seçebilir. İki bulut sağlayıcısı arka arkaya bağlanacak ve BGP doğrudan yönlendiricileri arasında kurulacak. Bu tür bağlantılar bugün Oracle ile kullanılabilir.

## <a name="site-to-site-vpn"></a>Siteler arası VPN

Müşteriler, Azure'daki örneklerini diğer genel bulutlarla bağlamak için Internet'i kullanabilir. Hemen hemen tüm bulut sağlayıcıları siteden siteye VPN özellikleri sunar. Ancak, bazı türevlerinin eksikliği nedeniyle uyumsuzluklar olabilir. Örneğin, bazı bulut sağlayıcıları yalnızca IKEv1'i destekler, bu nedenle bu bulutta bir VPN sonlandırma bitiş noktası gerekir. IKEv2'yi destekleyen bulut sağlayıcıları için her iki bulut sağlayıcısında da VPN ağ geçitleri arasında doğrudan bir tünel kurulabilir.

Siteden siteye VPN yüksek iş hacmi ve düşük gecikme sonu çözümü olarak kabul edilmez. Ancak, fiziksel bağlantı için yedek olarak kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute ve sanal ağ bağlantısı yla ilgili diğer sorularınız için [ExpressRoute SSS][ER-FAQ] bölümüne bakın.

Azure ve Oracle arasındaki bağlantı için [Azure ve Oracle Cloud arasında doğrudan bağlantı ayarlama][ER-OCI]

<!--Link References-->
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[ER-OCI]: https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-azure-oci-networking



