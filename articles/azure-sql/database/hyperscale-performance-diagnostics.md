---
title: Hiper ölçekte Performans Tanılama
description: Bu makalede, Azure SQL veritabanı 'nda hiper ölçekli performans sorunlarının nasıl giderileceği açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: seo-lt-2019 sqldbrb=1
ms.topic: troubleshooting
author: denzilribeiro
ms.author: denzilr
ms.reviewer: sstein
ms.date: 10/18/2019
ms.openlocfilehash: c9b69b751067ba36daad614b84367aee882d17b1
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84051951"
---
# <a name="sql-hyperscale-performance-troubleshooting-diagnostics"></a>SQL hiper ölçek performans sorunlarını giderme tanılaması
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Hiper ölçekli bir veritabanında performans sorunlarını gidermek için, Azure SQL veritabanı işlem düğümündeki [genel performans ayarlama yöntemleri](monitor-tune-overview.md) , performans araştırmasının başlangıç noktasıdır. Ancak, Hyperscale 'nin [Dağıtılmış mimarisi](service-tier-hyperscale.md#distributed-functions-architecture) verildiğinde yardım 'a ek Tanılamalar eklenmiştir. Bu makalede, hiper ölçeğe özgü Tanılama verileri açıklanmaktadır.

## <a name="log-rate-throttling-waits"></a>Günlük hızı azaltma bekler

