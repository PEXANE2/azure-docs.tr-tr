---
title: ExpressRoute özel eşlemesi üzerinden siteden siteye VPN bağlantıları
description: Bu makale, trafiği şifrelemek için ExpressRoute özel eşlemesi üzerinden siteden siteye VPN 'yi etkinleştirmenize yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/08/2020
ms.author: cherylmc
ms.openlocfilehash: effbe8e771922ea07ad908dd4871f8dcdb7c1d19
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941511"
---
# <a name="configure-a-site-to-site-vpn-connection-over-expressroute-private-peering-preview"></a>ExpressRoute özel eşlemesi (Önizleme) üzerinden siteden siteye VPN bağlantısı yapılandırma

Bir RFC 1918 IP adresi kullanarak bir ExpressRoute özel eşlemesi üzerinden bir sanal ağ geçidine siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırma aşağıdaki avantajları sağlar:

* Özel eşleme üzerinden trafik şifrelenir.

* Bir sanal ağ geçidine bağlanan Noktadan siteye kullanıcılar, şirket içi kaynaklara erişmek için ExpressRoute 'u (siteden siteye tünel aracılığıyla) kullanabilir.

>[!NOTE]
>Bu özellik yalnızca bölgesel olarak yedekli ağ geçitlerinde desteklenir. Örneğin, VpnGw1AZ, VpnGw2AZ, vb.
>

Bu yapılandırmayı gerçekleştirmek için aşağıdaki önkoşulları karşıladığınızdan emin olun:

* VPN ağ geçidinin oluşturulduğu (veya olacağı) VNet ile bağlantılı bir ExpressRoute devreniz var.

* Sanal ağda RFC1918 (özel) IP üzerinden ExpressRoute bağlantı hattı üzerinden kaynaklara ulaşabilirsiniz.

## <a name="routing"></a><a name="routing"></a>Yönlendirme

**Şekil 1** ' de ExpressRoute özel EŞLEMESI üzerinden VPN bağlantısı örneği gösterilmektedir. Bu örnekte, ExpressRoute özel eşlemesi üzerinden Azure hub VPN Gateway 'e bağlı olan şirket içi ağ içinde bir ağ görürsünüz. Bu yapılandırmanın önemli bir yönü, hem ExpressRoute hem de VPN yolları üzerinde şirket içi ağlar ve Azure arasındaki yönlendirbir yoldur.

Şekil 1

:::image type="content" source="media/site-to-site-vpn-private-peering/routing.png" alt-text="Şekil 1":::

Bağlantı kurma basittir:

1. ExpressRoute devresi ve özel eşleme ile ExpressRoute bağlantısı kurun.

1. Bu makaledeki adımları kullanarak VPN bağlantısı kurun.

### <a name="traffic-from-on-premises-networks-to-azure"></a>Şirket içi ağlardan Azure 'a trafik

Şirket içi ağlardan Azure 'a giden trafik için Azure önekleri hem ExpressRoute özel eşleme BGP hem de VPN BGP aracılığıyla tanıtılabilir. Sonuç, şirket içi ağlardan Azure 'a doğru iki ağ yolu (yollar) olur:

• IPsec korumalı yol üzerinde bir ağ yolu.

• IPSec koruması olmadan doğrudan ExpressRoute üzerinden bir ağ yolu.

İletişime şifreleme uygulamak için, **Şekil 1**' deki VPN bağlantılı ağ için, ŞIRKET içi VPN ağ geçidi üzerinden Azure rotalarının doğrudan ExpressRoute yolu üzerinden tercih edildiği şekilde emin olmanız gerekir.

### <a name="traffic-from-azure-to-on-premises-networks"></a>Azure 'dan şirket içi ağlara giden trafik

Aynı gereksinim, Azure 'dan şirket içi ağlara giden trafiğe de yöneliktir. IPSec yolunun doğrudan ExpressRoute yolu (IPSec olmadan) üzerinden tercih edildiğini sağlamak için iki seçeneğiniz vardır:

• VPN **bağlantılı ağ IÇIN VPN BGP oturumunda daha özel ön ekler duyurur**. ExpressRoute özel eşlemesi üzerinden VPN bağlantılı ağı kapsayan daha büyük bir Aralık verebilir ve ardından VPN BGP oturumunda daha belirgin aralıklar sağlayabilirsiniz. Örneğin, ExpressRoute üzerinden 10.0.0.0/16, VPN üzerinden 10.0.1.0/24 duyurusunu yapın.

• **VPN ve ExpressRoute için kopuk ön ekleri tanıtma**. VPN bağlantılı ağ aralıkları diğer ExpressRoute bağlantılı ağlardan ayrılarıysa, ön ekleri VPN ve ExpressRoute BGP oturumlarındaki tanıtabilirsiniz. Örneğin, ExpressRoute üzerinden 10.0.0.0/24, VPN üzerinden 10.0.1.0/24 duyurusunu yapın.

