---
title: DMV'leri kullanarak performansı izleme
description: Microsoft Azure SQL Veritabanı'nı izlemek için dinamik yönetim görünümlerini kullanarak sık karşılaşılan performans sorunlarını nasıl algılayıp tanılayizleyeceğinizi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 958dcd441d35b5c28746ff79a0b341e5aa7383a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214021"
---
# <a name="monitoring-performance-azure-sql-database-using-dynamic-management-views"></a>Dinamik yönetim görünümlerini kullanarak performansı izleme Azure SQL Veritabanı

Microsoft Azure SQL Veritabanı, engellenen veya uzun süre çalışan sorgular, kaynak darboğazları, kötü sorgu planları ve benzerlerinden kaynaklanabilecek performans sorunlarını tanılamak için dinamik yönetim görünümleri alt kümesini etkinleştiri. Bu konu, dinamik yönetim görünümleri kullanarak yaygın performans sorunlarının nasıl algılanabildiği hakkında bilgi sağlar.

SQL Veritabanı kısmen dinamik yönetim görünümleri üç kategoride destekler:

- Veritabanı ile ilgili dinamik yönetim görünümleri.
- Yürütmeyle ilgili dinamik yönetim görünümleri.
- İşlemle ilgili dinamik yönetim görünümleri.

Dinamik yönetim görünümleri hakkında ayrıntılı bilgi için SQL Server Books Online'da [Dinamik Yönetim Görünümleri ve Fonksiyonları (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) adresine bakın.

## <a name="permissions"></a>İzinler

SQL Veritabanı'nda dinamik bir yönetim görünümünü sorgulamak **IÇIN VIEW DATABASE STATE** izinleri gerekir. **VIEW DATABASE STATE** izni, geçerli veritabanındaki tüm nesneler le ilgili bilgileri döndürür.
**View DATABASE STATE** iznini belirli bir veritabanı kullanıcısına vermek için aşağıdaki sorguyu çalıştırın:

```sql
GRANT VIEW DATABASE STATE TO database_user;
```

Şirket içi SQL Server örneğinde, dinamik yönetim görünümleri sunucu durumu bilgilerini döndürer. SQL Veritabanı'nda, yalnızca geçerli mantıksal veritabanınızla ilgili bilgileri döndürerler.

Bu makalede, aşağıdaki tür sorgu performans sorunlarını algılamak için SQL Server Management Studio veya Azure Data Studio kullanarak yürütebileceğiniz DMV sorguları koleksiyonu içerir:

