---
title: "Azure VPN Gateway 'de OpenVPN 'i yapılandırma: PowerShell"
description: Azure için OpenVPN yapılandırma adımları VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 7505420cc31fe751ecc0c114a89fea0734cbc6cf
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162416"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure Noktadan siteye VPN Gateway için OpenVPN 'yi yapılandırma

Bu makale, Azure VPN Gateway üzerinde **OpenVPN® Protokolü** ayarlamanıza yardımcı olur. Makalesinde, zaten çalışan bir noktadan siteye ortamınız olduğunu varsayılmaktadır. Bunu yapmazsanız, Noktadan siteye VPN oluşturmak için adım 1 ' deki yönergeleri kullanın.



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
