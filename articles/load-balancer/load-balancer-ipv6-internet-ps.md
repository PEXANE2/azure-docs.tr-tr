---
title: IPv6 ile Internet'e bakan bir yük dengeleyicisi oluşturun - Azure PowerShell
titleSuffix: Azure Load Balancer
description: Resource Manager için PowerShell'i kullanarak IPv6 ile Internet'e bakan yük dengeleyicisini nasıl oluşturabilirsiniz öğrenin
services: load-balancer
documentationcenter: na
author: asudbring
keywords: ipv6, azure yük dengeleyici, çift yığın, genel ip, yerli ipv6, mobil, iot
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: allensu
ms.openlocfilehash: e5f9762533dc2ad47f855714822ba39c645bf847
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76045460"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Resource Manager için PowerShell'i kullanarak IPv6 ile Internet'e bakan bir yük dengeleyici oluşturmaya başlayın

> [!div class="op_single_selector"]
> * [Powershell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)

>[!NOTE] 
>Bu makalede, Temel Yük Dengeleyicileri hem IPv4 ve IPv6 bağlantısı sağlamak için izin vermek için bir giriş IPv6 özelliği açıklanır. IPv6 bağlantılarını Sanal Ağlarınızla birleştiren ve IPv6 Ağ Güvenlik Grubu kuralları, IPv6 Kullanıcı tanımlı yönlendirme, IPv6 Temel ve Standart yük dengeleme ve daha fazlası gibi temel özellikleri içeren [Azure VNET'ler için](../virtual-network/ipv6-overview.md) IPv6 ile kapsamlı IPv6 bağlantısı artık kullanılabilir.  Azure VNET'ler için IPv6, Azure'daki IPv6 uygulamaları için önerilen standarttır. [Azure VNET Powershell Dağıtımı için IPv6'ya](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) bakın 

Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram, bu makalede dağıtılan yük dengeleme çözümlerini göstermektedir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Bu senaryoda aşağıdaki Azure kaynaklarını oluşturursunuz:

* IPv4 ve IPv6 Genel IP adresi ne kadar internete bakan bir Yük Dengeleyicisi
* kamu VIP'lerini özel uç noktalarına eşlemek için iki yük dengeleme kuralı
* iki VM içeren bir Kullanılabilirlik Kümesi
* iki sanal makine (VM)
* hem IPv4 hem de IPv6 adresleri atanmış her VM için bir sanal ağ arabirimi

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Azure PowerShell'i kullanarak çözümü dağıtma

Aşağıdaki adımlar, PowerShell ile Azure Kaynak Yöneticisi'ni kullanarak Internet'e bakan bir yük dengeleyicisinin nasıl oluşturulacağımı gösterir. Azure Kaynak Yöneticisi ile her kaynak ayrı ayrı oluşturulur ve yapılandırılır ve ardından bir kaynak oluşturmak üzere bir araya getirilir.

Bir yük dengeleyicisi dağıtmak için aşağıdaki nesneleri oluşturur ve yapılandırırsınız:

