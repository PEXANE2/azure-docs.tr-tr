---
title: 'Hızlı başlangıç: ortak yük dengeleyici oluşturma-Azure PowerShell'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta Azure PowerShell kullanarak yük dengeleyici oluşturma gösterilmektedir
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2020
ms.author: allensu
ms:custom: seodec18
ms.openlocfilehash: b8a95687b1567eb6e063ccc871a4a130c5f2db69
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290355"
---
# <a name="quickstart-create-a-public-load-balancer-to-load-balance-vms-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak VM 'Lerin yükünü dengelemek için ortak yük dengeleyici oluşturma

Ortak yük dengeleyici ve üç sanal makine oluşturmak için Azure PowerShell kullanarak Azure Load Balancer kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure PowerShell yerel olarak veya Azure Cloud Shell yüklendi

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için öğesini de çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun:

* **Myresourcegrouplb**adlı adlandırılmış.
* **Eastus** konumunda.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'

New-AzResourceGroup -Name $rg -Location $loc
```
---

# <a name="option-1-default-create-a-load-balancer-standard-sku"></a>[Seçenek 1 (varsayılan): yük dengeleyici oluşturma (Standart SKU)](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. 

[New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu şu şekilde kullanın:

* **Mypublicıp**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIP'
$sku = 'Standard'
$all = 'static'

$publicIp = 
New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $all -SKU $sku
```

Bölge 1 ' de genel bir IP adresi oluşturmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIP'
$sku = 'Standard'
$all = 'static'

$publicIp = 
New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $all -SKU $sku -zone 1
```

## <a name="create-standard-load-balancer"></a>Standart yük dengeleyici oluştur

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-frontend-ip"></a>Ön uç IP oluşturma

[New-Azloadbalancerfrontendıpconfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig)ile bir ön uç IP 'si oluşturun:

* **Myön uç**adı.
* Genel IP **Mypublicıp**'ye eklendi.

```azurepowershell-interactive
## Variables for the commands ##
$fe = 'myFrontEnd'
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIP'

$publicIp = 
Get-AzPublicIpAddress -Name $pubIP -ResourceGroupName $rg

$feip = 
New-AzLoadBalancerFrontendIpConfig -Name $fe -PublicIpAddress $publicIp
```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzunu yapılandırma

[New-Azloadbalancerbackendadddresspoolconfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig)ile arka uç adres havuzu oluşturun: 

* **Mybackendpool**adlı adlandırılmış.
* VM 'Ler, kalan adımlarda bu arka uç havuzuna ekler.

```azurepowershell-interactive
## Variable for the command ##
$be = 'myBackEndPool'

$bepool = 
New-AzLoadBalancerBackendAddressPoolConfig -Name $be
```

### <a name="create-the-health-probe"></a>Durum araştırması oluşturma

Bir sistem durumu araştırması, tüm sanal makine örneklerini denetleyerek ağ trafiği gönderebilecekleri emin olmanızı sağlar. 

Başarısız araştırma denetimine sahip bir sanal makine yük dengeleyiciden kaldırılır. Hata çözüldüğünde sanal makine yük dengeleyiciye geri eklenir.

[Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig)ile bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurepowershell-interactive
## Variables for the command ##
$hp = 'myHealthProbe'
$pro = 'http'
$port = '80'
$int = '360'
$cnt = '5'

$probe = 
New-AzLoadBalancerProbeConfig -Name $hp -Protocol $pro -Port $port -RequestPath / -IntervalInSeconds $int -ProbeCount $cnt
```

### <a name="create-the-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı şunları tanımlar:

* Gelen trafik için ön uç IP yapılandırması.
* Trafiği almak için arka uç IP Havuzu.
* Gerekli kaynak ve hedef bağlantı noktası. 

[Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig)ile bir yük dengeleyici kuralı oluşturun: 

* Adlandırılmış **Myhttprule**
* Ön **uç**havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını**kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması**kullanılıyor.
* Protokol **TCP**.

```azurepowershell-interactive
## Variables for the command ##
$lbr = 'myHTTPRule'
$pro = 'tcp'
$port = '80'

## $feip and $bePool are the variables from previous steps. ##

$rule = 
New-AzLoadBalancerRuleConfig -Name $lbr -Protocol $pro -Probe $probe -FrontendPort $port -BackendPort $port -FrontendIpConfiguration $feip -BackendAddressPool $bePool -DisableOutboundSNAT
```

### <a name="create-load-balancer-resource"></a>Yük dengeleyici kaynağı oluştur

