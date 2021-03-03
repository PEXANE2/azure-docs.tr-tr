---
title: Yük devretme kümesi örnekleri
description: Azure sanal makinelerinde SQL Server yük devretme kümesi örnekleri (FC,) hakkında bilgi edinin.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.subservice: hadr
ms.topic: overview
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: a7735de9763f3924cd6baae6af1258f6448c874e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101690932"
---
# <a name="failover-cluster-instances-with-sql-server-on-azure-virtual-machines"></a>Azure sanal makineler 'de SQL Server yük devretme kümesi örnekleri
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Azure sanal makinelerinde (VM) SQL Server için yük devretme kümesi örnekleriyle (FCı) çalışırken özellik farklılıkları tanıtılmaktadır. 

## <a name="overview"></a>Genel Bakış

Azure VM 'lerinde SQL Server, sunucu örneği düzeyinde yedeklilik aracılığıyla yerel yüksek kullanılabilirlik sağlamak için Windows sunucu yük devretme kümelemesi (WSFC) işlevini kullanır: bir yük devretme kümesi örneği. FCı, WSFC (veya yalnızca küme) düğümleri arasında yüklenmiş SQL Server tek bir örneğidir ve muhtemelen birden çok alt ağ arasında. Ağda, bir FCı tek bir bilgisayarda çalışan bir SQL Server örneği gibi görünür. Ancak FCı, geçerli düğüm kullanılamaz hale gelirse bir WSFC düğümünden diğerine yük devretme sağlar.

Makalenin geri kalanı, Azure VM 'lerinde SQL Server ile kullanıldığında yük devretme kümesi örneklerinin farklılıklarına odaklanır. Yük Devretme Kümelemesi teknolojisi hakkında daha fazla bilgi edinmek için bkz.: 

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server yük devretme kümesi örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)

## <a name="quorum"></a>Çekirdeğin

Azure sanal makinelerinde SQL Server olan yük devretme kümesi örnekleri, küme çekirdeği için bir disk tanığı, bulut tanığı veya dosya paylaşma tanığı kullanmayı destekler.

Daha fazla bilgi edinmek için bkz. [Azure 'da SQL Server VM 'lerle Ilgili çekirdek en iyi yöntemleri](hadr-cluster-best-practices.md#quorum). 


## <a name="storage"></a>Depolama

Geleneksel şirket içi kümelenmiş ortamlarda Windows Yük devretme kümesi, paylaşılan depolama alanı olarak her iki düğüm tarafından erişilebilen bir depolama alanı ağı (SAN) kullanır. SQL Server dosyalar paylaşılan depolama alanında barındırılır ve yalnızca etkin düğüm dosyalara tek seferde erişebilir. 

Azure VM 'lerinde SQL Server, SQL Server yük devretme kümesi örneklerinin dağıtımı için paylaşılan depolama çözümü olarak çeşitli seçenekler sunar: 

||[Azure paylaşılan diskleri](../../../virtual-machines/disks-shared.md)|[Premium dosya paylaşımları](../../../storage/files/storage-how-to-create-file-share.md) |[Depolama Alanları Doğrudan (S2D)](/windows-server/storage/storage-spaces/storage-spaces-direct-overview)|
|---------|---------|---------|---------|
|**En düşük işletim sistemi sürümü**| Tümü |Windows Server 2012|Windows Server 2016|
|**En düşük SQL Server sürümü**|Tümü|SQL Server 2012|SQL Server 2016|
|**Desteklenen VM kullanılabilirliği** |Yakınlık yerleşimi gruplarıyla kullanılabilirlik kümeleri (Premium SSD Için) </br> Aynı Kullanılabilirlik bölgesi (Ultra SSD Için) |Kullanılabilirlik kümeleri ve kullanılabilirlik bölgeleri|Kullanılabilirlik kümeleri |
|**FILESTREAM 'i destekler**|Yes|Hayır|Yes |
|**Azure Blob önbelleği**|Hayır|Hayır|Yes|

Bu bölümün geri kalanında, Azure VM 'lerinde SQL Server için kullanılabilen her depolama seçeneğinin avantajları ve sınırlamaları listelenmektedir. 

### <a name="azure-shared-disks"></a>Azure paylaşılan diskleri

[Azure paylaşılan diskler](../../../virtual-machines/disks-shared.md) , [Azure yönetilen disklerinin](../../../virtual-machines/managed-disks-overview.md)bir özelliğidir. Windows Server Yük Devretme Kümelemesi, yük devretme kümesi örneğiyle Azure Paylaşılan diskleri kullanmayı destekler. 

**Desteklenen işletim sistemi**: tümü   
**Desteklenen SQL sürümü**: tümü     

**Avantajlar**: 
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) mimarisini olduğu gibi tutarken Azure 'a geçiş yapmak isteyen uygulamalar için faydalıdır. 
- , SCSI kalıcı ayırmaları (SCSI PR) desteği nedeniyle kümelenmiş uygulamaları Azure 'a geçirebilirler. 
- , Paylaşılan Azure Premium SSD ve Azure Ultra Disk depolamayı destekler.
- Paylaşılan bir depolama havuzu oluşturmak için tek bir paylaşılan disk kullanabilir veya birden çok paylaşılan disk oluşturabilirsiniz. 
- FILESTREAM 'i destekler.
- Premium SSD 'Ler kullanılabilirlik kümelerini destekler. 


