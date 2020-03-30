---
title: Hiperölçekte performans tanılama
description: Bu makalede, Azure SQL Veritabanı'ndaki Hiper ölçek performans sorunlarının nasıl giderilen olduğu açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: seo-lt-2019
ms.topic: troubleshooting
author: denzilribeiro
ms.author: denzilr
ms.reviewer: sstein
ms.date: 10/18/2019
ms.openlocfilehash: 26bd6ddb9d8255b8e2510133fc4b6aa645f89f68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75615056"
---
# <a name="sql-hyperscale-performance-troubleshooting-diagnostics"></a>SQL Hiper ölçekli performans sorun giderme tanılama

Bir Hyperscale veritabanındaki performans sorunlarını gidermek için, Azure SQL veritabanı işlem düğümündeki [genel performans alaş metodolojileri](sql-database-monitor-tune-overview.md) performans araştırmasının başlangıç noktasıdır. Ancak, [Hyperscale'in dağıtılmış mimarisi](sql-database-service-tier-hyperscale.md#distributed-functions-architecture) göz önüne alındığında, yardımcı olmak için ek tanılama lar eklenmiştir. Bu makalede, Hiper ölçek özgü tanı verileri açıklanmaktadır.

## <a name="log-rate-throttling-waits"></a>Günlük hızı azaltma beklemeleri

Her Azure SQL Veritabanı hizmet düzeyi, [günlük oranı yönetimi](sql-database-resource-limits-database-server.md#transaction-log-rate-governance)aracılığıyla uygulanan günlük oluşturma oranı sınırlarına sahiptir. Hyperscale'de, günlük oluşturma sınırı şu anda hizmet düzeyine bakılmaksızın 100 MB/sn olarak ayarlanır. Ancak, birincil işlem yinelemesinde günlük oluşturma hızının kurtarılabilirlik SLA'larını korumak için daraltılması gereken zamanlar vardır. Bu azaltma, bir sayfa sunucusu veya başka bir [bilgi işlem yinelemesi](sql-database-service-tier-hyperscale.md#distributed-functions-architecture) Log hizmetinden yeni günlük kayıtlarının uygulanmasının önemli ölçüde arkasında olduğunda gerçekleşir.

Aşağıdaki bekleme türleri [(sys.dm_os_wait_stats'](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql/)de) günlük oranının birincil bilgi işlem yinelemesinde neden azaltılabilir nedenini açıklar:

|Bekleme Türü    |Açıklama                         |
|-------------          |------------------------------------|
|RBIO_RG_STORAGE        | Bir Hyperscale veritabanı birincil bilgi işlem düğümü günlük oluşturma oranı sayfa sunucusunda gecikmiş günlük tüketimi nedeniyle daraltılmış olduğunda oluşur.         |
|RBIO_RG_DESTAGE        | Hyperscale veritabanı hesaplama düğümü günlük oluşturma hızı, uzun süreli günlük depolaması tarafından gecikmiş günlük tüketimi nedeniyle daraltıldığında oluşur.         |
|RBIO_RG_REPLICA        | Bir Hyperscale veritabanı bilgi işlem düğümü günlük oluşturma oranı okunabilir ikincil çoğaltma (lar) tarafından gecikmiş günlük tüketimi nedeniyle daraltılır oluşur.         |
|RBIO_RG_LOCALDESTAGE   | Hyperscale veritabanı hesaplama düğümü günlük oluşturma hızı, günlük hizmeti tarafından gecikmiş günlük tüketimi nedeniyle daraltıldığında oluşur.         |

## <a name="page-server-reads"></a>Sayfa sunucusu okur

İşlem yinelemeleri veritabanının tam bir kopyasını yerel olarak önbelleğe vermez. Bilgi işlem yinelemesine yerel olan veriler Arabellek Havuzunda (bellekte) ve veri sayfalarının kısmi (kaplamasız) önbelleğinde depolanır. Bu yerel RBPEX önbelleği, işlem boyutuyla orantılı olarak boyutlandırılır ve işlem katmanının üç katı bellektir. RBPEX, en sık erişilen verilere sahip olması nedeniyle Buffer Pool'a benzer. Diğer taraftan, her sayfa sunucusu, veritabanının koruduğu bölümü için bir kaplama RBPEX önbelleğine sahiptir.
 
Bir işlem yinelemesinde okuma yayımlandığında, veri Arabellek Havuzu'nda veya yerel RBPEX önbelleğinde yoksa, getPage (pageId, LSN) işlev çağrısı verilir ve sayfa ilgili sayfa sunucusundan alınır. Sayfa sunucularından gelen okumalar uzaktan okunur ve böylece yerel RBPEX'ten alınan okumalardan daha yavaşdır. IO ile ilgili performans sorunlarını giderirken, nispeten daha yavaş uzak sayfa sunucusu okumaları aracılığıyla kaç iOs yapıldığını söyleyebilmemiz gerekir.

Birkaç DMV ve genişletilmiş olaylar, bir sayfa sunucusundan gelen uzak okuma sayısını belirten sütunlara ve alanlara sahiptir ve bu da toplam okumasayısıyla karşılaştırılabilir. Sorgu deposu da sorgu çalıştırma süresi istatistiklerinin bir parçası olarak uzaktan okuma yakalar.

- Sayfa sunucusu okumalarını rapor etmek için sütunlar yürütme DMV'leri ve katalog görünümleri gibi mevcuttur:
    - [_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql/)
    - [_exec_query_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql/)
    - [sys.dm_exec_procedure_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql/)
    - [sys.dm_exec_trigger_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql/)
    - [sys.query_store_runtime_stats](/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql/)
- Sayfa sunucusu okumaları aşağıdaki genişletilmiş olaylara eklenir:
    - sql_statement_completed
    - sp_statement_completed
    - sql_batch_completed
    - rpc_completed
    - scan_stopped
    - query_store_begin_persist_runtime_stat
    - sorgu-store_execution_runtime_info
- ActualPageServerReads/ActualPageServerReadAheads gerçek planlar için XML sorgu planına eklenir. Örnek:

`<RunTimeCountersPerThread Thread="8" ActualRows="90466461" ActualRowsRead="90466461" Batches="0" ActualEndOfScans="1" ActualExecutions="1" ActualExecutionMode="Row" ActualElapsedms="133645" ActualCPUms="85105" ActualScans="1" ActualLogicalReads="6032256" ActualPhysicalReads="0" ActualPageServerReads="0" ActualReadAheads="6027814" ActualPageServerReadAheads="5687297" ActualLobLogicalReads="0" ActualLobPhysicalReads="0" ActualLobPageServerReads="0" ActualLobReadAheads="0" ActualLobPageServerReadAheads="0" />`

> [!NOTE]
> Sorgu planı özellikleri penceresinde bu öznitelikleri görüntülemek için SSMS 18.3 veya daha sonra gereklidir.

## <a name="virtual-file-stats-and-io-accounting"></a>Sanal dosya istatistikleri ve IO muhasebesi

Azure SQL Veritabanı'nda, [sys.dm_io_virtual_file_stats()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) DMF, SQL Server IO'yu izlemenin birincil yoludur. Hiperölçekte IO özellikleri [dağıtılmış mimarisi](sql-database-service-tier-hyperscale.md#distributed-functions-architecture)nedeniyle farklıdır. Bu bölümde, bu DMF'de görüldüğü gibi veri dosyalarına IO (okuma ve yazma) üzerinde odaklanıyoruz. Hyperscale'de, bu DMF'de görünen her veri dosyası uzak bir sayfa sunucusuna karşılık gelir. Burada bahsedilen RBPEX önbelleği, işlem yinelemesi üzerinde kaplamaolmayan bir önbellek olan yerel bir SSD tabanlı önbellektir.

### <a name="local-rbpex-cache-usage"></a>Yerel RBPEX önbellek kullanımı

Yerel RBPEX önbelleği, işlem yinelemesi üzerinde, yerel SSD depolamada bulunur. Böylece, bu önbelleğe karşı IO uzak sayfa sunucularına karşı IO daha hızlıdır. Şu anda, bir Hyperscale veritabanında [sys.dm_io_virtual_file_stats()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) işlem yinelemesi üzerinde yerel RBPEX önbelleğine karşı IO raporlama özel bir satır vardır. Bu satır, hem de `database_id` `file_id` sütunlar için 0 değerine sahiptir. Örneğin, aşağıdaki sorgu veritabanı başlangıcından beri RBPEX kullanım istatistiklerini döndürür.

`select * from sys.dm_io_virtual_file_stats(0,NULL);`

RBPEX'te yapılan okumaoranı, diğer tüm veri dosyalarında yapılan toplu okumaoranı RBPEX önbellek isabet oranı sağlar.

### <a name="data-reads"></a>Veri okuma

- Okumalar, SQL Server altyapısı tarafından bir bilgi işlem yinelemesi üzerinde verildiğinde, bunlara yerel RBPEX önbelleği veya uzak sayfa sunucuları veya birden çok sayfa yı okurken ikisinin birleşimi tarafından sunulabilir.
- Bilgi işlem yinelemesi belirli bir dosyadan bazı sayfaları okuduğunda ( örneğin 1 file_id, bu veriler yalnızca yerel RBPEX önbelleğinde yer alıyorsa, bu okumaiçin tüm IO file_id 0 (RBPEX) ile birlikte muhasebeleştirilmiştir. Bu verilerin bir kısmı yerel RBPEX önbelleğindeyse ve bir kısmı uzak bir sayfa sunucusundaysa, IO RBPEX'ten sunulan parça için file_id 0'a doğru muhasebeleştirilir ve uzak sayfa sunucusundan sunulan parça file_id 1'e doğru muhasebeleştirilir. 
- Bir işlem çoğaltma, bir sayfa sunucusundan belirli bir [LSN'de](/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide/) bir sayfa istediğinde, sayfa sunucusu istenen LSN'yi yakalayamadıysa, sayfa sunucusu sayfa nın bilgi işlem yinelemesine döndürülmeden önce sayfa sunucusu nun yakalamasını bekler. Bilgi işlem yinelemesindeki bir sayfa sunucusundan okunan herhangi bir okuma için, bu IO'yu bekliyorsa PAGEIOLATCH_* bekleme türünü görürsünüz. Hyperscale'de bu bekleme süresi, hem sayfa sunucusunda istenen sayfayı lsn'ye yetişmek için gereken süreyi hem de sayfayı sayfa sunucusundan bilgi işlem yinelemesine aktarmak için gereken süreyi içerir.
- Okuma-ileri gibi büyük okumalar genellikle ["Scatter-Gather" Reads](/sql/relational-databases/reading-pages/)kullanılarak yapılır. Bu, SQL Server altyapısında tek bir okuma olarak kabul edilen, aynı anda en fazla 4 MB sayfa okuma olanağı sağlar. Ancak, okunan veriler RBPEX olduğunda, bu okumalar birden fazla tek tek 8 KB okur olarak muhasebeleştirilir, çünkü arabellek havuzu ve RBPEX her zaman 8 KB sayfa kullanır. Sonuç olarak, RBPEX'e karşı görülen okuma iOs sayısı, motor tarafından gerçekleştirilen gerçek IOs sayısından daha büyük olabilir.

### <a name="data-writes"></a>Veri yazma

- Birincil işlem yinelemesi doğrudan sayfa sunucularına yazmıyor. Bunun yerine, Log hizmetinin günlük kayıtları ilgili sayfa sunucularında yeniden oynatılır. 
- Hesaplama çoğaltma olur yazıyor ağırlıklı olarak yerel RBPEX (file_id 0) yazıyor. 8 KB'den daha büyük mantıksal dosyalarüzerinde yazmak için, başka bir deyişle bu [Topla-yazma](/sql/relational-databases/writing-pages/)kullanılarak yapılan , her yazma işlemi birden fazla 8 KB bireysel rbpex için tampon havuzu ve RBPEX her zaman 8 KB sayfaları kullanmak beri yazar tercüme edilir. Sonuç olarak, RBPEX'e karşı görülen yazma iOs sayısı, motor tarafından gerçekleştirilen gerçek IOs sayısından daha büyük olabilir.
- RBPEX olmayan dosyalar veya sayfa sunucularına karşılık gelen file_id 0 dışındaki veri dosyaları da yazılarını gösterir. Hiper ölçek hizmet katmanında, bu yazılar benzetilir, çünkü işlem yinelemeleri hiçbir zaman doğrudan sayfa sunucularına yazmaz. IOPS ve iş bölümü, bilgi işlem yinelemesi üzerinde oluştukları gibi muhasebeleştirilir, ancak file_id 0 dışındaki veri dosyalarının gecikmesi, sayfa sunucusunun yazdığı gerçek gecikme süresini yansıtmaz.

### <a name="log-writes"></a>Günlük yazıyor

- Birincil işlemde, bir günlük yazma sys.dm_io_virtual_file_stats file_id 2 olarak muhasebelenir. Birincil işlem de bir günlük yazma günlük İniş Bölgesi için bir yazıdır.
- Günlük kayıtları, bir commit'daki ikincil yinelemede sertleşmez. Hyperscale'de günlük, Log hizmeti tarafından ikincil yinelemelere eşit olarak uygulanır. Günlük yazmaları aslında ikincil yinelemelerde oluşmadığından, ikincil yinelemelerde Log IOs'ların herhangi bir muhasebesi yalnızca izleme amaçlıdır.

## <a name="data-io-in-resource-utilization-statistics"></a>Kaynak kullanım istatistiklerinde Veri IO

Hiperölçekli olmayan bir veritabanında, [kaynak yönetişim](/azure/sql-database/sql-database-resource-limits-database-server#resource-governance) veri IOPS sınırına göre veri dosyalarına karşı IOPS'yi birleştirilmiş okuma ve yazma, `avg_data_io_percent` sütunda [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) görünümlerinde bildirilir. Aynı değer portalda Veri _IO Yüzdesi_olarak raporlanır. 

Bir Hyperscale veritabanında, bu sütun veri IOPS kullanımı sadece bilgi işlem çoğaltma yerel depolama için sınırı nazaran raporlar, RBPEX karşı özellikle IO ve `tempdb`. Bu sütundaki %100 değer, kaynak yönetiminin yerel depolama IOPS'yi sınırladığını gösterir. Bu bir performans sorunu yla ilişkiliyse, daha az IO oluşturmak için iş yükünü ayarlayın veya kaynak yönetimi _Max Data IOPS_ [sınırını](sql-database-vcore-resource-limits-single-databases.md)artırmak için veritabanı hizmet hedefini artırın. RBPEX okur ve yazma kaynak yönetimi için, sistem sql server motoru tarafından verilebilir büyük IOs yerine tek tek 8 KB IOs sayar. 

Uzak sayfa sunucularına karşı veri IO kaynak kullanımı görünümleri veya portalda rapor edilmez, ancak daha önce belirtildiği gibi [sys.dm_io_virtual_file_stats()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) DMF'de bildirilir.


## <a name="additional-resources"></a>Ek kaynaklar

- Bir Hyperscale tek veritabanı için vCore kaynak sınırları için [Bkz. Hyperscale hizmet katmanı vCore Limitleri](sql-database-vcore-resource-limits-single-databases.md#hyperscale---provisioned-compute---gen5)
- Azure SQL Veritabanı performans atonlama için Azure [SQL Veritabanı'nda Sorgu performansı](sql-database-performance-guidance.md)
- Sorgu Deposu'nun performansı nın alamı için [Sorgu deposu'nun kullanımıyla Performans izleme](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store/)
- DMV izleme komut dosyaları için [dinamik yönetim görünümlerini kullanarak performans](sql-database-monitoring-with-dmvs.md) Azure SQL Veritabanı'nı izleme
