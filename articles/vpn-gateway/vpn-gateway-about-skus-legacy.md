---
title: Eski Azure sanal ağ VPN Gateway SKU 'Ları
description: Eski sanal ağ geçidi SKU 'Ları ile çalışma Temel, standart ve HighPerformance.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/15/2019
ms.author: cherylmc
ms.openlocfilehash: 9c5e6d5aca51bd560a46837ba47de86362665773
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78379222"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Sanal ağ geçidi SKU 'Ları (eski SKU 'Lar) ile çalışma

Bu makale, eski (eski) sanal ağ geçidi SKU 'Ları hakkında bilgi içerir. Eski SKU 'Lar, zaten oluşturulmuş olan VPN ağ geçitlerinin her iki dağıtım modelinde de çalışır. Klasik VPN ağ geçitleri, hem mevcut ağ geçitleri hem de yeni ağ geçitleri için eski SKU 'Ları kullanmaya devam eder. Yeni Kaynak Yöneticisi VPN ağ geçitleri oluştururken, yeni ağ geçidi SKU 'Larını kullanın. Yeni SKU 'Lar hakkında bilgi için bkz. [VPN Gateway](vpn-gateway-about-vpngateways.md).

## <a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

Eski ağ geçidi fiyatlandırmasını, [ExpressRoute fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/expressroute)bulunan **sanal ağ geçitleri** bölümünde görüntüleyebilirsiniz.

## <a name="agg"></a>SKU 'ya göre tahmini toplam verimlilik

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="config"></a>SKU ve VPN türüne göre desteklenen konfigürasyonlar

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize"></a>Ağ geçidini yeniden boyutlandırma

Ağ geçidinizi aynı SKU ailesi içindeki bir ağ geçidi SKU 'SU ile yeniden boyutlandırabilirsiniz. Örneğin, standart bir SKU 'SU varsa, HighPerformance SKU 'suna yeniden boyutlandırabilirsiniz. Ancak, eski SKU 'Lar ve yeni SKU aileleri arasında VPN ağ geçidinizi yeniden boyutlandıramazsınız. Örneğin, standart bir SKU 'dan bir VpnGw2 SKU 'suna veya temel SKU 'yu VpnGw1 'e gidemezsiniz.

### <a name="resource-manager"></a>Resource Manager

PowerShell kullanarak Kaynak Yöneticisi dağıtım modeli için bir ağ geçidini yeniden boyutlandırmak üzere aşağıdaki komutu kullanın:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

Ayrıca Azure portal bir ağ geçidini yeniden boyutlandırabilirsiniz.

### <a name="classicresize"></a>Klasik

Klasik dağıtım modeli için bir ağ geçidini yeniden boyutlandırmak üzere hizmet yönetimi PowerShell cmdlet 'lerini kullanmanız gerekir. Aşağıdaki komutu kullanın:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="change"></a>Yeni ağ geçidi SKU 'Larına değiştirme

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni ağ geçidi SKU 'Ları hakkında daha fazla bilgi için bkz. [ağ geçidi SKU 'ları](vpn-gateway-about-vpngateways.md#gwsku).

Yapılandırma ayarları hakkında daha fazla bilgi için bkz. [VPN Gateway yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md).
