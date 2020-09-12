---
title: 'Azure ExpressRoute: Global Reach kullanarak Microsoft Bulut bağlanma'
description: Azure ExpressRoute Global Reach 'ın, şirket içi ağlarınızla özel bir ağ oluşturmak için ExpressRoute devreleri birbirine nasıl bağlayabileceğini öğrenin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: duau
ms.openlocfilehash: 8c6ed19a585bc1ebae65045cd1cc4c442f113597
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89651500"
---
# <a name="expressroute-global-reach"></a>ExpressRoute Global Reach hakkında
ExpressRoute, şirket içi ağlarınızı Microsoft Bulut bağlamak için özel ve dayanıklı bir yoldur. Azure ve Microsoft 365 gibi birçok Microsoft bulut hizmetine, özel veri merkezinizden veya şirket ağınızdan erişebilirsiniz. Örneğin, aynı şehirde bulunan bir ExpressRoute devresini içeren bir Expressvalley ve diğer şube ofisindeki bir ExpressRoute devresini içeren bir şube ofisiniz olabilir. Her iki şube ofisi ABD Batı ve UK Güney Azure kaynaklarıyla yüksek hızda bağlantı sağlar. Ancak, şube ofisleri birbirine bağlanamaz ve verileri birbiriyle doğrudan gönderemez. Diğer bir deyişle, 10.0.1.0/24, verileri 10.0.3.0/24 ve 10.0.4.0/24 ağına gönderebilir, ancak 10.0.2.0/24 ağ 'a vermez.

![Hızlı rota Global Reach ile bağlantılı olmayan devreleri gösteren diyagram.][1]

**Expressroute Global Reach**ile, şirket içi ağlarınızla özel bir ağ oluşturmak Için ExpressRoute bağlantı devresine bir araya getirebilirsiniz. Yukarıdaki örnekte, ExpressRoute Global Reach eklenmesiyle, San Francisco Office (10.0.1.0/24), mevcut ExpressRoute devreleri ve Microsoft 'un küresel ağı aracılığıyla Londra Office (10.0.2.0/24) ile verileri doğrudan değiş tokuş edebilir. 

![Express Route Global Reach ile birbirine bağlı devreleri gösteren diyagram.][2]

## <a name="use-case"></a>Kullanım örneği
ExpressRoute Global Reach, hizmet sağlayıcınızın WAN uygulamasını tamamlamak ve şube ofislerinizi dünya çapında bağlamak üzere tasarlanmıştır. Örneğin, hizmet sağlayıcınız birincil olarak Birleşik Devletler ve tüm Dallarınızı ABD 'de bağlamışsa, ancak hizmet sağlayıcısı ExpressRoute Global Reach ile Japonya ve Hong Kong 'da çalışmazsa, bir yerel hizmet sağlayıcısıyla çalışabilirsiniz ve Microsoft, ExpressRoute 'u ve küresel ağımızı kullanarak ABD 'deki şubelerde buraya bağlayacaktır.

![Express Route Global Reach için kullanım durumunu gösteren diyagram.][3]

## <a name="availability"></a>Kullanılabilirlik 
ExpressRoute Global Reach, ExpressRoute 'un şu anda desteklendiği birçok bölgede desteklenir. Desteklenen geçerli bölgeler için [ExpressRoute bağlantı sağlayıcılarına](expressroute-locations-providers.md#partners) başvurabilirsiniz. 

> [!NOTE] 
> [Farklı geopolitik bölgeler](expressroute-locations-providers.md#locations)arasında expressroute Global Reach etkinleştirmek için, Devrelerinizin **Premium SKU 'su**olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- [Sss Global Reach](expressroute-faqs.md#globalreach)görüntüleyin.
- [Global Reach etkinleştirmeyi](expressroute-howto-set-global-reach.md)öğrenin.
- [Sanal ağınıza bir ExpressRoute devresine bağlamayı](expressroute-howto-linkvnet-arm.md)öğrenin.

<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Global erişim olmadan diyagram"
[2]: ./media/expressroute-global-reach/2.png "Genel erişim içeren diyagram"
[3]: ./media/expressroute-global-reach/3.png "Genel erişim durumunu kullanın"
