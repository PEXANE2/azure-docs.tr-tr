---
title: İş otomasyonu
description: Bir veya daha fazla Azure SQL veritabanı kümesinde Transact-SQL (T-SQL) komut dosyalarını çalıştırmak için İş Otomasyonu'nu kullanın
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: overview
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 03/10/2020
ms.openlocfilehash: dcaaf3c2f793e7148e1695cdfaa68c768db5fff6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240544"
---
# <a name="automate-management-tasks-using-database-jobs"></a>Veritabanı işlerini kullanarak yönetim görevlerini otomatikleştirin

Azure SQL Veritabanı, T-SQL sorgularını çalıştırmak ve bakım görevlerini gerçekleştirmek için bir veya birden çok veritabanına karşı düzenli olarak yürütülebilecek işler oluşturmanıza ve zamanlamanıza olanak tanır.
Her iş yürütme durumunu kaydeder ve herhangi bir hata oluşursa işlemleri otomatik olarak yeniden dener.
İşyürütülecek hedef veritabanı veya Azure SQL veritabanları grupları tanımlayabilir ve bir işi çalıştırmak için zamanlamaları da tanımlayabilirsiniz.
Bir iş, hedef veritabanında oturum açma görevini işler. Ayrıca, bir grup Azure SQL veritabanında yürütülecek Transact-SQL komut dosyalarını tanımlar, korur ve kalıcı hale alırsınız.

## <a name="when-to-use-automated-jobs"></a>Otomatik işlerin ne zaman kullanılacağı

İş otomasyonu kullanabileceğiniz birkaç senaryo vardır:

- Yönetim görevlerini otomatikleştirin ve hafta içi her gün, mesai saatleri vb. çalışacak şekilde zamanlayın.
  - Şema değişiklikleri, kimlik bilgileri yönetimi, performans verisi toplama veya kiracı (müşteri) telemetri verilerini toplama gibi görevleri dağıtın.
  - Başvuru verilerini (tüm veritabanlarında yaygın olan bilgiler) güncelleştirin, Azure Blob depolamadan veri yükleyin.
  - Sorgu performansını artırmak için dizinleri yeniden oluşturun. İşleri bir veritabanı koleksiyonunda yoğun saatlerin dışında yenilenecek şekilde yapılandırın.
  - Bir veritabanı kümesinden alınan sorgu sonuçlarını düzenli olarak merkezi bir tabloya toplayın. Performans sorguları sürekli yürütülebilir ve yürütülecek ek görevleri tetikleyecek şekilde yapılandırılabilir.
- Raporlama için veri toplama
  - Bir Azure SQL veritabanı koleksiyonunda bulunan verileri tek bir hedef tabloda toplayın.
  - Çok sayıda veritabanında müşteri telemetri verilerinin toplanması gibi daha uzun süre çalışan veri işleme sorguları çalıştırın. Sonuçlar daha ayrıntılı analiz için tek bir hedef tabloda toplanır.
- Veri hareketleri
  - Veritabanlarınızda yapılan değişiklikleri diğer veritabanlarında çoğaltan veya uzak veritabanlarında yapılan güncelleştirmeleri toplayan ve veritabanında değişiklikler uygulayan işler oluşturun.
  - SQL Server Integration Services (SSIS) kullanarak veritabanlarınızdan veya veritabanlarınıza veri yükleyen işler oluşturun.

## <a name="overview"></a>Genel Bakış

Aşağıdaki iş zamanlama teknolojileri Azure SQL Veritabanı'nda kullanılabilir:

- **SQL Agent Jobs,** Yönetilen Örnek'te kullanılabilen klasik ve savaş testine tabi SQL Server iş zamanlama bileşenidir. SQL Aracı İşleri Azure SQL tek veritabanlarında kullanılamaz.
- **Elastik Veritabanı İşleri (önizleme),** bir veya birden çok Azure SQL Veritabanında özel işler yürüten İş Zamanlama hizmetleridir.

SQL Agent (şirket içinde ve SQL Veritabanı Yönetilen Örneği'nin bir parçası olarak kullanılabilir) ile Veritabanı Elastik İş aracısı (Azure SQL veritabanındaki tek veritabanları ve SQL Veri Ambarı'ndaki veritabanları için kullanılabilir) arasındaki birkaç farka dikkat çekmektedir.

