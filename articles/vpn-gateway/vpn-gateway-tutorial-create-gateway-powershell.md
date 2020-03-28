---
title: Öğretici - Azure VPN Ağ Geçidi'ni kullanarak bir ağ geçidi oluşturma ve yönetme
description: Öğretici - Azure PowerShell modülü ile VPN ağ geçidi oluşturma ve yönetme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 03/11/2020
ms.author: cherylmc
ms.openlocfilehash: 66efa0f2922e70908616c7c447d782efee8f6b1b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79137186"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Öğretici: PowerShell'i kullanarak bir VPN ağ geçidi oluşturun ve yönetin

Azure VPN ağ geçitleri, müşterinin iş yeri ile Azure arasında konumlar arası bağlantı sağlar. Bu öğretici, bir VPN ağ geçidi oluşturma ve yönetme gibi temel Azure VPN ağ geçidi dağıtım öğelerini kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Genel IP adresini görüntüleme
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

Aşağıdaki diyagramda, bu öğreticinin bir parçası olarak oluşturulan sanal ağ ve VPN ağ geçidi gösterilmiştir.

![Sanal Ağ ve VPN ağ geçidi](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="working-with-azure-cloud-shell-and-azure-powershell"></a>Azure Bulut Shell ve Azure PowerShell ile çalışma

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Ortak ağ parametre değerleri

Aşağıda bu öğretici için kullanılan parametre değerleri verilmiştir. Örneklerde, değişkenler aşağıdakilere çevirir:

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

Ortamınıza ve ağ kurulumunuza bağlı olarak aşağıdaki değerleri değiştirin, ardından bu öğreticinin değişkenlerini ayarlamak için kopyalayıp yapıştırın. Cloud Shell oturumunuz zaman zaman doluyorsa veya farklı bir PowerShell penceresi kullanmanız gerekiyorsa, değişkenleri yeni oturumunuza kopyalayıp yapıştırın ve öğreticiye devam edin.

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

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Önce bir kaynak grubu oluşturulmalıdır. Aşağıdaki örnekte, *Doğu ABD* bölgesinde *TestRG1* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Azure VPN ağ geçidi, sanal ağınız için konumlar arası bağlantı ve P2S VPN sunucusu işlevselliği sağlar. VPN ağ geçidini mevcut bir sanal ağa ekleyin veya yeni bir sanal ağ ile ağ geçidi oluşturun. Örneğin ağ geçidi alt ağının adını özellikle belirttiğine dikkat edin. Düzgün çalışması için ağ geçidi alt netinin adını her zaman "GatewaySubnet" olarak belirtmeniz gerekir. Bu örnek üç alt ağlar ile yeni bir sanal ağ oluşturur: Frontend, Backend ve GatewaySubnet [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) ve [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)kullanarak:

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

Azure VPN ağ geçitleri, İnternet üzerinden şirket içi VPN cihazlarınızla iletişim kurarak IKE (İnternet Anahtar Değişimi) anlaşması gerçekleştirir ve IPSec tünelleri oluşturur. [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) ve [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig)ile aşağıdaki örnekte gösterildiği gibi VPN ağ geçidinize genel bir IP adresi oluşturun ve atayın:

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

## <a name="create-a-vpn-gateway"></a>VPN ağ geçidi oluşturma

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında bağlantı oluşturabilirsiniz. Dilerseniz sanal ağınız ile şirket içindeki bir konum arasında da bir bağlantı oluşturabilirsiniz. [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet'i kullanarak bir VPN ağ geçidi oluşturun.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Anahtar parametre değerleri:
* GatewayType: Siteden siteye ve sanal ağdan sanal ağa bağlantılar için **Vpn** değerini kullanın
* VpnType: Daha geniş bir VPN cihazı yelpazesiyle etkileşim kurmak ve daha fazla yönlendirme özelliğinden yararlanmak için **RouteBased** seçeneğini kullanın
* GatewaySku: **VpnGw1** varsayılandır; daha yüksek iş bağlantılarına veya daha fazla bağlantıya ihtiyacınız varsa başka bir VpnGw SKU olarak değiştirin. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

TryIt kullanıyorsanız, oturumunuz zaman dolabilir. Tamam. Ağ geçidi yine de oluşturulur.

Ağ geçidi oluşturma işlemi tamamlandığında sanal ağınız ile başka bir sanal ağ arasında ya da sanal ağınız ile şirket içi bir konum arasında bağlantı oluşturabilirsiniz. Ayrıca, bir istemci bilgisayardan sanal ağınıza yönelik bir P2S bağlantısı yapılandırabilirsiniz.

## <a name="view-the-gateway-public-ip-address"></a>Ağ geçidi genel IP adresini görüntüleme

Genel IP adresinin adını biliyorsanız, ağ geçidine atanan genel IP adresini göstermek için [Get-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) kullanın.

Oturumunuz zaman dolduysa, bu öğreticinin başlangıcından itibaren ortak ağ parametrelerini yeni oturumunuza kopyalayın ve devam edin.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Ağ geçidini yeniden boyutlandırma

Ağ geçidi oluşturulduktan sonra VPN ağ geçidi SKU’sunu değiştirebilirsiniz. Farklı ağ geçidi SDA'ları, iş bağlantıları, bağlantı sayısı vb. gibi farklı özellikleri destekler. Aşağıdaki örnekte, ağ geçidinizi VpnGw1'den VpnGw2'ye yeniden boyutlandırmak için [Resize-AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) kullanır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Bir VPN ağ geçidinin yeniden boyutlandırılması da yaklaşık 30-45 dakika sürer, ancak bu işlem mevcut bağlantıları ve yapılandırmaları kesintiye **uğratmaz** veya kaldırmaz.

## <a name="reset-a-gateway"></a>Ağ geçidini sıfırlama

Sorun giderme adımlarının bir parçası olarak VPN ağ geçidinizi IPsec/IKE tünel yapılandırmalarını yeniden başlatmaya zorlamak için VPN ağ geçidini sıfırlayabilirsiniz. Ağ geçidinizi sıfırlamak için [Sıfırla-AzVirtualNetworkAğ Ağ Geçidi'ni](/powershell/module/az.network/Reset-azVirtualNetworkGateway) kullanın.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Daha fazla bilgi için bkz. [Bir VPN ağ geçidini sıfırlama](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir [sonraki öğreticiye](vpn-gateway-tutorial-vpnconnection-powershell.md)doğru ilerliyorsanız, ön koşullar olduğu için bu kaynakları korumak isteyeceksiniz.

Ancak, ağ geçidi bir prototip, test veya kavram kanıtı dağıtımının bir parçasıysa, kaynak grubunu, VPN ağ geçidini ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakiler gibi temel VPN ağ geçidi oluşturma ve yönetim görevlerini öğrendiniz:

> [!div class="checklist"]
> * VPN ağ geçidi oluşturma
> * Genel IP adresini görüntüleme
> * Bir VPN ağ geçidini yeniden boyutlandırma
> * Bir VPN ağ geçidini sıfırlama

S2S, Sanal Ağdan Sanal Ağa ve P2S bağlantıları hakkında bilgi edinmek için sonraki öğreticilere ilerleyin.

> [!div class="nextstepaction"]
> * [S2S bağlantıları oluşturma](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [VNet'den VNet'e bağlantılar oluşturma](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [P2S bağlantıları oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
