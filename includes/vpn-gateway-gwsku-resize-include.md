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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73495761"
---
PowerShell cmdlet'i `Resize-AzVirtualNetworkGateway` Generation1 veya Generation2 SKU'yu yükseltmek veya düşürmek için kullanabilirsiniz (Temel SKU'lar hariç tüm VpnGw SKU'lar yeniden boyutlandırılabilir). Temel ağ geçidi SKU kullanıyorsanız, ağ geçidinizi yeniden boyutlandırmak için [bu yönergeleri kullanın.](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize)

Aşağıdaki PowerShell örneği, VpnGw2'ye yeniden boyutlandırılan bir ağ geçidi SKU'sunu gösterir.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Ayrıca, sanal ağ ağ ağ geçidiniz için **Yapılandırma** sayfasına giderek ve açılır geçitten farklı bir SKU seçerek Azure portalındaki bir ağ geçidini yeniden boyutlandırabilirsiniz.
