---
title: "Azure SQL yönetilen örneği 'ne SQL Server: geçişe genel bakış"
description: SQL Server veritabanlarınızı Azure SQL yönetilen örneği 'ne geçirebileceğiniz farklı araçlar ve seçenekler hakkında bilgi edinin.
ms.service: sql-managed-instance
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: mokabiru
ms.author: mokabiru
ms.reviewer: MashaMSFT
ms.date: 02/18/2020
ms.openlocfilehash: 1c187ae83ce87c9d4d8da4aa1a5dc38163261b52
ms.sourcegitcommit: a8ff4f9f69332eef9c75093fd56a9aae2fe65122
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105024906"
---
# <a name="migration-overview-sql-server-to-azure-sql-managed-instance"></a>Geçişe genel bakış: Azure SQL yönetilen örneği 'ne SQL Server
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlmi.md)]

SQL Server Azure SQL yönetilen örneği 'ne geçirmeye yönelik farklı geçiş seçenekleri ve konuları hakkında bilgi edinin. 

Şirket içinde veya üzerinde çalışan SQL Server geçirebilirsiniz: 

- Sanal Makinelerde SQL Server  
- Amazon Web Services (AWS) EC2 
- Amazon Ilişkisel veritabanı hizmeti (AWS RDS) 
- İşlem altyapısı (Google Cloud Platform-GCP)  
- SQL Server için bulut SQL (Google Cloud Platform – GCP) 