Bu örneklerde, Azure, VPN koruması olmadan doğrudan ExpressRoute üzerinden değil, VPN bağlantısı üzerinden 10.0.1.0/24 ' e trafik gönderir.

>[!Warning]
>Aynı önekleri hem ExpressRoute hem de VPN bağlantıları üzerinden tanıyorsanız Azure >, doğrudan VPN koruması olmadan ExpressRoute yolunu kullanacaktır.
>

## <a name="portal-steps"></a><a name="portal"></a>Portal adımları

1. Siteden siteye bağlantı yapılandırın. Adımlar için, [siteden siteye yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md) makalesine bakın. Ağ Geçidi için bir bölge yedekli ağ geçidi SKU 'SU seçtiğinizden emin olun. Bölgesel olarak yedekli SKU 'Ların SKU 'nun sonunda "AZ" vardır. Örneğin, VpnGw1AZ.

   :::image type="content" source="media/site-to-site-vpn-private-peering/gateway.png" alt-text="Ağ Geçidi özel IP 'Leri":::
1. Ağ geçidinde özel IP 'Leri etkinleştirin. **Yapılandırma**' yı seçin, ardından **ağ geçidi özel IP** 'lerini **etkin**olarak ayarlayın. Değişikliklerinizi kaydetmek için **Kaydet** seçeneğini belirleyin.
1. **Genel bakış** sayfasında, özel IP adresini görüntülemek Için **daha fazla göster** ' i seçin. Yapılandırma adımlarında daha sonra kullanmak için bu bilgileri yazın.

   :::image type="content" source="media/site-to-site-vpn-private-peering/gateway-overview.png" alt-text="Genel Bakış sayfası" lightbox="media/site-to-site-vpn-private-peering/gateway-overview.png":::
1. Bağlantıda **Azure özel IP adresi kullanımını** etkinleştirmek için  **yapılandırma**' yı seçin. **Azure özel IP adresi kullan** ' ı **etkin**olarak ayarlayın ve **Kaydet**' i seçin.

   :::image type="content" source="media/site-to-site-vpn-private-peering/connection.png" alt-text="Ağ Geçidi özel IP 'Leri-etkin":::
1. Güvenlik duvarınızdan, adım 3 ' te yazdığınız özel IP 'ye ping gönderin. Özel IP 'nin ExpressRoute özel eşlemesi üzerinden erişilebilir olması gerekir.
1. ExpressRoute özel eşlemesi üzerinden siteden siteye tüneli oluşturmak için bu özel IP 'yi şirket içi güvenlik duvarınız üzerinde uzak IP olarak kullanın.

## <a name="powershell-steps"></a><a name="powershell"></a>PowerShell adımları

1. Siteden siteye bağlantı yapılandırın. Adımlar için, [siteden sıteye VPN yapılandırma](vpn-gateway-howto-site-to-site-resource-manager-portal.md) makalesine bakın. Ağ Geçidi için bir bölge yedekli ağ geçidi SKU 'SU seçtiğinizden emin olun. Bölgesel olarak yedekli SKU 'Ların SKU 'nun sonunda "AZ" vardır. Örneğin, VpnGw1AZ.
1. Aşağıdaki PowerShell komutlarını kullanarak ağ geçidinde özel IP 'yi kullanmak için bayrağını ayarlayın:

   ```azurepowershell-interactive
   $Gateway = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroup <name of resource group>

   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $Gateway -EnablePrivateIpAddress $true
   ```

   Ortak ve özel bir IP adresi görmeniz gerekir. Çıktının "TunnelIpAddresses" bölümü altındaki IP adresini yazın. Bu bilgileri sonraki bir adımda kullanacaksınız.
1. Aşağıdaki PowerShell komutunu kullanarak özel IP adresini kullanmak için bağlantıyı ayarlayın:

   ```azurepowershell-interactive
   $Connection = get-AzVirtualNetworkGatewayConnection -Name <name of the connection> -ResourceGroupName <name of resource group>

   Set-AzVirtualNetworkGatewayConnection --VirtualNetworkGatewayConnection $Connection -UseLocalAzureIpAddress $true
   ```
1. Güvenlik duvarınızdan, adım 2 ' de yazdığınız özel IP 'ye ping gönderin. ExpressRoute özel eşlemesi üzerinden erişilebilmelidir.
1. ExpressRoute özel eşlemesi üzerinden siteden siteye tüneli oluşturmak için bu özel IP 'yi şirket içi güvenlik duvarınız üzerinde uzak IP olarak kullanın.

## <a name="next-steps"></a>Sonraki adımlar

VPN Gateway hakkında daha fazla bilgi için bkz. [VPN Gateway nedir?](vpn-gateway-about-vpngateways.md)
