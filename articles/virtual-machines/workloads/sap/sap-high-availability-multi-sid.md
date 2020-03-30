---
title: Azure'da SAP multi-SID yapılandırması oluşturma | Microsoft Dokümanlar
description: Windows sanal makinelerde yüksek kullanılabilirlik SAP NetWeaver multi-SID yapılandırma kılavuzu
services: virtual-machines-windows, virtual-network, storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 0b89b4f8-6d6c-45d7-8d20-fe93430217ca
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/05/2017
ms.author: radeltch
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7938f7db22f004a0bf6cdf2e22dc8e103896719
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617391"
---
# <a name="create-an-sap-netweaver-multi-sid-configuration"></a>SAP NetWeaver multi-SID yapılandırması oluşturma

[load-balancer-multivip-overview]:../../../load-balancer/load-balancer-multivip-overview.md
[sap-ha-guide]:sap-high-availability-guide.md 
[sap-ha-guide-figure-6001]:./media/virtual-machines-shared-sap-high-availability-guide/6001-sap-multi-sid-ascs-scs-sid1.png
[sap-ha-guide-figure-6002]:./media/virtual-machines-shared-sap-high-availability-guide/6002-sap-multi-sid-ascs-scs.png
[sap-ha-guide-figure-6003]:./media/virtual-machines-shared-sap-high-availability-guide/6003-sap-multi-sid-full-landscape.png
[sap-ha-guide-figure-6004]:./media/virtual-machines-shared-sap-high-availability-guide/6004-sap-multi-sid-dns.png
[sap-ha-guide-figure-6005]:./media/virtual-machines-shared-sap-high-availability-guide/6005-sap-multi-sid-azure-portal.png
[sap-ha-guide-figure-6006]:./media/virtual-machines-shared-sap-high-availability-guide/6006-sap-multi-sid-sios-replication.png
[networking-limits-azure-resource-manager]:../../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits
[sap-ha-guide-9.1.1]:sap-high-availability-guide.md#a97ad604-9094-44fe-a364-f89cb39bf097 
[sap-ha-guide-8.8]:sap-high-availability-guide.md#f19bd997-154d-4583-a46e-7f5a69d0153c
[sap-ha-guide-8.12.3.3]:sap-high-availability-guide.md#d9c1fc8e-8710-4dff-bec2-1f535db7b006 
[sap-ha-guide-9]:sap-high-availability-guide.md#a06f0b49-8a7a-42bf-8b0d-c12026c5746b
[sap-ha-guide-9.1]:sap-high-availability-guide.md#31c6bd4f-51df-4057-9fdf-3fcbc619c170 
[sap-ha-guide-9.1.2]:sap-high-availability-guide.md#eb5af918-b42f-4803-bb50-eff41f84b0b0 
[sap-ha-guide-9.1.3]:sap-high-availability-guide.md#e4caaab2-e90f-4f2c-bc84-2cd2e12a9556 
[sap-ha-guide-9.1.4]:sap-high-availability-guide.md#10822f4f-32e7-4871-b63a-9b86c76ce761 
[sap-ha-guide-9.4]:sap-high-availability-guide.md#094bc895-31d4-4471-91cc-1513b64e406a 
[sap-ha-guide-9.5]:sap-high-availability-guide.md#2477e58f-c5a7-4a5d-9ae3-7b91022cafb5 
[sap-ha-guide-9.6]:sap-high-availability-guide.md#0ba4a6c1-cc37-4bcf-a8dc-025de4263772 
[sap-ha-guide-10]:sap-high-availability-guide.md#18aa2b9d-92d2-4c0e-8ddd-5acaabda99e9

Eylül 2016'da Microsoft, azure dahili yük dengeleyicisi kullanarak birden çok sanal IP adresini yönetebileceğiniz bir özellik yayımladı. Bu işlev Azure dış yük dengeleyicisinde zaten var.

SAP dağıtımınız varsa, [Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver kılavuzunda][sap-ha-guide]belirtildiği gibi SAP ASCS/SCS için bir Windows kümesi yapılandırması oluşturmak için dahili yük dengeleyicisi kullanabilirsiniz.

Bu makalede, varolan bir Windows Server Failover Clustering (WSFC) kümesine ek SAP ASCS/SCS kümelenmiş örnekleri yükleyerek tek bir ASCS/SCS yüklemesinden SAP multi-SID yapılandırmasına nasıl taşınış gerektiği üzerinde duruluyor. Bu işlem tamamlandığında, bir SAP multi-SID kümesi ni yapılandırmış olursunuz.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar
[Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver kılavuzunda][sap-ha-guide] açıklandığı ve bu diyagramda gösterildiği gibi, bir SAP ASCS/SCS örneği için kullanılan bir WSFC kümesini zaten yapılandırdınız.

