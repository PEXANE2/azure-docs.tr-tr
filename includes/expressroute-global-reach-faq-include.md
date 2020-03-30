---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 03/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 317a480c13c5c6e00653fd61878a379df3f65ac4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188107"
---
### <a name="what-is-expressroute-global-reach"></a>ExpressRoute Global Reach nedir?

ExpressRoute Global Reach, şirket içi ağlarınızı Microsoft'un küresel ağı üzerinden ExpressRoute hizmeti üzerinden bağlayan bir Azure hizmetidir. Örneğin, Kaliforniya'da Silikon Vadisi'ndeki ExpressRoute'a bağlı özel bir veri merkeziniz ve Teksas'ta Dallas'taki ExpressRoute'a bağlı başka bir özel veri merkeziniz varsa, ExpressRoute Global Reach ile özel veri merkezlerinizi birbirine bağlayabilirsiniz iki ExpressRoute bağlantısı aracılığıyla çapraz veri merkezi trafiğiniz Microsoft'un ağ omurgası üzerinden geçecektir.

### <a name="how-do-i-enable-or-disable-expressroute-global-reach"></a>ExpressRoute Global Erişimini nasıl etkinleştirebilirim veya devre dışı kılabilirim?

ExpressRoute devrelerinizi birbirine bağlayarak ExpressRoute Global Reach'i etkinleştirin. Devreleri keserek özelliği devre dışı kılmış olursunuz. [Yapılandırmaya](../articles/expressroute/expressroute-howto-set-global-reach.md)bakın.

### <a name="do-i-need-expressroute-premium-for-expressroute-global-reach"></a>ExpressRoute Global Reach için ExpressRoute Premium'a ihtiyacım var mı?

ExpressRoute devreleriniz aynı jeopolitik bölgedeyse, bunları birbirine bağlamak için ExpressRoute Premium'a ihtiyacınız yoktur. İki ExpressRoute devresi farklı jeopolitik bölgelerdeyse, aralarında bağlantı sağlamak için her iki devre için expressroute premium gerekir. 

### <a name="how-will-i-be-charged-for-expressroute-global-reach"></a>ExpressRoute Global Reach için nasıl ücretlendirilirim?

ExpressRoute, şirket içi ağınızdan Microsoft bulut hizmetlerine bağlantı sağlar. ExpressRoute Global Reach, Microsoft'un küresel ağından yararlanarak mevcut ExpressRoute devreleriniz aracılığıyla kendi şirket içi ağlarınız arasında bağlantı sağlar. ExpressRoute Global Reach, mevcut ExpressRoute hizmetinden ayrı olarak faturalandırılır. Her ExpressRoute devresinde bu özelliği etkinleştirmek için bir Eklenti ücreti vardır. ExpressRoute Global Reach tarafından etkinleştirilen şirket içi ağlarınız arasındaki trafik, kaynaktaki bir çıkış oranı ve varış noktasındaki giriş hızı için faturalandırılır. Oranlar, devrelerin bulunduğu bölgeye göre kurulur.

### <a name="where-is-expressroute-global-reach-supported"></a>ExpressRoute Global Reach nerede desteklenir?

ExpressRoute Global Reach [belirli ülkelerde/bölgelerde veya yerlerde](../articles/expressroute/expressroute-global-reach.md)desteklenir. ExpressRoute devreleri, bu ülkelerdeki/bölgelerdeki veya konumlardaki bakan konumlarda oluşturulmalıdır.

### <a name="i-have-more-than-two-on-premises-networks-each-connected-to-an-expressroute-circuit-can-i-enable-expressroute-global-reach-to-connect-all-of-my-on-premises-networks-together"></a>Her biri bir ExpressRoute devresine bağlı ikiden fazla şirket içi atüm var. ExpressRoute Global Reach'in tüm şirket içi ağlarımı birbirine bağlamasını etkinleştirebilir miyim?

Evet, devreler desteklenen ülkelerde/bölgelerde olduğu sürece yapabilirsiniz. Aynı anda iki ExpressRoute devresini bağlamanız gerekir. Tam olarak meshed ağ oluşturmak için, tüm devre çiftleri sayısalve yapılandırmayı yinelemeniz gerekir. 

### <a name="can-i-enable-expressroute-global-reach-between-two-expressroute-circuits-at-the-same-peering-location"></a>Aynı konumdaki iki ExpressRoute devresi arasında ExpressRoute Global Reach'i etkinleştirebilir miyim?

Hayır. İki devre farklı eşleme konumlarından olmalıdır. Desteklenen bir ülke/bölgedeki bir metroda birden fazla ExpressRoute izleme konumu varsa, o metrodaki farklı konumlarda oluşturulan ExpressRoute devrelerini birbirine bağlayabilirsiniz. 

### <a name="if-expressroute-global-reach-is-enabled-between-circuit-x-and-circuit-y-and-between-circuit-y-and-circuit-z-will-my-on-premises-networks-connected-to-circuit-x-and-circuit-z-talk-to-each-other-via-microsofts-network"></a>ExpressRoute Global Reach, X devresi ile Y devresi arasında ve Y devresi ile Z devresi arasında etkinleştirilirse, X ve devre Z devresine bağlı şirket içi ağlarım Microsoft'un ağı üzerinden birbirleriyle konuşacak mı?

Hayır. Şirket içi ağlarınızdan herhangi ikisi arasında bağlantıyı etkinleştirmek için, ilgili ExpressRoute devrelerini açıkça bağlamanız gerekir. Yukarıdaki örnekte, X ve devre Z'yi bağlamanız gerekir. 

### <a name="what-is-the-network-throughput-i-can-expect-between-my-on-premises-networks-after-i-enable-expressroute-global-reach"></a>ExpressRoute Global Reach'i etkinleştirdikten sonra şirket içi ağlarım arasında ne gibi ağ iş çıktısı bekleyebilirim?

ExpressRoute Global Reach tarafından etkinleştirilen şirket içi ağlarınız arasındaki ağ girişi, iki ExpressRoute devresinin daha küçük olmasıyla kapatılır. Bina-to-Azure trafiği ve şirket içi trafik aynı devreyi paylaşır ve aynı bant genişliği kapağına tabidir. 

### <a name="with-expressroute-global-reach-what-are-the-limits-on-the-number-of-routes-i-can-advertise-and-the-number-of-routes-i-will-receive"></a>ExpressRoute Global Reach ile, reklam verebileceğim güzergah sayısı ve alacağım rota sayısı yla ilgili sınırlar nelerdir?

Azure özel bakışta Microsoft'a reklam verebileceğiniz rota sayısı Standart devrede 4000 veya Premium devrede 10000'dir. Azure özel bakışlarında Microsoft'tan alacağınız rota sayısı, Azure sanal ağlarınızın yollarının ve ExpressRoute Global Reach üzerinden bağlanan diğer şirket içi ağlarınızdan gelen yolların toplamı olacaktır. Lütfen şirket içi yönlendiricinizde uygun bir maksimum önek sınırı belirlediğinizden emin olun. 

### <a name="what-is-the-sla-for-expressroute-global-reach"></a>ExpressRoute Global Reach için SLA nedir?

ExpressRoute Global Reach, normal ExpressRoute hizmetiyle aynı [kullanılabilirlik SLA'yı](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/) sağlayacaktır.
