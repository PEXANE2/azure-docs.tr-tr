---
title: Azure SQL veritabanı kaynak sınırları | Microsoft Docs
description: Bu makalede, tek veritabanları ve elastik havuzlar için Azure SQL veritabanı kaynak sınırlarına genel bir bakış sunulmaktadır. Ayrıca, bu kaynak limitleri isabet edildiğinde veya aşıldığında ne olacağı hakkında bilgi de sağlar.
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
ms.openlocfilehash: fa41649e002bd4845b95e787c1d0589ed1987588
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78391088"
---
# <a name="sql-database-resource-limits-and-resource-governance"></a>SQL veritabanı kaynak sınırları ve kaynak İdaresi

Bu makalede, tek veritabanlarını ve elastik havuzları yöneten bir SQL veritabanı sunucusu için SQL veritabanı kaynak sınırlarına genel bir bakış sunulmaktadır. Bu kaynak limitleri isabet edildiğinde veya aşıldığında ne olacağı hakkında bilgi sağlar ve bu sınırları zorlamak için kullanılan kaynak idare mekanizmalarını açıklar.

> [!NOTE]
> Yönetilen örnek sınırları için bkz. [yönetilen örnekler Için SQL veritabanı kaynak sınırları](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>En fazla kaynak sınırı

| Kaynak | Sınır |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Herhangi bir bölgedeki abonelik başına varsayılan sunucu sayısı | 20 |
| Herhangi bir bölgedeki abonelik başına en fazla sunucu sayısı | 200 |  
| Sunucu başına DTU/eDTU kotası | 54,000 |  
| sunucu/örnek başına sanal çekirdek kotası | 540 |
| Sunucu başına en fazla havuz | DTU sayısıyla veya sanal çekirdekler ile sınırlıdır. Örneğin, her havuz 1000 DTU ise, bir sunucu 54 havuzlarını destekleyebilir.|
|||

> [!IMPORTANT]
> Veritabanlarının sayısı SQL veritabanı sunucusu başına sınıra yaklaşırsa, şunlar meydana gelebilir:
>
> - Ana veritabanına karşı sorguları çalıştırırken gecikme süresini artırma.  Bu, sys. resource_stats gibi kaynak kullanımı istatistiklerinin görünümlerini içerir.
> - Sunucuda veritabanlarının listesini oluşturmayı içeren Yönetim işlemlerinde ve işleme portalı görüntüleme noktalarında gecikme süresini artırma.

> [!NOTE]
> Varsayılan miktardan daha fazla DTU/eDTU kotası, vCore kotası veya daha fazla sunucu almak için Azure portal yeni bir destek isteği gönderebilirsiniz. Daha fazla bilgi için bkz. [Azure SQL veritabanı Için istek kotası artışları](quota-increase-request.md).

### <a name="storage-size"></a>Depolama boyutu

Tek veritabanı kaynak depolama boyutları için, fiyatlandırma katmanı başına depolama boyutu sınırları için [DTU tabanlı kaynak sınırlarına](sql-database-dtu-resource-limits-single-databases.md) veya [sanal çekirdek tabanlı kaynak sınırlarına](sql-database-vcore-resource-limits-single-databases.md) bakın.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak sınırlarına ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus--vcores"></a>Hesaplama (DTU 'Lar ve eDTU 'lar/sanal çekirdekler)

Veritabanı işlem kullanımı (DTU 'lar ve eDTU 'lar ya da sanal çekirdekler) yüksek hale geldiğinde sorgu gecikmesi artar ve sorgular bile zaman aşımına uğrar. Bu koşullar altında, sorgular hizmet tarafından kuyruğa alınabilir ve kaynaklar ücretsiz hale geldiğinde yürütme için kaynaklar sağlanır.
Yüksek işlem kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanını daha fazla işlem kaynağı sağlamak için veritabanının veya elastik havuzun işlem boyutunu artırma. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](sql-database-single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).
- Her bir sorgunun kaynak kullanımını azaltmak için sorguları en iyi duruma getirme. Daha fazla bilgi için bkz. [sorgu ayarlama/Ipuçcu](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı en büyük boyut sınırına ulaştığında, veri boyutunu artıran veritabanı eklemeleri ve güncelleştirmeleri [hata iletisi](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md)alır. SELECT ve DELETE deyimleri başarılı olmaya devam eder.

Yüksek alan kullanımıyla karşılaşdığınızda, risk azaltma seçenekleri şunlardır:

- Veritabanının veya elastik havuzun en büyük boyutunu artırma veya daha fazla depolama alanı ekleme. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](sql-database-single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).
- Veritabanı elastik bir havuztasa alternatif olarak, depolama alanı diğer veritabanlarıyla paylaşılmaması için veritabanı havuzun dışına taşınabilir.
- Kullanılmayan alanı geri kazanmak için bir veritabanını daraltın. Daha fazla bilgi için bkz. [Azure SQL veritabanı 'nda dosya alanını yönetme](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanlar (istekler)

En fazla oturum ve çalışan sayısı, hizmet katmanı ve işlem boyutu (DTU/eDTU veya sanal çekirdekler) tarafından belirlenir. Oturum veya çalışan sınırlarına ulaşıldığında yeni istekler reddedilir ve istemciler bir hata iletisi alır. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebileceği sürece, eşzamanlı çalışanların sayısı genellikle tahmine ve denetime göre daha zordur. Bu özellikle, veritabanı kaynak sınırlarına ulaşıldığında ve çalışanlar daha uzun süre çalışan sorgular, büyük blok zincirler veya aşırı sorgu paralelliği nedeniyle oluşturulan yoğun yük dönemlerinde geçerlidir.

Yüksek oturum veya çalışan kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik havuzun hizmet katmanını veya işlem boyutunu artırma. Bkz. [tek veritabanı kaynaklarını ölçeklendirme](sql-database-single-database-scale.md) ve [elastik havuz kaynaklarını ölçeklendirme](sql-database-elastic-pool-scale.md).
- Artan çalışan kullanımının nedeni, işlem kaynakları için çekişme nedeniyle, her bir sorgunun kaynak kullanımını azaltmak için sorguları en iyi duruma getirme. Daha fazla bilgi için bkz. [sorgu ayarlama/Ipuçcu](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="resource-governance"></a>Kaynak idaresi

Azure SQL veritabanı, kaynak sınırlarını zorlamak için, Azure 'da bir SQL Server veritabanı hizmetini çalıştırmak üzere SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor), değiştirilme ve uzatılmayı temel alan bir kaynak idare uygulamasını kullanır. Hizmette her bir SQL Server örneğinde, [bir hizmet olarak dengeli bir veritabanı](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/)sağlamak için her iki havuzda ve grup düzeyinde ayarlanmış kaynak limitleriyle birden fazla [kaynak havuzu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-resource-pool) ve [iş yükü grubu](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-workload-group)vardır. Kullanıcı iş yükü ve iç iş yükleri ayrı kaynak havuzları ve iş yükü grupları halinde sınıflandırılır. Coğrafi çoğaltmalar dahil olmak üzere birincil ve okunabilir ikincil çoğaltmalarda Kullanıcı iş yükü, `N` veritabanı KIMLIĞI değeri için temsil edilen `SloSharedPool1` kaynak havuzuna ve `UserPrimaryGroup.DBId[N]` iş yükü grubuna sınıflandırılmaktadır. Ayrıca, çeşitli iç iş yükleri için birden fazla kaynak havuzu ve iş yükü grubu vardır.

Azure SQL veritabanı, SQL Server işlemindeki kaynakları yönetmek için Resource Governor kullanmanın yanı sıra işlem düzeyi kaynak idaresi için Windows [Iş nesnelerini](https://docs.microsoft.com/windows/win32/procthread/job-objects) ve depolama kotası yönetimi Için Windows [dosya sunucusu Kaynak Yöneticisi (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) kullanır.

Azure SQL veritabanı kaynak İdaresi, doğası gereği hiyerarşik bir şekilde yapılır. Üstten alta kadar, işletim sistemi kaynak idare mekanizmaları ve Resource Governor ve ardından Resource Governor kullanılarak kaynak havuzu düzeyinde ve sonra kullanarak iş yükü grubu düzeyinde, sınırlar işletim sistemi düzeyinde ve depolama birimi düzeyinde zorlanır. Resource Governor. Geçerli veritabanı veya elastik havuz için yürürlükte olan kaynak idare limitleri, [sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) görünümünde ortaya çıkmış. 

### <a name="data-io-governance"></a>Veri GÇ yönetimi

Veri GÇ yönetimi, Azure SQL veritabanı 'nda, hem okuma hem de yazma fiziksel GÇ 'yi bir veritabanının veri dosyalarına göre sınırlandırmak için kullanılan bir işlemdir. "Gürültülü komşu" efektini en aza indirmek, çok kiracılı hizmette kaynak ayırma eşitliği sağlamak ve temel alınan donanımın ve depolamanın özellikleri dahilinde kalmak için her bir hizmet düzeyi için ıOPS sınırları ayarlanır.

Tek veritabanları için, iş yükü grubu sınırları veritabanına göre tüm depolama GÇ kaynaklarına uygulanır, ancak kaynak havuzu limitleri, `tempdb` veritabanı dahil olmak üzere aynı SQL Server örneğindeki tüm veritabanlarına yönelik tüm depolama ıO 'larda geçerlidir. Elastik havuzlar için, iş yükü grubu sınırları havuzdaki her bir veritabanı için geçerlidir, ancak kaynak havuzu sınırı, havuzdaki tüm veritabanları arasında paylaşılan `tempdb` veritabanı da dahil olmak üzere tüm elastik havuz için geçerlidir. Genel olarak, kaynak havuzu limitleri bir veritabanında (tek veya havuza alınmış) iş yükü tarafından ulaşılabilir olamaz, çünkü iş yükü grubu sınırları kaynak havuzu limitinden daha düşüktür ve ıOPS/aktarım hızını daha erken sınırlayın. Ancak, aynı SQL Server örneğindeki birden çok veritabanına karşı, havuz sınırlarına, Birleşik iş yükü tarafından ulaşılmış olabilir.

Örneğin, bir sorgu herhangi bir GÇ kaynak İdaresi olmadan 1000 ıOPS oluşturursa, ancak iş yükü grubu maksimum ıOPS sınırı 900 ıOPS olarak ayarlanırsa, sorgu en fazla 900 ıOPS oluşturamayacak. Ancak, kaynak havuzu maksimum ıOPS sınırı 1500 ıOPS olarak ayarlanmışsa ve kaynak havuzuyla ilişkili tüm iş yükü gruplarından gelen toplam GÇ 1500 ıOPS 'yi aşarsa, aynı sorgunun GÇ değeri, 900 ıOPS çalışma grubu sınırının altında azaltılabilir.

[Sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) görünümü tarafından döndürülen IOPS ve aktarım hızı/maksimum değerleri, garanti olarak değil, limit/Caps olarak davranır. Ayrıca, kaynak İdaresi belirli bir depolama gecikmesini garanti etmez. Belirli bir kullanıcı iş yükü için en iyi ulaşılabilir gecikme süresi, ıOPS ve aktarım hızı yalnızca GÇ kaynak idare sınırlarına, ayrıca kullanılan GÇ boyutlarının karışımına ve temel depolamanın özelliklerine göre değişir. SQL Server, 512 KB ve 4 MB arasında bir boyut farklılık gösteren IOs kullanır. IOPS sınırlarını zorlama amaçları doğrultusunda, Azure Storage 'da veri dosyaları olan veritabanlarının dışında, her GÇ boyutundan bağımsız olarak hesaba katılmaz. Bu durumda, Azure Storage ıO Accounting ile hizalamak için 256 KB 'den büyük olan IOs birden fazla 256 KB IOs olarak hesaba katılmaz.

Azure depolama 'daki veri dosyalarını kullanan temel, standart ve Genel Amaçlı veritabanları için, bir veritabanında bu ıOPS sayısını birikecek yeterli veri dosyası yoksa veya veriler dosyalar arasında eşit olarak dağıtılmıyorsa veya temeldeki Blobların performans katmanı kaynak idare sınırının altındaki ıOPS/aktarım hızını sınırlıyorsa, `primary_group_max_io` değeri ulaşılabilir olmayabilir. Benzer şekilde, sık işlem işleme tarafından oluşturulan küçük günlük IOs ile, temel Azure depolama blobunun ıOPS sınırı nedeniyle `primary_max_log_rate` değeri bir iş yükü tarafından ulaşılabilir olmayabilir.

[Sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)ve [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) görünümlerinde bildirilen `avg_data_io_percent` ve `avg_log_write_percent`gibi kaynak kullanım değerleri, maksimum kaynak idare limitlerinin yüzdesi olarak hesaplanır. Bu nedenle, kaynak İdaresi, ıOPS/aktarım hızı dışındaki faktörler olduğunda, bildirilen kaynak kullanımı %100 altında kalsa da, iş yükünün arttığı sırada ıOPS/üretilen iş düzleştirme ve gecikme sürelerini görmek mümkündür. 

Veritabanı dosyası başına okuma ve yazma ıOPS, aktarım hızı ve gecikme süresini görmek için [sys. dm_io_virtual_file_stats ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) işlevini kullanın. Bu işlev, `avg_data_io_percent`'e göre hesaba katılmaz, ancak temeldeki depolamanın ıOPS ve aktarım hızını kullanır ve gözlemlenen depolama gecikmesini etkileyebilir. İşlev Ayrıca, sırasıyla `io_stall_queued_read_ms` ve `io_stall_queued_write_ms` sütunlarında, okuma ve yazma işlemleri için GÇ kaynak İdaresi tarafından kullanıma sunulmuş ek gecikme süresi de sunmaktadır.

### <a name="transaction-log-rate-governance"></a>İşlem günlüğü oranı idare

İşlem günlüğü oranı yönetimi, toplu ekleme, seçme ve dizin oluşturma gibi iş yüklerinin yüksek alım tarifelerinin sınırlandırılmasına yönelik Azure SQL veritabanı 'nda kullanılan bir işlemdir. Bu sınırlar, veri dosyalarına göre kaç IOs yayımlanabileceğine bakılmaksızın iş verimini sınırlayan, alt ikinci düzeyde günlük kaydı oluşturma hızına göre izlenir ve zorlanır.  İşlem günlüğü oluşturma ücretleri Şu anda, sanal çekirdek satın alma modeliyle maksimum günlük hızı 96 MB/sn olmasına izin verilen, donanıma bağımlı olan bir noktaya göre ölçeklendirilmeye göre ölçeklenebilir. 

> [!NOTE]
> Gerçek fiziksel IOs işlem günlüğü dosyalarına yönetilmez veya sınırlı değildir.

Günlük ücretleri, çeşitli senaryolarda elde edilebilecekleri ve sürekli olarak kullanılabilir, ancak genel sistem kendi işlevlerini Kullanıcı yüküne en aza indirilerek koruyabileceğinden bu şekilde ayarlanır. Günlük hızı yönetimi, işlem günlüğü yedeklemelerinin yayımlanan kurtarmalar içinde kalmasını sağlar.  Bu idare, ikincil çoğaltmalarda aşırı biriktirme listesini de önler.

Günlük kayıtları üretildiğinden, her işlem değerlendirilir ve en fazla istenen günlük hızının (saniyede MB/s) korunması için geciktirilmesi gerekip gerekmediğini değerlendirirler. Günlük kayıtları depoya boşaltıldığınızda gecikmeler eklenmez, bunun yerine günlük hızı oluşturma sırasında günlük hızı Yönetimi uygulanır.

Çalışma zamanında uygulanan gerçek günlük oluşturma ücretleri ayrıca, geri bildirim mekanizmalarından etkilenerek, sistemin sabitlenebilmesi için izin verilen günlük hızlarını geçici olarak azaltabilirsiniz. Günlük dosyası alanı yönetimi, oturum açma alanı koşullarını ve kullanılabilirlik grubu çoğaltma mekanizmalarını çalıştırmanın genel sistem sınırlarını geçici olarak azalmasını önleme.

Günlük hızı idarecisi trafik şekillendirme, aşağıdaki bekleme türleri aracılığıyla ortaya çıkarılır ( [sys. dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) DMV içinde gösterilir):

| Bekleme türü | Notlar |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Veritabanı sınırlandırma |
| POOL_LOG_RATE_GOVERNOR | Havuz sınırlandırma |
| INSTANCE_LOG_RATE_GOVERNOR | Örnek düzeyi sınırlandırma |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Geri bildirim denetimi, Premium 'da kullanılabilirlik grubu fiziksel çoğaltma ve İş Açısından Kritik güncel değil |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Bir günlük alanı koşulunu önlemek için geri bildirim denetimi, oranları sınırlandırma |
|||

İstenen ölçeklenebilirliği gösteren bir günlük hızı sınırı ile karşılaşıldığında, aşağıdaki seçenekleri göz önünde bulundurun:
- En yüksek 96 MB/sn günlük hızını almak için daha yüksek bir hizmet düzeyine kadar ölçeklendirin. 
- Yüklenen veriler geçici ise (örneğin, bir ETL işleminde veri hazırlama gibi), tempdb 'ye yüklenebilir (en az bir şekilde günlüğe kaydedilir). 
- Analitik senaryolar için, kümelenmiş bir columnstore kapsamına alınmış bir tabloya yükleyin. Bu, sıkıştırma nedeniyle gerekli günlük oranını azaltır. Bu teknik, CPU kullanımını artırır ve yalnızca kümelenmiş columnstore dizinlerinden faydalan veri kümelerine uygulanabilir. 

## <a name="next-steps"></a>Sonraki adımlar

- Genel Azure limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md).
- DTU 'Lar ve eDTU 'lar hakkında daha fazla bilgi için bkz. [DTU ve eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Tempdb boyut limitleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
