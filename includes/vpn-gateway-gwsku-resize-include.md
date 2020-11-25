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
ms.openlocfilehash: a3ae2a876d6a3772d941fec0b8a1ea3f537e60c3
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96010859"
---
`Resize-AzVirtualNetworkGateway`PowerShell cmdlet 'ini kullanarak bir Generation1 veya Generation2 SKU 'su yükseltebilir veya indirgeyede (tüm VpnGw SKU 'Ları temel SKU 'lar dışında yeniden boyutlandırılabilir). Temel ağ geçidi SKU 'SU kullanıyorsanız, ağ geçidinizi yeniden boyutlandırmak [yerine bu yönergeleri kullanın](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) .

Aşağıdaki PowerShell örneği, VpnGw2 için yeniden boyutlandırılan bir ağ geçidi SKU 'SU gösterir.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```