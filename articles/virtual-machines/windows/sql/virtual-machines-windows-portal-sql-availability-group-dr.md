---
title: Kullanılabilirlik grubunu farklı bölgeler arasında yapılandırma
description: Bu makalede, Azure sanal makinelerde farklı bir bölgede çoğaltma ile bir SQL Server kullanılabilirlik grubu nasıl yapılandırılabilen açıklanmaktadır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 388c464e-a16e-4c9d-a0d5-bb7cf5974689
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: 8ca871a6f525d4e68ce70060e6faddbcfc8e1f3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060122"
---
# <a name="configure-an-availability-group-on-azure-sql-server-virtual-machines-in-different-regions"></a>Farklı bölgelerdeki Azure SQL Server sanal makinelerinde kullanılabilirlik grubunu yapılandırma

Bu makalede, uzak bir Azure konumundaki Azure sanal makinelerde sql server her zaman kullanılabilirlik grubu yinelemesinde nasıl yapılandırılanın açıklanmaktadır. Olağanüstü durum kurtarmayı desteklemek için bu yapılandırmayı kullanın.

Bu makale, Kaynak Yöneticisi modundaAzure Sanal Makineler için geçerlidir.

Aşağıdaki resim, Azure sanal makinelerinde kullanılabilirlik grubunun yaygın olarak dağıtılmasını gösterir:

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic.png)

Bu dağıtımda, tüm sanal makineler tek bir Azure bölgesindedir. Kullanılabilirlik grubu yinelemeleri SQL-1 ve SQL-2'de otomatik hata ile eşzamanlı işlemeye sahip olabilir. Bu mimariyi oluşturmak için [Kullanılabilirlik Grubu şablonuna veya öğreticiye](virtual-machines-windows-portal-sql-availability-group-overview.md)bakın.

Azure bölgesine erişilemiyorsa, bu mimari kapalı kalma sürelerine karşı savunmasızdır. Bu güvenlik açığının üstesinden gelmek için farklı bir Azure bölgesine yineleme ekleyin. Aşağıdaki diyagram, yeni mimarinin nasıl görüneceğini gösterir:

   ![Kullanılabilirlik Grubu DR](./media/virtual-machines-windows-portal-sql-availability-group-dr/00-availability-group-basic-dr.png)

Önceki diyagramda SQL-3 adı verilen yeni bir sanal makine gösterilmektedir. SQL-3 farklı bir Azure bölgesindedir. SQL-3, Windows Server Failover Cluster'a eklenir. SQL-3 kullanılabilirlik grubu yinelemebarındırabilir. Son olarak, SQL-3 için Azure bölgesinin yeni bir Azure yük dengeleyicisi olduğunu unutmayın.

>[!NOTE]
> Birden fazla sanal makine aynı bölgede olduğunda Azure kullanılabilirlik kümesi gereklidir. Bölgede yalnızca bir sanal makine varsa, kullanılabilirlik kümesi gerekli değildir. Sanal bir makineyi yalnızca oluşturma sırasında bir kullanılabilirlik kümesine yerleştirebilirsiniz. Sanal makine zaten bir kullanılabilirlik kümesindeyse, daha sonra ek bir yineleme için sanal bir makine ekleyebilirsiniz.

Bu mimaride, uzak bölgedeki yineleme normalde kullanılabilirlik modu ve el ile başarısız modu ile yapılandırılır.

Kullanılabilirlik grubu yinelemeleri farklı Azure bölgelerindeki Azure sanal makinelerinde olduğunda, her bölge şunları gerektirir:

* Sanal ağ ağ geçidi
* Sanal ağ ağ geçidi bağlantısı

Aşağıdaki diyagram, ağların veri merkezleri arasında nasıl iletişim kurduğunu gösterir.

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/01-vpngateway-example.png)

