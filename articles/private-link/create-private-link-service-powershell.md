---
title: Create an Azure private link service using Azure PowerShell| Microsoft Dokümanlar
description: Azure PowerShell'i kullanarak Azure özel bağlantı hizmeti oluşturmayı öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 225ae9d07cc6df2fa809e250083ee6007ab2f945
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76932074"
---
# <a name="create-a-private-link-service-using-azure-powershell"></a>Azure PowerShell'i kullanarak Özel Bağlantı hizmeti oluşturma
Bu makalede, Azure PowerShell kullanarak Azure'da nasıl özel bir bağlantı hizmeti oluşturabileceğiniz gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalenin en son Azure PowerShell modülü sürümü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Özel bağlantınızı oluşturamadan önce, [Yeni-AzResourceGroup'a](/powershell/module/az.resources/new-azresourcegroup)sahip bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *WestCentralUS* konumunda *myResourceGroup* adında bir kaynak grubu oluşturur:

```azurepowershell
$location = "westcentralus"
$rgName = "myResourceGroup"
New-AzResourceGroup `
  -ResourceGroupName $rgName `
  -Location $location
```
## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile özel bağlantınız için sanal bir ağ oluşturun. Aşağıdaki örnek, frontend (*frontendSubnet*), backend (*backendSubnet*), private link *(otherSubnet)* için alt ağ ile *myvnet* adlı bir sanal ağ oluşturur:

```azurepowershell
$virtualNetworkName = "myvnet"


# Create subnet config

$frontendSubnet = New-AzVirtualNetworkSubnetConfig `
-Name frontendSubnet `
-AddressPrefix "10.0.1.0/24"  

$backendSubnet = New-AzVirtualNetworkSubnetConfig `
-Name backendSubnet `
-AddressPrefix "10.0.2.0/24"  

$otherSubnet = New-AzVirtualNetworkSubnetConfig `
-Name otherSubnet `
-AddressPrefix "10.0.3.0/24" `
-PrivateLinkServiceNetworkPolicies "Disabled" 

