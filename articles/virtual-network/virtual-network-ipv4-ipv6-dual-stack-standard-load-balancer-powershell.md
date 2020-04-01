---
title: IPv6 çift yığın uygulamasını dağıtın - Standart Yük Dengeleyicisi - PowerShell
titlesuffix: Azure Virtual Network
description: Bu makalede, Azure Powershell kullanarak Azure sanal ağında Standart Yük Dengeleyicisi ile bir IPv6 çift yığın uygulamasınasıl dağıtılır gösterilmektedir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2020
ms.author: kumud
ms.openlocfilehash: d6b61e27324220fc78ace3e964aed98f9ba114d3
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420942"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---powershell"></a>Azure'da Bir IPv6 çift yığın uygulaması dağıtma - PowerShell

Bu makalede, Azure'da çift yığınlı sanal ağ ve alt ağ, çift (IPv4 + IPv6) ön uç yapılandırmalı standart yük dengeleyicisi, çift IP yapılandırmasına sahip NIC'lere sahip VM'ler, ağ güvenlik grubu ve genel IP'ler içeren Standart Yük Dengeleyicisi'ni kullanarak bir çift yığın (IPv4 + IPv6) uygulamasını nasıl dağıtabileceğiniz gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 6.9.0 veya sonrası gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Çift yığınlı sanal ağınızı oluşturmadan önce, [Yeni-AzResourceGroup'a](/powershell/module/az.resources/new-azresourcegroup)sahip bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *doğu ABD'de* yer *myRGDualStack* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
   $rg = New-AzResourceGroup `
  -ResourceGroupName "dsRG1"  `
  -Location "east us"
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses"></a>IPv4 ve IPv6 genel IP adresleri oluşturma
Sanal makinelerinize Internet'ten erişmek için yük dengeleyicisi için IPv4 ve IPv6 genel IP adreslerine ihtiyacınız vardır. [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)ile ortak IP adresleri oluşturun. Aşağıdaki örnek, *dsRG1* kaynak grubunda *dsPublicIP_v4* ve *dsPublicIP_v6* adlı IPv4 ve IPv6 ortak IP adresini oluşturur:

```azurepowershell-interactive
$PublicIP_v4 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v4" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Static `
  -IpAddressVersion IPv4 `
  -Sku Standard
  
$PublicIP_v6 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Static `
  -IpAddressVersion IPv6 `
  -Sku Standard
```
RDP bağlantısını kullanarak sanal makinelerinize erişmek için [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)ile sanal makineler için bir IPV4 genel IP adresi oluşturun.

```azurepowershell-interactive
  $RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Static `
  -Sku Standard `
  -IpAddressVersion IPv4
  
  $RdpPublicIP_2 = New-AzPublicIpAddress `
   -Name "RdpPublicIP_2" `
   -ResourceGroupName $rg.ResourceGroupName `
   -Location $rg.Location  `
   -AllocationMethod Static `
   -Sku Standard `
   -IpAddressVersion IPv4
```

## <a name="create-standard-load-balancer"></a>Standart Load Balancer oluşturma

Bu bölümde, çift ön uç IP (IPv4 ve IPv6) ve yük dengeleyicisi için arka uç adresi havuzunu yapılandırıp bir Standart Yük Dengeleyicisi oluşturursunuz.

### <a name="create-front-end-ip"></a>Ön uç IP'si oluşturma

[New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig)ile bir ön uç IP oluşturun. Aşağıdaki örnek, *dsLbFrontEnd_v4* ve *dsLbFrontEnd_v6*adlı IPv4 ve IPv6 frontend IP yapılandırmaları oluşturur:

```azurepowershell-interactive
$frontendIPv4 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v4" `
  -PublicIpAddress $PublicIP_v4

$frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6

