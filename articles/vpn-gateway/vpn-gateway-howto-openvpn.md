---
title: Azure 'da OpenVPN 'i Yapılandırma VPN Gateway
description: PowerShell kullanarak noktadan siteye ortamı için Azure VPN Gateway 'da OpenVPN protokolünü nasıl etkinleştirebileceğinizi öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/05/2021
ms.author: cherylmc
ms.openlocfilehash: 4b48e98ab35b620030b15165d4c9341c0f11b440
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100393284"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure Noktadan siteye VPN Gateway için OpenVPN 'yi yapılandırma

Bu makale, Azure VPN Gateway üzerinde **OpenVPN® Protokolü** ayarlamanıza yardımcı olur. Portalı ya da PowerShell yönergelerini kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Makalesinde, zaten çalışan bir noktadan siteye ortamınız olduğunu varsayılmaktadır. Aksi takdirde, aşağıdaki yöntemlerden birini kullanarak bir tane oluşturun.

  * [Portal-Noktadan siteye oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)

  * [PowerShell-Noktadan siteye oluştur](vpn-gateway-howto-point-to-site-rm-ps.md)

* VPN Gateway 'nizin temel SKU 'YU kullanmadığından emin olun. Temel SKU, OpenVPN için desteklenmez.

## <a name="portal"></a>Portal

1. Portalda, **sanal ağ geçidinizin > Noktadan siteye yapılandırmaya** gidin.
1. **Tünel türü** için açılan listeden **OpenVPN (SSL)** öğesini seçin.

   :::image type="content" source="./media/vpn-gateway-howto-openvpn/portal.png" alt-text="Açılan menüden OpenVPN SSL 'yi seçin":::
1. Değişikliklerinizi kaydedin ve **sonraki adımlarla** devam edin.

## <a name="enable-openvpn-on-your-gateway-using-powershell"></a>PowerShell kullanarak ağ geçidinizdeki OpenVPN 'i etkinleştirin.

1. Aşağıdaki örneği kullanarak ağ geçidinizdeki OpenVPN 'i etkinleştirin:

   ```azurepowershell-interactive
   $gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
   ```
1. **Sonraki adımlarla** devam edin.

## <a name="next-steps"></a>Sonraki adımlar

İstemcileri OpenVPN için yapılandırmak için bkz. [OpenVPN Istemcilerini yapılandırma](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
