---
title: Azure Sanal WAN’ye genel bakış | Microsoft Docs
description: Sanal WAN otomatik ölçeklenebilir daldan şubeye bağlantı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 02/05/2020
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 927c09f61ce0847c72cefb51935116070e956861
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743110"
---
# <a name="about-azure-virtual-wan"></a>Azure Virtual WAN Hakkında

Azure Virtual WAN, tek bir operasyonel arabirim sağlamak için birçok ağ, güvenlik ve yönlendirme işlevlerini bir araya getiren bir ağ hizmetidir. Bu işlevler arasında Şube bağlantısı (SD-WAN veya VPN CPE gibi Sanal WAN İş Ortağı aygıtlarından bağlantı otomasyonu yoluyla) , Siteden siteye VPN bağlantısı, Uzaktan Kullanıcı VPN (noktaya sayfa) bağlantısı, Özel (ExpressRoute) bağlantısı, Bulut içi bağlantı (Sanal Ağlar için geçişli bağlantı), VPN ExpressRoute Bağlantı, Routing, Azure güvenlik duvarı, şifreleme vb. bulunmaktadır. Sanal WAN kullanmaya başlamak için bu kullanım örneklerinin tümüne sahip olmak zorunda değildir. Sadece tek bir kullanım örneği ile başlamak ve geliştikçe ağınızı ayarlayabilirsiniz. Virtual WAN mimarisi, şubeler (VPN/SD-WAN aygıtları), kullanıcılar (Azure VPN/OpenVPN/IKEv2 Istemcileri), ExpressRoute devreleri ve Sanal Ağlar için ölçek ve performans yerleşik bir hub ve kollu mimarisidir. Bulut barındırılan ağ 'hub'ının farklı sözcü türlerine dağıtılabilen uç noktalar arasında geçişli bağlantı sağladığı [küresel geçiş ağı mimarisine](virtual-wan-global-transit-network-architecture.md) olanak tanır.

Azure bölgeleri, bağlanmayı seçebileceğiniz hub'lar olarak hizmet vermektedir. Tüm hub'lar Standart Sanal WAN'da tam ağ la bağlanır ve kullanıcının Microsoft omurgasını herhangi bir (herhangi bir konuşmalı) bağlantı için kullanmasını kolaylaştırır. SD-WAN/VPN aygıtlarıyla yapılan konuşmalı bağlantı için kullanıcılar, Azure Sanal WAN'da el ile ayarlayabilir veya Azure bağlantısı ayarlamak için Virtual WAN CPE (SD-WAN/VPN) iş ortağı çözümünü kullanabilir. Azure Virtual WAN ile bağlantı otomasyonu (aygıt bilgilerini Azure'a dışa aktarma, Azure yapılandırmasını indirme ve bağlantı kurma) destekleyen iş ortaklarının bir listesi var. Daha fazla bilgi için [Virtual WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesine bakın. 

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan1.png)

Bu makale, Azure Virtual WAN'da ağ bağlantısına hızlı bir görünüm sağlar. Sanal WAN şu avantajları sunar:

* **Hub ve spoke'de entegre bağlantı çözümleri:** Şirket içi siteler ve Azure hub'ı arasında siteden siteye yapılandırmayı ve bağlantıyı otomatikleştirin.
* **Otomatik uç kurulumu ve yapılandırması:** Sanal ağlarınızı ve iş yüklerinizi Azure hub'ına rahatça bağlayın.
* **Sezgisel sorun giderme:** Azure'da uçuça akışı görebilir ve gerekli eylemleri yapmak için bu bilgileri kullanabilirsiniz.

## <a name="basic-and-standard-virtual-wans"></a><a name="basicstandard"></a>Temel ve Standart sanal WAN'lar

İki tür sanal WAN vardır: Temel ve Standart. Aşağıdaki tablo, her tür için kullanılabilir yapılandırmaları gösterir.

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

