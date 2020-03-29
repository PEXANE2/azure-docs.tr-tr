---
title: Azure Sanal Ağ'da IPv6'ya bir IPv4 uygulamasını yükseltme - PowerShell
titlesuffix: Azure Virtual Network
description: Bu makalede, IPv6 adreslerinin Azure Powershell kullanarak Azure sanal ağındaki varolan bir uygulamaya nasıl dağıtılanıncaya kadar dağıtılanın.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/21/2019
ms.author: kumud
ms.openlocfilehash: d08ce1c382d173ac98a0e61e6117ed50b958ba44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76119848"
---
# <a name="upgrade-an-ipv4-application-to-ipv6-in-azure-virtual-network---powershell-preview"></a>Azure sanal ağında IPv6'ya bir IPv4 uygulamasını yükseltme - PowerShell (Önizleme)

Bu makalede, Standart Yük Dengeleyicisi ve Genel IP'si olan bir Azure sanal ağındaki mevcut bir IPv4 uygulamasına IPv6 bağlantısının nasıl eklendirilebildiğini gösterilmektedir. Yerinde yükseltme şunları içerir:
- Sanal ağ ve alt ağ için IPv6 adres alanı
- Hem IPv4 hem de IPV6 ön uç konfigürasyonlarına sahip standart yük dengeleyicisi
- Hem IPv4 + IPv6 yapılandırması olan NIC'li VM'ler
- IPv6 Genel IP böylece yük dengeleyici Internet'e bakan IPv6 bağlantısı vardır

> [!Important]
> Azure Sanal Ağı için IPv6 desteği şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 6.9.0 veya sonrası gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar

### <a name="register-the-service"></a>Hizmeti kaydedin

Azure'da bir çift yığın uygulaması dağıtmadan önce, aboneliğinizi bu önizleme özelliği için aşağıdaki Azure PowerShell'i kullanarak yapılandırmanız gerekir:

Aşağıdaki gibi kaydolun:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
Register-AzProviderFeature -FeatureName AllowIPv6CAOnStandardLB -ProviderNamespace Microsoft.Network
```
Özellik kaydının tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure PowerShell komutunu çalıştırarak kayıt durumunuzu kontrol edebilirsiniz: Kaydı aşağıdaki gibi kontrol edin:
```azurepowershell
Get-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
Get-AzProviderFeature -FeatureName AllowIPv6CAOnStandardLB -ProviderNamespace Microsoft.Network
```
Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

### <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma
Bu makalede, [Quickstart: Create a Standard Load Balancer - Azure PowerShell'de](../load-balancer/quickstart-create-standard-load-balancer-powershell.md)açıklandığı gibi bir Standart Yük Dengeleyicisi dağıtıldığınızı varsayar.

## <a name="retrieve-the-resource-group"></a>Kaynak grubunu alma

Çift yığınlı sanal ağınızı oluşturmadan önce [Get-AzResourceGroup](/powershell/module/az.resources/get-azresourcegroup)ile kaynak grubunu almanız gerekir.

```azurepowershell
 $rg = Get-AzResourceGroup  -ResourceGroupName "myResourceGroupSLB"
```

## <a name="create-an-ipv6-ip-addresses"></a>IPv6 IP adresleri oluşturma

Standart Yük Dengeleyiciniz için [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) ile ortak bir IPv6 adresi oluşturun. Aşağıdaki örnek, *myResourceGroupSLB* kaynak grubunda *PublicIP_v6* adlı bir IPv6 genel IP adresi oluşturur:

```azurepowershell
  
  $PublicIP_v6 = New-AzPublicIpAddress `
  -Name "PublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Sku Standard  `
  -AllocationMethod Static `
  -IpAddressVersion IPv6
```

## <a name="configure-load-balancer-frontend"></a>Yük dengeleyici ön ucunu yapılandır

Varolan yük dengeleyici yapılandırmasını alın ve [Add-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/Add-AzLoadBalancerFrontendIpConfig) kullanarak yeni IPv6 IP adresini aşağıdaki gibi ekleyin:

```azurepowershell
# Retrieve the load balancer configuration
$lb = Get-AzLoadBalancer -ResourceGroupName $rg.ResourceGroupName -Name "MyLoadBalancer"
# Add IPv6 components to the local copy of the load balancer configuration
$lb | Add-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6
#Update the running load balancer with the new frontend
$lb | Set-AzLoadBalancer
```

## <a name="configure-load-balancer-backend-pool"></a>Yük dengeleyici arka uç havuzuyapıla

Yük dengeleyici yapılandırmasının yerel kopyasında arka uç havuzunu oluşturun ve çalışan yük dengeleyicisini yeni arka uç havuzu yapılandırmasıyla aşağıdaki gibi güncelleştirin:

