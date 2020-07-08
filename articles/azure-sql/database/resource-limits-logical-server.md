---
title: Azure 'da mantıksal sunucular için kaynak sınırları
description: Bu makalede Azure SQL veritabanı ve Azure SYNAPSE Analytics tarafından kullanılan Azure 'daki mantıksal sunucu için kaynak sınırlarına genel bir bakış sunulmaktadır. Ayrıca, bu kaynak limitleri isabet edildiğinde veya aşıldığında ne olacağı hakkında bilgi de sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
ms.date: 06/10/2020
ms.openlocfilehash: eac5814eb977a01135ad2fcd9551b3475673dbca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84691773"
---
# <a name="resource-limits-for-azure-sql-database-and-azure-synapse-analytics-servers"></a>Azure SQL veritabanı ve Azure SYNAPSE Analytics sunucuları için kaynak sınırları
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Bu makalede, Azure SQL veritabanı ve Azure SYNAPSE Analytics tarafından kullanılan mantıksal sunucu için kaynak sınırlarına genel bir bakış sunulmaktadır. Bu kaynak limitleri isabet edildiğinde veya aşıldığında ne olacağı ve bu sınırları zorlamak için kullanılan kaynak idare mekanizmalarını açıklayan bilgiler sağlar.

> [!NOTE]
> Azure SQL yönetilen örnek sınırları için bkz. [yönetilen örnekler Için SQL veritabanı kaynak sınırları](../managed-instance/resource-limits.md).

## <a name="maximum-resource-limits"></a>En fazla kaynak sınırı

| Kaynak | Sınır |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Herhangi bir bölgedeki abonelik başına varsayılan sunucu sayısı | 20 |
| Herhangi bir bölgedeki abonelik başına en fazla sunucu sayısı | 200 |  
| Sunucu başına DTU/eDTU kotası | 54.000 |  
| sunucu/örnek başına sanal çekirdek kotası | 540 |
| Sunucu başına en fazla havuz | DTU sayısıyla veya sanal çekirdekler ile sınırlıdır. Örneğin, her havuz 1000 DTU ise, bir sunucu 54 havuzlarını destekleyebilir.|
|||

> [!IMPORTANT]
> Veritabanı sayısı, sunucu başına sınıra yaklaşırsa, şunlar olabilir:
>
> - Ana veritabanına karşı sorguları çalıştırırken gecikme süresini artırma.  Bu, sys. resource_stats gibi kaynak kullanımı istatistiklerinin görünümlerini içerir.
> - Sunucuda veritabanlarının listesini oluşturmayı içeren Yönetim işlemlerinde ve işleme portalı görüntüleme noktalarında gecikme süresini artırma.

> [!NOTE]
> Varsayılan miktardan daha fazla DTU/eDTU kotası, vCore kotası veya daha fazla sunucu almak için Azure portal yeni bir destek isteği gönderebilirsiniz. Daha fazla bilgi için bkz. [Azure SQL veritabanı Için istek kotası artışları](quota-increase-request.md).

### <a name="storage-size"></a>Depolama boyutu

Tek veritabanı kaynak depolama boyutları için, fiyatlandırma katmanı başına depolama boyutu sınırları için [DTU tabanlı kaynak sınırlarına](resource-limits-dtu-single-databases.md) veya [sanal çekirdek tabanlı kaynak sınırlarına](resource-limits-vcore-single-databases.md) bakın.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak sınırlarına ulaşıldığında ne olur?

### <a name="compute-cpu"></a>İşlem CPU 'SU

