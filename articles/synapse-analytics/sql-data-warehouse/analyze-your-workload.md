---
title: İş yükünüzü çözümleme
description: Azure SYNAPSE Analytics 'teki iş yükünüz için sorgu önceliklendirmesini çözümleme teknikleri.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 6a38fe65b4aedf4f594531f5e9cd8cf9b5dfaac7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80631255"
---
# <a name="analyze-your-workload-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te iş yükünüzü çözümleyin

Azure SYNAPSE Analytics 'te SYNAPSE SQL iş yükünüzü çözümleme teknikleri.

## <a name="resource-classes"></a>Kaynak Sınıfları

SYNAPSE SQL, sorgulara sistem kaynakları atamak için kaynak sınıfları sağlar.  Kaynak sınıfları hakkında daha fazla bilgi için bkz. [kaynak sınıfları & iş yükü yönetimi](resource-classes-for-workload-management.md).  Sorguya atanan kaynak sınıfının şu anda kullanılabilir olandan daha fazla kaynak ihtiyacı varsa sorgular bekleyecektir.

## <a name="queued-query-detection-and-other-dmvs"></a>Sıraya alınan sorgu algılama ve diğer DMVs 'ler

Bir eşzamanlılık kuyruğunda bekleyen `sys.dm_pdw_exec_requests` sorguları tanımlamak için DMV ' y i kullanabilirsiniz. Eşzamanlılık yuvası için bekleyen sorguların durumu **askıya alındı**.

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

İş yükü yönetimi rolleri ile `sys.database_principals`görüntülenebilir.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

Aşağıdaki sorgu, her bir kullanıcının hangi rolü atandığını gösterir.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SYNAPSE SQL aşağıdaki bekleme türlerini içerir:

* **LocalQueriesConcurrencyResourceType**: eşzamanlılık yuva çerçevesinin dışında oturlan sorgular. DMV sorguları ve gibi sistem işlevleri `SELECT @@VERSION` , yerel sorguların örnekleridir.
* **UserConcurrencyResourceType**: eşzamanlılık yuva çerçevesinin içinde yer alan sorgular. Son Kullanıcı tablolarına yönelik sorgular, bu kaynak türünü kullanacak örnekleri temsil eder.
* **DmsConcurrencyResourceType**: bekleyen veri taşıma işlemleri.
* **BackupConcurrencyResourceType**: Bu bekleme bir veritabanının yedeklenmekte olduğunu gösterir. Bu kaynak türü için en büyük değer 1 ' dir. Aynı anda birden çok yedekleme isteniyorsa, diğerleri kuyruk. Genel olarak, 10 dakikalık ardışık anlık görüntüler arasında en az bir süre öneriyoruz.

Bir `sys.dm_pdw_waits` isteğin beklediği kaynakları görmek için DMV kullanılabilir.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

DMV, `sys.dm_pdw_resource_waits` belirli bir sorgu için bekleme bilgilerini gösterir. Kaynak bekleme süresi, kaynakların sağlanması için bekleyen süreyi ölçer. Sinyal bekleme süresi, temel alınan SQL sunucularının sorguyu CPU 'ya zamanlaması için gereken süredir.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```

Ayrıca, `sys.dm_pdw_resource_waits` DMV ' de kaç eşzamanlılık yuvası verildiğini hesaplayabilirsiniz.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots
FROM    sys.[dm_pdw_resource_waits]
WHERE   [state]           = 'Granted'
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

DMV, `sys.dm_pdw_wait_stats` bekleyen eğilim analizi için kullanılabilir.

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı kullanıcılarını ve güvenliğini yönetme hakkında daha fazla bilgi için bkz. [SYNAPSE SQL 'de veritabanını güvenli hale getirme](sql-data-warehouse-overview-manage-security.md). Daha büyük kaynak sınıflarının kümelenmiş columnstore dizini kalitesini nasıl iyileştirebilecek hakkında daha fazla bilgi için bkz. [segment kalitesini artırmak için dizinleri yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).
