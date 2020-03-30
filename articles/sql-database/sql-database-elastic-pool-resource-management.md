---
title: Yoğun elastik havuzlarda kaynak yönetimi | Microsoft Dokümanlar
description: Birçok veritabanıyla Azure SQL elastik havuzlarında bilgi işlem kaynaklarını yönetin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: carlrab
ms.date: 03/13/2019
ms.openlocfilehash: 014f9edca1706c39930c6e48bb64cd8873bcace9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473736"
---
# <a name="resource-management-in-dense-elastic-pools"></a>Yoğun elastik havuzlardaki kaynak yönetimi

Azure SQL Veritabanı [elastik havuzları,](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) değişen kaynak kullanımıyla birçok veritabanını yönetmek için uygun maliyetli bir çözümdür. Elastik bir havuzdaki tüm veritabanları, **yalnızca havuzdaki veritabanlarının bir alt kümesinin herhangi bir zamanda bilgi işlem kaynaklarını kullanacağı**varsayımıüzerine CPU, bellek, alt iş parçacıkları, depolama alanı, tempdb gibi kaynakların aynı tahsisini paylaşır. Bu varsayım, elastik havuzların uygun maliyetli olmasını sağlar. Müşteriler, her bir veritabanının ihtiyaç duyabileceği tüm kaynaklar için ödeme yapmak yerine, havuzdaki tüm veritabanları arasında paylaşılan çok daha küçük bir kaynak kümesi için ödeme yapabilir.

## <a name="resource-governance"></a>Kaynak idaresi

Kaynak paylaşımı, yüksek kaynak tüketimine sahip bir veritabanının aynı elastik havuzdaki diğer veritabanlarını etkilediği "gürültülü komşu" etkisini en aza indirmek için sistemin kaynak kullanımını dikkatle denetlemesini gerektirir. Aynı zamanda, sistemin güvenilir bir şekilde çalışması için yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR), yedekleme ve geri yükleme, izleme, Sorgu Deposu, Otomatik ayar, vb. gibi özellikler için yeterli kaynak sağlaması gerekir.