![Yüksek kullanılabilirlik SAP ASCS/SCS örneği][sap-ha-guide-figure-6001]

## <a name="target-architecture"></a>Hedef mimari

Amaç, burada gösterildiği gibi, aynı WSFC kümesinde birden çok SAP ABAP ASCS veya SAP Java SCS kümelenmiş örnekleri yüklemektir:

![Azure'da birden çok SAP ASCS/SCS kümelenmiş örnekleri][sap-ha-guide-figure-6002]

> [!NOTE]
>Her Azure dahili yük dengeleyicisi için özel ön uç IP sayısının bir sınırı vardır.
>
>Bir WSFC kümesindeki maksimum SAP ASCS/SCS örneği, her Azure dahili yük dengeleyicisi için maksimum özel ön uç IP sayısına eşittir.
>

Yük bakiyesi sınırları hakkında daha fazla bilgi için Ağ sınırlarındaki "Yük bakiyesi başına özel ön uç IP IP"e [bakın: Azure Kaynak Yöneticisi.][networking-limits-azure-resource-manager]

İki yüksek kullanılabilirlik SAP sistemi ile tam manzara şu na benzer:

![İki SAP sistemi SID ile SAP yüksek kullanılabilirlikli multi-SID kurulumu][sap-ha-guide-figure-6003]

> [!IMPORTANT]
> Kurulum aşağıdaki koşulları karşılamalıdır:
> - SAP ASCS/SCS örnekleri aynı WSFC kümesini paylaşmalıdır.
> - Her DBMS SID'nin kendi özel WSFC kümesi olmalıdır.
> - Bir SAP sistemi SID'ye ait SAP uygulama sunucularının kendi özel VM'leri olmalıdır.


## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama
Altyapınızı hazırlamak için aşağıdaki parametrelerle ek bir SAP ASCS/SCS örneği yükleyebilirsiniz:

| Parametre adı | Değer |
| --- | --- |
| SAP ASCS/SCS SID |pr1-lb-ascs |
| SAP DBMS dahili yük dengeleyicisi | PR5 |
| SAP sanal ana bilgisayar adı | pr5-sap-cl |
| SAP ASCS/SCS sanal ana bilgisayar IP adresi (ek Azure yük bakiyesi IP adresi) | 10.0.0.50 |
| SAP ASCS/SCS örnek numarası | 50 |
| Ek SAP ASCS/SCS örneği için ILB prob bağlantı noktası | 62350 |

> [!NOTE]
> SAP ASCS/SCS küme örnekleri için her IP adresi benzersiz bir sonda bağlantı noktası gerektirir. Örneğin, Bir Azure dahili yük dengeleyicisindeki bir IP adresi sonda bağlantı noktası 62300 kullanıyorsa, bu yük dengeleyicisindeki başka hiçbir IP adresi sonda bağlantı noktası 62300'i kullanamaz.
>
>Bizim amaçlarımız için, sonda bağlantı noktası 62300 zaten rezerve olduğundan, biz prob portu 62350 kullanıyoruz.

Varolan WSFC kümesine iki düğümle ek SAP ASCS/SCS örnekleri yükleyebilirsiniz:

| Sanal makine rolü | Sanal makine ana bilgisayar adı | Statik IP adresi |
| --- | --- | --- |
| ASCS/SCS örneği için 1. |pr1-ascs-0 |10.0.0.10 |
| ASCS/SCS örneği için 2. |pr1-ascs-1 |10.0.0.9 |

### <a name="create-a-virtual-host-name-for-the-clustered-sap-ascsscs-instance-on-the-dns-server"></a>DNS sunucusunda kümelenmiş SAP ASCS/SCS örneği için sanal bir ana bilgisayar adı oluşturma

Aşağıdaki parametreleri kullanarak ASCS/SCS örneğinin sanal ana bilgisayar adı için bir DNS girişi oluşturabilirsiniz:

| Yeni SAP ASCS/SCS sanal ana bilgisayar adı | İlişkili IP adresi |
| --- | --- |
|pr5-sap-cl |10.0.0.50 |

Yeni ana bilgisayar adı ve IP adresi, aşağıdaki ekran görüntüsünde gösterildiği gibi DNS Yöneticisi'nde görüntülenir:

![Yeni SAP ASCS/SCS küme sanal adı ve TCP/IP adresi için tanımlanan DNS girişini vurgulayan DNS Yöneticisi listesi][sap-ha-guide-figure-6004]

DNS girişi oluşturma yordamı, [Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver için][sap-ha-guide-9.1.1]ana kılavuzda ayrıntılı olarak açıklanmıştır.

> [!NOTE]
> Ek ASCS/SCS örneğinin sanal ana bilgisayar adına atadığınız yeni IP adresi, SAP Azure yük bakiyecine atadığınız yeni IP adresiyle aynı olmalıdır.
>
>Senaryomuzda IP adresi 10.0.0.50'dir.

### <a name="add-an-ip-address-to-an-existing-azure-internal-load-balancer-by-using-powershell"></a>PowerShell'i kullanarak mevcut bir Azure dahili yük dengeleyicisine IP adresi ekleme

Aynı WSFC kümesinde birden fazla SAP ASCS/SCS örneği oluşturmak için, varolan bir Azure dahili yük dengeleyicisine IP adresi eklemek için PowerShell'i kullanın. Her IP adresi kendi yük dengeleme kurallarını, sonda bağlantı noktasını, ön uç IP havuzunu ve arka uç havuzunu gerektirir.

Aşağıdaki komut dosyası, varolan bir yük dengeleyicisine yeni bir IP adresi ekler. Ortamınız için PowerShell değişkenlerini güncelleştirin. Komut dosyası, tüm SAP ASCS/SCS bağlantı noktaları için gerekli tüm yük dengeleme kurallarını oluşturur.

```powershell

# Select-AzSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>
Clear-Host
$ResourceGroupName = "SAP-MULTI-SID-Landscape"      # Existing resource group name
$VNetName = "pr2-vnet"                        # Existing virtual network name
$SubnetName = "Subnet"                        # Existing subnet name
$ILBName = "pr2-lb-ascs"                      # Existing ILB name                      
$ILBIP = "10.0.0.50"                          # New IP address
$VMNames = "pr2-ascs-0","pr2-ascs-1"          # Existing cluster virtual machine names
$SAPInstanceNumber = 50                       # SAP ASCS/SCS instance number: must be a unique value for each cluster
[int]$ProbePort = "623$SAPInstanceNumber"     # Probe port: must be a unique value for each IP and load balancer

$ILB = Get-AzLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName

$count = $ILB.FrontendIpConfigurations.Count + 1
$FrontEndConfigurationName ="lbFrontendASCS$count"
$LBProbeName = "lbProbeASCS$count"

# Get the Azure VNet and subnet
$VNet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName
$Subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

# Add second front-end and probe configuration
Write-Host "Adding new front end IP Pool '$FrontEndConfigurationName' ..." -ForegroundColor Green
$ILB | Add-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id
$ILB | Add-AzLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 10  | Set-AzLoadBalancer

# Get new updated configuration
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
# Get new updated LP FrontendIP COnfig
$FEConfig = Get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

# Add new back-end configuration into existing ILB
$BackEndConfigurationName  = "backendPoolASCS$count"
Write-Host "Adding new backend Pool '$BackEndConfigurationName' ..." -ForegroundColor Green
$BEConfig = Add-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB | Set-AzLoadBalancer

# Get new updated config
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

# Assign VM NICs to backend pool
$BEPool = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
foreach($VMName in $VMNames){
        $VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)        
        $NIC = Get-AzNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName                
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools += $BEPool
        Write-Host "Assigning network card '$NICName' of the '$VMName' VM to the backend pool '$BackEndConfigurationName' ..." -ForegroundColor Green
        Set-AzNetworkInterface -NetworkInterface $NIC
        #start-AzVM -ResourceGroupName $ResourceGroupName -Name $VM.Name
}


# Create load-balancing rules
$Ports = "445","32$SAPInstanceNumber","33$SAPInstanceNumber","36$SAPInstanceNumber","39$SAPInstanceNumber","5985","81$SAPInstanceNumber","5$SAPInstanceNumber`13","5$SAPInstanceNumber`14","5$SAPInstanceNumber`16"
$ILB = Get-AzLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName
$FEConfig = get-AzLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB
$BEConfig = Get-AzLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB
$HealthProbe  = Get-AzLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

Write-Host "Creating load balancing rules for the ports: '$Ports' ... " -ForegroundColor Green

foreach ($Port in $Ports) {

        $LBConfigrulename = "lbrule$Port" + "_$count"
        Write-Host "Creating load balancing rule '$LBConfigrulename' for the port '$Port' ..." -ForegroundColor Green

        $ILB | Add-AzLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $HealthProbe -Protocol tcp -FrontendPort  $Port -BackendPort $Port -IdleTimeoutInMinutes 30 -LoadDistribution Default -EnableFloatingIP   
}

$ILB | Set-AzLoadBalancer

Write-Host "Successfully added new IP '$ILBIP' to the internal load balancer '$ILBName'!" -ForegroundColor Green

```
Komut dosyası çalıştırıladıktan sonra, sonuçlar aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında görüntülenir:

![Azure portalında yeni ön uç IP havuzu][sap-ha-guide-figure-6005]

### <a name="add-disks-to-cluster-machines-and-configure-the-sios-cluster-share-disk"></a>Küme makinelerine disk ekleme ve SIOS küme paylaşım diskini yapılandırma

Her ek SAP ASCS/SCS örneği için yeni bir küme paylaşım diski eklemeniz gerekir. Windows Server 2012 R2 için, şu anda kullanılmakta olan WSFC küme paylaşım diski SIOS DataKeeper yazılım çözümüdür.

Şunları yapın:
1. Küme düğümlerinin her birine aynı boyutta (şeritlemeniz gereken) ek bir disk veya disk ekleyin ve bunları biçimlendirin.
2. Depolama çoğaltmaişlemini SIOS DataKeeper ile yapılandırın.

Bu yordam, WSFC küme makinelerine SIOS DataKeeper'ı yüklediğinizi varsayar. Yüklediyseniz, artık makineler arasında çoğaltmayı yapılandırmanız gerekir. İşlem, [Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver için][sap-ha-guide-8.12.3.3]ana kılavuzda ayrıntılı olarak açıklanmıştır.  

![Yeni SAP ASCS/SCS paylaşım diski için DataKeeper senkron yansıtma][sap-ha-guide-figure-6006]

### <a name="deploy-vms-for-sap-application-servers-and-dbms-cluster"></a>SAP uygulama sunucuları ve DBMS kümesi için VM'leri dağıtma

İkinci SAP sistemi için altyapı hazırlıkını tamamlamak için aşağıdakileri yapın:

1. SAP uygulama sunucuları için özel VM'leri dağıtın ve bunları kendi özel kullanılabilirlik grubuna koyun.
2. DBMS kümesi için özel VM'leri dağıtın ve bunları kendi kullanılabilirlik grubuna koyun.


## <a name="install-the-second-sap-sid2-netweaver-system"></a>İkinci SAP SID2 NetWeaver sistemini kurun

İkinci bir SAP SID2 sistemi yükleme işleminin [tamamı, Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver için][sap-ha-guide-9]ana kılavuzda açıklanmıştır.

Üst düzey yordam aşağıdaki gibidir:

1. [SAP ilk küme düğüm'üne yükleyin.][sap-ha-guide-9.1.2]  
 Bu adımda, **VAROLAN WSFC küme düğümü 1'e**yüksek kullanılabilirlikli BIR ASCS/SCS örneği ile SAP yüklüyorsunuz.

2. [ASCS/SCS örneğinin SAP profilini değiştirin.][sap-ha-guide-9.1.3]

3. [Sonda bağlantı noktasını yapılandırın.][sap-ha-guide-9.1.4]  
 Bu adımda, PowerShell kullanarak bir SAP küme kaynağı SAP-SID2-IP sonda bağlantı noktasını yapılandırıyorsunuz. Bu yapılandırmayı SAP ASCS/SCS küme düğümlerinden birinde yürütün.

4. [Veritabanı örneğini yükleyin] [sap-ha-guide-9.2].  
 Bu adımda, Özel bir WSFC kümesine DBMS yüklüyorsunuz.

5. [İkinci küme düğümyükleme] [sap-ha-guide-9.3].  
 Bu adımda, sap'ı varolan WSFC küme düğümü 2'ye yüksek kullanılabilirlikli BIR ASCS/SCS örneği yle yüklüyorsunuz.

6. SAP ASCS/SCS örneği ve ProbePort için Windows Güvenlik Duvarı bağlantı noktalarını açın.  
 SAP ASCS/SCS örnekleri için kullanılan her iki küme düğümlerinde, SAP ASCS/SCS tarafından kullanılan tüm Windows Güvenlik Duvarı bağlantı noktalarını açıyorsunuz. Bu bağlantı [noktaları, Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver için kılavuzda][sap-ha-guide-8.8]listelenmiştir.  
 Ayrıca senaryomuzda 62350 olan Azure dahili yük dengeleyici sondası bağlantı noktasını da açın.

7. [SAP ERS Windows hizmet örneğinin başlangıç türünü değiştirin.][sap-ha-guide-9.4]

8. [SAP birincil uygulama sunucusunu][sap-ha-guide-9.5] yeni özel VM'ye yükleyin.

9. [SAP ek uygulama sunucusunu][sap-ha-guide-9.6] yeni özel VM'ye yükleyin.

10. [SAP ASCS/SCS örneğini test edin başarısız ve SIOS çoğaltma.][sap-ha-guide-10]

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ sınırları: Azure Kaynak Yöneticisi][networking-limits-azure-resource-manager]
- [Azure Yük Dengeleyicisi için birden çok VIP][load-balancer-multivip-overview]
- [Windows VM'lerde yüksek kullanılabilirlik sap NetWeaver kılavuzu][sap-ha-guide]
