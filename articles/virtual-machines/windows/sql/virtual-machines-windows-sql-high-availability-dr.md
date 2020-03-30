---
title: SQL Server için Yüksek Kullanılabilirlik ve Olağanüstü Durum Kurtarma | Microsoft Dokümanlar
description: Azure Sanal Makinelerde çalışan SQL Server için çeşitli HADR stratejilerinin tartışılması.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 53981f7e-8370-4979-b26a-93a5988d905f
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/27/2017
ms.author: mikeray
ms.openlocfilehash: f58bb534728660b85f7d16910dde7a37914fd571
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249769"
---
# <a name="high-availability-and-disaster-recovery-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler’de SQL Server için yüksek kullanılabilirlik ve olağanüstü durum kurtarma

SQL Server'a sahip Microsoft Azure sanal makineleri (VM'ler), yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR) veritabanı çözümünün maliyetini düşürmeye yardımcı olabilir. SQL Server HADR çözümlerinin çoğu Azure sanal makinelerinde hem Azure hem de karma çözümler olarak desteklenir. Yalnızca Azure çözümünde, tüm HADR sistemi Azure'da çalışır. Karma yapılandırmada, çözümün bir bölümü Azure'da, diğer bölümü de kuruluşunuziçin şirket içinde çalışır. Azure ortamının esnekliği, SQL Server veritabanı sistemlerinizin bütçe ve HADR gereksinimlerini karşılamak için kısmen veya tamamen Azure'a geçmenizi sağlar.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="understanding-the-need-for-an-hadr-solution"></a>HADR çözümihtiyacını anlamak
Veritabanı sisteminizin hizmet düzeyi sözleşmesinin (SLA) gerektirdiği HADR özelliklerine sahip olduğundan emin olmak size kalmış. Azure'un bulut hizmetleri için hizmet iyileştirme ve Sanal Makineler için hata kurtarma algılama gibi yüksek kullanılabilirlik mekanizmaları sağlaması, istediğiniz SLA'yı karşılaabileceğinizi garanti etmez. Bu mekanizmalar VM'lerin yüksek kullanılabilirliğini korur, ancak VM'lerin içinde çalışan SQL Server'ın yüksek kullanılabilirliğini korumaz. VM çevrimiçi ve sağlıklıyken SQL Server örneğinin başarısız olması mümkündür. Dahası, Azure tarafından sağlanan yüksek kullanılabilirlik mekanizmaları bile, yazılım veya donanım hatalarından kurtarma ve işletim sistemi yükseltmeleri gibi olaylar nedeniyle VM'lerin kapalı kalma süresine izin verir.

Buna ek olarak, Azure'daki Coğrafi Yedekli Depolama (GRS), coğrafi çoğaltma adı verilen bir özellik ile uygulanan veritabanlarınız için yeterli bir olağanüstü durum kurtarma çözümü olmayabilir. Coğrafi çoğaltma verileri eşit bir şekilde gönderdiğinden, son güncelleştirmeler felaket durumunda kaybolabilir. Coğrafi çoğaltma sınırlamaları ile ilgili daha fazla [bilgi, ayrı diskler bölümünde veri ve günlük dosyaları için desteklenmeyen Coğrafi çoğaltma](#geo-replication-support) kapsamındadır.

## <a name="hadr-deployment-architectures"></a>HADR dağıtım mimarileri
Azure'da desteklenen SQL Server HADR teknolojileri şunlardır:

