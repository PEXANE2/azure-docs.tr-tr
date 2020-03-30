---
title: Azure Kullanılabilirlik Bölgelerinde bölge gereksiz sanal ağ ağ geçidi oluşturma
description: Kullanılabilirlik Bölgelerinde VPN Ağ Geçidi ve ExpressRoute ağ geçitlerini dağıtma
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/10/2020
ms.author: cherylmc
ms.openlocfilehash: d8c6b68a38d4b60cf7a3194e6a5ded8804cc416f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77150221"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Azure Kullanılabilirlik Bölgelerinde bölge gereksiz sanal ağ ağ geçidi oluşturma

Azure Kullanılabilirlik Bölgelerinde VPN ve ExpressRoute ağ geçitlerini dağıtabilirsiniz. Bu seçenek, sanal ağ geçitlerine dayanıklılık, ölçeklenebilirlik ve daha yüksek kullanılabilirlik getirir. Ağ geçitlerini Azure Kullanılabilirlik Alanları içinde dağıtmak, bir bölge içindeki ağ geçitlerini fiziksel ve mantıksal olarak birbirinden ayırırken, Azure ile şirket içi ağ bağlantınızı alan düzeyindeki hatalardan korur. Daha fazla bilgi için, [bölge yedekli sanal ağ ağ geçitleri](about-zone-redundant-vnet-gateways.md) ve [Azure Kullanılabilirlik Bölgeleri Hakkında'ya](../availability-zones/az-overview.md)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="1-declare-your-variables"></a><a name="variables"></a>1. Değişkenlerinizi bildirin

Kullanmak istediğiniz değişkenleri bildirin. Aşağıdaki örneği kullanın ve gerektiğinde, değerleri kendi değerlerinizle değiştirin. Egzersiz sırasında PowerShell/Cloud Shell oturumunuzu herhangi bir noktada kapatırsanız, değişkenleri yeniden bildirmek için değerleri kopyalayıp yapıştırmanız gerekir. Konum belirtirken, belirttiğiniz bölgenin desteklendiğinden doğrulayın. Daha fazla bilgi için [SSS](#faq)bölümüne bakın.

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

## <a name="2-create-the-virtual-network"></a><a name="configure"></a>2. Sanal ağ oluşturma

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

## <a name="3-add-the-gateway-subnet"></a><a name="gwsub"></a>3. Ağ geçidi alt ağı ekle

Ağ geçidi alt ağı, sanal ağ ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ağ geçidi alt ağı eklemek ve ayarlamak için aşağıdaki örnekleri kullanın:

Ağ geçidi alt ağını ekleyin.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Sanal ağ için ağ geçidi alt ağ yapılandırmasını ayarlayın.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="4-request-a-public-ip-address"></a><a name="publicip"></a>4. Genel bir IP adresi isteyin
 
Bu adımda, oluşturmak istediğiniz ağ geçidine uygulanan yönergeleri seçin. Ağ geçitlerini dağıtmak için bölgelerin seçimi, ortak IP adresi için belirtilen bölgelere bağlıdır.

### <a name="for-zone-redundant-gateways"></a><a name="ipzoneredundant"></a>Bölge yedekli ağ geçitleri için

**Standart** PublicIpaddress SKU içeren genel bir IP adresi isteyin ve herhangi bir bölge belirtmeyin. Bu durumda, oluşturulan Standart genel IP adresi, bölge gereksiz bir genel IP olacaktır.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="for-zonal-gateways"></a><a name="ipzonalgw"></a>Zonal ağ geçitleri için

**Standart** PublicIpaddress SKU içeren genel bir IP adresi isteyin. Bölgeyi belirtin (1, 2 veya 3). Tüm ağ geçidi örnekleri bu bölgede dağıtılacaktır.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="for-regional-gateways"></a><a name="ipregionalgw"></a>Bölgesel ağ geçitleri için

**Temel** PublicIpaddress SKU ile ortak bir IP adresi isteyin. Bu durumda, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik herhangi bir bölge artıklığı yoktur. Ağ geçidi örnekleri sırasıyla herhangi bir bölgede oluşturulur.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="5-create-the-ip-configuration"></a><a name="gwipconfig"></a>5. IP yapılandırmasını oluşturun

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="6-create-the-gateway"></a><a name="gwconfig"></a>6. Ağ geçidi oluşturma

Sanal ağ ağ ağ geçidini oluşturun.

### <a name="for-expressroute"></a>ExpressRoute için

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute -GatewaySku ErGw1AZ
```

### <a name="for-vpn-gateway"></a>VPN Ağ Geçidi için

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1AZ
```

## <a name="faq"></a><a name="faq"></a>SSS

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Bu yeni STU'ları dağıtdığımda ne değişecek?

Sizin bakış açınızdan, ağ geçitlerinizi bölge artıklığıyla dağıtabilirsiniz. Bu, ağ geçitlerinin tüm örneklerinin Azure Kullanılabilirlik Bölgeleri arasında dağıtılacayaçalışacağı ve her Kullanılabilirlik Bölgesinin farklı bir hata ve güncelleştirme etki alanı olduğu anlamına gelir. Bu, ağ geçitlerinizi bölge hatalarına karşı daha güvenilir, kullanılabilir ve esnek hale getirir.

### <a name="can-i-use-the-azure-portal"></a>Azure portalını kullanabilir miyim?

Evet, yeni SNU'ları dağıtmak için Azure portalını kullanabilirsiniz. Ancak, bu yeni SK'leri yalnızca Azure Kullanılabilirlik Bölgeleri olan Azure bölgelerinde görürsünüz.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Yeni SNU'ları kullanabilmek için hangi bölgeler kullanılabilir?

Kullanılabilir bölgelerin en son listesi için [Kullanılabilirlik Bölgeleri'ne](../availability-zones/az-overview.md#services-support-by-region) bakın.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Can I change/migrate/upgrade my existing virtual network gateways to zone-redundant or zonal gateways?

Varolan sanal ağ ağağlarınızı bölge gereksiz veya bölge ağ geçitlerine geçirmek şu anda desteklenmez. Ancak, varolan ağ geçidinizi silebilir ve bölge gereksiz veya bölgesel ağ geçidini yeniden oluşturabilirsiniz.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Vpn ve Express Route ağ geçitlerini aynı sanal ağda dağıtabilir miyim?

Aynı sanal ağda hem VPN hem de Express Route ağ geçitlerinin birlikte liği desteklenir. Ancak, ağ geçidi alt ağı için bir /27 IP adresi aralığı rezerve etmelisiniz.
