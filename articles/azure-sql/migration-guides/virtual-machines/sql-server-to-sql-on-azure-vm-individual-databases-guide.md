---
title: Azure VM 'lerinde SQL Server SQL Server (Geçiş Kılavuzu)
description: Bireysel SQL Server veritabanlarınızı Azure sanal makinelerinde (VM) SQL Server geçirmek için bu kılavuzu izleyin.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: ''
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: 1558c396566b2fcfc098a749407d5e7a28316b6f
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95019458"
---
# <a name="migration-guide-sql-server-to-sql-server-on-azure-vms"></a>Geçiş Kılavuzu: Azure VM 'lerinde SQL Server SQL Server 
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

Bu geçiş kılavuzunda, değerlendirme için [veritabanı geçiş Yardımcısı (DMA)](/sql/dma/dma-overview) kullanarak yedekleme ve geri yükleme ve günlük aktarma aracılığıyla SQL Server Kullanıcı veritabanlarınızı Azure sanal makinelerinde (vm) SQL Server bir örneğine bulmanıza, **değerlendirmenize** ve **geçireceğinizi** **öğreder**. 

Şirket içinde veya üzerinde çalışan SQL Server geçirebilirsiniz:

- Sanal Makinelerde SQL Server  
- Amazon Web Services (AWS) EC2 
- Amazon Ilişkisel veritabanı hizmeti (AWS RDS) 
- İşlem altyapısı (Google Cloud Platform-GCP)

Ek geçiş stratejileri hakkında daha fazla bilgi için [SQL Server VM geçişe genel bakış](sql-server-to-sql-on-azure-vm-migration-overview.md)bölümüne bakın.

:::image type="content" source="media/sql-server-to-sql-on-azure-vm-migration-overview/migration-process-flow-small.png" alt-text="Geçiş işlem akışı":::

## <a name="prerequisites"></a>Önkoşullar

Azure VM 'lerinde SQL Server 'e geçiş yapmak için şunlar gerekir: 

