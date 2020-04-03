---
title: İş yükünüzü çözümleme
description: Azure Synapse Analytics'teki iş yükünüz için sorgu önceliklendirmelerini çözümleme teknikleri.
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
ms.openlocfilehash: d10a642f9309e4bb93368564488fc75be15fa27c
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586072"
---
# <a name="analyze-your-workload-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te iş yükünüzü analiz edin

Azure Synapse Analytics'te Synapse SQL iş yükünüzü analiz etme teknikleri.

## <a name="resource-classes"></a>Kaynak Sınıfları

Synapse SQL, sorgulara sistem kaynakları atamak için kaynak sınıfları sağlar.  Kaynak sınıfları hakkında daha fazla bilgi için kaynak [sınıfları & iş yükü yönetimine](resource-classes-for-workload-management.md)bakın.  Sorguya atanan kaynak sınıfışu kullanılabilir olandan daha fazla kaynağa ihtiyaç duyarsa sorgular bekleyecektir.

## <a name="queued-query-detection-and-other-dmvs"></a>Sıralı sorgu algılama ve diğer DMV'ler

Eşzamanlılık kuyruğunda bekleyen sorguları tanımlamak için `sys.dm_pdw_exec_requests` DMV'yi kullanabilirsiniz. Eşzamanlılık yuvası bekleyen sorguların **askıya alınma**durumu vardır.

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

İş yükü yönetimi rolleri `sys.database_principals`ile görüntülenebilir.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

Aşağıdaki sorgu, her kullanıcının hangi role atandığını gösterir.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

Synapse SQL aşağıdaki bekleme türlerine sahiptir:

* **LocalQueriesConcurrencyResourceType**: Eşzamanlılık yuvası çerçevesinin dışında yer alan sorgular. DMV sorguları ve sistem `SELECT @@VERSION` işlevleri gibi yerel sorguörnekleridir.
* **UserConcurrencyResourceType**: Eşzamanlılık yuvası çerçevesi içinde yer alan sorgular. Son kullanıcı tablolarına yönelik sorgular, bu kaynak türünü kullanacak örnekleri temsil eder.
* **DmsConcurrencyResourceType**: Veri hareketi işlemlerinden kaynaklanan bekler.
* **BackupConcurrencyResourceType**: Bu bekleme, bir veritabanının yedeklendiğini gösterir. Bu kaynak türü için en büyük değer 1'dir. Aynı anda birden çok yedekleme isteniyorsa, diğerleri sıraya alınır. Genel olarak, 10 dakikalık ardışık anlık görüntüler arasında en az bir süre öneririz. 

DMV, `sys.dm_pdw_waits` bir isteğin beklediği kaynakları görmek için kullanılabilir.

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

DMV, `sys.dm_pdw_resource_waits` belirli bir sorguiçin bekleme bilgilerini gösterir. Kaynak bekleme süresi, kaynakların sağlanmasını bekleyen zamanı ölçer. Sinyal bekleme süresi, altta yatan SQL sunucuların sorguyu CPU'ya zamanlaması için gereken süredir.

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

`sys.dm_pdw_resource_waits` Ayrıca, dmv kaç eşzamanlılık yuvası verildiğini hesaplamak için de kullanabilirsiniz.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots
FROM    sys.[dm_pdw_resource_waits]
WHERE   [state]           = 'Granted'
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

`sys.dm_pdw_wait_stats` DMV beklemelerin tarihsel eğilim analizi için kullanılabilir.

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

Veritabanı kullanıcılarını ve güvenliğini yönetme hakkında daha fazla bilgi için [Synapse SQL'de güvenli veritabanı](sql-data-warehouse-overview-manage-security.md)na bakın. Daha büyük kaynak sınıflarının kümelenmiş sütun deposu dizini kalitesini nasıl iyileştirebileceği hakkında daha fazla bilgi [için](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)bkz.
