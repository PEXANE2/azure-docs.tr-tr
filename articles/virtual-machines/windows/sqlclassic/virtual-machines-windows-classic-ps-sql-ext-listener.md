---
title: Kullanılabilirlik grupları için harici bir dinleyiciyi yapılandırma
description: Bu öğretici, ilgili bulut hizmetinin herkese açık Sanal IP adresini kullanarak Azure'da dışarıdan erişilebilen bir Her Zaman Kullanılabilirlik Grubu Dinleyicisi oluşturma adımlarında size yol sunar.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: ca13d5e8369d007188a17352913519172ed8744e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978190"
---
# <a name="configure-an-external-listener-for-availability-groups-on-azure-sql-server-vms"></a>Azure SQL Server VM'lerde kullanılabilirlik Grupları için harici bir dinleyiciyi yapılandırma
> [!div class="op_single_selector"]
> * [Dahili Dinleyici](../classic/ps-sql-int-listener.md)
> * [Harici Dinleyici](../classic/ps-sql-ext-listener.md)
> 
> 

Bu konu, internet üzerinden dışarıdan erişilebilen bir Always On Availability Group için dinleyiciyi nasıl yapılandırabileceğinizi gösterir. Bu, bulut hizmetinin genel Sanal IP **(VIP)** adresini dinleyiciyle ilişkilendirerek mümkün olur.

> [!IMPORTANT] 
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Kullanılabilirlik Grubunuz, karma yapılandırmalar için yalnızca şirket içi, yalnızca Azure veya şirket içi ve Azure'a yayılan yinelemeler içerebilir. Azure yinelemeleri, birden çok sanal ağ (VNet) kullanarak aynı bölgede veya birden çok bölgede bulunabilir. Aşağıdaki adımlar, bir [kullanılabilirlik grubunu](../classic/portal-sql-alwayson-availability-groups.md) zaten yapılandırdığınızı, ancak bir dinleyiciyi yapılandırmadığınızı varsayar.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Dış dinleyiciler için yönergeler ve sınırlamalar
Bulut hizmeti genel VIP adresini kullanırken Azure'daki kullanılabilirlik grubu dinleyicisi hakkında aşağıdaki yönergelere dikkat edin:

