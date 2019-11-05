---
title: Azure VPN Gateway bağlantıları için özel trafik seçicileri | Microsoft Docs
description: VPN Gateway bağlantılarında özel trafik seçicileri kullanma hakkında bilgi edinin
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: radwiv
ms.openlocfilehash: cf9401b21d4aa736a6edeae5146d1355f2d49d1d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73512164"
---
# <a name="custom-traffic-selectors-for-vpn-gateway-connections"></a>VPN Gateway bağlantıları için özel trafik seçicileri

VPN Gateway bağlantılarınız için özel trafik seçicileri ayarlayabilirsiniz. Trafik seçicileri ayarlama özelliği, trafiği göndermek istediğiniz VPN tünellerinin her iki tarafınızdan adres alanlarını daraltmanıza olanak tanır. Özel trafik seçicileri özellikle büyük bir VNet adres alanınız olduğunda yararlıdır, ancak alt ağlardan birini IPSec/ıKE anlaşması için kullanmak istersiniz.

Özel trafik seçicileri yeni bir bağlantı oluşturulurken eklenebilir veya mevcut bir bağlantı için güncelleştirilemeyebilir. `TrafficSelectorPolicies` parametresi bir trafik seçicileri dizisinden oluşur. Her trafik seçici, CıDR biçiminde yerel ve uzak adres aralıklarının bir koleksiyonunu barındırır.

## <a name="add-custom-traffic-selectors"></a>Özel trafik seçicileri ekleme

Aşağıdaki örneklerde, PowerShell komutları kullanılarak sanal ağ geçidi bağlantısı için özel trafik seçicileri oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir. Sanal ağ geçidi bağlantısı oluşturma hakkında yardım için bkz. [siteden siteye bağlantı yapılandırma](vpn-gateway-create-site-to-site-rm-powershell.md).

1. $Trafficselectorpolicy *trafficselectorpolicies* parametresinin değerlerini ayarlayın. Bu ayarı yaparken *New-AzTrafficSelectorPolicy* değerinin bir dizide yerel ve uzak adres aralıkları aldığını unutmayın.

   ```azurepowershell-interactive
   $trafficSelectorPolicy = New-AzTrafficSelectorPolicy `
   -LocalAddressRanges ("10.10.10.0/24", "20.20.20.0/24") `
   -RemoteAddressRanges ("30.30.30.0/24", "40.40.40.0/24")
   ```
2. Yeni sanal ağ geçidi bağlantısı oluşturun. Gereksinimlerinize göre değer parametrelerini ayarlayın.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection `
   -ResourceGroupName $rgname `
   -name $vnetConnectionName `
   -location $location `
   -VirtualNetworkGateway1 $vnetGateway `
   -LocalNetworkGateway2 $localnetGateway `
   -ConnectionType IPsec `
   -RoutingWeight 3 `
   -SharedKey $sharedKey `
   -UsePolicyBasedTrafficSelectors $true `
   -TrafficSelectorPolicies ($trafficSelectorPolicy)
   ```

Ayrıca, ' set-AzVirtualNetworkGatewayConnection ' kullanarak var olan bir sanal ağ geçidi bağlantısı için özel trafik seçicileri güncelleştirebilirsiniz. PowerShell değerleri için bkz. [sanal ağ geçidi bağlantısı](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworkgatewayconnection?).

## <a name="next-steps"></a>Sonraki adımlar

VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN Gateway](vpn-gateway-about-vpngateways.md).