```azurepowershell
$lb | Add-AzLoadBalancerBackendAddressPoolConfig -Name "LbBackEndPool_v6"
# Update the running load balancer with the new backend pool
$lb | Set-AzLoadBalancer
```

## <a name="configure-load-balancer-rules"></a>Yük dengeleyici kurallarını yapılandırma
Varolan yük dengeleyici ön uç ve arka uç havuzu yapılandırmasını alın ve [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/Add-AzLoadBalancerRuleConfig)kullanarak yeni yük dengeleme kuralları ekleyin.

```azurepowershell
# Retrieve the updated (live) versions of the frontend and backend pool
$frontendIPv6 = Get-AzLoadBalancerFrontendIpConfig -Name "dsLbFrontEnd_v6" -LoadBalancer $lb
$backendPoolv6 = Get-AzLoadBalancerBackendAddressPoolConfig -Name "LbBackEndPool_v6" -LoadBalancer $lb
# Create new LB rule with the frontend and backend
$lb | Add-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
#Finalize all the load balancer updates on the running load balancer
$lb | Set-AzLoadBalancer
```
## <a name="add-ipv6-address-ranges"></a>IPv6 adres aralıkları ekleme

Sanal ağa ve VM'leri barındıran alt ağa IPv6 adres aralıklarını aşağıdaki gibi ekleyin:

```azurepowershell
#Add IPv6 ranges to the VNET and subnet
#Retreive the VNET object
$vnet = Get-AzVirtualNetwork  -ResourceGroupName $rg.ResourceGroupName -Name "myVnet" 
#Add IPv6 prefix to the VNET
$vnet.addressspace.addressprefixes.add("ace:cab:deca::/48")
#Update the running VNET
$vnet |  Set-AzVirtualNetwork

#Retrieve the subnet object from the local copy of the VNET
$subnet= $vnet.subnets[0]
#Add IPv6 prefix to the Subnet (subnet of the VNET prefix, of course)
$subnet.addressprefix.add("ace:cab:deca::/64")
#Update the running VNET with the new subnet configuration
$vnet |  Set-AzVirtualNetwork

```
## <a name="add-ipv6-configuration-to-nic"></a>NIC'ye IPv6 yapılandırması ekleme

[Add-AzNetworkInterfaceIpConfig](/powershell/module/az.network/Add-AzNetworkInterfaceIpConfig) kullanarak tüm VM NIC'lerini IPv6 adresiyle yapılandırın:

```azurepowershell

#Retrieve the NIC objects
$NIC_1 = Get-AzNetworkInterface -Name "myNic1" -ResourceGroupName $rg.ResourceGroupName
$NIC_2 = Get-AzNetworkInterface -Name "myNic2" -ResourceGroupName $rg.ResourceGroupName
$NIC_3 = Get-AzNetworkInterface -Name "myNic3" -ResourceGroupName $rg.ResourceGroupName
#Add an IPv6 IPconfig to NIC_1 and update the NIC on the running VM
$NIC_1 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_1 | Set-AzNetworkInterface
#Add an IPv6 IPconfig to NIC_2 and update the NIC on the running VM
$NIC_2 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_2 | Set-AzNetworkInterface
#Add an IPv6 IPconfig to NIC_3 and update the NIC on the running VM
$NIC_3 | Add-AzNetworkInterfaceIpConfig -Name MyIPv6Config -Subnet $vnet.Subnets[0]  -PrivateIpAddressVersion IPv6 -LoadBalancerBackendAddressPool $backendPoolv6 
$NIC_3 | Set-AzNetworkInterface

```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure portalında IPv6 çift yığın sanal ağı görüntüleyin
Azure portalındaki IPv6 çift yığın sanal ağını aşağıdaki gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğuna *myVnet*girin.
2. **myVnet** arama sonuçlarında göründüğünde, onu seçin. Bu *myVNet*adlı çift yığını sanal ağın **Genel Bakış** sayfası başlattı. Çift yığın sanal ağ *mySubnet*adlı çift yığın alt ağında bulunan hem IPv4 ve IPv6 yapılandırmaları ile üç NIC gösterir.

  ![Azure'da IPv6 çift yığın sanal ağ](./media/ipv6-add-to-existing-vnet-powershell/ipv6-dual-stack-vnet.png)

> [!NOTE]
> Azure için IPv6 sanal ağı, bu önizleme sürümü için salt okunur olarak Azure portalında kullanılabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyAzureResourceGroupSLB
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, iPv4 frontend IP yapılandırması ile varolan bir Standart Yük Dengeleyicisini çift yığın (IPv4 ve IPv6) yapılandırmasına güncelleştirmişsiniz. Ayrıca, arka uç havuzundaki VM'lerin NIC'lerine ve bunları barındıran Sanal Ağa IPv6 yapılandırmaları eklediniz. Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md) e bakın?