* [Her Zaman Kullanılabilirlik Gruplarında](https://technet.microsoft.com/library/hh510230.aspx)
* [Her Zaman Failover Küme Örnekleri açık](https://technet.microsoft.com/library/ms189134.aspx)
* [Log Kargo](https://technet.microsoft.com/library/ms187103.aspx)
* [Azure Blob Depolama Hizmeti ile SQL Server Yedekleme ve Geri Yükleme](https://msdn.microsoft.com/library/jj919148.aspx)
* [Veritabanı Yansıtma](https://technet.microsoft.com/library/ms189852.aspx) - SQL Server 2016'da Amortismana Uğradı

Hem yüksek kullanılabilirlik hem de olağanüstü durum kurtarma özelliklerine sahip bir SQL Server çözümlerini uygulamak için teknolojileri bir araya getirmek mümkündür. Kullandığınız teknolojiye bağlı olarak, karma dağıtım, Azure sanal ağına sahip bir VPN tüneli gerektirebilir. Aşağıdaki bölümlerde bazı örnek dağıtım mimarileri göster.

## <a name="azure-only-high-availability-solutions"></a>Yalnızca Azure: Yüksek kullanılabilirlik çözümleri

Kullanılabilirlik grupları olarak adlandırılan Her Zaman Kullanılabilirlik Grupları ile veritabanı düzeyinde SQL Server için yüksek kullanılabilirlik çözümüne sahip olabilirsiniz. Ayrıca, Her Zaman Failover Küme Örnekleri - failover küme örnekleri ile bir örnek düzeyinde yüksek kullanılabilirlik çözümü oluşturabilirsiniz. Ek fazlalık için, failover küme örneklerinde kullanılabilirlik grupları oluşturarak her iki düzeyde de fazlalık oluşturabilirsiniz. 

| Teknoloji | Örnek Mimariler |
| --- | --- |
| **Kullanılabilirlik grupları** |Aynı bölgede Bulunan Azure VM'lerde çalışan kullanılabilirlik yinelemeleri yüksek kullanılabilirlik sağlar.  Windows failover kümeleme etkin dizin etki alanı gerektirdiğinden, bir etki alanı denetleyicivm yapılandırmanız gerekir.<br/><br/> Daha yüksek fazlalık ve kullanılabilirlik için Azure VM'leri [kullanılabilirlik grubuna genel bakışta](virtual-machines-windows-portal-sql-availability-group-overview.md)belgelendirildikçe farklı [kullanılabilirlik bölgelerinde](../../../availability-zones/az-overview.md) dağıtılabilir. Kullanılabilirlik grubundaki SQL Server VM'ler kullanılabilirlik bölgelerinde dağıtılıyorsa, dinleyici için [standart yük bakiyesini](../../../load-balancer/load-balancer-standard-overview.md) bu makalelerde belgelenmiş olarak kullanın - [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) & [Azure Quickstart şablonları.](virtual-machines-windows-sql-availability-group-quickstart-template.md)<br/> ![Kullanılabilirlik Grupları](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-ha-always-on.png)<br/>Daha fazla bilgi için [azure'da Kullanılabilirlik Gruplarını Yapılandır (GUI) 'ye](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)bakın. |
| **Failover küme örnekleri** |Paylaşılan depolama gerektiren Failover Cluster Örnekleri (FCI) 4 farklı şekilde oluşturulabilir.<br/><br/>1. Yazılım tabanlı sanal SAN sağlamak için [Windows Server 2016 Depolama Alanları Direct \(\) S2D'yi](virtual-machines-windows-portal-sql-create-failover-cluster.md) kullanarak ekli depolama ile Azure VM'lerde çalışan iki düğümlü bir arıza kümesi.<br/><br/> 2. [Premium Dosya Paylaşımı'nı](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share.md)kullanarak Azure VM'lerde çalışan iki düğümlü bir başarısız küme. Premium dosya paylaşımları, Failover Cluster Instance ile kullanılmak üzere tamamen desteklenen SSD destekli sürekli-düşük gecikmeli dosya paylaşımlarıdır.<br/><br/>3. Azure VM'lerde çalışan ve üçüncü taraf kümeleme çözümü tarafından desteklenen depolama alanı yla çalışan iki düğümlü bir başarısız küme. SIOS DataKeeper kullanan belirli bir örnek [için, failover kümeleme ve üçüncü taraf yazılım SIOS DataKeeper kullanarak bir dosya paylaşımı için yüksek kullanılabilirlik](https://azure.microsoft.com/blog/high-availability-for-a-file-share-using-wsfc-ilb-and-3rd-party-software-sios-datakeeper/)bakın.<br/><br/>4. Uzak iSCSI Target ile Azure VM'lerde çalışan iki düğümlü bir arıza kümesi ExpressRoute üzerinden blok depolamayı paylaştı. Örneğin, NetApp Private Storage (NPS), Equinix ile ExpressRoute üzerinden bir iSCSI hedefini Azure VM'lere sunar.<br/><br/>Üçüncü taraf paylaşılan depolama ve veri çoğaltma çözümleri için, failover verilerine erişimle ilgili sorunlar için satıcıya başvurmalısınız.<br/><br/>|

## <a name="azure-only-disaster-recovery-solutions"></a>Yalnızca Azure: Olağanüstü durum kurtarma çözümleri
Kullanılabilirlik gruplarını, veritabanı yansıtmayı veya yedeklemeyi kullanarak Azure'daki SQL Server veritabanlarınız için bir olağanüstü durum kurtarma çözümüne sahip olabilirsiniz ve depolama kalıpları ile geri yükleyebilirsiniz.

| Teknoloji | Örnek Mimariler |
| --- | --- |
| **Kullanılabilirlik Grupları** |Olağanüstü durum kurtarma için Azure VM'lerinde birden çok veri merkezinde çalışan kullanılabilirlik yinelemeleri. Bu bölgeler arası çözüm, tam site kesintisine karşı koruma da tır. <br/> ![Kullanılabilirlik Grupları](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-alwayson.png)<br/>Bir bölge içinde, tüm yinelemeler aynı bulut hizmeti ve aynı VNet içinde olmalıdır. Her bölgede ayrı bir VNet olduğundan, bu çözümler VNet-VNet bağlantısı gerektirir. Daha fazla bilgi için bkz: [Azure portalını kullanarak VNet'ten VNet'e bağlantı yapılandırın.](../../../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) Ayrıntılı talimatlar için bkz. [Farklı Bölgelerdeki Azure Sanal Makinelerde SQL Server Kullanılabilirlik Grubu Yapılandır.](virtual-machines-windows-portal-sql-availability-group-dr.md)|
| **Veritabanı Yansıtma** |Olağanüstü durum kurtarma için farklı veri merkezlerinde çalışan asıl ve ayna ve sunucular. Sunucu sertifikalarını kullanarak dağıtmanız gerekir. SQL Server veritabanı yansıtma, Azure VM'de SQL Server 2008 veya SQL Server 2008 R2 için desteklenmez. <br/>![Veritabanı Yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-dbmirroring.png) |
| **Azure Blob Depolama Hizmeti ile Yedekleme ve Geri Yükleme** |Üretim veritabanları, olağanüstü durum kurtarma için farklı bir veri merkezinde blob depolama doğrudan yedeklenir.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-backup-restore.png)<br/>Daha fazla bilgi için [Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme'ye](virtual-machines-windows-sql-backup-recovery.md)bakın. |
| **Azure Site Kurtarma ile SQL Server'ı Azure'a Çoğaltma ve Başarısız** |Olağanüstü durum kurtarma için farklı Azure veri merkezinin Azure Depolamasına doğrudan çoğaltılan bir Azure veri merkezinin Üretim SQL Sunucusu.<br/>![Azure Site Kurtarma'yı kullanarak çoğaltma](./media/virtual-machines-windows-sql-high-availability-dr/azure-only-dr-standalone-sqlserver-asr.png)<br/>Daha fazla bilgi için sql [server olağanüstü kurtarma ve Azure Site Kurtarma kullanarak SQL Server'ı Koru'ya](../../../site-recovery/site-recovery-sql.md)bakın. |


## <a name="hybrid-it-disaster-recovery-solutions"></a>Hibrit BT: Olağanüstü durum kurtarma çözümleri
Kullanılabilirlik gruplarını, veritabanı yansıtmayı, günlük gönderimi ve yedekleme ve Azure blog depolama ile yedekleme ve geri yükleme yi kullanarak SQL Server veritabanlarınız için karma BT ortamında olağanüstü durum kurtarma çözümüne sahip olabilirsiniz.

| Teknoloji | Örnek Mimariler |
| --- | --- |
| **Kullanılabilirlik Grupları** |Azure VM'lerinde çalışan bazı kullanılabilirlik yinelemeleri ve site ler arası olağanüstü durum kurtarma için şirket içinde çalışan diğer yinelemeler. Üretim alanı şirket içinde veya Azure veri merkezinde olabilir.<br/>![Kullanılabilirlik Grupları](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-alwayson.png)<br/>Tüm kullanılabilirlik yinelemeleri aynı failover kümesinde olması gerektiğinden, kümenin her iki ağa da (çok alt ağ üzerinden küme) yayması gerekir. Bu yapılandırma, Azure ve şirket içi ağ arasında bir VPN bağlantısı gerektirir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için, olağanüstü durum kurtarma sitesinde bir yineleme etki alanı denetleyicisi de yüklemeniz gerekir.<br/><br/>Varolan Her Zaman Kullanılabilirlik Grubuna Bir Azure yinelemesi eklemek için SSMS'te Yineleme Ekle Sihirbazı'nı kullanmak mümkündür. Daha fazla bilgi için Bkz. Öğretici: Her Zaman Kullanılabilirlik Grubunuzu Azure'a genişletin. |
| **Veritabanı Yansıtma** |Bir iş ortağı Bir Azure VM'de, diğeri de sunucu sertifikalarını kullanarak site ler arası olağanüstü durum kurtarma için şirket içinde çalışıyor. İş ortaklarının aynı Active Directory etki alanında olması gerekmez ve VPN bağlantısı gerekmez.<br/>![Veritabanı Yansıtma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-dbmirroring.png)<br/>Başka bir veritabanı yansıtma senaryosu, bir iş ortağının Azure VM'de çalışmasını ve diğerinin site ler arası olağanüstü durum kurtarma için aynı Etkin Dizin etki alanında çalışmasını içerir. [Azure sanal ağı ile şirket içi ağ arasında VPN bağlantısı](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) gereklidir.<br/><br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için, olağanüstü durum kurtarma sitesinde bir yineleme etki alanı denetleyicisi de yüklemeniz gerekir. SQL Server veritabanı yansıtma, Azure VM'de SQL Server 2008 veya SQL Server 2008 R2 için desteklenmez. |
| **Log Kargo** |Bir sunucu Azure VM'de, diğeri ise site ler arası olağanüstü durum kurtarma için şirket içinde çalışır. Günlük gönderimi Windows dosya paylaşımına bağlıdır, bu nedenle Azure sanal ağı ile şirket içi ağ arasında VPN bağlantısı gereklidir.<br/>![Log Kargo](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-log-shipping.png)<br/>Veritabanlarınızın başarılı olağanüstü durum kurtarma için, olağanüstü durum kurtarma sitesinde bir yineleme etki alanı denetleyicisi de yüklemeniz gerekir. |
| **Azure Blob Depolama Hizmeti ile Yedekleme ve Geri Yükleme** |Şirket içi üretim veritabanları, olağanüstü durum kurtarma için doğrudan Azure blob depolamasına yedeklenmiştir.<br/>![Yedekleme ve Geri Yükleme](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-backup-restore.png)<br/>Daha fazla bilgi için [Azure Sanal Makinelerde SQL Server için Yedekleme ve Geri Yükleme'ye](virtual-machines-windows-sql-backup-recovery.md)bakın. |
| **Azure Site Kurtarma ile SQL Server'ı Azure'a Çoğaltma ve Başarısız** |Şirket içi üretim SQL Server, olağanüstü durum kurtarma için doğrudan Azure Depolama'ya çoğaltılır.<br/>![Azure Site Kurtarma'yı kullanarak çoğaltma](./media/virtual-machines-windows-sql-high-availability-dr/hybrid-dr-standalone-sqlserver-asr.png)<br/>Daha fazla bilgi için sql [server olağanüstü kurtarma ve Azure Site Kurtarma kullanarak SQL Server'ı Koru'ya](../../../site-recovery/site-recovery-sql.md)bakın. |


## <a name="free-dr-replica-in-azure"></a>Azure'da ücretsiz DR çoğaltma

[Yazılım Güvenceniz](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot:primaryr3)varsa, pasif DR örneği için ek lisans lama maliyetleri olmadan SQL Server ile karma olağanüstü durum kurtarma (DR) planlarını uygulayabilirsiniz.

Aşağıdaki resimde, kurulum, 12 çekirdek kullanan şirket içi BIR SQL Server dağıtımı için 12 çekirdek kullanan bir Azure Sanal Makine'de çalışan SQL Server'ı kullanır. Geçmişte, şirket içi ve Azure Sanal Makine dağıtımı için SQL Server'ın 12 çekirdeğini lisanslanız gerekir. Yeni avantaj, Azure Sanal Makine'de çalışan pasif çoğaltma avantajları sunar. Azure Virtual Machine'deki pasif çoğaltma için olağanüstü durum kurtarma ölçütleri karşılandığı sürece, sql server'ın yalnızca 12 çekirdeğini şirket içinde çalıştırmayı lisanslamalısınız.

![Azure'da ücretsiz DR çoğaltma](media/virtual-machines-windows-sql-high-availability-dr/free-dr-replica-azure.png)

Daha fazla bilgi için [Ürün Lisans Koşulları'na](https://www.microsoft.com/licensing/product-licensing/products)bakın. 

Bu avantajı sağlamak için [SQL Server sanal makine kaynağınıza](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource)gidin, ayarlar altında **Yapılanla'yı** seçin ve ardından **SQL Server Lisansı**altında Olağanüstü Durum **Kurtarma** seçeneğini seçin. Bu SQL Server VM'nin pasif yineleme olarak kullanılacağını doğrulamak için onay kutusunu seçin ve ardından ayarlarınızı kaydetmek için **Uygula'yı** seçin. 

![Azure'da DR yinelemeyi yapılandırma](media/virtual-machines-windows-sql-high-availability-dr/dr-replica-in-portal.png)


## <a name="important-considerations-for-sql-server-hadr-in-azure"></a>Azure'da SQL Server HADR için önemli noktalar
Azure VM'leri, depolama ve ağ, şirket içi, sanallaştırılmayan BT altyapısından farklı operasyonel özelliklere sahiptir. Azure'da bir HADR SQL Server çözümünün başarılı bir şekilde uygulanması, bu farklılıkları anlamanızı ve çözümünüzü bunları karşılayacak şekilde tasarlamanızı gerektirir.

### <a name="high-availability-nodes-in-an-availability-set"></a>Kullanılabilirlik kümesindeki yüksek kullanılabilirlik düğümleri
Azure'daki kullanılabilirlik kümeleri, yüksek kullanılabilirlik düğümlerini ayrı Fay Etki Alanları (FD'ler) ve Etki Alanlarını (UD'leri) Güncelleştirme'ye yerleştirmenize olanak tanır. Kullanılabilirlik kümenizdeki her sanal makineye, temel alınan Azure platformu tarafından bir güncelleme etki alanı ve bir hata etki alanı atanır. Veri merkezi içindeki bu yapılandırma, planlanmış veya planlanmamış bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını ve %99,95 Azure SLA'sını karşılamasını sağlar. Yüksek kullanılabilirlik kurulumlarını yapılandırmak için, bir bakım olayı sırasında uygulama veya veri kaybını önlemek için tüm katılımcı SQL Sanal Makinelerini aynı kullanılabilirlik kümesine yerleştirin. Yalnızca aynı bulut hizmetindeki düğümler aynı kullanılabilirlik kümesine katılabilir. Daha fazla bilgi için bkz. [Sanal Makinelerin Kullanılabilirliğini Yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="high-availability-nodes-in-an-availability-zone"></a>Kullanılabilirlik bölgesinde yüksek kullanılabilirlik düğümleri
Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Bir bölgedeki Kullanılabilirlik Bölgeleri'nin fiziksel olarak ayrılması, en az bir sanal makinenin kullanılabilir olmasını sağlayarak uygulamaları ve verileri veri merkezi hatalarından korur ve %99,99 Azure SLA'sını karşılar. Yüksek kullanılabilirliği yapılandırmak için, katılımcı SQL Sanal Makineleri bölgedeki kullanılabilir Kullanılabilirlik Bölgeleri'ne yayılın. İlave Stoklar Arası Bölge VM-vM veri aktarım ücretleri olacaktır. Daha fazla bilgi için [Kullanılabilirlik bölgeleri'ne](/azure/availability-zones/az-overview)bakın. 


### <a name="failover-cluster-behavior-in-azure-networking"></a>Azure ağlarında başarısız küme davranışı
Azure'daki RFC uyumlu olmayan DHCP hizmeti, küme ağ adının küme düğümlerinden biri gibi aynı IP adresi gibi yinelenen bir IP adresi atanması nedeniyle, belirli başarısız küme yapılandırmalarının oluşturulmasının başarısız olmasına neden olabilir. Bu, Windows failover kümesi özelliğine bağlı olan Kullanılabilirlik Grupları'nı uyguladığınız zaman bir sorundur.

İki düğümlü bir küme oluşturulduğunda ve çevrimiçi duruma getirildiğinde senaryoyu düşünün:

1. Küme çevrimiçi olarak gelir, sonra NODE1 küme ağı adı için dinamik olarak atanmış bir IP adresi ister.
2. DHCP hizmeti isteğin NODE1'in kendisinden geldiğini kabul ettiği için, DHCP hizmeti tarafından NODE1'in kendi IP adresi dışında hiçbir IP adresi verilmediğinden, DHCP hizmeti tarafından verilir.
3. Windows, yinelenen bir adresin hem NODE1'e hem de başarısız küme ağ adına atandığını ve varsayılan küme grubunun çevrimiçi olmadığını algılar.
4. Varsayılan küme grubu, NODE1'in IP adresini küme IP adresi olarak ele alan ve varsayılan küme grubunu çevrimiçi duruma getiren NODE2'ye taşınır.
5. NODE2 NODE1 ile bağlantı kurmaya çalıştığında, NODE1'e yönelik paketler NODE1'in IP adresini kendisine çözdüğünden NODE2'den asla ayrılmaz. NODE2 NODE1 ile bağlantı kuramaz, daha sonra çoğunluk kaybeder ve küme kapatır.
6. Bu arada, NODE1 paketleri NODE2'ye gönderebilir, ancak NODE2 yanıt veremez. NODE1 yeterli çoğunluk kaybeder ve kümeyi kapatır.

Bu senaryo, küme ağ adını çevrimiçi duruma getirmek için küme ağı adına 169.254.1.1 gibi bir bağlantı yerel IP adresi gibi kullanılmayan bir statik IP adresi atayarak önlenebilir. Bu işlemi kolaylaştırmak [için, kullanılabilirlik grupları için Azure'da Windows başarısız kümesini yapılandırma'ya](https://social.technet.microsoft.com/wiki/contents/articles/14776.configuring-windows-failover-cluster-in-windows-azure-for-alwayson-availability-groups.aspx)bakın.

Daha fazla bilgi için azure ['daki kullanılabilirlik gruplarını yapılandır](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)'a (GUI) bakın.

### <a name="availability-group-listener-support"></a>Kullanılabilirlik grubu dinleyici desteği
Kullanılabilirlik grubu dinleyicileri, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016 çalıştıran Azure VM'lerde desteklenir. Bu destek, kullanılabilirlik grubu düğümleri olan Azure VM'lerinde etkinleştirilmiş yük dengeli uç noktalarının kullanılmasıyla sağlanır. Dinleyicilerin Hem Azure'da çalışan istemci uygulamaları hem de şirket içinde çalışan istemci uygulamaları için çalışması için özel yapılandırma adımlarını izlemeniz gerekir.

Dinleyicinizi ayarlamak için iki ana seçenek vardır: dış (genel) veya dahili. Harici (genel) dinleyici yük dengeleyicisi ile karşı karşıya bir internet kullanır ve internet üzerinden erişilebilen genel bir Sanal IP (VIP) ile ilişkilidir. Dahili bir dinleyici dahili yük dengeleyicisi kullanır ve yalnızca aynı Sanal Ağ içindeki istemcileri destekler. Yük bakiyesi türü için Doğrudan Sunucu İadesi'ni etkinleştirmeniz gerekir. 

Kullanılabilirlik Grubu birden çok Azure alt ağına (Azure bölgeleri geçen bir dağıtım gibi) yayılıyorsa, istemci bağlantı dizesi "**MultisubnetFailover=True**" içermelidir. Bu, farklı alt ağlardaki yinelemelere paralel bağlantı girişimleriyle sonuçlanır. Dinleyici ayarlama yla ilgili talimatlar için bkz.

* [Azure'daki kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın.](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md)
* [Azure'daki kullanılabilirlik grupları için harici bir dinleyiciyi yapılandırın.](../sqlclassic/virtual-machines-windows-classic-ps-sql-ext-listener.md)

Doğrudan hizmet örneğine bağlanarak her kullanılabilirlik yinelemesine ayrı ayrı bağlanmaya devam edebilirsiniz. Ayrıca, kullanılabilirlik grupları veritabanı yansıtma istemcileri ile geriye doğru uyumlu olduğundan, yinelemeler veritabanı yansıtmabenzer şekilde yapılandırıldığı sürece veritabanı yansıtma ortakları gibi kullanılabilirlik yinelemelerine bağlanabilirsiniz:

* Bir birincil yineleme ve bir ikincil yineleme
* İkincil yineleme okunamayan olarak yapılandırılır (**Okunabilir İkincil** seçenek **No**olarak ayarlanır )

ADO.NET veya SQL Server Native Client kullanarak bu veritabanı yansıtma benzeri yapılandırmaya karşılık gelen örnek bir istemci bağlantı dizesi aşağıda verilmiştir:

    Data Source=ReplicaServer1;Failover Partner=ReplicaServer2;Initial Catalog=AvailabilityDatabase;

İstemci bağlantısı hakkında daha fazla bilgi için bkz:

* [Bağlantı String Anahtar Kelimelerini SQL Server Native Client ile Kullanma](https://msdn.microsoft.com/library/ms130822.aspx)
* [İstemcileri Veritabanı Yansıtma Oturumuna Bağla (SQL Server)](https://technet.microsoft.com/library/ms175484.aspx)
* [Hibrit BT'de Kullanılabilirlik Grubu Dinleyicisine Bağlanma](https://blogs.msdn.com/b/sqlalwayson/archive/2013/02/14/connecting-to-availability-group-listener-in-hybrid-it.aspx)
* [Kullanılabilirlik Grubu Dinleyicileri, İstemci Bağlantısı ve Uygulama Failover (SQL Server)](https://technet.microsoft.com/library/hh213417.aspx)
* [Kullanılabilirlik Gruplarıyla Veritabanı Yansıtma Bağlantı Dizelerini Kullanma](https://technet.microsoft.com/library/hh213417.aspx)

### <a name="network-latency-in-hybrid-it"></a>Karma BT'de ağ gecikmesi
HADR çözümünüzü, şirket içi ağınızla Azure arasında yüksek ağ gecikmesi nedeniyle zaman dilimleri olabileceği varsayımıyla dağıtmanız gerekir. Yinelemeleri Azure'a dağıtırken, eşitleme modu için eşzamanlı işleme yerine eşsenkronize commit kullanmalısınız. Veritabanı yansıtma sunucularını hem şirket içinde hem de Azure'da dağıtırken, yüksek güvenlik modu yerine yüksek performans modunu kullanın.

### <a name="geo-replication-support"></a>Coğrafi çoğaltma desteği
Azure disklerinde coğrafi çoğaltma, ayrı disklerde depolanacak aynı veritabanının veri dosyasını ve günlük dosyasını desteklemez. GRS, her diskteki değişiklikleri bağımsız olarak ve eşzamanlı olarak çoğaltır. Bu mekanizma, coğrafi olarak çoğaltılan kopyadaki tek bir diskteki yazma sırasını garanti eder, ancak birden çok diskin coğrafi olarak çoğaltılan kopyaları arasında garanti etmez. Veri dosyasını ve günlük dosyasını ayrı disklerde depolamak için bir veritabanını yapılandırırsanız, bir felaketten sonra kurtarılan diskler, sql server'daki yazma oturumu ve ACID özelliklerini kıran günlük dosyasından daha güncel bir veri dosyası içerebilir Hareket. Depolama hesabında coğrafi çoğaltmayı devre dışı atma seçeneğiniz yoksa, belirli bir veritabanıiçin tüm verileri ve günlük dosyalarını aynı diskte tutmanız gerekir. Veritabanının boyutu nedeniyle birden fazla disk kullanmanız gerekiyorsa, veri artıklığını sağlamak için yukarıda listelenen olağanüstü durum kurtarma çözümlerinden birini dağıtmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
SQL Server ile bir Azure sanal makinesi oluşturmanız gerekiyorsa, [Azure'da SQL Server Virtual Machine Sağlama](virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın.

Azure VM'de çalışan SQL Server'dan en iyi performansı elde etmek için Azure [Sanal Makinelerde SQL Server için En İyi Performans Uygulamaları kılavuzuna](virtual-machines-windows-sql-performance.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makineler'deki SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın.

### <a name="other-resources"></a>Diğer kaynaklar
* [Azure'da yeni bir Active Directory ormanı yükleme](../../../active-directory/active-directory-new-forest-virtual-machine.md)
* [Azure VM'deki kullanılabilirlik grupları için Failover Kümesi oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a)

