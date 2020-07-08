---
title: Farklı bölgelerde SQL Server Always on kullanılabilirlik grubu yapılandırma
description: Bu makalede, farklı bir bölgedeki bir çoğaltmayla Azure sanal makinelerinde SQL Server Always on kullanılabilirlik grubunun nasıl yapılandırılacağı açıklanmaktadır.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
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
ms.openlocfilehash: 8ab62a93546719e172eec34168a0692daccf281a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669316"
---
# <a name="configure-a-sql-server-always-on-availability-group-across-different-azure-regions"></a>Farklı Azure bölgelerinde SQL Server Always on kullanılabilirlik grubu yapılandırma

[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, uzak bir Azure konumundaki Azure sanal makinelerinde SQL Server Always on kullanılabilirlik grubu çoğaltmasının nasıl yapılandırılacağı açıklanmaktadır. Olağanüstü durum kurtarmayı desteklemek için bu yapılandırmayı kullanın.

Bu makale Kaynak Yöneticisi modundaki Azure sanal makineleri için geçerlidir.

Aşağıdaki görüntüde, Azure sanal makinelerinde bir kullanılabilirlik grubunun ortak dağıtımı gösterilmektedir:

   ![Kullanılabilirlik Grubu](./media/availability-group-manually-configure-multiple-regions/00-availability-group-basic.png)

Bu dağıtımda, tüm sanal makineler tek bir Azure bölgesidir. Kullanılabilirlik grubu çoğaltmalarının, SQL-1 ve SQL-2 ' de otomatik yük devretme ile zaman uyumlu yürütmesi olabilir. Bu mimariyi derlemek için bkz. [kullanılabilirlik grubu şablonu veya öğreticisi](availability-group-overview.md).

Bu mimari, Azure bölgesinin erişilemez hale gelmesi durumunda kapalı kalma süresine karşı savunmasız kalır. Bu güvenlik açığını aşmak için, farklı bir Azure bölgesine bir çoğaltma ekleyin. Aşağıdaki diyagramda yeni mimarinin nasıl görüneceğine gösterilmektedir:

   ![Kullanılabilirlik grubu DR](./media/availability-group-manually-configure-multiple-regions/00-availability-group-basic-dr.png)

Yukarıdaki diyagramda, SQL-3 adlı yeni bir sanal makine gösterilmektedir. SQL-3 farklı bir Azure bölgesidir. SQL-3, Windows Server yük devretme kümesine eklenir. SQL-3, bir kullanılabilirlik grubu çoğaltmasını barındırabilir. Son olarak, SQL-3 için Azure bölgesinin yeni bir Azure yük dengeleyiciye sahip olduğuna dikkat edin.

>[!NOTE]
> Aynı bölgede birden fazla sanal makine olduğunda bir Azure kullanılabilirlik kümesi gereklidir. Bölgede yalnızca bir sanal makine varsa, kullanılabilirlik kümesi gerekli değildir. Bir sanal makineyi bir kullanılabilirlik kümesine, oluşturma zamanında yerleştirebilirsiniz. Sanal makine zaten bir kullanılabilirlik kümesinde ise, daha sonra ek bir çoğaltma için bir sanal makine ekleyebilirsiniz.

Bu mimaride, uzak bölgedeki Çoğaltma normalde zaman uyumsuz tamamlama kullanılabilirliği moduyla ve el ile yük devretme moduyla yapılandırılır.

Kullanılabilirlik grubu çoğaltmaları farklı Azure bölgelerindeki Azure sanal makinelerdeyse her bölge şunları gerektirir:

* Bir sanal ağ geçidi
* Bir sanal ağ geçidi bağlantısı

Aşağıdaki diyagramda, ağların veri merkezleri arasında nasıl iletişim kurduğu gösterilmektedir.

   ![Kullanılabilirlik Grubu](./media/availability-group-manually-configure-multiple-regions/01-vpngateway-example.png)

>[!IMPORTANT]
>Bu mimari, Azure bölgeleri arasında çoğaltılan veriler için giden veri ücretleri doğurur. Bkz. [bant genişliği fiyatlandırması](https://azure.microsoft.com/pricing/details/bandwidth/).  

## <a name="create-remote-replica"></a>Uzak çoğaltma oluştur

Uzak bir veri merkezinde çoğaltma oluşturmak için aşağıdaki adımları uygulayın:

1. [Yeni bölgede bir sanal ağ oluşturun](../../../virtual-network/manage-virtual-network.md#create-a-virtual-network).

1. [Azure Portal kullanarak VNET-VNet bağlantısı yapılandırın](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).

   >[!NOTE]
   >Bazı durumlarda, sanal ağdan sanal ağa bağlantı oluşturmak için PowerShell kullanmanız gerekebilir. Örneğin, farklı Azure hesapları kullanıyorsanız, portalda bağlantıyı yapılandıramazsınız. Bu durumda bkz. [Azure Portal kullanarak VNET-VNet bağlantısı yapılandırma](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

1. [Yeni bölgede bir etki alanı denetleyicisi oluşturun](../../../active-directory/active-directory-new-forest-virtual-machine.md).

   Birincil sitedeki etki alanı denetleyicisi kullanılamıyorsa, bu etki alanı denetleyicisi kimlik doğrulaması sağlar.

1. [Yeni bölgede SQL Server sanal makine oluşturun](create-sql-vm-portal.md).

1. [Yeni bölgedeki ağda bir Azure yük dengeleyici oluşturun](availability-group-manually-configure-tutorial.md#configure-internal-load-balancer).

   Bu yük dengeleyici şunları içermelidir:

   - Yeni sanal makine ile aynı ağ ve alt ağ içinde olmalıdır.
   - Kullanılabilirlik grubu dinleyicisi için bir statik IP adresi vardır.
   - Yük dengeleyiciyle aynı bölgedeki yalnızca sanal makinelerden oluşan bir arka uç havuzu ekleyin.
   - IP adresine özgü bir TCP bağlantı noktası araştırması kullanın.
   - Aynı bölgedeki SQL Server özel bir yük dengeleme kuralına sahip olmalıdır.  
   - Arka uç havuzundaki sanal makineler tek bir kullanılabilirlik kümesinin ya da sanal makine ölçek kümesinin bir parçası değilse, bir Standart Load Balancer olun. Daha fazla bilgi için [Azure Load Balancer standart genel bakış ' a](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)bakın.

1. [Yeni SQL Server Yük Devretme Kümelemesi özelliği ekleyin](availability-group-manually-configure-prerequisites-tutorial.md#add-failover-clustering-features-to-both-sql-server-vms).

1. [Yeni SQL Server etki alanına ekleyin](availability-group-manually-configure-prerequisites-tutorial.md#joinDomain).

1. [Yeni SQL Server hizmet hesabını bir etki alanı hesabı kullanacak şekilde ayarlayın](availability-group-manually-configure-prerequisites-tutorial.md#setServiceAccount).

1. [Yeni SQL Server Windows Server yük devretme kümesine ekleyin](availability-group-manually-configure-tutorial.md#addNode).

1. Kümeye bir IP adresi kaynağı ekleyin.

   IP adresi kaynağını Yük Devretme Kümesi Yöneticisi ' de oluşturabilirsiniz. Kümenin adını seçin, ardından küme **çekirdek kaynakları** altında küme adına sağ tıklayın ve **Özellikler**' i seçin: 

   ![Küme özellikleri](./media/availability-group-manually-configure-multiple-regions/cluster-name-properties.png)

   **Özellikler** iletişim kutusunda, **IP adresi**altında **Ekle** ' yi seçin ve ardından uzak ağ bölgesinden küme adının IP adresini ekleyin. **IP adresi** Iletişim kutusunda **Tamam** ' ı SEÇIN ve ardından yeni IP adresini kaydetmek için **küme özellikleri** iletişim kutusunda yeniden **Tamam** ' ı seçin. 

   ![Küme IP 'si Ekle](./media/availability-group-manually-configure-multiple-regions/add-cluster-ip-address.png)


1. IP adresini çekirdek küme adı için bir bağımlılık olarak ekleyin.

   Küme özelliklerini daha sonra açın ve **Bağımlılıklar** sekmesini seçin. iki IP adresi IÇIN bir veya bağımlılığı yapılandırın: 

   ![Küme özellikleri](./media/availability-group-manually-configure-multiple-regions/cluster-ip-dependencies.png)

1. Kümedeki kullanılabilirlik grubu rolüne bir IP adresi kaynağı ekleyin. 

   Yük Devretme Kümesi Yöneticisi ' de kullanılabilirlik grubu rolüne sağ tıklayın, **Kaynak Ekle**, **daha fazla kaynak**ve **IP adresi**Seç ' i seçin.

   ![IP adresi oluştur](./media/availability-group-manually-configure-multiple-regions/20-add-ip-resource.png)

   Bu IP adresini şu şekilde yapılandırın:

   - Uzak veri merkezinden ağı kullanın.
   - IP adresini yeni Azure Yük dengeleyicisinden atayın. 

1. IP adresi kaynağını, dinleyici istemci erişim noktası (ağ adı) kümesine yönelik bir bağımlılık olarak ekleyin.

   Aşağıdaki ekran görüntüsünde düzgün yapılandırılmış bir IP adresi küme kaynağı gösterilmektedir:

   ![Kullanılabilirlik Grubu](./media/availability-group-manually-configure-multiple-regions/50-configure-dependency-multiple-ip.png)

   >[!IMPORTANT]
   >Küme kaynak grubu her iki IP adresini de içerir. Her iki IP adresi de dinleyici istemci erişim noktası için bağımlılıklardır. Küme bağımlılığı yapılandırmasında **or** işlecini kullanın.

1. [PowerShell 'de küme parametrelerini ayarlayın](availability-group-manually-configure-tutorial.md#setparam).

   PowerShell betiğini, yeni bölgedeki yük dengeleyicide yapılandırdığınız küme ağ adı, IP adresi ve araştırma bağlantı noktasıyla çalıştırın.

   ```powershell
   $ClusterNetworkName = "<MyClusterNetworkName>" # The cluster name for the network in the new region (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name).
   $IPResourceName = "<IPResourceName>" # The cluster name for the new IP Address resource.
   $ILBIP = "<n.n.n.n>" # The IP Address of the Internal Load Balancer (ILB) in the new region. This is the static IP address for the load balancer you configured in the Azure portal.
   [int]$ProbePort = <nnnnn> # The probe port you set on the ILB.

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

1. SQL Server Yapılandırma Yöneticisi yeni SQL Server, [Always on kullanılabilirlik grupları 'nı etkinleştirin](/sql/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server).

1. [Yeni SQL Server güvenlik duvarı bağlantı noktalarını açın](availability-group-manually-configure-prerequisites-tutorial.md#endpoint-firewall).

   Açmanız gereken bağlantı noktası numaraları ortamınıza bağlıdır. Yansıtma uç noktası ve Azure Yük Dengeleyici Sistem durumu araştırması için bağlantı noktalarını açın.


1. [Yeni SQL Server kullanılabilirlik grubuna bir çoğaltma ekleyin](/sql/database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio).

   Uzak bir Azure bölgesindeki bir çoğaltma için el ile yük devretme ile zaman uyumsuz çoğaltma için ayarlayın.  

## <a name="set-connection-for-multiple-subnets"></a>Birden çok alt ağ için bağlantı ayarlama

Uzak veri merkezindeki çoğaltma, kullanılabilirlik grubunun bir parçasıdır ancak farklı bir alt ağda yer alabilir. Bu çoğaltma birincil çoğaltma olursa, uygulama bağlantısı zaman aşımları meydana gelebilir. Bu davranış, çok alt ağ dağıtımında şirket içi kullanılabilirlik grubuyla aynıdır. İstemci uygulamalarından gelen bağlantılara izin vermek için, istemci bağlantısını güncelleştirin ya da küme ağ adı kaynağında ad çözümlemesi önbelleğe alma 'yı yapılandırın.

Tercihen, ayarlanacak istemci bağlantı dizelerini güncelleştirin `MultiSubnetFailover=Yes` . Bkz. [MultiSubnetFailover Ile bağlanma](https://msdn.microsoft.com/library/gg471494#Anchor_0).

Bağlantı dizelerini değiştiremeyeceğiniz takdirde ad çözümlemesi önbelleği yapılandırabilirsiniz. Bkz. [zaman aşımı hatası ve çok alt ağ ortamında SQL Server 2012 AlwaysOn kullanılabilirlik grubu dinleyicisine bağlanamazsınız](https://support.microsoft.com/help/2792139/time-out-error-and-you-cannot-connect-to-a-sql-server-2012-alwayson-av).

## <a name="fail-over-to-remote-region"></a>Uzak bölgeye yük devretme

Uzak bölgeye dinleyici bağlantısını test etmek için çoğaltmayı uzak bölgeye devreder. Çoğaltma zaman uyumsuz olsa da, yük devretme olası veri kaybına karşı savunmasız olur. Veri kaybı olmadan yük devretmek için kullanılabilirlik modunu eşzamanlı olarak değiştirin ve yük devretme modunu otomatik olarak ayarlayın. Aşağıdaki adımları kullanın:

1. **Nesne Gezgini**' de, birincil çoğaltmayı barındıran SQL Server örneğine bağlanın.
1. **AlwaysOn kullanılabilirlik grupları**, **kullanılabilirlik grupları**' nın altında, kullanılabilirlik grubunuza sağ tıklayıp **Özellikler**' i seçin.
1. **Genel** sayfasında, **kullanılabilirlik ÇOĞALTMALARı**altında, Dr sitesindeki Ikincil çoğaltmayı, **zaman uyumlu tamamlama** kullanılabilirlik modunu ve **Otomatik** yük devretme modunu kullanacak şekilde ayarlayın.
1. Yüksek kullanılabilirlik için birincil çoğaltmayla aynı sitede ikincil bir çoğaltmeniz varsa, bu çoğaltmayı **zaman uyumsuz işlemeye** ve **el ile**ayarlayın.
1. Tamam'ı seçin.
1. **Nesne Gezgini**, kullanılabilirlik grubuna sağ tıklayın ve **panoyu göster**' i seçin.
1. Panoda, DR sitesindeki çoğaltmanın eşitlendiğinden emin olun.
1. **Nesne Gezgini**, kullanılabilirlik grubuna sağ tıklayın ve **Yük devretme...** seçeneğini belirleyin. SQL Server Management Studios, SQL Server yük devretmek için bir sihirbaz açar.  
1. **İleri**' yi SEÇIN ve DR sitesindeki SQL Server örneğini seçin. **İleri ' yi** tekrar seçin.
1. DR sitesindeki SQL Server örneğine bağlanın ve **İleri ' yi**seçin.
1. **Özet** sayfasında, ayarları doğrulayın ve **son**' u seçin.

Bağlantıyı test ettikten sonra birincil çoğaltmayı birincil veri merkezinize geri taşıyın ve kullanılabilirlik modunu normal işletim ayarlarına geri doğru ayarlayın. Aşağıdaki tabloda, bu belgede açıklanan mimarinin normal işletimsel ayarları gösterilmektedir:

| Konum | Sunucu örneği | Rol | Kullanılabilirlik modu | Yük devretme modu
| ----- | ----- | ----- | ----- | -----
| Birincil veri merkezi | SQL-1 | Birincil | Zaman Uyumlu | Automatic
| Birincil veri merkezi | SQL-2 | İkincil | Zaman Uyumlu | Automatic
| İkincil veya uzak veri merkezi | SQL-3 | İkincil | Zaman Uyumsuz | El ile


### <a name="more-information-about-planned-and-forced-manual-failover"></a>Planlı ve zorlamalı el ile yük devretme hakkında daha fazla bilgi

Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

- [Kullanılabilirlik grubunun planlı bir el Ile yük devretmesini gerçekleştirme (SQL Server)](https://msdn.microsoft.com/library/hh231018.aspx)
- [Kullanılabilirlik grubunun zorla el Ile yük devretmesini gerçekleştirme (SQL Server)](https://msdn.microsoft.com/library/ff877957.aspx)

## <a name="next-steps"></a>Sonraki adımlar

* [AlwaysOn Kullanılabilirlik Grupları](https://msdn.microsoft.com/library/hh510230.aspx)
* [Azure Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/windows/)
* [Azure yük dengeleyiciler](availability-group-manually-configure-tutorial.md#configure-internal-load-balancer)
* [Azure kullanılabilirlik kümeleri](../../../virtual-machines/linux/manage-availability.md)
