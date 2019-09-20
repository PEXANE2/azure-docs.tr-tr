---
title: Global erişim - Azure ExpressRoute kullanarak Microsoft Cloud için şirket içi ağlara bağlanın | Microsoft Docs
description: Bu makalede, ExpressRoute Global erişim açıklanmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: e522c7ea306667f675af4bbe7486e25fb7f4ec9f
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71122795"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
ExpressRoute, Microsoft Cloud, şirket içi ağlara bağlanmak için özel ve esnek bir yoludur. Azure ve Office 365 gibi birçok Microsoft bulut hizmetine, özel veri merkezinizden veya şirket ağınızdan erişebilirsiniz. Örneğin, bir ExpressRoute bağlantı hattı Silikon ve ile ExpressRoute bağlantı hattına aynı şehirdeki başka bir şube ofisindeki, Londra, San Francisco bir şube ofisi olabilir. Her iki şube ofisleri, ABD Batı ve UK Güney Azure kaynaklarını yüksek hızlı bağlantıyı olabilir. Ancak, şube ofislerinde doğrudan birbirleri ile veri değişimi olamaz. Diğer bir deyişle, 10.0.1.0/24 10.0.3.0/24 ve 10.0.4.0/24, ancak 10.0.2.0/24 veriler gönderebilir.

![Olmadan][1]

İle **ExpressRoute Global erişim**, birlikte özel bir ağ, şirket içi ağlar arasında yapmak için ExpressRoute bağlantı hattına bağlayabilirsiniz. Yukarıdaki örnekte, ek olarak, ExpressRoute Global erişim, İstanbul ofis (10.0.1.0/24) verilerle İstanbul ofis (10.0.2.0/24) var olan ExpressRoute bağlantı hatları aracılığıyla ve global Microsoft ağı üzerinden doğrudan değiştirebilir. 

![ile][2]

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
* Singapur
* İsviçre
* Birleşik Krallık
* Amerika Birleşik Devletleri

Konumunda, ExpressRoute devreleri oluşturulması [ExpressRoute eşleme konumlarına](expressroute-locations.md) yukarıdaki ülke veya bölge. ExpressRoute Global erişim arasında etkinleştirmek için [farklı coğrafi bölgeler](expressroute-locations.md), Premium SKU, devreler olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-faqs.md)
2. [ExpressRoute Global erişim olanağı tanıma](expressroute-howto-set-global-reach.md)
3. [ExpressRoute bağlantı hattı için Azure sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Küresel erişim olmadan diyagramı"
[2]: ./media/expressroute-global-reach/2.png "Küresel erişime sahip diyagramı"
[3]: ./media/expressroute-global-reach/3.png "Küresel erişim durumunun kullanın"
