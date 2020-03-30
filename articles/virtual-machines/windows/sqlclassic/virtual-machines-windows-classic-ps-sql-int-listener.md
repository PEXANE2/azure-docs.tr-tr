---
title: Kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın (Klasik)
description: Bu öğretici, klasik dağıtım modeliyle oluşturulan kaynakları kullanır ve Azure'da dahili yük dengeleyicisi kullanan bir SQL Server VM için her zaman kullanılabilirlik grubu dinleyicisi oluşturur.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77j
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: f26c5a6c6fc2774d19beaa021015357a1991f0ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978164"
---
# <a name="configure-an-ilb-listener-for-availability-groups-on-azure-sql-server-vms"></a>Azure SQL Server VM'lerde kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırma
> [!div class="op_single_selector"]
> * [Dahili dinleyici](../classic/ps-sql-int-listener.md)
> * [Harici dinleyici](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Kaynak Yöneticisi ve klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, klasik dağıtım modelinin kullanımı kapsA. Yeni dağıtımların çoğunun Kaynak Yöneticisi modelini kullanmasını öneririz.

Kaynak Yöneticisi modelinde Her Zaman Açık kullanılabilirlik grubu için bir dinleyiciyi yapılandırmak [için, Azure'daki Her Zaman kullanılabilirlik grubu için bir yük dengeleyicisini yapılandır'](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md)ı 'ya bakın.

Kullanılabilirlik grubunuz, yalnızca şirket içi veya yalnızca Azure'da veya karma yapılandırmalar için hem şirket içi hem de Azure'a yayılan yinelemeler içerebilir. Azure yinelemeleri aynı bölgede veya birden çok sanal ağ kullanan birden çok bölgede bulunabilir. Bu makaledeki yordamlar, bir [kullanılabilirlik grubunu](../classic/portal-sql-alwayson-availability-groups.md) zaten yapılandırdığınızı, ancak henüz bir dinleyiciyi yapılandırmadığınızı varsayar.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>İç dinleyiciler için yönergeler ve sınırlamalar
Azure'da kullanılabilirlik grubu dinleyicisi olan bir dahili yük dengeleyicisinin (ILB) kullanımı aşağıdaki yönergelere tabidir:

* Kullanılabilirlik grubu dinleyicisi Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2'de desteklenir.
* Dinleyici ILB'ye göre yapılandırıldığından ve her bulut hizmeti için yalnızca bir ILB olduğundan, her bulut hizmeti için yalnızca bir dahili kullanılabilirlik grubu dinleyicisi desteklenir. Ancak, birden çok dış dinleyici oluşturmak mümkündür. Daha fazla bilgi için, [Azure'daki Her Zaman Kullanılabilirlik Grupları için harici dinleyici yi yapılandırma'ya](../classic/ps-sql-ext-listener.md)bakın.

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyicinin erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makalede, Bir ILB kullanan bir dinleyici oluşturmaya odaklanır. Ortak veya harici bir dinleyiciye ihtiyacınız varsa, bu makalenin harici bir [dinleyici](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)ayarlamayı anlatan sürümüne bakın.

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Doğrudan sunucu iadesi ile yük dengeli VM uç noktaları oluşturma
Önce bu bölümde daha sonra komut dosyası çalıştırarak bir ILB oluşturun.

Azure yinelemesi barındıran her VM için yük dengeli bir bitiş noktası oluşturun. Birden çok bölgede yinelemeler varsa, bu bölge için her yineleme aynı Azure sanal ağında aynı bulut hizmetinde olmalıdır. Birden çok Azure bölgesine yayılan kullanılabilirlik grubu yinelemeleri oluşturmak için birden çok sanal ağ yapılandırmak gerekiyor. Çapraz sanal ağ bağlantısını yapılandırma hakkında daha fazla bilgi [için](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)bkz.

1. Azure portalında, ayrıntıları görüntülemek için bir yineleme barındıran her VM'ye gidin.

2. Her VM için **Uç Noktaları** sekmesini tıklatın.

3. Kullanmak istediğiniz dinleyici bitiş noktasının **Adı** ve **Ortak Bağlantı Noktası'nın** zaten kullanılmadığını doğrulayın. Bu bölümdeki örnekte, adı *MyEndpoint*ve bağlantı noktası *1433*olduğunu.

4. Yerel istemcinizde, en son [PowerShell modüllerini](https://azure.microsoft.com/downloads/)indirin ve kurun.

5. Azure PowerShell'i başlatın.  
    Azure yönetim modülleri yüklendi.

6. `Get-AzurePublishSettingsFile` öğesini çalıştırın. Bu cmdlet, yayımlama ayarları dosyasını yerel bir dizine indirmek için sizi bir tarayıcıya yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilir.

7. İndirdiğiniz `Import-AzurePublishSettingsFile` yayımlama ayarları dosyasının yolu ile aşağıdaki komutu çalıştırın:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Yayımlama ayarları dosyası alındıktan sonra, Azure aboneliğinizi PowerShell oturumunda yönetebilirsiniz.

8. *ILB*için statik bir IP adresi atayın. Aşağıdaki komutu çalıştırarak geçerli sanal ağ yapılandırmasını inceleyin:

        (Get-AzureVNetConfig).XMLConfiguration
9. Yinelemeleri barındıran VM'leri içeren alt ağ için *Alt net* adını not edin. Bu ad, komut dosyasındaki $SubnetName parametresinde kullanılır.

10. Çoğaltmaları barındıran VM'leri içeren alt ağ için *VirtualNetworkSite* adını ve başlangıç *AdresÖnek'ini* not edin. Her iki değeri `Test-AzureStaticVNetIP` de komuta geçirerek ve Kullanılabilir *Adresler'i*inceleyerek kullanılabilir bir IP adresi arayın. Örneğin, sanal ağın adı *MyVNet* ise ve *172.16.0.128'de*başlayan bir alt net adresi aralığı varsa, aşağıdaki komut kullanılabilir adresleri listeler:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Kullanılabilir adreslerden birini seçin ve bir sonraki adımda komut dosyasının $ILBStaticIP parametresinde kullanın.

12. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişken değerlerini ortamınıza uyacak şekilde ayarlayın. Varsayılanlar bazı parametreler için sağlanmıştır.  

    Yakınlık gruplarını kullanan varolan dağıtımlar Bir ILB ekleyemez. ILB gereksinimleri hakkında daha fazla bilgi için [İç yük dengeleyicisi genel görünümüne](../../../load-balancer/load-balancer-internal-overview.md)bakın.

    Ayrıca, kullanılabilirlik grubunuz Azure bölgelerine yayılıyorsa, bulut hizmeti ve bu veri merkezinde bulunan düğümler için komut dosyasını her veri merkezinde bir kez çalıştırmanız gerekir.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. Değişkenleri ayarladıktan sonra, çalıştırmak için komut dosyasını metin düzenleyicisinden PowerShell oturumunuza kopyalayın. Komut istemi hala **>>** gösteriliyorsa, komut dosyasının çalışmaya başladığından emin olmak için Enter'a yeniden basın.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Gerekirse KB2854082 yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümlerinde güvenlik duvarı bağlantı noktalarını açma
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini oluşturma

Kullanılabilirlik grubu dinleyicisini iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturmak ve bağımlılıkları yapılandırmak. İkinci olarak, PowerShell'deki küme kaynaklarını yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktasını oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'deki küme kaynaklarını yapılandırma
1. ILB için, daha önce oluşturulan ILB IP adresini kullanmanız gerekir. PowerShell'de bu IP adresini elde etmek için aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. VM'lerden birinde, işletim sisteminizin PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişkenleri daha önce belirttiğiniz değerlere ayarlayın.

    Windows Server 2012 veya sonraki için aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Windows Server 2008 R2 için aşağıdaki komut dosyasını kullanın:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP address resource name
        $ILBIP = "<X.X.X.X>" # the IP address of the ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Değişkenleri ayarladıktan sonra, yükseltilmiş bir Windows PowerShell penceresi açın, metni metin düzenleyicisinden PowerShell oturumunuza yapıştırın. Komut istemi hala **>>** gösteriliyorsa, komut dosyasının çalışmaya başladığından emin olmak için Enter tuşuna basın.

4. Her VM için önceki adımları yineleyin.  
    Bu komut dosyası, IP adresi kaynağını bulut hizmetinin IP adresiyle yapılandırır ve sonda bağlantı noktası gibi diğer parametreleri ayarlar. IP adresi kaynağı çevrimiçi duruma getirildiğinde, sonda bağlantı noktasındaki yoklamalara daha önce oluşturduğunuz yük dengeli bitiş noktasından yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyiciyi çevrimiçi duruma getirin
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-virtual-network"></a>Kullanılabilirlik grubu dinleyicisini test edin (aynı sanal ağ içinde)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
