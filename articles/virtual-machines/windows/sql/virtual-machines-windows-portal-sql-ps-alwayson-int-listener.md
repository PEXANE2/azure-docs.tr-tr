---
title: Kullanılabilirlik grubu dinleyicilerini & yük dengeleyicisini (PowerShell) yapılandırın
description: Bir veya daha fazla IP adresine sahip dahili yük dengeleyicisini kullanarak Kullanılabilirlik Grubu Dinleyicilerini Azure Kaynak Yöneticisi modelinde yapılandırın.
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: 14b39cde-311c-4ddf-98f3-8694e01a7d3b
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/06/2019
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: cabfc84d2bc0c9d08a457e67c0182d7550f04ceb
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668895"
---
# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Bir veya daha fazla Always On availability grup dinleyicisini yapılandırın - Kaynak Yöneticisi
Bu konu nasıl gösterin:

* PowerShell cmdlets kullanarak SQL Server kullanılabilirlik grupları için bir dahili yük dengeleyicioluşturun.
* Birden fazla kullanılabilirlik grubu için bir yük dengeleyicisine ek IP adresleri ekleyin. 

Kullanılabilirlik grubu dinleyicisi, istemcilerin veritabanı erişimi için bağlandığı sanal ağ adıdır. Azure sanal makinelerde, bir yük dengeleyicisi dinleyicinin IP adresini tutar. Yük bakiyesi trafiği sonda bağlantı noktasında dinleyen SQL Server örneğine yönlendirir. Genellikle, kullanılabilirlik grubu bir iç yük dengeleyici kullanır. Azure dahili yük dengeleyicisi bir veya birden çok IP adresi barındırabilir. Her IP adresi belirli bir sonda bağlantı noktası kullanır. Bu belge, bir yük dengeleyicisi oluşturmak veya SQL Server kullanılabilirlik grupları için varolan bir yük dengeleyicisine IP adresleri eklemek için PowerShell'in nasıl kullanılacağını gösterir. 

Bir dahili yük dengeleyicisine birden çok IP adresi atama olanağı Azure için yenidir ve yalnızca Kaynak Yöneticisi modelinde kullanılabilir. Bu görevi tamamlamak için, Kaynak Yöneticisi modelinde Azure sanal makinelerde dağıtılan bir SQL Server kullanılabilirlik grubuna sahip olmanız gerekir. Her iki SQL Server sanal makinesi de aynı kullanılabilirlik kümesine ait olmalıdır. Azure Kaynak Yöneticisi'nde kullanılabilirlik grubunu otomatik olarak oluşturmak için [Microsoft şablonunu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kullanabilirsiniz. Bu şablon, sizin için dahili yük dengeleyicisi de dahil olmak üzere kullanılabilirlik grubunu otomatik olarak oluşturur. İsterseniz, bir [Always On kullanılabilirlik grubunu el ile yapılandırabilirsiniz.](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

Bu konu, kullanılabilirlik gruplarınızın zaten yapılandırılmış olması gerektiğini gerektirir.  

İlgili konular şunlardır:

