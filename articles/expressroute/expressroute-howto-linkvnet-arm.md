---
title: 'ExpressRoute: sanal ağı bir devreye bağlama: Azure PowerShell'
description: Bu belge, PowerShell ve Resource Manager dağıtım modeli kullanarak ExpressRoute devresine bağlama sanal ağları (Vnet) genel bakış sağlar.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 05/20/2018
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 2685b9b519eaac453726f4923c46f1604cbd4681
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78384277"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Bir sanal ağı ExpressRoute devresine bağlama
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video-Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makale, Resource Manager dağıtım modeli ve PowerShell kullanarak, sanal ağlar (Vnet'ler) bağlantı Azure ExpressRoute bağlantı hatları için olur. Sanal ağlar aynı abonelikte veya başka bir abonelik parçası olarak ya da olabilir. Bu makalede ayrıca bir sanal ağ bağlantısı güncelleştirme gösterilmektedir.

* Standart bir ExpressRoute bağlantı hattı için en fazla 10 sanal ağlara bağlayabilirsiniz. Tüm sanal ağları, standart bir ExpressRoute bağlantı hattını kullanırken aynı jeopolitik bölgede olması gerekir. 

* En fazla dört ExpressRoute bağlantı hatları için tek bir sanal ağa bağlanabilir. Bağlanmakta olduğunuz her bir ExpressRoute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için bu makaledeki adımları kullanın. ExpressRoute bağlantı hatları, aynı abonelik, farklı Aboneliklerde veya her ikisinin bir karışımı olabilir.

* ExpressRoute devresinin coğrafi bölge dışında sanal ağlara bağlantı ya da ExpressRoute premium eklentisi etkinleştirildiğinde, çok sayıda sanal ağları ExpressRoute devreniz bağlayın. Premium eklenti hakkında daha fazla bilgi için [SSS bölümüne](expressroute-faqs.md) bakın.


## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * [ExpressRoute](expressroute-howto-circuit-arm.md) bağlantı hattı oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcınız tarafından devre dışı bırakıldı. 
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Yönlendirme yönergeleri için [yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md) makalesine bakın. 
  * Azure özel eşdüzey hizmet sağlama yapılandırılır ve uçtan uca bağlantıyı etkinleştirmek üzere ağınız ile Microsoft arasında BGP eşliği ayarlama olduğundan emin olun.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan sanal ağ geçidi olduğundan emin olun. [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md)yönergelerini izleyin. ExpressRoute için sanal ağ geçidi, GatewayType 'ExpressRoute' VPN'değil kullanır.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Bir sanal ağ ile aynı abonelikte devreye bağlama
Aşağıdaki cmdlet'i kullanarak ExpressRoute bağlantı hattına bir sanal ağ geçidine bağlanabilir. Sanal ağ geçidi oluşturulur ve cmdlet çalıştırılmadan önce bağlamak için hazır olduğundan emin olun:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir ExpressRoute bağlantı hattı birden çok farklı abonelikler arasında paylaşabilirsiniz. Aşağıdaki şekilde birden fazla aboneliği analiz basit bir ExpressRoute bağlantı hatları için nasıl paylaşım Works şematik gösterir.

Her küçük bulutların büyük bulut içinde bir kuruluştaki farklı departmanlara ait abonelikleri temsil etmek için kullanılır. Her kuruluş içindeki bölümlerin dağıtmak için kendi hizmetlerini--ancak kendi aboneliğini kullanabilirsiniz, şirket içi ağınıza bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşabilirsiniz. Tek bir bölüm (Bu örnekte: BT) ExpressRoute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler, ExpressRoute bağlantı hattı kullanabilirsiniz.

> [!NOTE]
> Abonelik sahibi için ExpressRoute bağlantı hattı için bağlantı ve bant genişliği ücretleri uygulanır. Tüm sanal ağları, aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim - bağlantı hattı sahibi ve bağlantı hattı kullanıcıları

'Bağlantı hattı sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcıdır. Bağlantı hattı sahibinden 'devre kullanıcıları tarafından' ödenebilecek yetkilendirmeleri oluşturabilirsiniz. ExpressRoute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitleri sahipleri bağlantı hattını kullanıcılardır. Devre kullanıcılarının, yetkilendirmeleri (sanal ağ başına bir yetkilendirme) kullanmak.

Bağlantı hattı sahibinden yetkilendirme dilediğiniz zaman iptal et ve değiştirmek için gücüne sahiptir. Bir yetkilendirme sonuçlarına tüm bağlantı erişimini iptal edildi abonelikten silinmesini iptal ediliyor.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Yetkilendirme oluşturmak için**

Bağlantı hattı sahibinden bir yetkilendirme oluşturur. ExpressRoute bağlantı hattına kendi sanal ağ geçitlerine bağlanmak için bir bağlantı hattı kullanıcısı tarafından kullanılan bir yetkilendirme anahtarına oluşturulmasını sonuçlanır. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

Aşağıdaki cmdlet kod parçacığında, bir yetkilendirme oluşturma işlemi gösterilmektedir:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


Bu yanıt durumu ve yetkilendirme anahtarını içerir:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**Yetkilendirmeleri gözden geçirmek için**

Bağlantı hattı sahibinden belirli bir bağlantı hattı üzerinde aşağıdaki cmdlet'i çalıştırarak düzenlenen tüm yetkilendirmeleri gözden geçirebilirsiniz:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilendirmeler eklemek için**

Bağlantı hattı sahibinden yetkilendirme, aşağıdaki cmdlet'i kullanarak ekleyebilirsiniz:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilendirmeleri silmek için**

Bağlantı hattı sahibinden iptal etme/yetkilendirmeleri kullanıcı için aşağıdaki cmdlet'i çalıştırarak ya da silebilir:

```azurepowershell-interactive
Remove-AzExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcısı eş kimliği ve bağlantı hattı sahibinden yetkilendirme anahtarı gerekir. Yetkilendirme anahtarı bir GUID'dir.

Aşağıdaki komutu eş kimliği denetlenebilir:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bir bağlantı yetkilendirmesini kullanma**

Bağlantı hattı kullanıcısı bağlantı yetkilendirme kullanmak için aşağıdaki cmdlet'i çalıştırabilirsiniz:

```azurepowershell-interactive
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bir bağlantı yetkilendirmesini serbest bırakmak için**

Sanal ağı ExpressRoute bağlantı hattına bağlayan bağlantı silerek bir yetkilendirme serbest bırakabilirsiniz.

## <a name="modify-a-virtual-network-connection"></a>Sanal ağ bağlantısı değiştirme
Belirli bir sanal ağ bağlantısı özellikleri güncelleştirebilirsiniz. 

**Bağlantı ağırlığını güncelleştirmek için**

Sanal ağınız için birden çok ExpressRoute bağlantı hattına bağlı olabilir. Aynı öneke birden fazla ExpressRoute devresinden alabilirsiniz. Bu önek için hangi trafiğin gönderileceğini seçmek üzere bir bağlantının *Routingweight* değerini değiştirebilirsiniz. Trafik en yüksek *Routingweight*bağlantısı üzerinden gönderilir.

```azurepowershell-interactive
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

*Routingweight* aralığı 0 ile 32000 arasındadır. Varsayılan değer 0’dır.

## <a name="configure-expressroute-fastpath"></a>ExpressRoute FastPath yapılandırma 
ExpressRoute bağlantı hattınızı [ExpressRoute Direct](expressroute-erdirect-about.md) üzerinde ise ve sanal ağ geçidiniz Ultra Performance veya ErGw3AZ Ise [ExpressRoute FastPath](expressroute-about-virtual-network-gateways.md) 'i etkinleştirebilirsiniz. FastPath, şirket içi ağınız ve sanal ağınız arasında saniye başına paket ve bağlantı gibi veri yolu performansını geliştirir. 

**Yeni bir bağlantıda FastPath yapılandırma**

```azurepowershell-interactive 
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG" 
$gw = Get-AzVirtualNetworkGateway -Name "MyGateway" -ResourceGroupName "MyRG" 
$connection = New-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" -ExpressRouteGatewayBypass -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute -Location "MyLocation" 
``` 

**FastPath 'i etkinleştirmek için mevcut bir bağlantı güncelleştiriliyor**

```azurepowershell-interactive 
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" 
$connection.ExpressRouteGatewayBypass = $True
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
``` 

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