| |Elastik İşler |SQL Aracısı |
|---------|---------|---------|
|Kapsam | İş aracısıyla aynı Azure bulutundaki herhangi bir sayıda Azure SQL veritabanı ve/veya veri ambarı. Hedefler farklı SQL Veritabanı sunucularında, aboneliklerinde ve/veya bölgelerde olabilir. <br><br>Hedef gruplar tek veritabanı veya veri ambarlarının yanı sıra bir sunucu, havuz veya parça eşlemesi içindeki tüm veritabanlarından (iş zamanında dinamik olarak numaralandırılır) oluşabilir. | SQL aracısı ile aynı SQL Server örneğinde herhangi bir tek tek veritabanı. |
|Desteklenen API’ler ve Araçlar | Portal, PowerShell, T-SQL, Azure Resource Manager | T-SQL, SQL Server Management Studio (SSMS) |

## <a name="sql-agent-jobs"></a>SQL Aracı İşler

SQL Agent Jobs, veritabanınıza karşı belirli bir T-SQL komut dosyası serisidir. Bir veya daha fazla kez çalıştırılabilen ve başarı veya başarısızlık için izlenen yönetim görevlerini tanımlamak için işleri kullanın.
Bir iş tek bir yerel sunucuda veya birden çok uzak sunucuda çalıştırılabilir. SQL Agent Jobs, Yönetilen Örnek hizmeti içinde yürütülen bir iç Veritabanı Altyapısı bileşenidir.
SQL Agent Jobs'ta birkaç temel kavram vardır:

- İş içinde yürütülmesi gereken bir veya çok sayıda adımdan oluşan **iş adımları.** Her iş adımı için yeniden deneme stratejisini ve iş adımı başarılı veya başarısız olursa gerçekleşmesi gereken eylemi tanımlayabilirsiniz.
- **Zamanlamalar,** işin ne zaman yürütülmesi gerektiğini tanımlar.
- **Bildirimler,** iş tamamlandıktan sonra operatörleri e-posta yoluyla bilgilendirmek için kullanılacak kuralları tanımlamanıza olanak tanır.

### <a name="job-steps"></a>İş adımları

SQL Agent Job adımları, SQL Agent'ın yürütmesi gereken eylem dizileridir. Her adım, adım başarılı veya başarısız olursa, başarısızlık durumunda yeniden deneme sayısı yürütülmesi gereken aşağıdaki adım vardır.
SQL Agent, veritabanına karşı tek bir Transact-SQL toplu iş yürütmesi veya özel işletim sistemi komut dosyası yürütebilen işletim sistemi komutu/PowerShell adımları, SSIS iş adımlarını kullanarak veri yüklemenize olanak tanıyan İş Komutu/PowerShell adımları veya veritabanınızdan diğer veritabanlarına değişiklik yayımlayabilen [çoğaltma](sql-database-managed-instance-transactional-replication.md) adımları gibi farklı türde iş adımları oluşturmanıza olanak tanır.

[İşlem çoğaltma,](sql-database-managed-instance-transactional-replication.md) bir veya birden çok tabloda yapılan değişiklikleri tek bir veritabanında yayımlamanızı ve bunları bir abone veritabanı kümesine yayımlamanızı/dağıtmanızı sağlayan bir Veritabanı Altyapısı özelliğidir. Değişikliklerin yayımlama aşağıdaki SQL Agent iş adımı türleri kullanılarak uygulanır:

- İşlem günlüğü okuyucu.
- An -lık görüntü.
- Dağıtımcı.

Diğer iş adımları türleri şu anda desteklenmez:

- Birleştirme çoğaltma iş adımı desteklenmez.
- Sıra Okuyucu desteklenmez.
- Analiz Hizmetleri desteklenmiyor

### <a name="job-schedules"></a>İş zamanlamaları

Bir iş çalıştığında bir zamanlama belirtir. Aynı zamanlamada birden fazla iş çalıştırılabilir ve aynı işe birden fazla zamanlama uygulanabilir.
Zamanlama, bir işin çalıştığı süre için aşağıdaki koşulları tanımlayabilir:

- Instance yeniden başlatıldığında (veya SQL Server Agent başlatıldığında). İş her başarısızlıktan sonra etkinleştirilir.
- Bir kez, belirli bir tarih ve zamanda, hangi bazı iş gecikmiş yürütme için yararlıdır.
- Yinelenen bir programda.

> [!Note]
> Yönetilen Örnek şu anda, örnek "boşta" olduğunda bir işe başlamanızı sağlamaz.

### <a name="job-notifications"></a>İş bildirimleri

SQL Agent Jobs, iş başarılı bir şekilde bittiğinde veya başarısız olduğunda bildirim almanızı sağlar. Bildirimleri e-posta yoluyla alabilirsiniz.

İlk olarak, e-posta bildirimlerini göndermek için kullanılacak e-posta hesabını ayarlamanız ve hesabı `AzureManagedInstance_dbmail_profile`aşağıdaki örnekte gösterildiği gibi, adlı e-posta profiline atamanız gerekir:

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

