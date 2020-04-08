---
title: Azure SQL Veritabanı kaynak sınırları | Microsoft Dokümanlar
description: Bu makalede, tek veritabanları ve esnek havuzlar için Azure SQL Veritabanı kaynak sınırlarına genel bir bakış sağlanmaktadır. Ayrıca, bu kaynak sınırları vurulduğunda veya aşıldığında ne olduğuna ilişkin bilgiler de sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
ms.date: 11/19/2019
ms.openlocfilehash: afb30a17d7a1450f169402c18f41ce249415e89d
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804835"
---
# <a name="sql-database-resource-limits-and-resource-governance"></a>SQL Veritabanı kaynak sınırları ve kaynak yönetimi

Bu makalede, tek veritabanları ve elastik havuzları yöneten bir SQL Veritabanı sunucusu için SQL Veritabanı kaynak sınırları genel bir bakış sağlar. Bu kaynak sınırları vurulduğunda veya aşıldığında ne olduğu hakkında bilgi sağlar ve bu sınırları uygulamak için kullanılan kaynak yönetim mekanizmalarını açıklar.

> [!NOTE]
> Yönetilen Örnek sınırları [için, yönetilen örnekler için SQL Veritabanı kaynak sınırlarını](sql-database-managed-instance-resource-limits.md)görün.

## <a name="maximum-resource-limits"></a>Maksimum kaynak sınırları

| Kaynak | Sınır |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Herhangi bir bölgedeki abonelik başına varsayılan sunucu sayısı | 20 |
| Herhangi bir bölgedeki abonelik başına maksimum sunucu sayısı | 200 |  
| Sunucu başına DTU / eDTU kotası | 54,000 |  
| sunucu/örnek başına vCore kotası | 540 |
| Sunucu başına maksimum havuzlar | DTUs veya vCore sayısı ile sınırlıdır. Örneğin, her havuz 1000 DUS ise, sunucu 54 havuzu destekleyebilir.|
|||

> [!IMPORTANT]
> Veritabanı sayısı SQL Veritabanı sunucusu başına sınıra yaklaştıkça aşağıdakiler oluşabilir:
>
> - Ana veritabanına karşı sorguları çalıştırmada gecikme yitirme.  Bu, sys.resource_stats gibi kaynak kullanım istatistiklerinin görünümlerini içerir.
> - Yönetim işlemlerinde gecikmenin artırılması ve sunucudaki veritabanlarının sayısallaştırılmasını içeren portal bakış açılarının işlenmesi.

> [!NOTE]
> Varsayılan tutardan daha fazla DTU/eDTU kotası, vCore kotası veya daha fazla sunucu elde etmek için Azure portalında yeni bir destek isteği gönderin. Daha fazla bilgi için Azure [SQL Veritabanı için İstek kotası artışlarına](quota-increase-request.md)bakın.

### <a name="storage-size"></a>Depolama boyutu

