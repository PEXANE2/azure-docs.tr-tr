---
title: Azure Kullanılabilirlik Alanları bölgede yedekli bir sanal ağ geçidi oluşturma
description: Azure Kullanılabilirlik Alanları 'de VPN ve ExpressRoute ağ geçitlerini dağıtmayı, VNet ağ geçitlerine dayanıklılık, ölçeklenebilirlik ve daha yüksek kullanılabilirlik eklemeyi öğrenin.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/10/2020
ms.author: cherylmc
ms.openlocfilehash: ccb6050ae4d56d2f8e57b4a590d01fb6acd9bd8a
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87925166"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Azure Kullanılabilirlik Alanları bölgede yedekli bir sanal ağ geçidi oluşturma

VPN ve ExpressRoute ağ geçitlerini Azure Kullanılabilirlik Alanları dağıtabilirsiniz. Bu seçenek, sanal ağ geçitlerine dayanıklılık, ölçeklenebilirlik ve daha yüksek kullanılabilirlik getirir. Ağ geçitlerini Azure Kullanılabilirlik Alanları içinde dağıtmak, bir bölge içindeki ağ geçitlerini fiziksel ve mantıksal olarak birbirinden ayırırken, Azure ile şirket içi ağ bağlantınızı alan düzeyindeki hatalardan korur. Daha fazla bilgi için bkz. bölgesel olarak [yedekli sanal ağ geçitleri](about-zone-redundant-vnet-gateways.md) ve [Azure kullanılabilirlik alanları hakkında](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="1-declare-your-variables"></a><a name="variables"></a>1. değişkenlerinizi bildirin

Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin. Alıştırma sırasında herhangi bir noktada PowerShell/Cloud Shell oturumunuzu kapatırsanız, değişkenleri yeniden bildirmek için değerleri kopyalamanız ve yeniden yapıştırmanız yeterlidir. Konum belirtirken, belirttiğiniz bölgenin desteklendiğini doğrulayın. Daha fazla bilgi için bkz. [SSS](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="2-create-the-virtual-network"></a><a name="configure"></a>2. sanal ağı oluşturma

Bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="3-add-the-gateway-subnet"></a><a name="gwsub"></a>3. ağ geçidi alt ağını ekleme

Ağ geçidi alt ağı, sanal ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Bir ağ geçidi alt ağı eklemek ve ayarlamak için aşağıdaki örnekleri kullanın:

Ağ geçidi alt ağını ekleyin.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Sanal ağ için ağ geçidi alt ağ yapılandırmasını ayarlayın.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="4-request-a-public-ip-address"></a><a name="publicip"></a>4. genel IP adresi isteyin
 
Bu adımda, oluşturmak istediğiniz ağ geçidine uygulanan yönergeleri seçin. Ağ geçitlerini dağıtmaya yönelik bölgelerin seçimi, genel IP adresi için belirtilen bölgelere bağlıdır.

### <a name="for-zone-redundant-gateways"></a><a name="ipzoneredundant"></a>Bölgesel olarak yedekli ağ geçitleri için

**Standart** bir PUBLICıPADDRESS SKU 'su olan genel bir IP adresi isteyin ve herhangi bir bölge belirtmeyin. Bu durumda, oluşturulan Standart genel IP adresi bölgesel olarak yedekli bir genel IP olacaktır.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="for-zonal-gateways"></a><a name="ipzonalgw"></a>Bölgesel ağ geçitleri için

**Standart** bir PUBLICıPADDRESS SKU 'su ile genel bir IP adresi isteyin. Bölgeyi belirtin (1, 2 veya 3). Tüm ağ geçidi örnekleri, bu bölgeye dağıtılacak.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="for-regional-gateways"></a><a name="ipregionalgw"></a>Bölgesel ağ geçitleri için

**Temel** bir PUBLICıPADDRESS SKU 'su ile genel bir IP adresi isteyin. Bu durumda, ağ geçidi bölgesel ağ geçidi olarak dağıtılır ve ağ geçidine yerleştirilmiş bölge yedekliliği yoktur. Ağ Geçidi örnekleri, sırasıyla herhangi bir bölgede oluşturulur.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="5-create-the-ip-configuration"></a><a name="gwipconfig"></a>5. IP yapılandırmasını oluşturma

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="6-create-the-gateway"></a><a name="gwconfig"></a>6. ağ geçidini oluşturun

Sanal ağ geçidini oluşturun.

### <a name="for-expressroute"></a>ExpressRoute için

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute -GatewaySku ErGw1AZ
```

### <a name="for-vpn-gateway"></a>VPN Gateway için

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1AZ
```

## <a name="faq"></a><a name="faq"></a>SSS

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Bu yeni SKU 'Ları dağıtırken ne değişecektir?

Perspektifinden, ağ geçitlerini bölge yedekliliği ile dağıtabilirsiniz. Bu, ağ geçitlerinin tüm örneklerinin Azure Kullanılabilirlik Alanları üzerinden dağıtılacağı ve her kullanılabilirlik bölgesinin farklı bir hata ve güncelleştirme etki alanı olması anlamına gelir. Bu, ağ geçitlerinizin bölge hatalarıyla daha güvenilir, kullanılabilir ve dayanıklı olmasını sağlar.

### <a name="can-i-use-the-azure-portal"></a>Azure portal kullanabilir miyim?

Evet, yeni SKU 'Ları dağıtmak için Azure portal kullanabilirsiniz. Ancak, bu yeni SKU 'Ları yalnızca Azure Kullanılabilirlik Alanları olan Azure bölgelerinde görürsünüz.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Yeni SKU 'Ları kullanamadığı için hangi bölgeler kullanılabilir?

Kullanılabilir bölgelerin en son listesi için bkz. [kullanılabilirlik alanları](../availability-zones/az-region.md) .

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Var olan sanal ağ geçitlerini, bölgesel olarak yedekli veya bölgesel ağ geçitlerine değiştirebilir/geçirebilir/yükseltebilir miyim?

Mevcut sanal ağ geçitlerinizi bölgesel olarak yedekli veya bölgesel ağ geçitlerine geçirme işlemi şu anda desteklenmiyor. Bununla birlikte, mevcut ağ geçidinizi silebilir ve bölgesel olarak yedekli veya bölgesel ağ geçidini yeniden oluşturabilirsiniz.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Aynı sanal ağda hem VPN hem de hızlı rota ağ geçitlerini dağıtabilir miyim?

Aynı sanal ağ üzerinde hem VPN hem de hızlı rota ağ geçitlerinin birlikte bulunması desteklenir. Ancak, ağ geçidi alt ağı için bir/27 IP adresi aralığı ayırmanız gerekir.
