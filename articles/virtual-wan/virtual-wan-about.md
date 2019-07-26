---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Sanal WAN otomatik ölçeklenebilir dalı-dal bağlantısı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/22/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: f1576e963f9c25821b5e3f57907662e3d86df4e0
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406368"
---
# <a name="what-is-azure-virtual-wan"></a>Azure Sanal WAN nedir?

Azure sanal WAN, Azure ile ve arasında iyileştirilmiş ve otomatik şube bağlantısı sağlayan bir ağ hizmetidir. Azure bölgeleri, Dallarınızı bağlamak için seçebileceğiniz hub olarak görev yapar. Aynı zamanda dalları bağlamak ve şube-VNet bağlantısının avantajlarından yararlanmak için Azure omurgasına sahip olabilirsiniz. Azure sanal WAN VPN ile bağlantı Otomasyonu 'nu destekleyen iş ortaklarının listesi sunuyoruz. Daha fazla bilgi için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

Azure sanal WAN, tek bir işletim arabirimine siteden siteye VPN ve ExpressRoute gibi birçok Azure bulut bağlantı hizmetini birlikte getirir. Azure VNet bağlantısı, sanal ağ bağlantıları kullanılarak oluşturulur.

Sanal WAN için ExpressRoute Şu anda önizleme aşamasındadır.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan1.png)

Bu makalede, Azure sanal WAN 'da ağ bağlantısına hızlı bir bakış sunulmaktadır. Sanal WAN şu avantajları sunar:

* **Hub ve bağlı bileşen 'de tümleşik bağlantı çözümleri:** Siteden siteye yapılandırmayı ve şirket içi siteler ile bir Azure hub 'ı arasındaki bağlantıyı otomatikleştirin.
* **Otomatik bağlı bileşen kurulumu ve yapılandırması:** Sanal ağlarınızı ve iş yüklerinizi sorunsuz bir şekilde Azure hub 'ına bağlayın.
* **Sezgisel sorun giderme:** Azure 'da uçtan uca akışı görebilir ve sonra gerekli eylemleri gerçekleştirmek için bu bilgileri kullanabilirsiniz.

## <a name="resources"></a>Sanal WAN kaynakları

Uçtan uca bir sanal WAN yapılandırmak için aşağıdaki kaynakları oluşturursunuz:

* **virtualWAN:** VirtualWAN kaynağı, Azure ağınızın sanal bir kaplamasını temsil eder ve birden çok kaynak koleksiyonudur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Sanal WAN’daki Sanal Hub'lar birbiriyle iletişim kurmaz. ' Dala Dala Izin ver ' özelliği VPN siteleri ve ExpressRoute (Şu anda önizleme aşamasında) özellikli sitelerde VPN arasında trafik sağlar.

* **Hub** Bir sanal hub, Microsoft tarafından yönetilen bir sanal ağ. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir. Azure portalı kullanarak bir hub oluşturduğunuzda, bir sanal hub VNet’i ve vpngateway sanal hub'ı oluşturulur.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, sanal WAN kullanırken, şirket içi sitenizden doğrudan VNet 'iniz için siteden siteye bağlantı oluşturmazsınız. Bunun yerine, hub 'a siteden siteye bağlantı oluşturursunuz. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır.

* **Hub sanal ağ bağlantısı:** Hub sanal ağ bağlantısı kaynağı, hub 'ı sanal ağınıza sorunsuz bir şekilde bağlamak için kullanılır. Şu anda, yalnızca aynı hub bölgesi içinde yer alan sanal ağlara bağlanabilirsiniz.

* **Hub yol tablosu:**  Sanal hub yolu oluşturabilir ve yolu sanal hub yolu tablosuna uygulayabilirsiniz. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz.

**Ek sanal WAN kaynakları**

  * **Bölgesi** Bu kaynak yalnızca siteden siteye bağlantılar için kullanılır. Site kaynağı **vpnsite**. Şirket içi VPN cihazınızı ve ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

## <a name="connectivity"></a>Bilirlik

Sanal WAN iki bağlantı türüne izin verir: Siteden siteye ve ExpressRoute (Önizleme).

### <a name="s2s"></a>Siteden siteye VPN bağlantıları

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Bir sanal WAN siteden siteye bağlantı oluşturduğunuzda, kullanılabilir bir iş ortağıyla çalışabilirsiniz. Bir iş ortağı kullanmak istemiyorsanız bağlantıyı el ile yapılandırabilirsiniz. Daha fazla bilgi için bkz. [sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md).

#### <a name="s2spartner"></a>Sanal WAN iş akışı

Bir sanal WAN ortağıyla çalışırken iş akışı şu şekilde olur:

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, [Azure Hizmet Sorumlusu](../active-directory/develop/howto-create-service-principal-portal.md) kullanarak site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’a bir siteden siteye bağlantısı (iki etkin tünel) kurulur. Azure hem IKEv1’i hem de IKEv2'yi destekler. BGP isteğe bağlıdır.

#### <a name="partners"></a>Siteden siteye sanal WAN bağlantıları için iş ortakları

Kullanılabilir iş ortaklarının ve konumların bir listesi için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.


### <a name="er"></a>ExpressRoute bağlantıları (Önizleme)

ExpressRoute, şirket içi ağı özel bir bağlantı üzerinden Azure 'a bağlamanıza olanak tanır. Bağlantıyı oluşturmak için bkz. [sanal WAN kullanarak ExpressRoute bağlantısı oluşturma](virtual-wan-expressroute-portal.md).

## <a name="locations"></a>Yerlerini

Konum bilgileri için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

## <a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)
