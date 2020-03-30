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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188192"
---
VNet-to-VNet SSS VPN ağ geçidi bağlantıları için geçerlidir. VNet eşleme hakkında daha fazla bilgi için [Sanal ağ eşleme'ye](../articles/virtual-network/virtual-network-peering-overview.md)bakın.

### <a name="does-azure-charge-for-traffic-between-vnets"></a>Azure sanal ağlar arasındaki trafik için ücretli midir?

VPN ağ geçidi bağlantısı kullandığınızda, aynı bölgedeki VNet'ten VNet'e trafik her iki yönde de ücretsizdir. Bölgeler arası VNet-to-VNet çıkış trafiği, kaynak bölgelere göre giden inter-VNet veri aktarım hızları ile ücretlendirilir. Daha fazla bilgi için [VPN Ağ Geçidi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/vpn-gateway/)bakın. VPN ağ geçidi yerine VNet eşlemesini kullanarak VNet'lerinizi bağlıyorsanız, [Sanal ağ fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network/)bölümüne bakın.

### <a name="does-vnet-to-vnet-traffic-travel-across-the-internet"></a>VNet-to-VNet trafiği internet üzerinden seyahat ediyor mu?

Hayır. VNet'ten VNet'e trafik, Internet'ten değil, Microsoft Azure omurgasına doğru ilerler.

### <a name="can-i-establish-a-vnet-to-vnet-connection-across-azure-active-directory-aad-tenants"></a>Azure Active Directory (AAD) kiracıları arasında Bir VNet-vNet bağlantısı kurabilir miyim?

Evet, Azure VPN ağ geçitlerini kullanan VNet'ten VNet'e bağlantılar AAD kiracılarında çalışır.

### <a name="is-vnet-to-vnet-traffic-secure"></a>Sanal Ağdan Sanal Ağa trafiği güvenli mi?

Evet, IPsec/IKE şifrelemesi ile korunuyor.

### <a name="do-i-need-a-vpn-device-to-connect-vnets-together"></a>Sanal ağları birbirine bağlamak için bir VPN cihazı gerekiyor mu?

Hayır. İşletmeler arası bağlantı gerekmediği sürece birden fazla Azure sanal ağını birleştirmek için VPN cihazına gerek yoktur.

### <a name="do-my-vnets-need-to-be-in-the-same-region"></a>Sanal ağlarımın aynı bölgede olması gerekiyor mu?

Hayır. Sanal ağlar aynı ya da farklı Azure bölgelerinde (konumlarında) bulunabilir.

### <a name="if-the-vnets-arent-in-the-same-subscription-do-the-subscriptions-need-to-be-associated-with-the-same-active-directory-tenant"></a>VNet'ler aynı abonelikte değilse, aboneliklerin aynı Active Directory kiracısıyla ilişkilendirilmesi gerekir mi?

Hayır.

### <a name="can-i-use-vnet-to-vnet-to-connect-virtual-networks-in-separate-azure-instances"></a>Farklı Azure örneklerinde sanal ağları bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim? 

Hayır. Sanal Ağdan Sanal Ağa, aynı Azure örneğindeki sanal ağları bağlamayı destekler. Örneğin, genel Azure ile Çince/Almanya/ABD devlet tarafından denetlenebilen Azure örnekleri arasında bağlantı oluşturamazsınız. Bu senaryolar için Siteden Siteye VPN bağlantısı kullanmayı düşünün.

### <a name="can-i-use-vnet-to-vnet-along-with-multi-site-connections"></a>Sanal Ağdan Sanal Ağa bağlantıyı çoklu site bağlantılarıyla birlikte kullanabilir miyim?

Evet. Sanal ağ bağlantısı, çoklu site VPN’leri ile eşzamanlı olarak kullanılabilir.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Bir sanal ağ kaç şirket içi siteye ve sanal ağa bağlanabilir?

Ağ [Geçidi gereksinimleri](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#requirements) tablosuna bakın.

### <a name="can-i-use-vnet-to-vnet-to-connect-vms-or-cloud-services-outside-of-a-vnet"></a>Bir sanal ağın dışındaki sanal makineleri veya bulut hizmetlerini bağlamak için Sanal Ağdan Sanal Ağa bağlantı kullanabilir miyim?

Hayır. Sanal Ağdan Sanal Ağa, sanal ağları bağlamayı destekler. Sanal ağda olmayan sanal makinelerin veya bulut hizmetlerinin bağlanmasını desteklemez.

### <a name="can-a-cloud-service-or-a-load-balancing-endpoint-span-vnets"></a>Bir bulut hizmeti veya yük dengeleme uç noktası VNet'lere yayılabilir mi?

Hayır. Bir bulut hizmeti veya yük dengeleme uç noktası, birbirine bağlı olsalar bile sanal ağlar arasında yayılmaz.

### <a name="can-i-use-a-policybased-vpn-type-for-vnet-to-vnet-or-multi-site-connections"></a>VNet-to-VNet veya Multi-Site bağlantıları için Politika Tabanlı VPN türü kullanabilir miyim?

Hayır. VNet'ten VNet'e ve Çok Siteli bağlantılar, RouteBased (önceden dinamik yönlendirme olarak adlandırılır) VPN türlerine sahip Azure VPN ağ geçitleri gerektirir.

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>PolicyBased VPN türüne sahip VNet’i RouteBased VPN türüne sahip başka bir VNet’e bağlayabilir miyim?

Hayır, her iki sanal ağ da rota tabanlı (daha önce dinamik yönlendirme olarak adlandırılır) VPN'leri kullanmalı.

### <a name="do-vpn-tunnels-share-bandwidth"></a>VPN tünelleri bant genişliğini paylaşır mı?

Evet. Sanal ağ üzerindeki tüm VPN tünelleri, Azure VPN ağ geçidi ve aynı ağ geçidi çalışma süresi SLA’sı üzerinde aynı kullanılabilir bant genişliğini paylaşır.

### <a name="are-redundant-tunnels-supported"></a>Yedekli tüneller destekleniyor mu?

Bir sanal ağ çifti arasındaki yedekli tüneller, sanal ağ geçitlerinden biri etkin-etkin olarak yapılandırıldığında desteklenir.

### <a name="can-i-have-overlapping-address-spaces-for-vnet-to-vnet-configurations"></a>Sanal Ağdan Sanal Ağa yapılandırmalar için çakışan adres alanları kullanabilir miyim?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.

### <a name="can-there-be-overlapping-address-spaces-among-connected-virtual-networks-and-on-premises-local-sites"></a>Bağlı sanal ağlar ve şirket içi yerel siteleri arasında çakışan adres alanları olabilir mi?

Hayır. Çakışan IP adresi aralıklarını kullanamazsınız.



