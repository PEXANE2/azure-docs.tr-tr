---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Sanal WAN otomatik ölçeklenebilir dalı-dal bağlantısı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 05/14/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 8bdba64445212c564a3d4762bc8497be15f7d9a0
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83656999"
---
# <a name="about-azure-virtual-wan"></a>Azure sanal WAN hakkında

Azure sanal WAN, tek bir işlemsel arabirim sağlamak için birçok ağ, güvenlik ve yönlendirme işlevlerini birlikte getiren bir ağ hizmetidir. Bu işlevler, dal bağlantısını (SD-WAN veya VPN CPE gibi sanal WAN Iş ortağı cihazlarından bağlantı Otomasyonu aracılığıyla) içerir. siteden siteye VPN bağlantısı, uzak kullanıcı VPN (Noktadan siteye) bağlantısı, özel (ExpressRoute) bağlantısı, bulut içi bağlantı (sanal ağlar için geçişli bağlantı), VPN ExpressRoute arası bağlantı, yönlendirme, Azure Güvenlik Duvarı ve özel bağlantı için şifreleme. Sanal WAN kullanmaya başlamak için bu kullanım örneklerinin tümünün olması gerekmez. Yalnızca tek bir kullanım durumu ile çalışmaya başlayabilir ve ardından ağınızı geliştikçe ayarlayabilirsiniz.

Sanal WAN mimarisi, dallar (VPN/SD-WAN cihazları), kullanıcılar (Azure VPN/OpenVPN/Ikev2 istemcileri), ExpressRoute devreleri ve sanal ağlar için yerleşik ölçek ve performansa sahip bir hub ve bağlı olan mimaridir. Bulut barındırılan Ağı ' hub ' 'ın, farklı türlerde (' tekerlek) dağıtılabilecek uç noktalar arasında geçişli bağlantı sağladığından, [genel aktarım ağı mimarisine](virtual-wan-global-transit-network-architecture.md)izin verebilir.

Azure bölgeleri, bağlanmayı seçebileceğiniz hub olarak görev yapar. Tüm Hub 'lar standart bir sanal WAN 'da tam ağa bağlanır ve kullanıcının herhangi bir (bağlı olan) bağlantı için Microsoft omurgasını kullanmasını kolaylaştırır. SD-WAN/VPN cihazlarıyla bağlı bileşen bağlantısı için, kullanıcılar Azure sanal WAN 'da el ile ayarlayabilir veya sanal WAN CPE (SD-WAN/VPN) iş ortağı çözümünü kullanarak Azure bağlantısı kurabilirsiniz. Bağlantı otomasyonunu destekleyen iş ortakları listeliyoruz (cihaz bilgilerini Azure 'a dışarı aktarma, Azure yapılandırmasını indirme ve bağlantı kurma özelliği) ile Azure sanal WAN. Daha fazla bilgi için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan1.png)

Bu makalede, Azure sanal WAN 'da ağ bağlantısına hızlı bir bakış sunulmaktadır. Sanal WAN şu avantajları sunar:

* **Hub ve bağlı bileşen 'de tümleşik bağlantı çözümleri:** Siteden siteye yapılandırmayı ve şirket içi siteler ile bir Azure hub 'ı arasındaki bağlantıyı otomatikleştirin.
* **Otomatik uç kurulumu ve yapılandırması:** Sanal ağlarınızı ve iş yüklerinizi Azure hub'ına rahatça bağlayın.
* **Sezgisel sorun giderme:** Azure 'da uçtan uca akışı görebilir ve sonra gerekli eylemleri gerçekleştirmek için bu bilgileri kullanabilirsiniz.

## <a name="basic-and-standard-virtual-wans"></a><a name="basicstandard"></a>Temel ve standart sanal WAN 'Lar

İki tür sanal WAN vardır: temel ve standart. Aşağıdaki tabloda her tür için kullanılabilir yapılandırma gösterilmektedir.

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

Bir sanal WAN yükseltme adımları için bkz. [bir sanal WAN 'ı temel 'Ten standart sürümüne yükseltme](upgrade-virtual-wan.md).

## <a name="architecture"></a><a name="architecture"></a>Mimari

Sanal WAN mimarisi ve sanal WAN 'a geçiş hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Sanal WAN mimarisi](migrate-from-hub-spoke-topology.md)
* [Genel geçiş ağ mimarisi](virtual-wan-global-transit-network-architecture.md)

## <a name="virtual-wan-resources"></a><a name="resources"></a>Sanal WAN kaynakları

Uçtan uca sanal WAN'yi yapılandırmak için şu kaynakları oluşturursunuz:

