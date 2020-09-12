---
title: 'Rota tabanlı VPN Gateway oluşturma: Portal'
titleSuffix: Azure VPN Gateway
description: Şirket içi ağınıza yönelik bir VPN bağlantısı veya sanal ağlara bağlanmak için hızlı bir şekilde rota tabanlı Azure VPN Gateway oluşturmak için Azure portal kullanın.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 104d911164d0194efba41f1405c17fc240e8906d
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89393777"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Azure portal kullanarak rota tabanlı bir VPN ağ geçidi oluşturma

Bu makale, Azure portal kullanarak hızlı bir şekilde rota tabanlı Azure VPN Gateway oluşturmanıza yardımcı olur.  VPN ağ geçidi, şirket içi ağınıza bir VPN bağlantısı oluştururken kullanılır. VNET 'leri bağlamak için bir VPN ağ geçidi de kullanabilirsiniz. 

Bu makaledeki adımlarda bir VNet, alt ağ, bir ağ geçidi alt ağı ve rota tabanlı VPN Gateway (sanal ağ geçidi) oluşturulur. Ağ geçidi oluşturma işlemi tamamlandıktan sonra bağlantılar oluşturabilirsiniz. Bu adımlar, bir Azure aboneliği gerektirir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Sanal ağ oluşturma

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="configure-and-create-the-gateway"></a><a name="gwvalues"></a>Ağ geçidini yapılandırma ve oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [Create a gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

>[!NOTE]
>Temel ağ geçidi SKU 'SU Ikev2 veya RADIUS kimlik doğrulamasını desteklemez. Mac istemcilerinin sanal ağınıza bağlanmasını planlıyorsanız, temel SKU 'YU kullanmayın.

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>VPN ağ geçidini görüntüleme

1. Ağ Geçidi oluşturulduktan sonra, portalda VNet1 adresine gidin. VPN ağ geçidi, bağlı bir cihaz olarak genel bakış sayfasında görünür.

   ![Bağlı cihazlar](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Bağlı cihazlar")

2. Daha fazla bilgi görüntülemek için cihaz listesinde **VNet1GW** ' e tıklayın.

   ![VPN ağ geçidini görüntüle](./media/create-routebased-vpn-gateway-portal/view-gateway.png "VPN ağ geçidini görüntüle")

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidinin oluşturma işlemi tamamlandıktan sonra, sanal ağınız ile başka bir VNet arasında bir bağlantı oluşturabilirsiniz. Veya, sanal ağınız ile şirket içi bir konum arasında bir bağlantı oluşturun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Başka bir VNet bağlantısı oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
