---
title: İş yükünü izleme-Azure portal
description: Azure portal kullanarak SYNAPSE SQL 'i izleme
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 327174974affb3b2511eac60755aa1bf047b3b5e
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133458"
---
# <a name="monitor-workload---azure-portal"></a>İş yükünü izleme-Azure portal

Bu makalede, iş yükünüzü izlemek için Azure portal nasıl kullanılacağı açıklanır. Bu, [SYNAPSE SQL](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/)için Log Analytics kullanarak sorgu yürütmeyi ve iş yükü eğilimlerini araştırmak üzere Azure izleyici günlüklerini ayarlamayı içerir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- SQL havuzu: bir SQL havuzu için günlükleri toplayacağız. Sağlanmış bir SQL havuzu yoksa, [SQL havuzu oluşturma](load-data-from-azure-blob-storage-using-polybase.md)' daki yönergelere bakın.

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Log Analytics çalışma alanları için gezinme dikey penceresine gidin ve bir çalışma alanı oluşturun

![Log Analytics çalışma alanları](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspaces.png)

![Analytics çalışma alanı Ekle](./media/sql-data-warehouse-monitor-workload-portal/add_analytics_workspace.png)

![Analytics çalışma alanı Ekle](./media/sql-data-warehouse-monitor-workload-portal/add_analytics_workspace_2.png)

Çalışma alanları hakkında daha fazla bilgi için aşağıdaki [belgeleri](../../azure-monitor/learn/quick-create-workspace.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.jsond#create-a-workspace)ziyaret edin.

## <a name="turn-on-resource-logs"></a>Kaynak günlüklerini aç

SQL havuzunuzdaki günlükleri göstermek için tanılama ayarlarını yapılandırın. Günlükler, en sık kullanılan performans sorunlarını giderme DMVs ile eşdeğer telemetri görünümlerinden oluşur. Şu anda Şu görünümler desteklenir:

- [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_dms_workers](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_waits](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [sys.dm_pdw_sql_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

![Kaynak günlüklerini etkinleştirme](./media/sql-data-warehouse-monitor-workload-portal/enable_diagnostic_logs.png)

Günlükler Azure depolama, Stream Analytics veya Log Analytics dağıtılabilir. Bu öğretici için Log Analytics ' yi seçin.

![Günlükleri belirtin](./media/sql-data-warehouse-monitor-workload-portal/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Sorguları Log Analytics karşı Çalıştır

Log Analytics çalışma alanınıza giderek şunları yapabilirsiniz:

- Günlük sorgularını kullanarak günlükleri çözümleyin ve sorguları yeniden kullanım için kaydedin
- Sorguları yeniden kullanmak üzere Kaydet
- Günlük uyarıları oluşturma
- Sorgu sonuçlarını panoya sabitleme

Günlük sorgularının özellikleri hakkında ayrıntılı bilgi için aşağıdaki [belgeleri](../../azure-monitor/log-query/query-language.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)ziyaret edin.

![Log Analytics çalışma alanı Düzenleyicisi](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspace_editor.png)

![Log Analytics çalışma alanı sorguları](./media/sql-data-warehouse-monitor-workload-portal/log_analytics_workspace_queries.png)

## <a name="sample-log-queries"></a>Örnek günlük sorguları

```Kusto
//List all queries
AzureDiagnostics
| where Category contains "ExecRequests"
| project TimeGenerated, StartTime_t, EndTime_t, Status_s, Command_s, ResourceClass_s, duration=datetime_diff('millisecond',EndTime_t, StartTime_t)
```

```Kusto
//Chart the most active resource classes
AzureDiagnostics
| where Category contains "ExecRequests"
| where Status_s == "Completed"
| summarize totalQueries = dcount(RequestId_s) by ResourceClass_s
| render barchart
```

```Kusto
//Count of all queued queries
AzureDiagnostics
| where Category contains "waits"
| where Type_s == "UserConcurrencyResourceType"
| summarize totalQueuedQueries = dcount(RequestId_s)
```

## <a name="next-steps"></a>Sonraki adımlar

Azure izleyici günlüklerini ayarlayıp yapılandırdığınıza göre, [Azure panoları](../../azure-portal/azure-portal-dashboards.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) takımınızın genelinde paylaşılacak şekilde özelleştirin.
