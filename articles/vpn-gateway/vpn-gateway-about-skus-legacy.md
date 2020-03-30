---
title: Eski Azure sanal ağ VPN ağ geçidi SK'lar
description: Eski sanal ağ ağ ağ geçidi SUS ile nasıl çalışılı; Temel, Standart ve Yüksek Performans.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/15/2019
ms.author: cherylmc
ms.openlocfilehash: 9c5e6d5aca51bd560a46837ba47de86362665773
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279396"
---
# <a name="working-with-virtual-network-gateway-skus-legacy-skus"></a>Sanal ağ ağ geçidi SUS 'lerle (eski SK'ler) çalışma

Bu makalede, eski (eski) sanal ağ ağ ağ Geçidi SUS hakkında bilgi içerir. Eski SNU'lar, zaten oluşturulmuş VPN ağ geçitleri için her iki dağıtım modelinde de çalışmaya devam etmektedir. Klasik VPN ağ geçitleri, hem varolan ağ geçitleri hem de yeni ağ geçitleri için eski STU'ları kullanmaya devam eder. Yeni Kaynak Yöneticisi VPN ağ geçitleri oluştururken, yeni ağ geçidi STU'larını kullanın. Yeni STU'lar hakkında bilgi için [VPN Ağ Geçidi hakkında](vpn-gateway-about-vpngateways.md)bilgi alalım.

## <a name="gateway-skus"></a><a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [Legacy gateway SKUs](../../includes/vpn-gateway-gwsku-legacy-include.md)]

ExpressRoute fiyatlandırma sayfasında bulunan **Sanal Ağ Ağ Ağ Geçitleri** bölümünde eski ağ geçidi [fiyatlandırmasını](https://azure.microsoft.com/pricing/details/expressroute)görüntüleyebilirsiniz.

## <a name="estimated-aggregate-throughput-by-sku"></a><a name="agg"></a>SKU'ya göre tahmini toplam verimlilik

[!INCLUDE [Aggregated throughput by legacy SKU](../../includes/vpn-gateway-table-gwtype-legacy-aggtput-include.md)]

## <a name="supported-configurations-by-sku-and-vpn-type"></a><a name="config"></a>SKU ve VPN türüne göre desteklenen yapılandırmalar

[!INCLUDE [Table requirements for old SKUs](../../includes/vpn-gateway-table-requirements-legacy-sku-include.md)]

## <a name="resize-a-gateway"></a><a name="resize"></a>Ağ geçidini yeniden boyutlandırma

Aynı SKU ailesi içinde bir ağ geçidi SKU için ağ geçidi yeniden boyutlandırabilirsiniz. Örneğin, standart SKU'nuz varsa, Yüksek Performanslı SKU olarak yeniden boyutlandırabilirsiniz. Ancak, vpn ağ geçidinizi eski SKU'lar ve yeni SKU aileleri arasında yeniden boyutlandıramaz. Örneğin, Standart SKU'dan VpnGw2 SKU'ya veya Temel SKU'dan VpnGw1'e gidemezsiniz.

### <a name="resource-manager"></a>Resource Manager

PowerShell kullanarak Kaynak Yöneticisi dağıtım modeli için bir ağ geçidini yeniden boyutlandırmak için aşağıdaki komutu kullanın:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

Azure portalında bir ağ geçidini yeniden boyutlandırabilirsiniz.

### <a name="classic"></a><a name="classicresize"></a>Klasik

Klasik dağıtım modeli için bir ağ geçidini yeniden boyutlandırmak için Service Management PowerShell cmdlets'i kullanmanız gerekir. Aşağıdaki komutu kullanın:

```powershell
Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="change-to-the-new-gateway-skus"></a><a name="change"></a>Yeni ağ geçidi STU'larına geçiş

[!INCLUDE [Change to the new SKUs](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni Ağ Geçidi SK'leri hakkında daha fazla bilgi için [Ağ Geçidi STU'larına](vpn-gateway-about-vpngateways.md#gwsku)bakın.

Yapılandırma ayarları hakkında daha fazla bilgi için [VPN Ağ Geçidi yapılandırma ayarları hakkında](vpn-gateway-about-vpn-gateway-settings.md)bilgi alabiliyorum.
