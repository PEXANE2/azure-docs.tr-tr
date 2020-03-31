---
title: "OpenVPN Azure VPN Ağ Geçidi'nde nasıl yapılandırılabilen: PowerShell"
description: Azure VPN Ağ Geçidi için OpenVPN'i yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 7505420cc31fe751ecc0c114a89fea0734cbc6cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162416"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure noktaya noktaya VPN Ağ Geçidi için OpenVPN'i yapılandırın

Bu makale, Azure VPN Ağ Geçidi'nde **OpenVPN® Protokolü'nü** kurmanıza yardımcı olur. Makale, zaten bir çalışma noktası-to-site ortamı var varsayar. Bunu yapmazsanız, bir noktadan siteye VPN oluşturmak için adım 1 yönergeleri kullanın.



## <a name="1-create-a-point-to-site-vpn"></a><a name="vnet"></a>1. Bir noktadan siteye VPN oluşturma

Zaten işleyen bir noktadan siteye ortamınız yoksa, bir ortam oluşturmak için yönergeyi izleyin. Bkz. Yerel Azure sertifikası kimlik doğrulaması yla bir noktadan siteye VPN ağ geçidi oluşturmak ve yapılandırmak için bir noktadan siteye [VPN oluştur.](vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

> [!IMPORTANT]
> Temel SKU OpenVPN için desteklenmez.

## <a name="2-enable-openvpn-on-the-gateway"></a><a name="enable"></a>2. Ağ geçidinde OpenVPN'i etkinleştirin

Ağ geçidinizde OpenVPN'i etkinleştirin. Ağ geçidinin, aşağıdaki komutları çalıştırmadan önce noktadan noktaya (IKEv2 veya SSTP) göre zaten yapılandırıldıklarından emin olun:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Sonraki adımlar

OpenVPN istemcilerini yapılandırmak için [OpenVPN istemcilerini yapılandırmaya](vpn-gateway-howto-openvpn-clients.md)bakın.

**"OpenVPN", OpenVPN Inc. şirketinin ticari markasıdır.**
