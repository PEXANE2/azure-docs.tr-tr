---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/03/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 72ddd0b6cd6c3e12417d3698c403f89312b531f4
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188192"
---
VNet-VNet ile ilgili SSS VPN Gateway bağlantıları için geçerlidir. VNet eşlemesi hakkında daha fazla bilgi için bkz. [sanal ağ eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Azure sanal ağlar arasındaki trafik için ücretli midir?

VPN Ağ Geçidi bağlantısı kullandığınızda aynı bölgedeki VNet-VNet trafiği her iki yönde de ücretsizdir. Bölgeler arası VNet 'ten VNet 'e çıkış trafiği, kaynak bölgelere göre giden sanal ağlar arası veri aktarımı ücretleri ile ücretlendirilir. Daha fazla bilgi için bkz. [VPN Gateway fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/vpn-gateway/). Sanal ağlarınızı VPN Gateway yerine VNet eşlemesi ile bağlıyorsanız, bkz. [sanal ağ fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>VNet-VNet trafiği internet üzerinden seyahat ediyor mu?

Hayır. VNet-VNet trafiği, internet değil Microsoft Azure omurgası arasında dolaşır.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-azure-active-directory-aad-tenants"></a>Azure Active Directory (AAD) kiracılarından VNET 'ten VNet 'e bağlantı kurabilir miyim?

Evet, Azure VPN ağ geçitleri kullanan VNet 'ten VNet 'e bağlantılar AAD kiracılarında çalışır.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Sanal Ağdan Sanal Ağa trafiği güvenli mi?

Evet, IPSec/ıKE şifrelemesi tarafından korunuyor.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Sanal ağları birbirine bağlamak için bir VPN cihazı gerekiyor mu?

Hayır. İşletmeler arası bağlantı gerekmediği sürece birden fazla Azure sanal ağını birleştirmek için VPN cihazına gerek yoktur.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Sanal ağlarımın aynı bölgede olması gerekiyor mu?

Hayır. Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.

### <a name="if-the-vnets-arent-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-active-directory-tenant"></a>Sanal ağlar aynı abonelikte değilse, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmesi gerekir mi?

Hayır.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>Farklı Azure örneklerinde sanal ağları bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim? 

Hayır. Sanal Ağdan Sanal Ağa, aynı Azure örneğindeki sanal ağları bağlamayı destekler. Örneğin, küresel Azure ile Çince/ABD kamu Azure örnekleri arasında bir bağlantı oluşturamazsınız. Bu senaryolar için siteden siteye VPN bağlantısı kullanmayı göz önünde bulundurun.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Sanal Ağdan Sanal Ağa bağlantıyı çoklu site bağlantılarıyla birlikte kullanabilir miyim?

Evet. Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Bir sanal ağ kaç şirket içi siteye ve sanal ağa bağlanabilir?

[Ağ geçidi gereksinimleri](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) tablosuna bakın.

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Bir sanal ağın dışındaki sanal makineleri veya bulut hizmetlerini bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim?

Hayır. Sanal Ağdan Sanal Ağa, sanal ağları bağlamayı destekler. Bir sanal ağ içinde olmayan sanal makinelerin veya bulut hizmetlerinin bağlanmasını desteklemez.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Bulut hizmeti ya da yük dengeleme uç noktası sanal ağlara yayılamidir?

Hayır. Bir bulut hizmeti veya yük dengeleme uç noktası, birbirine bağlı olsalar dahi sanal ağlar arasında yayılamaz.

### <a name="can-i-use-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>VNet-VNet veya çok siteli bağlantılar için PolicyBased VPN türü kullanabilir miyim?

Hayır. VNet-VNet ve çok siteli bağlantılar, Azure VPN ağ geçitlerini (daha önce dinamik yönlendirme adı verilir) VPN türleri gerektirir.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>PolicyBased VPN türüne sahip VNet’i RouteBased VPN türüne sahip başka bir VNet’e bağlayabilir miyim?

Hayır, her iki sanal ağın da rota tabanlı (önceki adı dinamik yönlendirme) VPN 'Leri kullanması gerekır.

### <a name="do-vpn-tunnels-share-bandwidth"></a>VPN tünelleri bant genişliğini paylaşır mı?

Evet. Sanal ağ üzerindeki tüm VPN tünelleri, Azure VPN ağ geçidi ve aynı ağ geçidi çalışma süresi SLA’sı üzerinde aynı kullanılabilir bant genişliğini paylaşır.

### <a name="are-redundant-tunnels-supported"></a>Yedekli tüneller destekleniyor mu?

Bir sanal ağ çifti arasındaki yedekli tüneller, sanal ağ geçitlerinden biri etkin-etkin olarak yapılandırıldığında desteklenir.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Sanal Ağdan Sanal Ağa yapılandırmalar için çakışan adres alanları kullanabilir miyim?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Bağlı sanal ağlar ve şirket içi yerel siteleri arasında çakışan adres alanları olabilir mi?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.