```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzunda yapılandırma

[New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig)ile bir arka uç adres havuzu oluşturun. VM'ler kalan adımlardaki bu arka uç havuzuna eklenir. Aşağıdaki örnek, *dsLbBackEndPool_v4* adlı arka uç adres havuzları oluşturur ve hem IPV4 hem de IPv6 NIC yapılandırmalarına sahip VM'leri içerecek *dsLbBackEndPool_v6:*

```azurepowershell-interactive
$backendPoolv4 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v4"

$backendPoolv6 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v6"
```
### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
VM'lerin durumunu izlemek için bir sistem durumu sondası oluşturmak için [Add-AzLoadBalancerProbeConfig'i](/powershell/module/az.network/add-azloadbalancerprobeconfig) kullanın.
```azurepowershell
$probe = New-AzLoadBalancerProbeConfig -Name MyProbe -Protocol tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
```
### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca sağlıklı VM'lerin trafik aldığından emin olmak için isteğe bağlı olarak bir sistem durumu sondası tanımlayabilirsiniz. Temel yük dengeleyicisi, VM'lerde hem IPv4 hem de IPv6 uç noktalarının durumunu değerlendirmek için bir IPv4 prob kullanır. Standart yük dengeleyicisi açıkça IPv6 sistem sondaları için destek içerir.

[Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig)ile bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek, *dsLBrule_v4* ve *dsLBrule_v6* adlı yük dengeleyici kuralları oluşturur ve *TCP* bağlantı noktası *80'deki* trafiği IPv4 ve IPv6 frontend IP yapılandırmalarına dengeler:

```azurepowershell-interactive
$lbrule_v4 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v4" `
  -FrontendIpConfiguration $frontendIPv4 `
  -BackendAddressPool $backendPoolv4 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
   -probe $probe

$lbrule_v6 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
   -probe $probe
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile Standart Yük Dengeleyicisi oluşturun. Aşağıdaki örnek, iPv4 ve IPv6 frontend IP yapılandırmalarını, arka uç havuzlarını ve önceki adımlarda oluşturduğunuz yük dengeleme kurallarını kullanarak *myLoadBalancer* adlı genel bir Standart Yük Dengeleyicisi oluşturur:

```azurepowershell-interactive
$lb = New-AzLoadBalancer `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "MyLoadBalancer" `
-Sku "Standard" `
-FrontendIpConfiguration $frontendIPv4,$frontendIPv6 `
-BackendAddressPool $backendPoolv4,$backendPoolv6 `
-LoadBalancingRule $lbrule_v4,$lbrule_v6

```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Bazı Sanal M'leri dağıtmadan ve bakiyecinizi sınamadan önce, kullanılabilirlik kümesi, ağ güvenlik grubu, sanal ağ ve sanal NIC'ler gibi destekleyici ağ kaynakları oluşturmanız gerekir. 
### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Uygulamanızın yüksek oranda kullanılabilir olmasını sağlamak için VM’lerinizi bir kullanılabilirlik kümesine yerleştirin.

[New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset)ile kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurepowershell-interactive
$avset = New-AzAvailabilitySet `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsAVset" `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2 `
  -Sku aligned
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

VNET'inizde gelen ve giden iletişimi yönetecek kurallar için bir ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile 3389 bağlantı noktası üzerinden RDP bağlantılarına izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurepowershell-interactive
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

```azurepowershell-interactive
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

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "dsNSG1"  `
-SecurityRules $rule1,$rule2
```
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek *mySubnet*ile *dsVnet* adlı bir sanal ağ oluşturur:

```azurepowershell-interactive
# Create dual stack subnet
$subnet = New-AzVirtualNetworkSubnetConfig `
-Name "dsSubnet" `
-AddressPrefix "10.0.0.0/24","ace:cab:deca:deed::/64"

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsVnet" `
  -AddressPrefix "10.0.0.0/16","ace:cab:deca::/48"  `
  -Subnet $subnet
```

### <a name="create-nics"></a>NIC’leri oluşturma

[New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)ile sanal NIC'ler oluşturun. Aşağıdaki örnek, Hem IPv4 hem de IPv6 yapılandırmalarına sahip iki sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC).

