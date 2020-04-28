---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4842c55b2b1fd23f4d6b7996ccf02e7141504836
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73495761"
---
`Resize-AzVirtualNetworkGateway` PowerShell cmdlet 'ini kullanarak bir Generation1 veya Generation2 SKU 'su yükseltebilir veya indirgeyede (tüm Vpngw SKU 'Ları temel SKU 'lar dışında yeniden boyutlandırılabilir). Temel ağ geçidi SKU 'SU kullanıyorsanız, ağ geçidinizi yeniden boyutlandırmak [yerine bu yönergeleri kullanın](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) .

Aşağıdaki PowerShell örneği, VpnGw2 için yeniden boyutlandırılan bir ağ geçidi SKU 'SU gösterir.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Ayrıca, sanal ağ geçidinizin **yapılandırma** sayfasına gidip açılan listeden farklı bir SKU seçerek Azure Portal bir ağ geçidini yeniden boyutlandırabilirsiniz.
