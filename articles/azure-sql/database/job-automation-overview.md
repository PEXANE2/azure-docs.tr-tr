---
title: İş otomasyonu
description: Transact-SQL (T-SQL) betiklerini bir veya daha fazla veritabanı kümesi üzerinde çalıştırmak için Iş Otomasyonu kullanma
services: sql-database
ms.service: sql-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: overview
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 03/10/2020
ms.openlocfilehash: 519ed71977a1e31bcf3f88fbff56f35656789271
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054620"
---
# <a name="automate-management-tasks-using-database-jobs"></a>Veritabanı işlerini kullanarak yönetim görevlerini otomatikleştirme
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

T-SQL sorguları çalıştırmak ve bakım görevlerini gerçekleştirmek için bir veya daha fazla veritabanına göre düzenli olarak yürütülen işleri oluşturabilir ve zamanlayabilirsiniz.

İşin gerçekleştirileceği hedef veritabanını veya veritabanı gruplarını tanımlayabilir ve ayrıca bir işi çalıştırmaya yönelik zamanlamalar tanımlayabilirsiniz.
Bir iş, hedef veritabanında oturum açma görevini işler. Ayrıca, bir veritabanı grubu genelinde yürütülecek Transact-SQL betiklerini tanımlar, korur ve kalıcı hale getirin.

Her iş, yürütme durumunu günlüğe kaydeder ve herhangi bir hata oluşursa işlemleri otomatik olarak yeniden dener.

## <a name="when-to-use-automated-jobs"></a>Otomatikleştirilmiş işlerin ne zaman kullanılacağı

İş Otomasyonu 'nu kullanabileceğiniz birkaç senaryo vardır:

- Yönetim görevlerini otomatikleştirin ve saatler, saat sonra her hafta içinde çalışacak şekilde zamanlayın.
  - Şema değişiklikleri, kimlik bilgileri yönetimi, performans verisi toplama veya kiracı (müşteri) telemetri verilerini toplama gibi görevleri dağıtın.
  - Başvuru verilerini güncelleştirme (tüm veritabanlarında ortak bilgiler), Azure Blob depolamadan veri yükleme.
  - Sorgu performansını artırmak için dizinleri yeniden oluşturun. İşleri bir veritabanı koleksiyonunda yoğun saatlerin dışında yenilenecek şekilde yapılandırın.
  - Bir veritabanı kümesinden alınan sorgu sonuçlarını düzenli olarak merkezi bir tabloya toplayın. Performans sorguları sürekli yürütülebilir ve yürütülecek ek görevleri tetikleyecek şekilde yapılandırılabilir.
- Raporlama için veri toplama
  - Veritabanları koleksiyonundan tek bir hedef tabloya veri toplama.
  - Çok sayıda veritabanında müşteri telemetri verilerinin toplanması gibi daha uzun süre çalışan veri işleme sorguları çalıştırın. Sonuçlar daha ayrıntılı analiz için tek bir hedef tabloda toplanır.
- Veri taşımaları
  - Veritabanlarınızda yapılan değişiklikleri diğer veritabanlarına çoğaltan veya uzak veritabanlarında yapılan güncelleştirmeleri toplayacak ve değişiklikleri veritabanına uygulayan işler oluşturun.
  - SQL Server Integration Services (SSIS) kullanarak veritabanlarınızdan veri yükleyen işler oluşturun.

## <a name="overview"></a>Genel Bakış

Aşağıdaki iş zamanlama teknolojileri kullanılabilir:

- **SQL Aracısı işleri** , Azure SQL yönetilen örneği 'nde kullanılabilen, klasik ve yeniden çalıştırılan SQL Server iş zamanlama bileşenidir. SQL Aracısı Işleri Azure SQL veritabanı 'nda kullanılamıyor.
- **Elastik veritabanı işleri (Önizleme)** bir veya birden çok Azure SQL veritabanında özel Işler çalıştıran iş zamanlama hizmetlerdir.