Veritabanı işlem CPU kullanımı yüksek hale geldiğinde sorgu gecikmesi artar ve sorgular bile zaman aşımına uğrar. Bu koşullar altında, sorgular hizmet tarafından kuyruğa alınabilir ve kaynaklar ücretsiz hale geldiğinde yürütme için kaynaklar sağlanır.
Yüksek işlem kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanını daha fazla işlem kaynağı sağlamak için veritabanının veya elastik havuzun işlem boyutunu artırma. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](elastic-pool-scale.md).
- Her bir sorgunun CPU kaynak kullanımını azaltmak için sorguları en iyi duruma getirme. Daha fazla bilgi için bkz. [sorgu ayarlama/Ipuçcu](performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı en büyük boyut sınırına ulaştığında, veri boyutunu artıran veritabanı eklemeleri ve güncelleştirmeleri [hata iletisi](troubleshoot-common-errors-issues.md)alır. SELECT ve DELETE deyimleri başarılı olmaya devam eder.

Yüksek alan kullanımıyla karşılaşdığınızda, risk azaltma seçenekleri şunlardır:

- Veritabanının veya elastik havuzun en büyük boyutunu artırma veya daha fazla depolama alanı ekleme. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](elastic-pool-scale.md).
- Veritabanı elastik bir havuztasa alternatif olarak, depolama alanı diğer veritabanlarıyla paylaşılmaması için veritabanı havuzun dışına taşınabilir.
- Kullanılmayan alanı geri kazanmak için bir veritabanını daraltın. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda dosya alanını yönetme](file-space-manage.md)

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanlar (istekler)

En fazla oturum ve çalışan sayısı, hizmet katmanı ve işlem boyutu (DTU/eDTU veya Vçekirdekler) tarafından belirlenir. Oturum veya çalışan sınırlarına ulaşıldığında yeni istekler reddedilir ve istemciler bir hata iletisi alır. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebileceği sürece, eşzamanlı çalışanların sayısı genellikle tahmine ve denetime göre daha zordur. Bu özellikle, veritabanı kaynak sınırlarına ulaşıldığında ve çalışanlar daha uzun süre çalışan sorgular, büyük blok zincirler veya aşırı sorgu paralelliği nedeniyle oluşturulan yoğun yük dönemlerinde geçerlidir.

