---
title: 'Azure VPN Ağ Geçidi: P2S VPN istemcileri için özel rotaların reklamını yapın'
description: Nokta-to-site istemcilerinize özel yolların reklamını yapmak için adımlar
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/11/2019
ms.author: cherylmc
ms.openlocfilehash: 3588755e2aab1c84d443e917eca8c7fca280b49a
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756889"
---
# <a name="advertise-custom-routes-for-p2s-vpn-clients"></a>P2S VPN istemcileri için özel rotaların reklamını yapın

Tüm noktaya sayfa VPN istemcilerinize özel yolların reklamını yapmak isteyebilirsiniz. Örneğin, VNet'inizde depolama uç noktalarını etkinleştirdiğinizde ve uzak kullanıcıların bu depolama hesaplarına VPN bağlantısı üzerinden erişebilmesini istediğinizde. Depolama hesabının trafiğinin genel Internet'ten değil, VPN tünelinden gitmesi için depolama bitiş noktasının IP adresini tüm uzak kullanıcılarınıza tanıtabilirsiniz.

![Azure VPN Gateway Çok Siteli bağlantı örneği](./media/vpn-gateway-p2s-advertise-custom-routes/custom-routes.png)

## <a name="to-advertise-custom-routes"></a>Özel yolların reklamını yapmak için

Özel yolların reklamını yapmak `Set-AzVirtualNetworkGateway cmdlet`için, .' Aşağıdaki örnek, [Contoso depolama hesabı tabloları](https://contoso.table.core.windows.net)için IP'nin reklamını nasıl verebilirsiniz.

1. Ping *contoso.table.core.windows.net* ve IP adresini not edin. Örnek:

    ```cmd
    C:\>ping contoso.table.core.windows.net
    Pinging table.by4prdstr05a.store.core.windows.net [13.88.144.250] with 32 bytes of data:
    ```

2. Aşağıdaki PowerShell komutlarını çalıştırın:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of gateway> -ResourceGroupName <name of resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -CustomRoute 13.88.144.250/32
    ```

3. Birden çok özel yol eklemek için, adresleri ayırmak için virgül ve boşluk kullanın. Örnek:

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

Ek P2S yönlendirme bilgileri için, [sayfadan siteye yönlendirme hakkında](vpn-gateway-about-point-to-site-routing.md)bkz.