Sanal bir WAN yükseltme adımları için, [temelden Standarda sanal bir WAN yükseltme'ye](upgrade-virtual-wan.md)bakın.

## <a name="architecture"></a><a name="architecture"></a>Mimari

Sanal WAN mimarisi ve Virtual WAN'a nasıl geçirilir hakkında bilgi için aşağıdaki makalelere bakın:

* [Sanal WAN mimarisi](migrate-from-hub-spoke-topology.md)
* [Genel geçiş ağ mimarisi](virtual-wan-global-transit-network-architecture.md)

## <a name="virtual-wan-resources"></a><a name="resources"></a>Sanal WAN kaynakları

Uçtan uca sanal WAN'yi yapılandırmak için şu kaynakları oluşturursunuz:

* **virtualWAN:** virtualWAN kaynağı Azure ağınızdaki sanal bir katmanı temsil eder ve birden fazla kaynağı içeren bir koleksiyondur. Sanal WAN’de bulunmasını istediğiniz tüm sanal hub'larınızın bağlantılarını içerir. Sanal WAN kaynakları birbirinden yalıtılmıştır ve ortak bir hub içeremezler. Virtual WAN'daki sanal hub'lar birbirleriyle iletişim kurmaz.

* **Hub:** Sanal hub Microsoft tarafından yönetilen bir sanal ağdır. Hub, bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları içerir. Şirket içi ağınızdan (vpn sitesi) sanal hub içindeki bir VPN Ağ Geçidi'ne bağlanabilir, ExpressRoute devrelerini sanal bir hub'a bağlayabilir ve hatta mobil kullanıcıları sanal hub'daki bir noktadan siteye ağ geçidine bağlayabilirsiniz. Hub, bir bölgedeki ağınızın merkezidir. Her Azure bölgesinde tek bir hub bulunabilir.

  Hub ağ geçidi, ExpressRoute ve VPN Gateway için kullandığınız sanal ağ geçidiyle aynı değildir. Örneğin, Virtual WAN kullanırken, şirket içi sitenizden doğrudan VNet'inize site-site bağlantısı oluşturmazsınız. Bunun yerine, hub'a siteden siteye bağlantı oluşturursunuz. Trafik her zaman hub ağ geçidi üzerinden gider. Başka bir deyişle, Sanal Ağlarınızın kendi sanal ağ geçitlerine gerek kalmaz. Sanal WAN, Sanal Ağlarınızın sanal hub ve sanal hub ağ geçidi aracılığıyla ölçeklemeden kolayca yararlanmasına olanak tanır.

* **Hub sanal ağ bağlantısı:** Hub'ı sorunsuzca sanal ağınıza bağlamak için bir hub sanal ağ bağlantı kaynağı kullanılır.

* **(Önizleme) Hub'dan Hub'a bağlantı** - Hub'ların tümü sanal BIR WAN'da birbirine bağlıdır. Bu, yerel bir hub'a bağlı bir şubenin, kullanıcının veya VNet'in bağlı hub'ların tam kafes mimarisini kullanarak başka bir dalla veya VNet ile iletişim kurabileceği anlamına gelir. Ayrıca, sanal hub üzerinden geçen bir hub'ın içindeki VNets'i ve hub üzerinden vnet'leri hub'a bağlı çerçeveyi kullanarak bağlayabilirsiniz.

* **Hub rota tablosu:** Bir sanal hub rotası oluşturarak rotayı sanal hub rota tablosuna uygulayabilirsiniz. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz.

**Ek Sanal WAN kaynakları**

  * **Site:** Bu kaynak yalnızca siteden siteye bağlantılar için kullanılır. Site kaynağı **vpnsite**. Şirket içi VPN aygıtınızı ve ayarlarını temsil eder. Sanal WAN iş ortağıyla çalıştığınızda, bu bilgileri otomatik olarak Azure’a aktarmak için yerleşik bir çözümünüz olur.

## <a name="types-of-connectivity"></a><a name="connectivity"></a>Bağlantı türleri

Virtual WAN aşağıdaki bağlantı türlerine izin verir: Siteden Siteye VPN, Kullanıcı VPN 'i (Noktadan Siteye) ve ExpressRoute.

### <a name="site-to-site-vpn-connections"></a><a name="s2s"></a>Siteden Siteye VPN bağlantıları

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Sanal bir WAN site-site bağlantısı oluşturduğunuzda, kullanılabilir bir iş ortağıyla çalışabilirsiniz. İş ortağı kullanmak istemiyorsanız, bağlantıyı el ile yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Virtual WAN kullanarak siteden siteye bağlantı oluştur.](virtual-wan-site-to-site-portal.md)

#### <a name="virtual-wan-partner-workflow"></a><a name="s2spartner"></a>Sanal WAN iş ortağı iş akışı

Sanal WAN iş ortağıyla çalışırken, iş akışı:

1. Dal cihazı (VPN/SDWAN) denetleyicisinin, [Azure Hizmet Sorumlusu](../active-directory/develop/howto-create-service-principal-portal.md) kullanarak site merkezli bilgileri Azure’a aktarmak için kimliği doğrulanır.
2. Dal cihazı (VPN/SDWAN) denetleyicisi Azure bağlantı yapılandırmasını alır ve yerel cihazı güncelleştirir. Bu, şirket içi VPN cihazının yapılandırma indirme, düzenleme ve güncelleştirme işlemlerini otomatikleştirir.
3. Cihazda doğru Azure yapılandırması olduktan sonra, Azure WAN’a bir siteden siteye bağlantısı (iki etkin tünel) kurulur. Azure hem IKEv1’i hem de IKEv2'yi destekler. BGP isteğe bağlıdır.

#### <a name="partners-for-site-to-site-virtual-wan-connections"></a><a name="partners"></a>Siteden siteye sanal WAN bağlantıları için iş ortakları

Kullanılabilir iş ortaklarının ve konumlarının listesi için [Sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesine bakın.

### <a name="user-vpn-point-to-site-connections"></a><a name="uservpn"></a>Kullanıcı VPN (noktadan siteye) bağlantıları

IPsec/IKE (IKEv2) veya OpenVPN bağlantısı üzerinden Azure'daki kaynaklarınıza bağlanabilirsiniz. Bu tür bir bağlantı istemci bilgisayarda yapılandırılması için bir VPN istemcisi gerektirir. Daha fazla bilgi için [bkz.](virtual-wan-point-to-site-portal.md)

### <a name="expressroute-connections"></a><a name="er"></a>ExpressRoute bağlantıları
ExpressRoute, şirket içi ağı azure'a özel bir bağlantı üzerinden bağlamanızı sağlar. Bağlantıyı oluşturmak için bkz. [Sanal WAN kullanarak ExpressRoute bağlantısı oluştur.](virtual-wan-expressroute-portal.md)

## <a name="locations"></a><a name="locations"></a>Konumlar

Konum bilgileri için [Sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesine bakın.

## <a name="faq"></a><a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Virtual WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)
