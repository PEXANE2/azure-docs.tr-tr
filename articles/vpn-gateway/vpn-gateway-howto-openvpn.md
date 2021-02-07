---
title: Azure 'da OpenVPN 'i Yapılandırma VPN Gateway
description: PowerShell kullanarak noktadan siteye ortamı için Azure VPN Gateway 'da OpenVPN protokolünü nasıl etkinleştirebileceğinizi öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/05/2021
ms.author: cherylmc
ms.openlocfilehash: 34f24b8fbdb28e1b1f73e9db428c510d3f4661ce
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2021
ms.locfileid: "99804856"
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
1. **Tünel türü** için açılan listeden **OpenVPN (SSL)** veya **ıKEV2 ve OpenVPN (SSL)** öğesini seçin.

   :::image type="content" source="./media/vpn-gateway-howto-openvpn/portal.png" alt-text="Açılan menüden OpenVPN SSL 'yi seçin":::
1. Değişikliklerinizi kaydedin ve **sonraki adımlarla** devam edin.

Ağ geçidinizdeki OpenVPN 'i etkinleştirin.

1. Aşağıdaki örneği kullanarak ağ geçidinizdeki OpenVPN 'i etkinleştirin:

   ```azurepowershell-interactive
   $gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
   Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
   ```
1. **Sonraki adımlarla** devam edin.

## <a name="next-steps"></a>Sonraki adımlar

İstemcileri OpenVPN için yapılandırmak için bkz. [OpenVPN Istemcilerini yapılandırma](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
