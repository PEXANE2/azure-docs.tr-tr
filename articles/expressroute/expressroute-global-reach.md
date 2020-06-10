---
title: 'Azure ExpressRoute: Global Reach kullanarak Microsoft Bulut bağlanma'
description: Bu makalede ExpressRoute Global Reach açıklanmaktadır.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 06/09/2020
ms.author: cherylmc
ms.openlocfilehash: 5022efacf33a2c07f1388666b5de26ee86322739
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636742"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
ExpressRoute, şirket içi ağlarınızın Microsoft Bulut 'e bağlanması için özel ve dayanıklı bir yoldur. Azure ve Office 365 gibi birçok Microsoft bulut hizmetine, özel veri merkezinizden veya şirket ağınızdan erişebilirsiniz. Örneğin, aynı şehirde bulunan bir ExpressRoute devresini içeren bir Expressvalley ve diğer şube ofisindeki bir ExpressRoute devresini içeren bir şube ofisiniz olabilir. Her iki şube ofisi ABD Batı ve UK Güney Azure kaynaklarıyla yüksek hızlı bağlantıya sahip olabilir. Ancak, şube ofislerde doğrudan veri alışverişi yapılamaz. Diğer bir deyişle 10.0.1.0/24, 10.0.3.0/24 ve 10.0.4.0/24 ' e veri gönderebilir, ancak 10.0.2.0/24 ' e kadar DEĞILDIR.

![koymadan][1]

**Expressroute Global Reach**ile, şirket içi ağlarınızla özel bir ağ oluşturmak Için ExpressRoute bağlantı devresine bir araya getirebilirsiniz. Yukarıdaki örnekte, ExpressRoute Global Reach eklenmesiyle, San Francisco Office (10.0.1.0/24), mevcut ExpressRoute devreleri ve Microsoft 'un küresel ağı aracılığıyla Londra Office (10.0.2.0/24) ile verileri doğrudan değiş tokuş edebilir. 

![kullanılarak][2]

## <a name="use-case"></a>Kullanım örneği
ExpressRoute Global Reach, hizmet sağlayıcınızın WAN uygulamasını tamamlamak ve şube ofislerinizi dünya çapında bağlamak üzere tasarlanmıştır. Örneğin, hizmet sağlayıcınız birincil olarak Birleşik Devletler ve tüm Dallarınızı ABD 'de bağlamışsa, ancak hizmet sağlayıcısı ExpressRoute Global Reach ile Japonya ve Hong Kong 'da çalışmazsa, bir yerel hizmet sağlayıcısıyla çalışabilirsiniz ve Microsoft, ExpressRoute 'u ve küresel ağımızı kullanarak ABD 'deki şubelerde buraya bağlayacaktır.

![Kullanım örneği][3]

## <a name="availability"></a>Kullanılabilirlik 
ExpressRoute Global Reach Şu anda aşağıdaki konumlarda desteklenir.

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
* Norveç
* Singapur
* İsveç
* İsviçre
* Birleşik Krallık
* Birleşik Devletler

ExpressRoute Devrelerinizin, yukarıdaki ülkelerde veya bölgede bulunan [ExpressRoute eşleme konumlarında](expressroute-locations.md) oluşturulması gerekir. [Farklı geopolitik bölgeler](expressroute-locations.md)arasında expressroute Global Reach etkinleştirmek için, Devrelerinizin PREMIUM SKU 'su olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
1. [Global Reach SSS 'yi görüntüleyin](expressroute-faqs.md#globalreach)
2. [Global Reach etkinleştirmeyi öğrenin](expressroute-howto-set-global-reach.md)
3. [ExpressRoute bağlantı hattını sanal ağınıza bağlamayı öğrenin](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Global erişim olmadan diyagram"
[2]: ./media/expressroute-global-reach/2.png "Genel erişim içeren diyagram"
[3]: ./media/expressroute-global-reach/3.png "Genel erişim durumunu kullanın"
