---
title: DMVs kullanarak SQL havuzu iş yükünüzü izleme
description: DMVs kullanarak Azure Synapse Analytics SQL havuz iş yükünüzü ve sorgu yürütmenizi nasıl izleyeceğinizi öğrenin.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/24/2020
ms.author: rortloff
ms.reviewer: igorstan
ms.custom: synapse-analytics
ms.openlocfilehash: 5360d91a17a7eee2dd0373ac311c79d22e085939
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416094"
---
# <a name="monitor-your-azure-synapse-analytics-sql-pool-workload-using-dmvs"></a>DMVs kullanarak Azure Synapsanalytics SQL havuz iş yükünüzü izleme

Bu makalede, SQL havuzunda sorgu yürütmesini araştırma da dahil olmak üzere iş yükünüzü izlemek için Dinamik Yönetim Görünümleri'nin (DMV) nasıl kullanılacağı açıklanmaktadır.

## <a name="permissions"></a>İzinler

Bu makaledeki DMV'leri sorgulamak için **VIEW DATABASE STATE** veya **CONTROL** iznine ihtiyacınız vardır. Genellikle, ÇOK DAHA kısıtlayıcı olduğu için **VIEW DATABASE STATE** verilmesi tercih edilen izindir.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Bağlantıları izleme

