---
title: "Azure ExpressRoute: Global Reach'i kullanarak Microsoft Cloud'a bağlanın"
description: Bu makalede, ExpressRoute Global Reach açıklanmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: cherylmc
ms.openlocfilehash: f574576044b7e4ddd34289b5cc45fe5ca353f180
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538513"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
ExpressRoute, şirket içi ağlarınızı Microsoft Cloud'a bağlamanın özel ve esnek bir yoludur. Azure ve Office 365 gibi birçok Microsoft bulut hizmetine özel veri merkezinizden veya kurumsal ağınızdan erişebilirsiniz. Örneğin, San Francisco'da Silikon Vadisi'nde ExpressRoute devresi ve Aynı şehirde ExpressRoute devresi olan Londra'da başka bir şubesi olabilir. Her iki şube de ABD Batı ve İngiltere Güney'deki Azure kaynaklarına yüksek hızlı bağlantı sağlayabilir. Ancak, şubeler birbirleriyle doğrudan veri alışverişinde bulunamaz. Başka bir deyişle, 10.0.1.0/24 10.0.3.0/24 ve 10.0.4.0/24 veri gönderebilir, ancak NOT 10.0.2.0/24.

![Olma -dan][1]

**ExpressRoute Global Reach**ile, şirket içi ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlayabilirsiniz. Yukarıdaki örnekte, ExpressRoute Global Reach'in eklenmesiyle, San Francisco ofiğiniz (10.0.1.0/24) mevcut ExpressRoute devreleri aracılığıyla ve Microsoft'un küresel ağı aracılığıyla Londra ofisiniz (10.0.2.0/24) ile doğrudan veri alışverişinde bulunabilir. 

![Ile][2]

## <a name="use-case"></a>Kullanım örneği
ExpressRoute Global Reach, servis sağlayıcınızın WAN uygulamasını tamamlamak ve dünya çapındaki şubelerinizi birbirine bağlamak için tasarlanmıştır. Örneğin, hizmet sağlayıcınız öncelikle ABD'de çalışıyorsa ve ABD'deki tüm şubelerinizi birbirine bağladıysa, ancak servis sağlayıcı Japonya ve Hong Kong'da çalışmıyorsa, ExpressRoute Global Reach ile yerel bir hizmet sağlayıcısıyla çalışabilirsiniz ve Microsoft, ExpressRoute'u ve küresel ağımızı kullanarak oradaki şubelerinizi ABD'deki şubelere bağlar.

![kullanım örneği][3]

## <a name="availability"></a>Kullanılabilirlik 
ExpressRoute Global Reach şu anda aşağıdaki yerlerde desteklenir.

* Avustralya
* Kanada
* Fransa
* Almanya
* Hong Kong ÖİB
* İrlanda
* Japonya
* Güney Kore
* Hollanda
* Yeni Zelanda
* Singapur
* İsviçre
* Birleşik Krallık
* Amerika Birleşik Devletleri

ExpressRoute devreleriniz, yukarıdaki ülke veya bölgedeki [ExpressRoute bakış konumlarında](expressroute-locations.md) oluşturulmalıdır. ExpressRoute Global Reach'i [farklı jeopolitik bölgeler](expressroute-locations.md)arasında etkinleştirmek için devreleriniz Premium SKU olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-faqs.md)
2. [ExpressRoute Global Reach nasıl etkinleştirilir?](expressroute-howto-set-global-reach.md)
3. [ExpressRoute devresini Azure sanal ağına bağla](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "küresel erişim olmadan diyagram"
[2]: ./media/expressroute-global-reach/2.png "küresel erişim ile diyagram"
[3]: ./media/expressroute-global-reach/3.png "küresel erişim in kullanım durumu"