Her Azure SQL veritabanı hizmet düzeyinde günlük oluşturma oranı sınırlamaları, [günlük hızı](resource-limits-logical-server.md#transaction-log-rate-governance)yönetimi aracılığıyla uygulanır. Hiper ölçekte, günlük oluşturma sınırı hizmet düzeyinden bağımsız olarak şu anda 100 MB/sn olarak ayarlanmıştır. Ancak, birincil işlem çoğaltmasındaki günlük oluşturma hızının, kurtarılabilirlik sağlamak için kısıtlandığı durumlar vardır. Bu kısıtlama, bir [sayfa sunucusu veya başka bir işlem çoğaltması](service-tier-hyperscale.md#distributed-functions-architecture) , günlük hizmetinden yeni günlük kayıtlarının uygulanması önemli ölçüde geride olduğunda gerçekleşir.

Aşağıdaki bekleme türleri ( [sys. dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql/)' de), günlük hızının birincil işlem çoğaltmasında nasıl kısıtlanamadığına ilişkin nedenleri anlatmaktadır:

|Bekleme türü    |Açıklama                         |
|-------------          |------------------------------------|
|RBIO_RG_STORAGE        | Bir hiper ölçek veritabanı birincil işlem düğümü günlük oluşturma hızı, sayfa sunucusunda gecikmeli günlük kullanımı nedeniyle kısıtlanmakta olduğunda gerçekleşir.         |
|RBIO_RG_DESTAGE        | Bir hiper ölçek veritabanı işlem düğüm günlüğü oluşturma hızı, uzun süreli günlük depolama tarafından Gecikmeli günlük tüketimine karşı kısıtlanmakta olduğunda gerçekleşir.         |
|RBIO_RG_REPLICA        | Bir hiper ölçek veritabanı işlem düğüm günlüğü oluşturma hızı, okunabilir ikincil çoğaltmalar tarafından Gecikmeli günlük tüketimine karşı kısıtlanmakta olduğunda gerçekleşir.         |
|RBIO_RG_LOCALDESTAGE   | Bir hiper ölçek veritabanı işlem düğüm günlüğü oluşturma hızı, günlük hizmeti tarafından Gecikmeli günlük tüketimine karşı kısıtlanmakta olduğunda gerçekleşir.         |

## <a name="page-server-reads"></a>Sayfa sunucusu okuma

İşlem çoğaltmaları veritabanının tam bir kopyasını yerel olarak önbelleğe vermez. İşlem çoğaltmasındaki yerel veriler, arabellek havuzunda (bellekte) ve veri sayfalarının kısmi (kapsayan olmayan) önbelleği olan yerel dayanıklı arabellek havuzu uzantısı (RBPEX) önbelleğinde depolanır. Bu yerel RBPEX önbelleği, işlem boyutuna orantılı bir şekilde boyutlandırılır ve işlem katmanının üç katı olur. RBPEX, en sık erişilen verileri içeren arabellek havuzuna benzerdir. Diğer taraftaki her sayfa sunucusunda, tuttuğu veritabanının bölümü için kapsayan bir RBPEX önbelleği vardır.

Bir işlem çoğaltmasında okuma yapıldığında, veriler arabellek havuzunda veya yerel RBPEX önbelleğinde yoksa bir getPage (PageId, LSN) işlev çağrısı verilir ve sayfa ilgili sayfa sunucusundan getirilir. Sayfa sunucularından gelen okumalar uzaktan okumalardır ve bu nedenle yerel RBPEX 'dan okumalarından daha yavaştır. GÇ ile ilgili performans sorunları giderirken, görece yavaş uzak sayfa sunucusu okumaları ile kaç tane IOs yapıldığını anladık.

Birçok DMVs ve genişletilmiş olay, bir sayfa sunucusundan uzaktan okuma sayısını belirten sütunlara ve alanlara sahiptir ve bu, toplam okumaların karşılaştırılabileceği anlamına gelir. Sorgu deposu Ayrıca sorgu çalışma zamanı istatistiklerinin bir parçası olarak uzaktan okumaları de yakalar.

- Sayfa sunucusu okumaları raporlamak için sütunlar, yürütme DMVs ve katalog görünümlerinde mevcuttur, örneğin:

  - [sys. dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql/)
  - [sys. dm_exec_query_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql/)
  - [sys. dm_exec_procedure_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql/)
  - [sys. dm_exec_trigger_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql/)
  - [sys. query_store_runtime_stats](/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql/)
- Sayfa sunucusu okuma aşağıdaki genişletilmiş olaylara eklenir:
  - sql_statement_completed
  - sp_statement_completed
  - sql_batch_completed
  - rpc_completed
  - scan_stopped
  - query_store_begin_persist_runtime_stat
  - sorgu-store_execution_runtime_info
- Gerçek planlar için sorgu planı XML 'e Actualpageserverokumaları/ActualPageServerReadAheads eklenir. Örneğin:

`<RunTimeCountersPerThread Thread="8" ActualRows="90466461" ActualRowsRead="90466461" Batches="0" ActualEndOfScans="1" ActualExecutions="1" ActualExecutionMode="Row" ActualElapsedms="133645" ActualCPUms="85105" ActualScans="1" ActualLogicalReads="6032256" ActualPhysicalReads="0" ActualPageServerReads="0" ActualReadAheads="6027814" ActualPageServerReadAheads="5687297" ActualLobLogicalReads="0" ActualLobPhysicalReads="0" ActualLobPageServerReads="0" ActualLobReadAheads="0" ActualLobPageServerReadAheads="0" />`

> [!NOTE]
> Bu öznitelikleri sorgu planı özellikleri penceresinde görüntülemek için SSMS 18,3 veya üzeri gereklidir.

## <a name="virtual-file-stats-and-io-accounting"></a>Sanal dosya istatistikleri ve GÇ hesabı

Azure SQL veritabanı 'nda, [sys. dm_io_virtual_file_stats ()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) DMF, SQL veritabanı GÇ 'yi izlemenin birincil yoludur. Hyperscale içindeki GÇ özellikleri, [Dağıtılmış mimarisi](service-tier-hyperscale.md#distributed-functions-architecture)nedeniyle farklıdır. Bu bölümde, bu DMF 'de görüldüğü gibi veri dosyalarına GÇ 'ye (okuma ve yazma) odaklanıyoruz. Hiper ölçekte, bu DMF 'de görünür olan her veri dosyası uzak bir sayfa sunucusuna karşılık gelir. Burada bahsedilen RBPEX önbelleği, işlem çoğaltmasındaki kapsayan olmayan bir önbellek olan yerel bir SSD tabanlı önbelleğidir.

### <a name="local-rbpex-cache-usage"></a>Yerel RBPEX önbelleği kullanımı

Yerel bir SSD depolamada, işlem çoğaltmasında yerel RBPEX önbelleği var. Bu nedenle, bu önbellekteki GÇ, uzak sayfa sunucularında GÇ 'den daha hızlıdır. Şu anda, bir hiper ölçek veritabanında bulunan [sys. dm_io_virtual_file_stats ()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) , işlem çoğaltmasındaki yerel RBPEX ÖNBELLEĞINE karşı GÇ 'yi bildiren özel bir satıra sahiptir. Bu satırda hem hem de sütunları için 0 değeri `database_id` bulunur `file_id` . Örneğin, aşağıdaki sorgu, veritabanının başlamasından itibaren RBPEX kullanım istatistikleri döndürüyor.

`select * from sys.dm_io_virtual_file_stats(0,NULL);`

Tüm diğer veri dosyalarında yapılan toplanmış okumaların, RBPEX üzerinde yapılan okumaların oranı, RBPEX isabetli önbellek okuması oranı sağlar.

### <a name="data-reads"></a>Veri okuma

- Okuma işlemleri, SQL veritabanı altyapısı tarafından bir işlem çoğaltmasında verildiğinde, yerel RBPEX önbelleği veya uzak sayfa sunucuları tarafından ya da birden çok sayfa okunıyorsa ikisinin bir birleşimi tarafından sunuluyor olabilir.
- İşlem çoğaltması belirli bir dosyadaki bazı sayfaları okuduğunda, örneğin file_id 1, bu veriler yalnızca yerel RBPEX önbelleğinde bulunuyorsa, bu okuma için tüm GÇ 'ler file_id 0 (RBPEX) ile hesaba katılmaz. Bu verilerin bir bölümü yerel RBPEX önbelleğinde ise ve bazı bölümleri uzak sayfa sunucusunda ise, bu durumda, RBPEX 'tan sunulan bölüm için GÇ file_id ve uzak sayfa sunucusundan sunulan Bölüm file_id 1 ' e doğru bir şekilde hesaba katılmaz.
- Bir işlem çoğaltması bir sayfa sunucusundan belirli bir [LSN](/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide/) 'nin bir sayfasını istediğinde, sayfa sunucusu istenen LSN 'ye bakmadığında, işlem çoğaltmasındaki okuma işlemi sayfa sunucusu işlem çoğaltmasına döndürülmeden önce yakalanana kadar bekler. İşlem çoğaltmasında bir sayfa sunucusundan okunan her türlü, bu GÇ üzerinde bekliyorsa, PAGEIOLATCH_ * bekleme türünü görürsünüz. Hiper ölçekte bu bekleme süresi, sayfa sunucusundaki istenen sayfayı gereken LSN 'ye ve sayfa sunucusundan bilgi işlem çoğaltmasına aktarmak için gereken süreyi de içerir.
- Okuma sonrası gibi büyük okumalar genellikle ["dağılım-toplama" okumaları](/sql/relational-databases/reading-pages/)kullanılarak yapılır. Bu, aynı anda 4 MB 'a kadar sayfa okumayı sağlar ve SQL veritabanı altyapısında tek bir okuma kabul edilir. Ancak, okunan veriler RBPEX ise, bu okumalar birden çok ayrı 8 KB okuma olarak hesaba katılmaz ve bu, arabellek havuzu ve RBPEX her zaman 8 KB boyutunda sayfa kullanır. Sonuç olarak, RBPEX 'ye karşı görülen okuma IOs sayısı, altyapı tarafından gerçekleştirilen gerçek IOs sayısından daha büyük olabilir.

### <a name="data-writes"></a>Veri yazma

- Birincil işlem çoğaltması doğrudan sayfa sunucularına yazmaz. Bunun yerine, günlük hizmetindeki günlük kayıtları ilgili sayfa sunucularında yeniden yürütülür.
- İşlem çoğaltmasında gerçekleşen yazma işlemleri, yerel RBPEX (file_id 0) ağırlıklı. 8 KB 'den büyük mantıksal dosyalardaki yazma işlemleri için, [toplama-yazma](/sql/relational-databases/writing-pages/)kullanılarak gerçekleştirilen diğer bir deyişle, her yazma işlemi, bellek havuzu ve rbpex her zaman 8 KB 'lık sayfa kullandığından, her yazma IŞLEMI, rbpex 'e birden fazla 8 KB 'a çevrilir. Sonuç olarak, RBPEX 'ye karşı görülen yazma IOs sayısı, altyapı tarafından gerçekleştirilen gerçek IOs sayısından daha büyük olabilir.
- RBPEX dosyaları veya sayfa sunucularına karşılık gelen file_id 0 dışında bir veri dosyası, aynı zamanda yazma işlemlerini gösterir. Hiper ölçek hizmet katmanında, işlem çoğaltmaları hiçbir şekilde doğrudan sayfa sunucularına yazmadığından bu yazma 'lar benzetilir. Yazma ıOPS ve aktarım hızı, işlem çoğaltmasında gerçekleştikleri şekilde hesaba katılmaz, ancak file_id 0 dışında bir veri dosyası gecikmesi sayfa sunucusu yazma işleminin gerçek gecikmesini yansıtmaz.

### <a name="log-writes"></a>Günlük yazma işlemleri

- Birincil işlem sırasında, file_id 2 ' nin sys. dm_io_virtual_file_stats ' de bir günlük yazma işlemi hesaba katılmaz. Birincil işlem üzerine bir günlük yazma, günlük giriş bölgesine bir yazma işlemi olur.
- Günlük kayıtları, bir işlemede ikincil çoğaltmada sağlamlaştırılmış değildir. Hiper ölçekte günlük hizmeti, günlük hizmeti tarafından zaman uyumsuz olarak uygulanır. Günlük yazma işlemleri ikincil çoğaltmalarda gerçekten gerçekleşmediğinden, ikincil çoğaltmalarda herhangi bir günlük IOs hesabı yalnızca izleme amaçlıdır.

## <a name="data-io-in-resource-utilization-statistics"></a>Kaynak Kullanım istatistiklerinde veri GÇ

Hiper olmayan ölçekli bir veritabanında, [kaynak idare](/azure/sql-database/sql-database-resource-limits-database-server#resource-governance) verileri IOPS sınırına göre veri dosyalarına yönelik Birleşik okuma ve Yazma IOPS 'si, sütununda [sys. dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) ve [sys. resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) görünümlerinde raporlanır `avg_data_io_percent` . Portalda _VERI GÇ yüzdesi_olarak aynı değer raporlanır.

Hiper ölçekli bir veritabanında, bu sütun yalnızca işlem çoğaltmasındaki yerel depolama alanı için olan sınıra göre veri ıOPS kullanımını bildirir ve özellikle RBPEX ve ile karşı GÇ `tempdb` . Bu sütundaki %100 değeri, kaynak yönetimi 'nin yerel depolama ıOPS 'yi sınırlandırdığını gösterir. Bu bir performans sorunuyla bağıntılı ise, daha az GÇ oluşturmak için iş yükünü ayarlayın veya kaynak yönetimi _en yüksek VERI IOPS_ [sınırını](resource-limits-vcore-single-databases.md)artırmak için veritabanı hizmeti hedefini artırın. RBPEX okumaları ve yazmaları 'nın kaynak idaresi için, sistem SQL veritabanı altyapısı tarafından verilebilen daha büyük IOs yerine ayrı 8 KB 'lık IOs sayısını sayar.

Kaynak Kullanımı görünümlerinde veya portalda, uzak sayfa sunucularındaki veri GÇ 'leri raporlanır, ancak daha önce belirtildiği gibi [sys. dm_io_virtual_file_stats ()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql/) Dmf içinde raporlanır.

## <a name="additional-resources"></a>Ek kaynaklar

- Tek bir hiper ölçekli tek bir veritabanı için vCore kaynak sınırları için bkz. [hyperscale Service Tier vCore limitleri](resource-limits-vcore-single-databases.md#hyperscale---provisioned-compute---gen5)
- Azure SQL veritabanı performans ayarlaması için bkz. [Azure SQL veritabanı 'Nda sorgu performansı](performance-guidance.md)
- Sorgu deposu kullanarak performans ayarlaması için bkz. [sorgu deposu kullanarak performans izleme](/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store/)
- DMV İzleme betikleri için bkz. [dinamik yönetim görünümlerini kullanarak performansı Izleme Azure SQL veritabanı](monitoring-with-dmvs.md)
