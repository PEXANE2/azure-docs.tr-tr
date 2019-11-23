---
title: Tutorial - Create and manage a gateway using Azure VPN Gateway
description: Öğretici - Azure PowerShell modülü ile VPN ağ geçidi oluşturma ve yönetme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: b144a70ee88138966d9cc38a56e1cff1e63fca1b
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74424136"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Tutorial: Create and manage a VPN gateway using PowerShell

Azure VPN ağ geçitleri, müşterinin iş yeri ile Azure arasında konumlar arası bağlantı sağlar. Bu öğretici, bir VPN ağ geçidi oluşturma ve yönetme gibi temel Azure VPN ağ geçidi dağıtım öğelerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturun
> * View the public IP address
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

Aşağıdaki diyagramda, bu öğreticinin bir parçası olarak oluşturulan sanal ağ ve VPN ağ geçidi gösterilmiştir.

![Sanal Ağ ve VPN ağ geçidi](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell ve Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Ortak ağ parametre değerleri

Below are the parameter values used for this tutorial. In the examples, the variables translate to the following:

```
#$RG1         = The name of the resource group
#$VNet1       = The name of the virtual network
#$Location1   = The location region
#$FESubnet1   = The name of the first subnet
#$BESubnet1   = The name of the second subnet
#$VNet1Prefix = The address range for the virtual network
#$FEPrefix1   = Addresses for the first subnet
#$BEPrefix1   = Addresses for the second subnet
#$GwPrefix1   = Addresses for the GatewaySubnet
#$VNet1ASN    = ASN for the virtual network
#$DNS1        = The IP address of the DNS server you want to use for name resolution
#$Gw1         = The name of the virtual network gateway
#$GwIP1       = The public IP address for the virtual network gateway
#$GwIPConf1   = The name of the IP configuration
```

Change the values below based on your environment and network setup, then copy and paste to set the variables for this tutorial. If your Cloud Shell session times out, or you need to use a different PowerShell window, copy and paste the variables to your new session and continue the tutorial.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Create a resource group with the [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) command. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Önce bir kaynak grubu oluşturulmalıdır. Aşağıdaki örnekte, *Doğu ABD* bölgesinde *TestRG1* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturun

Azure VPN ağ geçidi, sanal ağınız için konumlar arası bağlantı ve P2S VPN sunucusu işlevselliği sağlar. VPN ağ geçidini mevcut bir sanal ağa ekleyin veya yeni bir sanal ağ ile ağ geçidi oluşturun. Notice that the example specifies the name of the gateway subnet specifically. You must always specify the name of the gateway subnet as "GatewaySubnet" in order for it to function properly. This example creates a new virtual network with three subnets: Frontend, Backend, and GatewaySubnet using [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) and [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -AddressPrefix $GwPrefix1
$vnet   = New-AzVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>VPN ağ geçidi için genel bir IP adresi isteme

Azure VPN ağ geçitleri, İnternet üzerinden şirket içi VPN cihazlarınızla iletişim kurarak IKE (İnternet Anahtar Değişimi) anlaşması gerçekleştirir ve IPSec tünelleri oluşturur. Create and assign a public IP address to your VPN gateway as shown in the example below with [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) and [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig):

> [!IMPORTANT]
> Şu anda ağ geçidi için yalnızca Dinamik bir genel IP adresi kullanabilirsiniz. Azure VPN ağ geçitlerinde Statik IP adresi desteklenmez.

```azurepowershell-interactive
$gwpip    = New-AzPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-a-vpn-gateway"></a>VPN ağ geçidi oluşturun

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında bağlantı oluşturabilirsiniz. Dilerseniz sanal ağınız ile şirket içindeki bir konum arasında da bir bağlantı oluşturabilirsiniz. Create a VPN gateway using the [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Anahtar parametre değerleri:
* GatewayType: Siteden siteye ve sanal ağdan sanal ağa bağlantılar için **Vpn** değerini kullanın
* VpnType: Daha geniş bir VPN cihazı yelpazesiyle etkileşim kurmak ve daha fazla yönlendirme özelliğinden yararlanmak için **RouteBased** seçeneğini kullanın
* GatewaySku: **VpnGw1** is the default; change it to another VpnGw SKU if you need higher throughputs or more connections. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

If you are using the TryIt, your session may time out. That's OK. The gateway will still create.

Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında ya da sanal ağınız ile şirket içi bir konum arasında bağlantı oluşturabilirsiniz. Ayrıca, bir istemci bilgisayardan sanal ağınıza yönelik bir P2S bağlantısı yapılandırabilirsiniz.

## <a name="view-the-gateway-public-ip-address"></a>View the gateway public IP address

If you know the name of the public IP address, use [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) to show the public IP address assigned to the gateway.

If your session timed out, copy the common network parameters from the beginning of this tutorial into your new session and proceed, then proceed.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Resize a gateway

Ağ geçidi oluşturulduktan sonra VPN ağ geçidi SKU’sunu değiştirebilirsiniz. Different gateway SKUs support different specifications such as throughputs, number of connections, etc. The following example uses [Resize-AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) to resize your gateway from VpnGw1 to VpnGw2. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Bir VPN ağ geçidinin yeniden boyutlandırılması da yaklaşık 30-45 dakika sürer, ancak bu işlem mevcut bağlantıları ve yapılandırmaları kesintiye **uğratmaz** veya kaldırmaz.

## <a name="reset-a-gateway"></a>Reset a gateway

Sorun giderme adımlarının bir parçası olarak VPN ağ geçidinizi IPsec/IKE tünel yapılandırmalarını yeniden başlatmaya zorlamak için VPN ağ geçidini sıfırlayabilirsiniz. Use [Reset-AzVirtualNetworkGateway](/powershell/module/az.network/Reset-azVirtualNetworkGateway) to reset your gateway.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Daha fazla bilgi için bkz. [Bir VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

If you're advancing to the [next tutorial](vpn-gateway-tutorial-vpnconnection-powershell.md), you will want to keep these resources because they are the prerequisites.

However, if the gateway is part of a prototype, test, or proof-of-concept deployment, you can use the [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) command to remove the resource group, the VPN gateway, and all related resources.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler gibi temel VPN ağ geçidi oluşturma ve yönetim görevlerini öğrendiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturun
> * View the public IP address
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

S2S, Sanal Ağdan Sanal Ağa ve P2S bağlantıları hakkında bilgi edinmek için sonraki öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> * [S2S bağlantıları oluşturma](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Sanal Ağdan Sanal Ağa bağlantılar oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S bağlantıları oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
