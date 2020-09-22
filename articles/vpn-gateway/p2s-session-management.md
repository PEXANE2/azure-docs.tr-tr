---
title: 'Azure VPN Gateway: Noktadan siteye VPN oturumu yönetimi'
description: Bu makale, Noktadan siteye VPN oturumlarını görüntülemenize ve bunların bağlantısını kesmenize yardımcı olur.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 08/26/2020
ms.author: cherylmc
ms.openlocfilehash: 3c9023e23795fd51e5519727a0ab174dc6f75830
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941514"
---
# <a name="point-to-site-vpn-session-management"></a>Noktadan siteye VPN oturumu yönetimi

Azure sanal ağ geçitleri, geçerli noktadan siteye VPN oturumlarını görüntülemenin ve bunların bağlantısını kesmenin kolay bir yolunu sunar. Bu makale, geçerli oturumları görüntülemenize ve bunların bağlantısını kesmenize yardımcı olur.

>[!NOTE]
>Oturum durumu 5 dakikada bir güncelleştirilir. Anında güncellenmez.
>

## <a name="portal"></a>Portal

Portalda bir oturumu görüntülemek ve bağlantısını kesmek için:

1. VPN ağ geçidine gidin.
1. **İzleme** bölümünde, **Noktadan siteye oturumlar**' ı seçin.

   :::image type="content" source="./media/p2s-session-management/portal.png" alt-text="Portal örneği":::
1. Tüm geçerli oturumları WindowPane içinde görüntüleyebilirsiniz.
1. Bağlantısını kesmek istediğiniz oturum için **"..."** seçeneğini belirleyin, ardından **bağlantıyı kes**' i seçin.

## <a name="powershell"></a>PowerShell

PowerShell kullanarak bir oturumu görüntülemek ve bağlantısını kesmek için:

1. Etkin oturumları listelemek için aşağıdaki PowerShell komutunu çalıştırın:

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayVpnClientConnectionHealth -VirtualNetworkGatewayName <name of the gateway>  -ResourceGroupName <name of the resource group>
   ```
1. Bağlantısını kesmek istediğiniz oturumun **Vpnconnectionıd** değerini kopyalayın.

   :::image type="content" source="./media/p2s-session-management/powershell.png" alt-text="PowerShell örneği":::
1. Oturumun bağlantısını kesmek için aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   Disconnect-AzVirtualNetworkGatewayVpnConnection -VirtualNetworkGatewayName <name of the gateway> -ResourceGroupName <name of the resource group> -VpnConnectionId <VpnConnectionId of the session>
   ```

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [noktadan sıteye VPN hakkında](point-to-site-about.md).