>[!IMPORTANT]
>Bu mimari, Azure bölgeleri arasında çoğaltılan veriler için giden veri ücretlerine neden olur. Bkz. [Bant Genişliği Fiyatlandırması.](https://azure.microsoft.com/pricing/details/bandwidth/)  

## <a name="create-remote-replica"></a>Uzak yineleme oluşturma

Uzak bir veri merkezinde yineleme oluşturmak için aşağıdaki adımları yapın:

1. [Yeni bölgede sanal ağ oluşturun.](../../../virtual-network/manage-virtual-network.md#create-a-virtual-network)

1. [Azure portalını kullanarak VNet'ten VNet'e bağlantı yapılandırıyor.](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)

   >[!NOTE]
   >Bazı durumlarda, VNet-to-VNet bağlantısını oluşturmak için PowerShell'i kullanmanız gerekebilir. Örneğin, farklı Azure hesapları kullanıyorsanız, portaldaki bağlantıyı yapılandıramazsınız. Bu durumda [bkz.](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

1. [Yeni bölgede bir etki alanı denetleyicisi oluşturun.](../../../active-directory/active-directory-new-forest-virtual-machine.md)

   Bu etki alanı denetleyicisi, birincil sitedeki etki alanı denetleyicisi kullanılamıyorsa kimlik doğrulaması sağlar.

1. [Yeni bölgede bir SQL Server sanal makine oluşturun.](virtual-machines-windows-portal-sql-server-provision.md)

1. [Yeni bölgede ağda bir Azure yük dengeleyicisi oluşturun.](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)

   Bu yük dengeleyicisi şunları yapmalı:

   - Yeni sanal makineyle aynı ağda ve alt ağda olun.
   - Kullanılabilirlik grubu dinleyicisi için statik bir IP adresine sahip olmak.
   - Yük dengeleyicisi ile aynı bölgede yalnızca sanal makinelerden oluşan bir arka uç havuzu ekleyin.
   - IP adresine özgü bir TCP bağlantı noktası sondası kullanın.
   - Aynı bölgede SQL Server'a özgü bir yük dengeleme kuralına sahip olun.  
   - Arka uç havuzundaki sanal makineler tek bir kullanılabilirlik kümesinin veya sanal makine ölçeği kümesinin parçası değilse Standart Yük Dengeleyicisi olun. Ek bilgi incelemesi için [Azure Yük Dengeleyici Standardına genel bakış.](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)

1. [Yeni SQL Server'a Failover Kümeleme özelliği ekleyin.](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms)

1. [Etki alanına yeni SQL Server katılın.](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain)

1. [Etki alanı hesabı kullanacak yeni SQL Server hizmet hesabını ayarlayın.](virtual-machines-windows-portal-sql-availability-group-prereq.md#setServiceAccount)

1. [Yeni SQL Server'ı Windows Server Failover Cluster'a ekleyin.](virtual-machines-windows-portal-sql-availability-group-tutorial.md#addNode)

1. Kümeye bir IP adresi kaynağı ekleyin.

   Failover Cluster Manager'da IP adresi kaynağını oluşturabilirsiniz. Kümenin adını seçin, ardından **Küme Çekirdek Kaynakları** altında küme adını sağ tıklatın ve **Özellikler'i**seçin: 

   ![Küme özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-dr/cluster-name-properties.png)

   **Özellikler** iletişim kutusunda, **IP Adresi**altında **Ekle'yi** seçin ve ardından uzak ağ bölgesinden küme adının IP adresini ekleyin. **IP Adresi** iletişim kutusunda **Tamam'ı** seçin ve ardından yeni IP adresini kaydetmek için **Küme Özellikleri** iletişim kutusunda tekrar **Tamam'ı** seçin... 

   ![Küme IP ekle](./media/virtual-machines-windows-portal-sql-availability-group-dr/add-cluster-ip-address.png)


1. IP adresini çekirdek küme adı için bağımlılık olarak ekleyin.

   Küme özelliklerini bir kez daha açın ve **Bağımlılıklar** sekmesini seçin. İki IP adresi için bir OR bağımlılığını yapılandırın: 

   ![Küme özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-dr/cluster-ip-dependencies.png)

1. Kümedeki kullanılabilirlik grubu rolüne bir IP adresi kaynağı ekleyin. 

   Failover Cluster Manager'da kullanılabilirlik grubu rolünü sağ tıklatın, Kaynak Ekle, **Daha Fazla Kaynak** **ekleyin'i**seçin ve **IP Adresi'ni**seçin.

   ![IP Adresi Oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-dr/20-add-ip-resource.png)

   Bu IP adresini aşağıdaki gibi yapılandırın:

   - Uzak veri merkezinden ağı kullanın.
   - IP adresini yeni Azure yük bakiyesinden atayın. 

1. Dinleyici istemci erişim noktası (ağ adı) kümesi için bir bağımlılık olarak IP adresi kaynağı ekleyin.

   Aşağıdaki ekran görüntüsü, düzgün yapılandırılmış bir IP adresi küme kaynağını gösterir:

   ![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-dr/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >Küme kaynak grubu her iki IP adresini de içerir. Her iki IP adresi de dinleyici istemci erişim noktası için bağımlılıklardır. Küme bağımlılık yapılandırmasında **OR** işleci kullanın.

1. [PowerShell'de küme parametrelerini ayarlayın.](virtual-machines-windows-portal-sql-availability-group-tutorial.md#setparam)

PowerShell komut dosyasını, yeni bölgedeki yük dengeleyicisinde yapılandırdığınız küme ağ adı, IP adresi ve sonda bağlantı noktasıyla çalıştırın.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = "<n.n.n.n>" # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

1. SQL Server Configuration Manager'daki yeni SQL Server'da [Her Zaman Kullanılabilirlik Gruplarında'yı etkinleştirin.](/sql/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server)

1. [Yeni SQL Server'da güvenlik duvarı bağlantı noktalarını açın.](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall)

   Açmanız gereken bağlantı noktası numaraları ortamınıza bağlıdır. Yansıtma uç noktası ve Azure yük dengeleyici sabunu sondası için bağlantı noktalarını açın.


1. [Yeni SQL Server'daki kullanılabilirlik grubuna yineleme ekleyin.](/sql/database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio)

   Uzak bir Azure bölgesindeki bir yineleme için, el ile başarısız olan eşzamanlı çoğaltma için ayarlayın.  

## <a name="set-connection-for-multiple-subnets"></a>Birden çok alt ağ için bağlantı ayarlama

Uzak veri merkezindeki yineleme kullanılabilirlik grubunun bir parçasıdır, ancak farklı bir alt ağdadır. Bu yineleme birincil yineleme haline gelirse, uygulama bağlantısı zaman-outs oluşabilir. Bu davranış, çoklu alt ağ dağıtımında şirket içi kullanılabilirlik grubuyla aynıdır. İstemci uygulamalarından bağlantılara izin vermek için istemci bağlantısını güncelleştirin veya küme ağ adı kaynağında ad çözümlemesi yapılandırın.

Tercihen, istemci bağlantı dizeleri `MultiSubnetFailover=Yes`ayarlamak için güncelleştirin. Bkz. [MultiSubnetFailover ile Bağlanma.](https://msdn.microsoft.com/library/gg471494#Anchor_0)

Bağlantı dizelerini değiştiremiyorsanız, ad çözümlemesi önbelleğe alınmış olarak yapılandırabilirsiniz. Zaman çıkış hatasına bakın [ve çok alt ağ ortamında bir SQL Server 2012 AlwaysOn kullanılabilirlik grubu dinleyicisine bağlanamazsınız.](https://support.microsoft.com/help/2792139/time-out-error-and-you-cannot-connect-to-a-sql-server-2012-alwayson-av)

## <a name="fail-over-to-remote-region"></a>Uzak bölgeye geçme

Dinleyicibağlantısını uzak bölgeye sınamak için, çoğaltmadan uzak bölgeye geçebilirsiniz. Yineleme eşzamanlı olsa da, failover olası veri kaybına karşı savunmasızdır. Veri kaybı olmadan başarısız olmak için kullanılabilirlik modunu eşzamanlı olarak değiştirin ve failover modunu otomatik olarak ayarlayın. Aşağıdaki adımları kullanın:

1. **Object**Explorer'da, birincil yinelemeyi barındıran SQL Server örneğine bağlanın.
1. **AlwaysOn Kullanılabilirlik Grupları**altında, **Kullanılabilirlik Grupları,** kullanılabilirlik grubunuzun sağ tıklatın ve **Özellikleri**tıklatın.
1. **Genel** sayfada, **Kullanılabilirlik Yinelemeleri**altında, Dr sitesindeki ikincil yinelemeyi **Synchronous Commit** kullanılabilirlik modunu ve **Otomatik** başarısızlık modunu kullanacak şekilde ayarlayın.
1. Yüksek kullanılabilirlik için birincil yineleme ile aynı sitede ikincil bir yineleme varsa, bu **yinelemeyi Asynchronous Commit** ve **Manual**olarak ayarlayın.
1. Tamam'a tıklayın.
1. **Object Explorer'da**kullanılabilirlik grubunu sağ tıklatın ve **Panoyu Göster'i**tıklatın.
1. Panoda, DR sitesindeki yinelemenin eşitlenmiş olduğundan doğrulayın.
1. **Object Explorer'da**kullanılabilirlik grubunu sağ tıklatın ve **Failover'ı tıklatın...** seçeneğini tıklatın. SQL Server Management Studios, SQL Server üzerinde başarısız olmak için bir sihirbaz açar.  
1. **İleri'yi**tıklatın ve DR sitesindeki SQL Server örneğini seçin. Tekrar **İleri**'ye tıklayın.
1. DR sitesindeki SQL Server örneğine bağlanın ve **İleri'yi**tıklatın.
1. **Özet** sayfasında, ayarları doğrulayın ve **Bitir'i**tıklatın.

Bağlantıyı test ettikten sonra, birincil yinelemeyi birincil veri merkezinize taşıyın ve kullanılabilirlik modunu normal çalışma ayarlarına geri ayarlayın. Aşağıdaki tablo, bu belgede açıklanan mimari için normal çalışma ayarlarını gösterir:

| Konum | Sunucu Örneği | Rol | Kullanılabilirlik Modu | Failover Modu
| ----- | ----- | ----- | ----- | -----
| Birincil veri merkezi | SQL-1 | Birincil | Zaman uyumlu | Automatic
| Birincil veri merkezi | SQL-2 | İkincil | Zaman uyumlu | Automatic
| İkincil veya uzak veri merkezi | SQL-3 | İkincil | Zaman uyumsuz | El ile


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Planlı ve zorunlu el ile başarısız lık hakkında daha fazla bilgi

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Kullanılabilirlik Grubunun (SQL Server) Planlı Manuel Hata Yapma](https://msdn.microsoft.com/library/hh231018.aspx)
- [Kullanılabilirlik Grubunun (SQL Server) Zorla El İle Başarısız Lığı Gerçekleştirme](https://msdn.microsoft.com/library/ff877957.aspx)

## <a name="additional-links"></a>Ek Linkler

* [Her Zaman Kullanılabilirlik Gruplarında](https://msdn.microsoft.com/library/hh510230.aspx)
* [Azure Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure Yük Dengeleyicileri](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer)
* [Azure Kullanılabilirlik Setleri](../manage-availability.md)
