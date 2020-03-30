---
title: Birden çok IP yapılandırmalarında yük dengelemesi - Azure CLI
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure CLI kullanarak birincil ve ikincil IP yapılandırmaları arasında yük dengeleme hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: 6ac9e362314cc45e6adbdcf1390f70cbe6b05de8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075973"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>PowerShell kullanarak birden fazla IP yapılandırması üzerinde yük dengeleme

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [CLI](load-balancer-multiple-ip-cli.md)
> * [Powershell](load-balancer-multiple-ip-powershell.md)


Bu makalede, ikincil bir ağ arabiriminde (NIC) birden çok IP adresi olan Azure Yük Dengeleyicisi'nin nasıl kullanılacağı açıklanmaktadır. Bu senaryo için, windows çalıştıran iki VM'miz vardır, her biri birincil ve ikincil bir NIC'si vardır. İkincil NIC'lerin her birinin iki IP yapılandırması vardır. Her VM, contoso.com ve fabrikam.com web sitelerini de barındırar. Her web sitesi ikincil NIC'deki IP yapılandırmalarından birine bağlıdır. Trafiği web sitesi için ilgili IP yapılandırmasına dağıtmak için her web sitesi için bir tane olmak üzere iki ön uç IP adresini ortaya çıkarmak için Azure Yük Dengeleyicisi'ni kullanırız. Bu senaryo, her iki ön uçta da aynı bağlantı noktası numarasını ve her iki arka uç ip adresini kullanır.

![LB senaryo görüntüsü](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Birden çok IP yapılandırmasına bakiye yüklemek için adımlar

Bu makalede özetlenen senaryoyu elde etmek için aşağıdaki adımları izleyin:

1. Azure PowerShell'i yükleyin. Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
2. Aşağıdaki ayarları kullanarak bir kaynak grubu oluşturun:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Daha fazla bilgi için kaynak grubu oluşturma adım [2'ye](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)bakın.

3. VM'lerinizi içerecek [bir Kullanılabilirlik Kümesi oluşturun.](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) Bu senaryo için aşağıdaki komutu kullanın:

    ```powershell
    New-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Tek bir NIC'li bir VM oluşturulmasını hazırlamak için [Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) makale oluşturma makalesinde 3'ten 5'e kadar olan yönergeleri izleyin. 6.1 adımını uygulayın ve adım 6.2 yerine aşağıdakileri kullanın:

    ```powershell
    $availset = Get-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Ardından Windows [VM adımlarını](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) 6,3'ten 6,8'e tamamlayın.

5. VM'lerin her birine ikinci bir IP yapılandırması ekleyin. Sanal makineler makalesine [birden çok IP adresi atama talimatını](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) izleyin. Aşağıdaki yapılandırma ayarlarını kullanın:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Bu öğreticinin amacı için ikincil IP yapılandırmalarını genel IP'lerle ilişkilendirmeniz gerekmez. Ortak IP ilişkilendirme bölümünü kaldırmak için komutu edin.

6. VM2 için bu makalenin 4 ile 6 arası adımlarını tamamlayın. Bunu yaparken VM adını VM2 olarak değiştirdiğinden emin olun. İkinci VM için sanal ağ oluşturmanız gerekmediğini unutmayın. Kullanım durumunuza bağlı olarak yeni bir alt ağ oluşturabilirsiniz veya oluşturmayabilirsiniz.

7. İki genel IP adresi oluşturun ve bunları gösterildiği gibi uygun değişkenlerde saklayın:

    ```powershell
    $publicIP1 = New-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. İki frontend IP yapılandırması oluşturun:

    ```powershell
    $frontendIP1 = New-AzLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Arka uç adres havuzlarınızı, sondanızı ve yük dengeleme kurallarınızı oluşturun:

    ```powershell
    $beaddresspool1 = New-AzLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Bu kaynakları oluşturduktan sonra yük bakiyecinizi oluşturun:

    ```powershell
    $mylb = New-AzLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. Yeni oluşturduğunuz yük dengeleyicinize ikinci arka uç adresi havuzunu ve ön uç IP yapılandırmasını ekleyin:

    ```powershell
    $mylb = Get-AzLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzLoadBalancer

    $mylb | Add-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzLoadBalancer
    
    Add-AzLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzLoadBalancer
    ```

12. Aşağıdaki komutlar NIC'leri alır ve yük dengeleyicisinin arka uç adresi havuzuna her ikincil NIC'nin her iki IP yapılandırmasını da ekler:

    ```powershell
    $nic1 = Get-AzNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzLoadBalancer

    $nic1 | Set-AzNetworkInterface
    $nic2 | Set-AzNetworkInterface
    ```

13. Son olarak, DNS kaynak kayıtlarını Yük Dengeleyicisinin ilgili ön uç IP adresini işaret etmek üzere yapılandırmanız gerekir. Etki alanlarınızı Azure DNS'de barındırabilirsiniz. Yük Bakiyesi ile Azure DNS kullanma hakkında daha fazla bilgi [için](../dns/dns-for-azure-services.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da yük dengeleme hizmetlerini kullanmada](../traffic-manager/traffic-manager-load-balancing-azure.md)Azure'da yük dengeleme hizmetlerini nasıl birleştirebilirsiniz hakkında daha fazla bilgi edinin.
- [Azure Yük Dengeleyicisi için Azure Monitor günlüklerinde](../load-balancer/load-balancer-monitor-log.md)yük dengeleyicisini yönetmek ve sorun gidermek için Azure'da farklı türde günlükleri nasıl kullanabileceğinizi öğrenin.