* Kullanılabilirlik grubu dinleyicisi Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2'de desteklenir.
* İstemci uygulaması, kullanılabilirlik grubu VM'lerinizi içeren den farklı bir bulut hizmetinde bulunmalıdır. Azure, aynı bulut hizmetinde istemci ve sunucuyla doğrudan sunucu iadesini desteklemez.
* Varsayılan olarak, bu makaledeki adımlar bulut hizmeti Sanal IP (VIP) adresini kullanacak şekilde bir dinleyicinin nasıl yapılandırılabildiğini gösterir. Ancak, bulut hizmetiniz için birden fazla VIP adresi rezerve etmek ve oluşturmak mümkündür. Bu, her biri farklı bir VIP ile ilişkili birden çok dinleyici oluşturmak için bu makaledeki adımları kullanmanıza olanak sağlar. Birden çok VIP adresi oluşturma hakkında bilgi için [bulut hizmeti başına birden çok VIP'ye](../../../load-balancer/load-balancer-multivip.md)bakın.
* Karma bir ortam için bir dinleyici oluşturuyorsanız, şirket içi ağın Azure sanal ağıyla siteden siteye VPN'e ek olarak genel Internet bağlantısına sahip olması gerekir. Azure alt netindeyken, kullanılabilirlik grubu dinleyicisine yalnızca ilgili bulut hizmetinin genel IP adresi tarafından ulaşılabilir.
* İç Yük Dengeleyicisi 'ni (ILB) kullanan dahili bir dinleyicinin de bulunduğu aynı bulut hizmetinde harici bir dinleyici oluşturmak desteklenmez.

## <a name="determine-the-accessibility-of-the-listener"></a>Dinleyicinin erişilebilirliğini belirleme
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Bu makalede, **dış yük dengeleme**kullanan bir dinleyici oluşturmaya odaklanır. Sanal ağınıza özel bir dinleyici istiyorsanız, [ILB ile](../classic/ps-sql-int-listener.md) dinleyici ayarlama adımları sağlayan bu makalenin sürümüne bakın

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Doğrudan sunucu iadesi ile yük dengeli VM uç noktaları oluşturma
Dış yük dengeleme, Sanal M'lerinizi barındıran bulut hizmetinin sanal sanal Sanal IP adresini kullanır. Bu nedenle, bu durumda yük dengeleyicisini oluşturmanız veya yapılandırmanız gerekmez.

Azure yinelemesi barındıran her VM için yük dengeli bir bitiş noktası oluşturmanız gerekir. Birden çok bölgede yinelemeler varsa, bu bölge için her yineleme aynı VNet aynı bulut hizmeti nde olmalıdır. Birden çok Azure bölgesine yayılan Kullanılabilirlik Grubu yinelemeleri oluşturma, birden çok VNet yapılandırmayı gerektirir. Çapraz VNet bağlantısını yapılandırma hakkında daha fazla bilgi için [VNet'i VNet Bağlantısına Yapılandırın'a](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)bakın.

1. Azure portalında, çoğaltma barındıran her VM'ye gidin ve ayrıntıları görüntüleyin.
2. VM'lerin her biri için **Uç Noktalar** sekmesini tıklatın.
3. Kullanmak istediğiniz dinleyici bitiş noktasının **Adı** ve **Ortak Bağlantı Noktası'nın** zaten kullanılmadığını doğrulayın. Aşağıdaki örnekte, adı "MyEndpoint" ve bağlantı noktası "1433" olduğunu.
4. Yerel istemcinizde, en [son PowerShell modüllerini indirin](https://azure.microsoft.com/downloads/)ve kurun.
5. **Azure PowerShell'i**başlatın. Azure yönetim modülleri yüklendikleriyle yeni bir PowerShell oturumu açılır.
6. **Get-AzurePublishSettingsFile'ı**çalıştırın. Bu cmdlet, yayımlama ayarları dosyasını yerel bir dizine indirmek için sizi bir tarayıcıya yönlendirir. Azure aboneliğiniz için oturum açma kimlik bilgileriniz istenebilebilir.
7. İndirdiğiniz yayımlama ayarları dosyasının yolu ile **Alma-AzurePublishSettingsFile** komutunu çalıştırın:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Yayımlama ayarları dosyası alındıktan sonra, Azure aboneliğinizi PowerShell oturumunda yönetebilirsiniz.
    
1. Aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişken değerlerini ortamınıza uyacak şekilde ayarlayın (varsayılan değerler bazı parametreler için sağlanmıştır). Kullanılabilirlik grubunuz Azure bölgelerine yayılıyorsa, bulut hizmeti ve bu veri merkezinde bulunan düğümler için komut dosyasını her veri merkezinde bir kez çalıştırmanız gerektiğini unutmayın.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Değişkenleri ayarladıktan sonra, çalıştırmak için metin düzenleyicisinden Azure PowerShell oturumunuza komut dosyasını kopyalayın. Komut istemi hala >> gösteriyorsa, komut dosyasının çalışmaya başladığından emin olmak için ENTER'u yeniden yazın.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Gerekirse KB2854082 yüklü olduğunu doğrulayın
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Kullanılabilirlik grubu düğümlerinde güvenlik duvarı bağlantı noktalarını açma
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisini oluşturma

Kullanılabilirlik grubu dinleyicisini iki adımda oluşturun. İlk olarak, istemci erişim noktası küme kaynağı oluşturmak ve bağımlılıkları yapılandırmak. İkinci olarak, küme kaynaklarını PowerShell ile yapılandırın.

### <a name="create-the-client-access-point-and-configure-the-cluster-dependencies"></a>İstemci erişim noktasını oluşturun ve küme bağımlılıklarını yapılandırın
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-the-cluster-resources-in-powershell"></a>PowerShell'deki küme kaynaklarını yapılandırma
1. Dış yük dengelemesi için, yinelemelerinizi içeren bulut hizmetinin genel sanal IP adresini almanız gerekir. Azure portalında oturum açın. Kullanılabilirlik grubuvm içeren bulut hizmetine gidin. **Pano** görünümünü açın.
2. **Genel Sanal IP (VIP) Adresi**altında gösterilen adrese dikkat edin. Çözümünüz VNet'lere yayılıyorsa, yineleme barındıran bir VM içeren her bulut hizmeti için bu adımı yineleyin.
3. VM'lerden birinde, aşağıdaki PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın ve değişkenleri daha önce belirttiğiniz değerlere ayarlayın.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Değişkenleri ayarladıktan sonra yükseltilmiş bir Windows PowerShell penceresi açın ve ardından metin düzenleyicisinden komut dosyasını kopyalayın ve çalıştırmak için Azure PowerShell oturumunuza yapıştırın. Komut istemi hala >> gösteriyorsa, komut dosyasının çalışmaya başladığından emin olmak için ENTER'u yeniden yazın.
5. Bunu her VM'de tekrarlayın. Bu komut dosyası, IP Adresi kaynağını bulut hizmetinin IP adresiyle yapılandırır ve sonda bağlantı noktası gibi diğer parametreleri ayarlar. IP Adresi kaynağı çevrimiçi duruma getirildiğinde, bu öğreticide daha önce oluşturulan yük dengeli bitiş noktasından sonda bağlantı noktasındaki yoklamalara yanıt verebilir.

## <a name="bring-the-listener-online"></a>Dinleyiciyi çevrimiçi duruma getirin
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>İzleme öğeleri
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Kullanılabilirlik grubu dinleyicisini test edin (aynı VNet içinde)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Kullanılabilirlik grubu dinleyicisini test edin (internet üzerinden)
Dinleyiciye sanal ağ dışından erişmek için, yalnızca aynı VNet içinde erişilebilen ILB yerine harici/genel yük dengeleme (bu konuda açıklanan) kullanıyor olmalısınız. Bağlantı dizesinde bulut hizmeti adını belirtirsiniz. Örneğin, *mycloudservice*adında bir bulut hizmetiniz varsa, sqlcmd deyimi aşağıdaki gibi olacaktır:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Önceki örnekten farklı olarak, arayan internet üzerinden windows kimlik doğrulamasını kullanamadığı için SQL kimlik doğrulaması kullanılmalıdır. Daha fazla bilgi için Azure [VM: İstemci Bağlantı Senaryolarında Her Zaman Kullanılabilirlik Grubunda'ya](https://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx)bakın. SQL kimlik doğrulamasını kullanırken, her iki yinelemede de aynı girişi oluşturduğunuzdan emin olun. Kullanılabilirlik Grupları ile sorun giderme girişleri hakkında daha fazla bilgi için, [diğer yinelemelere bağlanmak ve kullanılabilirlik veritabanlarına eşlemek için oturum açma veya içerdiği SQL veritabanı kullanıcısını nasıl kullanacağına](https://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx)bakın.

Always On kopyaları farklı alt ağlardaysa, istemciler bağlantı dizesinde **MultisubnetFailover=True** belirtmelidir. Bu, farklı alt ağlarda yinelemelere paralel bağlantı girişimleri ile sonuçlanır. Bu senaryonun her zaman kullanılabilirlik grubu dağıtımında bir bölgeler arası durum içerdiğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