* Frontend IP yapılandırması - gelen ağ trafiği için genel IP adresleri içerir.
* Arka uç adres havuzu - sanal makinelerin yük bakiyesinden ağ trafiği alması için ağ arabirimleri (NIC' ler) içerir.
* Yük dengeleme kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleme kurallarını içerir.
* Gelen NAT kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleme kurallarını içerir.
* Araştırmalar: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

Daha fazla bilgi için Azure [Yük Bakiyesi bileşenlerine](./concepts-limitations.md#load-balancer-components)bakın.

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell’i Resource Manager’ı kullanacak şekilde ayarlama

PowerShell için Azure Kaynak Yöneticisi modülünün en son üretim sürümüne sahip olduğundan emin olun.

1. Azure’da oturum açma

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

    İstendiğinde kimlik bilgilerinizi girin.

2. Hesapla ilişkili abonelikleri kontrol etme

   ```azurepowershell-interactive
    Get-AzSubscription
    ```

3. Hangi Azure aboneliğinizin kullanılacağını seçin.

    ```azurepowershell-interactive
    Select-AzSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Kaynak grubu oluşturma (varolan bir kaynak grubu kullanıyorsanız bu adımı atlayın)

    ```azurepowershell-interactive
    New-AzResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Ön uç IP havuzu için sanal ağ ve genel IP adresi oluşturma

1. Alt ağ içeren bir sanal ağ oluşturun.

    ```azurepowershell-interactive
    $backendSubnet = New-AzVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Ön uç IP adresi havuzu için Azure Genel IP adresi (PIP) kaynakları oluşturun. Aşağıdaki komutları çalıştırmadan `-DomainNameLabel` önce değerini değiştirdiğinden emin olun. Değer, Azure bölgesinde benzersiz olmalıdır.

    ```azurepowershell-interactive
    $publicIPv4 = New-AzPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > Yük dengeleyicisi, FQDN'si için önek olarak genel IP'nin etki alanı etiketini kullanır. Bu örnekte, FQDN'ler *lbnrpipv4.westus.cloudapp.azure.com* ve *lbnrpipv6.westus.cloudapp.azure.com.*

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Ön Uç IP yapılandırmaları ve Arka Uç Adres Havuzu oluşturma

1. Oluşturduğunuz Genel IP adreslerini kullanan ön uç adresi yapılandırmasını oluşturun.

    ```azurepowershell-interactive
    $FEIPConfigv4 = New-AzLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Arka uç adres havuzları oluşturun.

    ```azurepowershell-interactive
    $backendpoolipv4 = New-AzLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB kuralları, NAT kuralları, sonda ve yük dengeleyicisi oluşturma

Bu örnek aşağıdaki nesneleri oluşturur:

* bağlantı noktası 443'teki tüm gelen trafiği 4443 bağlantı noktasına çevirmek için bir NAT kuralı
* 80 numaralı bağlantı noktasına gelen tüm trafiği arka uç havuzundaki adreslerin 80 numaralı bağlantı noktasıyla dengeleyen yük dengeleyici kuralı.
* bağlantı noktası 3389'daki VM'lere RDP bağlantısına izin veren bir yük dengeleyici kuralı.
* *HealthProbe.aspx* adlı bir sayfada veya 8080 portundaki bir hizmette sağlık durumunu kontrol etmek için bir sonda kuralı
* tüm bu nesneleri kullanan bir yük dengeleyicisi

1. NAT kurallarını oluşturun.

    ```azurepowershell-interactive
    $inboundNATRule1v4 = New-AzLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Durum araştırması oluşturun. Araştırmaları iki şekilde yapılandırabilirsiniz:

    HTTP araştırma

    ```azurepowershell-interactive
    $healthProbe = New-AzLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    veya TCP sondası

    ```azurepowershell-interactive
    $healthProbe = New-AzLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    Bu örnekiçin, TCP sondalarını kullanacağız.

3. Yük dengeleyici kuralı oluşturun.

    ```azurepowershell-interactive
    $lbrule1v4 = New-AzLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. Daha önce oluşturulan nesneleri kullanarak yük dengeleyicisini oluşturun.

    ```azurepowershell-interactive
    $NRPLB = New-AzLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a>Arka uç VM'ler için NIC'ler oluşturma

1. NIC'lerin oluşturulması gereken Sanal Ağ ve Sanal Ağ Alt Net'ini alın.

    ```azurepowershell-interactive
    $vnet = Get-AzVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. VM'ler için IP yapılandırmaları ve NIC'ler oluşturun.

    ```azurepowershell-interactive
    $nic1IPv4 = New-AzNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Sanal makineler oluşturun ve yeni oluşturulan NIC'leri atayın

VM oluşturma hakkında daha fazla bilgi için Kaynak [Yöneticisi ve Azure PowerShell ile Windows Sanal Makine Oluştur ve Önceden Yapılandırma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Kullanılabilirlik Kümesi ve Depolama hesabı oluşturma

    ```azurepowershell-interactive
    New-AzAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Her VM oluşturun ve önceki oluşturulan NIC'leri atama

    ```azurepowershell-interactive
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

    $vm1 = New-AzVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```


