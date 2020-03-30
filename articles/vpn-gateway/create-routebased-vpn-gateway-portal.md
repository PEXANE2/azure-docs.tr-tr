---
title: 'Rota tabanlı VPN ağ geçidi oluşturma: portal'
titleSuffix: Azure VPN Gateway
description: Azure portalını kullanarak rota tabanlı VPN Ağ Geçidi oluşturma
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/03/2020
ms.author: cherylmc
ms.openlocfilehash: 6156d689a29ee348f9b1974d1520eb7d186a8d8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331358"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Azure portalını kullanarak rota tabanlı BIR VPN ağ geçidi oluşturma

Bu makale, Azure portalını kullanarak rota tabanlı bir Azure VPN ağ geçidi oluşturmanıza yardımcı olur.  Şirket içi ağınıza VPN bağlantısı oluşturulurken VPN ağ geçidi kullanılır. VNets'i bağlamak için vpn ağ geçidi de kullanabilirsiniz. 

Bu makaledeki adımlar bir VNet, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra, bağlantılar oluşturabilirsiniz. Bu adımlar için Azure aboneliği gerekir. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Sanal ağ oluşturma

[!INCLUDE [Basic Point-to-Site VNet](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="configure-and-create-the-gateway"></a><a name="gwvalues"></a>Ağ geçidini yapılandırma ve oluşturma

Bu adımda sanal ağınız için sanal ağ geçidi oluşturacaksınız. Bir ağ geçidinin oluşturulması, seçili ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

[!INCLUDE [Create a gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

>[!NOTE]
>Temel ağ geçidi SKU IKEv2 veya RADIUS kimlik doğrulamasını desteklemez. Mac istemcilerinin sanal ağınıza bağlanmasını planlıyorsanız, Temel SKU'yu kullanmayın.

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>VPN ağ geçidini görüntüleme

1. Ağ geçidi oluşturulduktan sonra portaldaki VNet1'e gidin. VPN ağ geçidi Genel Bakış sayfasında bağlı bir aygıt olarak görünür.

   ![Bağlı cihazlar](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "Bağlı cihazlar")

2. Aygıt listesinde, daha fazla bilgi görüntülemek için **VNet1GW'yi** tıklatın.

   ![VPN ağ geçidini görüntüleme](./media/create-routebased-vpn-gateway-portal/view-gateway.png "VPN ağ geçidini görüntüleme")

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma yı bitirdikten sonra, sanal ağınızla başka bir VNet arasında bağlantı oluşturabilirsiniz. Veya sanal ağınızla şirket içi konumunuz arasında bir bağlantı kurun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Başka bir VNet'e bağlantı oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