* **virtualWAN:** virtualWAN kaynağı Azure ağınızdaki sanal bir katmanı temsil eder ve birden fazla kaynağı içeren bir koleksiyondur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Sanal WAN genelindeki sanal hub 'lar birbirleriyle iletişim kurmaz.

* **Hub:** Sanal hub Microsoft tarafından yönetilen bir sanal ağdır. Hub, bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları içerir. Şirket içi ağınızdan (vpnsite), sanal hub 'ın içindeki bir VPN Gateway bağlanabilir, ExpressRoute devrelerini bir sanal hub 'a bağlayabilir veya mobil kullanıcıları sanal hub 'daki Noktadan siteye ağ geçidine bağlayabilirsiniz. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, sanal WAN kullanırken, şirket içi sitenizden doğrudan VNet 'iniz için siteden siteye bağlantı oluşturmazsınız. Bunun yerine, hub 'a siteden siteye bağlantı oluşturursunuz. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır.

* **Hub sanal ağ bağlantısı:** Hub'ı sorunsuzca sanal ağınıza bağlamak için bir hub sanal ağ bağlantı kaynağı kullanılır.

* **(Önizleme) hub 'Dan hub 'a bağlantı** -hub 'ların hepsi bır sanal WAN 'da birbirlerine bağlanır. Bu, bir yerel hub 'a bağlı bir dalın, kullanıcının veya VNet 'in bağlı hub 'ların tam ağ mimarisini kullanarak başka bir dalla veya VNet ile iletişim kurabildiğini gösterir. Ayrıca, hub 'dan hub 'a bağlı çerçeveyi kullanarak sanal hub 'dan ve hub 'daki VNET 'lerde VNET 'ler arasında VNET 'ler de bağlayabilirsiniz.

* **Hub rota tablosu:** Bir sanal hub rotası oluşturarak rotayı sanal hub rota tablosuna uygulayabilirsiniz. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz.

**Ek sanal WAN kaynakları**

  * **Site:** Bu kaynak yalnızca siteden siteye bağlantılar için kullanılır. Site kaynağı **vpnsite**. Şirket içi VPN cihazınızı ve ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

## <a name="types-of-connectivity"></a><a name="connectivity"></a>Bağlantı türleri

Sanal WAN, aşağıdaki bağlantı türlerine izin verir: siteden siteye VPN, kullanıcı VPN (Noktadan siteye) ve ExpressRoute.

### <a name="site-to-site-vpn-connections"></a><a name="s2s"></a>Siteden Siteye VPN bağlantıları

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Bir sanal WAN siteden siteye bağlantı oluşturduğunuzda, kullanılabilir bir iş ortağıyla çalışabilirsiniz. Bir iş ortağı kullanmak istemiyorsanız bağlantıyı el ile yapılandırabilirsiniz. Daha fazla bilgi için bkz. [sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md).

#### <a name="virtual-wan-partner-workflow"></a><a name="s2spartner"></a>Sanal WAN iş akışı

Bir sanal WAN ortağıyla çalışırken iş akışı şu şekilde olur:

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, [Azure Hizmet Sorumlusu](../active-directory/develop/howto-create-service-principal-portal.md) kullanarak site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’a bir siteden siteye bağlantısı (iki etkin tünel) kurulur. Azure hem IKEv1’i hem de IKEv2'yi destekler. BGP isteğe bağlıdır.

#### <a name="partners-for-site-to-site-virtual-wan-connections"></a><a name="partners"></a>Siteden siteye sanal WAN bağlantıları için iş ortakları

Kullanılabilir iş ortaklarının ve konumların bir listesi için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

### <a name="user-vpn-point-to-site-connections"></a><a name="uservpn"></a>Kullanıcı VPN (Noktadan siteye) bağlantıları

Azure 'daki kaynaklarınıza bir IPSec/ıKE (Ikev2) veya OpenVPN bağlantısı üzerinden bağlanabilirsiniz. Bu tür bir bağlantı, istemci bilgisayarda bir VPN istemcisinin yapılandırılmasını gerektirir. Daha fazla bilgi için bkz. [Noktadan siteye bağlantı oluşturma](virtual-wan-point-to-site-portal.md).

### <a name="expressroute-connections"></a><a name="er"></a>ExpressRoute bağlantıları
ExpressRoute, şirket içi ağı özel bir bağlantı üzerinden Azure 'a bağlamanıza olanak tanır. Bağlantıyı oluşturmak için bkz. [sanal WAN kullanarak ExpressRoute bağlantısı oluşturma](virtual-wan-expressroute-portal.md).

## <a name="locations"></a><a name="locations"></a>Konumlar

Konum bilgileri için bkz. [sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesi.

## <a name="faq"></a><a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Sanal WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)
