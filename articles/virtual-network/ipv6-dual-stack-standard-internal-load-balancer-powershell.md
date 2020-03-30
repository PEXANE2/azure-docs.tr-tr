---
title: Azure'da Standart İç Yük Dengeleyicisi'ni kullanarak bir IPv6 çift yığın uygulaması dağıtma - PowerShell
titlesuffix: Azure Virtual Network
description: Bu makalede, Azure Powershell kullanarak Azure sanal ağında Standart İç Yük Dengeleyicisi ile bir IPv6 çift yığın uygulamasınasıl dağıtılanınca gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/14/2019
ms.author: kumud
ms.openlocfilehash: fdf726fd31e8b92a04a1c136eb5cd7110e0c6d5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72333371"
---
# <a name="deploy-an-ipv6-dual-stack-application-using-standard-internal-load-balancer-in-azure---powershell-preview"></a>Azure'da Standart İç Yük Dengeleyicisini kullanarak bir IPv6 çift yığın uygulaması dağıtma - PowerShell (Önizleme)

Bu makalede, Azure'da çift yığınlı sanal ağ ve alt ağ, çift (IPv4 + IPv6) ön uç yapılandırmaları olan standart dahili yük dengeleyicisi, çift IP'si olan NIC'ler içeren bir çift yığın (IPv4 + IPv6) uygulaması nasıl dağıtılabildiğinizi gösterir yapılandırma, ağ güvenlik grubu ve genel IP'ler.

> [!Important]
> Azure Sanal Ağı için IPv6 desteği şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IPv6 özellikli bir İç Yük Dengeleyicisi oluşturma [yordamı, burada](virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md)açıklanan Internet'e bakan bir IPv6 Yük Dengeleyicisi oluşturma işlemiyle hemen hemen aynıdır. Bir iç yük dengeleyici oluşturmak için tek fark aşağıdaki PowerShell örneğinde gösterildiği gibi ön uç yapılandırmavardır:

```azurepowershell
 $frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
 -Name "dsLbFrontEnd_v6" `
 -PrivateIpAddress "ace:cab:deca:deed::100" `
 -PrivateIpAddressVersion "IPv6" `
 -Subnet $DsSubnet
```

Yukarıdaki leri bir iç yük dengeleyici ön uç yapılandırması yapan değişiklikler şunlardır:
- "IPv6" `PrivateIpAddressVersion` olarak belirtilir
- Bağımsız `-PublicIpAddress` değişken atlandı veya 'ile `-PrivateIpAddress`değiştirildi. Özel adresin, dahili yük dengeleyicisinin dağıtılanacağı Subnet IP alanı aralığında olması gerektiğini unutmayın. Statik `-PrivateIpAddress` atlanırsa, bir sonraki ücretsiz IPv6 adresi iç yük Dengeleyicisinin dağıtıldığı alt ağdan seçilir.
- İç yük dengeleyicisinin dağıtılacayacağı olan çift `-Subnet` `-SubnetId` yığın alt ağı a veya bağımsız değişkenle belirtilir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 6.9.0 veya sonrası gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar
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

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Çift yığınlı sanal ağınızı oluşturmadan önce, [Yeni-AzResourceGroup'a](/powershell/module/az.resources/new-azresourcegroup)sahip bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *doğu ABD'deki* konumda *dsStd_ILB_RG* adlı bir kaynak grubu oluşturur:

```azurepowershell
$rg = New-AzResourceGroup `
  -ResourceGroupName "dsStd_ILB_RG"  `
  -Location "east us"
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses"></a>IPv4 ve IPv6 genel IP adresleri oluşturma
Sanal makinelerinize Internet'ten erişmek için VM'ler için IPv4 ve IPv6 genel IP adreslerine ihtiyacınız vardır. [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)ile ortak IP adresleri oluşturun. Aşağıdaki örnek, *dsStd_ILB_RG* kaynak grubunda *RdpPublicIP_1* ve *RdpPublicIP_2* adlı IPv4 ve IPv6 genel IP adresini oluşturur:

```azurepowershell
$RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Static `
  -IpAddressVersion IPv4  `
  -sku Standard
  
$RdpPublicIP_2 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_2" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Static `
  -IpAddressVersion IPv4  `
  -sku Standard
```
## <a name="create-the-virtual-network-and-the-subnet"></a>Sanal ağ ve alt ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) kullanarak bir sanal ağ oluşturun çift yığın bir alt net yapılandırma [new-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)kullanarak. Aşağıdaki örnek, *dsSubnet*ile *dsVnet* adlı bir sanal ağ oluşturur.