[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile ortak yük dengeleyici oluşturma:

* Adlandırılmış **Myloadbalancer**
* **Eastus**içinde.
* Kaynak grubu **Myresourcegrouplb**.

```azurepowershell-interactive
## Variables for the command ##
$lbn = 'myLoadBalancer'
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$sku = 'Standard'

## $feip, $bepool, $probe, $rule are variables with configuration information from previous steps. ##

$lb = 
New-AzLoadBalancer -ResourceGroupName $rg -Name $lbn -SKU $sku -Location $loc -FrontendIpConfiguration $feip -BackendAddressPool $bepool -Probe $probe -LoadBalancingRule $rule
```

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyicinizi test etmeden önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile bir sanal ağ oluşturun:

* **Myvnet**adında.
* Kaynak grubu **Myresourcegrouplb**.
* **Mybackendsubnet**adlı alt ağ.
* **10.0.0.0/16**sanal ağı.
* Alt ağ **10.0.0.0/24**.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$sub = 'myBackendSubnet'
$spfx = '10.0.0.0/24'
$vnm = 'myVNet'
$vpfx = '10.0.0.0/16'

## Create subnet config ##
$subnetConfig = 
New-AzVirtualNetworkSubnetConfig -Name $sub -AddressPrefix $spfx

## Create the virtual network ##
$vnet = 
New-AzVirtualNetwork -ResourceGroupName $rg -Location $loc -Name $vnm -AddressPrefix $vpfx -Subnet $subnetConfig
```
### <a name="create-public-ip-addresses-for-the-vms"></a>VM 'Ler için genel IP adresleri oluşturma

Bir RDP bağlantısı kullanarak sanal makinelerinize erişmek için VM 'Ler için genel IP adreslerine ihtiyacınız vardır. 

İçin standart genel IP adresleri oluşturmak üzere [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) kullanın:

#### <a name="vm1"></a>VM1

* **MyVMPubIP1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip1 = 'myVMPubIP1'
$sku = 'Standard'
$all = 'static'

$RdpPubIP1 = 
New-AzPublicIpAddress -Name $ip1 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

#### <a name="vm2"></a>VM2

* **MyVMPubIP2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma. 

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip2 = 'myVMPubIP2'
$sku = 'Standard'
$all = 'static'

$RdpPubIP2 = 
New-AzPublicIpAddress -Name $ip2 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

#### <a name="vm3"></a>VM3

* **MyVMPubIP3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma. 

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip3 = 'myVMPubIP3'
$sku = 'Standard'
$all = 'static'

$RdpPubIP3 = 
New-AzPublicIpAddress -Name $ip3 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Sanal ağınıza gelen bağlantıları tanımlamak için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir ağ güvenlik grubu oluşturun:

* **Mynsgrulerdp**adında.
* RDP 'ye **Izin ver**açıklaması.
* **Allow**erişimi.
* Protokol **TCP**.
* Yön **gelen**.
* Öncelik **1000**.
* **Internet**kaynağı.
* Kaynak bağlantı noktası aralığı **(*)**.
* Hedef adres ön eki **(*)**.
* Hedef **bağlantı noktası 3389**.

```azurepowershell-interactive
## Variables for command ##
$rnm = 'myNSGRuleRDP'
$des = 'Allow RDP'
$acc = 'Allow'
$pro = 'Tcp'
$dir = 'Inbound'
$pri = '1000'
$spfx = 'Internet'
$spr = '*'
$dpfx = '*'
$dpr = '3389'

$rule1 = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $des -Access $acc -Protocol $pro -Direction $dir -Priority $pri -SourceAddressPrefix $spfx -SourcePortRange $spr -DestinationAddressPrefix $dpfx -DestinationPortRange $dpr
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp**adında.
* HTTP 'ye **Izin ver**açıklaması.
* **Allow**erişimi.
* Protokol **TCP**.
* Yön **gelen**.
* Öncelik **2000**.
* **Internet**kaynağı.
* Kaynak bağlantı noktası aralığı **(*)**.
* Hedef adres ön eki **(*)**.
* Hedef **bağlantı noktası 80**.

```azurepowershell-interactive
## Variables for command ##
$rnm = 'myNSGRuleHTTP'
$des = 'Allow HTTP'
$acc = 'Allow'
$pro = 'Tcp'
$dir = 'Inbound'
$pri = '2000'
$spfx = 'Internet'
$spr = '*'
$dpfx = '*'
$dpr = '80'

$rule2 = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $des -Access $acc -Protocol $pro -Direction $dir -Priority $pri -SourceAddressPrefix $spfx -SourcePortRange $spr -DestinationAddressPrefix $dpfx -DestinationPortRange $dpr
```

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun:

* **Mynsg**adlı adlandırılmış.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Bir değişkende depolanan önceki adımlarda oluşturulan güvenlik kuralları.

```azurepowershell
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nmn = 'myNSG'

## $rule1 and $rule2 are variables with configuration information from the previous steps. ##
$nsg = 
New-AzNetworkSecurityGroup -ResourceGroupName $rg -Location $loc -Name $nmn -SecurityRules $rule1,$rule2
```

### <a name="create-network-interfaces"></a>Ağ arabirimlerini oluşturma

[New-Aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface)ile üç ağ arabirimi oluşturun:

#### <a name="vm-1"></a>VM 1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic1 = 'myNicVM1'
$vnt = 'myVNet'
$ip1 = 'myVMPubIP1'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM1 ##
$pub1 = 
Get-AzPublicIPAddress -Name $ip1 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM1 ##
$nicVM1 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic1 -PublicIpAddress $pub1 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```

#### <a name="vm-2"></a>VM 2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic2 = 'myNicVM2'
$vnt = 'myVNet'
$ip2 = 'myVMPubIP2'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM2 ##
$pub2 = 
Get-AzPublicIPAddress -Name $ip2 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM2 ##
$nicVM2 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic2 -PublicIpAddress $pub2 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```

#### <a name="vm-3"></a>VM 3

* **MyNicVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic3 = 'myNicVM3'
$vnt = 'myVNet'
$ip3 = 'myVMPubIP3'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM3 ##
$pub3 = 
Get-AzPublicIPAddress -Name $ip3 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM3 ##
$nicVM3 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic3 -PublicIpAddress $pub3 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```
### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell
$cred = Get-Credential
```

Sanal makineleri şu şekilde oluşturun:

* [New-AzVM](/powershell/module/az.compute/new-azvm)
* [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)
* [Set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem)
* [Set-Azvmsourceımage](/powershell/module/az.compute/set-azvmsourceimage)
* [Add-Azvmnetworkınterface](/powershell/module/az.compute/add-azvmnetworkinterface)


#### <a name="vm1"></a>VM1

* **MyVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM1**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Bölge 1**.
* **Eastus** konumunda.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM1'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$zn = '1'
$loc = 'eastus'

## Create a virtual machine configuration. $cred and $nicVM1 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM1.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Zone $zn -Location $loc -VM $vmConfig
```


#### <a name="vm2"></a>VM2

* **MyVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM2**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Bölge 2**.
* **Eastus** konumunda.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM2'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$zn = '2'
$loc = 'eastus'

## Create a virtual machine configuration. $cred and $nicVM2 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM2.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Zone $zn -Location $loc -VM $vmConfig
```

#### <a name="vm3"></a>VM3

* **MyVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM3**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Bölge 3**.
* **Eastus** konumunda.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM3'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$zn = '3'
$loc = 'eastus'

## Create a virtual machine configuration. $cred and $nicVM3 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM3.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Zone $zn -Location $loc -VM $vmConfig
```

Üç VM 'yi oluşturmak ve yapılandırmak birkaç dakika sürer.

## <a name="create-outbound-rule-configuration"></a>Giden kuralı yapılandırması oluştur
Yük dengeleyici giden kuralları arka uç havuzundaki VM 'Ler için giden kaynak ağ adresi çevirisini (SNAT) yapılandırır. 

Giden bağlantılar hakkında daha fazla bilgi için bkz. [Azure 'Da giden bağlantılar](load-balancer-outbound-connections.md).

### <a name="create-outbound-public-ip-address"></a>Giden genel IP adresi oluştur

[New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu şu şekilde kullanın:

* **Mypublicıpoıb Utbağlanmadı**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIPOutbound'
$sku = 'Standard'
$all = 'static'

$publicIp = 
New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $all -SKU $sku
```

Bölge 1 ' de genel bir IP adresi oluşturmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIPOutbound'
$sku = 'Standard'
$all = 'static'

$publicIp = 
New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $all -SKU $sku -zone 1
```
### <a name="create-outbound-frontend-ip-configuration"></a>Giden ön uç IP yapılandırması oluştur

[Add-Azloadbalancerfrontendıpconfig](/powershell/module/az.network/add-azloadbalancerfrontendipconfig)ile yeni bir ön uç IP yapılandırması oluşturun:

* **MyFrontEndOutbound**adlı.
* Genel IP adresiyle ilişkili **Mypublicıpoıb Utbağlanmadı**.

```azurepowershell-interactive
## Variables for the command ##
$fen = 'myFrontEndOutbound'

## Get the load balancer configuration  and apply the frontend config##
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg | Add-AzLoadBalancerFrontendIPConfig -Name $fen -PublicIpAddress $publicIP | Set-AzLoadBalancer
```

### <a name="create-outbound-pool"></a>Giden Havuz oluştur

[Add-Azloadbalancerbackendadddresspoolconfig](/powershell/module/az.network/add-azloadbalancerbackendaddresspoolconfig)ile yeni bir giden havuz oluşturun. 

[Set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer)ile yük dengeleyicisine havuzu ve ön uç IP adresini uygulayın:

* **Mybackendpooloutbound**adlı adlandırılmış.

```azurepowershell-interactive
## Variables for the command ##
$ben = 'myBackEndPoolOutbound'
$lbn = 'myLoadBalancer'
$rg = 'myResourceGroupLB'

## Get the load balancer configuration and create the outbound backend address pool##
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg | Add-AzLoadBalancerBackendAddressPoolConfig -Name $ben | Set-AzLoadBalancer
```
### <a name="create-outbound-rule-and-apply-to-load-balancer"></a>Giden kuralı oluştur ve yük dengeleyiciye Uygula

[Add-AzLoadBalancerOutboundRuleConfig](/powershell/module/az.network/new-azloadbalanceroutboundruleconfig)ile giden arka uç havuzu için yeni bir giden kuralı oluşturun. 

[Set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer)ile kuralı yük dengeleyiciye uygulayın:

* Adlandırılmış **mbir Boundrule**.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.
* Ön uç **myFrontEndOutbound**ile ilişkili.
* **Tüm**protokol.
* **15**' in boşta kalma süresi.
* **10000** giden bağlantı noktası.
* Arka uç havuzuyla ilişkili **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.

```azurepowershell-interactive
## Variables for the commands ##
$rg = 'myResourceGroupLB'
$lbn = 'myLoadBalancer'
$brn = 'myOutboundRule'
$pro = 'All'
$idl = '15'
$por = '10000'

## Get the load balancer configuration ##
$lb = 
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg 

## Apply the outbound rule configuration to the load balancer. ##
$lb | Add-AzLoadBalancerOutBoundRuleConfig -Name $brn -FrontendIPConfiguration $lb.FrontendIpConfigurations[1] -BackendAddressPool $lb.BackendAddressPools[1] -Protocol $pro -IdleTimeoutInMinutes $idl -AllocatedOutboundPort $por | Set-AzLoadBalancer
```

### <a name="add-virtual-machines-to-outbound-pool"></a>Sanal makineleri giden havuzuna Ekle

Sanal makine ağ arabirimlerini [Add-Aznetworkınterfaceipconfig](/powershell/module/az.network/add-aznetworkinterfaceipconfig)ile yük dengeleyicinin giden havuzuna ekleyin:


#### <a name="vm1"></a>VM1
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM1** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurepowershell-interactive
## Variables for the commands ##
$rg = 'myResourceGroupLB'
$lbn = 'myLoadBalancer'
$bep = 'myBackEndPoolOutbound'
$nic1 = 'myNicVM1'
$ipc = 'ipconfig1'

## Get the load balancer configuration ##
$lb = 
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg

## Get the network interface configuration ##
$nic = 
Get-AzNetworkInterface -Name $nic1 -ResourceGroupName $rg

## Apply the backend to the network interface ##
$nic | Set-AzNetworkInterfaceIpConfig -Name $ipc -LoadBalancerBackendAddressPoolId $lb.BackendAddressPools[1].id | Set-AzNetworkInterface
```

#### <a name="vm2"></a>VM2
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM2** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurepowershell-interactive
## Variables for the commands ##
$rg = 'myResourceGroupLB'
$lbn = 'myLoadBalancer'
$bep = 'myBackEndPoolOutbound'
$nic2 = 'myNicVM2'
$ipc = 'ipconfig1'

## Get the load balancer configuration ##
$lb = 
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg

## Get the network interface configuration ##
$nic = 
Get-AzNetworkInterface -Name $nic2 -ResourceGroupName $rg

## Apply the backend to the network interface ##
$nic | Set-AzNetworkInterfaceIpConfig -Name $ipc -LoadBalancerBackendAddressPoolId $lb.BackendAddressPools[1].id | Set-AzNetworkInterface
```

#### <a name="vm3"></a>VM3
* Arka uç adres havuzunda **Mybackendpooloutbound**.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimi **myNicVM3** ve **ipconfig1**ile ilişkili.
* Yük dengeleyici **Myloadbalancer**ile ilişkili.

```azurepowershell-interactive
## Variables for the commands ##
$rg = 'myResourceGroupLB'
$lbn = 'myLoadBalancer'
$bep = 'myBackEndPoolOutbound'
$nic3 = 'myNicVM3'
$ipc = 'ipconfig1'

## Get the load balancer configuration ##
$lb = 
Get-AzLoadBalancer -Name $lbn -ResourceGroupName $rg

## Get the network interface configuration ##
$nic = 
Get-AzNetworkInterface -Name $nic3 -ResourceGroupName $rg

## Apply the backend to the network interface ##
$nic | Set-AzNetworkInterfaceIpConfig -Name $ipc -LoadBalancerBackendAddressPoolId $lb.BackendAddressPools[1].id | Set-AzNetworkInterface

```

# <a name="option-2-create-a-load-balancer-basic-sku"></a>[2. seçenek: yük dengeleyici oluşturma (temel SKU)](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir. SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. 

[New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) komutunu şu şekilde kullanın:

* **Mypublicıp**adlı standart bölge YEDEKLI genel IP adresi oluşturun.
* **Myresourcegrouplb**içinde.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIP'
$sku = 'Basic'
$all = 'static'

$publicIp = 
New-AzPublicIpAddress -ResourceGroupName $rg -Name $pubIP -Location $loc -AllocationMethod $all -SKU $sku
```

## <a name="create-basic-load-balancer"></a>Temel yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:

  * Yük dengeleyicide gelen ağ trafiğini alan bir ön uç IP Havuzu.
  * Ön uç havuzunun yük dengeli ağ trafiğini gönderdiği bir arka uç IP Havuzu.
  * Arka uç sanal makine örneklerinin sistem durumunu belirleyen bir sistem durumu araştırması.
  * Trafiğin VM 'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici kuralı.

### <a name="create-frontend-ip"></a>Ön uç IP oluşturma

[New-Azloadbalancerfrontendıpconfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig)ile bir ön uç IP 'si oluşturun:

* **Myön uç**adı.
* Genel IP **Mypublicıp**'ye eklendi.

```azurepowershell-interactive
## Variables for the commands ##
$fe = 'myFrontEnd'
$rg = 'MyResourceGroupLB'
$loc = 'eastus'
$pubIP = 'myPublicIP'

$publicIp = 
Get-AzPublicIpAddress -Name $pubIP -ResourceGroupName $rg

$feip = 
New-AzLoadBalancerFrontendIpConfig -Name $fe -PublicIpAddress $publicIp
```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzunu yapılandırma

[New-Azloadbalancerbackendadddresspoolconfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig)ile arka uç adres havuzu oluşturun: 

* **Mybackendpool**adlı adlandırılmış.
* VM 'Ler, kalan adımlarda bu arka uç havuzuna ekler.

```azurepowershell-interactive
## Variable for the command ##
$be = 'myBackEndPool'

$bepool = 
New-AzLoadBalancerBackendAddressPoolConfig -Name $be
```

### <a name="create-the-health-probe"></a>Durum araştırması oluşturma

Bir sistem durumu araştırması, tüm sanal makine örneklerini denetleyerek ağ trafiği gönderebilecekleri emin olmanızı sağlar. 

Başarısız araştırma denetimine sahip bir sanal makine yük dengeleyiciden kaldırılır. Hata çözüldüğünde sanal makine yük dengeleyiciye geri eklenir.

[Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig)ile bir sistem durumu araştırması oluşturun:

* Sanal makinelerin sistem durumunu izler.
* Adlandırılmış **Myhealtharaştırma**.
* Protokol **TCP**.
* İzleme **bağlantı noktası 80**.

```azurepowershell-interactive
## Variables for the command ##
$hp = 'myHealthProbe'
$pro = 'http'
$port = '80'
$int = '360'
$cnt = '5'

$probe = 
New-AzLoadBalancerProbeConfig -Name $hp -Protocol $pro -Port $port -RequestPath / -IntervalInSeconds $int -ProbeCount $cnt
```

### <a name="create-the-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı şunları tanımlar:

* Gelen trafik için ön uç IP yapılandırması.
* Trafiği almak için arka uç IP Havuzu.
* Gerekli kaynak ve hedef bağlantı noktası. 

[Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig)ile bir yük dengeleyici kuralı oluşturun: 

* Adlandırılmış **Myhttprule**
* Ön **uç**havuzundaki **80 numaralı bağlantı noktası** dinleniyor.
* **80 numaralı bağlantı noktasını**kullanan **mybackendpool** arka uç adres havuzuna yük dengeli ağ trafiği gönderiliyor. 
* Sistem durumu araştırması **Myhealtharaştırması**kullanılıyor.
* Protokol **TCP**.
* Ön uç IP adresini kullanarak giden kaynak ağ adresi çevirisini (SNAT) etkinleştirin.


```azurepowershell-interactive
## Variables for the command ##
$lbr = 'myHTTPRule'
$pro = 'tcp'
$port = '80'

## $feip and $bePool are the variables from previous steps. ##

$rule = 
New-AzLoadBalancerRuleConfig -Name $lbr -Protocol $pro -Probe $probe -FrontendPort $port -BackendPort $port -FrontendIpConfiguration $feip -BackendAddressPool $bePool
```

### <a name="create-load-balancer-resource"></a>Yük dengeleyici kaynağı oluştur

[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile ortak yük dengeleyici oluşturma:

* Adlandırılmış **Myloadbalancer**
* **Eastus**içinde.
* Kaynak grubu **Myresourcegrouplb**.

```azurepowershell-interactive
## Variables for the command ##
$lbn = 'myLoadBalancer'
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$sku = 'Basic'

## $feip, $bepool, $probe, $rule are variables with configuration information from previous steps. ##

$lb = 
New-AzLoadBalancer -ResourceGroupName $rg -Name $lbn -SKU $sku -Location $loc -FrontendIpConfiguration $feip -BackendAddressPool $bepool -Probe $probe -LoadBalancingRule $rule
```

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

VM 'Leri dağıtmadan ve yük dengeleyicinizi test etmeden önce destekleyici sanal ağ kaynaklarını oluşturun.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile bir sanal ağ oluşturun:

* **Myvnet**adında.
* Kaynak grubu **Myresourcegrouplb**.
* **Mybackendsubnet**adlı alt ağ.
* **10.0.0.0/16**sanal ağı.
* Alt ağ **10.0.0.0/24**.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$sub = 'myBackendSubnet'
$spfx = '10.0.0.0/24'
$vnm = 'myVNet'
$vpfx = '10.0.0.0/16'

## Create subnet config ##
$subnetConfig = 
New-AzVirtualNetworkSubnetConfig -Name $sub -AddressPrefix $spfx

## Create the virtual network ##
$vnet = 
New-AzVirtualNetwork -ResourceGroupName $rg -Location $loc -Name $vnm -AddressPrefix $vpfx -Subnet $subnetConfig
```
### <a name="create-public-ip-addresses-for-the-vms"></a>VM 'Ler için genel IP adresleri oluşturma

Bir RDP bağlantısı kullanarak sanal makinelerinize erişmek için VM 'Ler için genel IP adreslerine ihtiyacınız vardır. 

İçin standart genel IP adresleri oluşturmak üzere [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) kullanın:

#### <a name="vm1"></a>VM1

* **MyVMPubIP1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma.

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip1 = 'myVMPubIP1'
$sku = 'Basic'
$all = 'static'

$RdpPubIP1 = 
New-AzPublicIpAddress -Name $ip1 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

#### <a name="vm2"></a>VM2

* **MyVMPubIP2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma. 

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip2 = 'myVMPubIP2'
$sku = 'Basic'
$all = 'static'

$RdpPubIP2 = 
New-AzPublicIpAddress -Name $ip2 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

#### <a name="vm3"></a>VM3

* **MyVMPubIP3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.
* **Standart** SKU.
* IP adresi için **statik** ayırma. 

```azurepowershell-interactive
## Variables for the command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$ip3 = 'myVMPubIP3'
$sku = 'Basic'
$all = 'static'

$RdpPubIP2 = 
New-AzPublicIpAddress -Name $ip3 -ResourceGroupName $rg -Location $loc -SKU $sku -AllocationMethod $all
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Sanal ağınıza gelen bağlantıları tanımlamak için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir ağ güvenlik grubu oluşturun:

* **Mynsgrulerdp**adında.
* RDP 'ye **Izin ver**açıklaması.
* **Allow**erişimi.
* Protokol **TCP**.
* Yön **gelen**.
* Öncelik **1000**.
* **Internet**kaynağı.
* Kaynak bağlantı noktası aralığı **(*)**.
* Hedef adres ön eki **(*)**.
* Hedef **bağlantı noktası 3389**.

```azurepowershell-interactive
## Variables for command ##
$rnm = 'myNSGRuleRDP'
$des = 'Allow RDP'
$acc = 'Allow'
$pro = 'Tcp'
$dir = 'Inbound'
$pri = '1000'
$spfx = 'Internet'
$spr = '*'
$dpfx = '*'
$dpr = '3389'

$rule1 = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $des -Access $acc -Protocol $pro -Direction $dir -Priority $pri -SourceAddressPrefix $spfx -SourcePortRange $spr -DestinationAddressPrefix $dpfx -DestinationPortRange $dpr
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir ağ güvenlik grubu kuralı oluşturun:

* **Mynsgrutahttp**adında.
* HTTP 'ye **Izin ver**açıklaması.
* **Allow**erişimi.
* Protokol **TCP**.
* Yön **gelen**.
* Öncelik **2000**.
* **Internet**kaynağı.
* Kaynak bağlantı noktası aralığı **(*)**.
* Hedef adres ön eki **(*)**.
* Hedef **bağlantı noktası 80**.

```azurepowershell-interactive
## Variables for command ##
$rnm = 'myNSGRuleHTTP'
$des = 'Allow HTTP'
$acc = 'Allow'
$pro = 'Tcp'
$dir = 'Inbound'
$pri = '2000'
$spfx = 'Internet'
$spr = '*'
$dpfx = '*'
$dpr = '80'

$rule2 = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $des -Access $acc -Protocol $pro -Direction $dir -Priority $pri -SourceAddressPrefix $spfx -SourcePortRange $spr -DestinationAddressPrefix $dpfx -DestinationPortRange $dpr
```

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun:

* **Mynsg**adlı adlandırılmış.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Bir değişkende depolanan önceki adımlarda oluşturulan güvenlik kuralları.

```azurepowershell
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nmn = 'myNSG'

## $rule1 and $rule2 are variables with configuration information from the previous steps. ##
$nsg = 
New-AzNetworkSecurityGroup -ResourceGroupName $rg -Location $loc -Name $nmn -SecurityRules $rule1,$rule2
```

### <a name="create-network-interfaces"></a>Ağ arabirimlerini oluşturma

[New-Aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface)ile üç ağ arabirimi oluşturun:

#### <a name="vm-1"></a>VM 1

* **MyNicVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic1 = 'myNicVM1'
$vnt = 'myVNet'
$ip1 = 'myVMPubIP1'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM1 ##
$pub1 = 
Get-AzPublicIPAddress -Name $ip1 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM1 ##
$nicVM1 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic1 -PublicIpAddress $pub1 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```

#### <a name="vm-2"></a>VM 2

* **MyNicVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic2 = 'myNicVM2'
$vnt = 'myVNet'
$ip2 = 'myVMPubIP2'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM2 ##
$pub2 = 
Get-AzPublicIPAddress -Name $ip2 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM2 ##
$nicVM2 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic2 -PublicIpAddress $pub2 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```

#### <a name="vm-3"></a>VM 3

* **MyNicVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus**konumunda.
* Sanal ağ **\**sanal ağı 'nda.
* Alt ağda **Mybackendsubnet**.
* Ağ güvenlik grubu ' nda, **Mynsg**.
* **Mybackendpool**Içindeki **myloadbalancer** yük dengeleyiciye eklendi.

```azurepowershell-interactive
## Variables for command ##
$rg = 'myResourceGroupLB'
$loc = 'eastus'
$nic3 = 'myNicVM3'
$vnt = 'myVNet'
$ip3 = 'myVMPubIP3'
$lb = 'myLoadBalancer'
$ngn = 'myNSG'

## Command to get virtual network configuration. ##
$vnet = 
Get-AzVirtualNetwork -Name $vnt -ResourceGroupName $rg

## Command to get public ip address for VM3 ##
$pub3 = 
Get-AzPublicIPAddress -Name $ip3 -ResourceGroupName $rg

## Command to get load balancer configuration
$bepool = 
Get-AzLoadBalancer -Name $lb -ResourceGroupName $rg | Get-AzLoadBalancerBackendAddressPoolConfig

## Command to get network security group configuration ##
$nsg = 
Get-AzNetworkSecurityGroup -Name $ngn -ResourceGroupName $rg

## Command to create network interface for VM3 ##
$nicVM3 = 
New-AzNetworkInterface -ResourceGroupName $rg -Location $loc -Name $nic3 -PublicIpAddress $pub3 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg -Subnet $vnet.Subnets[0]
```

### <a name="create-availability-set-for-virtual-machines"></a>Sanal makineler için kullanılabilirlik kümesi oluştur

[New-AzAvailabilitySet](/powershell/module/az.compute/new-azvm) kullanarak sanal makineler için bir kullanılabilirlik kümesi oluşturun:

* **MyAvSet**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* **Eastus** konumunda.

```azurepowershell-interactive
## Variables used for the command. ##
$rg = 'myResourceGroupLB'
$avs = 'myAvSet'
$loc = 'eastus'

New-AzAvailabilitySet -ResourceGroupName $rg -Name $avs -Location $loc
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell
$cred = Get-Credential
```

Sanal makineleri şu şekilde oluşturun:

* [New-AzVM](/powershell/module/az.compute/new-azvm)
* [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)
* [Set-AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem)
* [Set-Azvmsourceımage](/powershell/module/az.compute/set-azvmsourceimage)
* [Add-Azvmnetworkınterface](/powershell/module/az.compute/add-azvmnetworkinterface)


#### <a name="vm1"></a>VM1

* **MyVM1**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM1**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Eastus** konumunda.
* **MyAvSet** kullanılabilirlik kümesinde.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM1'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$loc = 'eastus'
$avs = 'myAvSet'

## Create a virtual machine configuration. $cred and $nicVM1 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM1.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Location $loc -VM $vmConfig -AvailabilitySetName $avs
```


#### <a name="vm2"></a>VM2

* **MyVM2**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM2**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Eastus** konumunda.
* **MyAvSet** kullanılabilirlik kümesinde.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM2'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$loc = 'eastus'
$avs = 'myAvSet'

## Create a virtual machine configuration. $cred and $nicVM2 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM2.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Location $loc -VM $vmConfig -AvailabilitySetName $avs
```

#### <a name="vm3"></a>VM3

* **MyVM3**adlı.
* Kaynak grubu **Myresourcegrouplb**.
* Ağ arabirimine bağlı **myNicVM3**.
* Yük dengeleyici **Myloadbalancer**'a eklendi.
* **Eastus** konumunda.
* **MyAvSet** kullanılabilirlik kümesinde.

```azurepowershell-interactive
## Variables used for command. ##
$rg = 'myResourceGroupLB'
$vm = 'myVM3'
$siz = 'Standard_DS1_v2'
$pub = 'MicrosoftWindowsServer'
$off = 'WindowsServer'
$sku = '2019-Datacenter'
$ver = 'latest'
$loc = 'eastus'
$avs = 'myAvSet'

## Create a virtual machine configuration. $cred and $nicVM3 are variables with configuration from the previous steps. ##

$vmConfig = 
New-AzVMConfig -VMName $vm -VMSize $siz | Set-AzVMOperatingSystem -Windows -ComputerName $vm -Credential $cred | Set-AzVMSourceImage -PublisherName $pub -Offer WindowsServer -Skus $sku -Version $ver | Add-AzVMNetworkInterface -Id $nicVM3.Id

## Create the virtual machine ##
New-AzVM -ResourceGroupName $rg -Location $loc -VM $vmConfig -AvailabilitySetName $avs
```

Üç VM 'yi oluşturmak ve yapılandırmak birkaç dakika sürer.

---

### <a name="install-iis-with-a-custom-web-page"></a>Özel bir Web sayfasıyla IIS 'yi yükler

IIS 'yi her iki arka uç VM 'de aşağıdaki gibi özel bir Web sayfasıyla birlikte yüklemelisiniz:

1. [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress)kullanarak üç VM 'nın genel IP adreslerini alın.

   ```azurepowershell-interactive
   ## Variables for commands. ##
   $rg = 'myResourceGroupLB'
   $ip1 = 'myVMPubIP1'
   $ip2 = 'myVMPubIP2'
   $ip3 = 'myVMPubIP3'

   ## VM1 ##
   (Get-AzPublicIPAddress -ResourceGroupName $rg -Name $ip1).IpAddress

   ## VM2 ##
   (Get-AzPublicIPAddress -ResourceGroupName $rg -Name $ip2).IpAddress

   ## VM3 ##
   (Get-AzPublicIPAddress -ResourceGroupName $rg -Name $ip3).IpAddress
   ```
   
2. VM 'lerin genel IP adreslerini kullanarak **myVM1**, **myVM2**ve **myVM3** ile uzak masaüstü bağlantıları oluşturun.

3. RDP oturumunu başlatmak için her VM 'nin kimlik bilgilerini girin.

4. Her VM 'de Windows PowerShell 'i başlatın ve IIS sunucusunu yüklemek ve varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.

   ```powershell
   # Install IIS
   Install-WindowsFeature -name Web-Server -IncludeManagementTools

   # Remove default htm file
   remove-item  C:\inetpub\wwwroot\iisstart.htm
    
   #Add custom htm file
   Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from host " + $env:computername)
   ```

5. **MyVM1**, **myVM2**ve **myVM3**ile RDP bağlantılarını kapatın.


## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme
Yük dengeleyicinin genel IP adresini almak için [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress)komutunu kullanın:

* Adlandırılmış **Mypublicıp**
* Kaynak grubu **Myresourcegrouplb**.

```azurepowershell-interactive
## Variables for command. ##
$rg = 'myResourceGroupLB'
$ipn = 'myPublicIP'

Get-AzPublicIPAddress -ResourceGroupName $rg -Name $ipn | select IpAddress
```

Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.

![Yük dengeleyiciyi sınama](media/quickstart-create-basic-load-balancer-powershell/load-balancer-test.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tümünü kaldırabilirsiniz 

```azurepowershell-interactive
## Variable for command. ##
$rg = 'myResourceGroupLB'

Remove-AzResourceGroup -Name $rg
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta

* Standart veya temel bir genel yük dengeleyici oluşturdunuz
* Bağlı sanal makineler. 
* Yük dengeleyici trafik kuralı ve sistem durumu araştırması yapılandırıldı.
* Yük dengeleyici test edildi.

Azure Load Balancer hakkında daha fazla bilgi edinmek için [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular Load Balancer](load-balancer-faqs.md).

[Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
