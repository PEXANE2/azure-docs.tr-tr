---
title: Küme yapılandırması en iyi uygulamaları
description: Desteklenen çekirdeklerine veya bağlantı yönlendirme seçenekleri gibi Azure sanal makinelerinde SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) yapılandırırken desteklenen küme konfigürasyonları hakkında bilgi edinin.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2020
ms.author: mathoma
ms.openlocfilehash: 1a2c4364337083be005c550a8859079cd3bb1218
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167959"
---
# <a name="cluster-configuration-best-practices-sql-server-on-azure-vms"></a>Küme yapılandırması en iyi yöntemleri (Azure VM'leri üzerinde SQL Server)
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bir küme, Azure sanal makinelerinde (VM 'Ler) SQL Server yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) için kullanılır. 

Bu makalede, Azure VM 'lerinde SQL Server kullandığınızda hem [Yük devretme kümesi örnekleri (fcsıs)](failover-cluster-instance-overview.md) hem de [kullanılabilirlik grupları](availability-group-overview.md) için küme yapılandırması en iyi yöntemleri sağlanmaktadır. 


## <a name="networking"></a>Ağ

Sunucu başına tek bir NIC (küme düğümü) ve tek bir alt ağ kullanın. Azure ağ iletişimi, Azure sanal makine konuk kümesinde ek NIC 'ler ve alt ağları gereksiz hale getiren fiziksel yedekliliğe sahiptir. Küme doğrulama raporu, düğümlerin yalnızca tek bir ağda ulaşılabilir olduğunu size bildirir. Bu uyarıyı, Azure sanal makine konuk yük devretme kümelerinde yoksayabilirsiniz.

## <a name="quorum"></a>Çekirdeğin

İki düğümlü bir küme, [çekirdek kaynağı](/windows-server/storage/storage-spaces/understand-quorum)olmadan işlev yapabilse de, müşterilerin üretim desteği sağlamak için bir çekirdek kaynak kullanması kesinlikle gerekir. Küme doğrulaması, çekirdek kaynağı olmayan hiçbir kümeyi geçirmez. 

Teknik olarak, üç düğümlü bir küme, çekirdek kaynağı olmadan tek bir düğüm kaybını (iki düğüme kadar) devam edebilir. Ancak küme iki düğüme geçtikten sonra, bir düğüm kaybı veya bir bölünmüş beyinme senaryosunu engellemek için bir iletişim hatası durumunda kümelenmiş kaynakların çevrimdışı duruma gelmesi riski vardır.

Çekirdek kaynağı yapılandırmak, kümenin çevrimiçi olarak yalnızca bir düğüm çevrimiçi olarak devam etmesine izin verir.

Aşağıdaki tabloda, bir Azure VM ile kullanılması önerilen sırada kullanılabilir olan çekirdek seçenekleri, disk tanığı tercih edilen seçim olarak listelenmektedir: 


||[Disk tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |[Bulut tanığı](/windows-server/failover-clustering/deploy-cloud-witness)  |[Dosya paylaşımı tanığı](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum)  |
|---------|---------|---------|---------|
|**Desteklenen işletim sistemi**| Tümü |Windows Server 2016 +| Tümü|


### <a name="disk-witness"></a>Disk tanığı

Disk tanığı, küme kullanılabilir depolama grubundaki küçük bir kümelenmiş disktir. Bu disk yüksek oranda kullanılabilir ve düğümler arasında yük devredebilirler. Küme veritabanının bir kopyasını içerir ve genellikle 1 GB 'den daha az olan bir varsayılan boyuta sahiptir. Disk tanığı, Azure Paylaşılan diskleri (veya paylaşılan SCSI, Iscsı veya Fiber Kanal SAN gibi paylaşılan disk çözümlerini) kullanan herhangi bir küme için tercih edilen çekirdek seçenektir.  Kümelenmiş paylaşılan birim bir disk tanığı olarak kullanılamaz.

Disk tanığı olarak bir Azure Paylaşılan diski yapılandırın. 

Başlamak için bkz. [disk tanığını yapılandırma](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Desteklenen işletim sistemi**: tümü   


### <a name="cloud-witness"></a>Bulut tanığı

Bulut tanığı, küme çekirdeği üzerinde oy sağlamak için Microsoft Azure kullanan bir yük devretme kümesi çekirdek tanığı türüdür. Varsayılan boyut yaklaşık 1 MB 'dir ve yalnızca zaman damgasını içerir. Bulut tanığı, birden çok sitede, birden çok bölgede ve birden çok bölgede yer alan dağıtımlar için idealdir.

Başlamak için bkz. [bulut tanığını yapılandırma](/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp).


**Desteklenen işletim sistemi**: Windows Server 2016 ve üzeri   


### <a name="file-share-witness"></a>Dosya paylaşımı tanığı

Dosya paylaşma tanığı, genellikle Windows Server çalıştıran bir dosya sunucusunda yapılandırılan bir SMB dosya paylaşımıdır. Kümeleme bilgilerini bir tanık. log dosyasında tutar, ancak küme veritabanının bir kopyasını depolamaz. Azure 'da, dosya paylaşma tanığı olarak kullanmak üzere bir [Azure dosya paylaşımının](../../../storage/files/storage-how-to-create-file-share.md) yapılandırılması veya ayrı bir sanal makinede bir dosya paylaşımının kullanılması olabilir.

Azure dosya paylaşımından kullanacaksanız, [Premium dosya paylaşımının bağlanması](failover-cluster-instance-premium-file-share-manually-configure.md#mount-premium-file-share)için kullanılan aynı işlemle bağlayabilirsiniz. 

Başlamak için bkz. [dosya paylaşma tanığını yapılandırma](/windows-server/failover-clustering/manage-cluster-quorum#configure-the-cluster-quorum).


**Desteklenen işletim sistemi**: Windows Server 2012 ve üzeri   

## <a name="connectivity"></a>Bağlantı

Geleneksel bir şirket içi ağ ortamında, SQL Server yük devretme kümesi örneği tek bir bilgisayarda çalışan tek bir SQL Server örneği olarak görünür. Yük devretme kümesi örneği düğümünden düğüme yük devrettiğinden, örnek için sanal ağ adı (VNN) Birleşik bir bağlantı noktası sağlar ve uygulamaların hangi düğümün etkin olduğunu bilmeksizin SQL Server örneğine bağlanmasına izin verir. Yük devretme gerçekleştiğinde, sanal ağ adı yeni etkin düğümüne başladıktan sonra kaydedilir. Bu işlem, SQL Server bağlanan istemci veya uygulama için saydamdır ve bu, hata sırasında istemci veya uygulamanın karşılaştığı kapalı kalma süresini en aza indirir. Benzer şekilde, kullanılabilirlik grubu dinleyicisi trafiği uygun çoğaltmaya yönlendirmek için bir VNN kullanır. 

Trafiği, Azure VM 'lerinde SQL Server yük devretme kümesi örneğinin VNN 'ağıyla yönlendirmek veya bir kullanılabilirlik grubundaki mevcut VNN dinleyicisini değiştirmek için Azure Load Balancer veya dağıtılmış ağ adı (DNN) ile bir VNN kullanın. 


Aşağıdaki tabloda HADR bağlantısı Supportability karşılaştırılır: 

| |**Sanal Ağ Adı (VNN)**  |**Dağıtılmış Ağ Adı (DNN)**  |
|---------|---------|---------|
|**En düşük işletim sistemi sürümü**| Tümü | Windows Server 2016 |
|**En düşük SQL Server sürümü** |Tümü |SQL Server 2019 CU2 UYGULAMAZSANıZ (FCı için)<br/> SQL Server 2019 CU8 (AG için)|
|**Desteklenen HADR çözümü** | Yük devretme kümesi örneği <br/> Kullanılabilirlik grubu | Yük devretme kümesi örneği <br/> Kullanılabilirlik grubu|


### <a name="virtual-network-name-vnn"></a>Sanal Ağ Adı (VNN)

Sanal IP erişim noktası Azure 'da farklı çalıştığından, trafiği FCı düğümlerinin veya kullanılabilirlik grubu dinleyicisinin IP adresine yönlendirmek için [Azure Load Balancer](../../../load-balancer/index.yml) yapılandırmanız gerekir. Azure sanal makinelerinde, yük dengeleyici kümelenmiş SQL Server kaynaklarının kullandığı VNN için IP adresini tutar. Yük dengeleyici ön uca gelen gelen akışları dağıtır ve ardından bu trafiği arka uç havuzu tarafından tanımlanan örneklere yönlendirir. Trafik akışını, Yük Dengeleme kurallarını ve sistem durumu araştırmalarını kullanarak yapılandırırsınız. SQL Server FCı ile arka uç havuzu örnekleri, SQL Server çalıştıran Azure sanal makinelerdir. 

Yük dengeleyiciyi kullanırken hafif bir yük devretme gecikmesi vardır, çünkü durum araştırma iletken canlı bir şekilde varsayılan olarak her 10 saniyede bir kontrol eder. 

Başlamak için, [Yük devretme kümesi örneği](failover-cluster-instance-vnn-azure-load-balancer-configure.md) veya bir [kullanılabilirlik grubu](availability-group-vnn-azure-load-balancer-configure.md) için Azure Load Balancer yapılandırmayı öğrenin

**Desteklenen işletim sistemi**: tümü   
**Desteklenen SQL sürümü**: tümü   
**Desteklenen HADR çözümü**: yük devretme kümesi örneği ve kullanılabilirlik grubu   


### <a name="distributed-network-name-dnn"></a>Dağıtılmış Ağ Adı (DNN)

Dağıtılmış ağ adı, SQL Server 2019 için yeni bir Azure özelliğidir. DNN, SQL Server istemcilerinin yük dengeleyici kullanmadan SQL Server yük devretme kümesi örneğine veya kullanılabilirlik grubuna bağlanmasını sağlamak için alternatif bir yol sağlar. 

DNN kaynağı oluşturulduğunda, küme DNS adını kümedeki tüm düğümlerin IP adresleriyle bağlar. SQL istemcisi Bağlanılacak kaynağı bulmak için bu listedeki her bir IP adresine bağlanmaya çalışır.  Bağlantı dizesinde belirterek, bu işlemi hızlandırabilirsiniz `MultiSubnetFailover=True` . Bu ayar, sağlayıcının tüm IP adreslerini paralel olarak denemesini söyler, böylece istemci FCı veya dinleyiciye anında bağlanabilir. 

Mümkün olduğunda bir yük dengeleyicide dağıtılmış ağ adı önerilir: 
- Artık yük dengeleyici kaynağını sürdürmek zorunda kaldıklarından, uçtan uca çözüm daha sağlamdır. 
- Yük dengeleyici araştırmalarının ortadan kaldırılması yük devretme süresini en aza indirir. 
- DNN, yük devretme kümesi örneği veya kullanılabilirlik grubu dinleyicisinin Azure VM 'lerinde SQL Server sağlamasını ve yönetimini basitleştirir. 

Çoğu SQL Server Özellik DNN kullanılırken, FCı ve kullanılabilirlik gruplarıyla saydam şekilde çalışır, ancak özel bir değerlendirme gerektirebilecek bazı özellikler vardır. Daha fazla bilgi için bkz. [FCI ve DNN birlikte çalışabilirlik](failover-cluster-instance-dnn-interoperability.md) ve [AG ve DNN birlikte çalışabilirliği](availability-group-dnn-interoperability.md) . 

Başlamak için, [bir yük devretme kümesi örneği](failover-cluster-instance-distributed-network-name-dnn-configure.md) veya bir [kullanılabilirlik grubu](availability-group-distributed-network-name-dnn-listener-configure.md) için dağıtılmış ağ adı kaynağı yapılandırmayı öğrenin

**Desteklenen işletim sistemi**: Windows Server 2016 ve üzeri   
**Desteklenen SQL sürümü**: SQL Server 2019 CU2 uygulamazsanız (FCI) ve SQL Server 2019 CU8 (AG)   
**Desteklenen HADR çözümü**: yük devretme kümesi örneği ve kullanılabilirlik grubu   


## <a name="limitations"></a>Sınırlamalar

FCı veya kullanılabilirlik gruplarıyla çalışırken ve Azure sanal makinelerinde SQL Server aşağıdaki sınırlamaları göz önünde bulundurun. 

### <a name="msdtc"></a>MSDTC 

Azure sanal makineleri, kümelenmiş paylaşılan birimler (CSV) ve [azure standart Load Balancer](../../../load-balancer/load-balancer-standard-overview.md) ya da Azure paylaşılan diskler kullanan SQL Server VM 'lerde depolama Ile Windows Server 2019 ' de Microsoft Dağıtılmış işlem DÜZENLEYICISI (MSDTC) ' i destekler. 

Azure sanal makinelerde, kümelenmiş paylaşılan birimlerde Windows Server 2016 veya önceki sürümlerde MSDTC desteklenmez çünkü:

- Kümelenmiş MSDTC kaynağı, paylaşılan depolama alanı kullanacak şekilde yapılandırılamaz. Windows Server 2016 ' de, bir MSDTC kaynağı oluşturursanız, depolama alanı kullanılabilir olsa bile, kullanılabilir bir paylaşılan depolama alanı göstermez. Bu sorun Windows Server 2019 ' de düzeltildi.
- Temel yük dengeleyici RPC bağlantı noktalarını işlemez.


## <a name="next-steps"></a>Sonraki adımlar

Çözümünüz için uygun en iyi uygulamaları belirledikten sonra, [SQL Server VM FCI için hazırlarken](failover-cluster-instance-prepare-vm.md) veya [Azure Portal](availability-group-azure-portal-configure.md), [Azure CLI/PowerShell](availability-group-az-cli-configure.md)veya [Azure hızlı başlangıç şablonlarını](availability-group-quickstart-template-configure.md)kullanarak kullanılabilirlik grubunuzu oluşturarak başlayın. 