Diğer geçiş kılavuzlarında, bkz. [Veritabanı geçişi](https://docs.microsoft.com/data-migration). 

## <a name="overview"></a>Genel Bakış

[Azure SQL yönetilen örneği](../../managed-instance/sql-managed-instance-paas-overview.md) , sanal makineleri veya işletim sistemlerini yönetmek zorunda kalmadan tam olarak yönetilen bir hizmet gerektiren SQL Server iş yükleri için önerilen bir hedef seçenektir. SQL yönetilen örneği, yerel sanal ağ (VNet) desteğiyle örneklerinizi tamamen yalıtımıyla, en az uygulama veya veritabanı değişikliğine sahip şirket içi uygulamalarınızı Azure 'a eklemenize ve kaydırabilmenizi sağlar. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler 

Geçiş seçeneklerini değerlendirirken göz önünde bulundurmanız gereken temel faktörler şuna bağlıdır: 
- Sunucu ve veritabanı sayısı
- Veritabanlarının boyutu
- Geçiş işlemi sırasında kabul edilebilir çalışma süresi 

SQL Server 'larınızı SQL yönetilen örneği 'ne geçirmenin önemli avantajlarından biri, örneğin tamamını veya tek veritabanlarının yalnızca bir alt kümesini geçirmeyi tercih edebilirsiniz. Geçiş sürecinize aşağıdakileri eklemeyi dikkatle planlayın: 
- Aynı örneğe eklenmesi gereken tüm veritabanları 
- Uygulamanız için gereken örnek düzeyi nesneler, oturum açmalar, kimlik bilgileri, SQL Agent işleri ve işleçleri ve sunucu düzeyi Tetikleyiciler. 

> [!NOTE]
> Azure SQL yönetilen örneği kritik senaryolarda bile% 99,99 kullanılabilirlik sağlar, bu nedenle SQL MI içindeki bazı özelliklerden kaynaklanan ek yük devre dışı bırakılamaz. Daha fazla bilgi için, [SQL Server ve Azure SQL yönetilen örneği blogundan farklı performansa neden olabilecek kök nedenleri](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) bölümüne bakın. 


## <a name="choose-appropriate-target"></a>Uygun hedefi seçin

[Performans temeinizle](sql-server-to-managed-instance-performance-baseline.md)eşleşecek yardımcı olması Için SQL yönetilen örneği 'nin doğru hizmet katmanını ve özelliklerini seçmenize yardımcı olacak bazı genel yönergeler: 

- SQL Server örneğinizin kullandığı çekirdek sayısıyla eşleşen bir yönetilen örnek sağlamak için CPU kullanımı temelini kullanın. [Donanım oluşturma özellikleriyle](../../managed-instance/resource-limits.md#hardware-generation-characteristics)eşleşecek şekilde kaynakları ölçeklendirmek gerekebilir. 
- Bellek ayırmadan uygun bir şekilde eşleşen bir [Vcore seçeneği](../../managed-instance/resource-limits.md#service-tier-characteristics) seçmek için bellek kullanımı temelini kullanın. 
- Dosya alt sisteminin ana hat GÇ gecikmesini kullanarak Genel Amaçlı (5 MS 'den fazla gecikme) ve İş Açısından Kritik (3 MS 'den az gecikme) hizmet katmanlarından birini seçin. 
- Beklenen GÇ performansına ulaşmak için verilerin ve günlük dosyalarının boyutunu önceden ayırmak üzere temel aktarım hızını kullanın. 

Dağıtım sırasında işlem ve depolama kaynakları ' nı seçebilir ve sonra uygulamanız için kapalı kalma süresi olmadan [Azure Portal kullanarak bunları değiştirebilirsiniz](../../database/scale-resources.md) . 

> [!IMPORTANT]
> [Yönetilen örnek sanal ağ gereksinimlerindeki](../../managed-instance/connectivity-architecture-overview.md#network-requirements) herhangi bir tutarsızlık, yeni örnekler oluşturmanızı veya var olanları kullanmanızı engelleyebilir.  [Yeni oluşturma](../../managed-instance/virtual-network-subnet-create-arm-template.md)   ve [Mevcut ağları yapılandırma](../../managed-instance/vnet-existing-add-subnet.md)hakkında daha fazla bilgi edinin   . 

Azure SQL yönetilen örneği 'nde (Genel Amaçlı vs İş Açısından Kritik) hedef hizmet katmanının seçiminde başka bir anahtar, yalnızca İş Açısından Kritik katmanında bulunan In-Memory OLTP gibi bazı özelliklerin kullanılabilirliğinden oluşur. 

### <a name="sql-server-vm-alternative"></a>SQL Server VM alternatifi

İşletmeniz Azure VM 'lerde SQL Server Azure SQL yönetilen örneğinden daha uygun bir hedef haline getirmek için gerekli gereksinimlere sahip olabilir. 

İşletmeniz için şunlar uygunsa, bunun yerine bir SQL Server VM geçmeyi göz önünde bulundurun: 

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
| [Azure Geçişi](/azure/migrate/how-to-create-azure-sql-assessment) | Azure SQL için Azure geçişi, VMware 'de SQL verilerinizi keşfetmenize ve değerlendirmenize, Azure SQL dağıtım önerileri, hedef boyutlandırma ve aylık tahminler sağlar. | 
|[Azure Veritabanı Geçiş Hizmeti (DMS)](../../../dms/tutorial-sql-server-to-managed-instance.md)  | Geçiş işlemi sırasında kesinti süresini karşılayabilen uygulamalar için çevrimdışı modda geçişi destekleyen ilk taraf Azure hizmeti. Çevrimiçi modda sürekli geçişin aksine, çevrimdışı mod geçişi kaynaktan hedefe tam bir veritabanı yedeklemesi için tek seferlik bir geri yükleme çalıştırır. | 
|[Yerel yedekleme ve geri yükleme](../../managed-instance/restore-sample-database-quickstart.md) | SQL yönetilen örneği, yerel SQL Server veritabanı yedeklerinin (. bak dosyaları) geri yüklenmesini destekler, böylece Azure depolama 'ya tam veritabanı yedeklemeleri sağlayabilen müşteriler için en kolay geçiş seçeneği sağlanır. Tam ve fark yedeklemeleri, bu makalenin ilerleyen bölümlerinde yer alarak [geçiş varlıkları bölümünde](#migration-assets) de desteklenir ve belgelenmiştir.| 
|[Günlük yeniden yürütme hizmeti (LRS)](../../managed-instance/log-replay-service-migrate.md) | Bu, Azure depolama 'ya tam, değişiklik ve günlük veritabanı yedeklemeleri sağlayabilen müşteriler için bir geçiş seçeneği sunarak, SQL Server günlük aktarma teknolojisine bağlı olarak yönetilen örnek için etkinleştirilmiş bir bulut hizmetidir. LRS, yedekleme dosyalarını Azure Blob depolamadan SQL yönetilen örneğine geri yüklemek için kullanılır.| 
| | |

### <a name="alternative-tools"></a>Alternatif araçlar

Aşağıdaki tabloda alternatif geçiş araçları listelenmektedir: 

|**Teknoloji** |**Açıklama**  |
|---------|---------|
|[İşlem çoğaltması](../../managed-instance/replication-transactional-overview.md) | İşlem tutarlılığını koruyarak bir yayımcı-abone türü geçiş seçeneği sunarak, kaynak SQL Server veritabanı tabloları ile SQL yönetilen örneği arasında veri çoğaltın. | 
|[Toplu kopyalama](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| [Toplu kopyalama programı (bcp) yardımcı programı](/sql/tools/bcp-utility) , verileri bir SQL Server örneğinden bir veri dosyasına kopyalar. Kaynaktaki verileri dışarı aktarmak ve veri dosyasını hedef SQL yönetilen örneğine aktarmak için BCP yardımcı programını kullanın.</br></br> Verileri Azure SQL veritabanına taşımaya yönelik yüksek hızlı toplu kopyalama işlemleri için [akıllı toplu kopyalama aracı](/samples/azure-samples/smartbulkcopy/smart-bulk-copy/) , paralel kopyalama görevlerinden yararlanarak aktarım hızlarını en üst düzeye çıkarmak için kullanılabilir. | 
|[İçeri aktarma Verme Sihirbazı/BACPAC](../../database/database-import.md?tabs=azure-powershell)| [Bacpac](/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac) , bir `.bacpac` veritabanının şemasını ve verilerini kapsülleyen bir uzantıya sahip bir Windows dosyasıdır. BACPAC, kaynak SQL Server verileri dışarı aktarmak ve dosyayı Azure SQL yönetilen örneği 'ne geri aktarmak için kullanılabilir.  |  
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-managed-instance.md)| Azure Data Factory [kopyalama etkinliği](../../../data-factory/copy-activity-overview.md) , yerleşik bağlayıcılar ve bir [Integration Runtime](../../../data-factory/concepts-integration-runtime.md)kullanarak VERILERI kaynak SQL Server veritabanından SQL yönetilen örneğine geçirir.</br> </br> ADF, verileri SQL Server kaynaklarından SQL yönetilen örneğine taşımak için çok sayıda [bağlayıcı](../../../data-factory/connector-overview.md) destekler. |
| | |

## <a name="compare-migration-options"></a>Geçiş seçeneklerini karşılaştırma

Geçiş seçeneklerini, iş gereksinimlerinize uygun yolu seçmek üzere karşılaştırın. 

### <a name="recommended-options"></a>Önerilen seçenekler

Aşağıdaki tabloda önerilen geçiş seçenekleri karşılaştırılmaktadır: 

|Geçiş seçeneği  |Kullanılması gereken durumlar  |Dikkat edilmesi gerekenler  |
|---------|---------|---------|
|[Azure Veritabanı Geçiş Hizmeti (DMS)](../../../dms/tutorial-sql-server-to-managed-instance.md) | -Tek veritabanlarını veya birden çok veritabanını ölçeklendirmeye geçirin. </br> -Geçiş işlemi sırasında kapalı kalma süresine uyum sağlayabilir. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM |  -Ölçekte geçişler, [PowerShell](../../../dms/howto-sql-server-to-azure-sql-managed-instance-powershell-offline.md)aracılığıyla otomatikleştirilebilir. </br> -Geçişin tamamlanma süresi veritabanı boyutuna bağımlıdır ve yedekleme ve geri yükleme zamanından etkilenir. </br> -Yeterli kapalı kalma süresi gerekebilir. |
|[Yerel yedekleme ve geri yükleme](../../managed-instance/restore-sample-database-quickstart.md) | -Bireysel iş kolu uygulama veritabanlarını geçirin.  </br> -Ayrı bir geçiş hizmeti veya aracı olmadan hızlı ve kolay geçiş.  </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM | -Veritabanı yedeklemesi, Azure Blob depolama alanına veri aktarımını iyileştirmek için birden çok iş parçacığı kullanır, ancak ISV bant genişliği ve veritabanı boyutu aktarım hızını etkileyebilir. </br> -Kapalı kalma süresi, tam yedekleme ve geri yükleme (veri boyutu) için gereken süreyi içermelidir.| 
|[Günlük yeniden yürütme hizmeti (LRS)](../../managed-instance/log-replay-service-migrate.md) | -Bireysel iş kolu uygulama veritabanlarını geçirin.  </br> -Veritabanı geçişleri için daha fazla denetim gereklidir.  </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2008-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM | -Geçiş, SQL Server üzerinde tam veritabanı yedeklemeleri yapmayı ve yedekleme dosyalarını Azure Blob depolamaya kopyalamayı gerektirir. LRS, yedekleme dosyalarını Azure Blob depolamadan SQL yönetilen örneğine geri yüklemek için kullanılır. </br> -Geçiş işlemi sırasında geri yüklenen veritabanları geri yükleme modunda olur ve işlem tamamlanana kadar okumak veya yazmak için kullanılamaz.| 
| | | |

### <a name="alternative-options"></a>Alternatif seçenekler

Aşağıdaki tabloda alternatif geçiş seçenekleri karşılaştırılmaktadır: 

|Yöntem/teknoloji |Kullanılması gereken durumlar |Dikkat edilmesi gerekenler  |
|---------|---------|---------|
|[İşlem çoğaltması](../../managed-instance/replication-transactional-overview.md) | -Değişiklikleri kaynak veritabanı tablolarından hedef SQL yönetilen örnek veritabanı tablolarına sürekli olarak yayımlayarak geçirin. </br> -Seçili tabloların (veritabanı alt kümesi) tam veya kısmi veritabanı geçişleri.  </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2012-2019) bazı sınırlamalar </br> -AWS EC2  </br> -GCP Işlem SQL Server VM | </br> -Kurulum diğer geçiş seçeneklerine kıyasla görece karmaşıktır.   </br> -Verileri geçirmek için (veritabanlarını çevrimdışına almadan) sürekli bir çoğaltma seçeneği sağlar.</br> -İşlemsel çoğaltmanın, kaynak SQL Server yayımcıyı ayarlarken dikkate alınması gereken birkaç sınırlama vardır. Daha fazla bilgi için bkz. [nesneleri yayımlamayla Ilgili sınırlamalar](/sql/relational-databases/replication/publish/publish-data-and-database-objects#limitations-on-publishing-objects) .  </br> - [Çoğaltma etkinliğini izleme](/sql/relational-databases/replication/monitor/monitoring-replication) özelliği kullanılabilir.    |
|[Toplu kopyalama](/sql/relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server)| -Tam veya kısmi veri geçişleri geçiriliyor. </br> -Kapalı kalma süresine uyum sağlayabilir. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM   | -Verileri kaynaktan dışarı aktarmak ve hedefe içeri aktarmak için kapalı kalma süresi gerektirir. </br> -Dışarı aktarma/içeri aktarma işleminde kullanılan dosya biçimlerinin ve veri türlerinin tablo şemaları ile tutarlı olması gerekir. |
|[İçeri aktarma Verme Sihirbazı/BACPAC](../../database/database-import.md)| -Bireysel iş kolu uygulama veritabanlarını geçirin. </br>-Daha küçük veritabanları için uygundur.  </br>  Ayrı bir geçiş hizmeti veya aracı gerektirmez. </br> </br> Desteklenen kaynaklar: </br> -SQL Server (2005-2019) Şirket içi veya Azure VM </br> -AWS EC2 </br> -AWS RDS </br> -GCP Işlem SQL Server VM  |   </br> -Verilerin kaynakta aktarılması ve hedefte içeri aktarılması gerektiğinden kesinti olması gerekir.   </br> -Dışarı aktarma/içeri aktarma işleminde kullanılan dosya biçimlerinin ve veri türlerinin, kesme/veri türü uyuşmazlığı hatalarından kaçınmak için tablo şemaları ile tutarlı olması gerekir. </br> -Çok sayıda nesne içeren bir veritabanını dışarı aktarmak için geçen süre önemli ölçüde daha yüksek olabilir. |
|[Azure Data Factory (ADF)](../../../data-factory/connector-azure-sql-managed-instance.md)| -Kaynak SQL Server veritabanından veri geçiriliyor ve/veya dönüştürülüyor.</br> -Birden çok veri kaynağından verileri, genellikle Iş zekası (BI) iş yükleri için Azure SQL yönetilen örneği ile birleştirme.   </br> -Verileri kaynaktan hedefe taşımak için ADF 'de veri taşıma işlem hatları oluşturulması gerekir.   </br> - [Maliyet](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) önemli bir konudur ve işlem hattı tetikleyicilerine, etkinlik çalışmasına, veri taşıma süresine vb. göre belirlenir. |
| | | |

## <a name="feature-interoperability"></a>Özellik birlikte çalışabilirliği 

Diğer SQL Server özelliklerine dayanan iş yüklerini geçirirken ek konular vardır. 

#### <a name="sql-server-integration-services"></a>SQL Server Integration Services

SSSıSDB 'deki SQL Server Integration Services (SSIS) paketlerini ve projelerini [Azure veritabanı geçiş hizmeti 'ni (DMS)](../../../dms/how-to-migrate-ssis-packages-managed-instance.md)kullanarak Azure SQL yönetilen örneği 'ne geçirin. 

Yalnızca SSSıSDB içindeki SSIS paketleri SQL Server 2012 ile başlayarak geçiş için desteklenir. Geçişten önce eski SSIS paketlerini dönüştürün. Daha fazla bilgi için bkz. [Proje dönüştürme öğreticisi](/sql/integration-services/lesson-6-2-converting-the-project-to-the-project-deployment-model) . 


#### <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

SQL Server Reporting Services (SSRS) raporları, Power BI sayfalandırılmış raporlara geçirilebilir. Raporlarınızı hazırlama ve geçirmeye yardımcı olması için [rdl geçiş aracını](https://github.com/microsoft/RdlMigration) kullanın. Bu araç, Microsoft tarafından müşterilerin SSRS sunucularındaki RDL raporlarını Power BI'a geçirmelerine yardımcı olma amacıyla geliştirilmiştir. GitHub 'da kullanılabilir ve geçiş senaryosuna yönelik uçtan uca bir anlatım belgeler. 

#### <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

SQL Server 2012 ve üzeri SQL Server Analysis Services tablosal modeller, Azure 'da Analysis Services tablolu model için bir PaaS dağıtım modeli olan Azure Analysis Services 'e geçirilebilir. Bu [video öğreticisindeki](https://azure.microsoft.com/resources/videos/azure-analysis-services-moving-models/)Azure Analysis Services şirket içi modelleri geçirme hakkında daha fazla bilgi edinebilirsiniz.

Alternatif olarak, [yenı XMLA okuma/yazma uç noktalarını kullanarak](/power-bi/admin/service-premium-connect-tools)şirket Içi Analysis Services tablolu modellerinizi Power BI Premium taşımaya de göz önüne alabilirsiniz. 
> [!NOTE]
> Power BI XMLA okuma/yazma uç noktaları işlevselliği Şu anda genel önizleme aşamasındadır ve işlevler genel kullanıma sunuluncaya kadar üretim iş yükleri için düşünülmemelidir.

#### <a name="high-availability"></a>Yüksek kullanılabilirlik

Yüksek kullanılabilirlik mimarisi her zaman yük devretme kümesi örneklerinde ve her zaman açık kullanılabilirlik gruplarında SQL Server yüksek kullanılabilirlik mimarisi, hem [genel amaçlı (Standart kullanılabilirlik modeli)](../../database/high-availability-sla.md#basic-standard-and-general-purpose-service-tier-locally-redundant-availability) hem de [iş açısından kritik (Premium kullanılabilirlik modeli)](../../database/high-availability-sla.md#premium-and-business-critical-service-tier-locally-redundant-availability) SQL yönetilen örneği 'nde zaten yerleşik olarak bulunur. Premium kullanılabilirlik modeli Ayrıca, salt okuma amacıyla ikincil düğümlerden birine bağlanmayı sağlayan okuma ölçeğini de sağlar.     

SQL yönetilen örneği 'ne dahil olan yüksek oranda kullanılabilir mimarinin ötesinde, yönetilen bir örnekteki veritabanlarının başka bir bölgeye çoğaltılmasını ve yük devretmesini yönetmenizi sağlayan [otomatik yük devretme grupları](../../database/auto-failover-group-overview.md) özelliği de vardır. 

#### <a name="sql-agent-jobs"></a>SQL Aracısı işleri

[SQL Agent işlerini](../../../dms/howto-sql-server-to-azure-sql-managed-instance-powershell-offline.md)geçirmek Için çevrimdışı Azure veritabanı geçiş HIZMETI (DMS) seçeneğini kullanın. Aksi takdirde, SQL Server Management Studio kullanarak Transact-SQL (T-SQL) içindeki işleri betikten sonra hedef SQL yönetilen örneği üzerinde el ile yeniden oluşturun. 

> [!IMPORTANT]
> Şu anda Azure DMS yalnızca T-SQL alt sistemi adımlarıyla işleri destekliyor. SSIS paketi adımlarının bulunduğu işlerin el ile geçirilmesi gerekecektir. 

#### <a name="logins-and-groups"></a>Oturum açmalar ve gruplar

Kaynak SQL Server SQL oturum açmaları, çevrimdışı modda veritabanı geçiş hizmeti (DMS) kullanılarak Azure SQL yönetilen örneği 'ne taşınabilir.  Oturum açma işlemlerini hedef SQL yönetilen örneğinize geçirmek için **Geçiş Sihirbazı** 'nda **[oturum açma Seç](../../../dms/tutorial-sql-server-to-managed-instance.md#select-logins)** dikey penceresini kullanın. 

Varsayılan olarak, Azure veritabanı geçiş hizmeti yalnızca SQL oturum açmaları geçirmeyi destekler. Ancak, Windows oturumlarını şu şekilde geçirebilme özelliğini etkinleştirebilirsiniz:

Hedef SQL yönetilen örneğinin, **genel yönetici** rolüne sahip bir kullanıcı tarafından Azure Portal aracılığıyla YAPıLANDıRıLABILEN Azure AD okuma erişimi olduğundan emin olun.
Yapılandırma sayfasında Azure portal aracılığıyla ayarlanan Windows Kullanıcı/Grup oturum açma geçişlerini etkinleştirmek için Azure veritabanı geçiş hizmeti örneğinizi yapılandırma. Bu ayarı etkinleştirdikten sonra değişikliklerin etkili olması için hizmeti yeniden başlatın.

Hizmeti yeniden başlattıktan sonra, Windows Kullanıcı/Grup oturum açmalar geçiş için kullanılabilen oturum açma listesinde görünür. Geçiş yaptığınız herhangi bir Windows Kullanıcı/Grup oturumu açma için, sizden ilişkili etki alanı adını girmeniz istenir. Hizmet Kullanıcı hesapları (etki alanı adı NT YETKILISI olan hesap) ve Sanal Kullanıcı hesapları (etki alanı adı NT HIZMETI olan hesap adı) desteklenmez.

Daha fazla bilgi edinmek için bkz. [T-SQL kullanarak bir SQL Server örneğindeki Windows kullanıcılarını ve grupları Azure SQL yönetilen örneği 'ne geçirme](../../managed-instance/migrate-sql-server-users-to-instance-transact-sql-tsql-tutorial.md).

Alternatif olarak, Microsoft veri geçiş mimarları tarafından özel olarak tasarlanan [PowerShell yardımcı programı aracını](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins) kullanabilirsiniz. Yardımcı programı, oturum açma işlemlerini yeniden oluşturmak ve kaynaktan hedefe veritabanı kullanıcıları seçmek için bir T-SQL betiği oluşturmak üzere PowerShell kullanır. Araç Windows AD hesaplarını otomatik olarak Azure AD hesaplarıyla eşleştirir ve kaynak Active Directory karşı her oturum açma için bir UPN araması yapabilir. Araç, özel sunucu ve veritabanı rollerinin yanı sıra rol üyeliği, veritabanı rolü ve Kullanıcı izinleri için komut dosyaları verir. Kapsanan veritabanları Şu anda desteklenmiyor ve yalnızca olası SQL Server izinlerinin bir alt kümesi betikleştirilmiş. 

#### <a name="encryption"></a>Şifreleme

 [Saydam veri şifrelemesi](../../database/transparent-data-encryption-tde-overview.md)tarafından korunan veritabanlarını   yerel geri yükleme seçeneği kullanılarak yönetilen bir örneğe geçirirken, veritabanı geri *yüklemeden önce* [KARŞıLıK gelen sertifikayı](../../managed-instance/tde-certificate-migrate.md) kaynak SQL Server hedef SQL yönetilen örneğine geçirin. 

#### <a name="system-databases"></a>Sistem veritabanları

Sistem veritabanlarının geri yüklenmesi desteklenmiyor. Örnek düzeyi nesneleri (ana veya msdb veritabanlarında depolanır) geçirmek için Transact-SQL (T-SQL) kullanarak bunları betiği ve ardından bunları hedef yönetilen örnekte yeniden oluşturun. 

#### <a name="in-memory-oltp-memory-optimized-tables"></a>In-Memory OLTP (bellek için iyileştirilmiş tablolar)

SQL Server, yüksek aktarım hızı ve düşük gecikmeli işlem işleme gereksinimlerine sahip iş yüklerini çalıştırmak için bellek için iyileştirilmiş tabloların, bellek için iyileştirilmiş tablo türlerinin ve yerel koda derlenmiş SQL modüllerinin kullanılmasına izin veren In-Memory OLTP yeteneği sağlar. 

> [!IMPORTANT]
> In-Memory OLTP yalnızca Azure SQL yönetilen örneğindeki İş Açısından Kritik katmanında desteklenir (Genel Amaçlı katmanında desteklenmez).

Şirket içi SQL Server bellek için iyileştirilmiş tablolar veya bellek için iyileştirilmiş tablo türleriniz varsa ve Azure SQL yönetilen örneği 'ne geçiş yapmak istiyorsanız şunlardan birini yapmalısınız:

- In-Memory OLTP 'yi destekleyen hedef Azure SQL yönetilen örneğiniz için İş Açısından Kritik katmanını seçin veya
- Azure SQL yönetilen örneği 'nde Genel Amaçlı katmanına geçiş yapmak istiyorsanız, bellek için iyileştirilmiş tabloları, bellek için iyileştirilmiş tablo türlerini ve veritabanınızı geçirmeden önce bellek için iyileştirilmiş nesnelerle etkileşime geçen yerel koda derlenmiş SQL modüllerini kaldırın. Aşağıdaki T-SQL sorgusu, Genel Amaçlı katmana geçişten önce kaldırılması gereken tüm nesneleri tanımlamak için kullanılabilir:

```tsql
SELECT * FROM sys.tables WHERE is_memory_optimized=1
SELECT * FROM sys.table_types WHERE is_memory_optimized=1
SELECT * FROM sys.sql_modules WHERE uses_native_compilation=1
```

Bellek içi teknolojiler hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde bellek içi teknolojileri kullanarak performansı iyileştirme](https://docs.microsoft.com/azure/azure-sql/in-memory-oltp-overview)

## <a name="leverage-advanced-features"></a>Gelişmiş özelliklerden yararlanın 

SQL yönetilen örneği tarafından sunulan gelişmiş bulut tabanlı özelliklerden faydalandığınızdan emin olun. Örneğin, hizmet sizin için yaptığı için yedeklemeleri yönetme konusunda endişelenmenize gerek kalmaz. [Bekletme dönemi içinde](../../database/recovery-using-backups.md#point-in-time-restore)herhangi bir noktaya geri yükleme yapabilirsiniz. Ayrıca, yüksek kullanılabilirlik [yerleşik olarak oluşturulduğu](../../database/high-availability-sla.md)için yüksek kullanılabilirlik ayarlama konusunda endişelenmeniz gerekmez. 

Güvenliği güçlendirmek için [Azure Active Directory kimlik doğrulaması](../../database/authentication-aad-overview.md), [Denetim](../../managed-instance/auditing-configure.md), [tehdit algılama](../../database/azure-defender-for-sql.md), [satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security)ve [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)kullanmayı göz önünde bulundurun.

Gelişmiş Yönetim ve güvenlik özelliklerine ek olarak, SQL yönetilen örneği, [iş yükünüzü izlemenize ve ayarlamanıza](../../database/monitor-tune-overview.md)yardımcı olabilecek bir dizi gelişmiş araç sağlar. [Azure SQL Analytics](../../../azure-monitor/insights/azure-sql.md) , büyük bir yönetilen örnek kümesini merkezi bir şekilde izlemenize olanak sağlar.  [Otomatik ayarlama](/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction)   yönetilen örneklerde, SQL planı yürütme istatistiklerinizin performansını sürekli olarak izler ve belirlenen performans sorunlarını otomatik olarak düzeltir. 

Bazı özellikler yalnızca [veritabanı uyumluluk düzeyi](/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database) en son uyumluluk düzeyine (150) değiştirildiğinde kullanılabilir. 

## <a name="migration-assets"></a>Geçiş varlıkları 

Ek Yardım için, gerçek dünya geçiş projeleri için geliştirilen aşağıdaki kaynaklara bakın.

|Varlık  |Açıklama  |
|---------|---------|
|[Veri iş yükü değerlendirmesi modeli ve aracı](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Bu araç, belirli bir iş yükü için önerilen "en uygun" hedef platformları, bulut hazırlığı ve uygulama/veritabanı düzeltme düzeyini sağlar. Basit ve tek tıklamayla bir hesaplama ve rapor oluşturma olanağı sunarak, ve otomatikleştirilmiş ve Tekdüzen hedef platformu karar süreci sağlayarak büyük Emlak değerlendirmelerini hızlandırmaya yardımcı olur.|
|[DBLoader yardımcı programı](https://github.com/microsoft/DataMigrationTeam/tree/master/DBLoader%20Utility)|DBLoader, sınırlandırılmış metin dosyalarından SQL Server içine veri yüklemek için kullanılabilir. Bu Windows konsol yardımcı programı, Azure SQL MI dahil olmak üzere tüm SQL Server sürümlerinde çalışan SQL Server Native Client BulkLoad arabirimini kullanır.|
|[Şirket Içi SQL Server oturumlarını Azure SQL yönetilen örneğine taşımaya yönelik yardımcı program](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/MoveLogins)|Oturum açmaları yeniden oluşturmak ve şirket içi SQL Server Azure SQL yönetilen örneğine veritabanı kullanıcılarını seçmek için bir T-SQL komut betiği oluşturan bir PowerShell betiği. Araç Windows AD hesaplarının otomatik olarak Azure AD hesaplarına eşlenmesini ve isteğe bağlı olarak SQL Server yerel oturum açma işlemleri geçirilmesini sağlar.|
|[Logman kullanarak PerfMon veri toplama Otomasyonu](https://github.com/microsoft/DataMigrationTeam/tree/master/IP%20and%20Scripts/Perfmon%20Data%20Collection%20Automation%20Using%20Logman)|Geçiş hedefi önerisine yardımcı olan temel performansı anlamak için veri gerçekleştirme önerilerini toplayan bir araç. Bu araç, uzak bir SQL Server ayarlanan performans sayaçlarını oluşturacak, başlatacak, durdurmayacak ve silecek komutu oluşturmak için logman.exe kullanır.|
|[Teknik İnceleme-tam ve değişiklik yedeklemelerini geri yükleyerek Azure SQL yönetilen örneğine Veritabanı geçişi](https://github.com/microsoft/DataMigrationTeam/blob/master/Whitepapers/Database%20migrations%20to%20Azure%20SQL%20DB%20Managed%20Instance%20-%20%20Restore%20with%20Full%20and%20Differential%20backups.pdf)|Bu Teknik İnceleme, yalnızca tam ve fark yedeklemeleriniz varsa (ve günlük yedekleme özelliği yoksa) SQL Server ile Azure SQL yönetilen örneğine geçişleri hızlandırmaya yardımcı olmak için rehberlik ve adımlar sağlar.|

Bu kaynaklar, Azure veri grubu Mühendisliği ekibi tarafından sponsorlu Data SQL Dokja programının bir parçası olarak geliştirilmiştir. Data SQL Dokja programının temel kreter, Microsoft 'un Azure veri platformuna yönelik karmaşık modernleştirme ve rekabet veri platformu geçiş fırsatlarını engellemeyi ve hızlandırmanızı sağlar. Kuruluşunuzun Data SQL Dokja programına katılım ile ilgilenmesini düşünüyorsanız, lütfen hesap ekibinize başvurarak bir aday göndermesini isteyin.


## <a name="next-steps"></a>Sonraki adımlar

SQL Server Azure SQL yönetilen örneği 'ne geçirmeye başlamak için [SQL Server Azure SQL yönetilen örnek geçiş kılavuzuna](sql-server-to-managed-instance-guide.md)bakın.

- Çeşitli veritabanı ve veri geçişi senaryolarında ve özel görevlerin yanı sıra size yardımcı olmak için sunulan Microsoft ve üçüncü taraf hizmet ve araçların bir matrisi için bkz. [veri geçişi Için hizmet ve araçlar](../../../dms/dms-tools-matrix.md).

- Azure SQL yönetilen örneği hakkında daha fazla bilgi için bkz.:
   - [Azure SQL yönetilen örneği 'nde hizmet katmanları](../../managed-instance/sql-managed-instance-paas-overview.md#service-tiers)
   - [SQL Server ile Azure SQL yönetilen örneği arasındaki farklar](../../managed-instance/transact-sql-tsql-differences-sql-server.md)
   - [Azure toplam sahiplik Hesaplayıcı maliyeti](https://azure.microsoft.com/pricing/tco/calculator/) 


- Bulut geçişleri için çerçeve ve benimseme çevrimi hakkında daha fazla bilgi edinmek için bkz.
   -  [Azure için Bulut Benimseme Çerçevesi](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [İş yüklerini maliyetlendirme ve boyutlandırma için en iyi yöntemler Azure 'a geçiş](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 


- Uygulama erişim katmanını değerlendirmek için bkz. [veri erişimi geçiş araç seti (Önizleme)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)
- Veri erişim katmanını bir/B testi gerçekleştirme hakkında daha fazla bilgi için bkz. [veritabanı yükseltme deneyimi Yardımcısı](/sql/dea/database-experimentation-assistant-overview).