Tek veritabanları kaynak depolama boyutları için, fiyatlandırma katmanı başına depolama boyutu sınırları için [DTU tabanlı kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md) veya [vCore tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak sınırlarına ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus--vcores"></a>İşlem (DTUs ve eDTUs / vCores)

Veritabanı hesaplama kullanımı (DTUs ve eDTUs veya vCores ile ölçülen) yüksek olduğunda, sorgu gecikmesi artar ve sorgular zaman sonu bile olabilir. Bu koşullar altında, sorgular hizmet tarafından sıraya alınabilir ve kaynaklar özgür hale geldikçe yürütme için kaynaklar sağlanır.
Yüksek işlem kullanımıyla karşılaştığında, azaltma seçenekleri şunlardır:

- Veritabanına daha fazla bilgi işlem kaynağı sağlamak için veritabanının veya elastik havuzun işlem boyutunu artırma. Bkz. [Tek veritabanı kaynaklarını ölçeklendirin](sql-database-single-database-scale.md) ve esnek havuz kaynaklarını [ölçeklendirin.](sql-database-elastic-pool-scale.md)
- Her sorgunun kaynak kullanımını azaltmak için sorguları en iyi duruma getirmek. Daha fazla bilgi için sorgu [aparat/ipucu](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı maksimum boyut sınırına ulaştığında, veri boyutunu artıran veritabanı ekler ve güncelleştirmeler başarısız olursa ve istemciler bir [hata iletisi](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md)alırsınız. SELECT ve DELETE ifadeleri başarılı olmaya devam ediyor.

Yüksek alan kullanımıyla karşılaştığında azaltma seçenekleri şunlardır:

- Veritabanının veya elastik havuzun maksimum boyutunu artırma veya daha fazla depolama alanı ekleme. Bkz. [Tek veritabanı kaynaklarını ölçeklendirin](sql-database-single-database-scale.md) ve esnek havuz kaynaklarını [ölçeklendirin.](sql-database-elastic-pool-scale.md)
- Veritabanı elastik bir havuzdaysa, alternatif olarak veritabanı havuzun dışına taşınarak depolama alanının diğer veritabanlarıyla paylaşılmaz.
- Kullanılmayan alanı geri almak için veritabanını küçültün. Daha fazla bilgi için Azure [SQL Veritabanı'nda dosya alanını yönet'e](sql-database-file-space-management.md) bakın

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanlar (istekler)

Maksimum oturum sayısı ve çalışan sayısı hizmet katmanı ve işlem boyutuna (DTUs/eDTUs veya vCores) göre belirlenir. Oturum veya alt sınıra ulaşıldığında yeni istekler reddedilir ve istemciler bir hata iletisi alır. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir ken, eşzamanlı çalışan sayısı genellikle tahmin etmek ve kontrol etmek zordur. Bu, özellikle veritabanı kaynak sınırlarına ulaşıldığı ve çalışanların daha uzun çalışan sorgular, büyük engelleme zincirleri veya aşırı sorgu paralelliği nedeniyle biriktisi en yoğun yük dönemlerinde geçerlidir.

Yüksek oturum veya işçi kullanımı ile karşılaştığında, azaltma seçenekleri şunlardır:

- Veritabanının veya elastik havuzun hizmet katmanını veya bilgi işlem boyutunu artırma. Bkz. [Tek veritabanı kaynaklarını ölçeklendirin](sql-database-single-database-scale.md) ve esnek havuz kaynaklarını [ölçeklendirin.](sql-database-elastic-pool-scale.md)
- Artan alt kullanım nedeni bilgi işlem kaynakları için çekişme nedeniyle ise her sorgunun kaynak kullanımını azaltmak için sorguları en iyi duruma getirmek. Daha fazla bilgi için sorgu [aparat/ipucu](sql-database-performance-guidance.md#query-tuning-and-hinting).
- [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines) (maksimum paralellik derecesi) ayarını azaltma.
- Olay sayısını ve sorgu engelleme süresini azaltmak için sorgu iş yükünü en iyi duruma getirmek.

### <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>Kullanıcı iş yüklerine ve dahili işlemlere göre kaynak tüketimi

Her veritabanındaki kullanıcı iş yüklerine göre CPU ve bellek tüketimi [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) ve `avg_memory_usage_percent` [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) görünümlerinde, içinde `avg_cpu_percent` ve sütunlarında bildirilir. Elastik havuzlar için, havuz düzeyinde kaynak tüketimi [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümünde bildirilir. Kullanıcı iş yükü CPU tüketimi, havuz [düzeyindeki tek veritabanları](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) ve esnek [havuzlar](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) için `cpu_percent` Azure Monitor ölçümü aracılığıyla da bildirilir.

Azure SQL Veritabanı, yüksek kullanılabilirlik ve olağanüstü durum kurtarma, veritabanı yedekleme ve geri yükleme, izleme, Sorgu Deposu, Otomatik ayar, vb. gibi temel hizmet özelliklerini uygulamak için bilgi işlem kaynakları gerektirir. Sistem, [kaynak yönetim](#resource-governance) mekanizmalarını kullanarak bu iç işlemler için toplam kaynakların belirli bir bölümünü bir kenara ayırarak, kalan kaynakları kullanıcı iş yükleri için kullanılabilir hale getirir. İç işlemlerin işlem kaynaklarını kullanmadığı zamanlarda, sistem bunları kullanıcı iş yüklerine uygun hale getirir.

Tek bir veritabanıveya elastik bir havuz barındıran SQL Server örneğinde kullanıcı iş yüklerine ve dahili işlemlere göre toplam `avg_instance_cpu_percent` CPU `avg_instance_memory_percent` ve bellek tüketimi [sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) ve [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) görünümlerinde, içinde ve sütunlarında bildirilir. Bu veriler, havuz `sqlserver_process_core_percent` [düzeyindeki tek veritabanları](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) ve [elastik havuzlar](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) için Azure Monitor ölçümleri ve `sqlserver_process_memory_percent` Azure Monitor ölçümleri aracılığıyla da raporlanır.

[Sys.dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database) ve [sys.dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database) görünümlerinde, son kaynak tüketiminin kullanıcı iş yüklerine ve dahili işlemlere göre daha ayrıntılı bir dökümü bildirilir. Bu görünümlerde başvurulan kaynak havuzları ve iş yükü grupları yla ilgili ayrıntılar için kaynak [yönetimi'ne](#resource-governance)bakın. Bu görünümler, ilişkili kaynak havuzları ve iş yükü gruplarında kullanıcı iş yüklerine ve belirli iç işlemlere göre kaynak kullanımı hakkında rapor verir.

Performans izleme ve sorun giderme bağlamında, hem **kullanıcı CPU** tüketimini`avg_cpu_percent` `cpu_percent`hem de toplam CPU **tüketimini** kullanıcı iş`avg_instance_cpu_percent``sqlserver_process_core_percent`yüklerine ve dahili işlemlere göre dikkate almak önemlidir.

**Kullanıcı CPU tüketimi,** her hizmet hedefindeki kullanıcı iş yükü sınırlarının yüzdesi olarak hesaplanır. **%100 kullanıcı CPU kullanımı,** kullanıcı iş yükünün hizmet hedefinin sınırına ulaştığını gösterir. Ancak, **toplam CPU tüketimi** %70-100 aralığına ulaştığında, bildirilen **kullanıcı CPU tüketimi** %100'ün önemli ölçüde altında kalsa bile, kullanıcı iş yükünün iş yükünün düzleşerek ve sorgu gecikmesinin arttığını görmek mümkündür. Bu, bilgi işlem kaynaklarının orta düzeyde tahsisi ile daha küçük hizmet hedefleri kullanırken, ancak [yoğun elastik havuzlar](sql-database-elastic-pool-resource-management.md)gibi nispeten yoğun kullanıcı iş yükleri kullanırken daha olasıdır. Bu, iç işlemler geçici olarak ek kaynaklar gerektirdiğinde(örneğin veritabanının yeni bir kopyasını oluştururken) daha küçük hizmet hedefleri yle de oluşabilir.

**Toplam CPU tüketimi** yüksek olduğunda, azaltma seçenekleri daha önce belirtildiği gibi aynıdır ve hizmet hedefi artış ını ve/veya kullanıcı iş yükünü en iyi duruma getirilmesini içerir.

## <a name="resource-governance"></a>Kaynak idaresi

Azure SQL Veritabanı, kaynak sınırlarını zorlamak için, Azure'da bir SQL Server veritabanı hizmetini çalıştırmak üzere değiştirilen ve genişletilmiş SQL Server [Resource Governor'i](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor)temel alan bir kaynak yönetimi uygulaması kullanır. Hizmetteki her SQL Server örneğinde, dengeli bir [Hizmet Veritabanı](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/)sağlamak için hem havuz hem de grup düzeylerinde kaynak sınırları ayarlanan birden çok [kaynak havuzu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-resource-pool) ve iş [yükü grubu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-workload-group)vardır. Kullanıcı iş yükü ve iç iş yükleri ayrı kaynak havuzları ve iş yükü grupları olarak sınıflandırılır. Coğrafi yinelemeler de dahil olmak üzere birincil ve okunabilir ikincil yinelemelerde `SloSharedPool1` kullanıcı `UserPrimaryGroup.DBId[N]` iş yükü, `N` veritabanı kimliği değerini temsil eden kaynak havuzu ve iş yükü grubuna sınıflandırılır. Buna ek olarak, çeşitli iç iş yükleri için birden çok kaynak havuzu ve iş yükü grupları vardır.

Azure SQL Veritabanı, SQL Server işlemindeki kaynakları yönetmek için Kaynak Yöneticisi'ni kullanmanın yanı sıra, işlem düzeyi kaynak yönetimi için Windows [İş Nesneleri'ni](https://docs.microsoft.com/windows/win32/procthread/job-objects) ve depolama kotası yönetimi için Windows [File Server Resource Manager'ı (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) de kullanır.

Azure SQL Veritabanı kaynak yönetimi doğası gereği hiyerarşiktir. Yukarıdan aşağıya doğru, sınırlar işletim sistemi kaynak yönetim mekanizmaları ve Kaynak Yöneticisi, daha sonra Kaynak Valisi kullanarak kaynak havuzu düzeyinde ve daha sonra Kaynak Valisi kullanarak iş yükü grup düzeyinde kullanarak işletim sistemi düzeyinde ve depolama hacmi düzeyinde uygulanır. Geçerli veritabanı veya elastik havuz için geçerli olan kaynak yönetim sınırları [sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) görünümünde su yüzüne çıkar. 

### <a name="data-io-governance"></a>Veri IO yönetimi

Veri IO yönetişimi, Azure SQL Veritabanı'nda bir veritabanının veri dosyalarına karşı hem okuma hem de yazma fiziksel IO'su sınırlamak için kullanılan bir işlemdir. "Gürültülü komşu" etkisini en aza indirmek, çok kiracılı hizmette kaynak ayırma adaleti sağlamak ve temel donanım ve depolama nın yetenekleri içinde kalmak için her hizmet düzeyi için IOPS sınırları ayarlanır.

Tek veritabanları için iş yükü grubu sınırları veritabanına karşı tüm depolama IO'larına uygulanırken, `tempdb` kaynak havuzu sınırları veritabanı da dahil olmak üzere aynı SQL Server örneğindeki tüm veritabanlarına karşı tüm depolama IO'larına uygulanır. Esnek havuzlar için iş yükü grubu sınırları havuzdaki her veritabanıiçin geçerlidir, kaynak havuzu sınırı `tempdb` ise havuzdaki tüm veritabanları arasında paylaşılan veritabanı da dahil olmak üzere tüm esnek havuz için geçerlidir. İş yükü grubu sınırları kaynak havuzu sınırlarından daha düşük olduğundan ve IOPS/iş kaynağını daha erken sınırladığından, genel olarak kaynak havuzu sınırları veritabanına (tek veya havuzlu) karşı iş yükü yle elde edilemeyebilir. Ancak, havuz sınırlarına, aynı SQL Server örneğinde birden çok veritabanına karşı birleştirilmiş iş yükü tarafından ulaşılabilir.

Örneğin, bir sorgu herhangi bir IO kaynak yönetimi olmadan 1000 IOPS oluşturur, ancak iş yükü grubu maksimum IOPS sınırı 900 IOPS olarak ayarlanırsa, sorgu 900 IOPS'den fazla oluşturamaz. Ancak, kaynak havuzu maksimum IOPS sınırı 1500 IOPS olarak ayarlanırsa ve kaynak havuzuyla ilişkili tüm iş yükü gruplarından toplam IO 1500 IOPS'yi aşarsa, aynı sorgunun IO'su 900 IOPS çalışma grubu sınırının altına indirilebilir.

[Sys.dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) tarafından döndürülen IOPS ve iş yapma min/max değerleri garanti olarak değil, limit/kapak olarak hareket eder. Ayrıca, kaynak yönetimi belirli bir depolama gecikmesi garanti etmez. Belirli bir kullanıcı iş yükü için en iyi ulaşılabilir gecikme, IOPS ve iş yükü yalnızca IO kaynak yönetim sınırlarına değil, aynı zamanda kullanılan IO boyutlarının karışımına ve temel depolamanın yeteneklerine de bağlıdır. SQL Server boyutu 512 KB ile 4 MB arasında değişen iOs kullanır. IOPS sınırlarını zorlamak amacıyla, Azure Depolama'daki veri dosyaları olan veritabanları hariç olmak üzere, her IO boyutuna bakılmaksızın muhasebelenir. Bu durumda, Azure Depolama IO muhasebesiyle uyumlu hale getirmek için 256 KB'den büyük iO'lar birden çok 256 KB IO olarak muhasebelenir.

Azure Depolama'da veri dosyalarını kullanan Temel, Standart ve `primary_group_max_io` Genel Amaçlı veritabanlarında, bir veritabanında bu IOPS sayısını toplu olarak sağlamak için yeterli veri dosyası yoksa veya veriler dosyalar arasında eşit olarak dağıtılmamışsa veya temel lekelerin performans katmanı kaynak yönetim sınırının altında IOPS/iş başına kadar sınırlanıyorsa, bu değer elde edilemeyebilir. Benzer şekilde, sık sık işlem işlemek tarafından `primary_max_log_rate` oluşturulan küçük günlük IOs ile, değer temel Azure depolama blob Üzerinde IOPS sınırı nedeniyle bir iş yükü tarafından elde olmayabilir.

[sys.dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys.resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)ve [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümleri gibi `avg_data_io_percent` kaynak `avg_log_write_percent`kullanım değerleri, maksimum kaynak yönetim sınırlarının yüzdeleri olarak hesaplanır. Bu nedenle, kaynak yönetimi limiti IOPS/iş üretimi dışındaki faktörler olduğunda, bildirilen kaynak kullanımı %100'ün altında kalsa da, iş yükü arttıkça IOPS/iş üretimi düzleşme ve gecikmelerin arttığını görmek mümkündür. 

IOPS dosyasını okuma ve yazma, iş başı ve gecikme yi görmek için [sys.dm_io_virtual_file_stats()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) işlevini kullanın. Bu işlev, veritabanına karşı `avg_data_io_percent`tüm IO'yu yüzeye çıkarmaktadır, arka plan IO'su da dahil olmak üzere, iops ve temel depolamanın veri metotunu kullanır ve gözlenen depolama gecikmesini etkileyebilir. İşlev ayrıca, sırasıyla ve `io_stall_queued_read_ms` `io_stall_queued_write_ms` sütunlarda okuma ve yazma için IO kaynak yönetişimi tarafından tanıtılabilecek ek gecikme yitirme yüzeyi.

### <a name="transaction-log-rate-governance"></a>İşlem günlüğü oranı yönetimi

İşlem günlüğü oranı yönetimi, Azure SQL Veritabanı'nda toplu ekleme, SELECT INTO ve dizin oluşturma gibi iş yükleri için yüksek alım oranlarını sınırlamak için kullanılan bir işlemdir. Bu sınırlar, veri dosyalarına karşı kaç iOs verilebilir bakılmaksızın iş verisi oluşturma hızına göre ikinci alt düzeyde izlenir ve uygulanır.  İşlem günlüğü oluşturma oranları şu anda donanıma bağımlı bir noktaya kadar doğrusal olarak ölçeklendirilir ve maksimum günlük oranı vCore satın alma modeliyle 96 MB/s olarak izin verilir. 

> [!NOTE]
> Hareket günlüğü dosyalarına gerçek fiziksel IO'lar yönetilmez veya sınırlı değildir.

Günlük oranları, çeşitli senaryolarda elde edilebilen ve sürdürülebilecek şekilde ayarlanırken, genel sistem kullanıcı yüküne en aza indirgenmiş etkiyle işlevselliğini koruyabilir. Günlük oranı yönetimi, işlem günlüğü yedeklemelerinin yayımlanmış kurtarılabilirlik SLA'ları içinde kalmasını sağlar.  Bu yönetim, ikincil yinelemeler üzerinde aşırı biriktirme listesini de önler.

Günlük kayıtları oluşturulduğunda, her işlem, istenilen maksimum günlük hızını (saniyede MB/sn) korumak için geciktirilip geciktirilmeyeceği değerlendirilerek değerlendirilir. Günlük kayıtları depoya atıldığında gecikmeler eklenmez, daha çok günlük oranı yönetimi, günlük oranı oluşturma sırasında uygulanır.

Çalışma zamanında uygulanan gerçek günlük oluşturma oranları da geri bildirim mekanizmalarından etkilenerek, sistemin stabilize edilebilmesi için izin verilen günlük oranlarını geçici olarak azaltabilir. Günlük alanı yönetimi, günlük alanı koşulları ve Kullanılabilirlik Grubu çoğaltma mekanizmaları dışında çalışan kaçınarak geçici olarak genel sistem sınırlarını azaltabilir.

Günlük oranı vali trafik aşağıdaki bekleme türleri [(sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) ve [sys.dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) görünümlerde maruz) ile su yüzüne çıkar:

| Bekleme Türü | Notlar |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Veritabanı sınırlaması |
| POOL_LOG_RATE_GOVERNOR | Havuz sınırlaması |
| INSTANCE_LOG_RATE_GOVERNOR | Örnek düzeyi sınırlayıcı |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Geri bildirim kontrolü, Premium/Business Critical'te kullanılabilirlik grubu fiziksel çoğaltma |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Geri bildirim kontrolü, günlük dışı alan durumunu önlemek için oranları sınırlama |
| HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Coğrafi çoğaltma geri bildirim kontrolü, yüksek veri gecikmesi ve coğrafi-saniyelerin kullanılamamasını önlemek için günlük hızını sınırlayan|
|||

İstenilen ölçeklenebilirliği engelleyen bir günlük hızı sınırıyla karşılaştığında aşağıdaki seçenekleri göz önünde bulundurun:
- Maksimum 96 MB/s günlük ücretini almak veya farklı bir hizmet katmanına geçmek için daha yüksek bir hizmet düzeyine kadar ölçeklendirin. [Hyperscale](sql-database-service-tier-hyperscale.md) hizmet katmanı, seçilen hizmet düzeyine bakılmaksızın 100 MB/s günlük hızı sağlar.
- Yüklenen veriler, ETL işleminde veri hazırlama gibi geçiciyse, tempdb'ye yüklenebilir (en az günlüğe kaydedilir). 
- Analitik senaryolar için, kümelenmiş sütun deposu kapsamındaki tabloya yükleyin. Bu sıkıştırma nedeniyle gerekli günlük hızını azaltır. Bu teknik CPU kullanımını artırır ve yalnızca kümelenmiş sütun deposu dizinlerinden yararlanan veri kümeleri için geçerlidir. 

## <a name="next-steps"></a>Sonraki adımlar

- Genel Azure sınırları hakkında bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.
- DTUs ve eDTUs hakkında bilgi için [DTÜ ve eDTUs'a](sql-database-purchase-models.md#dtu-based-purchasing-model)bakın.
- Tempdb boyut sınırları hakkında daha fazla bilgi için [Azure SQL Veritabanı'nda TempDB'ye](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)bakın.