Yönetilen Örnek'te Veritabanı Postasını da etkinleştirmeniz gerekir:

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

SQL Agent işlerinizde bir şey olduğunu operatöre bildirebilirsiniz. İşleç, bir veya daha fazla Yönetilen Örneğin bakımından sorumlu bir kişinin iletişim bilgilerini tanımlar. Bazen, operatör sorumlulukları bir kişiye atanır.
Birden çok Yönetilen Örnek veya SQL Sunucusu olan sistemlerde, birçok kişi işleç sorumluluklarını paylaşabilir. İşleç güvenlik bilgileri içermez ve bir güvenlik ilkesi tanımlamaz.

Aşağıdaki örnekte gösterilen SSMS veya Transact-SQL komut dosyasını kullanarak işleçler oluşturabilirsiniz:

```sql
EXEC msdb.dbo.sp_add_operator
    @name=N'Mihajlo Pupun',
    @enabled=1,
    @email_address=N'mihajlo.pupin@contoso.com'
```

İş tamamlanırsa, başarısız olursa veya SSMS veya aşağıdaki Transact-SQL komut dosyasını kullanırsa e-posta yoluyla bildirilecek herhangi bir işi değiştirebilir ve operatörler atayabilirsiniz:

```sql
EXEC msdb.dbo.sp_update_job @job_name=N'Load data using SSIS',
    @notify_level_email=3, -- Options are: 1 on succeed, 2 on failure, 3 on complete
    @notify_email_operator_name=N'Mihajlo Pupun'
```

### <a name="sql-agent-job-limitations"></a>SQL Aracılı İş Sınırlamaları

SQL Server'da kullanılabilen SQL Agent özelliklerinden bazıları Yönetilen Örnek'te desteklenmez:

- SQL Agent ayarları yalnızca okunur. Yordam `sp_set_agent_properties` Yönetilen Örnek'te desteklenmez.
- SQL Agent'ı etkinleştirmek/devre dışı bırakmak şu anda Yönetilen Örnek'te desteklenmez. SQL Agent her zaman çalışıyor.
- Bildirimler kısmen desteklenir
  - Çağrı Cihazı desteklenmez.
  - NetSend desteklenmez.
  - Uyarılar desteklenmez.
- Vekiller desteklenmez.
- Eventlog desteklenmez.