```azurepowershell
# Create dual stack subnet config
$DsSubnet = New-AzVirtualNetworkSubnetConfig `
  -Name "dsSubnet" `
  -AddressPrefix "10.0.0.0/24","ace:cab:deca:deed::/64"

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsVnet" `
  -AddressPrefix "10.0.0.0/16","ace:cab:deca::/48"  `
  -Subnet $DsSubnet

#Refresh the fully populated subnet for use in load balancer frontend configuration
$DsSubnet = get-AzVirtualNetworkSubnetconfig -name dsSubnet -VirtualNetwork $vnet
```
## <a name="create-standard-load-balancer"></a>Standart Load Balancer oluşturma

Bu bölümde, çift ön uç IP (IPv4 ve IPv6) ve yük dengeleyicisi için arka uç adresi havuzunu yapılandırıp bir Standart Yük Dengeleyicisi oluşturursunuz.

### <a name="create-front-end-ip"></a>Ön uç IP'si oluşturma

[New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig)ile bir ön uç IP oluşturun. Aşağıdaki örnek, *dsLbFrontEnd_v4* ve *dsLbFrontEnd_v6*adlı IPv4 ve IPv6 frontend IP yapılandırmaları oluşturur:

```azurepowershell
$frontendIPv4 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v4" `
  -PrivateIpAddress "10.0.0.100"  `
  -PrivateIpAddressVersion "IPv4"   `
  -Subnet $DsSubnet

$frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PrivateIpAddress "ace:cab:deca:deed::100"  `
  -PrivateIpAddressVersion "IPv6"   `
  -Subnet $DsSubnet

```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzunda yapılandırma

[New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig)ile bir arka uç adres havuzu oluşturun. VM'ler kalan adımlardaki bu arka uç havuzuna eklenir. Aşağıdaki örnek, *dsLbBackEndPool_v4* adlı arka uç adres havuzları oluşturur ve hem IPV4 hem de IPv6 NIC yapılandırmalarına sahip VM'leri içerecek *dsLbBackEndPool_v6:*

```azurepowershell
$backendPoolv4 = New-AzLoadBalancerBackendAddressPoolConfig -Name "dsLbBackEndPool_v4"

$backendPoolv6 = New-AzLoadBalancerBackendAddressPoolConfig -Name "dsLbBackEndPool_v6"
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca sağlıklı VM'lerin trafik aldığından emin olmak için isteğe bağlı olarak bir sistem durumu sondası tanımlayabilirsiniz. Temel yük dengeleyicisi, VM'lerde hem IPv4 hem de IPv6 uç noktalarının durumunu değerlendirmek için bir IPv4 prob kullanır. Standart yük dengeleyicisi açıkça IPv6 sistem sondaları için destek içerir.

[Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig)ile bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek, *dsLBrule_v4* ve *dsLBrule_v6* adlı yük dengeleyici kuralları oluşturur ve *TCP* bağlantı noktası *80'deki* trafiği IPv4 ve IPv6 frontend IP yapılandırmalarına dengeler:

```azurepowershell
$lbrule_v4 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v4" `
  -FrontendIpConfiguration $frontendIPv4 `
  -BackendAddressPool $backendPoolv4 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

$lbrule_v6 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile Standart Yük Dengeleyicisi oluşturun. Aşağıdaki örnek, iPv4 ve IPv6 frontend IP yapılandırmalarını, arka uç havuzlarını ve önceki adımlarda oluşturduğunuz yük dengeleme kurallarını kullanarak *myInternalLoadBalancer* adlı genel bir Standart Yük Dengeleyicisi oluşturur:

```azurepowershell
$lb = New-AzLoadBalancer  `
  -ResourceGroupName $rg.ResourceGroupName  `
  -Location $rg.Location  `
  -Name  "MyInternalLoadBalancer"  `
  -Sku "Standard"  `
  -FrontendIpConfiguration  $frontendIPv4,$frontendIPv6  `
  -BackendAddressPool  $backendPoolv4,$backendPoolv6  `
  -LoadBalancingRule  $lbrule_v4,$lbrule_v6

```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Bazı Sanal M'leri dağıtmadan ve bakiyecinizi sınamadan önce, kullanılabilirlik kümesi, ağ güvenlik grubu ve sanal NIC'ler gibi destekleyici ağ kaynakları oluşturmanız gerekir. 

### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Uygulamanızın yüksek kullanılabilirliğini artırmak için, VM'lerinizi bir kullanılabilirlik kümesine yerleştirin.

[New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset)ile kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek *dsAVset*adlı bir kullanılabilirlik kümesi oluşturur:

```azurepowershell
$avset = New-AzAvailabilitySet `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsAVset" `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2 `
  -Sku aligned
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

