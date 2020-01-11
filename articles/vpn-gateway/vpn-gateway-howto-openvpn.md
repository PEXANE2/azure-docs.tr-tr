---
title: "Azure VPN Gateway 'de OpenVPN 'i yapılandırma: PowerShell"
description: Azure için OpenVPN yapılandırma adımları VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 02514285570b764c6f7392b789f2ff7b427bb3a6
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75863766"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure Noktadan siteye VPN Gateway için OpenVPN 'yi yapılandırma

Bu makale, Azure VPN Gateway üzerinde **OpenVPN® Protokolü** ayarlamanıza yardımcı olur. Makalesinde, zaten çalışan bir noktadan siteye ortamınız olduğunu varsayılmaktadır. Bunu yapmazsanız, Noktadan siteye VPN oluşturmak için adım 1 ' deki yönergeleri kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="vnet"></a>1. Noktadan siteye VPN oluşturma

Zaten çalışan bir noktadan siteye ortamınız yoksa, bir tane oluşturmak için yönergeyi izleyin. Yerel Azure sertifikası kimlik doğrulamasıyla Noktadan siteye VPN ağ geçidi oluşturmak ve yapılandırmak için bkz. [noktadan sıteye VPN oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) . 

> [!IMPORTANT]
> Temel SKU, OpenVPN için desteklenmez.

## <a name="enable"></a>2. ağ geçidinde OpenVPN 'i etkinleştirin

Ağ geçidinizdeki OpenVPN 'i etkinleştirin. Aşağıdaki komutları çalıştırmadan önce ağ geçidinin Noktadan siteye (Ikev2 veya SSTP) zaten yapılandırılmış olduğundan emin olun:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Sonraki adımlar

İstemcileri OpenVPN için yapılandırmak için bkz. [OpenVPN Istemcilerini yapılandırma](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