**Sınırlamalar**: 
- Sanal makinelerin aynı Kullanılabilirlik kümesine ve yakınlık yerleşimi grubuna yerleştirilmesi önerilir.
- Ultra diskler kullanılabilirlik kümelerini desteklemez. 
- Kullanılabilirlik alanları, Ultra diskler için desteklenir, ancak VM 'Lerin aynı Kullanılabilirlik bölgesinde olması gerekir ve bu da sanal makinenin kullanılabilirliğini azaltır. 
- Seçilen donanım kullanılabilirliği çözümüne bakılmaksızın, Azure paylaşılan diskler kullanılırken yük devretme kümesinin kullanılabilirliği her zaman% 99,9 ' dir. 
- Premium SSD disk önbelleğe alma desteklenmez.

 
Başlamak için bkz. [Azure Paylaşılan disklerle SQL Server yük devretme kümesi örneği](failover-cluster-instance-azure-shared-disks-manually-configure.md). 

### <a name="storage-spaces-direct"></a>Doğrudan Depolama Alanları

[Depolama alanları doğrudan](/windows-server/storage/storage-spaces/storage-spaces-direct-overview) , Azure sanal makinelerinde Yük Devretme Kümelemesi ile desteklenen bir Windows Server özelliğidir. Yazılım tabanlı bir sanal SAN sağlar.

**Desteklenen işletim sistemi**: Windows Server 2016 ve üzeri   
**Desteklenen SQL sürümü**: SQL Server 2016 ve üzeri   


**Larından** 
- Yeterli ağ bant genişliği, güçlü ve yüksek performanslı bir depolama çözümüne izin vermez. 
- , Azure blob önbelleğini destekler, bu nedenle okuma, önbellekten yerel olarak sunulabilir. (Güncelleştirmeler her iki düğüme de aynı anda çoğaltılır.) 
- FILESTREAM 'i destekler. 

**Sınırlamalar:**
- Yalnızca Windows Server 2016 ve üzeri için kullanılabilir. 
- Kullanılabilirlik alanları desteklenmiyor.
- Her iki sanal makineye de aynı disk kapasitesini gerektirir. 
- Devam eden disk çoğaltma nedeniyle yüksek performans elde etmek için yüksek ağ bant genişliği gereklidir. 
- Depolama her VM 'ye eklendiğinden daha büyük bir VM boyutu ve depolama için çift ödeme gerektirir. 

Başlamak için, bkz. [SQL Server yük devretme kümesi örneği depolama alanları doğrudan](failover-cluster-instance-storage-spaces-direct-manually-configure.md). 

### <a name="premium-file-share"></a>Premium dosya paylaşımı

[Premium dosya paylaşımları](../../../storage/files/storage-how-to-create-file-share.md) , [Azure dosyalarının](../../../storage/files/index.yml)bir özelliğidir. Premium dosya paylaşımları SSD olarak desteklenir ve sürekli düşük gecikme süresine sahiptir. Windows Server 2012 veya sonraki sürümlerde SQL Server 2012 veya üzeri yük devretme kümesi örnekleriyle birlikte kullanılmak üzere tam olarak desteklenmektedir. Bir dosya paylaşımını herhangi bir kesinti olmadan yeniden boyutlandırabilir ve ölçeklendirebileceğiniz için Premium dosya paylaşımları size daha fazla esneklik sağlar.

**Desteklenen işletim sistemi**: Windows Server 2012 ve üzeri   
**Desteklenen SQL sürümü**: SQL Server 2012 ve üzeri   

**Larından** 
- Yalnızca sanal makineler için paylaşılan depolama çözümü birden çok kullanılabilirlik bölgesine yayılır. 
- Tek basamaklı gecikme süreleriyle tam olarak yönetilen dosya sistemi ve Burstable g/ç performansı. 

**Sınırlamalar:**
- Yalnızca Windows Server 2012 ve üzeri için kullanılabilir. 
- FILESTREAM desteklenmiyor. 


Başlamak için bkz. [Premium dosya paylaşımıyla SQL Server yük devretme kümesi örneği](failover-cluster-instance-premium-file-share-manually-configure.md). 

### <a name="partner"></a>İş Ortağı

Desteklenen depolamaya sahip iş ortağı kümesi çözümleri vardır. 

**Desteklenen işletim sistemi**: tümü   
**Desteklenen SQL sürümü**: tümü   

