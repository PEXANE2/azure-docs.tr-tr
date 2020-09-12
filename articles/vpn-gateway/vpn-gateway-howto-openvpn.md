---
title: "Azure VPN Gateway 'de OpenVPN 'i yapılandırma: PowerShell"
description: PowerShell kullanarak noktadan siteye ortamı için Azure VPN Gateway 'da OpenVPN protokolünü nasıl etkinleştirebileceğinizi öğrenin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 57bc53c28866b3f16e742c27b6a1600bfe6f44ab
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89418832"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure Noktadan siteye VPN Gateway için OpenVPN 'yi yapılandırma

Bu makale, Azure VPN Gateway üzerinde **OpenVPN® Protokolü** ayarlamanıza yardımcı olur. Makalesinde, zaten çalışan bir noktadan siteye ortamınız olduğunu varsayılmaktadır. Bunu yapmazsanız, Noktadan siteye VPN oluşturmak için adım 1 ' deki yönergeleri kullanın.



## <a name="1-create-a-point-to-site-vpn"></a><a name="vnet"></a>1. Noktadan siteye VPN oluşturma

Zaten çalışan bir noktadan siteye ortamınız yoksa, bir tane oluşturmak için yönergeyi izleyin. Yerel Azure sertifikası kimlik doğrulamasıyla Noktadan siteye VPN ağ geçidi oluşturmak ve yapılandırmak için bkz. [noktadan sıteye VPN oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) . 

> [!IMPORTANT]
> Temel SKU, OpenVPN için desteklenmez.

## <a name="2-enable-openvpn-on-the-gateway"></a><a name="enable"></a>2. ağ geçidinde OpenVPN 'i etkinleştirin

Ağ geçidinizdeki OpenVPN 'i etkinleştirin. Aşağıdaki komutları çalıştırmadan önce ağ geçidinin Noktadan siteye (Ikev2 veya SSTP) zaten yapılandırılmış olduğundan emin olun:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Sonraki adımlar

İstemcileri OpenVPN için yapılandırmak için bkz. [OpenVPN Istemcilerini yapılandırma](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN", OpenVPN Inc 'nin ticari markasıdır.**