SQL Aracısı (Şirket içi ve SQL yönetilen örneğinin parçası olarak kullanılabilir) ve veritabanı elastik Iş Aracısı (Azure SQL veritabanı ve SQL veri ambarı 'ndaki veritabanları için kullanılabilir) arasında birkaç farklılık olduğunu belirtmekte bir değer.

| |Elastik İşler |SQL Aracısı |
|---------|---------|---------|
|Kapsam | İş aracısıyla aynı Azure bulutunda herhangi bir sayıda Azure SQL Veritabanı ve/veya veri ambarı. Hedefler farklı sunucularda, aboneliklerde ve/veya bölgelerde olabilir. <br><br>Hedef gruplar tek veritabanı veya veri ambarlarının yanı sıra bir sunucu, havuz veya parça eşlemesi içindeki tüm veritabanlarından (iş zamanında dinamik olarak numaralandırılır) oluşabilir. | SQL aracısıyla aynı örnekteki her bir veritabanı. |
|Desteklenen API’ler ve Araçlar | Portal, PowerShell, T-SQL, Azure Resource Manager | T-SQL, SQL Server Management Studio (SSMS) |

## <a name="sql-agent-jobs"></a>SQL Aracısı İşleri

SQL Aracısı Işleri, veritabanınıza göre belirtilen bir T-SQL komut dosyası dizisidir. İşleri, bir veya daha fazla kez çalıştırılabilen ve başarı veya başarısızlık için izlenen bir yönetim görevi tanımlamak için kullanın.
Bir iş, bir yerel sunucuda veya birden çok uzak sunucuda çalıştırılabilir. SQL Aracısı Işleri, yönetilen örnek hizmeti içinde yürütülen bir iç veritabanı altyapısı bileşenidir.
SQL Aracısı Işlerinde birkaç temel kavram vardır:

- İş içinde yürütülmesi gereken bir veya daha fazla adımın **iş adımları** kümesi. Her iş adımı için yeniden deneme stratejisi tanımlayabilir ve iş adımının başarılı veya başarısız olması durumunda gerçekleşmesi gereken eylemi belirleyebilirsiniz.
- **Zamanlamalar** , işin ne zaman yürütülmesi gerektiğini tanımlar.
- **Bildirimler** , iş tamamlandıktan sonra işleçlere e-posta aracılığıyla bildirimde bulunan kurallar tanımlamanızı sağlar.

### <a name="job-steps"></a>İş adımları

SQL Aracısı Iş adımları SQL aracısının yürütmesi gereken eylem dizileridir. Her adımda, adım başarılı veya başarısız olursa yürütülmesi gereken adım, hata durumunda yeniden deneme sayısı.
SQL Aracısı, veritabanında tek bir Transact-SQL toplu işi yürüten Transact-SQL iş adımı veya özel işletim sistemi betiği yürütebilen IŞLETIM sistemi komutu/PowerShell adımları gibi farklı türlerde iş adımları oluşturmanızı sağlar, SSIS iş adımları, veritabanınızdaki değişiklikleri veritabanından diğer veritabanlarına yayımlayabilen bir [çoğaltma](../managed-instance/replication-transactional-overview.md) adımları veya veritabanlarını yükleyebilir.

[İşlemsel çoğaltma](../managed-instance/replication-transactional-overview.md) , bir veya birden çok tabloda yapılan değişiklikleri tek bir veritabanında yayımlamanıza ve bunları bir abone veritabanları kümesine yayımlamanıza/dağıtmanıza olanak sağlayan bir veritabanı altyapısı özelliğidir. Değişiklikleri yayımlamak aşağıdaki SQL Aracısı işi adım türleri kullanılarak uygulanır:

- İşlem-günlük okuyucu.
- Görüntüye.
- Dağıtım.

Diğer iş adımı türleri şu anda desteklenmemektedir:

- Birleştirme çoğaltması işi adımı desteklenmiyor.
- Sıra okuyucusu desteklenmiyor.
- Analysis Services desteklenmez

### <a name="job-schedules"></a>İş zamanlamaları

Bir zamanlama, bir işin ne zaman çalışacağını belirtir. Aynı zamanlamaya göre birden fazla iş çalışabilir ve aynı iş için birden fazla zamanlama uygulanabilir.
Bir zamanlama, bir işin çalıştığı zaman için aşağıdaki koşulları tanımlayabilir:

- Örnek her yeniden başlatıldığında (veya SQL Server Agent başladığında). İş her yük devretmeden sonra etkinleştirilir.
- Bir kez, belirli bir tarih ve saatte, bazı bir işin gecikmeli yürütülmesi için faydalıdır.
- Yinelenen bir zamanlamaya göre.

> [!Note]
> SQL yönetilen örneği şu anda örnek "boşta" olduğunda bir iş başlatmayı etkinleştirmiyor.

### <a name="job-notifications"></a>İş bildirimleri

SQL Aracısı Işleri, iş başarıyla tamamlandığında veya başarısız olduğunda bildirim almanızı sağlar. E-posta aracılığıyla bildirim alabilirsiniz.

İlk olarak, `AzureManagedInstance_dbmail_profile` Aşağıdaki örnekte gösterildiği gibi e-posta bildirimlerini göndermek ve hesabı adlı e-posta profiline atamak için kullanılacak e-posta hesabını ayarlamanız gerekir:

```sql
-- Create a Database Mail account
EXECUTE msdb.dbo.sysmail_add_account_sp
    @account_name = 'SQL Agent Account',
    @description = 'Mail account for Azure SQL Managed Instance SQL Agent system.',
    @email_address = '$(loginEmail)',
    @display_name = 'SQL Agent Account',
    @mailserver_name = '$(mailserver)' ,
    @username = '$(loginEmail)' ,
    @password = '$(password)'

-- Create a Database Mail profile
EXECUTE msdb.dbo.sysmail_add_profile_sp
    @profile_name = 'AzureManagedInstance_dbmail_profile',
    @description = 'E-mail profile used for messages sent by Managed Instance SQL Agent.' ;

-- Add the account to the profile
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp
    @profile_name = 'AzureManagedInstance_dbmail_profile',
    @account_name = 'SQL Agent Account',
    @sequence_number = 1;
```

Ayrıca, yönetilen örnekte Veritabanı Postası etkinleştirmeniz gerekir:

```sql
GO
EXEC sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
EXEC sp_configure 'Database Mail XPs', 1;
GO
RECONFIGURE
```

SQL Agent işleriniz ile ilgili bir şeyin olduğunu işlecine bildirebilirsiniz. Bir işleç, bir veya daha fazla yönetilen örnek bakımda sorumlu için kişi bilgilerini tanımlar. Bazen, operatör sorumlulukları tek bir kişiye atanır.
Birden çok Azure SQL yönetilen örneği veya SQL Server örneği olan sistemlerde birçok kişi işleç sorumluluklarını paylaşabilir. Operatör güvenlik bilgilerini içermez ve bir güvenlik sorumlusu tanımlamaz.

SSMS veya Transact-SQL komut dosyasını kullanarak aşağıdaki örnekte gösterilen işleçler oluşturabilirsiniz:

```sql
EXEC msdb.dbo.sp_add_operator
    @name=N'Mihajlo Pupun',
    @enabled=1,
    @email_address=N'mihajlo.pupin@contoso.com'
```

Her türlü işi değiştirebilir ve iş tamamlandığında, başarısız olursa veya SSMS veya aşağıdaki Transact-SQL komut dosyası ile başarılı olursa e-posta yoluyla bildirim gönderilecek işleçleri atayabilirsiniz:

```sql
EXEC msdb.dbo.sp_update_job @job_name=N'Load data using SSIS',
    @notify_level_email=3, -- Options are: 1 on succeed, 2 on failure, 3 on complete
    @notify_email_operator_name=N'Mihajlo Pupun'
```

### <a name="sql-agent-job-limitations"></a>SQL Aracısı Iş sınırlamaları

SQL Server ' de kullanılabilir olan SQL Aracısı özelliklerinden bazıları yönetilen örnekte desteklenmez:

- SQL Aracısı ayarları salt okunurdur. Yordam `sp_set_agent_properties` yönetilen örnekte desteklenmiyor.
- SQL aracısının etkinleştirilmesi/devre dışı bırakılması Şu anda yönetilen örnekte desteklenmiyor. SQL Server Agent sürekli çalışır.
- Bildirimler kısmen destekleniyor
  - Sayfalayıcı desteklenmiyor.
  - NetSend desteklenmez.
  - Uyarılar desteklenmez.
- Proxy 'ler desteklenmez.
- EventLog desteklenmiyor.

SQL Server Agent hakkında bilgi için bkz. [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

## <a name="elastic-database-jobs-preview"></a>Elastik veritabanı Işleri (Önizleme)

**Elastik Veritabanı İşleri**, bir zaman çizelgesine veya istek üzerine çok sayıda veritabanı arasında bir veya daha fazla T-SQL betiğinin paralel olarak çalıştırılmasını sağlar.

**İşleri tüm veritabanı birleşimleri üzerinde çalıştırabilirsiniz**: Bir veya daha fazla tek veritabanı, bir sunucu üzerindeki tüm veritabanları, bir elastik havuz veya parça eşlemesi içindeki tüm veritabanları için, herhangi bir veritabanını dahil etme veya hariç tutma esnekliğinden faydalanabilirsiniz. **İşler birden fazla sunucu ve birden fazla havuzda çalışabilir, hatta farklı aboneliklerde bulunan veritabanlarını kullanabilir.** Sunucular ve havuzlar çalışma zamanında dinamik olarak numaralandırıldığından işler, yürütme zamanında hedef grupta bulunan tüm veritabanlarında çalışır.

Aşağıdaki resimde farklı türlerdeki hedef gruplarda iş yürüten bir iş aracısı gösterilmektedir:

![Elastik İş aracısı kavramsal modeli](./media/job-automation-overview/conceptual-diagram.png)

### <a name="elastic-job-components"></a>Elastik İş bileşenleri

|Bileşen | Açıklama (ek ayrıntılar tablonun altındadır) |
|---------|---------|
|[**Elastik İş aracısı**](#elastic-job-agent) | İşleri çalıştırmak ve yönetmek için oluşturduğunuz Azure kaynağıdır. |
|[**İş veritabanı**](#job-database) | İş aracısının işle ilgili verileri, iş tanımlarını vb. depolamak için kullandığı bir Azure SQL veritabanı. |
|[**Hedef grup**](#target-group) | Bir işin çalıştırılacağı sunucu, havuz, veritabanı ve parça eşlemesi kümesidir. |
|[**İşinden**](#job) | İş, bir veya daha fazla [iş adımından](#job-step)oluşan bir iş birimidir. İş adımları çalıştırılacak T-SQL betiğinin yanı sıra betiğin yürütülmesi için gerekli olan diğer ayrıntıları belirtir. |

#### <a name="elastic-job-agent"></a>Elastik İş aracısı

Elastik İş aracısı; işlerin oluşturulması, çalıştırılması ve yönetilmesi için kullanılan Azure kaynağıdır. Elastik İş aracısı, portalda oluşturduğunuz bir Azure kaynağıdır ([PowerShell](elastic-jobs-powershell-create.md) ve REST de desteklenir).

**Elastik iş Aracısı** oluşturmak için var olan BIR Azure SQL veritabanı gerekir. Aracı, mevcut veritabanını [*İş veritabanı*](#job-database) olarak yapılandırır.

Elastik İş aracısı ücretsizdir. İş veritabanı, herhangi bir Azure SQL veritabanı ile aynı hızda faturalandırılır.

#### <a name="job-database"></a>İş veritabanı

*İş veritabanı*, işleri tanımlamanın yanı sıra iş yürütme durumunu ve geçmişini takip etmek için kullanılır. *İş veritabanı* , aracı meta verilerini, günlükleri, sonuçları, iş tanımlarını depolamak için de kullanılır ve T-SQL ' i kullanarak iş oluşturmak, çalıştırmak ve yönetmek için çok sayıda kullanışlı saklı yordam ve diğer veritabanı nesneleri içerir.

Geçerli önizleme için, elastik bir Iş Aracısı oluşturmak için var olan bir Azure SQL veritabanı (S0 veya üzeri) gereklidir.

*İş veritabanının* yeni olması gerekmez, ancak temiz, boş, S0 veya daha yüksek bir hizmet hedefi olmalıdır. *İş veritabanının* önerilen hizmet hedefi S1 veya daha yükseği, ancak en uygun seçenek, işinizin performans ihtiyaçlarına bağlıdır: iş adımları sayısı, iş hedefi sayısı ve işlerin ne sıklıkta çalıştırıldığı. Örneğin, bir S0 veritabanı, çok sayıda işi çalıştıran bir iş Aracısı için yeterli olabilir, ancak her dakika, bir S0 veritabanı ile yeterince hızlı olmayabilir ve daha yüksek bir hizmet katmanı daha iyi olabilir.

İş veritabanına yönelik işlemler beklenenden yavaşsa, Azure portal veya [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) DMV kullanarak yavaşlığın süresi boyunca veritabanı performansını ve iş veritabanındaki kaynak kullanımını [izleyin](monitor-tune-overview.md#azure-sql-database-and-azure-sql-managed-instance-resource-monitoring) . CPU, veri g/ç gibi bir kaynağın kullanımı veya %100 günlük yazma yaklaşımının ve yavaşlığın süreleriyle ilişkili olması halinde, iş veritabanı performansı yeterince iyileştirene kadar veritabanını daha yüksek hizmet hedeflerine ( [DTU modelinde](service-tiers-dtu.md) veya [Vcore modelinde](service-tiers-vcore.md)) artımlı olarak ölçeklendirin.

##### <a name="job-database-permissions"></a>İş veritabanı izinleri

İş aracısı oluşturma sırasında *İş veritabanında* bir şema, tablolar ve *jobs_reader* adlı bir rol oluşturulur. Rol, aşağıdaki izinle oluşturulur ve yöneticilere iş izleme için daha ayrıntılı erişim denetimi sunmak üzere tasarlanmıştır:

|Rol adı |'jobs' şeması izinleri |'jobs_internal' şeması izinleri |
|---------|---------|---------|
|**jobs_reader** | SELECT | Yok |

> [!IMPORTANT]
> Veritabanı yöneticisi olarak *İş veritabanına* erişim izni vermeden önce güvenlik durumunu gözden geçirin. İş oluşturma veya düzenleme izinlerine sahip olan kötü niyetli bir kullanıcı, kendi denetimindeki bir veritabanına bağlanmak için kayıtlı kimlik bilgisini kullanan bir iş oluşturarak veya düzenleyerek ilgili kimlik bilgisinin parolasını belirleyebilir.

#### <a name="target-group"></a>Hedef grup

*Hedef grup*, işin üzerinde çalışacağı veritabanı kümesini tanımlar. Hedef grup içinde aşağıdaki bileşenlerden birden fazlası veya birleşimi bulunabilir:

- **MANTıKSAL SQL Server** -bir sunucu belirtilmişse, iş yürütmesi sırasında sunucuda bulunan tüm veritabanları grubun bir parçasıdır. İş yürütülmeden önce grubun numaralandırılması ve güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Elastik havuz**: Elastik havuz belirtilirse işin yürütülmesi sırasında elastik havuzda mevcut olan tüm veritabanları gruba dahil edilir. Sunucuda olduğu gibi iş yürütülmeden önce grubun güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Tek veritabanı**: Gruba eklenmek üzere bir veya daha fazla tek veritabanı belirtin.
- **Parça eşlemesi**: Bir parça eşlemesinin veritabanlarıdır.

> [!TIP]
> İşin yürütülmesi sırasında *dinamik numaralandırma*, sunucuları veya havuzları içeren hedef gruplardaki veritabanı kümesini yeniden değerlendirir. Dinamik numaralandırma, **işlerin, yürütüldükleri sırada sunucuda veya havuzda mevcut olan tüm veritabanlarında çalıştırılmasını sağlar**. Veritabanı listesinin çalışma zamanında yeniden değerlendirilmesi özellikle havuz veya sunucu üyeliğinin sık değiştiği senaryolar için kullanışlıdır.

Havuzlar ve tek veritabanları gruba dahil edilebilir veya gruptan hariç tutulabilir. Bu sayede istenen veritabanlarını içeren bir hedef grup oluşturulabilir. Örneğin hedef gruba bir sunucuyu ekleyebilir ancak bir elastik havuzdaki belirli veritabanlarını (veya bir havuzun tamamını) hariç tutabilirsiniz.

Hedef grupta birden fazla abonelikte ve bölgede bulunan veritabanları mevcut olabilir. Birden fazla bölgenin söz konusu olduğu yürütme işlemlerinin aynı bölgedeki yürütmelere kıyasla daha yüksek gecikme süresine sahip olacağını unutmayın.

Aşağıdaki örneklerde işin çalıştırılacağı veritabanlarının belirlenmesi için farklı hedef grubu tanımlarının iş yürütme sırasında nasıl dinamik olarak numaralandırıldığı gösterilmektedir:

![Hedef grup örnekleri](./media/job-automation-overview/targetgroup-examples1.png)

**Örnek 1**'de tek veritabanlarının listesini içeren bir hedef grup gösterilmektedir. Bir iş adımı bu hedef grup kullanılarak yürütüldüğünde iş adımının eylemi bu veritabanlarının her birinde yürütülür.<br>
**Örnek 2** , hedef olarak sunucu içeren bir hedef grubu gösterir. Bir iş adımı bu hedef grup kullanılarak yürütüldüğünde sunucudaki veritabanı listesinin belirlenmesi için sunucu dinamik olarak numaralandırılır. İş adımının eylemi bu veritabanlarının her birinde yürütülür.<br>
**Örnek 3**'te gösterilen hedef grup *Örnek 2*'dekine benzer ancak belirli veritabanları özel olarak hariç tutulmuştur. İş adımının eylemi hariç tutulan veritabanında *yürütülmez*.<br>
**Örnek 4**'te hedef olarak bir elastik havuz içeren bir hedef grup gösterilmektedir. *Örnek 2*'ye benzer şekilde havuz iş çalıştırma zamanında dinamik olarak numaralandırılarak havuzdaki veritabanlarının listesi belirlenir.
<br><br>

![Hedef grup örnekleri](./media/job-automation-overview/targetgroup-examples2.png)

**Örnek 5** ve **örnek 6** , sunucuların, elastik havuzların ve veritabanlarının dahil etme ve dışlama kuralları kullanılarak birleştirilebileceği gelişmiş senaryolar göstermektedir.<br>
**Örnek 7**'de parça eşlemesi içinde bulunan ve iş çalıştırma zamanında değerlendirilebilecek parçalar gösterilmektedir.

> [!NOTE]
> Iş veritabanının kendisi bir işin hedefi olabilir. Bu senaryoda, Iş veritabanı tıpkı diğer hedef veritabanları gibi değerlendirilir. İş kullanıcısının oluşturulması ve iş veritabanında yeterli izinleri verilmesi gerekir ve iş kullanıcısına ait veritabanı kapsamlı kimlik bilgileri, diğer hedef veritabanları için de olduğu gibi Iş veritabanında da bulunmalıdır.
>

#### <a name="job"></a>İş

*İş*, bir zamanlamaya göre veya tek seferlik yürütülen bir çalışma birimidir. Bir işte bir veya daha fazla *iş adımı* bulunur.

##### <a name="job-step"></a>İş adımı

Her işte yürütülecek bir T-SQL betiği, bu T-SQL betiğinin çalıştırılacağı bir veya daha fazla hedef grup ve iş aracısının hedef veritabanına bağlanması için gereken kimlik bilgileri belirtilir. İşin her adımı özelleştirilebilir zaman aşımı ve yeniden deneme ilkelerine sahiptir ve her adımda isteğe bağlı çıkış parametreleri belirtilebilir.

#### <a name="job-output"></a>İş çıktısı

İş adımlarının her bir hedef veritabanında elde ettiği sonuç ayrıntılı olarak kaydedilir ve betik çıktısı, belirtilen bir tabloya aktarılabilir. Bir işin döndürdüğü verilerin kaydedileceği bir veritabanı belirtebilirsiniz.

#### <a name="job-history"></a>İş geçmişi

İş yürütme geçmişi *İş veritabanında* depolanır. Sistem temizleme işlemi 45 günden daha eski olan yürütme geçmişi verilerini siler. 45 günden daha yeni olan geçmişi kaldırmak için *İş veritabanında***sp_purge_history** saklı yordamını çağırın.

### <a name="agent-performance-capacity-and-limitations"></a>Aracı performansı, kapasitesi ve sınırlamaları

Elastik İşler, uzun süren işlerin tamamlanması sırasında en az düzeyde işlem kaynağı kullanır.

Hedef veritabanı grubunun boyutuna ve bir işin istenen yürütme süresine (eşzamanlı çalışan sayısı) bağlı olarak aracı için gerekli olan işlem süresi ve *İş veritabanı* performansı değişiklik gösterir (hedef ve iş sayısı ne kadar yüksek olursa gereken işlem zamanı o kadar fazla olur).

Önizleme şu an için 100 eşzamanlı işle sınırlıdır.

#### <a name="prevent-jobs-from-reducing-target-database-performance"></a>İşlerin hedef veritabanının performansını düşürmesini engelleme

Bir SQL elastik havuzundaki veritabanları üzerinde iş çalıştırılması sırasında kaynakların aşırı yüklenmesini önlemek için işler aynı anda üzerinde çalışılabilecek veritabanı sayısını sınırlayacak şekilde yapılandırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server Agent nedir?](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent)
- [Elastik işler oluşturma ve yönetme](elastic-jobs-overview.md)
- [PowerShell kullanarak elastik Işler oluşturma ve yönetme](elastic-jobs-powershell-create.md)
- [Transact-SQL (T-SQL) kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-tsql-create-manage.md)