Bir örnek, depolama olarak SIOS veri Man kullanır. Daha fazla bilgi için bkz. blog girdisi [Yük Devretme Kümelemesi ve SIOS veri Man](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/).

### <a name="iscsi-and-expressroute"></a>Iscsı ve ExpressRoute

Ayrıca, Azure ExpressRoute aracılığıyla bir Iscsı hedefi paylaşılan blok depolama alanını kullanıma sunabilirsiniz. 

**Desteklenen işletim sistemi**: tümü   
**Desteklenen SQL sürümü**: tümü   

Örneğin, NetApp özel depolama (NPS), Azure VM 'lerine Equinx ile ExpressRoute aracılığıyla bir Iscsı hedefi sunar.

Microsoft iş ortaklarının paylaşılan depolama ve veri çoğaltma çözümleri için, yük devretmeyle ilgili verilere erişme hakkında herhangi bir sorun için satıcıya başvurun.

## <a name="connectivity"></a>Bağlantı

Azure sanal makinelerinde SQL Server olan yük devretme kümesi örnekleri, şu anda kümelenmiş kaynaklara sahip olan düğümden bağımsız olarak trafiği SQL Server örneğine yönlendirmek için Azure Load Balancer ile bir [dağıtılmış ağ adı (DNN)](failover-cluster-instance-distributed-network-name-dnn-configure.md) veya bir [sanal ağ adı (vnn)](failover-cluster-instance-vnn-azure-load-balancer-configure.md) kullanır. SQL Server FCı ile belirli özellikleri ve DNN 'yi kullanırken dikkate alınması gereken ek durumlar vardır. Daha fazla bilgi edinmek için [SQL Server FCI Ile DNN birlikte çalışabilirliği](failover-cluster-instance-dnn-interoperability.md) bölümüne bakın. 

Küme bağlantı seçenekleri hakkında daha fazla bilgi için bkz. [Azure VM 'lerinde SQL Server HADR bağlantıları yönlendirme](hadr-cluster-best-practices.md#connectivity). 

## <a name="limitations"></a>Sınırlamalar

Azure sanal makinelerinde SQL Server yük devretme kümesi örnekleri için aşağıdaki sınırlamaları göz önünde bulundurun. 

### <a name="lightweight-extension-support"></a>Hafif uzantı desteği   

Şu anda Azure sanal makinelerinde SQL Server yük devretme kümesi örnekleri yalnızca SQL Server IaaS aracı uzantısının [basit yönetim moduyla](sql-server-iaas-agent-extension-automate-management.md#management-modes) desteklenir. Tam uzantı modundan hafif olarak geçiş yapmak için, karşılık gelen VM 'Ler için **SQL sanal makine** kaynağını silin ve ardından bunları basıt modda SQL IaaS Aracısı Uzantısı ile kaydedin. Azure portal kullanarak **SQL sanal makine** kaynağını silerken, sanal makineyi silmeyi önlemek için doğru sanal makinenin yanındaki onay kutusunu temizleyin. 

Tam uzantı otomatik yedekleme, düzeltme eki uygulama ve gelişmiş Portal yönetimi gibi özellikleri destekler. Bu özellikler, hafif yönetim modunda kayıtlı SQL Server VM 'Ler için çalışmaz.

### <a name="msdtc"></a>MSDTC 

Azure sanal makineleri, kümelenmiş paylaşılan birimler (CSV) ve [azure standart Load Balancer](../../../load-balancer/load-balancer-overview.md) ya da Azure paylaşılan diskler kullanan SQL Server VM 'lerde depolama Ile Windows Server 2019 ' de Microsoft Dağıtılmış işlem DÜZENLEYICISI (MSDTC) ' i destekler. 

Azure sanal makinelerde, kümelenmiş paylaşılan birimlerde Windows Server 2016 veya önceki sürümlerde MSDTC desteklenmez çünkü:

- Kümelenmiş MSDTC kaynağı, paylaşılan depolama alanı kullanacak şekilde yapılandırılamaz. Windows Server 2016 ' de, bir MSDTC kaynağı oluşturursanız, depolama alanı kullanılabilir olsa bile, kullanılabilir bir paylaşılan depolama alanı göstermez. Bu sorun Windows Server 2019 ' de düzeltildi.
- Temel yük dengeleyici RPC bağlantı noktalarını işlemez.


## <a name="next-steps"></a>Sonraki adımlar

[Küme yapılandırmalarının en iyi yöntemlerini](hadr-cluster-best-practices.md)gözden geçirin ve ardından [SQL Server VM FCI için hazırlayabilirsiniz](failover-cluster-instance-prepare-vm.md). 

Daha fazla bilgi için bkz. 

- [Windows küme teknolojileri](/windows-server/failover-clustering/failover-clustering-overview)   
- [SQL Server yük devretme kümesi örnekleri](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server)