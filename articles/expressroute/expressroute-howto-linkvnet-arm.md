---
title: "ExpressRoute: Bir VNet'i bir devreye bağla: Azure PowerShell"
description: Bu belge, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak sanal ağların (VNet) ExpressRoute devrelerine nasıl bağlanılacak larına genel bir bakış sağlar.
services: expressroute
author: ganesr
ms.service: expressroute
ms.topic: article
ms.date: 05/20/2018
ms.author: ganesr
ms.custom: seodec18
ms.openlocfilehash: 242f52d643e817730772a7d678a219c2b6149d2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235460"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Sanal ağı ExpressRoute bağlantı hattına bağlama
> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Powershell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
>

Bu makale, Kaynak Yöneticisi dağıtım modelini ve PowerShell'i kullanarak sanal ağları (VNets) Azure ExpressRoute devrelerine bağlamanıza yardımcı olur. Sanal ağlar aynı abonelikte veya başka bir aboneliğin parçası olabilir. Bu makalede, sanal ağ bağlantısını nasıl güncelleştirdiğinizde de gösterilmektedir.

* Standart bir ExpressRoute devresine en fazla 10 sanal ağ bağlayabilirsiniz. Standart bir ExpressRoute devresi kullanırken tüm sanal ağlar aynı jeopolitik bölgede olmalıdır. 

* Tek bir VNet en fazla dört ExpressRoute devresine bağlanabilir. Bağlandığınız her ExpressRoute devresi için yeni bir bağlantı nesnesi oluşturmak için bu makaledeki adımları kullanın. ExpressRoute devreleri aynı abonelikte, farklı aboneliklerde veya her ikisinin bir karışımında olabilir.