Veri ambarınızdaki tüm girişler [sys.dm_pdw_exec_sessions'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)kaydedilir.  Bu DMV son 10.000 girişi içerir.  session_id birincil anahtardır ve her yeni oturum açma için sırayla atanır.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Sorgu yürütmeyi izleyin

SQL havuzunda yürütülen tüm sorgular [sys.dm_pdw_exec_requests'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)kaydedilir.  Bu DMV, yürütülen son 10.000 sorguyu içerir.  request_id benzersiz her sorgu tanımlar ve bu DMV için birincil anahtarıdır.  request_id her yeni sorgu için sırayla atanır ve sorgu kimliği anlamına gelen QID ile önceden belirlenmiştir.  Belirli bir session_id için bu DMV'yi sorgulamak, belirli bir oturum açmanın tüm sorgularını gösterir.

> [!NOTE]
> Depolanan yordamlar birden çok İstek T'leri kullanır.  İstek işleri ardından düşürülüyor.

Burada, sorgu yürütme planlarını ve belirli bir sorguiçin süreleri araştırmak için izlenir.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>ADIM 1: Araştırmak istediğiniz sorguyu tanımlayın

```sql
-- Monitor active queries
SELECT *
FROM sys.dm_pdw_exec_requests
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 *
FROM sys.dm_pdw_exec_requests
ORDER BY total_elapsed_time DESC;

```

Önceki sorgu sonuçlarından, araştırmak istediğiniz sorgunun **İstek Kimliğini not edin.**

**Askıya Alınan** durumdaki sorgular, çok sayıda etkin çalışan sorgu nedeniyle sıraya alınabilir. Bu sorgular [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) userConcurrencyResourceType türüile sorgu bekler de görünür. Eşzamanlılık sınırları hakkında bilgi için, iş yükü yönetimi için [Bellek ve eşzamanlılık sınırları](memory-concurrency-limits.md) veya Kaynak [sınıfları'na](resource-classes-for-workload-management.md)bakın. Sorgular nesne kilitleri gibi diğer nedenlerle de bekleyebilir.  Sorgunuz bir kaynak bekliyorsa, bu makalede kaynakların daha aşağıda [bekleyerek sorguları araştırma](#monitor-waiting-queries) başlıklı makalesine bakın.

[sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) tablosundaki bir sorgunun görünümünü basitleştirmek için, sys.dm_pdw_exec_requests görünümünde aranabilen sorgunuza yorum atamak için [LABEL'yi](/sql/t-sql/queries/option-clause-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanın.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

### <a name="step-2-investigate-the-query-plan"></a>ADIM 2: Sorgu planını araştırın

Sorgunun dağıtılmış SQL (DSQL) planını [sys.dm_pdw_request_steps'dan](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) almak için İstek Kimliğini kullanın

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Bir DSQL planı beklenenden daha uzun sürüyorsa, bunun nedeni birçok DSQL adımı içeren karmaşık bir plan veya yalnızca bir adım ın uzun zaman aldığı olabilir.  Plan, birkaç taşıma işlemiyle birlikte birçok adımdan biriyse, veri hareketini azaltmak için tablo dağılımlarınızı en iyi duruma düşürmeyi düşünün. [Tablo dağıtım](sql-data-warehouse-tables-distribute.md) makalesi, bir sorguyu çözmek için verilerin neden taşınması gerektiğini açıklar. Makalede ayrıca veri hareketini en aza indirmek için bazı dağıtım stratejileri açıklanmaktadır.

Tek bir adım la ilgili daha fazla ayrıntıyı araştırmak için, uzun süren sorgu adımının *operation_type* sütunu ve **Adım Dizini'ni**not edin:

* **SQL işlemleri**için Adım 3a ile devam edin : OnOperation, RemoteOperation, ReturnOperation.
* **Veri Hareketi işlemleri**için Adım 3b ile devam edin: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3-investigate-sql-on-the-distributed-databases"></a>ADIM 3: Dağıtılmış veritabanlarında SQL'i inceleyin

Tüm dağıtılmış veritabanlarında sorgu adımının yürütme bilgilerini içeren [sys.dm_pdw_sql_requests'den](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)ayrıntıları almak için İstek Kimliğini ve Adım Dizini'ni kullanın.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Sorgu adımı çalışırken, [DBCC PDW_SHOWEXECUTIONPLAN,](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) belirli bir dağıtımda çalışan adım için SQL Server planı önbelleğinden SQL Server tahmini planını almak için kullanılabilir.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL pool or control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-4-investigate-data-movement-on-the-distributed-databases"></a>ADIM 4: Dağıtılmış veritabanlarındaki veri hareketini araştırma

[Sys.dm_pdw_dms_workers'den](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)her dağıtımda çalışan bir veri hareketi adımı hakkında bilgi almak için İstek Kimliği'ni ve Adım Dizini'ni kullanın.

```sql
-- Find information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Belirli bir dağıtımın veri hareketi için diğerlerinden önemli ölçüde daha uzun sürdüğünü görmek için *total_elapsed_time* sütununa bakın.
* Uzun süren dağıtım için, bu dağıtımdan taşınan satır sayısının diğerlerinden önemli ölçüde daha büyük olup olmadığını görmek için *rows_processed* sütununa bakın. Bu nedenle, bu bulgu altta yatan verilerinizin çarpıkolduğunu gösterebilir.

Sorgu çalışıyorsa, belirli bir dağıtım içinde şu anda çalışan SQL Step için SQL Server planı önbelleğinden SQL Server tahmini planını almak için [DBCC PDW_SHOWEXECUTIONPLAN](/sql/t-sql/database-console-commands/dbcc-pdw-showexecutionplan-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL pool Compute or control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Bekleme sorgularını izleme

Bir kaynak beklediği için sorgunuzun ilerleme kaydetmediğini fark ederseniz, sorgunun beklediği tüm kaynakları gösteren bir sorgu aşağıda veda edebilirsiniz.

```sql
-- Find queries
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Sorgu etkin olarak başka bir sorgudan kaynakları bekliyorsa, durum **AcquireResources**olacaktır.  Sorgu gerekli tüm kaynaklara sahipse, durum **verilir.**

## <a name="monitor-tempdb"></a>Tempdb'yi izleyin

Tempdb sorgu yürütme sırasında ara sonuçları tutmak için kullanılır. Tempdb veritabanının yüksek kullanımı yavaş sorgu performansına yol açabilir. Yapılandırılan her DW100c için 399 GB tempdb alanı ayrılır (DW1000c toplam tempdb alanı 3,99 TB olurdu).  Aşağıda tempdb kullanımını izlemek ve sorgularınızda tempdb kullanımını azaltmak için ipuçları verilmiştir.

### <a name="monitoring-tempdb-with-views"></a>Tempdb'yi görünümlerle izleme

Tempdb kullanımını izlemek için önce [MICROSOFT Toolkit for SQL havuzundan](https://github.com/Microsoft/sql-data-warehouse-samples/tree/master/solutions/monitoring) [microsoft.vw_sql_requests](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/solutions/monitoring/scripts/views/microsoft.vw_sql_requests.sql) görünümünü yükleyin. Daha sonra tüm çalıştırılan sorgular için düğüm başına tempdb kullanımını görmek için aşağıdaki sorguyu çalıştırabilirsiniz:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN microsoft.vw_sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa'
ORDER BY sr.request_id;
```

Büyük miktarda bellek tüketen bir sorgunuz varsa veya tempdb'nin tahsisi ile ilgili bir hata iletisi aldıysanız, bunun nedeni son veri hareketi işleminde başarısız olan çok büyük bir [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) veya [INSERT SELECT](/sql/t-sql/statements/insert-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimi olabilir. Bu genellikle son INSERT SELECT'ten hemen önce dağıtılmış sorgu planında ShuffleMove işlemi olarak tanımlanabilir.  ShuffleMove işlemlerini izlemek için [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanın.

En yaygın azaltma, CTAS veya INSERT SELECT deyiminizi birden çok yük deyimine ayırmaktır, böylece veri hacmi düğüm başına 1 TB tempdb sınırını aşmaz. Ayrıca, kümenizi daha büyük bir boyuta ölçeklendirerek tempdb boyutunu daha fazla düğüme yayarak her bir düğümdeki tempdb'yi azaltabilirsiniz.

CTAS ve INSERT SELECT ifadelerine ek olarak, yetersiz bellekle çalışan büyük, karmaşık sorgular tempdb'ye dökülerek sorguların başarısız lığa neden olabilir.  Tempdb'ye dökülmeyi önlemek için daha büyük bir [kaynak sınıfıyla](resource-classes-for-workload-management.md) çalıştırmayı düşünün.

## <a name="monitor-memory"></a>Belleği izleme

Bellek yavaş performans ve bellek sorunları dışında kök nedeni olabilir. Sorgu yürütme sırasında SQL Server bellek kullanımının sınırlarına ulaştığını görürseniz veri ambarınızı ölçeklemeyi düşünün.

Aşağıdaki sorgu, SQL Server bellek kullanımını ve düğüm başına bellek basıncını döndürür:

```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB,
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```

## <a name="monitor-transaction-log-size"></a>Hareket günlüğü boyutunu izleme

Aşağıdaki sorgu, her dağıtımda hareket günlüğü boyutunu döndürür. Günlük dosyalarından biri 160 GB'a ulaşıyorsa, örneğini ölçeklemeyi veya işlem boyutunuzu sınırlamayı düşünmelisiniz.

```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id
FROM sys.dm_pdw_nodes_os_performance_counters
WHERE
instance_name like 'Distribution_%'
AND counter_name = 'Log File(s) Used Size (KB)'
```

## <a name="monitor-transaction-log-rollback"></a>Hareket günlüğü geri alma işlemini izleme

Sorgularınız başarısız oluyorsa veya devam etmek uzun sürüyorsa, geri alma işleminiz olup olmadığını kontrol edebilir ve izleyebilirsiniz.

```sql
-- Monitor rollback
SELECT
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="monitor-polybase-load"></a>PolyBase yükünü izleyin

Aşağıdaki sorgu, yükünüzün ilerlemesinin yaklaşık bir tahminini sağlar. Sorgu yalnızca şu anda işlenen dosyaları gösterir.

```sql

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files,
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="next-steps"></a>Sonraki adımlar

DMV'ler hakkında daha fazla bilgi için [Sistem görünümleri'ne](../sql/reference-tsql-system-views.md)bakın.