* [Azure VM'de (GUI) Her Zaman Kullanılabilirlik Gruplarını Yapılandırma](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Azure Resource Manager ve PowerShell kullanarak bir Sanal Ağdan Sanal Ağa bağlantısı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

[!INCLUDE [Start your PowerShell session](../../../../includes/sql-vm-powershell.md)]

## <a name="verify-powershell-version"></a>PowerShell sürümünü doğrulayın

Bu makaledeki örnekler Azure PowerShell modül sürümü 5.4.1 kullanılarak test edilmiştir.

PowerShell modülünüzün 5.4.1 veya daha sonra olduğunu doğrulayın.

Bkz. [Azure PowerShell modüllerini yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)

## <a name="configure-the-windows-firewall"></a>Windows Güvenlik Duvarını Yapılandırma

Windows Güvenlik Duvarını SQL Server erişimine izin verecek şekilde yapılandırın. Güvenlik duvarı kuralları, SQL Server örneğinin kullandığı bağlantı noktalarına tcp bağlantılarıve dinleyici sondası sağlar. Ayrıntılı yönergeler için veritabanı [motoru erişimi için bir Windows Güvenlik Duvarı Yapılandır'](https://msdn.microsoft.com/library/ms175043.aspx#Anchor_1)a bakın. SQL Server bağlantı noktası ve sonda bağlantı noktası için gelen bir kural oluşturun.

Bir Azure Ağ Güvenlik Grubu ile erişimi kısıtlıyorsanız, izin kurallarının arka uç SQL Server VM IP adreslerini ve varsa AG dinleyicisi ve küme çekirdeği IP adresi için yük bakiyesi kayan IP adreslerini içerdiğinden emin olun.

## <a name="determine-the-load-balancer-sku-required"></a>Gerekli yük dengeleyiciSku SKU'yu belirleyin

[Azure yük dengeleyicisi](../../../load-balancer/load-balancer-overview.md) 2 SCARI: Basic & Standard'da mevcuttur. Standart yük dengeleyici önerilir. Sanal makineler bir kullanılabilirlik kümesindeyse, temel yük dengeleyiciye izin verilir. Sanal makineler kullanılabilirlik bölgesindeyse, standart bir yük dengeleyicisi gereklidir. Standart yük dengeleyici, tüm VM IP adreslerinin standart IP adreslerini kullanmasını gerektirir.

Kullanılabilirlik grubu için geçerli [Microsoft şablonu,](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) temel IP adreslerine sahip temel bir yük dengeleyicisi kullanır.

   > [!NOTE]
   > Bulut tanığı için standart bir yük dengeleyicisi ve Azure Depolama kullanıyorsanız, bir [hizmet bitiş noktası](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network) yapılandırmanız gerekir. 


Bu makaledeki örneklerde standart yük dengeleyicisi belirtilmektedir. Örneklerde, komut dosyası `-sku Standard`içerir.

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe -sku Standard
```

Temel bir yük dengeleyicisi oluşturmak için yük dengeleyicisini oluşturan çizgiden kaldırın. `-sku Standard` Örnek:

```powershell
$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe
```

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Örnek Komut Dosyası: PowerShell ile bir dahili yük dengeleyicisi oluşturun

> [!NOTE]
> Kullanılabilirlik grubunuzu Microsoft [şablonuyla](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)oluşturduysanız, dahili yük dengeleyicisi zaten oluşturulmuştur.

Aşağıdaki PowerShell komut dosyası bir dahili yük dengeleyicisi oluşturur, yük dengeleme kurallarını yapılandırır ve yük dengeleyicisi için bir IP adresi ayarlar. Komut dosyasını çalıştırmak için Windows PowerShell ISE'yi açın ve komut dosyasını Komut Dosyası bölmesine yapıştırın. PowerShell'e giriş yapmak için kullanın. `Connect-AzAccount` Birden çok Azure aboneliğiniz `Select-AzSubscription` varsa, aboneliği ayarlamak için kullanın. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the front-end configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the back-end configuration

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($vm.NetworkProfile.NetworkInterfaces.Id.split('/') | select -last 1)
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzNetworkInterface -NetworkInterface $NIC
        start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a><a name="Add-IP"></a>Örnek komut dosyası: PowerShell ile varolan bir yük dengeleyicisine IP adresi ekleme
Birden fazla kullanılabilirlik grubu kullanmak için yük bakiyesine ek bir IP adresi ekleyin. Her IP adresi kendi yük dengeleme kuralını, sonda bağlantı noktasını ve ön bağlantı noktasını gerektirir.

Ön uç bağlantı noktası, uygulamaların SQL Server örneğine bağlanmak için kullandığı bağlantı noktasıdır. Farklı kullanılabilirlik grupları için IP adresleri aynı ön uç bağlantı noktasını kullanabilir.

> [!NOTE]
> SQL Server kullanılabilirlik grupları için her IP adresi belirli bir sonda bağlantı noktası gerektirir. Örneğin, bir yük dengeleyicisindeki bir IP adresi prob bağlantı noktası 59999 kullanıyorsa, bu yük dengeleyicisindeki başka hiçbir IP adresi prob bağlantı noktası 59999'u kullanamaz.

* Yük dengeleyici limitleri hakkında daha fazla bilgi için Ağ Sınırları - Azure Kaynak Yöneticisi altında **yük bakiyeleyici başına Özel ön uç IP'ye** bakın. [Networking Limits - Azure Resource Manager](../../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits)
* Kullanılabilirlik grubu sınırları hakkında bilgi için [Bkz. Kısıtlamalar (Kullanılabilirlik Grupları)](https://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Aşağıdaki komut dosyası, varolan bir yük dengeleyicisine yeni bir IP adresi ekler. ILB, yük dengeleme ön uç bağlantı noktası için dinleyici bağlantı noktasını kullanır. Bu bağlantı noktası, SQL Server'ın dinlediği bağlantı noktası olabilir. SQL Server'ın varsayılan örnekleri için bağlantı noktası 1433'tür. Kullanılabilirlik grubu için yük dengeleme kuralı kayan bir IP (doğrudan sunucu dönüşü) gerektirir, bu nedenle arka uç bağlantı noktası ön uç bağlantı noktasıyla aynıdır. Ortamınız için değişkenleri güncelleştirin. 

```powershell
# Connect-AzAccount
# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzLoadBalancer 

$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzLoadBalancer   
```

## <a name="configure-the-listener"></a>Dinleyiciyi yapılandırma

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-the-listener-port-in-sql-server-management-studio"></a>SQL Server Management Studio'da dinleyici bağlantı noktasını ayarlama

1. SQL Server Management Studio'yu başlatın ve birincil yinelemeye bağlanın.

1. **AlwaysOn Yüksek Kullanılabilirlik Kullanılabilirlik** | **Grupları** | **Kullanılabilirlik Grubu Dinleyiciler**gidin. 

1. Artık Failover Cluster Manager'da oluşturduğunuz dinleyici adını görmeniz gerekir. Dinleyici adını sağ tıklatın ve **Özellikler'i**tıklatın.

1. Bağlantı **Noktası** kutusunda, daha önce kullandığınız $EndpointPort (varsayılan 1433'tü) kullanarak kullanılabilirlik grubu dinleyicisinin bağlantı noktası numarasını belirtin, ardından **Tamam'ı**tıklatın.

## <a name="test-the-connection-to-the-listener"></a>Dinleyiciye olan bağlantıyı test etme

Bağlantıyı test etmek için:

1. RDP aynı sanal ağda olan bir SQL Server için, ancak çoğaltma sahibi değil. Bu kümedeki diğer SQL Server olabilir.

1. Bağlantıyı test etmek için **sqlcmd** yardımcı programını kullanın. Örneğin, aşağıdaki komut dosyası, Windows kimlik doğrulaması ile dinleyici aracılığıyla birincil yinelemeiçin bir **sqlcmd** bağlantısı kurar:
   
    ```
    sqlcmd -S <listenerName> -E
    ```
   
    Dinleyici varsayılan bağlantı noktası (1433) dışında bir bağlantı noktası kullanıyorsa, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komutu 1435 bağlantı noktasındaki bir dinleyiciye bağlanır: 
   
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQLCMD bağlantısı, birincil yinelemeyi barındıran SQL Server'ın hangi örneğine otomatik olarak bağlanır. 

> [!NOTE]
> Her iki SQL Server'ın güvenlik duvarında belirttiğiniz bağlantı noktasının açık olduğundan emin olun. Her iki sunucu da kullandığınız TCP bağlantı noktası için gelen bir kural gerektirir. Daha fazla bilgi için [Güvenlik Duvarı Kuralı Ekle veya Edit'e](https://technet.microsoft.com/library/cc753558.aspx) bakın. 
> 
> 

## <a name="guidelines-and-limitations"></a>Yönergeler ve sınırlamalar
Dahili yük dengeleyicisini kullanarak Azure'daki kullanılabilirlik grubu dinleyicisi ile ilgili aşağıdaki yönergelere dikkat edin:

* Dahili yük dengeleyicisi ile dinleyiciye yalnızca aynı sanal ağ dan erişebilirsiniz.

* Bir Azure Ağ Güvenlik Grubu ile erişimi kısıtlıyorsanız, izin kurallarının arka uç SQL Server VM IP adreslerini ve varsa AG dinleyicisi ve küme çekirdeği IP adresi için yük bakiyesi kayan IP adreslerini içerdiğinden emin olun.

* Bulut tanığı için Azure Depolama ile standart bir yük dengeleyicisi kullanırken bir hizmet bitiş noktası oluşturun. Daha fazla bilgi için bkz: [Sanal ağdan Erişim Tanına.](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
Daha fazla bilgi için azure [VM'deki her zaman kullanılabilirlik grubunda her zaman yapılandır'a](virtual-machines-windows-portal-sql-availability-group-tutorial.md)bakın.

## <a name="powershell-cmdlets"></a>PowerShell cmdlet'leri
Azure sanal makineleri için bir dahili yük dengeleyicisi oluşturmak için aşağıdaki PowerShell cmdlets'i kullanın.

* [Yeni-AzLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) bir yük dengeleyici oluşturur. 
* [Yeni-AzLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) bir yük dengeleyici için bir ön uç IP yapılandırma oluşturur. 
* [New-AzLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) bir yük dengeleyici için bir kural yapılandırmaoluşturur. 
* [New-AzLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) yük dengeleyicisi için bir arka uç adresi havuzu yapılandırması oluşturur. 
* [New-AzLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) yük dengeleyicisi için bir prob yapılandırması oluşturur.
* [Remove-AzLoadBalancer,](https://msdn.microsoft.com/library/mt603862.aspx) bir Azure kaynak grubundan bir yük dengeleyicisi kaldırır.
