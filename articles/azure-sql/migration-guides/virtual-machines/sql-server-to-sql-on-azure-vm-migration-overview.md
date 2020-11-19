---
title: Azure VM 'de SQL Server SQL Server (geçişe genel bakış)
description: SQL Server Azure VM 'lerine SQL Server geçirmek istediğinizde farklı geçiş stratejileri hakkında bilgi edinin.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: ''
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: d47abaade13958b4e28d3ad5f62b88e8a53e89a9
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94917850"
---
# <a name="migration-overview-sql-server-to-sql-server-on-azure-vms"></a>Geçişe genel bakış: Azure VM 'lerinde SQL Server SQL Server
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

SQL Server Azure sanal makinelerinde (VM) SQL Server geçirmek için farklı geçiş stratejileri hakkında bilgi edinin. 

Şirket içinde veya üzerinde çalışan SQL Server geçirebilirsiniz:

- Sanal Makinelerde SQL Server  
- Amazon Web Services (AWS) EC2 
- Amazon Ilişkisel veritabanı hizmeti (AWS RDS) 
- İşlem altyapısı (Google Cloud Platform-GCP)

Diğer senaryolar için [veritabanı geçiş kılavuzuna](https://datamigration.microsoft.com/)bakın. 

## <a name="overview"></a>Genel Bakış

İşletim sistemi denetimiyle tanıdık SQL Server ortamını kullanmak istediğinizde [Azure sanal makinelerinde (VM 'ler) SQL Server](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) geçirin ve yerleşik VM yüksek kullanılabilirlik, [otomatik yedeklemeler](../../virtual-machines/windows/automated-backup.md)ve [otomatik düzeltme eki uygulama](../../virtual-machines/windows/automated-patching.md)gibi bulut tarafından sunulan özelliklerden yararlanın. 

[Azure hibrit avantajı lisanslama modeliyle](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md) kendi lisansınızı taşıyarak veya [ücretsiz güvenlik güncelleştirmeleri](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)alarak SQL Server 2008 ve SQL Server 2008 R2 desteğini genişleterek maliyetlerde tasarruf edin. 


## <a name="choosing-appropriate-target"></a>Uygun hedef seçiliyor

Azure sanal makineleri birçok farklı Azure bölgesinde çalışır ve ayrıca çeşitli [makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) ve [depolama seçenekleri](https://docs.microsoft.com/azure/virtual-machines/disks-types)sunar. SQL Server iş yükünüz için doğru sanal makine ve depolama boyutunu belirlerken [Azure sanal makinelerinde SQL Server Için performans yönergelerine bakın.](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/performance-guidelines-best-practices#vm-size-guidance) İş yükünüz için VM boyutunu ve depolama gereksinimlerini belirleme. Bunların Performance-Based [Azure geçişi değerlendirmesi](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#types-of-assessments)aracılığıyla boyutlandırılması önerilir. Bu kullanılabilir bir seçenek değilse, [performans için kendi taban çizgisini](https://azure.microsoft.com/services/virtual-machines/sql-server/)oluşturma konusunda aşağıdaki makaleye bakın.

Ayrıca, bir VM üzerinde SQL Server doğru yükleme ve yapılandırma üzerinde de yapılmalıdır. Bu, doğru sürüm, sürüm ve işletim sistemi ile bir SQL Server VM oluşturmanıza olanak sağlayan [Azure SQL sanal makine görüntüsü galerisinin](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal) kullanılması önerilir. Ayrıca, otomatik yedeklemeler ve otomatik düzeltme eki uygulama gibi özellikleri etkinleştirerek Azure VM 'yi SQL Server [kaynak sağlayıcısıyla](https://docs.microsoft.com/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal) otomatik olarak kaydeder.

## <a name="migration-strategies"></a>Geçiş stratejileri

Kullanıcı veritabanlarınızı Azure VM 'lerinde bir SQL Server örneğine geçirmeye yönelik iki geçiş stratejisi vardır: **geçir** ve **Yükselt ve Kaydır**. 

İşletmeniz için uygun yaklaşım genellikle aşağıdaki etkenlere bağlıdır: 

- Geçişin boyutu ve ölçeği
- Geçişin hızı
- Kod değişikliği için uygulama desteği
- SQL Server sürümünü, Işletim sistemini veya her ikisini de değiştirmeniz gerekir.
- Mevcut ürünlerinizin desteklenebilirlik yaşam döngüsü
- Geçiş sırasında uygulama kesinti süresi için pencere

:::image type="content" source="media/sql-server-to-sql-on-azure-vm-individual-databases-guide/virtual-machine-migration-downtime.png" alt-text="sanal makine geçişi kapalı kalma süresi":::

Aşağıdaki tabloda, iki geçiş stratejisinden farkları açıklanmaktadır:
<br />

| **Geçiş stratejisi** | **Açıklama** | **Kullanılması gereken durumlar** |
| --- | --- | --- |
| **& Shift tuşunu kaldırın** | Tüm fiziksel veya sanal SQL Server geçerli konumundan, işletim sisteminde veya SQL Server sürümünde herhangi bir değişiklik yapmadan Azure VM 'deki bir SQL Server örneğine taşımak için Kaldır ve Shift geçiş stratejisini kullanın. Bir kaldırma ve kaydırma geçişini gerçekleştirmek için bkz. [Azure geçişi](../../../migrate/migrate-services-overview.md). <br /><br /> Kaynak sunucu, neredeyse sorunsuz bir geçişe izin vererek kaynak ve hedef sunucu verileri eşitlerken çevrimiçi ve hizmet istekleri olarak kalır. | Veri merkezi çıkışı gibi senaryolar için geçerli olan tek ve çok büyük ölçekli geçişler için kullanın. <br /><br /> Kullanıcı SQL veritabanları veya uygulamaları için gerekli kod değişikliği olmaması için en az, daha hızlı geçiş yapılmasına izin verir. <br /><br />[SSIS,](/sql/integration-services/sql-server-integration-services) [SSRS](/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports)ve [SSAS](/analysis-services/analysis-services-overview)gibi iş zekası hizmetlerini geçirmek için ek adım gerekmez. |
|**Geçiş** | Hedef SQL Server ve/veya işletim sistemi sürümünü yükseltmek istediğinizde bir geçiş stratejisi kullanın. <br /> <br /> Azure Marketi 'nden bir Azure VM veya kaynak SQL Server sürümüyle eşleşen hazırlanmış bir SQL Server görüntüsü seçin. | SQL Server veya daha yeni sürümlerde bulunan özellikleri kullanmak istediğinizde veya artık destek içinde olmayan eski SQL Server ve/veya işletim sistemi sürümlerini yükseltmek için bir gereksinim varsa kullanın.  <br /> <br /> SQL Server yükseltmesini desteklemek için bazı uygulama veya Kullanıcı veritabanı değişiklikleri gerektirebilir. <br /><br />Geçiş kapsamınızda [Business Intelligence](#business-intelligence) hizmetlerini geçirmeye yönelik ek hususlar olabilir. |


## <a name="lift-and-shift"></a>Lift and shift  

Aşağıdaki tabloda, SQL Server veritabanınızı Azure VM 'lerinde SQL Server geçirmek için, kaldırma **ve kaydırma** geçiş stratejisinin mevcut yöntemi ayrıntılı olarak verilmiştir:
<br />

|**Yöntem** | **En düşük kaynak sürümü** | **En düşük hedef sürüm** | **Kaynak yedekleme boyutu kısıtlaması** |  **Notlar** |
| --- | --- | --- | --- | --- |
| [Azure Geçişi](../../../migrate/index.yml) | SQL Server 2008 SP4| SQL Server 2008 SP4| [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  Bir Azure sanal makinesinde SQL Server örneğine olduğu gibi, mevcut SQL Server. , 35.000 adede kadar VM 'ye kadar geçiş iş yüklerini ölçeklendirebilir. <br /><br /> Kaynak sunucu, sunucu verilerinin eşitlenmesi sırasında çevrimiçi ve hizmet isteklerine devam eder, kapalı kalma süresini en aza indirir. <br /><br /> **Otomasyon & betiği oluşturma**: [Azure Site Recovery betikler](../../../migrate/how-to-migrate-at-scale.md) ve [ölçeklendirme ve Azure için planlama](/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale) örnekleri|

## <a name="migrate"></a>Geçiş  

Kurulum kolaylığı nedeniyle, önerilen geçiş yaklaşımı yerel bir SQL Server [yedeklemesini](/sql/t-sql/statements/backup-transact-sql) yerel olarak almak ve sonra dosyayı Azure 'a kopyalamaktır. Bu yöntem, 2008 ve daha büyük veritabanı yedeklerinden (>1 TB) başlayan tüm SQL Server sürümleri için daha büyük veritabanlarını (>1 TB) destekler. Ancak, 1 TB 'tan küçük olan ve Azure 'a iyi bir bağlantı içeren SQL Server 2014 ' den başlayan veritabanları için [yedekleme, URL 'ye SQL Server](/sql/relational-databases/backup-restore/sql-server-backup-to-url) daha iyi bir yaklaşımdır. 

SQL Server veritabanlarını Azure VM 'lerde bir SQL Server örneğine geçirirken, bu uygulama kapalı kalma süresi penceresini etkilediği için hedef sunucuya Cua eklemek istediğinizde uygun bir yaklaşım seçmek önemlidir.

Aşağıdaki tabloda, Azure VM 'lerinde SQL Server veritabanınızı SQL Server geçirmeye yönelik tüm mevcut yöntemler ayrıntılı olarak verilmiştir:
<br />

|**Yöntem** | **En düşük kaynak sürümü** | **En düşük hedef sürüm** | **Kaynak yedekleme boyutu kısıtlaması** | **Notlar** |
| --- | --- | --- | --- | --- |
| **[Bir dosyaya yedekleme](sql-server-to-sql-on-azure-vm-individual-databases-guide.md#migrate)** | SQL Server 2008 SP4 | SQL Server 2008 SP4| [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  Bu, veritabanlarını makineler arasında taşımak için basit ve iyi test edilmiş bir tekniktir. Aktarım için yedekleme boyutunu en aza indirmek için sıkıştırmayı kullanın. <br /><br /> **Automation & betiği oluşturma**: [Transact-SQL (T-SQL)](/sql/t-sql/statements/backup-transact-sql) ve [AzCopy for blob Storage](../../../storage/common/storage-use-azcopy-v10.md)  |
| **[URL 'ye yedekleme](/sql/relational-databases/backup-restore/sql-server-backup-to-url)** | SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ | SQL Server 2012 SP1 CU2 UYGULAMAZSANıZ| SQL Server 2016 için 12,8 TB, yoksa 1 TB | Azure Storage kullanarak yedekleme dosyasını VM 'ye taşımanın alternatif bir yolu. Aktarım için yedekleme boyutunu en aza indirmek için sıkıştırmayı kullanın. <br /><br /> **Otomasyon & betiği oluşturma**:  [T-SQL veya bakım planı](/sql/relational-databases/backup-restore/sql-server-backup-to-url) |
| **[Veritabanı Geçiş Yardımcısı (DMA)](/sql/dma/dma-overview)** | SQL Server 2005| SQL Server 2008 SP4| [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  [DMA](/sql/dma/dma-overview) değerlendirir, şirket içinde SQL Server ve daha sonra SQL Server veya Azure sanal makinelerinde, Azure SQL veritabanı veya Azure SQL yönetilen örneği SQL Server 'e geçirir. <br /><br /> FILESTREAM özellikli Kullanıcı veritabanlarında kullanılmamalıdır.<br /><br /> DMA Ayrıca [SQL ve Windows oturum açma işlemlerini](/sql/dma/dma-migrateserverlogins) geçirme ve [SSIS paketlerini](/sql/dma/dma-assess-ssis)değerlendirme özelliğini içerir. <br /><br /> **Automation & betiği oluşturma**: [komut satırı arabirimi](/sql/dma/dma-commandline) |
| **[Ayır ve Ekle](../../virtual-machines/windows/migrate-to-vm-from-sql-server.md#detach-and-attach-from-a-url)** | SQL Server 2008 SP4 | SQL Server 2014 | [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) | Bu [dosyaları Azure Blob depolama hizmetini kullanarak depolamayı](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure) ve BUNLARı Azure sanal makinesinde bir SQL Server örneğine iliştirmeyi planlarken, özellikle çok büyük veritabanlarında veya yedekleme ve geri yükleme zamanının çok uzun olduğu durumlarda bu yöntemi kullanın. <br /><br /> **Automation & betiği oluşturma**: blob depolamaya  [T-SQL](/sql/relational-databases/databases/detach-a-database#TsqlProcedure) ve [AzCopy](../../../storage/common/storage-use-azcopy-v10.md)|
|**[Günlük aktarma](sql-server-to-sql-on-azure-vm-individual-databases-guide.md#migrate)** | SQL Server 2008 SP4 (yalnızca Windows) | SQL Server 2008 SP4 (yalnızca Windows) | [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) | Günlük aktarma, işlem günlüğü dosyalarını Şirket içinden Azure VM üzerindeki bir SQL Server örneğine çoğaltır. <br /><br /> Bu, yük devretme sırasında en az kapalı kalma süresi sağlar ve her zaman açık kullanılabilirlik grubu ayarından daha az yapılandırma yüküne sahiptir. <br /><br /> **Otomasyon & betiği oluşturma**: [T-SQL](/sql/database-engine/log-shipping/log-shipping-tables-and-stored-procedures)  |
| **[Dağıtılmış kullanılabilirlik grubu](../../virtual-machines/windows/business-continuity-high-availability-disaster-recovery-hadr-overview.md#hybrid-it-disaster-recovery-solutions)** | SQL Server 2016| SQL Server 2016 | [Azure VM depolama sınırı](https://azure.microsoft.com/documentation/articles/azure-resource-manager/management/azure-subscription-service-limits/) |  [Dağıtılmış kullanılabilirlik grubu](/sql/database-engine/availability-groups/windows/distributed-availability-groups) , iki ayrı kullanılabilirlik grubunu kapsayan özel bir kullanılabilirlik grubu türüdür. Dağıtılmış bir kullanılabilirlik grubuna katılan kullanılabilirlik gruplarının aynı konumda olması ve etki alanları arası destek içermesi gerekmez. <br /><br /> Bu yöntem kapalı kalma süresini en aza indirir ve şirket içinde yapılandırılmış bir kullanılabilirlik grubunuz olduğunda kullanın. <br /><br /> **Otomasyon & betiği oluşturma**: [T-SQL](/sql/t-sql/statements/alter-availability-group-transact-sql)  |
| | | | | |

&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;
 
> [!TIP]
> Ağ seçenekleri olmadan sınırlı olan büyük veri aktarımları için bkz. [sınırlı bağlantı Ile büyük veri aktarımları](../../../storage/common/storage-solution-large-dataset-low-network.md).
> 

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıda, geçiş yöntemlerini gözden geçirirken dikkate alınması gereken önemli noktaların listesi verilmiştir:

- En iyi veri aktarımı performansı için, sıkıştırılmış bir yedekleme dosyası kullanarak veritabanlarını ve dosyaları Azure VM 'de bir SQL Server örneğine geçirin. Daha büyük veritabanları için, sıkıştırmaya ek olarak yedekleme ve aktarım sırasında daha yüksek performans için [yedekleme dosyasını daha küçük dosyalara bölebilirsiniz](/sql/relational-databases/backup-restore/back-up-files-and-filegroups-sql-server) . 
- SQL Server 2014 veya üzeri bir değere geçiş yapıyorsanız, ağ aktarımı sırasında verileri korumak için [yedeklemeleri şifrelemeyi](/sql/relational-databases/backup-restore/backup-encryption) göz önünde bulundurun.
- Veritabanı geçişi sırasında kapalı kalma süresini en aza indirmek için, Always on kullanılabilirlik grubu seçeneğini kullanın. 
- Kullanılabilirlik grubu yapılandırma yükü olmadan kapalı kalma süresini en aza indirmek için günlük aktarma seçeneğini kullanın. 
- Ağ seçeneği olmadan sınırlı olmamak için yedekleme ve geri yükleme gibi çevrimdışı geçiş yöntemlerini veya Azure 'da bulunan [disk aktarım hizmetlerini](../../../storage/common/storage-solution-large-dataset-low-network.md) kullanın.
- Ayrıca, Azure VM 'de bir SQL Server SQL Server sürümünü değiştirmek için, bkz. [SQL Server sürümünü değiştirme](../../virtual-machines/windows/change-sql-server-edition.md).

## <a name="business-intelligence"></a>İş Zekası 

SQL Server Iş zekası hizmetlerini Kullanıcı veritabanı geçişlerinin kapsamı dışında geçirirken ek hususlar olabilir. 

Bu hizmetler arasında şunlar bulunur:

- [**SQL Server Integration Services (SSIS)**](/sql/integration-services/install-windows/upgrade-integration-services)
- [**SQL Server Reporting Services (SSRS)**](/sql/reporting-services/install-windows/upgrade-and-migrate-reporting-services)
- [**SQL Server Analysis Services (SSAS)**](/sql/database-engine/install-windows/upgrade-analysis-services)

## <a name="supported-versions"></a>Desteklenen sürümler

SQL Server veritabanlarını Azure VM 'lerinde SQL Server geçirmeye hazırlanırken, desteklenen SQL Server sürümlerini göz önünde bulundurduğunuzdan emin olun. Azure VM 'lerinde desteklenen geçerli SQL Server sürümlerinin bir listesi için lütfen bkz. [Azure VM 'lerinde SQL Server](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md#get-started-with-sql-server-vms).


## <a name="next-steps"></a>Sonraki adımlar

SQL Server veritabanlarınızı Azure VM 'lerinde SQL Server geçirmeye başlamak için, [tek veritabanı geçiş kılavuzu](sql-server-to-sql-on-azure-vm-individual-databases-guide.md)' na bakın. 

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için kullanabileceğiniz Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için, [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md) makalesine bakın.

- Azure SQL hakkında daha fazla bilgi için bkz.
   - [Dağıtım seçenekleri](../../azure-sql-iaas-vs-paas-what-is-overview.md)
   - [Azure VM’lerinde SQL Server](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
   - [Azure toplam sahiplik Hesaplayıcı maliyeti](https://azure.microsoft.com/pricing/tco/calculator/) 


- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 

- Lisanslama hakkında daha fazla bilgi için bkz.
   - [Azure Hibrit Avantajı kendi lisansınızı getirin](../../virtual-machines/windows/licensing-model-azure-hybrid-benefit-ahb-change.md)
   - [SQL Server 2008 ve SQL Server 2008 R2 için ücretsiz genişletilmiş destek alın](../../virtual-machines/windows/sql-server-2008-extend-end-of-support.md)


- Uygulama erişim katmanını değerlendirmek için bkz. [veri erişimi geçiş araç seti (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Veri erişim katmanını bir/B testi gerçekleştirme hakkında daha fazla bilgi için bkz. [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview).