```azurepowershell-interactive
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_1
      
  $Ip6Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp6Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv6 `
    -LoadBalancerBackendAddressPool $backendPoolv6
    
  $NIC_1 = New-AzNetworkInterface `
    -Name "dsNIC1" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 
    
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_2  

  $NIC_2 = New-AzNetworkInterface `
    -Name "dsNIC2" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 

```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = get-credential -Message "DUAL STACK VNET SAMPLE:  Please enter the Administrator credential to log into the VMs."
```

Şimdi [Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturabilirsiniz. Aşağıdaki örnek, zaten yoksa iki VM ve gerekli sanal ağ bileşenleri oluşturur. 

```azurepowershell-interactive
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

## <a name="determine-ip-addresses-of-the-ipv4-and-ipv6-endpoints"></a>IPv4 ve IPv6 uç noktalarının IP adreslerini belirleme
Bu dağıtımda kullanılan IP'leri özetlemek için kaynak grubundaki `get-AzNetworkInterface`tüm Ağ Arabirimi Nesnelerini alın. Ayrıca, IPv4 ve IPv6 uç noktalarının Yük Dengeleyicisinin ön `get-AzpublicIpAddress`uç adreslerini .

```azurepowershell-interactive
$rgName= "dsRG1"
$NICsInRG= get-AzNetworkInterface -resourceGroupName $rgName 
write-host `nSummary of IPs in this Deployment: 
write-host ******************************************
foreach ($NIC in $NICsInRG) {
 
    $VMid= $NIC.virtualmachine.id 
    $VMnamebits= $VMid.split("/") 
    $VMname= $VMnamebits[($VMnamebits.count-1)] 
    write-host `nPrivate IP addresses for $VMname 
    $IPconfigsInNIC= $NIC.IPconfigurations 
    foreach ($IPconfig in $IPconfigsInNIC) {
 
        $IPaddress= $IPconfig.privateipaddress 
        write-host "    "$IPaddress 
        IF ($IPconfig.PublicIpAddress.ID) {
 
            $IDbits= ($IPconfig.PublicIpAddress.ID).split("/")
            $PipName= $IDbits[($IDbits.count-1)]
            $PipObject= get-azPublicIpAddress -name $PipName -resourceGroup $rgName
            write-host "    "RDP address:  $PipObject.IpAddress
                 }
         }
 }
 
 
 
  write-host `nPublic IP addresses on Load Balancer:
 
  (get-AzpublicIpAddress -resourcegroupname $rgName | where { $_.name -notlike "RdpPublicIP*" }).IpAddress
```
Aşağıdaki şekil, iki VM'nin özel IPv4 ve IPv6 adreslerini ve Yük Dengeleyicisinin ön uç IPv4 ve IPv6 IP adreslerini listeleyen bir örnek çıktısını gösterir.

![Azure'da çift yığın (IPv4/IPv6) uygulama dağıtımının IP özeti](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-application-summary.png)

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure portalında IPv6 çift yığın sanal ağı görüntüleyin
Azure portalındaki IPv6 çift yığın sanal ağını aşağıdaki gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğuna *dsVnet*girin.
2. **dsVnet** arama sonuçlarında göründüğünde, bunu seçin. Bu *dsVnet*adlı çift yığını sanal ağ **Genel Bakış** sayfası başlattı. Çift yığın sanal ağ *dsSubnet*adlı çift yığın alt ağında bulunan hem IPv4 ve IPv6 yapılandırmaları ile iki NIC gösterir.

  ![Azure'da IPv6 çift yığın sanal ağ](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name dsRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, çift frontend IP yapılandırmasına (IPv4 ve IPv6) sahip bir Standart Yük Dengeleyicisi oluşturdunuz. Ayrıca, yük dengeleyicisinin arka uç havuzuna eklenen çift IP yapılandırmalı (IPV4 + IPv6) NIC'leri içeren iki sanal makine oluşturdunuz. Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md) e bakın?
