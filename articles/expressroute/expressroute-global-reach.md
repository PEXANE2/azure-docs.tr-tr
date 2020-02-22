---
title: 'Azure ExpressRoute: Global Reach kullanarak Microsoft Bulut bağlanma'
description: Bu makalede, ExpressRoute Global erişim açıklanmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: cherylmc
ms.openlocfilehash: f574576044b7e4ddd34289b5cc45fe5ca353f180
ms.sourcegitcommit: 163be411e7cd9c79da3a3b38ac3e0af48d551182
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77538513"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
ExpressRoute, Microsoft Cloud, şirket içi ağlara bağlanmak için özel ve esnek bir yoludur. Azure ve Office 365 gibi birçok Microsoft bulut hizmetine, özel veri merkezinizden veya şirket ağınızdan erişebilirsiniz. Örneğin, bir ExpressRoute bağlantı hattı Silikon ve ile ExpressRoute bağlantı hattına aynı şehirdeki başka bir şube ofisindeki, Londra, San Francisco bir şube ofisi olabilir. Her iki şube ofisleri, ABD Batı ve UK Güney Azure kaynaklarını yüksek hızlı bağlantıyı olabilir. Ancak, şube ofislerinde doğrudan birbirleri ile veri değişimi olamaz. Diğer bir deyişle, 10.0.1.0/24 10.0.3.0/24 ve 10.0.4.0/24, ancak 10.0.2.0/24 veriler gönderebilir.

![koymadan][1]

**Expressroute Global Reach**ile, şirket içi ağlarınızla özel bir ağ oluşturmak Için ExpressRoute bağlantı devresine bir araya getirebilirsiniz. Yukarıdaki örnekte, ek olarak, ExpressRoute Global erişim, İstanbul ofis (10.0.1.0/24) verilerle İstanbul ofis (10.0.2.0/24) var olan ExpressRoute bağlantı hatları aracılığıyla ve global Microsoft ağı üzerinden doğrudan değiştirebilir. 

![kullanılarak][2]

## <a name="use-case"></a>Kullanım örneği
ExpressRoute Global erişim hizmet sağlayıcınızın WAN uygulama tamamlar ve şube ofislerinizde dünya genelindeki bağlanmak için tasarlanmıştır. Örneğin, hizmet sağlayıcınıza öncelikli olarak Amerika Birleşik Devletleri'nde çalışır ve tüm Dallarınızın ABD'deki bağladığından, ancak hizmet sağlayıcısı, Japonya ve Hong Kong çalışmaz, ExpressRoute Global erişim ile bir yerel hizmet sağlayıcısı ile çalışabilir ve Microsoft, ExpressRoute ile global ağımız ABD dışındaki Dallarınızı var. bağlanır.

![Kullanım örneği][3]

## <a name="availability"></a>Kullanılabilirlik 
ExpressRoute Global erişim şu anda aşağıdaki konumlarda desteklenir.

* Avustralya
* Kanada
* Fransa
* Almanya
* Hong Kong SAR
* İrlanda
* Japonya
* Güney Kore
* Hollanda
* Yeni Zelanda
* Singapur
* İsviçre
* Birleşik Krallık
* Amerika Birleşik Devletleri

ExpressRoute Devrelerinizin, yukarıdaki ülkelerde veya bölgede bulunan [ExpressRoute eşleme konumlarında](expressroute-locations.md) oluşturulması gerekir. [Farklı geopolitik bölgeler](expressroute-locations.md)arasında expressroute Global Reach etkinleştirmek için, Devrelerinizin PREMIUM SKU 'su olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global Reach hakkında daha fazla bilgi edinin](expressroute-faqs.md)
2. [ExpressRoute Global Reach etkinleştirme](expressroute-howto-set-global-reach.md)
3. [ExpressRoute devresini Azure sanal ağına bağlama](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Global erişim olmadan diyagram"
[2]: ./media/expressroute-global-reach/2.png "Genel erişim içeren diyagram"
[3]: ./media/expressroute-global-reach/3.png "Genel erişim durumunu kullanın"
