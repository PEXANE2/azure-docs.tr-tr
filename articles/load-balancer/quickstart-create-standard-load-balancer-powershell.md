---
title: 'Quickstart: Yük Dengeleyicisi Oluşturma - Azure PowerShell'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıç, Azure PowerShell kullanarak yük bakiyesi oluşturmanın nasıl yapılacağını gösterir
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: allensu
ms:custom: seodec18
ms.openlocfilehash: f169d7694199e496e472a6c32312cf6782270378
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80247223"
---
# <a name="quickstart-create-a-load-balancer-using-azure-powershell"></a>Quickstart: Azure PowerShell'i kullanarak Yük Dengeleyicisi Oluşturma

Bu hızlı başlangıçta, Azure PowerShell kullanarak bir Standart Load Balancer’ın nasıl oluşturulacağı gösterilmektedir. Yük dengeleyicisini sınamak için, Windows sunucusu çalıştıran üç sanal makine (VM) dağıtın ve VM'ler arasında bir web uygulamasını yükleyin. Standart Load Balancer hakkında daha fazla bilgi için bkz. [Standart Load Balancer nedir](load-balancer-standard-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Yük bakiyecinizi oluşturmadan [önce, Yeni-AzResourceGroup'a](/powershell/module/az.resources/new-azresourcegroup)sahip bir kaynak grubu oluşturmanız gerekir. Aşağıdaki örnek, *EastUS* konumunda *myResourceGroupSLB* adında bir kaynak grubu oluşturur:

```azurepowershell
$rgName='MyResourceGroupSLB'
$location='eastus'
New-AzResourceGroup -Name $rgName -Location $location
```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)ile ortak bir IP adresi oluşturun. Aşağıdaki örnek, *myResourceGroupSLB* kaynak grubunda *myPublicIP* adlı bir bölge gereksiz genel IP adresi oluşturur:

```azurepowershell
$publicIp = New-AzPublicIpAddress `
 -ResourceGroupName $rgName `
 -Name 'myPublicIP' `
 -Location $location `
 -AllocationMethod static `
 -SKU Standard
```

Bölge 1'de bir zonal Public IP adresi oluşturmak için aşağıdakileri kullanın:

```azurepowershell
$publicIp = New-AzPublicIpAddress `
 -ResourceGroupName $rgName `
 -Name 'myPublicIP' `
 -Location $location `
 -AllocationMethod static `
 -SKU Standard `
 -zone 1
```

Temel ```-SKU Basic``` Genel IP oluşturmak için kullanın. Temel Genel IP'ler **Standart** yük dengeleyicisi ile uyumlu değildir. Microsoft, üretim iş yükleri için **Standart'ı** kullanmanızı önerir.

> [!IMPORTANT]
> Bu hızlı başlatmanın geri kalanı, yukarıdaki SKU seçim işlemi sırasında **Standart** SKU'nun seçildiğini varsayar.

## <a name="create-load-balancer"></a>Yük Dengeleyicisi Oluştur

Bu bölümde, yük dengeleyicisi için ön uç IP'yi ve arka uç adres havuzunu yapılandırın ve ardından Standart Yük Dengeleyicisi'ni oluşturursunuz.

### <a name="create-frontend-ip"></a>Ön uç IP oluşturma

[New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig)ile bir ön uç IP oluşturun. Aşağıdaki örnek, *myFrontEnd* adında bir ön uç IP yapılandırması oluşturur ve *myPublicIP* adresini bağlar:

```azurepowershell
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzunda yapılandırma

[New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig)ile bir arka uç adres havuzu oluşturun. VM'ler kalan adımlardaki bu arka uç havuzuna eklenir. Aşağıdaki *örnekmyBackEndPool*adlı bir arka uç adresi havuzu oluşturur:

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. VM, 15 saniyelik aralıklarda art arda iki kez başarısız olursa varsayılan olarak yük dengeleyici dağıtımından kaldırılır. Bir protokolü temel alan bir durum araştırması veya uygulamanız için belirli bir sistem durumu denetim sayfası oluşturun.

Aşağıdaki örnek bir TCP araştırması oluşturur. Ayrıca daha ayrıntılı sistem durumu denetimleri için özel HTTP araştırmaları oluşturabilirsiniz. Özel bir HTTP yoklaması kullanırken *healthcheck.aspx* gibi bir durum denetimi sayfası oluşturmanız gerekir. Yük dengeleyicinin konağı rotasyonda tutması için yoklamanın **HTTP 200 OK** yanıtını döndürmesi gerekir.

