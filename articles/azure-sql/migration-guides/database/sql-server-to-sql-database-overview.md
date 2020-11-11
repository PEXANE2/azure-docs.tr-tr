---
title: SQL veritabanı 'na SQL Server-geçişe genel bakış
description: SQL Server veritabanlarınızı Azure SQL veritabanı 'na geçirmek için kullanabileceğiniz farklı araçlar ve seçenekler hakkında bilgi edinin.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 11/06/2020
ms.openlocfilehash: cafb32e5bd91c6b7f3cfef4641828963e0731797
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94497383"
---
# <a name="migration-overview-sql-server-to-sql-database"></a>Geçişe genel bakış: SQL veritabanı 'na SQL Server
[!INCLUDE[appliesto--sqldb](../../includes/appliesto-sqldb.md)]

SQL Server Azure SQL veritabanı 'na geçirmek için farklı geçiş seçenekleri ve konuları hakkında bilgi edinin. 

Şirket içinde veya üzerinde çalışan SQL Server geçirebilirsiniz: 

- Sanal Makinelerde SQL Server  
- Amazon Web Services (AWS) EC2 
- Amazon Ilişkisel veritabanı hizmeti (AWS RDS) 
- İşlem altyapısı (Google Cloud Platform-GCP)  
- SQL Server için bulut SQL (Google Cloud Platform – GCP) 