- [Veritabanı geçiş Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).
- Bir [Azure geçişi projesi](/azure/migrate/create-manage-projects).
- Kaynak SQL Server aynı veya daha fazla sürümü olan [Azure VM 'de](/azure/azure-sql/virtual-machines/windows/create-sql-vm-portal) hazırlanan bir hedef SQL Server.
- [Azure ile şirket içi arasında bağlantı](/azure/architecture/reference-architectures/hybrid-networking).
- [Uygun bir geçiş stratejisi seçme](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate).

## <a name="pre-migration"></a>Geçiş öncesi

Geçişinize başlamadan önce, SQL ortamınızın topolojisini bulun ve hedeflediğiniz geçişinizin uygunluğunu değerlendirin. 

### <a name="discover"></a>Bulma

Şirket içi bilgisayarlardan Azure geçişi değerlendirir geçiş uygunluğu, performans tabanlı boyutlandırma gerçekleştirir ve şirket içinde çalışmaya yönelik maliyet tahminleri sağlar. Geçişi planlamak için, [mevcut veri kaynaklarını ve SQL Server örneklerinizin kullanıldığı özelliklerle ilgili ayrıntıları belirlemek](../../../migrate/concepts-assessment-calculation.md) Için Azure geçişi ' ni kullanın. Bu işlem, kullanımdaki sürüm ve özelliklerle kuruluşunuzdaki tüm SQL Server örneklerinizi belirlemek üzere ağı taramayı içerir. 

> [!IMPORTANT]
> SQL Server Örneğiniz için bir hedef Azure sanal makinesi seçerken, [Azure VM 'lerinde SQL Server Için performans kılavuzunu](../../virtual-machines/windows/performance-guidelines-best-practices.md)göz önünde bulundurduğunuzdan emin olun.

Ek keşif araçları için bkz. veri geçiş senaryoları için kullanılabilen [Hizmetler ve araçlar](../../../dms/dms-tools-matrix.md#business-justification-phase) .


### <a name="assess"></a>Değerlendirme

Tüm veri kaynaklarını bulduktan sonra, kaynak ve hedef örnekler arasındaki boşlukları anlamak üzere Azure VM 'de bir SQL Server örneğine geçişi SQL Server değerlendirmek için [Data Migration Yardımcısı (DMA)](/sql/dma/dma-overview) kullanın. 


> [!NOTE]
> SQL Server sürümünü _yükseltmiyorsanız_ , bu adımı atlayın ve [geçiş](#migrate) bölümüne geçin. 


#### <a name="assess-user-databases"></a>Kullanıcı veritabanlarını değerlendirme 

Data Migration Yardımcısı (DMA), yeni SQL Server sürümünüzde veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılayarak modern bir veri platformuna geçişinize yardımcı olur. DMA, hedef ortamınız için performans ve güvenilirlik iyileştirmeleri önerir ve ayrıca, şema, veri ve oturum açma nesnelerinizi kaynak sunucunuzdaki hedef sunucunuza taşımanızı sağlar.

Daha fazla bilgi için bkz. [değerlendirme](/sql/dma/dma-migrateonpremsql) . 


> [!IMPORTANT]
>Değerlendirme türüne göre, kaynak SQL Server gereken izinler farklı olabilir. 
   > - **Özellik eşlik** Danışmanı için, kaynak SQL Server veritabanına bağlanmak için girilen kimlik bilgileri *sysadmin* sunucu rolünün bir üyesi olmalıdır.
   > - Uyumluluk sorunları Danışmanı için, belirtilen kimlik bilgileri en az `CONNECT SQL` ve izinlerine sahip olmalıdır `VIEW SERVER STATE` `VIEW ANY DEFINITION` .
   > - DMA, değerlendirmeyi çalıştırmadan önce, seçilen Advisor için gereken izinleri vurgulayacaktır.


#### <a name="assess-applications"></a>Uygulamaları değerlendirme

Genellikle bir uygulama katmanı, verileri kalıcı hale getirmek ve değiştirmek için Kullanıcı veritabanlarına erişir.  DMA, bir uygulamanın veri erişim katmanını iki şekilde değerlendirebilirler: 

- Yakalanan [Genişletilmiş olayları](/sql/relational-databases/extended-events/extended-events) veya Kullanıcı veritabanlarınızın [SQL Server Profiler izlemelerini ](/sql/tools/sql-server-profiler/create-a-trace-sql-server-profiler) kullanma. Ayrıca, A/B testi için de kullanılabilecek bir izleme günlüğü oluşturmak için [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-capture-trace) de kullanabilirsiniz.

- Kod içinde SQL sorgularının bulunmasını ve değerlendirmesini sağlayan [veri erişimi geçiş araç seti 'ni (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) (ZMT) kullanarak uygulama kaynak kodunu bir veritabanı platformundan diğerine geçirmek için kullanılır. Bu araç C# ve Java, XML ve Plaınt metinler dahil olmak üzere çeşitli popüler dosya türlerini destekler. Bir DAVMT değerlendirmesi gerçekleştirmeye yönelik bir kılavuz için bkz. [dvade blog kullanma](https://techcommunity.microsoft.com/t5/microsoft-data-migration/using-data-migration-assistant-to-assess-an-application-s-data/ba-p/990430) .

Kullanıcı veritabanlarının değerlendirmesi sırasında yakalanan izleme dosyalarını veya DAVMT dosyalarını [içeri aktarmak](/sql/dma/dma-assesssqlonprem#add-databases-and-extended-events-trace-to-assess) için DMA 'yı kullanın. 


#### <a name="scale-assessments"></a>Ölçek değerlendirmeleri

DMA değerlendirmesi gerektiren birden çok sunucunuz varsa, [komut satırı arabirimi](/sql/dma/dma-commandline)aracılığıyla işlemi otomatikleştirebilirsiniz. Arabirimini kullanarak, geçirme kapsamındaki her bir SQL Server örneği için değerlendirme komutlarını önceden hazırlayabilirsiniz. 

Büyük Estates üzerinde Özet raporlama için Data Migration Yardımcısı (DMA) değerlendirmeleri artık [Azure geçişi](/sql/dma/dma-assess-sql-data-estate-to-sqldb)ile birleştirilebilir.

#### <a name="refactor-databases-with-dma"></a>DMA ile veritabanlarını yeniden düzenleme

DMA değerlendirmesi sonuçlarına bağlı olarak, Kullanıcı veritabanınızın geçişten sonra doğru şekilde çalışmasını sağlamak için bir dizi öneriniz olabilir. DMA, etkilenen nesneler ve her bir sorunu çözme kaynakları hakkında ayrıntılar sağlar. Tüm son değişiklikler ve davranış değişikliklerinin üretim geçişinden önce çözümlenmesi önerilir.

Kullanım dışı bırakılan özellikler için, bu değişiklikleri yapmaktan kaçınmak ve geçişi hızlandırmak istiyorsanız Kullanıcı veritabanınızı özgün [Uyumluluk](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) modunda çalıştırmayı tercih edebilirsiniz. Ancak, bu, kullanım dışı bırakılan öğeler çözümlenene kadar [veritabanı uyumluluğuna yükseltme](/sql/database-engine/install-windows/compatibility-certification#compatibility-levels-and-database-engine-upgrades) yapılmasını engeller.

Tüm DMA düzeltmelerinin, [geçiş sonrası](#post-migration)için komut dosyası ve hedef SQL Server veritabanına uygulanması önemle önerilir.

> [!CAUTION]
> Tüm SQL Server sürümleri tüm uyumluluk modlarını desteklemez. [Hedef SQL Server sürümünüzün](/sql/t-sql/statements/alter-database-transact-sql-compatibility-level) seçtiğiniz veritabanı uyumluluğunu desteklediğinden emin olun. Örneğin, SQL Server 2019, düzey 90 uyumluluğu olan veritabanlarını desteklemez (SQL Server 2005). Bu veritabanları, en azından uyumluluk düzeyi 100 ' e yükseltme yapılmasını gerektirir.
>

## <a name="migrate"></a>Geçiş

Geçiş öncesi adımları tamamladıktan sonra, Kullanıcı veritabanlarını ve bileşenlerini geçirmeye hazırlanın. Tercih ettiğiniz [geçiş yönteminizi](sql-server-to-sql-on-azure-vm-migration-overview.md#migrate)kullanarak veritabanlarınızı geçirin.  

Aşağıdakiler, yedekleme ve geri yükleme kullanarak bir geçiş gerçekleştirmeye ya da yedekleme ve geri yükleme ile günlük aktarımına kadar en az kesinti süresi geçişine yönelik adımlar sağlar. 

### <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Yedekleme ve geri yükleme kullanarak standart bir geçiş gerçekleştirmek için aşağıdaki adımları izleyin: 

1. Gereksinimlerinize göre Azure VM 'de hedef SQL Server bağlantı kurun. Bkz. [Azure 'da SQL Server sanal makinesine bağlanma (Kaynak Yöneticisi)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Geçiş için tasarlanan veritabanlarını kullanan tüm uygulamaları duraklatın/durdurun. 
1. Kullanıcı veritabanlarının [Tek Kullanıcı modu](/sql/relational-databases/databases/set-a-database-to-single-user-mode)kullanılarak etkin olmadığından emin olun. 
1. Şirket içi bir konumda tam bir veritabanı yedeklemesi gerçekleştirin.
1. Şirket içi yedekleme dosyanızı Uzak Masaüstü, [Azure Veri Gezgini](/azure/data-explorer/data-explorer-overview)veya [AzCopy komut satırı YARDıMCı programını](../../../storage/common/storage-use-azcopy-v10.md) kullanarak sanal makinenize KOPYALAYıN (> 2 TB yedeklemeler önerilir).
1. Tam veritabanı yedeklerini Azure VM 'de SQL Server geri yükleyin.

### <a name="log-shipping--minimize-downtime"></a>Günlük aktarma (kapalı kalma süresini en aza indir)

Yedekleme, geri yükleme ve günlük dağıtımını kullanarak en az bir kesinti süresi geçişi gerçekleştirmek için şu adımları izleyin: 

1. Gereksinimlerinize göre Azure VM 'de hedef SQL Server bağlantı kurun. Bkz. [Azure 'da SQL Server sanal makinesine bağlanma (Kaynak Yöneticisi)](../../virtual-machines/windows/ways-to-connect-to-sql.md).
1. Geçirilecek şirket içi kullanıcı veritabanlarının tam veya toplu olarak günlüğe kaydedilmiş kurtarma modelinde olduğundan emin olun.
1. Şirket içi bir konumda tam bir veritabanı yedeklemesi gerçekleştirin ve günlük zincirini korumak için [copy_only](/sql/relational-databases/backup-restore/copy-only-backups-sql-server) anahtar sözcüğünü kullanmak üzere mevcut tüm veritabanı yedeklemeleri işlerini değiştirin.
1. Şirket içi yedekleme dosyanızı Uzak Masaüstü, [Azure Veri Gezgini](/azure/data-explorer/data-explorer-overview)veya [AzCopy komut satırı YARDıMCı programını](../../../storage/common/storage-use-azcopy-v10.md) kullanarak sanal makinenize KOPYALAYıN (>1 TB yedeklemeler önerilir).
1. Azure VM 'de SQL Server tam veritabanı yedeklerini geri yükleyin.
1. Azure VM 'de şirket içi veritabanı ve hedef SQL Server arasındaki [günlük dağıtımını](/sql/database-engine/log-shipping/configure-log-shipping-sql-server) ayarlayın. Önceki adımlarda zaten tamamlanmış olduğundan, veritabanlarını yeniden denetlediğinizden emin olun.
1. Hedef sunucuya **kesin** . 
   1. Geçirilecek veritabanlarını kullanarak uygulamaları duraklatın/durdurun. 
   1. Kullanıcı veritabanlarının [Tek Kullanıcı modu](/sql/relational-databases/databases/set-a-database-to-single-user-mode)kullanılarak etkin olmadığından emin olun. 
   1. Hazırlık sırasında, Azure VM 'de SQL Server hedeflemek için şirket içi veritabanlarının [yükünü devretmek](/sql/database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server) için bir günlük aktarma işlemi gerçekleştirin.



### <a name="migrating-objects-outside-user-databases"></a>Nesneleri kullanıcı veritabanlarının dışına geçirme

Kullanıcı veritabanlarınızın sorunsuz çalışması için gerekli olan ek SQL Server nesneleri geçiş sonrası olabilir. 

Aşağıdaki tabloda, Kullanıcı veritabanlarınızı geçişten önce veya sonra tamamlanmakta olan bir liste bileşeni ve önerilen geçiş yöntemleri verilmiştir: 


| **Özellik** | **Bileşen** | **Geçiş yöntemleri** |
| --- | --- | --- |
| **Veritabanları** | Model  | SQL Server Management Studio betiği |
|| 'Nin | TempDB 'yi en iyi performans için [Azure VM geçici diskine (SSD](../../virtual-machines/windows/performance-guidelines-best-practices.md#temporary-disk)) taşımayı planlayın. TempDB 'nize uyum sağlamak için yeterli yerel SSD 'ye sahip bir VM boyutu seçtiğinizden emin olun. |
|| FILESTREAM ile kullanıcı veritabanları |  Geçiş için [yedekleme ve geri yükleme](../../virtual-machines/windows/migrate-to-vm-from-sql-server.md#back-up-and-restore) yöntemlerini kullanın. DMA, FILESTREAM ile veritabanlarını desteklemez. |
| **Güvenlik** | SQL Server ve Windows oturumu açma | [Kullanıcı oturumlarını geçirmek](/sql/dma/dma-migrateserverlogins)için DMA 'yı kullanın. |
|| SQL Server rolleri | SQL Server Management Studio betiği |
|| Şifreleme sağlayıcıları | [Azure Key Vault hizmeti kullanmak için dönüştürmeyi](../../virtual-machines/windows/azure-key-vault-integration-configure.md)öneririz. Bu yordam, [SQL VM kaynak sağlayıcısını](../../virtual-machines/windows/sql-vm-resource-provider-register.md)kullanır. |
| **Sunucu nesneleri** | Yedekleme cihazları | [Azure Backup hizmetini](../../../backup/backup-sql-server-database-azure-vms.md) kullanarak veritabanı yedekleme ile değiştirin veya yedeklemeleri [Azure Storage](../../virtual-machines/windows/azure-storage-sql-server-backup-restore-use.md) 'a YAZıN (SQL Server 2012 SP1 CU2 uygulamazsanız +). Bu yordam, [SQL VM kaynak sağlayıcısını](../../virtual-machines/windows/sql-vm-resource-provider-register.md)kullanır.|
|| Bağlantılı Sunucular | SQL Server Management Studio betiği. |
|| Sunucu Tetikleyicileri | SQL Server Management Studio betiği. |
| **Çoğaltma** | Yerel yayınlar | SQL Server Management Studio betiği. |
|| Yerel aboneler | SQL Server Management Studio betiği. |
| **Polybase** | Polybase | SQL Server Management Studio betiği. |
| **Yönetim** | Veritabanı Postası | SQL Server Management Studio betiği. |
| **SQL Server Agent** | İşler | SQL Server Management Studio betiği. |
|| Uyarılar | SQL Server Management Studio betiği. |
|| İşleçler | SQL Server Management Studio betiği. |
|| Kullanıldığı | SQL Server Management Studio betiği. |
| **İşletim Sistemi** | Dosyalar, dosya paylaşımları | SQL sunucularınız tarafından kullanılan ve Azure VM hedefinde çoğaltılan ek dosya veya dosya paylaşımlarını bir yere unutmayın. |


## <a name="post-migration"></a>Geçiş sonrası

Geçiş aşamasını başarıyla tamamladıktan sonra, her şeyin mümkün olduğunca sorunsuz ve etkili bir şekilde çalıştığından emin olmak için bir dizi geçiş sonrası görevi izleyin.

### <a name="remediate-applications"></a>Uygulamaları düzelt

Veriler hedef ortama geçirildikten sonra, daha önce kaynağı tüketen tüm uygulamaların hedefi tüketmeye başlaması gerekir. Bu işlem, bazı durumlarda uygulamalarda değişiklik yapılmasını gerektirebilir.

Herhangi bir veritabanını önerilen düzeltmeleri Geçiş Yardımcısı Kullanıcı veritabanına uygulayın. Bunların tutarlılığını sağlamak ve otomasyon için izin vermek üzere komut dosyası kullanılması önerilir.

### <a name="perform-tests"></a>Testleri gerçekleştirme

Veritabanı geçişi için test yaklaşımı aşağıdaki etkinlikleri gerçekleştirmekten oluşur:

1. **Doğrulama testlerini geliştirin.**  Veritabanı geçişlerini test etmek için SQL sorguları kullanın. Hem kaynak hem de hedef veritabanlarında çalıştırılacak doğrulama sorguları oluşturun. Doğrulama sorgularınız tanımladığınız kapsamı kapsamalıdır.
2. **Test ortamını ayarlayın.**  Test ortamı, kaynak veritabanının ve hedef veritabanının bir kopyasını içermelidir. Test ortamını yalıtdığınızdan emin olun.
3. **Doğrulama testlerini çalıştırın.**  Doğrulama testlerini kaynak ve hedefe göre çalıştırın ve sonra sonuçları çözümleyin.
4. **Performans testlerini çalıştırın.**  Kaynak ve hedefte performans testi çalıştırın ve ardından sonuçları çözümleyip karşılaştırın.

> [!TIP]
> Hedef SQL Server performansını değerlendirmeye yardımcı olması için [veritabanı yükseltme deneyimi Yardımcısı (DEA)](/sql/dea/database-experimentation-assistant-overview) kullanın.
>

### <a name="optimize"></a>İyileştirme

Geçiş sonrası aşaması, veri doğruluğu ve bütünlüğü ile ilgili sorunları gidermek için ve iş yüküyle ilgili olası performans sorunlarını ele almak için önemlidir.

Bu sorunlar ve bu sorunları hafifletmek için özel adımlar hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Geçiş sonrası doğrulama ve Iyileştirme Kılavuzu.](/sql/relational-databases/post-migration-validation-and-optimization-guide)
- [Azure SQL sanal makinelerinde performansı ayarlama](../../virtual-machines/windows/performance-guidelines-best-practices.md).
- [Azure maliyet iyileştirme merkezi](https://azure.microsoft.com/overview/cost-optimization/).

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server geçerli hizmetlerin kullanılabilirliğini denetlemek için bkz. [Azure küresel altyapı merkezi](https://azure.microsoft.com/global-infrastructure/services/?regions=all&amp;products=synapse-analytics,virtual-machines,sql-database)

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