VNet'inizde gelen ve giden iletişimi yönetecek kurallar için bir ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile 3389 bağlantı noktası üzerinden RDP bağlantılarına izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurepowershell
$rule1 = New-AzNetworkSecurityRuleConfig `
  -Name 'myNetworkSecurityGroupRuleRDP' `
  -Description 'Allow RDP' `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389
```
#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bağlantı noktası 80 üzerinden internet bağlantılarına izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurepowershell
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name 'myNetworkSecurityGroupRuleHTTP' `
  -Description 'Allow HTTP' `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange 80 `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80
```
#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun.

```azurepowershell
$nsg = New-AzNetworkSecurityGroup `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsNSG1"  `
  -SecurityRules $rule1,$rule2
```
### <a name="create-nics"></a>NIC’leri oluşturma

[New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)ile sanal NIC'ler oluşturun. Aşağıdaki örnek, Hem IPv4 hem de IPv6 yapılandırmalarına sahip iki sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC).

```azurepowershell

# Create the IPv4 configuration for NIC 1
$Ip4Config=New-AzNetworkInterfaceIpConfig `
  -Name dsIp4Config `
  -Subnet $vnet.subnets[0] `
  -PrivateIpAddressVersion IPv4 `
  -LoadBalancerBackendAddressPool $backendPoolv4 `
  -PublicIpAddress  $RdpPublicIP_1

# Create the IPv6 configuration
$Ip6Config=New-AzNetworkInterfaceIpConfig `
  -Name dsIp6Config `
  -Subnet $vnet.subnets[0] `
  -PrivateIpAddressVersion IPv6 `
  -LoadBalancerBackendAddressPool $backendPoolv6

# Create NIC 1
$NIC_1 = New-AzNetworkInterface `
  -Name "dsNIC1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -NetworkSecurityGroupId $nsg.Id `
  -IpConfiguration $Ip4Config,$Ip6Config

# Create the IPv4 configuration for NIC 2
$Ip4Config=New-AzNetworkInterfaceIpConfig `
  -Name dsIp4Config `
  -Subnet $vnet.subnets[0] `
  -PrivateIpAddressVersion IPv4 `
  -LoadBalancerBackendAddressPool $backendPoolv4 `
  -PublicIpAddress  $RdpPublicIP_2

# Create NIC 2 reusing the IPv6 configuration from NIC 1
$NIC_2 = New-AzNetworkInterface `
  -Name "dsNIC2" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -NetworkSecurityGroupId $nsg.Id `
  -IpConfiguration $Ip4Config,$Ip6Config

```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell
$cred = get-credential -Message "DUAL STACK VNET SAMPLE:  Please enter the Administrator credential to log into the VM's"
```

Şimdi [Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturabilirsiniz. Aşağıdaki örnek, zaten yoksa iki VM ve gerekli sanal ağ bileşenleri oluşturur.

```azurepowershell
$vmsize = "Standard_A2"
$ImagePublisher = "MicrosoftWindowsServer"
$imageOffer = "WindowsServer"
$imageSKU = "2019-Datacenter"

$vmName= "dsVM1"
$VMconfig1 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_1.Id  3> $null
$VM1 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig1


$vmName= "dsVM2"
$VMconfig2 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_2.Id  3> $null
$VM2 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig2
```
## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure portalında IPv6 çift yığın sanal ağı görüntüleyin
Azure portalındaki IPv6 çift yığın sanal ağını aşağıdaki gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğuna *dsVnet*girin.
2. **dsVnet** arama sonuçlarında göründüğünde, bunu seçin. Bu *dsVnet*adlı çift yığını sanal ağ **Genel Bakış** sayfası başlattı. Çift yığın sanal ağ *dsSubnet*adlı çift yığın alt ağında bulunan hem IPv4 ve IPv6 yapılandırmaları ile iki NIC gösterir.

![Standart İç Yük Dengeleyicili IPv6 Çift Yığın Sanal Ağ](./media/ipv6-dual-stack-standard-internal-load-balancer-powershell/ipv6-dual-stack-virtual-network.png)


> [!NOTE]
> Azure için IPv6 sanal ağı, bu önizleme sürümü için salt okunur olarak Azure portalında kullanılabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell
Remove-AzResourceGroup -Name dsStd_ILB_RG
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çift frontend IP yapılandırmasına (IPv4 ve IPv6) sahip bir Standart Yük Dengeleyicisi oluşturdunuz. Ayrıca, yük dengeleyicisinin arka uç havuzuna eklenen çift IP yapılandırmalı (IPV4 + IPv6) NIC'leri içeren iki sanal makine oluşturdunuz. Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md) e bakın?