---
title: Yoğun elastik havuzlarda kaynak yönetimi | Microsoft Docs
description: Azure SQL elastik havuzlarındaki işlem kaynaklarını birçok veritabanı ile yönetin.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: carlrab
ms.date: 11/18/2019
ms.openlocfilehash: 4ce00743f6b77e6ac3e672e0ebce1e5eafc8235d
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74187195"
---
# <a name="resource-management-in-dense-elastic-pools"></a>Yoğun elastik havuzlardaki kaynak yönetimi

Azure SQL veritabanı [elastik havuzları](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) , farklı kaynak kullanımıyla birçok veritabanını yönetmeye yönelik ekonomik bir çözümdür. Elastik havuzdaki tüm veritabanları CPU, bellek, çalışan iş parçacıkları, depolama alanı, tempdb gibi kaynakların aynı ayırmasını paylaşır, bu da havuzdaki veritabanlarının yalnızca bir alt kümesinin belirli bir zamanda işlem kaynaklarını kullanacağı varsayımına dayanır. Bu varsayım, esnek havuzların düşük maliyetli olmasına olanak sağlar. Müşteriler, her veritabanı için potansiyel olarak gerek duyduğu tüm kaynaklar için ödeme yapmak yerine, havuzdaki tüm veritabanları arasında paylaşılan çok daha küçük bir kaynak kümesi için ödeme yapar.

## <a name="resource-governance"></a>Kaynak idaresi

Kaynak paylaşımı, yüksek kaynak tüketimine sahip bir veritabanının aynı elastik havuzdaki diğer veritabanlarını etkilediği "gürültülü komşu" efektini en aza indirmek için sistemin kaynak kullanımını dikkatle denetlemesini gerektirir. Aynı zamanda sistem, yüksek kullanılabilirlik ve olağanüstü durum kurtarma (HADR), yedekleme ve geri yükleme, izleme, sorgu deposu ve otomatik olarak ayarlama gibi özellikler için yeterli kaynak sağlamalıdır.

