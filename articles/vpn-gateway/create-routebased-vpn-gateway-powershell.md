---
title: 'Azure VPN Ağ Geçidi: Rota tabanlı ağ geçidi oluşturma: PowerShell'
description: PowerShell'i kullanarak rota tabanlı VPN Ağ Geçidi'ni hızla oluşturun
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 02/10/2020
ms.author: cherylmc
ms.openlocfilehash: 8a4bb9d2ac7b8124fa9b1e00f3ecceda4f4a4cdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152967"
---
# <a name="create-a-route-based-vpn-gateway-using-powershell"></a>PowerShell'i kullanarak rota tabanlı VPN ağ geçidi oluşturma

Bu makale, PowerShell'i kullanarak rota tabanlı bir Azure VPN ağ geçidi oluşturmanıza yardımcı olur. Şirket içi ağınıza VPN bağlantısı oluşturulurken VPN ağ geçidi kullanılır. VNets'i bağlamak için vpn ağ geçidi de kullanabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımlar bir VNet, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ ağ geçidi) oluşturur. Ağ geçidi oluşturma tamamlandıktan sonra, bağlantılar oluşturabilirsiniz. Bu adımlar için Azure aboneliği gerekir. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu oluşturun. PowerShell'i yerel olarak çalıştırıyorsanız, PowerShell konsolunuzu yüksek ayrıcalıklarla `Connect-AzAccount` açın ve komutu kullanarak Azure'a bağlanın.

```azurepowershell-interactive
New-AzResourceGroup -Name TestRG1 -Location EastUS
```

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek, **EastUS** konumunda **VNet1** adlı bir sanal ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName TestRG1 `
  -Location EastUS `
  -Name VNet1 `
  -AddressPrefix 10.1.0.0/16
```

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) cmdlet kullanarak bir alt net yapılandırması oluşturun.

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Frontend `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

[Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) cmdlet'i kullanarak sanal ağ için alt ağ yapılandırmasını ayarlayın.


```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="add-a-gateway-subnet"></a><a name="gwsubnet"></a>Ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı, sanal ağ ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ağ geçidi alt ağı eklemek için aşağıdaki örnekleri kullanın:

VNet'iniz için bir değişken ayarlayın.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork -ResourceGroupName TestRG1 -Name VNet1
```

[Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-azVirtualNetworkSubnetConfig) cmdlet kullanarak ağ geçidi alt netoluşturun.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $vnet
```

[Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork) cmdlet'i kullanarak sanal ağ için alt ağ yapılandırmasını ayarlayın.

```azurepowershell-interactive
$vnet | Set-AzVirtualNetwork
```

## <a name="request-a-public-ip-address"></a><a name="PublicIP"></a>Herkese açık bir IP adresi isteme

VPN ağ geçidinin dinamik olarak ayrılmış genel BIR IP adresi olmalıdır. VPN ağ geçidine bağlantı oluşturduğunuzda, bu belirttiğiniz IP adresidir. Genel bir IP adresi istemek için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$gwpip= New-AzPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1 -Location 'East US' -AllocationMethod Dynamic
```

## <a name="create-the-gateway-ip-address-configuration"></a><a name="GatewayIPConfig"></a>Ağ geçidi IP adresi yapılandırmasını oluşturma

Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork -Name VNet1 -ResourceGroupName TestRG1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```
## <a name="create-the-vpn-gateway"></a><a name="CreateGateway"></a>VPN ağ geçidini oluşturma

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi tamamlandıktan sonra, sanal ağınızla başka bir VNet arasında bağlantı oluşturabilirsiniz. Veya sanal ağınızla şirket içi konumunuz arasında bir bağlantı kurun. [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet'i kullanarak bir VPN ağ geçidi oluşturun.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'East US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku VpnGw1
```

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>VPN ağ geçidini görüntüleme

Vpn ağ geçidini [Get-AzVirtualNetworkGateway](/powershell/module/az.network/Get-azVirtualNetworkGateway) cmdlet'i kullanarak görüntüleyebilirsiniz.

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -Name Vnet1GW -ResourceGroup TestRG1
```

Çıktı bu örneğe benzer:

```
Name                   : VNet1GW
ResourceGroupName      : TestRG1
Location               : eastus
Id                     : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provide
                         rs/Microsoft.Network/virtualNetworkGateways/VNet1GW
Etag                   : W/"0952d-9da8-4d7d-a8ed-28c8ca0413"
ResourceGuid           : dc6ce1de-2c4494-9d0b-20b03ac595
ProvisioningState      : Succeeded
Tags                   :
IpConfigurations       : [
                           {
                             "PrivateIpAllocationMethod": "Dynamic",
                             "Subnet": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet"
                             },
                             "PublicIpAddress": {
                               "Id": "/subscriptions/<subscription ID>/resourceGroups/Te
                         stRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP"
                             },
                             "Name": "default",
                             "Etag": "W/\"0952d-9da8-4d7d-a8ed-28c8ca0413\"",
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                         RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/de
                         fault"
                           }
                         ]
GatewayType            : Vpn
VpnType                : RouteBased
EnableBgp              : False
ActiveActive           : False
GatewayDefaultSite     : null
Sku                    : {
                           "Capacity": 2,
                           "Name": "VpnGw1",
                           "Tier": "VpnGw1"
                         }
VpnClientConfiguration : null
BgpSettings            : {
     
```

## <a name="view-the-public-ip-address"></a><a name="viewgwpip"></a>Genel IP adresini görüntüleme

VPN ağ geçidinizin genel IP adresini görüntülemek için [Get-AzPublicIpAddress](/powershell/module/az.network/Get-azPublicIpAddress) cmdlet'i kullanın.

```azurepowershell-interactive
Get-AzPublicIpAddress -Name VNet1GWIP -ResourceGroupName TestRG1
```

Örnek yanıtta, IpAddress değeri ortak IP adresidir.

```
Name                     : VNet1GWIP
ResourceGroupName        : TestRG1
Location                 : eastus
Id                       : /subscriptions/<subscription ID>/resourceGroups/TestRG1/provi
                           ders/Microsoft.Network/publicIPAddresses/VNet1GWIP
Etag                     : W/"5001666a-bc2a-484b-bcf5-ad488dabd8ca"
ResourceGuid             : 3c7c481e-9828-4dae-abdc-f95b383
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Dynamic
IpAddress                : 13.90.153.3
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                             "Id": "/subscriptions/<subscription ID>/resourceGroups/Test
                           RG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/
                           default"
                           }
DnsSettings              : null
Zones                    : {}
Sku                      : {
                             "Name": "Basic"
                           }
IpTags                   : {}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu silmek için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanın. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name TestRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma yı bitirdikten sonra, sanal ağınızla başka bir VNet arasında bağlantı oluşturabilirsiniz. Veya sanal ağınızla şirket içi konumunuz arasında bir bağlantı kurun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir VNet'e bağlantı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md)
