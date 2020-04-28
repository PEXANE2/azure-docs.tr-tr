---
title: 'Azure VPN Gateway: P2S VPN istemcileri için özel yollar tanıtma'
description: Özel yolları Noktadan siteye istemcilerinize tanıtma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/11/2019
ms.author: cherylmc
ms.openlocfilehash: 3588755e2aab1c84d443e917eca8c7fca280b49a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756889"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>P2S VPN istemcileri için özel rotalar bildirme

Tüm Noktadan siteye VPN istemcilerinize özel yollar tanıtmak isteyebilirsiniz. Örneğin, VNet 'iniz üzerinde depolama uç noktalarını etkinleştirdiyseniz ve uzak kullanıcıların bu depolama hesaplarına VPN bağlantısı üzerinden erişmesini istiyorsanız. Depolama hesabı trafiğinin, genel Internet değil VPN tünelinin üzerinden aktarılması için, depolama uç noktasının IP adresini tüm uzak kullanıcılarınıza tanıtabilirsiniz.

![Azure VPN Gateway Çok Siteli bağlantı örneği](./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png)

## <a name="to-advertise-custom-routes"></a>Özel yolları tanıtmak için

Özel yolları tanıtmak için öğesini kullanın `Set-AzVirtualNetworkGateway cmdlet`. Aşağıdaki örnekte, [contoso depolama hesabı tabloları](https://contoso.table.core.windows.net)için IP 'nin nasıl tanıtılyapılacağı gösterilmektedir.

1. *Contoso.Table.Core.Windows.net* ping YAPıN ve IP adresini aklınızda edin. Örneğin:

    ```cmd
    C:\>ping contoso.table.core.windows.net
    Pinging table.by4prdstr05a.store.core.windows.net [13.88.144.250] with 32 bytes of data:
    ```

2. Aşağıdaki PowerShell komutlarını çalıştırın:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 13.88.144.250/32
    ```

3. Birden çok özel yol eklemek için, adresleri ayırmak için virgül ve boşluk kullanın. Örneğin:

    ```azurepowershell-interactive
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute x.x.x.x/xx , y.y.y.y/yy
    ```
## <a name="to-view-custom-routes"></a>Özel yolları görüntülemek için

Özel yolları görüntülemek için aşağıdaki örneği kullanın:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  $gw.CustomRoutes | Format-List
  ```
## <a name="to-delete-custom-routes"></a>Özel yolları silmek için

Özel yolları silmek için aşağıdaki örneği kullanın:

  ```azurepowershell-interactive
  $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
  Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute @0
  ```
## <a name="next-steps"></a>Sonraki adımlar

Ek P2S yönlendirme bilgileri için bkz. [Noktadan siteye yönlendirme hakkında](vpn-gateway-about-point-to-site-routing.md).