Azure SQL veritabanı, işlem düzeyi kaynak yönetimine yönelik Windows [Iş nesneleri](https://docs.microsoft.com/windows/win32/procthread/job-objects) , depolama kotası yönetimi Için Windows [dosya sunucusu Kaynak Yöneticisi (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) ve Azure SQL veritabanı 'nda çalışan her bir SQL Server örneği içinde kaynak yönetimini uygulamak üzere SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) değiştirilmiş ve genişletilmiş bir sürümünü kullanarak bu hedeflere erişir.

Elastik havuzların tasarım hedefi, uygun maliyetli olmalıdır. Bu nedenle, sistem kasıtlı olarak müşterilerin _yoğun_ havuzlar oluşturmalarına olanak tanır. Bu, izin verilen en yüksek düzeyde veya en fazla bir işlem kaynakları ayırması ile gelen veritabanları sayısını içeren havuzlarıdır. Aynı nedenden dolayı, sistem iç işlemlerinde gerekli olabilecek tüm kaynakları ayırmaz, ancak iç süreçler ve Kullanıcı iş yükleri arasında kaynak paylaşımına izin verir.

Bu yaklaşım, müşterilerin yeterli performans ve büyük maliyet tasarrufları elde etmek için yoğun elastik havuzlar kullanmasına olanak sağlar. Ancak, yoğun bir havuzdaki veritabanlarına karşı iş yükü yeterince yoğun ise kaynak çakışması önemli hale gelir. Kaynak Çekişmesi, Kullanıcı iş yükü performansını azaltır ve iç süreçlerini olumsuz yönde etkileyebilir.

Daha seyrek paketlenmiş bir havuzda kaynak çekişmesi gerçekleştiğinde, müşteriler bunu azaltmak için aşağıdaki eylemlerden birini veya daha fazlasını seçebilir:
- Kaynak tüketimini azaltmak için sorgu iş yükünü ayarlayın.
- Bazı veritabanlarını başka bir havuza taşıyarak veya tek başına veritabanları yaparak havuz yoğunluğunu azaltın.
- Daha fazla kaynak almak için havuzu ölçeklendirin.

Son iki eylemin nasıl uygulanacağı hakkında öneriler için, bu makalenin ilerleyen kısımlarında bulunan [işletimsel öneriler](#operational-recommendations) bölümüne bakın. Kaynak çekişmelerinin her ikisi de Kullanıcı iş yüklerini ve iç süreçlerini azaltır ve sistemin beklenen hizmet düzeyini güvenilir bir şekilde korumasını sağlar.

## <a name="monitoring-resource-consumption"></a>İzleme kaynak tüketimi

Kaynak çakışması nedeniyle performans düşüşünü önlemek için yoğun elastik havuzlar kullanan müşterilerin kaynak tüketimini önceden izlemesi ve kaynak çekişmesinin artması iş yüklerini etkilemeye başlarsa işlem yapması gerekir. Kullanıcı iş yükünde yapılan değişiklikler, veri birimlerindeki değişiklikler, havuz yoğunluğu 'ndaki değişiklikler ve SQL Server veritabanı altyapısındaki değişiklikler nedeniyle, bir havuzdaki kaynak kullanımı zaman içinde değiştiği için sürekli izleme önemlidir. 

Azure SQL veritabanı, bu tür izlemelerle ilgili birkaç ölçüm sağlar. Her ölçüm için önerilen ortalama değeri aşmamak, havuzdaki kaynak çekişmesini gösterir ve daha önce bahsedilen eylemlerden biri kullanılarak değinilmesi gerekir.

|Ölçüm adı|Açıklama|Önerilen ortalama değer| 
|----------|--------------------------------|------------|
|`avg_instance_cpu_percent`|Temel alınan işletim sistemiyle ölçülen bir elastik havuz ile ilişkili SQL Server işleminin CPU kullanımı. Her veritabanındaki [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve `master` veritabanındaki [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve burada `sqlserver_process_core_percent`[adlandırılır](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) ve Azure Portal görüntülenebilir. Bu değer, aynı elastik havuzdaki her veritabanı için aynıdır.|%70 altında. %90 ' e kadar zaman zaman kısa artışlar kabul edilebilir.|
|`max_worker_percent`|[Çalışan iş parçacığı]( https://docs.microsoft.com/sql/relational-databases/thread-and-task-architecture-guide) kullanımı. Havuzdaki her veritabanı için ve, havuzun kendisi için de sunulur. Veritabanı düzeyindeki çalışan iş parçacıklarının sayısında ve havuz düzeyinde farklı sınırlamalar vardır. bu nedenle, bu ölçümün her iki düzeyde de izlenmesi önerilir. Her veritabanındaki [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve `master` veritabanındaki [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve burada `workers_percent`[adlandırılır](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) ve Azure Portal görüntülenebilir.|%80 altında. %100 ' e kadar olan ani artışlar bağlantı girişimlerinin ve sorguların başarısız olmasına neden olur.|
|`avg_data_io_percent`|Okuma ve yazma fiziksel GÇ için ıOPS kullanımı. Havuzdaki her veritabanı için ve, havuzun kendisi için de sunulur. Veritabanı düzeyindeki ıOPS sayısı ve havuz düzeyinde farklı sınırlamalar vardır, bu nedenle bu ölçümün her iki düzeyde de izlenmesi önerilir. Her veritabanındaki [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) görünümünde ve `master` veritabanındaki [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve burada `physical_data_read_percent`[adlandırılır](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) ve Azure Portal görüntülenebilir.|%80 altında. %100 ' e kadar zaman zaman kısa artışlar kabul edilebilir.|
|`avg_log_write_percent`|İşlem günlüğü yazma GÇ için üretilen iş yükü belirlemeleri. Havuzdaki her veritabanı için ve, havuzun kendisi için de sunulur. Veritabanı düzeyinde ve havuz düzeyinde günlük verimlilik üzerinde farklı sınırlar vardır, bu nedenle bu ölçümün her iki düzeyde de izlenmesi önerilir. Her veritabanındaki [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) görünümünde ve `master` veritabanındaki [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve burada `log_write_percent`[adlandırılır](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) ve Azure Portal görüntülenebilir. Bu ölçüm %100 ' e yakın olduğunda, tüm veritabanı değişiklikleri (INSERT, UPDATE, DELETE, MERGE deyimleri, SELECT... INTO, BULK INSERT vb.) daha yavaş olacaktır.|%90 altında. %100 ' e kadar zaman zaman kısa artışlar kabul edilebilir.|
|`oom_per_second`|Bellek baskısı göstergesi olan elastik havuzda bellek dışı (OOM) hatalarının hızı. [Sys. dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database?view=azuresqldb-current) görünümünde kullanılabilir. Bu ölçümü hesaplamak için örnek bir sorgu [örneklerine](#examples) bakın.|0|
|`avg_storage_percent`|Esnek havuz düzeyinde depolama alanı kullanımı. [Sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde `master` veritabanında kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve burada `storage_percent`[adlandırılır](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) ve Azure Portal görüntülenebilir.|%80 altında. Veri büyümesi olmayan havuzlar için %100 yaklaşımda bulunabilir.|
|`tempdb_log_used_percent`|`tempdb` veritabanında işlem günlüğü alanı kullanımı. Bir veritabanında oluşturulan geçici nesneler aynı elastik havuzdaki diğer veritabanlarında görünmese de, `tempdb` aynı havuzdaki tüm veritabanları için paylaşılan bir kaynaktır. Havuzdaki bir veritabanından başlatılan `tempdb` uzun süre çalışan veya boşta bir işlem, işlem günlüğünün büyük bir kısmını tüketebilir ve aynı havuzdaki diğer veritabanlarındaki sorgular için hatalara neden olabilir. [Sys. dm_db_log_space_usage](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-log-space-usage-transact-sql) görünümünde kullanılabilir. Bu ölçüm Ayrıca Azure Izleyici 'ye de yayılır ve Azure portal ' de görüntülenebilir. Bu ölçümün geçerli değerini döndürmek için örnek bir sorgu [örneklerine](#examples) bakın.|%50 altında. %80 ' e kadar zaman zaman artışlar kabul edilebilir.|
|||

Azure SQL veritabanı, bu ölçümlere ek olarak, gerçek kaynak idare sınırlarını döndüren bir görünüm ve kaynak havuzu düzeyinde kaynak kullanımı istatistikleri döndüren görünümler ve iş yükü grubu düzeyinde bir görünüm sağlar.

|Görünüm adı|Açıklama|  
|-----------------|--------------------------------|  
|[sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database)|Geçerli veritabanında veya elastik havuzda kaynak idare mekanizmaları tarafından kullanılan gerçek yapılandırma ve kapasite ayarlarını döndürür.|
|[sys.dm_resource_governor_resource_pools](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql)|Geçerli kaynak havuzu durumu, Kaynak havuzlarının geçerli yapılandırması ve toplu kaynak havuzu istatistikleri hakkındaki bilgileri döndürür.|
|[sys.dm_resource_governor_workload_groups](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql)|Toplu iş yükü grubu istatistiklerini ve iş yükü grubunun geçerli yapılandırmasını döndürür. Bu görünüm, kaynak havuzu bilgilerini almak için `pool_id` sütununda sys. dm_resource_governor_resource_pools ile eklenebilir.|
|[sys. dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database)|Son 32 dakika boyunca kaynak havuzu kullanım istatistiklerini döndürür. Her satır 20 saniyelik bir aralığı temsil eder. `delta_` sütunları, Aralık sırasında her istatistikteki değişikliği döndürür.|
|[sys. dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database)|Son 32 dakika için iş yükü grubu kullanım istatistiklerini döndürür. Her satır 20 saniyelik bir aralığı temsil eder. `delta_` sütunları, Aralık sırasında her istatistikteki değişikliği döndürür.|
|||

Bu görünümler, kaynak kullanımını izlemek ve neredeyse gerçek zamanlı kaynak çekişmesini gidermek için kullanılabilir. Coğrafi çoğaltmalar dahil olmak üzere birincil ve okunabilir ikincil çoğaltmalarda Kullanıcı iş yükü, `N` veritabanı KIMLIĞI değeri için temsil edilen `SloSharedPool1` kaynak havuzuna ve `UserPrimaryGroup.DBId[N]` iş yükü grubuna sınıflandırılmaktadır.

Güncel kaynak kullanımını izlemenin yanı sıra, yoğun havuzlar kullanan müşteriler geçmiş kaynak kullanımı verilerini ayrı bir veri deposunda koruyabilir. Bu veriler, kaynak kullanımını geçmiş ve dönemsel eğilimler temelinde proaktif olarak yönetmek için tahmine dayalı çözümlemede kullanılabilir.

## <a name="operational-recommendations"></a>İşletimsel öneriler

**Yeterli kaynak headodsını bırakın**. Kaynak Çekişmesi ve performans düşüşü gerçekleşirse, risk azaltma, bazı veritabanlarının etkilenen elastik havuzdan dışına taşınmasını veya daha önce belirtildiği gibi havuzu ölçeklendirmesini gerektirebilir. Ancak, bu eylemler için ek işlem kaynaklarının tamamlanmasını gerekir. Özellikle, Premium ve İş Açısından Kritik havuzlarında, bu eylemler, taşınan veritabanları için tüm verilerin veya havuzun ölçeği Azaltılsa, elastik havuzdaki tüm veritabanları için aktarılmasını gerektirir. Veri aktarımı, uzun süre çalışan ve Kaynak yoğunluklu bir işlemdir. Havuz zaten yüksek kaynak baskısı altındaysa, Azaltýcý işlem performansı daha da düşürebilir. Olağanüstü durumlarda, gerekli kaynaklar kullanılamadığından veritabanı taşıma veya havuz ölçeği aracılığıyla kaynak çekişmesini çözümlemek mümkün olmayabilir. Bu durumda, etkilenen elastik havuzdaki sorgu iş yükünü geçici olarak düşürmek tek çözüm olabilir.

Yoğun havuzlar kullanan müşteriler, daha önce açıklandığı gibi kaynak kullanımı eğilimlerini yakından izlemelidir ve ölçümler önerilen aralıklar dahilinde kaldığı ve elastik havuzda yeterli kaynak olduğundan, bu işlemleri azaltma işlemi gerçekleştirin.

Kaynak kullanımı, her bir veritabanı ve elastik havuz için zaman içinde değişen birden çok etkene bağlıdır. Yoğun havuzlardaki en iyi fiyat/performans oranının sağlanması sürekli izleme ve yeniden dengeleme gerektirir, bu da veritabanlarını daha fazla kullanılan havuzlardan daha az kullanılan havuzlara taşıyor ve artan iş yükünü karşılamak için gereken yeni havuzlar oluşturuyor.

**"Sık erişimli" veritabanlarını taşımayın**. Havuz düzeyindeki kaynak çekişmesinin birincil olarak az sayıda çok kullanılan veritabanı olması nedeniyle, bu veritabanlarını daha az kullanılan bir havuza taşımak veya tek başına veritabanları yapmak mümkündür. Ancak, bunun yapılması, bir veritabanı yüksek düzeyde kullanılabilir kaldığı sürece bunu yapmanız önerilmez, çünkü taşıma işlemi, hem veritabanı hem de tüm havuz için performansı düşürür. Bunun yerine, yüksek kullanım alt taraflarına kadar bekleyin veya havuz düzeyinde kaynak basıncını sağlamak yerine daha az kullanılan veritabanlarını taşıyın. Ancak, çok düşük kullanımı olan veritabanlarının taşınması, havuz düzeyinde kaynak kullanımını önemli ölçüde azalmadığından, bu durumda herhangi bir avantaj sağlamaz.

**"Karantina" havuzunda yeni veritabanları oluşturun**. Veritabanı başına kiracı modeli kullanan uygulamalar gibi, sıklıkla yeni veritabanlarının oluşturulduğu senaryolarda, mevcut bir elastik havuza yerleştirilmiş yeni bir veritabanının beklenmedik şekilde önemli kaynakları tüketmesi ve diğer veritabanlarını etkilemesi gerekir ve havuzdaki iç süreçler. Bu riski azaltmak için, kaynakların çok fazla ayrılması ile ayrı bir "Karantina" havuzu oluşturun. Henüz bilinmeyen kaynak tüketim desenleriyle bu havuzu yeni veritabanları için kullanın. Bir veritabanı, hafta veya ay gibi bir iş çevrimi için bu havuzda bir kez daha olduğunda ve kaynak tüketimi bilindiğinde, bu ek kaynak kullanımına uyum sağlamak için yeterli kapasiteye sahip bir havuza taşınabilir.

**Aşırı yoğun mantıksal sunuculardan kaçının**. Azure SQL veritabanı, mantıksal sunucu başına en fazla 5000 veritabanını [destekler](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server) . Binlerce veritabanı içeren elastik havuzlar kullanan müşteriler, tek bir sunucuya birden çok elastik havuz yerleştirmeyi göz önünde bulundurarak desteklenen sınıra kadar toplam veritabanı sayısına sahip olabilir. Ancak, binlerce veritabanına sahip mantıksal sunucular işlemsel zorluk oluşturur. Bir sunucudaki tüm veritabanlarının (örneğin, portaldaki veritabanlarını görüntüleme) listelenmesi gereken işlemler daha yavaş olacaktır. Sunucu düzeyinde oturum açma işlemleri veya güvenlik duvarı kuralları için yanlış değişiklik gibi işletimsel hatalar, daha fazla sayıda veritabanını etkileyecektir. Mantıksal sunucunun yanlışlıkla silinmesi, silinen sunucudaki veritabanlarını kurtarmak için Microsoft Desteği 'den yardım gerektirecektir ve etkilenen tüm veritabanları için uzun bir kesinti oluşmasına neden olur.

Mantıksal sunucu başına veritabanlarının sayısını desteklenenden daha düşük bir sayıyla sınırlamanızı öneririz. Birçok senaryoda, sunucu başına en fazla 1000-2000 veritabanı kullanılması idealdir. Yanlışlıkla sunucu silme olasılığını azaltmak için mantıksal sunucuya veya kaynak grubuna bir [silme kilidi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) yerleştirmenizi öneririz.

Geçmişte, veritabanlarının aynı mantıksal sunucu üzerindeki elastik havuzlar arasında, dışarı veya dışarı taşınmasını kapsayan bazı senaryolar veritabanlarını mantıksal sunucular arasında taşırken daha hızlıdır. Şu anda, kaynak ve hedef mantıksal sunucudan bağımsız olarak tüm veritabanı aynı hızda yürütülür.

## <a name="examples"></a>Örnekler

### <a name="monitoring-memory-utilization"></a>Bellek kullanımını izleme

Bu sorgu, son 32 dakika içinde her kaynak havuzu için `oom_per_second` ölçüsünü hesaplar. Bu sorgu, elastik havuzdaki herhangi bir veritabanında yürütülebilir.

```
SELECT pool_id,
       name AS resource_pool_name,
       IIF(name LIKE 'SloSharedPool%' OR name LIKE 'UserPool%', 'user', 'system') AS resource_pool_type,
       SUM(CAST(delta_out_of_memory_count AS decimal))/(SUM(duration_ms)/1000.) AS oom_per_second
FROM sys.dm_resource_governor_resource_pools_history_ex
GROUP BY pool_id, name
ORDER BY pool_id;
```
### <a name="monitoring-tempdb-log-space-utilization"></a>`tempdb` günlük alanı kullanımını izleme

Bu sorgu `tempdb_log_used_percent` ölçüsünün geçerli değerini döndürür. Bu sorgu, elastik havuzdaki herhangi bir veritabanında yürütülebilir.

```
SELECT used_log_space_in_percent AS tempdb_log_used_percent
FROM tempdb.sys.dm_db_log_space_usage;
```

## <a name="next-steps"></a>Sonraki adımlar

* Elastik havuzlara giriş için bkz. [elastik havuzlar, birden çok Azure SQL veritabanını yönetmenize ve ölçeklendirmenize yardımcı olur](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool).
* Kaynak kullanımını azaltmak üzere sorgu iş yüklerini ayarlama hakkında daha fazla bilgi için bkz. [izleme ve ayarlama]( https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-tuning-index), [izleme ve performans ayarlama](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview).
