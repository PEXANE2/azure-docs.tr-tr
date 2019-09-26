---
title: Noktadan siteye VPN istemcileri için özel yollar tanıtma | Azure | Microsoft Docs
description: Özel yolları Noktadan siteye istemcilerinize tanıtma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/25/2019
ms.author: cherylmc
ms.openlocfilehash: 18a0effcf5157ec2797707db78f9614ef12a4669
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310249"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>P2S VPN istemcileri için özel rotalar bildirme

Tüm Noktadan siteye VPN istemcilerinize özel yollar tanıtmak isteyebilirsiniz. Örneğin, VNet 'iniz üzerinde depolama uç noktalarını etkinleştirdiyseniz ve uzak kullanıcıların bu depolama hesaplarına VPN bağlantısı üzerinden erişmesini istiyorsanız. Depolama hesabı trafiğinin, genel Internet değil VPN tünelinin üzerinden aktarılması için, depolama uç noktasının IP adresini tüm uzak kullanıcılarınıza tanıtabilirsiniz.

## <a name="to-advertise-custom-routes"></a>Özel yolları tanıtmak için

Özel yolları tanıtmak için öğesini kullanın `Set-AzVirtualNetworkGateway cmdlet`. Aşağıdaki örnekte, [Conotoso depolama hesabı tabloları](https://contoso.table.core.windows.net)için IP 'nin nasıl tanıtılyapılacağı gösterilmektedir.

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

3. Birden çok özel yol eklemek için, adresleri ayırmak üzere bir Coma ve boşluklar kullanın. Örneğin:

    ```azurepowershell-interactive
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute x.x.x.x/xx , y.y.y.y/yy
    ```
## <a name="to-view-custom-routes"></a>Özel yolları görüntülemek için

Özel yolları görüntülemek için aşağıdaki örneği kullanın:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    $gw.CustomRoutes | Format-List
    ```

## <a name="next-steps"></a>Sonraki adımlar

Ek P2S yönlendirme bilgileri için bkz. [Noktadan siteye yönlendirme hakkında](vpn-gateway-about-point-to-site-routing.md).
