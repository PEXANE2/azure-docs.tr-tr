---
title: 'Azure VPN Ağ Geçidi: yapılandırma ayarları'
description: Azure sanal ağ ağ geçitleri için VPN Ağ Geçidi ayarları hakkında bilgi edinin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: cherylmc
ms.openlocfilehash: d7a2040748d170b4e536df59947ea811f149d931
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244868"
---
# <a name="about-vpn-gateway-configuration-settings"></a>VPN Ağ Geçidi yapılandırma ayarları hakkında

VPN ağ geçidi, sanal ağınız ile şirket içi konumunuz arasında genel bağlantı üzerinden şifreli trafik gönderen bir sanal ağ ağ geçidi türüdür. Azure omurgasına sanal ağlar arasında trafik göndermek için vpn ağ geçidi de kullanabilirsiniz.

VPN ağ geçidi bağlantısı, her biri yapılandırılabilir ayarlar içeren birden çok kaynağın yapılandırmasını oluşturur. Bu makaledeki bölümler, Kaynak Yöneticisi dağıtım modelinde oluşturulan sanal bir ağ için VPN ağ geçidiyle ilgili kaynakları ve ayarları tartışır. [VPN Ağ Geçidi hakkında](vpn-gateway-about-vpngateways.md) makalede her bağlantı çözümü için açıklamalar ve topoloji diyagramları bulabilirsiniz.

Bu makaledeki değerler VPN ağ geçitleri (-GatewayType Vpn kullanan sanal ağ ağ geçitleri) uygular. Bu makale, tüm ağ geçidi türlerini veya bölge yedekağ geçitlerini kapsamaz.

