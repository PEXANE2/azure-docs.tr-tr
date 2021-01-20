---
title: 'Hızlı başlangıç: Azure PowerShell kullanarak bir Azure özel bağlantı hizmeti oluşturma'
description: Azure PowerShell kullanarak bir Azure özel bağlantı hizmeti oluşturmayı öğrenin
services: private-link
author: asudbring
ms.service: private-link
ms.topic: how-to
ms.date: 01/20/2021
ms.author: allensu
ms.openlocfilehash: 66ad5aae9f8175d154bb07a8b112dada175a205a
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98610072"
---
# <a name="create-a-private-link-service-using-azure-powershell"></a>Azure PowerShell kullanarak özel bir bağlantı hizmeti oluşturma

Hizmetinize başvuran bir özel bağlantı hizmeti oluşturmaya başlayın.  Azure Standart Load Balancer arkasında dağıtılan hizmetinize veya kaynağa özel bağlantı erişimi verin.  Hizmetinizin kullanıcılarının sanal ağından özel erişimi vardır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell yerel olarak veya Azure Cloud Shell yüklendi

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun:

```azurepowershell-interactive
New-AzResourceGroup -Name 'CreatePrivLinkService-rg' -Location 'eastus2'

```
---
## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

Bu bölümde, bir sanal ağ ve bir iç Azure Load Balancer oluşturacaksınız.

### <a name="virtual-network"></a>Sanal ağ

Bu bölümde, özel bağlantı hizmetinize erişen yük dengeleyiciyi barındırmak için bir sanal ağ ve alt ağ oluşturursunuz.

* [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile bir sanal ağ oluşturun.

```azurepowershell-interactive
## Create backend subnet config ##
$subnet = @{
    Name = 'mySubnet'
    AddressPrefix = '10.1.0.0/24'
    PrivateLinkServiceNetworkPolicies = 'Disabled'
}
$subnetConfig = New-AzVirtualNetworkSubnetConfig @subnet 

## Create the virtual network ##
$net = @{
    Name = 'myVNet'
    ResourceGroupName = 'CreatePrivLinkService-rg'
    Location = 'eastus2'
    AddressPrefix = '10.1.0.0/16'
    Subnet = $subnetConfig
}
$vnet = New-AzVirtualNetwork @net

```

### <a name="create-standard-load-balancer"></a>Standart yük dengeleyici oluştur

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

* Ön uç IP havuzu için [New-Azloadbalancerfrontendıpconfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) ile bir ön uç IP 'si oluşturun. Bu IP, yük dengeleyicide gelen trafiği alır

* Yük dengeleyicinin ön uçınızdan gönderilen trafik için [New-Azloadbalancerbackendadddresspoolconfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) ile arka uç adres havuzu oluşturun. Bu havuz, arka uç sanal makinelerinizin dağıtıldığı yerdir.

* [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig) ile arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması oluşturun.

* Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig) ile bir yük dengeleyici kuralı oluşturun.

* [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile bir genel yük dengeleyici oluşturun.


```azurepowershell-interactive
## Place virtual network created in previous step into a variable. ##
$vnet = Get-AzVirtualNetwork -Name 'myVNet' -ResourceGroupName 'CreatePrivLinkService-rg'

## Create load balancer frontend configuration and place in variable. ##
$lbip = @{
    Name = 'myFrontEnd'
    PrivateIpAddress = '10.1.0.4'
    SubnetId = $vnet.subnets[0].Id
}
$feip = New-AzLoadBalancerFrontendIpConfig @lbip

## Create backend address pool configuration and place in variable. ##
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'

## Create the health probe and place in variable. ##
$probe = @{
    Name = 'myHealthProbe'
    Protocol = 'http'
    Port = '80'
    IntervalInSeconds = '360'
    ProbeCount = '5'
    RequestPath = '/'
}
$healthprobe = New-AzLoadBalancerProbeConfig @probe

## Create the load balancer rule and place in variable. ##
$lbrule = @{
    Name = 'myHTTPRule'
    Protocol = 'tcp'
    FrontendPort = '80'
    BackendPort = '80'
    IdleTimeoutInMinutes = '15'
    FrontendIpConfiguration = $feip
    BackendAddressPool = $bePool
}
$rule = New-AzLoadBalancerRuleConfig @lbrule -EnableTcpReset

## Create the load balancer resource. ##
$loadbalancer = @{
    ResourceGroupName = 'CreatePrivLinkService-rg'
    Name = 'myLoadBalancer'
    Location = 'eastus2'
    Sku = 'Standard'
    FrontendIpConfiguration = $feip
    BackendAddressPool = $bePool
    LoadBalancingRule = $rule
    Probe = $healthprobe
}
New-AzLoadBalancer @loadbalancer

```

## <a name="create-a-private-link-service"></a>Özel bağlantı hizmeti oluşturma

Bu bölümde, önceki adımda oluşturulan standart Azure Load Balancer kullanan bir özel bağlantı hizmeti oluşturun.

* [New-AzPrivateLinkServiceIpConfig](/powershell/module/az.network/new-azprivatelinkserviceipconfig)ile özel bağlantı hizmeti IP yapılandırması oluşturun.

* [New-AzPrivateLinkService](/powershell/module/az.network/new-azprivatelinkservice)ile özel bağlantı hizmeti oluşturun.

```azurepowershell
## Place the virtual network into a variable. ##
$vnet = Get-AzVirtualNetwork -Name 'myVNet' -ResourceGroupName 'CreatePrivLinkService-rg'

## Create the IP configuration for the private link service. ##
$ipsettings = @{
    Name = 'myIPconfig'
    PrivateIpAddress = '10.1.0.5'
    Subnet = $vnet.subnets[0]
}
$ipconfig = New-AzPrivateLinkServiceIpConfig @ipsettings

## Place the load balancer frontend configuration into a variable. ##
$fe = Get-AzLoadBalancer -Name 'myLoadBalancer' | Get-AzLoadBalancerFrontendIpConfig

## Create the private link service for the load balancer. ##
$privlinksettings = @{
    Name = 'myPrivateLinkService'
    ResourceGroupName = 'CreatePrivLinkService-rg'
    Location = 'eastus2'
    LoadBalancerFrontendIpConfiguration = $fe
    IpConfiguration = $ipconfig
}
New-AzPrivateLinkService @privlinksettings
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve kalan kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name 'CreatePrivLinkService-rg'
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Bir sanal ağ ve dahili Azure Load Balancer oluşturulur.
* Özel bir bağlantı hizmeti oluşturuldu

Azure özel uç noktası hakkında daha fazla bilgi edinmek için devam edin:
> [!div class="nextstepaction"]
> [Hızlı başlangıç: Azure PowerShell kullanarak özel uç nokta oluşturma](create-private-endpoint-powershell.md)