- [Aşırı CPU tüketimiyle ilgili sorguları tanımlama](#identify-cpu-performance-issues)
- [PAGELATCH_* ve WRITE_LOG IO darboğazları ile ilgili bekler](#identify-io-performance-issues)
- [PAGELATCH_* tarafından neden bekler TempDB çekişme](#identify-tempdb-performance-issues)
- [RESOURCE_SEMAHPORE bellek hibe bekleme sorunları nedeniyle bekler](#identify-memory-grant-wait-performance-issues)
- [Veritabanı ve nesne boyutlarını tanımlama](#calculating-database-and-objects-sizes)
- [Etkin oturumlar hakkında bilgi alma](#monitoring-connections)
- [Sistem genelinde ve veritabanı kaynak kullanım bilgilerini alma](#monitor-resource-use)
- [Sorgu performans bilgilerini alma](#monitoring-query-performance)

## <a name="identify-cpu-performance-issues"></a>CPU performans sorunlarını belirleme

CPU tüketimi uzun süreler için %80'in üzerindeyse, aşağıdaki sorun giderme adımlarını göz önünde bulundurun:

### <a name="the-cpu-issue-is-occurring-now"></a>CPU sorunu şimdi oluşuyor

Sorun şu anda oluşuyorsa, iki olası senaryo vardır:

#### <a name="many-individual-queries-that-cumulatively-consume-high-cpu"></a>Kümülatif olarak yüksek CPU tüketen birçok ayrı sorgu

Üst sorgu işlerini tanımlamak için aşağıdaki sorguyu kullanın:

```sql
PRINT '-- top 10 Active CPU Consuming Queries (aggregated)--';
SELECT TOP 10 GETDATE() runtime, *
FROM (SELECT query_stats.query_hash, SUM(query_stats.cpu_time) 'Total_Request_Cpu_Time_Ms', SUM(logical_reads) 'Total_Request_Logical_Reads', MIN(start_time) 'Earliest_Request_start_Time', COUNT(*) 'Number_Of_Requests', SUBSTRING(REPLACE(REPLACE(MIN(query_stats.statement_text), CHAR(10), ' '), CHAR(13), ' '), 1, 256) AS "Statement_Text"
    FROM (SELECT req.*, SUBSTRING(ST.text, (req.statement_start_offset / 2)+1, ((CASE statement_end_offset WHEN -1 THEN DATALENGTH(ST.text)ELSE req.statement_end_offset END-req.statement_start_offset)/ 2)+1) AS statement_text
          FROM sys.dm_exec_requests AS req
                CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) AS ST ) AS query_stats
    GROUP BY query_hash) AS t
ORDER BY Total_Request_Cpu_Time_Ms DESC;
```

#### <a name="long-running-queries-that-consume-cpu-are-still-running"></a>CPU tüketen uzun süren sorgular hala çalışıyor

Bu sorguları tanımlamak için aşağıdaki sorguyu kullanın:

```sql
PRINT '--top 10 Active CPU Consuming Queries by sessions--';
SELECT TOP 10 req.session_id, req.start_time, cpu_time 'cpu_time_ms', OBJECT_NAME(ST.objectid, ST.dbid) 'ObjectName', SUBSTRING(REPLACE(REPLACE(SUBSTRING(ST.text, (req.statement_start_offset / 2)+1, ((CASE statement_end_offset WHEN -1 THEN DATALENGTH(ST.text)ELSE req.statement_end_offset END-req.statement_start_offset)/ 2)+1), CHAR(10), ' '), CHAR(13), ' '), 1, 512) AS statement_text
FROM sys.dm_exec_requests AS req
    CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) AS ST
ORDER BY cpu_time DESC;
GO
```

### <a name="the-cpu-issue-occurred-in-the-past"></a>CPU sorunu geçmişte oluştu

Sorun geçmişte oluştuysa ve kök neden çözümlemesi yapmak istiyorsanız, [Sorgu Deposu'nu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)kullanın. Veritabanı erişimi olan kullanıcılar Sorgu Deposu verilerini sorgulamak için T-SQL'i kullanabilir. Sorgu Deposu varsayılan yapılandırmaları 1 saatlik bir parçalılık kullanır. Yüksek CPU tüketen sorgular için aktiviteye bakmak için aşağıdaki sorguyu kullanın. Bu sorgu, en iyi 15 CPU tüketen sorguları döndürür. Değiştirmeyi `rsi.start_time >= DATEADD(hour, -2, GETUTCDATE()`unutmayın:

```sql
-- Top 15 CPU consuming queries by query hash
-- note that a query  hash can have many query id if not parameterized or not parameterized properly
-- it grabs a sample query text by min
WITH AggregatedCPU AS (SELECT q.query_hash, SUM(count_executions * avg_cpu_time / 1000.0) AS total_cpu_millisec, SUM(count_executions * avg_cpu_time / 1000.0)/ SUM(count_executions) AS avg_cpu_millisec, MAX(rs.max_cpu_time / 1000.00) AS max_cpu_millisec, MAX(max_logical_io_reads) max_logical_reads, COUNT(DISTINCT p.plan_id) AS number_of_distinct_plans, COUNT(DISTINCT p.query_id) AS number_of_distinct_query_ids, SUM(CASE WHEN rs.execution_type_desc='Aborted' THEN count_executions ELSE 0 END) AS Aborted_Execution_Count, SUM(CASE WHEN rs.execution_type_desc='Regular' THEN count_executions ELSE 0 END) AS Regular_Execution_Count, SUM(CASE WHEN rs.execution_type_desc='Exception' THEN count_executions ELSE 0 END) AS Exception_Execution_Count, SUM(count_executions) AS total_executions, MIN(qt.query_sql_text) AS sampled_query_text
                       FROM sys.query_store_query_text AS qt
                            JOIN sys.query_store_query AS q ON qt.query_text_id=q.query_text_id
                            JOIN sys.query_store_plan AS p ON q.query_id=p.query_id
                            JOIN sys.query_store_runtime_stats AS rs ON rs.plan_id=p.plan_id
                            JOIN sys.query_store_runtime_stats_interval AS rsi ON rsi.runtime_stats_interval_id=rs.runtime_stats_interval_id
                       WHERE rs.execution_type_desc IN ('Regular', 'Aborted', 'Exception')AND rsi.start_time>=DATEADD(HOUR, -2, GETUTCDATE())
                       GROUP BY q.query_hash), OrderedCPU AS (SELECT query_hash, total_cpu_millisec, avg_cpu_millisec, max_cpu_millisec, max_logical_reads, number_of_distinct_plans, number_of_distinct_query_ids, total_executions, Aborted_Execution_Count, Regular_Execution_Count, Exception_Execution_Count, sampled_query_text, ROW_NUMBER() OVER (ORDER BY total_cpu_millisec DESC, query_hash ASC) AS RN
                                                              FROM AggregatedCPU)
SELECT OD.query_hash, OD.total_cpu_millisec, OD.avg_cpu_millisec, OD.max_cpu_millisec, OD.max_logical_reads, OD.number_of_distinct_plans, OD.number_of_distinct_query_ids, OD.total_executions, OD.Aborted_Execution_Count, OD.Regular_Execution_Count, OD.Exception_Execution_Count, OD.sampled_query_text, OD.RN
FROM OrderedCPU AS OD
WHERE OD.RN<=15
ORDER BY total_cpu_millisec DESC;
```

Sorunlu sorguları tanımladıktan sonra, CPU kullanımını azaltmak için bu sorguları ayarlamanın zamanı gelmiştir.  Sorguları ayarlamak için zamanınız yoksa, sorunu çözmek için veritabanının SLO'yu yükseltmeyi de seçebilirsiniz.

## <a name="identify-io-performance-issues"></a>IO performans sorunlarını belirleme

IO performans sorunları tanımlanırken, IO sorunlarıile ilişkili en iyi bekleme türleri şunlardır:

- `PAGEIOLATCH_*`

  Veri dosyası IO sorunları `PAGEIOLATCH_SH`için `PAGEIOLATCH_EX` `PAGEIOLATCH_UP`(, , , dahil).  Bekleme türü adının içinde **IO** varsa, bir IO sorununa işaret emz. Sayfa mandal bekleme adında **IO** yoksa, farklı bir sorun türüne işaret eder (örneğin, tempdb çekişmesi).

- `WRITE_LOG`

  İşlem günlüğü IO sorunları için.

### <a name="if-the-io-issue-is-occurring-right-now"></a>IO sorunu şu anda meydana geliyorsa

görmek için [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) veya [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) `wait_type` `wait_time`kullanın.

#### <a name="identify-data-and-log-io-usage"></a>Verileri belirleme ve IO kullanımını kaydetme

Verileri tanımlamak ve IO kullanımını günlüğe kaydetmek için aşağıdaki sorguyu kullanın. Veri veya günlük IO%80'in üzerindeyse, bu, kullanıcıların SQL DB hizmet katmanı için kullanılabilir IO'yu kullandığı anlamına gelir.

```sql
SELECT end_time, avg_data_io_percent, avg_log_write_percent
FROM sys.dm_db_resource_stats
ORDER BY end_time DESC;
```

IO sınırına ulaşıldıysa, iki seçeneğiniz vardır:

- Seçenek 1: İşlem boyutunu veya hizmet katmanını yükseltme
- Seçenek 2: En çok IO tüketen sorguları tanımlayın ve ayarlayın.

#### <a name="view-buffer-related-io-using-the-query-store"></a>Sorgu Deposu'nu kullanarak arabellekle ilgili IO'yu görüntüleme

Seçenek 2 için, izlenen etkinliğin son iki saatini görüntülemek için arabellekle ilgili IO için Sorgu Deposu'na karşı aşağıdaki sorguyu kullanabilirsiniz:

```sql
-- top queries that waited on buffer
-- note these are finished queries
WITH Aggregated AS (SELECT q.query_hash, SUM(total_query_wait_time_ms) total_wait_time_ms, SUM(total_query_wait_time_ms / avg_query_wait_time_ms) AS total_executions, MIN(qt.query_sql_text) AS sampled_query_text, MIN(wait_category_desc) AS wait_category_desc
                    FROM sys.query_store_query_text AS qt
                         JOIN sys.query_store_query AS q ON qt.query_text_id=q.query_text_id
                         JOIN sys.query_store_plan AS p ON q.query_id=p.query_id
                         JOIN sys.query_store_wait_stats AS waits ON waits.plan_id=p.plan_id
                         JOIN sys.query_store_runtime_stats_interval AS rsi ON rsi.runtime_stats_interval_id=waits.runtime_stats_interval_id
                    WHERE wait_category_desc='Buffer IO' AND rsi.start_time>=DATEADD(HOUR, -2, GETUTCDATE())
                    GROUP BY q.query_hash), Ordered AS (SELECT query_hash, total_executions, total_wait_time_ms, sampled_query_text, wait_category_desc, ROW_NUMBER() OVER (ORDER BY total_wait_time_ms DESC, query_hash ASC) AS RN
                                                        FROM Aggregated)
SELECT OD.query_hash, OD.total_executions, OD.total_wait_time_ms, OD.sampled_query_text, OD.wait_category_desc, OD.RN
FROM Ordered AS OD
WHERE OD.RN<=15
ORDER BY total_wait_time_ms DESC;
GO
```

#### <a name="view-total-log-io-for-writelog-waits"></a>WRITELOG bekler için toplam günlük IO görüntüle

Bekleme türü ise, `WRITELOG`toplam günlük IO'yu ifadeye göre görüntülemek için aşağıdaki sorguyu kullanın:

```sql
-- Top transaction log consumers
-- Adjust the time window by changing
-- rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
WITH AggregatedLogUsed
AS (SELECT q.query_hash,
           SUM(count_executions * avg_cpu_time / 1000.0) AS total_cpu_millisec,
           SUM(count_executions * avg_cpu_time / 1000.0) / SUM(count_executions) AS avg_cpu_millisec,
           SUM(count_executions * avg_log_bytes_used) AS total_log_bytes_used,
           MAX(rs.max_cpu_time / 1000.00) AS max_cpu_millisec,
           MAX(max_logical_io_reads) max_logical_reads,
           COUNT(DISTINCT p.plan_id) AS number_of_distinct_plans,
           COUNT(DISTINCT p.query_id) AS number_of_distinct_query_ids,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Aborted' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Aborted_Execution_Count,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Regular' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Regular_Execution_Count,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Exception' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Exception_Execution_Count,
           SUM(count_executions) AS total_executions,
           MIN(qt.query_sql_text) AS sampled_query_text
    FROM sys.query_store_query_text AS qt
        JOIN sys.query_store_query AS q
            ON qt.query_text_id = q.query_text_id
        JOIN sys.query_store_plan AS p
            ON q.query_id = p.query_id
        JOIN sys.query_store_runtime_stats AS rs
            ON rs.plan_id = p.plan_id
        JOIN sys.query_store_runtime_stats_interval AS rsi
            ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
    WHERE rs.execution_type_desc IN ( 'Regular', 'Aborted', 'Exception' )
          AND rsi.start_time >= DATEADD(HOUR, -2, GETUTCDATE())
    GROUP BY q.query_hash),
     OrderedLogUsed
AS (SELECT query_hash,
           total_log_bytes_used,
           number_of_distinct_plans,
           number_of_distinct_query_ids,
           total_executions,
           Aborted_Execution_Count,
           Regular_Execution_Count,
           Exception_Execution_Count,
           sampled_query_text,
           ROW_NUMBER() OVER (ORDER BY total_log_bytes_used DESC, query_hash ASC) AS RN
    FROM AggregatedLogUsed)
SELECT OD.total_log_bytes_used,
       OD.number_of_distinct_plans,
       OD.number_of_distinct_query_ids,
       OD.total_executions,
       OD.Aborted_Execution_Count,
       OD.Regular_Execution_Count,
       OD.Exception_Execution_Count,
       OD.sampled_query_text,
       OD.RN
FROM OrderedLogUsed AS OD
WHERE OD.RN <= 15
ORDER BY total_log_bytes_used DESC;
GO
```

## <a name="identify-tempdb-performance-issues"></a>Performans `tempdb` sorunlarını belirleme

IO performans sorunları tanımlanırken, sorunlarla `tempdb` ilişkili `PAGELATCH_*` en `PAGEIOLATCH_*`iyi bekleme türleri (değil). Ancak, `PAGELATCH_*` bekler her zaman çekişme `tempdb` var anlamına gelmez.  Bu bekleme, aynı veri sayfasını hedefleyen eşzamanlı istekler nedeniyle kullanıcı nesnesi veri sayfası çekişmesi olduğu anlamına da gelebilir. Çekişmeyi `tempdb` daha fazla doğrulamak için, wait_resource değerinin veritabanı `tempdb` kimliği, `x` dosya kimliği ve `y` sayfa kimliği ile başladığını `2:x:y` doğrulamak için [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) kullanın.  

Tempdb çekişmesi için, yaygın bir yöntem, temeli atlayan uygulama kodunu azaltmak veya yeniden `tempdb`yazmaktır.  Yaygın `tempdb` kullanım alanları şunlardır:

- Geçici tablolar
- Tablo değişkenleri
- Tablo değeri olan parametreler
- Sürüm deposu kullanımı (özellikle uzun süren işlemlerle ilişkilidir)
- Sıralama, karma birleştirme ve makara kullanan sorgu planları olan sorgular

### <a name="top-queries-that-use-table-variables-and-temporary-tables"></a>Tablo değişkenlerini ve geçici tabloları kullanan en iyi sorgular

Tablo değişkenlerini ve geçici tabloları kullanan en iyi sorguları tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT plan_handle, execution_count, query_plan
INTO #tmpPlan
FROM sys.dm_exec_query_stats
     CROSS APPLY sys.dm_exec_query_plan(plan_handle);
GO

WITH XMLNAMESPACES('http://schemas.microsoft.com/sqlserver/2004/07/showplan' AS sp)
SELECT plan_handle, stmt.stmt_details.value('@Database', 'varchar(max)') 'Database', stmt.stmt_details.value('@Schema', 'varchar(max)') 'Schema', stmt.stmt_details.value('@Table', 'varchar(max)') 'table'
INTO #tmp2
FROM(SELECT CAST(query_plan AS XML) sqlplan, plan_handle FROM #tmpPlan) AS p
    CROSS APPLY sqlplan.nodes('//sp:Object') AS stmt(stmt_details);
GO

SELECT t.plan_handle, [Database], [Schema], [table], execution_count
FROM(SELECT DISTINCT plan_handle, [Database], [Schema], [table]
     FROM #tmp2
     WHERE [table] LIKE '%@%' OR [table] LIKE '%#%') AS t
    JOIN #tmpPlan AS t2 ON t.plan_handle=t2.plan_handle;
```

### <a name="identify-long-running-transactions"></a>Uzun süren hareketleri tanımlama

Uzun süren hareketleri tanımlamak için aşağıdaki sorguyu kullanın. Uzun süren işlemler sürüm deposu nun temizlenmesini engeller.

```sql
SELECT DB_NAME(dtr.database_id) 'database_name',
       sess.session_id,
       atr.name AS 'tran_name',
       atr.transaction_id,
       transaction_type,
       transaction_begin_time,
       database_transaction_begin_time transaction_state,
       is_user_transaction,
       sess.open_transaction_count,
       LTRIM(RTRIM(REPLACE(
                              REPLACE(
                                         SUBSTRING(
                                                      SUBSTRING(
                                                                   txt.text,
                                                                   (req.statement_start_offset / 2) + 1,
                                                                   ((CASE req.statement_end_offset
                                                                         WHEN -1 THEN
                                                                             DATALENGTH(txt.text)
                                                                         ELSE
                                                                             req.statement_end_offset
                                                                     END - req.statement_start_offset
                                                                    ) / 2
                                                                   ) + 1
                                                               ),
                                                      1,
                                                      1000
                                                  ),
                                         CHAR(10),
                                         ' '
                                     ),
                              CHAR(13),
                              ' '
                          )
                  )
            ) Running_stmt_text,
       recenttxt.text 'MostRecentSQLText'
FROM sys.dm_tran_active_transactions AS atr
    INNER JOIN sys.dm_tran_database_transactions AS dtr
        ON dtr.transaction_id = atr.transaction_id
    LEFT JOIN sys.dm_tran_session_transactions AS sess
        ON sess.transaction_id = atr.transaction_id
    LEFT JOIN sys.dm_exec_requests AS req
        ON req.session_id = sess.session_id
           AND req.transaction_id = sess.transaction_id
    LEFT JOIN sys.dm_exec_connections AS conn
        ON sess.session_id = conn.session_id
    OUTER APPLY sys.dm_exec_sql_text(req.sql_handle) AS txt
    OUTER APPLY sys.dm_exec_sql_text(conn.most_recent_sql_handle) AS recenttxt
WHERE atr.transaction_type != 2
      AND sess.session_id != @@spid
ORDER BY start_time ASC;
```

## <a name="identify-memory-grant-wait-performance-issues"></a>Bellek hibe bekleme performansı sorunlarını belirleme

En iyi bekleme `RESOURCE_SEMAHPORE` türünüz yüksek cpu kullanım sorununa sahip değilseniz, bellek hibe bekleme sorununuz olabilir.

### <a name="determine-if-a-resource_semahpore-wait-is-a-top-wait"></a>Beklemenin `RESOURCE_SEMAHPORE` en iyi bekleyiş olup olmadığını belirleme

`RESOURCE_SEMAHPORE` Beklemenin en iyi bekleyiş olup olmadığını belirlemek için aşağıdaki sorguyu kullanın

```sql
SELECT wait_type,
       SUM(wait_time) AS total_wait_time_ms
FROM sys.dm_exec_requests AS req
    JOIN sys.dm_exec_sessions AS sess
        ON req.session_id = sess.session_id
WHERE is_user_process = 1
GROUP BY wait_type
ORDER BY SUM(wait_time) DESC;
```

### <a name="identity-high-memory-consuming-statements"></a>Kimlik yüksek bellek tüketen ifadeler

Yüksek bellek tüketen ifadeleri tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT IDENTITY(INT, 1, 1) rowId,
    CAST(query_plan AS XML) query_plan,
    p.query_id
INTO #tmp
FROM sys.query_store_plan AS p
    JOIN sys.query_store_runtime_stats AS r
        ON p.plan_id = r.plan_id
    JOIN sys.query_store_runtime_stats_interval AS i
        ON r.runtime_stats_interval_id = i.runtime_stats_interval_id
WHERE start_time > '2018-10-11 14:00:00.0000000'
      AND end_time < '2018-10-17 20:00:00.0000000';
GO
;WITH cte
AS (SELECT query_id,
        query_plan,
        m.c.value('@SerialDesiredMemory', 'INT') AS SerialDesiredMemory
    FROM #tmp AS t
        CROSS APPLY t.query_plan.nodes('//*:MemoryGrantInfo[@SerialDesiredMemory[. > 0]]') AS m(c) )
SELECT TOP 50
    cte.query_id,
    t.query_sql_text,
    cte.query_plan,
    CAST(SerialDesiredMemory / 1024. AS DECIMAL(10, 2)) SerialDesiredMemory_MB
FROM cte
    JOIN sys.query_store_query AS q
        ON cte.query_id = q.query_id
    JOIN sys.query_store_query_text AS t
        ON q.query_text_id = t.query_text_id
ORDER BY SerialDesiredMemory DESC;
```

### <a name="identify-the-top-10-active-memory-grants"></a>En iyi 10 etkin bellek bağışını belirleme

En iyi 10 etkin bellek hibesini tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT TOP 10
    CONVERT(VARCHAR(30), GETDATE(), 121) AS runtime,
       r.session_id,
       r.blocking_session_id,
       r.cpu_time,
       r.total_elapsed_time,
       r.reads,
       r.writes,
       r.logical_reads,
       r.row_count,
       wait_time,
       wait_type,
       r.command,
       OBJECT_NAME(txt.objectid, txt.dbid) 'Object_Name',
       LTRIM(RTRIM(REPLACE(
                              REPLACE(
                                         SUBSTRING(
                                                      SUBSTRING(
                                                                   text,
                                                                   (r.statement_start_offset / 2) + 1,
                                                                   ((CASE r.statement_end_offset
                                                                         WHEN -1 THEN
                                                                             DATALENGTH(text)
                                                                         ELSE
                                                                             r.statement_end_offset
                                                                     END - r.statement_start_offset
                                                                    ) / 2
                                                                   ) + 1
                                                               ),
                                                      1,
                                                      1000
                                                  ),
                                         CHAR(10),
                                         ' '
                                     ),
                              CHAR(13),
                              ' '
                          )
                  )
            ) stmt_text,
       mg.dop,                                               --Degree of parallelism
       mg.request_time,                                      --Date and time when this query requested the memory grant.
       mg.grant_time,                                        --NULL means memory has not been granted
       mg.requested_memory_kb / 1024.0 requested_memory_mb,  --Total requested amount of memory in megabytes
       mg.granted_memory_kb / 1024.0 AS granted_memory_mb,   --Total amount of memory actually granted in megabytes. NULL if not granted
       mg.required_memory_kb / 1024.0 AS required_memory_mb, --Minimum memory required to run this query in megabytes.
       max_used_memory_kb / 1024.0 AS max_used_memory_mb,
       mg.query_cost,                                        --Estimated query cost.
       mg.timeout_sec,                                       --Time-out in seconds before this query gives up the memory grant request.
       mg.resource_semaphore_id,                             --Non-unique ID of the resource semaphore on which this query is waiting.
       mg.wait_time_ms,                                      --Wait time in milliseconds. NULL if the memory is already granted.
       CASE mg.is_next_candidate --Is this process the next candidate for a memory grant
           WHEN 1 THEN
               'Yes'
           WHEN 0 THEN
               'No'
           ELSE
               'Memory has been granted'
       END AS 'Next Candidate for Memory Grant',
       qp.query_plan
FROM sys.dm_exec_requests AS r
    JOIN sys.dm_exec_query_memory_grants AS mg
        ON r.session_id = mg.session_id
           AND r.request_id = mg.request_id
    CROSS APPLY sys.dm_exec_sql_text(mg.sql_handle) AS txt
    CROSS APPLY sys.dm_exec_query_plan(r.plan_handle) AS qp
ORDER BY mg.granted_memory_kb DESC;
```

## <a name="calculating-database-and-objects-sizes"></a>Veritabanı ve nesne boyutlarını hesaplama

Aşağıdaki sorgu veritabanınızın boyutunu döndürür (megabaytolarak):

```sql
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

Aşağıdaki sorgu veritabanınızda tek tek nesnelerin boyutunu (megabayt olarak) döndürür:

```sql
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Bağlantıları izleme

Belirli bir Azure SQL Veritabanı sunucusuna kurulan bağlantılar ve her bağlantının ayrıntıları hakkında bilgi almak için [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) görünümünü kullanabilirsiniz. Buna ek olarak, [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) görünümü tüm etkin kullanıcı bağlantıları ve dahili görevler hakkında bilgi alırken yararlıdır.
Aşağıdaki sorgu geçerli bağlantıdaki bilgileri alır:

```sql
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> **sys.dm_exec_requests** ve **sys.dm_exec_sessions görünümlerini**yürüterken, veritabanında **VIEW DATABASE STATE** izniniz varsa, veritabanındaki tüm yürütme oturumlarını görürsünüz; aksi takdirde, yalnızca geçerli oturumu görürsünüz.

## <a name="monitor-resource-use"></a>Kaynak kullanımını izleme

SQL Database Query [Performance Insight](sql-database-query-performance.md) ve [Query Store'u](https://msdn.microsoft.com/library/dn817826.aspx)kullanarak kaynak kullanımını izleyebilirsiniz.

Ayrıca, şu iki görünümü kullanarak kullanımı izleyebilirsiniz:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdm_db_resource_stats"></a>sys.dm_db_resource_stats

Her SQL veritabanında [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) görünümünü kullanabilirsiniz. **sys.dm_db_resource_stats** görünümü, hizmet katmanına göre son kaynak kullanım verilerini gösterir. CPU, veri IO, günlük yazma ve bellek için ortalama yüzdeleri her 15 saniyede kaydedilir ve 1 saat boyunca tutulur.

Bu görünüm kaynak kullanımına daha ayrıntılı bir görünüm sağladığından, geçerli durum çözümlemesi veya sorun giderme için önce **sys.dm_db_resource_stats** kullanın. Örneğin, bu sorgu, son bir saat içinde geçerli veritabanı için ortalama ve maksimum kaynak kullanımını gösterir:

```sql
SELECT  
    AVG(avg_cpu_percent) AS 'Average CPU use in percent',
    MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
    AVG(avg_data_io_percent) AS 'Average data IO in percent',
    MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
    AVG(avg_log_write_percent) AS 'Average log write use in percent',
    MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
    AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
    MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
FROM sys.dm_db_resource_stats;  
```

Diğer sorgular için [sys.dm_db_resource_stats'deki](https://msdn.microsoft.com/library/dn800981.aspx)örneklere bakın.

### <a name="sysresource_stats"></a>sys.resource_stats

**Ana** veritabanındaki [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümü, SQL veritabanınızın performansını belirli hizmet katmanı nda ve bilgi işlem boyutunda izlemenize yardımcı olabilecek ek bilgilere sahiptir. Veriler her 5 dakikada bir toplanır ve yaklaşık 14 gün boyunca muhafaza edilir. Bu görünüm, SQL veritabanınızın kaynakları nasıl kullandığının uzun vadeli bir geçmiş çözümlemesi için yararlıdır.

Aşağıdaki grafik, bir hafta içinde her saat için P2 bilgi işlem boyutuna sahip bir Premium veritabanı için CPU kaynak kullanımını gösterir. Bu grafik Pazartesi günü başlar, 5 iş günü gösterir ve uygulamada çok daha az olduğunda bir hafta sonu gösterir.

![SQL veritabanı kaynak kullanımı](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Verilerden, bu veritabanı şu anda p2 bilgi işlem boyutuna göre (Salı günü öğle nisbeten) sadece yüzde 50 CPU kullanımı nın üzerinde bir pik CPU yükü vardır. CPU uygulamanın kaynak profilinde baskın faktör ise, o zaman P2 iş yükü her zaman uygun olduğunu garanti etmek için doğru bilgi işlem boyutu olduğuna karar verebilirsiniz. Bir uygulamanın zaman içinde büyümesini bekliyorsanız, uygulamanın performans düzeyi sınırına ulaşmaması için ek bir kaynak arabelleği olması iyi bir fikirdir. Bilgi işlem boyutunu artırırsanız, veritabanı, özellikle gecikmeye duyarlı ortamlarda istekleri etkili bir şekilde işlemek için yeterli güce sahip olmadığında oluşabilecek müşteri görünür hataları önlemeye yardımcı olabilirsiniz. Örnek, veritabanı çağrılarının sonuçlarına göre web sayfalarını boyayen bir uygulamayı destekleyen bir veritabanıdır.

Diğer uygulama türleri aynı grafiği farklı yorumlayabilir. Örneğin, bir uygulama bordro verilerini her gün işlemeye çalışırsa ve aynı grafiğe sahipse, bu tür bir "toplu iş" modeli P1 bilgi işlem boyutunda iyi sonuç verebilir. P1 işlem boyutu, P2 işlem boyutunda 200 DSU ile karşılaştırıldığında 100 DTÜ'ye sahiptir. P1 işlem boyutu, P2 işlem boyutunun performansının yarısını sağlar. Yani, P2 CPU kullanımının yüzde 50 P1 yüzde 100 CPU kullanımı eşittir. Uygulamanın zaman acısı yoksa, bir işin tamamlanması2 saat veya 2,5 saat sürse, bugün yapılması önemli olmayabilir. Bu kategorideki bir uygulama büyük olasılıkla Bir P1 bilgi işlem boyutu kullanabilirsiniz. Kaynak kullanımının daha düşük olduğu gün boyunca zaman dilimleri olduğu gerçeğinden yararlanabilirsiniz, böylece herhangi bir "büyük tepe" günün ilerleyen saatlerinde oluklardan birine dökülebilir. P1 bilgi işlem boyutu, işler her gün zamanında bitebildiği sürece bu tür bir uygulama için iyi olabilir (ve paradan tasarruf edebilirsiniz).

Azure SQL Veritabanı, her sunucudaki **ana** veritabanının **sys.resource_stats** görünümünde her etkin veritabanı için tüketilen kaynak bilgilerini ortaya çıkarır. Tablodaki veriler 5 dakikalık aralıklarla toplanır. Temel, Standart ve Premium hizmet katmanlarıyla verilerin tabloda görünmesi 5 dakikadan fazla sürebilir, bu nedenle bu veriler gerçek zamanlı çözüme yakın analizden çok geçmiş analizler için daha kullanışlıdır. Bir veritabanının yakın geçmişini görmek ve seçtiğiniz rezervasyonun gerektiğinde istediğiniz performansı teslim edip etmediğini doğrulamak için **sys.resource_stats** görünümünü sorgulayın.

> [!NOTE]
> Aşağıdaki örneklerde **sys.resource_stats** sorgusuiçin SQL Veritabanı sunucunuzun **ana** veritabanına bağlanmalısınız.

Bu örnek, bu görünümdeki verilerin nasıl açıkta kaldığını gösterir:

```sql
SELECT TOP 10 *
FROM sys.resource_stats
WHERE database_name = 'resource1'
ORDER BY start_time DESC
```

![sys.resource_stats katalog görünümü](./media/sql-database-performance-guidance/sys_resource_stats.png)

Sonraki örnek, SQL veritabanınızın kaynakları nasıl kullandığı hakkında bilgi almak için **sys.resource_stats** kataloğu görünümünü kullanabileceğiniz farklı yolları gösterir:

1. Veritabanı userdb1 için geçen hafta kaynak kullanımına bakmak için bu sorguyu çalıştırabilirsiniz:

    ```sql
    SELECT *
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND
        start_time > DATEADD(day, -7, GETDATE())
    ORDER BY start_time DESC;
    ```

2. İş yükünüzün bilgi işlem boyutuna ne kadar iyi uyduğunu değerlendirmek için kaynak ölçümlerinin her yönünü incelemeniz gerekir: CPU, okuma, yazma, çalışan sayısı ve oturum sayısı. Bu kaynak ölçümlerinin ortalama ve maksimum değerlerini bildirmek için **sys.resource_stats** kullanarak gözden geçirilmiş bir sorgu aşağıda veda edilmiştir:

    ```sql
    SELECT
        avg(avg_cpu_percent) AS 'Average CPU use in percent',
        max(avg_cpu_percent) AS 'Maximum CPU use in percent',
        avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
        max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
        avg(avg_log_write_percent) AS 'Average log write use in percent',
        max(avg_log_write_percent) AS 'Maximum log write use in percent',
        avg(max_session_percent) AS 'Average % of sessions',
        max(max_session_percent) AS 'Maximum % of sessions',
        avg(max_worker_percent) AS 'Average % of workers',
        max(max_worker_percent) AS 'Maximum % of workers'
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

3. Her kaynak ölçümünün ortalama ve maksimum değerleri hakkındaki bu bilgilerle, iş yükünüzün seçtiğiniz bilgi işlem boyutuna ne kadar iyi uyduğunu değerlendirebilirsiniz. Genellikle, **sys.resource_stats** ortalama değerleri hedef boyutuna karşı kullanmak için iyi bir taban çizgisi verir. Bu senin birincil ölçüm çubuğun olmalı. Örneğin, S2 işlem boyutuna sahip Standart hizmet katmanını kullanıyor olabilirsiniz. CPU ve IO okuma ve yazma için ortalama kullanım yüzdeleri yüzde 40'ın altında, ortalama işçi sayısı 50'nin altında ve ortalama oturum sayısı 200'ün altındadır. İş yükünüz S1 işlem boyutuna sığabilir. Veritabanınızın çalışana ve oturum sınırlarına uygun olup olmadığını görmek kolaydır. Bir veritabanının CPU ile ilgili olarak daha düşük bir bilgi işlem boyutuna uygun olup olmadığını görmek için, alt işlem boyutunun DTU numarasını geçerli işlem boyutunuzun DTU sayısına bölün ve sonucu 100 ile çarpın:

    ```S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40```

    Sonuç, iki işlem boyutu arasındaki göreli performans yüzdesi farkıdır. Kaynak kullanımınız bu tutarı aşarsa, iş yükünüz alt bilgi işlem boyutuna sığabilir. Ancak, tüm kaynak kullanım değerlerine bakmanız ve veritabanı iş yükünüzün alt bilgi işlem boyutuna ne sıklıkta uyacağını yüzde olarak belirlemeniz gerekir. Aşağıdaki sorgu, bu örnekte hesapladığımız yüzde 40 eşiğe bağlı olarak kaynak boyutu başına sığdırma yüzdesini çıkar:

   ```sql
    SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent',
        (COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent',
        (COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

    Veritabanı hizmet katmanınıza bağlı olarak, iş yükünüzün alt işlem boyutuna uygun olup olmadığına karar verebilirsiniz. Veritabanı iş yükü hedefiniz yüzde 99,9 ise ve önceki sorgu üç kaynak boyutu için yüzde 99,9'dan büyük değerler döndürürse, iş yükünüz büyük olasılıkla alt bilgi işlem boyutuna sığar.

    Uygun luk yüzdesine bakmak, amacınıza ulaşmak için bir sonraki yüksek bilgi işlem boyutuna geçmeniz gerekip gerekmediği konusunda da fikir verir. Örneğin, userdb1 geçen hafta için aşağıdaki CPU kullanımını gösterir:

   | Ortalama CPU yüzdesi | Maksimum CPU yüzdesi |
   | --- | --- |
   | 24.5 |100,00 |

    Ortalama CPU, veritabanının işlem boyutuna iyi uyacak olan işlem boyutu sınırının yaklaşık dörtte biridir. Ancak, en büyük değer veritabanının bilgi işlem boyutu sınırına ulaştığını gösterir. Bir sonraki yüksek işlem boyutuna geçmeniz gerekiyor mu? İş yükünüzün kaç kez yüzde 100'e ulaştığına bakın ve veritabanı iş yükü hedefiniz ile karşılaştırın.

    ```sql
    SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

    Bu sorgu, üç kaynak boyutundan herhangi biri için yüzde 99,9'dan daha az bir değer döndürürse, bir sonraki daha yüksek bilgi işlem boyutuna geçmeyi veya SQL veritabanındaki yükü azaltmak için uygulama ayar tekniklerini kullanmayı düşünün.

4. Bu alıştırma, gelecekte öngörülen iş yükü artışını da dikkate alır.

Elastik havuzlar için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Elastik havuz izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).

### <a name="maximum-concurrent-requests"></a>Maksimum eşzamanlı istekler

Eşzamanlı istek sayısını görmek için bu Transact-SQL sorgusunu SQL veritabanınızda çalıştırın:

```sql
SELECT COUNT(*) AS [Concurrent_Requests]
FROM sys.dm_exec_requests R;
```

Şirket içi bir SQL Server veritabanının iş yükünü çözümlemek için, bu sorguyu çözümlemek istediğiniz belirli veritabanına filtre uygulayacak şekilde değiştirin. Örneğin, MyDatabase adında bir şirket içi veritabanınız varsa, bu Transact-SQL sorgusu bu veritabanındaki eşzamanlı isteklerin sayısını döndürür:

```sql
SELECT COUNT(*) AS [Concurrent_Requests]
FROM sys.dm_exec_requests R
INNER JOIN sys.databases D ON D.database_id = R.database_id
AND D.name = 'MyDatabase';
```

Bu sadece zamanın tek bir noktasındaki bir anlık görüntü. İş yükünüzü ve eşzamanlı istek gereksinimlerinizi daha iyi anlamak için zaman içinde birçok örnek toplamanız gerekir.

### <a name="maximum-concurrent-logins"></a>Maksimum eşzamanlı oturum açma

Oturum açma sıklığı hakkında bir fikir almak için kullanıcı ve uygulama kalıplarınızı analiz edebilirsiniz. Ayrıca, bu makalede tartıştığımız bu veya diğer sınırlara isabet etmediğinizden emin olmak için gerçek dünya yüklerini test ortamında da çalıştırabilirsiniz. Size eşzamanlı giriş sayılarını veya geçmişini gösterebilecek tek bir sorgu veya dinamik yönetim görünümü (DMV) yoktur.

Birden çok istemci aynı bağlantı dizesini kullanıyorsa, hizmet her oturum açmanın kimliğini doğrular. 10 kullanıcı aynı kullanıcı adı ve parolayı kullanarak aynı anda bir veritabanına bağlanırsa, 10 eşzamanlı giriş olur. Bu sınır yalnızca oturum açma ve kimlik doğrulama süresi için geçerlidir. Aynı 10 kullanıcı veritabanına sırayla bağlanırsa, eşzamanlı giriş sayısı hiçbir zaman 1'den büyük olmaz.

> [!NOTE]
> Şu anda, bu sınır elastik havuzlarda veritabanları için geçerli değildir.

### <a name="maximum-sessions"></a>Maksimum oturumlar

Geçerli etkin oturumların sayısını görmek için bu Transact-SQL sorgusunu SQL veritabanınızda çalıştırın:

```sql
SELECT COUNT(*) AS [Sessions]
FROM sys.dm_exec_connections
```

Şirket içi bir SQL Server iş yükünü çözümlüyorsanız, sorguyu belirli bir veritabanına odaklanmak için değiştirin. Bu sorgu, veritabanını Azure SQL Veritabanı'na taşımayı düşünüyorsanız, veritabanı için olası oturum gereksinimlerini belirlemenize yardımcı olur.

```sql
SELECT COUNT(*) AS [Sessions]
FROM sys.dm_exec_connections C
INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
INNER JOIN sys.databases D ON (D.database_id = S.database_id)
WHERE D.name = 'MyDatabase'
```

Yine, bu sorgular bir nokta-in-time sayısı döndürün. Zaman içinde birden çok örnek toplarsanız, oturum kullanımınızı en iyi şekilde anlamış olursunuz.

SQL Veritabanı çözümlemesi için, [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümünü sorgulayarak ve **active_session_count** sütununa inceleyerek oturumlarla ilgili geçmiş istatistikleri alabilirsiniz.

## <a name="monitoring-query-performance"></a>Sorgu performansını izleme

Yavaş veya uzun süren sorgular önemli sistem kaynaklarını tüketebilir. Bu bölümde, birkaç yaygın sorgu performans sorunları algılamak için dinamik yönetim görünümleri nasıl kullanılacağını gösterir. Microsoft TechNet'teki [SQL Server 2008 makalesinde](https://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) sorun giderme Performansı Sorunları, sorun giderme için daha eski ama yine de yararlı bir başvurudur.

### <a name="finding-top-n-queries"></a>En iyi N sorgularını bulma

Aşağıdaki örnek, ortalama CPU süresine göre sıralanan ilk beş sorgu hakkında bilgi verir. Bu örnek, sorguları sorgu karmalarına göre toplar, böylece mantıksal olarak eşdeğer sorgular kümülatif kaynak tüketimine göre gruplandırılır.

```sql
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
     MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
        SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
FROM sys.dm_exec_query_stats AS QS
CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Engellenen sorguları izleme

Yavaş veya uzun süren sorgular aşırı kaynak tüketimine katkıda bulunabilir ve engellenen sorguların sonucu olabilir. Engellemenin nedeni kötü uygulama tasarımı, kötü sorgu planları, yararlı dizinlerin eksikliği ve benzeri olabilir. Azure SQL Veritabanınızdaki geçerli kilitleme etkinliği hakkında bilgi almak için sys.dm_tran_locks görünümünü kullanabilirsiniz. Örneğin kod, SQL Server Books Online'daki [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) bakın.

### <a name="monitoring-query-plans"></a>Sorgu planlarını izleme

Verimsiz bir sorgu planı da CPU tüketimini artırabilir. Aşağıdaki örnek, hangi sorgunun en kümülatif CPU kullandığını belirlemek için [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) görünümünü kullanır.

```sql
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Ayrıca bkz.

[SQL Database'e Giriş](sql-database-technical-overview.md)