Yüksek oturum veya çalışan kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik havuzun hizmet katmanını veya işlem boyutunu artırma. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](elastic-pool-scale.md).
- Artan çalışan kullanımının nedeni, işlem kaynakları için çekişme nedeniyle, her bir sorgunun kaynak kullanımını azaltmak için sorguları en iyi duruma getirme. Daha fazla bilgi için bkz. [sorgu ayarlama/Ipuçcu](performance-guidance.md#query-tuning-and-hinting).
- [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines) (en fazla paralellik derecesi) ayarı azaltıyor.
- Yineleme sayısını ve sorgu engelleme süresini azaltmak için sorgu iş yükünü iyileştirme.

### <a name="memory"></a>Bellek

Diğer kaynakların aksine (CPU, çalışan, depolama), bellek sınırına ulaşmak, sorgu performansını olumsuz yönde etkilemez ve hatalara ve hatalara neden olmaz. [Bellek yönetimi mimarisi Kılavuzu](https://docs.microsoft.com/sql/relational-databases/memory-management-architecture-guide)'nda ayrıntılı olarak açıklandığı gibi, SQL Server veritabanı altyapısı, tasarım yoluyla genellikle kullanılabilir tüm belleği kullanır. Bellek, daha pahalı depolama erişiminin önüne geçmek için öncelikle verileri önbelleğe almak için kullanılır. Bu nedenle, daha yüksek bellek kullanımı genellikle depolama 'dan daha yavaş okuma yerine bellekten daha hızlı okuma nedeniyle sorgu performansını geliştirir.

Veritabanı altyapısı başlatıldıktan sonra, iş yükü depolamadan veri okumaya başladığı için veritabanı altyapısı verileri bellekte saklar. Bu ilk ayarlama süresinden sonra, `avg_memory_usage_percent` ve `avg_instance_memory_percent` [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ' deki ve sütunlarının %100 ' e eşit veya daha büyük olması beklenir. Bu, özellikle boşta olmayan veritabanları için ve belleğe tamamen sığmıyor.

Veri önbelleğinin yanı sıra, bellek veritabanı altyapısının diğer bileşenlerinde de kullanılır. Bellek için talep olduğunda ve kullanılabilir tüm bellek veri önbelleği tarafından kullanılıyorsa, veritabanı altyapısı belleği diğer bileşenlere kullanılabilir hale getirmek için veri önbelleği boyutunu dinamik olarak küçültir ve diğer bileşenler belleği serbest bırakrken veri önbelleğini dinamik olarak büyütürlerdir.

Nadir durumlarda, yeterince yoğun bir iş yükü yetersiz bellek koşuluna neden olabilir ve bu da bellek dışı hatalara yol açabilir. Bu durum, %0 ile %100 arasında herhangi bir bellek kullanımında gerçekleşebilir. Bu, daha büyük bir daha küçük bilgi işlem boyutlarında ve/veya [yoğun elastik havuzlarda](elastic-pool-resource-management.md)olduğu gibi sorgu işleme için daha fazla bellek kullanan iş yükleriyle meydana gelir.

Yetersiz bellek hatalarıyla karşılaşdığınızda, risk azaltma seçenekleri şunlardır:
- Veritabanı veya elastik havuzun hizmet katmanını veya işlem boyutunu artırma. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](elastic-pool-scale.md).
- Bellek kullanımını azaltmak için sorguları ve yapılandırmayı en iyi duruma getirme. Ortak çözümler aşağıdaki tabloda açıklanmıştır.

|Çözüm|Açıklama|
| :----- | :----- |
|Bellek izin verdiği boyutu azaltma|Bellek onayları hakkında daha fazla bilgi için bkz. [SQL Server belleği vermeyi anlama](https://techcommunity.microsoft.com/t5/sql-server/understanding-sql-server-memory-grant/ba-p/383595) blog gönderisi. Aşırı büyük bellek onayları önlemeye yönelik yaygın bir çözüm, [İstatistikleri](https://docs.microsoft.com/sql/relational-databases/statistics/statistics) güncel tutmaktan sorumludur. Bu, sorgu altyapısı tarafından bellek tüketiminin daha doğru tahminlerine neden olur, böylece gereksiz büyük bellek onayları önlenir.</br></br>Uyumluluk düzeyi 140 ve üzeri kullanan veritabanlarında veritabanı altyapısı, [toplu iş modu bellek verme geri bildirimi](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?view=sql-server-ver15#batch-mode-memory-grant-feedback)'ni kullanarak bellek verme boyutunu otomatik olarak ayarlayabilir. Uyumluluk düzeyi 150 ve üstünü kullanan veritabanlarında, veritabanı altyapısı benzer şekilde daha yaygın satır modu sorguları için [satır modu bellek verme geri bildirimi](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?view=sql-server-ver15#row-mode-memory-grant-feedback)kullanır. Bu yerleşik işlevsellik, gereksiz büyük bellek izni nedeniyle yetersiz bellek hatalarından kaçınmaya yardımcı olur.|
|Sorgu planı önbelleğinin boyutunu küçültün|Veritabanı altyapısı, her sorgu yürütmesi için bir sorgu planı derlenmesini önlemek üzere sorgu planlarını bellekte önbelleğe alır. Yalnızca bir kez kullanılan önbelleğe alma planlarını nedeniyle sorgu planı önbelleği blobunun neden olduğu bir engel olmak için, [veritabanı kapsamındaki OPTIMIZE_FOR_AD_HOC_WORKLOADS yapılandırmasını](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql)etkinleştirin.|
|Kilitleme belleği boyutunu azaltma|Veritabanı altyapısı, [kilitler](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide#Lock_Engine)için bellek kullanır. Mümkün olduğunda, çok sayıda kilit elde edebilir ve yüksek kilit belleği tüketimine neden olabilecek büyük işlemlerden kaçının.|


## <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>Kullanıcı iş yükleri ve iç işlemlere göre kaynak tüketimi

Her bir veritabanındaki kullanıcı iş yükleri tarafından CPU ve bellek tüketimi, [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) ve [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) görünümlerinde, `avg_cpu_percent` ve `avg_memory_usage_percent` sütunlarında raporlanır. Elastik havuzlar için, [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde havuz düzeyi kaynak tüketimi raporlanır. Kullanıcı iş yükü CPU tüketimi `cpu_percent` , [tek veritabanları](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) ve havuz düzeyindeki [elastik havuzlar](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) için Azure izleyici ölçümü aracılığıyla da raporlanır.

Azure SQL veritabanı, yüksek kullanılabilirlik ve olağanüstü durum kurtarma, veritabanı yedekleme ve geri yükleme, izleme, sorgu deposu, otomatik ayarlama vb. gibi çekirdek hizmet özelliklerini uygulamak için işlem kaynakları gerektirir. Sistem, [kaynak idare](#resource-governance) mekanizmalarını kullanarak bu iç işlemlere ait belirli bir sınırlı kısmını, kaynakların geri kalanını Kullanıcı iş yükleri için kullanılabilir hale getirir. İç işlemlerin işlem kaynaklarını kullanmayan zamanlarda, sistem bunları Kullanıcı iş yükleri için kullanılabilir hale getirir.

Kullanıcı iş yükleri ve iç işlemlere göre toplam CPU ve bellek tüketimi, [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) ve [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) görünümlerinde, `avg_instance_cpu_percent` ve `avg_instance_memory_percent` sütunlarında raporlanır. Bu veriler aynı zamanda `sqlserver_process_core_percent` ve `sqlserver_process_memory_percent` Azure izleyici ölçümleri aracılığıyla, [tek veritabanları](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) ve havuz düzeyindeki [elastik havuzlar](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) aracılığıyla da raporlanır.

Kullanıcı iş yükleri ve iç işlemlere göre son kaynak tüketiminin daha ayrıntılı bir dökümü, [sys. dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database) ve [sys. dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database) görünümlerinde raporlanır. Bu görünümlerde başvurulan kaynak havuzlarıyla ve iş yükü gruplarıyla ilgili ayrıntılar için bkz. [Resource idare](#resource-governance). Bu görünümler, Kullanıcı iş yükleri ve ilişkili kaynak havuzları ve iş yükü gruplarındaki belirli iç süreçler tarafından kaynak kullanımını raporlar.

Performans izleme ve sorun giderme bağlamında, Kullanıcı **user CPU consumption** `avg_cpu_percent` `cpu_percent` iş yükleri ve iç işlemlere (,) göre hem Kullanıcı CPU kullanımını (,) hem de **toplam CPU tüketimini** göz önünde bulundurmanız önemlidir `avg_instance_cpu_percent` `sqlserver_process_core_percent` .

**Kullanıcı CPU tüketimi** , her hizmet hedefi için Kullanıcı iş yükü sınırlarının yüzdesi olarak hesaplanır. %100 ' deki **Kullanıcı CPU kullanımı** , Kullanıcı iş yükünün hizmet hedefi sınırına ulaşmış olduğunu gösterir. Ancak, **toplam CPU tüketimi** % 70-100 aralığına ulaştığında, BILDIRILEN **Kullanıcı CPU tüketimi** %100 ' nin altında önemli olmaya devam ediyor olsa da, Kullanıcı iş yükü aktarım hızını genişletme ve sorgu gecikme süresinin artabilmesi mümkündür. Bu durum, yoğun bir işlem kaynakları ayırması ile daha küçük hizmet hedefleri kullanılırken oluşma olasılığını daha yüksektir, ancak [yoğun elastik havuzlarda](elastic-pool-resource-management.md)olduğu gibi nispeten daha yoğun Kullanıcı iş yükleri. Bu, örneğin, veritabanının yeni bir çoğaltmasını oluştururken, iç süreçler geçici olarak ek kaynaklar gerektirdiğinde daha küçük hizmet hedefleriyle da oluşabilir.

**Toplam CPU tüketimi** yüksek olduğunda, azaltma seçenekleri daha önce bahsedilen ve hizmet hedefi artışını ve/veya Kullanıcı iş yükü iyileştirmesini içerir.

## <a name="resource-governance"></a>Kaynak idaresi

Azure SQL veritabanı, kaynak sınırlarını zorlamak için, Azure SQL veritabanı 'nda çalışmak üzere SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor), değiştirilmiş ve genişletilmiş bir kaynak idare uygulamasını kullanır. SQL veritabanı 'nda, kaynak limitlerinin her iki havuzda ve grup düzeyinde ayarlandığı birden çok [kaynak havuzu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-resource-pool) ve [iş yükü grubu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-workload-group), [bir hizmet olarak dengeli bir veritabanı](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/)sağlar. Kullanıcı iş yükü ve iç iş yükleri ayrı kaynak havuzları ve iş yükü grupları halinde sınıflandırılır. Coğrafi çoğaltmalar dahil olmak üzere birincil ve okunabilir ikincil çoğaltmalarda Kullanıcı iş yükü, `SloSharedPool1` `UserPrimaryGroup.DBId[N]` `N` veritabanı kimliği değerini temsil eden kaynak havuzu ve iş yükü grubu olarak sınıflandırılmaktadır. Ayrıca, çeşitli iç iş yükleri için birden fazla kaynak havuzu ve iş yükü grubu vardır.

Azure SQL veritabanı, SQL işlemindeki kaynakları yönetmek için Resource Governor kullanmanın yanı sıra işlem düzeyi kaynak idaresi için Windows [Iş nesnelerini](https://docs.microsoft.com/windows/win32/procthread/job-objects) ve depolama kotası yönetimi Için Windows [dosya sunucusu Kaynak Yöneticisi (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) kullanır.

Azure SQL veritabanı kaynak İdaresi, doğası gereği hiyerarşik bir şekilde yapılır. Üstten alta kadar, işletim sistemi kaynak idare mekanizmaları ve Resource Governor ve ardından Resource Governor kullanılarak kaynak havuzu düzeyinde ve sonra da Resource Governor kullanarak iş yükü grubu düzeyinde, sınırlar işletim sistemi düzeyinde ve depolama birimi düzeyinde zorlanır. Geçerli veritabanı veya elastik havuz için yürürlükte olan kaynak idare limitleri, [sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) görünümünde ortaya çıkmış.

### <a name="data-io-governance"></a>Veri GÇ yönetimi

Veri GÇ yönetimi, Azure SQL veritabanı 'nda, hem okuma hem de yazma fiziksel GÇ 'yi bir veritabanının veri dosyalarına göre sınırlandırmak için kullanılan bir işlemdir. "Gürültülü komşu" efektini en aza indirmek, çok kiracılı hizmette kaynak ayırma eşitliği sağlamak ve temel alınan donanımın ve depolamanın özellikleri dahilinde kalmak için her bir hizmet düzeyi için ıOPS sınırları ayarlanır.

Tek veritabanları için, iş yükü grubu sınırları veritabanına göre tüm depolama GÇ kaynaklarına uygulanır, ancak kaynak havuzu limitleri, veritabanı dahil olmak üzere aynı SQL havuzundaki tüm veritabanlarına yönelik tüm depolama ıO 'larda geçerlidir `tempdb` . Elastik havuzlar için iş yükü grubu sınırları havuzdaki her bir veritabanı için geçerlidir, ancak kaynak havuzu sınırı, `tempdb` havuzdaki tüm veritabanları arasında paylaşılan veritabanı da dahil olmak üzere tüm elastik havuz için geçerlidir. Genel olarak, kaynak havuzu limitleri bir veritabanında (tek veya havuza alınmış) iş yükü tarafından ulaşılabilir olamaz, çünkü iş yükü grubu sınırları kaynak havuzu limitinden daha düşüktür ve ıOPS/aktarım hızını daha erken sınırlayın. Ancak, havuz sınırlarına aynı havuzdaki birden fazla veritabanına karşı Birleşik iş yükü tarafından erişilebilir.

Örneğin, bir sorgu herhangi bir GÇ kaynak İdaresi olmadan 1000 ıOPS oluşturursa, ancak iş yükü grubu maksimum ıOPS sınırı 900 ıOPS olarak ayarlanırsa, sorgu en fazla 900 ıOPS oluşturamayacak. Ancak, kaynak havuzu maksimum ıOPS sınırı 1500 ıOPS olarak ayarlanmışsa ve kaynak havuzuyla ilişkili tüm iş yükü gruplarından gelen toplam GÇ 1500 ıOPS 'yi aşarsa, aynı sorgunun GÇ değeri, 900 ıOPS çalışma grubu sınırının altında azaltılabilir.

[Sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) görünümü tarafından döndürülen IOPS ve aktarım hızı/maksimum değerleri, garanti olarak değil, limit/Caps olarak davranır. Ayrıca, kaynak İdaresi belirli bir depolama gecikmesini garanti etmez. Belirli bir kullanıcı iş yükü için en iyi ulaşılabilir gecikme süresi, ıOPS ve aktarım hızı yalnızca GÇ kaynak idare sınırlarına, ayrıca kullanılan GÇ boyutlarının karışımına ve temel depolamanın özelliklerine göre değişir. SQL veritabanı, 512 KB ve 4 MB arasında bir boyut farklılık gösteren IOs kullanır. IOPS sınırlarını zorlama amaçları doğrultusunda, Azure Storage 'da veri dosyaları olan veritabanlarının dışında, her GÇ boyutundan bağımsız olarak hesaba katılmaz. Bu durumda, Azure Storage ıO Accounting ile hizalamak için 256 KB 'den büyük olan IOs birden fazla 256 KB 'lık IOs olarak hesaba katılmaz.

Azure depolama 'daki veri dosyalarını kullanan temel, standart ve Genel Amaçlı veritabanları için, `primary_group_max_io` bir veritabanında bu IOPS sayısını birikecek şekilde yeterli veri dosyası yoksa veya veriler dosyalar arasında eşit olarak dağıtılmadıysa veya temeldeki Blobların performans katmanı kaynak idare sınırının altındakı IOPS/aktarım hızını sınırlıyorsa, değer ulaşılabilir olmayabilir. Benzer şekilde, sık işlem işleme tarafından oluşturulan küçük günlük IOs ile, `primary_max_log_rate` temel Azure depolama BLOBUNUN IOPS sınırı nedeniyle değer bir iş yükü tarafından ulaşılabilir olmayabilir.

`avg_data_io_percent` `avg_log_write_percent` [Sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)ve [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümlerinde bildirilen ve gibi kaynak kullanımı değerleri, maksimum kaynak idare limitlerinin yüzdesi olarak hesaplanır. Bu nedenle, kaynak yönetimi, ıOPS/aktarım hızı dışındaki faktörler olduğunda, bildirilen kaynak kullanımı %100 altında kalsa da, iş yükünün arttığı sırada ıOPS/verimlilik düzleştirme ve gecikme sürelerini görmek mümkündür.

Veritabanı dosyası başına okuma ve yazma ıOPS, aktarım hızı ve gecikme süresini görmek için [sys. dm_io_virtual_file_stats ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) işlevini kullanın. Bu işlev, öğesine göre hesaba dahil olmayan arka plan GÇ da dahil olmak üzere veritabanına karşı tüm GÇ yüzeylerini, `avg_data_io_percent` ancak temel DEPOLAMANıN IOPS ve aktarım hızını kullanır ve gözlemlenen depolama gecikmesini etkileyebilir. İşlevi, `io_stall_queued_read_ms` sırasıyla ve sütunlarında, okuma ve yazma işlemleri IÇIN GÇ kaynak İdaresi tarafından kullanıma sunulmuş ek gecikme süresini de yüzey halinde gösterir `io_stall_queued_write_ms` .

### <a name="transaction-log-rate-governance"></a>İşlem günlüğü oranı idare

İşlem günlüğü oranı yönetimi, toplu ekleme, seçme ve dizin oluşturma gibi iş yüklerinin yüksek alım tarifelerinin sınırlandırılmasına yönelik Azure SQL veritabanı 'nda kullanılan bir işlemdir. Bu sınırlar, veri dosyalarına göre kaç IOs yayımlanabileceğine bakılmaksızın iş verimini sınırlayan, alt ikinci düzeyde günlük kaydı oluşturma hızına göre izlenir ve zorlanır.  İşlem günlüğü oluşturma ücretleri Şu anda, sanal çekirdek satın alma modeliyle maksimum günlük hızı 96 MB/sn olmasına izin verilen, donanıma bağımlı olan bir noktaya göre ölçeklendirilmeye göre ölçeklenebilir.

> [!NOTE]
> Gerçek fiziksel IOs işlem günlüğü dosyalarına yönetilmez veya sınırlı değildir.

Günlük ücretleri, çeşitli senaryolarda elde edilebilecekleri ve sürekli olarak kullanılabilir, ancak genel sistem kendi işlevlerini Kullanıcı yüküne en aza indirilerek koruyabileceğinden bu şekilde ayarlanır. Günlük hızı yönetimi, işlem günlüğü yedeklemelerinin yayımlanan kurtarmalar içinde kalmasını sağlar.  Bu idare, ikincil çoğaltmalarda aşırı biriktirme listesini de önler.

Günlük kayıtları üretildiğinden, her işlem değerlendirilir ve en fazla istenen günlük hızının (saniyede MB/s) korunması için geciktirilmesi gerekip gerekmediğini değerlendirirler. Günlük kayıtları depoya boşaltıldığınızda gecikmeler eklenmez, bunun yerine günlük hızı oluşturma sırasında günlük hızı Yönetimi uygulanır.

Çalışma zamanında uygulanan gerçek günlük oluşturma ücretleri ayrıca, geri bildirim mekanizmalarından etkilenerek, sistemin sabitlenebilmesi için izin verilen günlük hızlarını geçici olarak azaltabilirsiniz. Günlük dosyası alanı yönetimi, oturum açma alanı koşullarını ve kullanılabilirlik grubu çoğaltma mekanizmalarını çalıştırmanın genel sistem sınırlarını geçici olarak azalmasını önleme.

Günlük hızı idarecisi trafik şekillendirme, aşağıdaki bekleme türleri aracılığıyla ortaya çıkarılır ( [sys. dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys. dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) görünümlerinde gösterilir):

| Bekleme türü | Notlar |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Veritabanı sınırlandırma |
| POOL_LOG_RATE_GOVERNOR | Havuz sınırlandırma |
| INSTANCE_LOG_RATE_GOVERNOR | Örnek düzeyi sınırlandırma |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Geri bildirim denetimi, Premium 'da kullanılabilirlik grubu fiziksel çoğaltma ve İş Açısından Kritik güncel değil |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Bir günlük alanı koşulunu önlemek için geri bildirim denetimi, oranları sınırlandırma |
| HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Coğrafi çoğaltma geri bildirim denetimi, yüksek veri gecikmesini önlemek ve coğrafi ikincil öğeler kullanılamamak için günlük hızını sınırlandırma|
|||

İstenen ölçeklenebilirliği gösteren bir günlük hızı sınırı ile karşılaşıldığında, aşağıdaki seçenekleri göz önünde bulundurun:

- En yüksek 96 MB/sn günlük hızını almak veya farklı bir hizmet katmanına geçiş yapmak için daha yüksek bir hizmet düzeyine kadar ölçeklendirin. [Hiper ölçek](service-tier-hyperscale.md) hizmeti katmanı, seçilen hizmet düzeyinden bağımsız olarak 100 MB/s günlük hızı sağlar.
- Yüklenen veriler geçici ise (örneğin, bir ETL işleminde veri hazırlama gibi), tempdb 'ye yüklenebilir (en az bir şekilde günlüğe kaydedilir).
- Analitik senaryolar için, kümelenmiş bir columnstore kapsamına alınmış bir tabloya yükleyin. Bu, sıkıştırma nedeniyle gerekli günlük oranını azaltır. Bu teknik, CPU kullanımını artırır ve yalnızca kümelenmiş columnstore dizinlerinden faydalan veri kümelerine uygulanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- DTU 'Lar ve eDTU 'lar hakkında daha fazla bilgi için bkz. [DTU ve eDTU](purchasing-models.md#dtu-based-purchasing-model).
- Tempdb boyut limitleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
 