Bir TCP sistem durumu sondası oluşturmak için [Add-AzLoadBalancerProbeConfig'i](/powershell/module/az.network/add-azloadbalancerprobeconfig)kullanırsınız. Aşağıdaki örnek her VM’yi *80* numaralı *HTTP* bağlantı noktasında izleyen *myHealthProbe* adında bir durum yoklaması oluşturur:

```azurepowershell
$probe = New-AzLoadBalancerProbeConfig `
 -Name 'myHealthProbe' `
 -Protocol Http -Port 80 `
 -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca durumu iyi olan VM’lerin trafik almasını sağlamak için kullanılacak durum araştırmasını da tanımlamanız gerekir.

[Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig)ile bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek, *myLoadBalancerRule* adlı bir yük dengeleyici kuralı oluşturur ve *80* numaralı *TCP* bağlantı noktasında trafiği dengeler:

```azurepowershell
$rule = New-AzLoadBalancerRuleConfig `
  -Name 'myLoadBalancerRuleWeb' -Protocol Tcp `
  -Probe $probe -FrontendPort 80 -BackendPort 80 `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bePool
```

### <a name="create-the-nat-rules"></a>NAT kurallarını oluşturma

[New-AzLoadBalancerInboundNatRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig)ile NAT kuralları oluşturun. Aşağıdaki örnek, 4221 ve 4222 no'lu bağlantı noktası olan arka uç sunucularına RDP bağlantılarına izin vermek için *myLoadBalancerRDP1* ve *myLoadBalancerRDP2* adlı NAT kuralları oluşturur:

```azurepowershell
$natrule1 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP1' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp -FrontendPort 4221 `
  -BackendPort 3389

$natrule2 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP2' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4222 `
  -BackendPort 3389

$natrule3 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP3' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4223 `
  -BackendPort 3389
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

[New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer)ile Standart Yük Dengeleyicisini oluşturun. Aşağıdaki örnek, önceki adımlarda oluşturduğunuz ön uç IP yapılandırması, arka uç havuzu, sistem durumu sondası, yük dengeleme kuralı ve NAT kurallarını kullanarak myLoadBalancer adlı genel bir Standart Yük Dengeleyici oluşturur:

```azurepowershell
$lb = New-AzLoadBalancer `
  -ResourceGroupName $rgName `
  -Name 'MyLoadBalancer' `
  -SKU Standard `
  -Location $location `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bepool `
  -Probe $probe `
  -LoadBalancingRule $rule `
  -InboundNatRule $natrule1,$natrule2,$natrule3
```

Temel ```-SKU Basic``` Yük Dengeleyicisi oluşturmak için kullanın. Microsoft, üretim iş yükleri için Standart'ı kullanmanızı önerir.

> [!IMPORTANT]
> Bu hızlı başlatmanın geri kalanı, yukarıdaki SKU seçim işlemi sırasında **Standart** SKU'nun seçildiğini varsayar.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Bazı VM’leri dağıtabilmeniz ve yük dengeleyicinizi test edebilmeniz için destekleyici ağ kaynakları (sanal ağ ve sanal NIC’ler) oluşturmanız gerekir. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek *mySubnet* alt ağına sahip *myVnet* adında bir sanal ağ oluşturur:

```azurepowershell
# Create subnet config
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.2.0/24

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName "myResourceGroupSLB" `
  -Location $location `
  -Name "myVnet" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-addresses-for-the-vms"></a>VM'ler için genel IP adresleri oluşturma

RDP bağlantısını kullanarak VM'lerinize erişmek için VM'ler için genel IP adresine ihtiyacınız vardır. Bu senaryoda standart yük dengeleyicisi kullanıldığından, [New-AzPublicIpAddress'e](/powershell/module/az.network/new-azpublicipaddress)sahip VM'ler için Standart genel IP adresleri oluşturmanız gerekir.

```azurepowershell
$RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static
 

$RdpPublicIP_2 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_2" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static


$RdpPublicIP_3 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_3" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static

```

Temel ```-SKU Basic``` Genel IP'ler oluşturmak için kullanın. Microsoft, üretim iş yükleri için Standart'ı kullanmanızı önerir.

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Sanal ağınıza gelen bağlantıları tanımlamak için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile 3389 bağlantı noktası üzerinden RDP bağlantılarına izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurepowershell

$rule1 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleRDP' -Description 'Allow RDP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 1000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 3389
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bağlantı noktası 80 üzerinden gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturun.

```azurepowershell
$rule2 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleHTTP' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 2000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 80
```

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun.

```azurepowershell
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
-Name 'myNetworkSecurityGroup' -SecurityRules $rule1,$rule2
```

### <a name="create-nics"></a>NIC’leri oluşturma
Sanal NIC'ler oluşturun ve önceki adımlarda oluşturulan genel IP adresi ve ağ güvenlik gruplarıyla [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)ile ilişkilendirin. Aşağıdaki örnek üç sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

```azurepowershell
# Create NIC for VM1
$nicVM1 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic1' -PublicIpAddress $RdpPublicIP_1 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule1 -Subnet $vnet.Subnets[0]

$nicVM2 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic2' -PublicIpAddress $RdpPublicIP_2 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule2 -Subnet $vnet.Subnets[0]

$nicVM3 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic3' -PublicIpAddress $RdpPublicIP_3 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule3 -Subnet $vnet.Subnets[0]
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell
$cred = Get-Credential
```

Şimdi [Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturabilirsiniz. Aşağıdaki örnek, zaten yoksa iki VM ve gerekli sanal ağ bileşenleri oluşturur. Bu örnekte, NICs (*MyNic1*, *MyNic2*, ve *MyNic3*) önceki adımda oluşturulan sanal makineler *myVM1,* *myVM2*ve *VM3*atanır. Ayrıca, NIC'ler yük dengeleyicisinin arka uç havuzuyla ilişkilendirildiğinden, VM'ler otomatik olarak arka uç havuzuna eklenir.

```azurepowershell

# ############## VM1 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM1.Id

# Create a virtual machine
$vm1 = New-AzVM -ResourceGroupName $rgName -Zone 1 -Location $location -VM $vmConfig

# ############## VM2 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM2' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM2.Id

# Create a virtual machine
$vm2 = New-AzVM -ResourceGroupName $rgName -Zone 2 -Location $location -VM $vmConfig

# ############## VM3 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM3' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM3' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
| Add-AzVMNetworkInterface -Id $nicVM3.Id

# Create a virtual machine
$vm3 = New-AzVM -ResourceGroupName $rgName -Zone 3 -Location $location -VM $vmConfig
```

Üç VM'yi oluşturmak ve yapılandırmak birkaç dakika sürer.

### <a name="install-iis-with-a-custom-web-page"></a>IIS'yi özel bir web sayfasıyla yükleme

IIS'yi her iki arka uç VM'de özel bir web sayfasıyla aşağıdaki gibi yükleyin:

1. Kullanarak üç VM'nin genel IP `Get-AzPublicIPAddress`adreslerini alın.

   ```azurepowershell
     $vm1_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_1").IpAddress
     $vm2_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_2").IpAddress
     $vm3_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_3").IpAddress
    ```
2. Aşağıdaki gibi VM'lerin genel IP adreslerini kullanarak *myVM1,* *myVM2*ve *myVM3* ile uzak masaüstü bağlantıları oluşturun: 

   ```azurepowershell    
     mstsc /v:$vm1_rdp_ip
     mstsc /v:$vm2_rdp_ip
     mstsc /v:$vm3_rdp_ip
   
    ```

3. RDP oturumunu başlatmak için her VM için kimlik bilgilerini girin.
4. Windows PowerShell'i her VM'de başlatın ve IIS sunucusunu yüklemek ve varsayılan HTM dosyasını güncelleştirmek için aşağıdaki komutları kullanarak.

    ```azurepowershell
    # Install IIS
     Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from host " + $env:computername)
    ```

5. *MYVM1, myVM2*ve *myVM2* *myVM3*ile RDP bağlantılarını kapatın.


## <a name="test-load-balancer"></a>Yük dengeleyiciyi sınama
[Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress)ile yük bakiyecinizin genel IP adresini edinin. Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```azurepowershell
Get-AzPublicIPAddress `
  -ResourceGroupName "myResourceGroupSLB" `
  -Name "myPublicIP" | select IpAddress
```

Daha sonra genel IP adresini bir web tarayıcısına girebilirsiniz. Aşağıdaki örnekteki gibi yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adının dahil olduğu web sitesi görüntülenir:

![Yük dengeleyiciyi sınama](media/quickstart-create-basic-load-balancer-powershell/load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtma işlemini görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroupSLB
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Standart Yük Dengeleyicisi oluşturdunuz, ona VM'ler iliştirdin, Yük Dengeleyicitrafik kuralını, sistem sondasını yapılandırdın ve ardından Yük Dengeleyicisini test ettiniz. Azure Yük Bakiyesi hakkında daha fazla bilgi edinmek için [Azure Yük Bakiyesi öğreticilerine](tutorial-load-balancer-standard-public-zone-redundant-portal.md)devam edin.

[Yük Dengeleyicisi ve Kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