* ExpressRoute devresinin jeopolitik bölgesinin dışında sanal ağları bağlayabilir veya ExpressRoute premium eklentisini etkinleştirdiyseniz daha fazla sayıda sanal ağı ExpressRoute devrenize bağlayabilirsiniz. Premium eklenti hakkında daha fazla bilgi için [SSS'yi](expressroute-faqs.md) kontrol edin.


## <a name="before-you-begin"></a>Başlamadan önce

* Yapılandırmaya başlamadan önce [ön koşulları,](expressroute-prerequisites.md) [yönlendirme gereksinimlerini](expressroute-routing.md)ve [iş akışlarını](expressroute-workflows.md) gözden geçirin.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. 
  * [Bir ExpressRoute devresi oluşturmak](expressroute-howto-circuit-arm.md) ve devreyi bağlantı sağlayıcınız tarafından etkinleştirmek için yönergeleri izleyin. 
  * Devreniz için Azure özel eşlemeyapılı olduğundan emin olun. Yönlendirme yönergeleri için [yapılandırma yönlendirme](expressroute-howto-routing-arm.md) makalesine bakın. 
  * Azure özel eşlemenin yapılandırıldığından ve ağınızdan Microsoft'a bgp eşlemesi doldu böylece uçuca bağlantı sağlayabilirsiniz.
  * Sanal bir ağa ve sanal ağ ağ geçidinin oluşturulduğundan ve tam olarak sağlanmış olduğundan emin olun. [ExpressRoute için sanal ağ ağ geçidi oluşturmak için](expressroute-howto-add-gateway-resource-manager.md)yönergeleri izleyin. ExpressRoute için bir sanal ağ ağ geçidi, VPN değil, GatewayType 'ExpressRoute' kullanır.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Aynı abonelikteki sanal ağı bir devreye bağlama
Aşağıdaki cmdlet'i kullanarak bir ExpressRoute devresine sanal ağ ağ geçidi bağlayabilirsiniz. Cmdlet'i çalıştırmadan önce sanal ağ ağ geçidinin oluşturulduğundan ve bağlanmaya hazır olduğundan emin olun:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Farklı abonelikteki bir sanal ağı devreye bağlama
Bir ExpressRoute devresi'ni birden çok abonelik arasında paylaşabilirsiniz. Aşağıdaki şekil, expressroute devreleri için birden çok abonelik te paylaşımın nasıl çalıştığını gösteren basit bir şema dır.

Büyük bulutiçindeki küçük bulutların her biri, kuruluştaki farklı bölümlere ait abonelikleri temsil etmek için kullanılır. Kuruluş içindeki departmanların her biri, hizmetlerini dağıtmak için kendi aboneliklerini kullanabilir, ancak şirket içi ağınıza bağlanmak için tek bir ExpressRoute devresini paylaşabilir. Tek bir departman (bu örnekte: BT) ExpressRoute devresi sahibi olabilir. Kuruluş içindeki diğer abonelikler ExpressRoute devresini kullanabilir.

> [!NOTE]
> ExpressRoute devresi için bağlantı ve bant genişliği ücretleri abonelik sahibine uygulanır. Tüm sanal ağlar aynı bant genişliğini paylaşır.
> 
> 

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Yönetim - devre sahipleri ve devre kullanıcıları

'Devre sahibi' ExpressRoute devre kaynağının yetkili bir Güç Kullanıcısıdır. Devre sahibi 'devre kullanıcıları' tarafından kullanılabilir yetkilendirmeler oluşturabilirsiniz. Devre kullanıcıları, ExpressRoute devresi ile aynı abonelik içinde olmayan sanal ağ ağ ağ geçitlerinin sahipleridir. Devre kullanıcıları yetkilendirmeleri kullanabilir (sanal ağ başına bir yetkilendirme).

Devre sahibi, yetkilendirmeleri istediği zaman değiştirme ve iptal etme yetkisine sahiptir. Yetkilendirmenin iptaledilmesi, erişimi iptal edilen abonelikten tüm bağlantı bağlantılarının silinmelerine neden olur.

### <a name="circuit-owner-operations"></a>Devre sahibi işlemleri

**Yetkilendirme oluşturmak için**

Devre sahibi bir yetkilendirme oluşturur. Bu, bir devre kullanıcısı tarafından sanal ağ ağ ağ geçitlerini ExpressRoute devresine bağlamak için kullanılabilecek bir yetkilendirme anahtarı oluşturulmasıyla sonuçlanır. Yetkilendirme yalnızca bir bağlantı için geçerlidir.

Aşağıdaki cmdlet snippet nasıl bir yetkilendirme oluşturmak için gösterir:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


Buna yanıt yetkilendirme anahtarı ve durumu içerir:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**Yetkilendirmeleri gözden geçirmek için**

Devre sahibi aşağıdaki cmdlet çalıştırarak belirli bir devre üzerinde verilen tüm yetkileri gözden geçirebilirsiniz:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilendirme eklemek için**

Devre sahibi aşağıdaki cmdlet kullanarak yetkilendirme ekleyebilir:

```azurepowershell-interactive
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**Yetkilendirmeleri silmek için**

Devre sahibi aşağıdaki cmdlet çalıştırarak kullanıcıya yetkileri iptal /silme:

```azurepowershell-interactive
Remove-AzExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Devre kullanıcı işlemleri

Devre kullanıcısının eş kimliğine ve devre sahibinden bir yetkilendirme anahtarına ihtiyacı vardır. Yetkilendirme anahtarı bir GUID'dir.

Peer ID aşağıdaki komuttan denetlenebilir:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**Bağlantı yetkisini kullanmak için**

Devre kullanıcısı, bağlantı yetkilendirmesini kullanmak için aşağıdaki cmdlet'i çalıştırabilir:

```azurepowershell-interactive
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**Bağlantı yetkilendirmesi serbest bırakmak için**

ExpressRoute devresini sanal ağa bağlayan bağlantıyı silerek yetkilendirme bırakabilirsiniz.

## <a name="modify-a-virtual-network-connection"></a>Sanal ağ bağlantısını değiştirme
Sanal ağ bağlantısının belirli özelliklerini güncelleştirebilirsiniz. 

**Bağlantı ağırlığını güncelleştirmek için**

Sanal ağınız birden çok ExpressRoute devresine bağlanabilir. Aynı öneki birden fazla ExpressRoute devresinden alabilirsiniz. Bu önek için hangi bağlantıyı göndereceğinizi seçmek için, bir bağlantının *RoutingWeight'ini* değiştirebilirsiniz. Trafik en yüksek *RoutingWeight*ile bağlantı gönderilecektir.

```azurepowershell-interactive
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

*RoutingWeight* aralığı 0-32000 arasındadır. Varsayılan değer 0’dır.

## <a name="configure-expressroute-fastpath"></a>ExpressRoute FastPath'i yapılandır 
Sanal ağ ağ ağ geçidiniz Ultra Performance veya ErGw3AZ ise [ExpressRoute FastPath'i](expressroute-about-virtual-network-gateways.md) etkinleştirebilirsiniz. FastPath, şirket içi ağınızla sanal ağınız arasındaki saniyedeki paketler ve bağlantılar gibi veri yolu performansını artırır. 

**FastPath'i yeni bir bağlantıda yapılandırma**

```azurepowershell-interactive 
$circuit = Get-AzExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG" 
$gw = Get-AzVirtualNetworkGateway -Name "MyGateway" -ResourceGroupName "MyRG" 
$connection = New-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" -ExpressRouteGatewayBypass -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute -Location "MyLocation" 
``` 

**FastPath'i etkinleştirmek için varolan bağlantıyı güncelleştirme**

```azurepowershell-interactive 
$connection = Get-AzVirtualNetworkGatewayConnection -Name "MyConnection" -ResourceGroupName "MyRG" 
$connection.ExpressRouteGatewayBypass = $True
Set-AzVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
``` 

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