* -GatewayType 'ExpressRoute' için geçerli değerler [için ExpressRoute için Sanal Ağ Ağ Geçitleri'ne](../expressroute/expressroute-about-virtual-network-gateways.md)bakın.

* Bölge yedekli ağ geçitleri için [bölge yedekli ağ geçitleri hakkında](about-zone-redundant-vnet-gateways.md)bkz.

* Sanal WAN için [Virtual WAN hakkında](../virtual-wan/virtual-wan-about.md)bkz.



## <a name="gateway-types"></a><a name="gwtype"></a>Ağ geçidi türleri

Her sanal ağ, her türde yalnızca bir sanal ağ ağ geçidine sahip olabilir. Sanal ağ ağ geçidi oluştururken, ağ geçidi türünün yapılandırmanız için doğru olduğundan emin olmalısınız.

-GatewayType için kullanılabilir değerler şunlardır:

* VPN
* ExpressRoute

VPN ağ geçidi `-GatewayType` *VPN*gerektirir.

Örnek:

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <a name="gateway-skus"></a><a name="gwsku"></a>Ağ Geçidi SKU'ları

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-a-gateway-sku"></a>Ağ geçidi SKU'su yapılandırın

#### <a name="azure-portal"></a>Azure portalında

Kaynak Yöneticisi sanal ağ ağ geçidi oluşturmak için Azure portalını kullanıyorsanız, açılır açılır dosyayı kullanarak ağ geçidi SKU'yu seçebilirsiniz. Size sunulan seçenekler, seçtiğiniz Ağ Geçidi türüne ve VPN türüne karşılık gelir.

#### <a name="powershell"></a>PowerShell

Aşağıdaki PowerShell örneği VpnGw1 `-GatewaySku` olarak belirtir. Bir ağ geçidi oluşturmak için PowerShell kullanırken, önce IP yapılandırmasını oluşturmanız, sonra da ona başvurmak için bir değişken kullanmanız gerekir. Bu örnekte, yapılandırma değişkeni $gwipconfig.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1 `
-Location 'US East' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWPIP --resource-group TestRG1 --vnet VNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait
```

###  <a name="resizing-or-changing-a-sku"></a><a name="resizechange"></a>Bir SKU'nun yeniden boyutlandırılması veya değiştirilmesi

VPN ağ geçidiniz varsa ve farklı bir ağ geçidi SKU kullanmak istiyorsanız, seçenekleriniz ya ağ geçidinizi SKU'nuzu yeniden boyutlandırmak ya da başka bir SKU'ya değiştirmektir. Başka bir ağ geçidi SKU'ya geçiş yaptığınızda, varolan ağ geçidini tamamen siler ve yeni bir ağ geçidi oluşturursunuz. Bir ağ geçidinin oluşturulması 45 dakika kadar sürebilir. Buna karşılık, bir ağ geçidi SKU yeniden boyutlandırdığınızda, ağ geçidini silmek ve yeniden oluşturmak zorunda olmadığınız için çok fazla kapalı kalma süresi yoktur. Ağ geçidinizi değiştirmek yerine SKU'nuzu yeniden boyutlandırma seçeneğiniz varsa, bunu yapmak isteyeceksiniz. Ancak, yeniden boyutlandırma ile ilgili kurallar vardır:

1. Temel SKU dışında, aynı nesil (Generation1 veya Generation2) içinde başka bir VPN ağ geçidi SKU bir VPN ağ geçidi SKU yeniden boyutlandırabilirsiniz. Örneğin, Generation1 VpnGw1 Generation1 VpnGw2 için yeniden boyutlandırılabilir ama Generation2 VpnGw2 için değil.
2. Eski ağ geçidi SKU'larıyla çalışırken Temel, Standart ve Yüksek Performanslı SKU'lar arasında yeniden boyutlandırma yapabilirsiniz.
3. Temel/Standart/Yüksek Performanslı SK'lerden VpnGw SKUs'a yeniden **boyutlandıramazsınız.** Bunun yerine, yeni SNU'lara [geçiş](#change) yapmalısınız.

#### <a name="to-resize-a-gateway"></a><a name="resizegwsku"></a>Bir ağ geçidini yeniden boyutlandırmak için

[!INCLUDE [Resize a SKU](../../includes/vpn-gateway-gwsku-resize-include.md)]

####  <a name="to-change-from-an-old-legacy-sku-to-a-new-sku"></a><a name="change"></a>Eski (eski) SKU'dan yeni bir SKU'ya geçiş yapmak için

[!INCLUDE [Change a SKU](../../includes/vpn-gateway-gwsku-change-legacy-sku-include.md)]

## <a name="connection-types"></a><a name="connectiontype"></a>Bağlantı türleri

Kaynak Yöneticisi dağıtım modelinde, her yapılandırma belirli bir sanal ağ ağ ağ geçidi türü gerektirir. `-ConnectionType` için kullanılabilir Resource Manager PowerShell değerleri şunlardır:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

Aşağıdaki PowerShell örneğinde, bağlantı türü *IPsec*gerektiren bir S2S bağlantısı oluşturuyoruz.

```azurepowershell-interactive
New-AzVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <a name="vpn-types"></a><a name="vpntype"></a>VPN türleri

VPN ağ geçidi yapılandırması için sanal ağ ağ geçidi oluşturduğunuzda, bir VPN türü belirtmeniz gerekir. Seçtiğiniz VPN türü, oluşturmak istediğiniz bağlantı topolojisine bağlıdır. Örneğin, Bir P2S bağlantısı routebased VPN türü gerektirir. VPN türü de kullandığınız donanıma bağlı olabilir. S2S yapılandırmaları bir VPN aygıtı gerektirir. Bazı VPN aygıtları yalnızca belirli bir VPN türünü destekler.

Seçtiğiniz VPN türü, oluşturmak istediğiniz çözüm için tüm bağlantı gereksinimlerini karşılamalıdır. Örneğin, aynı sanal ağ için bir S2S VPN ağ geçidi bağlantısı ve P2S VPN ağ geçidi bağlantısı oluşturmak istiyorsanız, P2S RouteBased VPN türü gerektirdiğinden VPN türü *RouteBased'i* kullanırsınız. VPN cihazınızın RouteBased VPN bağlantısını desteklediğini de doğrulamanız gerekir. 

Sanal ağ ağ geçidi oluşturulduktan sonra VPN türünü değiştiremezsiniz. Sanal ağ ağ ağ geçidini silmek ve yeni bir ağ oluşturmak zorunda. İki VPN türü vardır:

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Aşağıdaki PowerShell örneği `-VpnType` *RouteBased*olarak belirtir. Bir ağ geçidi oluştururken, -VpnType öğesinin yapılandırmanız için doğru olduğundan emin olmanız gerekir.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <a name="gateway-requirements"></a><a name="requirements"></a>Ağ geçidi gereksinimleri

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <a name="gateway-subnet"></a><a name="gwsub"></a>Ağ geçidi alt ağı

VPN ağ geçidi oluşturmadan önce bir ağ geçidi alt ağı oluşturmanız gerekir. Ağ geçidi alt ağı, sanal ağ ağ geçidi VM'lerinin ve hizmetlerinin kullandığı IP adreslerini içerir. Sanal ağ ağ ağ geçidinizi oluşturduğunuzda, ağ geçidi VM'leri ağ geçidi alt ağına dağıtılır ve gerekli VPN ağ geçidi ayarlarıyla yapılandırılır. Ağ geçidi alt ağına başka bir şey (örneğin, ek VM'ler) dağıtmayın. Düzgün çalışması için ağ geçidi alt ağı 'GatewaySubnet' olarak adlandırılmalıdır. Ağ geçidi alt netini 'GatewaySubnet' olarak adlandırmak, Azure'un sanal ağ ağ vm'lerini ve hizmetlerini dağıtmak için alt ağ olduğunu bilmesini sağlar.

>[!NOTE]
>[!INCLUDE [vpn-gateway-gwudr-warning.md](../../includes/vpn-gateway-gwudr-warning.md)]
>

Ağ geçidi alt ağı oluştururken, alt ağın içerdiği IP adresi sayısını belirtirsiniz. Ağ geçidi alt netindeki IP adresleri ağ geçidi VM'lerine ve ağ geçidi hizmetlerine ayrılır. Bazı yapılandırmalar için diğerlerinden daha fazla IP adresi gerekir. 

Ağ geçidi alt ağ boyutunuzu planlarken, oluşturmayı planladığınız yapılandırma için belgelere bakın. Örneğin, ExpressRoute/VPN Ağ Geçidi birlikte var olan yapılandırma, diğer yapılandırmalardan daha büyük bir ağ geçidi alt ağı gerektirir. Ayrıca, ağ geçidi alt ağınızın gelecekteki olası ek yapılandırmaları barındıracak kadar IP adresi içerdiğinden emin olmak isteyebilirsiniz. /29 gibi küçük bir ağ geçidi alt ağı oluşturabilirsiniz, ancak bunu yapmak için kullanılabilir adres alanınız varsa /27 veya daha büyük (/27, /26 vb.) bir ağ geçidi alt ağı oluşturmanızı öneririz. Bu, çoğu yapılandırmayı barındırır.

Aşağıdaki Kaynak Yöneticisi PowerShell örneği, GatewaySubnet adlı bir ağ geçidi alt ağı gösterir. CIDR notasyonunun, şu anda var olan yapılandırmaların çoğu için yeterli IP adresine olanak tanıyan bir /27 belirtirolduğunu görebilirsiniz.

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="local-network-gateways"></a><a name="lng"></a>Yerel ağ ağ geçitleri

 Yerel ağ ağ geçidi, sanal ağ ağ geçidinden farklıdır. VPN ağ geçidi yapılandırması oluştururken, yerel ağ ağ geçidi genellikle şirket içi konumunuzu temsil eder. Klasik dağıtım modelinde, yerel ağ geçidi için Yerel Site olara ifade edilir.

Yerel ağ ağ geçidine bir ad, şirket içi VPN aygıtının genel IP adresini verir ve şirket içi konumda bulunan adres önekleri belirtirsiniz. Azure, ağ trafiği için hedef adres öneklerine bakar, yerel ağ ağ ağ geçidiniz için belirttiğiniz yapılandırmaya başvurur ve paketleri buna göre yönlendirir. VPN ağ geçidi bağlantısı kullanan VNet-to-VNet yapılandırmaları için yerel ağ ağ ağ geçitleri de belirtirsiniz.

Aşağıdaki PowerShell örneği yeni bir yerel ağ ağ geçidi oluşturur:

```azurepowershell-interactive
New-AzLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

Bazen yerel ağ ağ geçidi ayarlarını değiştirmeniz gerekir. Örneğin, adres aralığını eklediğinizde veya değiştirdiğinizde veya VPN aygıtının IP adresi değişirse. Bkz. [PowerShell'i kullanarak yerel ağ ağ ağ geçidi ayarlarını değiştir.](vpn-gateway-modify-local-network-gateway.md)

## <a name="rest-apis-powershell-cmdlets-and-cli"></a><a name="resources"></a>REST API'ler, PowerShell cmdlets ve CLI

VPN Ağ Geçidi yapılandırmaları için REST API'leri, PowerShell cmdlets veya Azure CLI kullanırken ek teknik kaynaklar ve belirli sözdizimi gereksinimleri için aşağıdaki sayfalara bakın:

| **Klasik** | **Kaynak Yöneticisi** |
| --- | --- |
| [Powershell](/powershell/module/az.network/#networking) |[Powershell](/powershell/module/az.network#vpn) |
| [REST API](https://msdn.microsoft.com/library/jj154113) |[REST API](/rest/api/network/virtualnetworkgateways) |
| Desteklenmiyor | [Azure CLI](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir bağlantı yapılandırmaları hakkında daha fazla bilgi için [VPN Ağ Geçidi Hakkında'ya](vpn-gateway-about-vpngateways.md)bakın.