Azure SQL Veritabanı, işlem düzeyi kaynak yönetimi için Windows [İş Nesneleri,](https://docs.microsoft.com/windows/win32/procthread/job-objects) depolama kotası yönetimi için Windows [Dosya Sunucusu Kaynak Yöneticisi (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) ve Azure SQL Veritabanı'nda çalışan her SQL Server örneğinde kaynak yönetimini uygulamak için SQL Server [Resource Governor'in](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) değiştirilmiş ve genişletilmiş sürümünü kullanarak birden çok kaynak yönetim mekanizması nı kullanarak bu hedeflere ulaşır.

Elastik havuzların birincil tasarım amacı uygun maliyetli olmaktır. Bu nedenle, sistem kasıtlı olarak müşterilerin _yoğun_ havuzlar, yani yaklaşan veya izin verilen en fazla veritabanları sayısı ile havuzları oluşturmak için izin verir, ancak bilgi işlem kaynaklarının ılımlı bir tahsisi ile. Aynı nedenle, sistem iç işlemleri için gerekli olabilecek tüm kaynakları ayırmaz, ancak iç işlemler ve kullanıcı iş yükleri arasında kaynak paylaşımına izin verir.

Bu yaklaşım, müşterilerin yeterli performans ve büyük maliyet tasarrufu elde etmek için yoğun elastik havuzları kullanmanızı sağlar. Ancak, yoğun bir havuzda birçok veritabanlarına karşı iş yükü yeterince yoğun ise, kaynak çekişme önemli olur. Kaynak çekişmesi kullanıcı iş yükü performansını azaltır ve iç işlemleri olumsuz etkileyebilir.

> [!IMPORTANT]
> Birçok etkin veritabanları ile yoğun havuzlarda, [dtu](sql-database-dtu-resource-limits-elastic-pools.md) ve [vCore](sql-database-vcore-resource-limits-elastic-pools.md) elastik havuzları için belgelenen maksimumlara kadar havuzdaki veritabanlarının sayısını artırmak mümkün olmayabilir.
> 
> Kaynak çekişmesi ve performans sorunlarına neden olmadan yoğun havuzlara yerleştirilebilen veritabanlarının sayısı, aynı anda etkin olan veritabanlarının sayısına ve her veritabanındaki kullanıcı iş yüklerine göre kaynak tüketimine bağlıdır. Kullanıcı iş yükleri değiştikçe bu sayı zaman içinde değişebilir.

Kaynak çekişmesi yoğun olarak paketlenmiş bir havuzda oluştuğunda, müşteriler bunu azaltmak için aşağıdaki eylemlerden birini veya birkaçını seçebilir:
- Kaynak tüketimini azaltmak veya kaynak tüketimini zaman içinde birden çok veritabanına yaymak için sorgu iş yükünü ayarlayın.
- Bazı veritabanlarını başka bir havuza taşıyarak veya bağımsız veritabanları yaparak havuz yoğunluğunu azaltın.
- Daha fazla kaynak elde etmek için havuzu ölçeklendirin.

Son iki eylemin nasıl uygulanacağı yla ilgili öneriler için, bu makalenin ilerleyen saatlerinde [Operasyonel önerilere](#operational-recommendations) bakın. Kaynak çekişmesini azaltmak hem kullanıcı iş yüklerinden hem de dahili işlemlerden yararlanır ve sistemin beklenen hizmet düzeyini güvenilir bir şekilde korumasını sağlar.

## <a name="monitoring-resource-consumption"></a>Kaynak tüketimini izleme

Kaynak çekişmesi nedeniyle performans bozulmasını önlemek için, yoğun elastik havuzlar kullanan müşteriler kaynak tüketimini proaktif olarak izlemeli ve kaynak çekişmesini artırmaya başlarsa zamanında harekete geçmelidir. Kullanıcı iş yükündeki değişiklikler, veri hacimleri ve dağıtım değişiklikleri, havuz yoğunluğundaki değişiklikler ve Azure SQL Veritabanı hizmetindeki değişiklikler nedeniyle havuzdaki kaynak kullanımı zaman içinde değiştiğinden sürekli izleme önemlidir. 

Azure SQL Veritabanı, bu izleme türüyle ilgili birkaç ölçüm sağlar. Her ölçüm için önerilen ortalama değeri aşmak havuzdaki kaynak çekişmesini gösterir ve daha önce bahsedilen eylemlerden biri kullanılarak ele alınmalıdır.

|Ölçüm adı|Açıklama|Önerilen ortalama değer| 
|----------|--------------------------------|------------|
|`avg_instance_cpu_percent`|Altta yatan işletim sistemi tarafından ölçülen, elastik bir havuzla ilişkili SQL Server işleminin CPU kullanımı. Her veritabanında [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve veritabanındaki [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` mevcuttur. Bu metrik, azure monitörüne de yayılır ve burada [azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) `sqlserver_process_core_percent`portalında görüntülenebilir. Bu değer, aynı elastik havuzdaki her veritabanı için aynıdır.|%70'in altında. Ara sıra %90'a varan kısa ani artışlar kabul edilebilir.|
|`max_worker_percent`|[İşçi iş parçacığı]( https://docs.microsoft.com/sql/relational-databases/thread-and-task-architecture-guide) kullanımı. Havuzdaki her veritabanı nın yanı sıra havuzun kendisi için de sağlanır. Veritabanı düzeyinde ve havuz düzeyinde çalışan iş parçacığı sayısında farklı sınırlar vardır, bu nedenle her iki düzeyde de bu metrik izlenmesi önerilir. Her veritabanında [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve veritabanındaki [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` mevcuttur. Bu metrik, azure monitörüne de yayılır ve burada [azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) `workers_percent`portalında görüntülenebilir.|%80'in altında. %100'e varan artışlar bağlantı denemelerinin ve sorguların başarısız lığa neden olur.|
|`avg_data_io_percent`|Okumak ve yazmak için iOPS kullanımı fiziksel IO. Havuzdaki her veritabanı nın yanı sıra havuzun kendisi için de sağlanır. Veritabanı düzeyinde IOPS sayısında farklı sınırlar vardır ve havuz düzeyinde, bu nedenle her iki düzeyde de bu metrik izlenmesi önerilir. Her veritabanında [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve veritabanındaki [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` mevcuttur. Bu metrik, azure monitörüne de yayılır ve burada [azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) `physical_data_read_percent`portalında görüntülenebilir.|%80'in altında. Ara sıra %100'e varan kısa ani artışlar kabul edilebilir.|
|`avg_log_write_percent`|İşlem günlüğü için iş itimat kullanımları IO yazın. Havuzdaki her veritabanı nın yanı sıra havuzun kendisi için de sağlanır. Veritabanı düzeyinde günlük iş verime farklı sınırlar vardır ve havuz düzeyinde, bu nedenle her iki düzeyde bu metrik izleme önerilir. Her veritabanında [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) görünümünde ve veritabanındaki [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` mevcuttur. Bu metrik, azure monitörüne de yayılır ve burada [azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) `log_write_percent`portalında görüntülenebilir. Bu metrik %100'e yakın olduğunda, tüm veritabanı değişiklikleri (INSERT, UPDATE, DELETE, MERGE deyimleri, SELECT ... INTO, TOPLU EKLEME, vb.) daha yavaş olacaktır.|%90'ın altında. Ara sıra %100'e varan kısa ani artışlar kabul edilebilir.|
|`oom_per_second`|Bellek basıncının bir göstergesi olan elastik bir havuzdaki bellek dışı (OOM) hatalarının oranı. [sys.dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database?view=azuresqldb-current) görünümünde mevcuttur. Bu ölçüthesaplamak için örnek sorgu [örneklerine](#examples) bakın.|0|
|`avg_storage_percent`|Elastik havuz düzeyinde depolama alanı kullanımı. Veritabanında [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` mevcuttur. Bu metrik, azure monitörüne de yayılır ve burada [azure](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) `storage_percent`portalında görüntülenebilir.|%80'in altında. Veri büyümesi olmayan havuzlar için %100'e yaklaşabilirsiniz.|
|`tempdb_log_used_percent`|`tempdb` Veritabanında işlem günlüğü alanı kullanımı. Bir veritabanında oluşturulan geçici nesneler aynı elastik havuzdaki diğer veritabanlarında görünmese de, `tempdb` aynı havuzdaki tüm veritabanları için paylaşılan bir kaynaktır. Havuzdaki `tempdb` bir veritabanından başlatılan uzun süren veya boşta kalan işlem, hareket günlüğünün büyük bir bölümünü tüketebilir ve aynı havuzdaki diğer veritabanlarındaki sorgular için hatalara neden olabilir. [sys.dm_db_log_space_usage](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql) görünümünde mevcuttur. Bu metrik azure monitöre de yayılır ve Azure portalında görüntülenebilir. Bu metnin geçerli değerini döndürmek için örnek sorgu [örneklerine](#examples) bakın.|%50'nin altında. Ara sıra %80'e varan ani artışlar kabul edilebilir.|
|||

Azure SQL Veritabanı, bu ölçümlere ek olarak, gerçek kaynak yönetim sınırlarını döndüren bir görünüm ve kaynak havuzu düzeyinde ve iş yükü grubu düzeyinde kaynak kullanım istatistikleridöndüren ek görünümler sağlar.

|Adı görüntüleme|Açıklama|  
|-----------------|--------------------------------|  
|[sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database)|Geçerli veritabanında veya esnek havuzda kaynak yönetim mekanizmaları tarafından kullanılan gerçek yapılandırma ve kapasite ayarlarını verir.|
|[sys.dm_resource_governor_resource_pools](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql)|Geçerli kaynak havuzu durumu, kaynak havuzlarının geçerli yapılandırması ve birikmeli kaynak havuzu istatistikleri hakkındaki bilgileri verir.|
|[sys.dm_resource_governor_workload_groups](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql)|Birikmeli iş yükü grubu istatistiklerini ve iş yükü grubunun geçerli yapılandırmasını verir. Bu görünüm, kaynak havuzu bilgilerini almak için `pool_id` sütundaki sys.dm_resource_governor_resource_pools ile birleştirilmiş olabilir.|
|[sys.dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database)|Kaynak havuzu kullanım istatistiklerini son 32 dakikaiçin döndürür. Her satır 20 saniyelik bir aralığı temsil eder. Sütunlar `delta_` aralık sırasında her istatistikteki değişikliği döndürer.|
|[sys.dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database)|Son 32 dakika için iş yükü grubu kullanım istatistiklerini verir. Her satır 20 saniyelik bir aralığı temsil eder. Sütunlar `delta_` aralık sırasında her istatistikteki değişikliği döndürer.|
|||

Bu görünümler, kaynak kullanımını izlemek ve kaynak çekişmesini neredeyse gerçek zamanlı olarak gidermek için kullanılabilir. Coğrafi yinelemeler de dahil olmak üzere birincil ve okunabilir ikincil yinelemelerde `SloSharedPool1` kullanıcı `UserPrimaryGroup.DBId[N]` iş yükü, `N` veritabanı kimliği değerini temsil eden kaynak havuzu ve iş yükü grubuna sınıflandırılır.

Yoğun havuzlar kullanan müşteriler, geçerli kaynak kullanımını izlemeye ek olarak, geçmiş kaynak kullanım verilerini ayrı bir veri deposunda saklayabilir. Bu veriler, tarihsel ve mevsimsel eğilimlere dayalı kaynak kullanımını proaktif olarak yönetmek için tahmine dayalı analizde kullanılabilir.

## <a name="operational-recommendations"></a>Operasyonel öneriler

**Yeterli kaynak alanı bırakın.** Kaynak çekişmesi ve performans bozulması oluşursa, azaltma bazı veritabanlarının etkilenen elastik havuzdan çıkarılmasını veya daha önce belirtildiği gibi havuzun ölçeklendirilmesini içerebilir. Ancak, bu eylemler tamamlamak için ek bilgi işlem kaynakları gerektirir. Özellikle, Premium ve İş Açısından Kritik havuzlar için bu eylemler, taşınan veritabanları veya havuz ölçeklendirildiyse elastik havuzdaki tüm veritabanları için tüm verilerin aktarılmasını gerektirir. Veri aktarımı uzun süren ve kaynak yoğun bir işlemdir. Havuz zaten yüksek kaynak baskısı altındaysa, azaltıcı işlemin kendisi performansı daha da düşürür. Ekstrem durumlarda, gerekli kaynaklar kullanılamadığından, veritabanı taşıma veya havuz ölçeklendirmesi yoluyla kaynak çekişmesini çözmek mümkün olmayabilir. Bu durumda, etkilenen elastik havuzda sorgu iş yükünü geçici olarak azaltmak tek çözüm olabilir.

Yoğun havuzkullanan müşteriler, daha önce açıklandığı gibi kaynak kullanım eğilimlerini yakından izlemeli ve ölçümler önerilen aralıklar içinde kalırken ve elastik havuzda hala yeterli kaynak varken azaltıcı eylemlerde bulunmalıdır.

Kaynak kullanımı, her veritabanı ve her elastik havuz için zaman içinde değişen birden çok etkene bağlıdır. Yoğun havuzlarda optimum fiyat/performans oranının sağlanması, veritabanlarını daha çok kullanılan havuzlardan daha az kullanılan havuzlara taşımak ve artan iş yükünü karşılamak için gerektiğinde yeni havuzlar oluşturmak olmak üzere sürekli izleme ve yeniden dengeleme gerektirir.

**"Sıcak" veritabanlarını hareket ettirin.** Havuz düzeyindeki kaynak çekişmesi öncelikle az sayıda çok sayıda çok sayıda çok sayıda veritabanlarından kaynaklanıyorsa, bu veritabanlarını daha az kullanılan bir havuza taşımak veya bağımsız veritabanları yapmak cazip olabilir. Ancak, taşıma işlemi hem taşınan veritabanı için hem de tüm havuz için performansı daha da düşüreceği için, veritabanı son derece kullanılırken bunu yapmak önerilmez. Bunun yerine, yüksek kullanım azalana kadar bekleyin veya havuz düzeyinde kaynak basıncını hafifletmek için daha az kullanılan veritabanlarını taşıyın. Ancak çok düşük kullanıma sahip veritabanlarının taşınması, havuz düzeyinde kaynak kullanımını önemli ölçüde azaltmadığından, bu durumda herhangi bir fayda sağlamaz.

**Bir "karantina" havuzunda yeni veritabanları oluşturun.** Kiracı başına veritabanı modelini kullanan uygulamalar gibi yeni veritabanlarının sık sık oluşturulduğu senaryolarda, varolan bir elastik havuza yerleştirilen yeni bir veritabanının beklenmedik bir şekilde önemli kaynakları tüketme ve diğer veritabanlarını etkileme riski vardır ve havuzda iç süreçler. Bu riski azaltmak için, yeterli kaynak tahsisi içeren ayrı bir "karantina" havuzu oluşturun. Henüz bilinmeyen kaynak tüketim desenleri olan yeni veritabanları için bu havuzu kullanın. Bir veritabanı bir hafta veya bir ay gibi bir iş döngüsü için bu havuzda kaldı ktan sonra ve kaynak tüketimi biliniyorsa, bu ek kaynak kullanımını karşılamak için yeterli kapasiteye sahip bir havuza taşınabilir.

**Aşırı yoğun mantıksal sunuculardan kaçının.** Azure SQL Veritabanı, mantıksal sunucu başına en fazla 5000 [veritabanını destekler.](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server) Binlerce veritabanına sahip elastik havuzlar kullanan müşteriler, desteklenen sınıra kadar toplam veritabanları sayısıyla birlikte tek bir sunucuya birden çok elastik havuz yerleştirmeyi düşünebilir. Ancak, binlerce veritabanına sahip mantıksal sunucular operasyonel zorluklar oluşturur. Bir sunucudaki tüm veritabanlarını (örneğin portaldaki veritabanlarını görüntülemek) içeren işlemler daha yavaş olacaktır. Sunucu düzeyi oturum açmaların veya güvenlik duvarı kurallarının yanlış değiştirilmesi gibi operasyonel hatalar daha fazla sayıda veritabanını etkiler. Mantıksal sunucunun yanlışlıkla silinmesi, silinen sunucudaki veritabanlarını kurtarmak için Microsoft Desteği'nden yardım gerektirir ve etkilenen tüm veritabanları için uzun süreli bir kesintiye neden olur.

Mantıksal sunucu başına veritabanı sayısını desteklenen maksimum dan daha düşük bir sayıyla sınırlamanızı öneririz. Birçok senaryoda, sunucu başına 1000-2000 veritabanına kadar kullanmak en iyi sidir. Yanlışlıkla sunucu silme olasılığını azaltmak için, mantıksal sunucuya veya kaynak grubuna silme [kilidi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) yerleştirmenizi öneririz.

Geçmişte, veritabanlarını aynı mantıksal sunucudaki elastik havuzlara taşımayı, dışarı veya arasında taşımayı içeren belirli senaryolar, veritabanlarını mantıksal sunucular arasında hareket ettirirken olduğundan daha hızlıydı. Şu anda, tüm veritabanı hamle kaynak ve hedef mantıksal sunucu ne olursa olsun aynı hızda yürütmek.

## <a name="examples"></a>Örnekler

### <a name="monitoring-memory-utilization"></a>Bellek kullanımını izleme

Bu sorgu, `oom_per_second` son 32 dakika içinde her kaynak havuzu için metrik hesaplar. Bu sorgu, elastik bir havuzda herhangi bir veritabanında yürütülebilir.

```
SELECT pool_id,
       name AS resource_pool_name,
       IIF(name LIKE 'SloSharedPool%' OR name LIKE 'UserPool%', 'user', 'system') AS resource_pool_type,
       SUM(CAST(delta_out_of_memory_count AS decimal))/(SUM(duration_ms)/1000.) AS oom_per_second
FROM sys.dm_resource_governor_resource_pools_history_ex
GROUP BY pool_id, name
ORDER BY pool_id;
```
### <a name="monitoring-tempdb-log-space-utilization"></a>Günlük `tempdb` alanı kullanımını izleme

Bu `tempdb_log_used_percent` sorgu, ölçümün geçerli değerini döndürür. Bu sorgu, elastik bir havuzda herhangi bir veritabanında yürütülebilir.

```
SELECT used_log_space_in_percent AS tempdb_log_used_percent
FROM tempdb.sys.dm_db_log_space_usage;
```

## <a name="next-steps"></a>Sonraki adımlar

* Esnek havuzlara giriş için [bkz.](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* Kaynak kullanımını azaltmak için sorgu iş yüklerini ayarlama hakkında bilgi için [Monitoring and performance tuning](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview) [bkz.]( https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-tuning-index)