Diğer senaryolar için [veritabanı geçiş kılavuzuna](https://datamigration.microsoft.com/)bakın. 

## <a name="overview"></a>Genel Bakış

[Azure SQL veritabanı](../../database/sql-database-paas-overview.md) , tam olarak yönetilen bir hizmet olarak platform (PaaS) gerektiren SQL Server iş yükleri için önerilen bir hedef seçenektir. SQL veritabanı birçok farklı uygulama türüne uyacak şekilde yerleşik yüksek kullanılabilirlik, akıllı sorgu işleme, ölçeklenebilirlik ve performans özellikleri ile birlikte çoğu veritabanı yönetim işlevini işler. 

SQL veritabanı, farklı türlerde uygulamalara veya iş yüklerine sahip birden çok [dağıtım modeli](../../database/sql-database-paas-overview.md#deployment-models) ve [hizmet katmanı](../../database/service-tiers-vcore.md#service-tiers) ile esneklik sağlar.

SQL veritabanına geçiş avantajlarından biri, PaaS özelliklerinden yararlanarak uygulamanızı modernleştirin ve SQL Aracısı işleri gibi örnek düzeyinde kapsamlı olan teknik bileşenlere herhangi bir bağımlılığı ortadan kaldırmanıza olanak sağlar.

Ayrıca, SQL Server şirket içi lisanslarınızı Azure SQL veritabanı 'na taşıyarak, [sanal çekirdek tabanlı satın alma modelini](../../database/service-tiers-vcore.md)seçmeniz gerekir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) SQL Server.

Bu kılavuz, SQL Server veritabanlarınızı Azure SQL veritabanı 'na geçirmeye hazırlanırken geçiş seçeneklerini ve önemli noktaları açıklığa kavuşturmaya yardımcı olacak.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Geçiş seçeneklerini değerlendirirken göz önünde bulundurmanız gereken temel faktörler şuna bağlıdır: 

- Sunucu ve veritabanı sayısı
- Veritabanlarının boyutu
- Geçiş işlemi sırasında kabul edilebilir çalışma süresi 

Bu kılavuzda listelenen geçiş seçenekleri bu faktörleri hesaba götürür. Azure SQL veritabanı 'na mantıksal veri geçişi için, geçiş süresi bir veritabanındaki nesne sayısına ve veritabanının boyutuna bağlı olabilir. 

Farklı iş yükleri ve Kullanıcı tercihleri için farklı araçlar mevcuttur. Bazı araçlar, Kullanıcı arabirimi tabanlı bir araç kullanarak tek bir veritabanının hızlı geçişini gerçekleştirmek için kullanılabilir, ancak diğer araçların ölçekteki geçişleri işlemek için otomatikleştirilen birden çok veritabanını geçirebilirler. 

## <a name="choose-appropriate-target"></a>Uygun hedefi seçin

Azure SQL veritabanı 'nın doğru dağıtım modelini ve hizmet katmanını seçmenize yardımcı olacak genel yönergeleri göz önünde bulundurun. Dağıtım sırasında işlem ve depolama kaynaklarını seçebilir ve sonra uygulamanız için kapalı kalma süresi olmadan  [Azure Portal](../../database/scale-resources.md)  kullanarak bunları daha sonra değiştirebilirsiniz.


**Dağıtım modelleri** : tek bir veritabanı veya elastik havuz arasında karar vermek için uygulama iş yükünüzü ve kullanım modelini anlayın. 

- [Tek bir veritabanı](../../database/single-database-overview.md) , en modern bulut uygulamaları ve mikro hizmetler için uygun olan tam olarak yönetilen bir veritabanını temsil eder.
- [Elastik havuz](../../database/elastic-pool-overview.md) , CPU veya bellek gibi paylaşılan bir kaynak kümesiyle tek bir veritabanı koleksiyonudur ve aynı kaynak kümesini etkin bir şekilde paylaşabilen öngörülebilir kullanım desenleriyle bir havuzdaki veritabanlarını birleştirmek için uygundur.

**Model satın alma** : Vcore, DTU veya sunucusuz satın alma modeli arasında seçim yapın. 

- [Vcore modeli](../../database/service-tiers-vcore.md) , Azure SQL veritabanınızın sanal çekirdekleri sayısını seçmenizi sağlar ve şirket içi SQL Server çevrilirken en kolay seçim yapar. Bu, [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/)ile lisans maliyetinde kaydetmeyi destekleyen tek seçenektir. 
- [DTU modeli](../../database/service-tiers-dtu.md) , KARıŞTıRıLAN bir DTU sağlamak için temeldeki işlem, bellek ve GÇ kaynaklarını soyutlar. 
- [Sunucusuz model](../../database/serverless-tier-overview.md) , kullanım için saniyede faturalandırılan işlem kaynaklarıyla otomatik olarak isteğe bağlı ölçeklendirme gerektiren iş yükleri için tasarlanmıştır. Sunucusuz işlem katmanı, etkin olmayan dönemler (yalnızca depolamanın faturalandırılması) sırasında veritabanlarını otomatik olarak duraklatır ve etkinlik döndüğünde veritabanlarını otomatik olarak sürdürür. 

**Hizmet katmanları** : farklı uygulama türleri için tasarlanan üç hizmet katmanı arasından seçim yapın.

- [Genel amaçlı/standart hizmet katmanı](../../database/service-tier-general-purpose.md) , yoğun bir işlem ve depolama alanı ile, depolama katmanında yerleşik olarak bulunan artıklardan kurtarmak için depolama katmanında bulunan yedeklerle birlikte, dengeli bir bütçe yönelimli seçenek sunar. Çoğu veritabanı iş yükleri için tasarlanmıştır. 
- [İş açısından kritik/Premium hizmet katmanı](../../database/service-tier-business-critical.md) , yüksek işlem hızları, düşük gecikmeli GÇ ve ikincil çoğaltmalarla hem yük devretmede hem de okuma iş yüklerinin yükünü boşaltarak yüksek düzeyde dayanıklılık gerektiren yüksek katmanlı uygulamalardır.
- [Hiper ölçekli hizmet katmanı](../../database/service-tier-hyperscale.md) , büyüyen veri birimlerine sahip veritabanları içindir ve 100 TB 'lık veritabanı boyutunu otomatik olarak ölçeklendirmeniz gerekir. Çok büyük veritabanları için tasarlanmıştır. 

> [!IMPORTANT]
> [İşlem günlüğü oranı,](../../database/resource-limits-logical-server.md#transaction-log-rate-governance) Azure SQL veritabanı 'na göre, yüksek alım tarifelerinin sınırlanmaya tabidir. Bu nedenle, geçiş sırasında, CPU veya işleme hızını kolaylaştırmak için hedef veritabanı kaynaklarının (sanal çekirdekler/DTU 'Lar) ölçeklendirilmesi gerekebilir. Uygun boyutta hedef veritabanını seçin, ancak gerekirse geçiş için kaynakları ölçeklendirmeye planlayın. 


### <a name="sql-server-on-azure-vm-alternative"></a>Azure VM alternatifi üzerinde SQL Server

İşletmeniz Azure [sanal makinelerinde SQL Server](../../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) Azure SQL veritabanı 'ndan daha uygun bir hedef olan gereksinimlere sahip olabilir. 

İşletmeniz için şunlar uygunsa, bunun yerine Azure VM 'de bir SQL Server geçmeyi göz önünde bulundurun: 

- SQL Server ile aynı sanal makineye üçüncü taraf veya özel aracılar yüklemek gibi işletim sistemine veya dosya sistemine doğrudan erişim istiyorsanız. 
- FILESTREAM/FileTable, PolyBase ve platformlar arası işlemler gibi hala desteklenmeyen özelliklerde kesin bağımlılığı varsa. 
- Kesinlikle SQL Server (örneğin, 2012) belirli bir sürümünde kalmanıza gerek duyuyorsanız. 
- İşlem gereksinimleriniz yönetilen örnek tekliflerinden çok daha düşükse (örneğin, bir sanal çekirdek) ve veritabanı birleştirme kabul edilebilir bir seçenek değildir. 


## <a name="migration-tools"></a>Geçiş araçları 

Geçiş için önerilen araçlar Data Migration Yardımcısı ve Azure veritabanı geçiş hizmetidir. Diğer alternatif geçiş seçenekleri de mevcuttur. 

### <a name="recommended-tools"></a>Önerilen araçlar

Aşağıdaki tabloda önerilen geçiş araçları listelenmektedir: 

|Teknoloji | Açıklama|
|---------|---------|
|[Data Migration Yardımcısı (DMA)](/sql/dma/dma-migrateonpremsqltosqldb)|Data Migration Yardımcısı, Azure SQL veritabanı 'na (şema ve veri) yönelik SQL Server ve geçişlere yönelik sorunsuz değerlendirmeler sağlayan bir masaüstü aracıdır. Araç, şirket içi veya kaynak veritabanlarınıza bağlantısı olan yerel makinenizde bir sunucuya yüklenebilir. Geçiş işlemi, kaynak ve hedef veritabanındaki nesneler arasındaki mantıksal bir veri taşıdır. </br> -Tek veritabanlarını geçirme (şema ve veri)|
|[Azure Veritabanı Geçiş Hizmeti (DMS)](../../../dms/tutorial-sql-server-to-azure-sql.md)|PowerShell ile Azure portal veya otomatik kullanarak SQL Server veritabanlarınızı Azure SQL veritabanı 'na geçirebilen birinci taraf bir Azure hizmeti. Azure DMS, kaynak SQL Server veritabanlarınıza bağlantı olduğundan emin olmak için sağlama sırasında tercih edilen bir Azure sanal ağı (VNet) seçmenizi gerektirir. </br> -Tek veritabanlarını veya ölçeklendirmeye geçirin. |
| | |


### <a name="alternative-tools"></a>Alternatif araçlar

Aşağıdaki tabloda alternatif geçiş araçları listelenmektedir: 

|Teknoloji |Açıklama  |
|---------|---------|
|[İşlem çoğaltması](../../database/replication-to-sql-database.md)|İşlem tutarlılığını koruyarak bir yayımcı-abone türü geçiş seçeneği sağlayarak kaynak SQL Server veritabanı tablosundan SQL veritabanına veri çoğaltın. Artımlı veri değişiklikleri, yayımcılar üzerinde gerçekleştikleri sürece abonelere dağıtılır.|
|[İçeri/dışarı aktarma hizmeti/BACPAC](../../database/database-import.md)|[Bacpac](/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) , bir `.bacpac` veritabanının şemasını ve verilerini kapsülleyen bir uzantıya sahip bir Windows dosyasıdır. BACPAC, verileri bir kaynak SQL Server dışa aktarmak ve verileri Azure SQL veritabanı 'na aktarmak için kullanılabilir. BACPAC dosyası Azure portal kullanarak yeni bir Azure SQL veritabanına aktarılabilir. </br></br> Büyük veritabanları boyutları veya çok sayıda veritabanı ile ölçek ve performans için veritabanlarını dışa ve içe aktarmak üzere [SqlPackage](../../database/database-import.md#using-sqlpackage) komut satırı yardımcı programını kullanmayı düşünmelisiniz.|
|[Toplu kopyalama](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)|[Toplu kopyalama programı (bcp) yardımcı programı](/sql/tools/bcp-utility) , verileri bir SQL Server örneğinden bir veri dosyasına kopyalar. Kaynaktaki verileri dışarı aktarmak ve veri dosyasını hedef SQL veritabanına aktarmak için BCP yardımcı programını kullanın. </br></br> Verileri Azure SQL veritabanı 'na taşımak için yüksek hızlı toplu kopyalama işlemleri için [akıllı toplu kopyalama aracı](/samples/azure-samples/smartbulkcopy/smart-bulk-copy/) , paralel kopyalama görevlerinden yararlanarak aktarım hızını en üst düzeye çıkarmak için kullanılabilir.|
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-database.md)|Azure Data Factory [kopyalama etkinliği](../../../data-factory/copy-activity-overview.md) , yerleşik bağlayıcılar ve bir [Integration Runtime](../../../data-factory/concepts-integration-runtime.md)kullanarak kaynak SQL Server veritabanından SQL veritabanı 'na veri geçirir.</br> </br> ADF, verileri SQL Server kaynaklarından SQL veritabanı 'na taşımak için çok çeşitli [bağlayıcıları](../../../data-factory/connector-overview.md) destekler.|
|[SQL Data Sync](../../database/sql-data-sync-data-sql-server-sql-database.md)|SQL Data Sync, hem şirket içinde hem de bulutta birden çok veritabanı üzerinde çift yönlü olarak seçtiğiniz verileri eşitlemenize olanak sağlayan Azure SQL veritabanı üzerinde oluşturulmuş bir hizmettir.</br>Veri eşitleme, verilerin Azure SQL veritabanı veya SQL Server çeşitli veritabanları arasında güncelleştirilmesi gereken durumlarda yararlıdır.|
| | |

## <a name="compare-migration-options"></a>Geçiş seçeneklerini karşılaştırma

Geçiş seçeneklerini, iş gereksinimlerinize uygun yolu seçmek üzere karşılaştırın. 

### <a name="recommended-options"></a>Önerilen seçenekler

Aşağıdaki tabloda önerilen geçiş seçenekleri karşılaştırılmaktadır: 

|Geçiş seçeneği  |Kullanılması gereken durumlar  |Dikkat edilmesi gerekenler  |
|---------|---------|---------|
|[Data Migration Yardımcısı (DMA)](/sql/dma/dma-migrateonpremsqltosqldb) | -Tek veritabanlarını geçirme (şema ve veri).  </br> -Veri geçiş işlemi sırasında kapalı kalma süresine uyum sağlayabilir. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM | -Geçiş etkinliği veritabanı nesneleri arasında (kaynaktan hedefe) veri hareketini gerçekleştirir ve bu nedenle yoğun olmayan saatlerde çalıştırılması önerilir. </br> -DMA geçirilen satır sayısı dahil olmak üzere veritabanı nesnesi başına geçişin durumunu raporlar.  </br> -Büyük geçişler için (veritabanlarının sayısı/veritabanı boyutu), aşağıda listelenen Azure veritabanı geçiş hizmetini kullanın.|
|[Azure Veritabanı Geçiş Hizmeti (DMS)](../../../dms/tutorial-sql-server-to-azure-sql.md)| -Tek veritabanlarını veya ölçeklendirmeye geçirin. </br> -Geçiş işlemi sırasında kapalı kalma süresine uyum sağlayabilir. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM | -Ölçekte geçişler, [PowerShell](../../../dms/howto-sql-server-to-azure-sql-powershell.md)aracılığıyla otomatikleştirilebilir. </br> -Geçişin tamamlanma süresi veritabanı boyutuna ve veritabanındaki nesne sayısına bağlıdır. </br> -Kaynak veritabanının salt okunurdur olarak ayarlanmasını gerektirir. |
| | | |

### <a name="alternative-options"></a>Alternatif seçenekler

Aşağıdaki tabloda alternatif geçiş seçenekleri karşılaştırılmaktadır: 

|Yöntem/teknoloji |Kullanılması gereken durumlar    |Dikkat edilmesi gerekenler  |
|---------|---------|---------|
|[İşlem çoğaltması](../../database/replication-to-sql-database.md)| -Değişiklikleri kaynak veritabanı tablolarından hedef SQL veritabanı tablolarına sürekli olarak yayımlayarak geçiş yapın. </br> -Seçili tabloların (veritabanı alt kümesi) tam veya kısmi veritabanı geçişleri.  </br> </br> Desteklenen kaynaklar: </br> - [SQL Server (2016-2019) bazı sınırlamalar](/sql/relational-databases/replication/replication-backward-compatibility) </br> -AWS EC2  </br> -GCP Işlem SQL Server VM  | -Kurulum diğer geçiş seçeneklerine kıyasla görece karmaşıktır.   </br> -Verileri geçirmek için (veritabanlarını çevrimdışına almadan) sürekli bir çoğaltma seçeneği sağlar.  </br> -İşlemsel çoğaltmanın, kaynak SQL Server yayımcıyı ayarlarken dikkate alınması gereken birkaç sınırlama vardır. Daha fazla bilgi için bkz. [nesneleri yayımlamayla Ilgili sınırlamalar](/sql/relational-databases/replication/publish/publish-data-and-database-objects#limitations-on-publishing-objects) . </br>- [Çoğaltma etkinliğini izlemek](/sql/relational-databases/replication/monitor/monitoring-replication)mümkündür.    |
|[İçeri/dışarı aktarma hizmeti/BACPAC](../../database/database-import.md)| -Bireysel iş kolu uygulama veritabanlarını geçirin. </br>-Daha küçük veritabanları için uygundur.  </br>  -Ayrı bir geçiş hizmeti veya aracı gerektirmez. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM  |  -Verilerin kaynakta aktarılması ve hedefte içeri aktarılması gerektiğinden kesinti olması gerekir.   </br> -Dışarı aktarma/içeri aktarma işleminde kullanılan dosya biçimlerinin ve veri türlerinin, kesme/veri türü uyuşmazlığı hatalarından kaçınmak için tablo şemaları ile tutarlı olması gerekir.  </br> -Çok sayıda nesne içeren bir veritabanını dışarı aktarmak için geçen süre önemli ölçüde daha yüksek olabilir.       |
|[Toplu kopyalama](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| -Tam veya kısmi veri geçişlerini geçirin. </br> -Kapalı kalma süresine uyum sağlayabilir. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM   | -Verileri kaynaktan dışarı aktarmak ve hedefe içeri aktarmak için kapalı kalma süresi gerektirir. </br> -Dışarı aktarma/içeri aktarma işleminde kullanılan dosya biçimlerinin ve veri türlerinin tablo şemaları ile tutarlı olması gerekir. |
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-database.md)| -Kaynak SQL Server veritabanlarından verileri geçirin ve/veya dönüştürme. </br> -Birden çok veri kaynağından verileri, genellikle Iş zekası (BI) iş yükleri için Azure SQL veritabanı 'na birleştirme.  |  -Verileri kaynaktan hedefe taşımak için ADF 'de veri taşıma işlem hatları oluşturulması gerekir.   </br> - [Maliyet](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) önemli bir konudur ve işlem hattı tetikleyicilerine, etkinlik çalışmasına, veri taşıma süresine vb. göre belirlenir. |
|[SQL Data Sync](../../database/sql-data-sync-data-sql-server-sql-database.md)| -Verileri kaynak ve hedef veritabanları arasında eşitler.</br> -İki yönlü bir akışta Azure SQL veritabanı ve şirket içi SQL Server arasında sürekli eşitleme çalıştırmak için uygundur. | -Azure SQL veritabanı, üye veritabanı olarak şirket içi SQL Server veritabanı ile eşitleme için hub veritabanı olmalıdır.</br> -Işlemsel çoğaltmaya kıyasla SQL Data Sync, şirket içi ve Azure SQL veritabanı arasında iki yönlü veri eşitlemesini destekler. </br> -İş yüküne bağlı olarak daha yüksek bir performans etkisi olabilir.|
| | | |

## <a name="feature-interoperability"></a>Özellik birlikte çalışabilirliği 

Diğer SQL Server özelliklerine dayanan iş yüklerini geçirirken ek konular vardır.

#### <a name="sql-server-integration-services"></a>SQL Server Integration Services
Paketleri [Azure Data Factory](../../../data-factory/introduction.md)Azure-SSIS çalışma zamanına yeniden dağıtarak SQL Server INTEGRATION SERVICES (SSIS) paketlerini Azure 'a geçirin. Azure Data Factory, Azure 'da SSIS paketlerini yürütmek için oluşturulmuş bir çalışma zamanı sağlayarak [SSIS paketlerinin geçirilmesini destekler](../../../data-factory/scenario-ssis-migration-overview.md#azure-sql-database-as-database-workload-destination) . Alternatif olarak, SSIS ETL mantığını, [veri akışlarını](../../../data-factory/concepts-data-flow-overview.md)kullanarak ADF 'de yerel olarak da yazabilirsiniz.


#### <a name="sql-server-reporting-services"></a>SQL Server Reporting Services
SQL Server Reporting Services (SSRS) raporlarını Power BI sayfalandırılmış raporlara geçirin. Raporlarınızı hazırlama ve geçirmeye yardımcı olması için [rdl geçiş aracını](https://github.com/microsoft/RdlMigration) kullanın. Bu araç, Microsoft tarafından müşterilerin SSRS sunucularındaki RDL raporlarını Power BI'a geçirmelerine yardımcı olma amacıyla geliştirilmiştir. GitHub 'da kullanılabilir ve geçiş senaryosuna yönelik uçtan uca bir anlatım belgeler. 

#### <a name="high-availability"></a>Yüksek kullanılabilirlik
Her zaman açık yük devretme kümesi örnekleri ve her zaman açık kullanılabilirlik grupları gibi SQL Server yüksek kullanılabilirlik özelliklerinin el ile kurulumu, hedef Azure SQL veritabanı 'nda hem [genel amaçlı (Standart kullanılabilirlik modeli)](../../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-locally-redundant-availability) hem de [iş açısından kritik (Premium kullanılabilirlik modeli)](../../database/high-availability-sla.md#premium-and-business-critical-service-tier-locally-redundant-availability) SQL veritabanı 'nda zaten yerleşik olarak bulunur. İş Açısından Kritik/Premium hizmet katmanı Ayrıca, salt okuma amacıyla ikincil düğümlerden birine bağlanmayı sağlayan okuma ölçeğini de sağlar. 

SQL veritabanı 'nda bulunan yüksek kullanılabilirlik mimarisinin ötesinde, yönetilen bir örnekteki veritabanlarının başka bir bölgeye çoğaltılmasını ve yük devretmesini yönetmenizi sağlayan [otomatik yük devretme grupları](../../database/auto-failover-group-overview.md) özelliği de vardır. 

#### <a name="sql-agent-jobs"></a>SQL Aracısı işleri
SQL Aracısı işleri Azure SQL veritabanı 'nda doğrudan desteklenmez ve [elastik veritabanı işlerine (Önizleme)](../../database/job-automation-overview.md#elastic-database-jobs-preview)dağıtılması gerekir.

#### <a name="logins-and-groups"></a>Oturum açmalar ve gruplar
Veritabanı geçiş hizmeti 'ni (DMS) kullanarak, çevrimdışı modda SQL oturumlarını kaynak SQL Server Azure SQL veritabanı 'na taşıyın.  Oturum açma işlemlerini hedef SQL veritabanınıza geçirmek için **geçiş sihirbazındaki** **Seçili oturum açma** dikey penceresini kullanın. 

Windows kullanıcıları ve grupları, DMS yapılandırma sayfasındaki ilgili iki durumlu düğmeyi etkinleştirerek DMS kullanılarak da geçirilebilir. 

Alternatif olarak, Microsoft veri geçiş mimarları tarafından özel olarak tasarlanan [PowerShell yardımcı programı aracını](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) kullanabilirsiniz. Yardımcı programı, oturum açma bilgilerini yeniden oluşturmak ve kaynaktan hedefe veritabanı kullanıcıları seçmek için bir Transact-SQL (T-SQL) betiği oluşturmak üzere PowerShell kullanır. Araç Windows AD hesaplarını otomatik olarak Azure AD hesaplarıyla eşleştirir ve kaynak Active Directory karşı her oturum açma için bir UPN araması yapabilir. Araç, özel sunucu ve veritabanı rollerinin yanı sıra rol üyeliği, veritabanı rolü ve Kullanıcı izinleri için komut dosyaları verir. Kapsanan veritabanları henüz desteklenmiyor ve yalnızca olası SQL Server izinlerinin bir alt kümesi betikleştirilmiş. 


#### <a name="system-databases"></a>Sistem veritabanları
Azure SQL veritabanı için, yalnızca geçerli sistem veritabanları [ana](/sql/relational-databases/databases/master-database) ve tempdb ' dir. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda tempdb](/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).

## <a name="leverage-advanced-features"></a>Gelişmiş özelliklerden yararlanın 

SQL veritabanı tarafından sunulan gelişmiş bulut tabanlı özelliklerden faydalandığınızdan emin olun. Örneğin, hizmet sizin için yaptığı için yedeklemeleri yönetme konusunda endişelenmenize gerek kalmaz. [Bekletme dönemi içinde](../../database/recovery-using-backups.md#point-in-time-restore)herhangi bir noktaya geri yükleme yapabilirsiniz. 

Güvenliği güçlendirmek için [Azure Active Directory kimlik doğrulaması](../../database/authentication-aad-overview.md), [Denetim](../../database/auditing-overview.md), [tehdit algılama](../../database/advanced-data-security.md), [satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security)ve [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)kullanmayı göz önünde bulundurun.

Gelişmiş Yönetim ve güvenlik özelliklerine ek olarak SQL veritabanı, [iş yükünüzü izlemenize ve ayarlamanıza](../../database/monitor-tune-overview.md)yardımcı olabilecek bir dizi gelişmiş araç sağlar. [Azure SQL Analytics (Önizleme)](../../../azure-monitor/insights/azure-sql.md) , tek bir GÖRÜNÜMDEKI Azure SQL veritabanı 'ndaki tüm veritabanlarınızın ve birden çok aboneliğin performansını izlemeye yönelik gelişmiş bir bulut izleme çözümüdür. Azure SQL Analytics performans sorunlarını gidermek için yerleşik zeka ile önemli performans ölçümlerini toplayıp görselleştirir.

[Otomatik ayarlama](/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction)   , SQL yürütme planı istatistiklerinizin performansını sürekli olarak izler ve belirlenen performans sorunlarını otomatik olarak düzeltir. 


## <a name="migration-assets"></a>Geçiş varlıkları 

Ek Yardım için, gerçek dünya geçiş projeleri için geliştirilen aşağıdaki kaynaklara bakın.

|Varlık  |Açıklama  |
|---------|---------|
|[Veri iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Otomatikleştirilmiş ve Tekdüzen hedef platformu karar süreci sağlayarak büyük Emlak değerlendirmelerine yardımcı olan basit, tek tıklamayla hesaplama ve rapor oluşturma işlemlerini sunar.|
|[DBLoader yardımcı programı](https://github.com/microsoft/DataMigrationTeam/tree/master/DBLoader%20Utility)|DBLoader, sınırlandırılmış metin dosyalarından SQL Server içine veri yüklemek için kullanılabilir. Bu Windows konsol yardımcı programı, Azure SQL veritabanı da dahil olmak üzere tüm SQL Server sürümleri üzerinde çalışan SQL Server Native Client BulkLoad arabirimini kullanır.|
|[PowerShell ile toplu veritabanı oluşturma](https://github.com/Microsoft/DataMigrationTeam/tree/master/Bulk%20Database%20Creation%20with%20PowerShell)|Bu, bir kaynak grubu (create_rg.ps1), [Azure 'daki mantıksal sunucu](../../database/logical-servers.md) (create_sqlserver.ps1) ve Azure SQL veritabanı (create_sqldb.ps1) oluşturan üç PowerShell betikleri kümesini içerir. Betikler, gereken sayıda sunucu ve veritabanı oluşturmak için döngü özelliklerini içerir.|
|[MSSQL-Scripter & PowerShell ile toplu şema dağıtımı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Bulk%20Schema%20Deployment%20with%20MSSQL-Scripter%20&%20PowerShell)|Bu varlık Azure SQL veritabanı 'nı barındırmak için Azure 'da bir kaynak grubu, bir veya birden çok [mantıksal sunucu](../../database/logical-servers.md) oluşturur, her şemayı şirket içi SQL Server (veya bırden çok SQL Server (2005 +) dışa aktarır ve Azure SQL veritabanı 'na aktarır.|
|[SQL Server Agent işleri elastik veritabanı Işlerine dönüştürme](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Convert%20SQL%20Server%20Agent%20Jobs%20into%20Elastic%20Database%20Jobs)|Bu betik, kaynak SQL Server Agent işlerinizi elastik veritabanı Işlerine geçirir.|
|[Azure SQL veritabanından posta gönder](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/AF%20SendMail)|Bu, şirket içi SQL Server sağlanan SendMail özelliğine alternatif olarak bir çözüm sağlar. Çözüm, Azure SQL veritabanından e-posta göndermek için Azure Işlevleri 'ni ve Azure SendGrid hizmetini kullanır.|
|[Şirket içi SQL Server oturumlarını Azure SQL veritabanı 'na taşımaya yardımcı program](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins)|Oturum açmaları yeniden oluşturmak ve şirket içi SQL Server Azure SQL veritabanı 'na veritabanı kullanıcılarını seçmek için bir T-SQL komut betiği oluşturan bir PowerShell betiği. Araç Windows AD hesaplarının otomatik olarak Azure AD hesaplarına eşlenmesini ve isteğe bağlı olarak SQL Server yerel oturum açma işlemleri geçirilmesini sağlar.|
|[Logman kullanarak PerfMon veri toplama Otomasyonu](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|Temel performansı anlamak için PerMon verilerini toplayan ve geçiş hedefi önerilerine yardım eden bir araç. Bu araç, uzak bir SQL Server ayarlanan performans sayaçlarını oluşturacak, başlatacak, durduracak ve silecektir komutunu oluşturmak için logman.exe kullanır|
|[Teknik İnceleme-BACPAC kullanarak Azure SQL DB 'ye veritabanı geçişi](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Database%20migrations%20-%20Benchmarks%20and%20Steps%20to%20Import%20to%20Azure%20SQL%20DB%20Single%20Database%20from%20BACPAC.pdf)|Bu Teknik İnceleme, BACPAC dosyalarını kullanarak SQL Server geçişleri Azure SQL veritabanı 'na hızlandırmaya yardımcı olacak rehberlik ve adımlar sağlar.|

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.


## <a name="next-steps"></a>Sonraki adımlar

SQL Server Azure SQL veritabanı 'na geçirmeye başlamak için [SQL Server SQL veritabanı geçiş kılavuzuna](sql-server-to-sql-database-guide.md)bakın.

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için sunulan Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için bkz. [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md).

- SQL veritabanı hakkında daha fazla bilgi için bkz.
   - [Azure SQL veritabanı 'na genel bakış](../../database/sql-database-paas-overview.md)
   - [Azure toplam sahiplik Hesaplayıcı maliyeti](https://azure.microsoft.com/pricing/tco/calculator/) 

- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 


- Uygulama erişim katmanını değerlendirmek için bkz. [veri erişimi geçiş araç seti (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Veri erişim katmanını bir/B testi gerçekleştirme hakkında daha fazla bilgi için bkz. [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview).