SQL Server Agent hakkında daha fazla bilgi için [SQL Server Agent'a](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent)bakın.

## <a name="elastic-database-jobs-preview"></a>Elastik Veritabanı İşleri (önizleme)

**Elastik Veritabanı İşleri**, bir zaman çizelgesine veya istek üzerine çok sayıda veritabanı arasında bir veya daha fazla T-SQL betiğinin paralel olarak çalıştırılmasını sağlar.

**İşleri tüm veritabanı birleşimleri üzerinde çalıştırabilirsiniz**: Bir veya daha fazla tek veritabanı, bir sunucu üzerindeki tüm veritabanları, bir elastik havuz veya parça eşlemesi içindeki tüm veritabanları için, herhangi bir veritabanını dahil etme veya hariç tutma esnekliğinden faydalanabilirsiniz. **İşler birden fazla sunucu ve birden fazla havuzda çalışabilir, hatta farklı aboneliklerde bulunan veritabanlarını kullanabilir.** Sunucular ve havuzlar çalışma zamanında dinamik olarak numaralandırıldığından işler, yürütme zamanında hedef grupta bulunan tüm veritabanlarında çalışır.

Aşağıdaki resimde farklı türlerdeki hedef gruplarda iş yürüten bir iş aracısı gösterilmektedir:

![Elastik İş aracısı kavramsal modeli](media/elastic-jobs-overview/conceptual-diagram.png)

### <a name="elastic-job-components"></a>Elastik İş bileşenleri

|Bileşen | Açıklama (ek ayrıntılar tablonun altındadır) |
|---------|---------|
|[**Elastik İş aracısı**](#elastic-job-agent) | İşleri çalıştırmak ve yönetmek için oluşturduğunuz Azure kaynağıdır. |
|[**İş veritabanı**](#job-database) | İş aracısının işle ilgili veriler, iş tanımları gibi bilgileri depolamak için kullandığı bir Azure SQL veritabanıdır. |
|[**Hedef grup**](#target-group) | Bir işin çalıştırılacağı sunucu, havuz, veritabanı ve parça eşlemesi kümesidir. |
|[**Iş**](#job) | Bir iş, bir veya daha fazla [iş adımlarından](#job-step)oluşan bir iş birimidir. İş adımları çalıştırılacak T-SQL betiğinin yanı sıra betiğin yürütülmesi için gerekli olan diğer ayrıntıları belirtir. |

#### <a name="elastic-job-agent"></a>Elastik İş aracısı

Elastik İş aracısı; işlerin oluşturulması, çalıştırılması ve yönetilmesi için kullanılan Azure kaynağıdır. Elastik İş aracısı, portalda oluşturduğunuz bir Azure kaynağıdır ([PowerShell](elastic-jobs-powershell.md) ve REST de desteklenir).

**Elastik İş aracısı** oluşturmak için bir SQL veritabanı gerekir. Aracı, mevcut veritabanını [*İş veritabanı*](#job-database) olarak yapılandırır.

Elastik İş aracısı ücretsizdir. İş veritabanı, herhangi bir SQL veritabanıyla aynı oranda faturalandırılır.

#### <a name="job-database"></a>İş veritabanı

*İş veritabanı*, işleri tanımlamanın yanı sıra iş yürütme durumunu ve geçmişini takip etmek için kullanılır. *İş veritabanı* ayrıca aracı meta verileri, günlükleri, sonuçları, iş tanımlarını depolamak için de kullanılır ve ayrıca T-SQL kullanarak işleri oluşturmak, çalıştırmak ve yönetmek için birçok yararlı depolanmış yordamlar ve diğer veritabanı nesneleri içerir.

Geçerli önizlemede, Elastik İş aracısı oluşturmak için bir Azure SQL veritabanı (S0 veya üzeri) gerekir.

*İş veritabanı* nın tam anlamıyla yeni olması gerekmez, ancak temiz, boş, S0 veya daha yüksek bir hizmet hedefi olmalıdır. *İş veritabanının* önerilen hizmet hedefi S1 veya daha yüksektir, ancak en iyi seçim işinizin performans gereksinimlerine bağlıdır: iş adımlarının sayısı, iş hedeflerinin sayısı ve işlerin ne sıklıkta çalıştırılması. Örneğin, bir S0 veritabanı, on veritabanlarından daha az hedefleyen bir saatte birkaç iş çalıştıran bir iş aracısı için yeterli olabilir, ancak her dakika bir işi çalıştırmak Bir S0 veritabanıyla yeterince hızlı olmayabilir ve daha yüksek bir hizmet katmanı daha iyi olabilir.

İş veritabanına yönelik işlemler beklenenden daha yavaşsa, Azure portalı veya [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) DMV'yi kullanarak yavaşlık dönemlerinde veritabanı performansını ve iş veritabanındaki kaynak kullanımını [izleyin.](sql-database-monitor-tune-overview.md#sql-database-resource-monitoring) CPU, Veri IO veya Log Write gibi bir kaynağın kullanımı %100'e yaklaşırsa ve yavaşlık dönemleri ile ilişkiliyse, iş veritabanı performansı yeterince iyileşene kadar veritabanını daha yüksek hizmet hedeflerine [(DTU modelinde](sql-database-service-tiers-dtu.md) veya [vCore modelinde)](sql-database-service-tiers-vcore.md)kademeli olarak ölçeklendirmeyi düşünün.

##### <a name="job-database-permissions"></a>İş veritabanı izinleri

İş aracısı oluşturma sırasında *İş veritabanında* bir şema, tablolar ve *jobs_reader* adlı bir rol oluşturulur. Rol, aşağıdaki izinle oluşturulur ve yöneticilere iş izleme için daha ayrıntılı erişim denetimi sunmak üzere tasarlanmıştır:

|Rol adı |'jobs' şeması izinleri |'jobs_internal' şeması izinleri |
|---------|---------|---------|
|**jobs_reader** | SELECT | None |

> [!IMPORTANT]
> Veritabanı yöneticisi olarak *İş veritabanına* erişim izni vermeden önce güvenlik durumunu gözden geçirin. İş oluşturma veya düzenleme izinlerine sahip olan kötü niyetli bir kullanıcı, kendi denetimindeki bir veritabanına bağlanmak için kayıtlı kimlik bilgisini kullanan bir iş oluşturarak veya düzenleyerek ilgili kimlik bilgisinin parolasını belirleyebilir.

#### <a name="target-group"></a>Hedef grup

*Hedef grup*, işin üzerinde çalışacağı veritabanı kümesini tanımlar. Hedef grup içinde aşağıdaki bileşenlerden birden fazlası veya birleşimi bulunabilir:

- **SQL Database server** - bir sunucu belirtilirse, iş yürütülmesi sırasında sunucuda bulunan tüm veritabanları grubun bir parçasıdır. İş yürütülmeden önce grubun numaralandırılması ve güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Elastik havuz**: Elastik havuz belirtilirse işin yürütülmesi sırasında elastik havuzda mevcut olan tüm veritabanları gruba dahil edilir. Sunucuda olduğu gibi iş yürütülmeden önce grubun güncelleştirilmesi için asıl veritabanı kimlik bilgisinin sağlanması gerekir.
- **Tek veritabanı**: Gruba eklenmek üzere bir veya daha fazla tek veritabanı belirtin.
- **Parça eşlemesi**: Bir parça eşlemesinin veritabanlarıdır.

> [!TIP]
> İşin yürütülmesi sırasında *dinamik numaralandırma*, sunucuları veya havuzları içeren hedef gruplardaki veritabanı kümesini yeniden değerlendirir. Dinamik numaralandırma, **işlerin, yürütüldükleri sırada sunucuda veya havuzda mevcut olan tüm veritabanlarında çalıştırılmasını sağlar**. Veritabanı listesinin çalışma zamanında yeniden değerlendirilmesi özellikle havuz veya sunucu üyeliğinin sık değiştiği senaryolar için kullanışlıdır.

Havuzlar ve tek veritabanları gruba dahil edilebilir veya gruptan hariç tutulabilir. Bu sayede istenen veritabanlarını içeren bir hedef grup oluşturulabilir. Örneğin hedef gruba bir sunucuyu ekleyebilir ancak bir elastik havuzdaki belirli veritabanlarını (veya bir havuzun tamamını) hariç tutabilirsiniz.

Hedef grupta birden fazla abonelikte ve bölgede bulunan veritabanları mevcut olabilir. Birden fazla bölgenin söz konusu olduğu yürütme işlemlerinin aynı bölgedeki yürütmelere kıyasla daha yüksek gecikme süresine sahip olacağını unutmayın.

Aşağıdaki örneklerde işin çalıştırılacağı veritabanlarının belirlenmesi için farklı hedef grubu tanımlarının iş yürütme sırasında nasıl dinamik olarak numaralandırıldığı gösterilmektedir:

![Hedef grup örnekleri](media/elastic-jobs-overview/targetgroup-examples1.png)

**Örnek 1**'de tek veritabanlarının listesini içeren bir hedef grup gösterilmektedir. Bir iş adımı bu hedef grup kullanılarak yürütüldüğünde iş adımının eylemi bu veritabanlarının her birinde yürütülür.<br>
**Örnek 2**'de hedef olarak bir Azure SQL Server içeren bir hedef grup gösterilmektedir. Bir iş adımı bu hedef grup kullanılarak yürütüldüğünde sunucudaki veritabanı listesinin belirlenmesi için sunucu dinamik olarak numaralandırılır. İş adımının eylemi bu veritabanlarının her birinde yürütülür.<br>
**Örnek 3**'te gösterilen hedef grup *Örnek 2*'dekine benzer ancak belirli veritabanları özel olarak hariç tutulmuştur. İş adımının eylemi hariç tutulan veritabanında *yürütülmez*.<br>
**Örnek 4**'te hedef olarak bir elastik havuz içeren bir hedef grup gösterilmektedir. *Örnek 2*'ye benzer şekilde havuz iş çalıştırma zamanında dinamik olarak numaralandırılarak havuzdaki veritabanlarının listesi belirlenir.
<br><br>

![Hedef grup örnekleri](media/elastic-jobs-overview/targetgroup-examples2.png)

**Örnek 5** ve **Örnek 6,** Azure SQL Sunucularının, esnek havuzların ve veritabanlarının ekle ve hariç kurallar kullanılarak birleştirilebildiği gelişmiş senaryoları gösterir.<br>
**Örnek 7**'de parça eşlemesi içinde bulunan ve iş çalıştırma zamanında değerlendirilebilecek parçalar gösterilmektedir.

> [!NOTE]
> İş veritabanının kendisi bir işin hedefi olabilir. Bu senaryoda, İş veritabanı diğer hedef veritabanı gibi işlenir. İş kullanıcısı oluşturulmalı ve İş veritabanında yeterli izinler verilmeli ve iş kullanıcısı için veritabanı kapsamı içinde kimlik bilgisi, diğer hedef veritabanında olduğu gibi İş veritabanında da bulunmalıdır.
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

- [SQL Server Agent nedir](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent)
- [Elastik işler nasıl oluşturulur ve yönetilir](elastic-jobs-overview.md)
- [PowerShell'i kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-powershell.md)
- [Transact-SQL (T-SQL) kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-tsql.md)