# Create the virtual network
$vnet = New-AzVirtualNetwork `
-Name $virtualNetworkName `
-ResourceGroupName $rgName `
-Location $location `
-AddressPrefix "10.0.0.0/16" `
-Subnet $frontendSubnet,$backendSubnet,$otherSubnet 
```
## <a name="create-internal-load-balancer"></a>Dahili Yük Dengeleyicisi Oluşturma
[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile dahili bir Standart Yük Dengeleyicioluşturun. Aşağıdaki örnek, önceki adımlarda oluşturduğunuz ön uç IP yapılandırması, sonda, kural ve arka uç havuzunu kullanarak bir dahili Standart Yük Dengeleyicisi oluşturur:

```azurepowershell

$lbBackendName = "LB-backend" 
$lbFrontName = "LB-frontend" 
$lbName = "lb"
 
#Create Internal Load Balancer
$frontendIP = New-AzLoadBalancerFrontendIpConfig -Name $lbFrontName -PrivateIpAddress 10.0.1.5 -SubnetId $vnet.subnets[0].Id 
$beaddresspool= New-AzLoadBalancerBackendAddressPoolConfig -Name $lbBackendName 
$probe = New-AzLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
$rule = New-AzLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beaddresspool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
$NRPLB = New-AzLoadBalancer -ResourceGroupName $rgName -Name $lbName -Location $location -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $probe -LoadBalancingRule $rule -Sku Standard 
```
## <a name="create-a-private-link-service"></a>Özel bağlantı hizmeti oluşturma
[New-AzPrivateLinkService](/powershell/module/az.network/new-azloadbalancer)ile özel bir bağlantı hizmeti oluşturun.  Bu örnek, *myResourceGroup*adlı kaynak grubunda Standart Yük Dengeleyicisi kullanılarak *myPLS* adlı özel bir bağlantı hizmeti oluşturur. 
```azurepowershell

$plsIpConfigName = "PLS-ipconfig" 
$plsName = "pls"
$peName = "pe" 
  
$IPConfig = New-AzPrivateLinkServiceIpConfig `
-Name $plsIpConfigName `
-Subnet $vnet.subnets[2] `
-PrivateIpAddress 10.0.3.5 

$fe = Get-AzLoadBalancer -Name $lbName | Get-AzLoadBalancerFrontendIpConfig 

$privateLinkService = New-AzPrivateLinkService `
-ServiceName $plsName `
-ResourceGroupName $rgName `
-Location $location `
-LoadBalancerFrontendIpConfiguration $frontendIP `
-IpConfiguration $IPConfig 
```

### <a name="get-private-link-service"></a>Özel bağlantı hizmeti alın
[Get-AzPrivateLinkService](/powershell/module/az.network/get-azprivatelinkservice) ile özel bağlantı hizmetiniz hakkında ayrıntılı bilgi alın:

```azurepowershell
$pls = Get-AzPrivateLinkService -Name $plsName -ResourceGroupName $rgName 
```

Bu aşamada, Özel Bağlantı Hizmetiniz başarıyla oluşturulur ve trafiği almaya hazırdır. Yukarıdaki örneğin yalnızca PowerShell'i kullanarak Özel Bağlantı Hizmeti oluşturmayı göstermek için olduğunu unutmayın.  Trafiği dinlemek için yük dengeleyici arka uç havuzlarını veya arka uç havuzlarındaki herhangi bir uygulamayı yapılandırmadık. Trafik akışlarının sonuna kadar görmek istiyorsanız, uygulamanızı standart yük bakiyecinizin arkasına yapılandırmanız önerilir. 

Daha sonra powershell kullanarak farklı VNet'te bu hizmeti özel bir bitiş noktasına nasıl haritalayabildiğimizi göstereceğiz. Yine, örnek Özel Bitiş Noktası oluşturma ve yukarıda oluşturulan Özel Bağlantı Hizmetine bağlanma ile sınırlıdır. Sanal Ağ'da sanal makineler oluşturarak senaryonuzu oluşturmak için özel bitiş noktasına trafik gönderebilirsiniz.. 

## <a name="create-a-private-endpoint"></a>Özel Uç Nokta oluşturma
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile özel bitiş noktanız için sanal bir ağ oluşturun. Bu örnek, *myResourceGroup*adlı kaynak grubunda *vnetPE* adlı bir sanal ağ oluşturur:
 
```azurepowershell
$virtualNetworkNamePE = "vnetPE"
 
# Create VNet for private endpoint
$peSubnet = New-AzVirtualNetworkSubnetConfig `
-Name peSubnet `
-AddressPrefix "11.0.1.0/24" `
-PrivateEndpointNetworkPolicies "Disabled" 

$vnetPE = New-AzVirtualNetwork `
-Name $virtualNetworkNamePE `
-ResourceGroupName $rgName `
-Location $location `
-AddressPrefix "11.0.0.0/16" `
-Subnet $peSubnet 
```

### <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma
Sanal aağınızda yukarıda oluşturulan özel bağlantı hizmetini tüketmek için özel bir bitiş noktası oluşturun:
 
```azurepowershell
 
$plsConnection= New-AzPrivateLinkServiceConnection `
-Name plsConnection `
-PrivateLinkServiceId  $privateLinkService.Id  

$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $rgName -Name $peName -Location $location -Subnet $vnetPE.subnets[0] -PrivateLinkServiceConnection $plsConnection -ByManualRequest 
```
 
### <a name="get-private-endpoint"></a>Özel bitiş noktası alın
Özel bitiş noktasının IP adresini `Get-AzPrivateEndpoint` aşağıdaki gibi alın:

```azurepowershell
# Get Private Endpoint and its IP Address 
$pe =  Get-AzPrivateEndpoint `
-Name $peName `
-ResourceGroupName $rgName  `
-ExpandResource networkinterfaces

$pe.NetworkInterfaces[0].IpConfigurations[0].PrivateIpAddress 

```

### <a name="approve-the-private-endpoint-connection"></a>Özel uç nokta bağlantısını onaylama
Özel bağlantı hizmetine özel bitiş noktası bağlantısını 'Approve-AzPrivateEndpointConnection' ile onaylayın.

```azurepowershell   

$pls = Get-AzPrivateLinkService `
-Name $plsName `
-ResourceGroupName $rgName 

Approve-AzPrivateEndpointConnection -ResourceId $pls.PrivateEndpointConnections[0].Id -Description "Approved" 

``` 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel bağlantısı](private-link-overview.md) hakkında daha fazla bilgi edinin
 
