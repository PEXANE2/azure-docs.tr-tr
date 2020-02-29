---
title: İş yükünü izleme-Azure portal
description: Azure portal kullanarak SQL analizlerini izleme
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 7e93aee405d8a66d850a4e3f07f2e788f1004ef8
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197258"
---
# <a name="monitor-workload---azure-portal"></a>İş yükünü izleme-Azure portal

Bu makalede, iş yükünüzü izlemek için Azure portal nasıl kullanılacağı açıklanır. Bu, [SQL Analytics](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/)için Log Analytics kullanarak sorgu yürütmeyi ve iş yükü eğilimlerini araştırmak üzere Azure izleyici günlüklerini ayarlamayı içerir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- SQL havuzu: bir SQL havuzu için günlükleri toplayacağız. Sağlanmış bir SQL havuzu yoksa, [SQL havuzu oluşturma](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-tutorial)' daki yönergelere bakın.

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Log Analytics çalışma alanları için gezinme dikey penceresine gidin ve bir çalışma alanı oluşturun 

![Log Analytics çalışma alanları](media/sql-data-warehouse-monitor/log_analytics_workspaces.png)

![Analytics çalışma alanı Ekle](media/sql-data-warehouse-monitor/add_analytics_workspace.png)

![Analytics çalışma alanı Ekle](media/sql-data-warehouse-monitor/add_analytics_workspace_2.png)

Çalışma alanları hakkında daha fazla bilgi için aşağıdaki [belgeleri](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace#create-a-workspace)ziyaret edin.

## <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini aç

SQL havuzunuzdaki günlükleri göstermek için tanılama ayarlarını yapılandırın. Günlükler, en sık kullanılan performans sorunlarını giderme DMVs ile eşdeğer telemetri görünümlerinden oluşur. Şu anda Şu görünümler desteklenir:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?view=aps-pdw-2016-au7)


![Tanılama günlüklerini etkinleştirme](media/sql-data-warehouse-monitor/enable_diagnostic_logs.png)

Günlükler Azure depolama, Stream Analytics veya Log Analytics dağıtılabilir. Bu öğretici için Log Analytics ' yi seçin.

![Günlükleri belirtin](media/sql-data-warehouse-monitor/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Sorguları Log Analytics karşı Çalıştır

Log Analytics çalışma alanınıza giderek şunları yapabilirsiniz:

- Günlük sorgularını kullanarak günlükleri çözümleyin ve sorguları yeniden kullanım için kaydedin
- Sorguları yeniden kullanmak üzere Kaydet
- Günlük uyarıları oluşturma
- Sorgu sonuçlarını panoya sabitleme

Günlük sorgularının özellikleri hakkında ayrıntılı bilgi için aşağıdaki [belgeleri](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language)ziyaret edin.

![Log Analytics çalışma alanı Düzenleyicisi](media/sql-data-warehouse-monitor/log_analytics_workspace_editor.png)



![Log Analytics çalışma alanı sorguları](media/sql-data-warehouse-monitor/log_analytics_workspace_queries.png)

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

Azure izleyici günlüklerini ayarlayıp yapılandırdığınıza göre, [Azure panoları](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) takımınızın genelinde paylaşılacak şekilde özelleştirin.
