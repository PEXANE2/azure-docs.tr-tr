---
title: "Öğretici: Azure Veri Gezgini'nde kodsuz izleme verilerini alma"
description: Bu eğitimde, tek bir kod satırı olmadan Azure Veri Gezgini'ne izleme verilerini nasıl yutabileceğinizi ve bu verileri sorgulamayı öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: kerend
ms.service: data-explorer
ms.topic: tutorial
ms.date: 01/29/2020
ms.openlocfilehash: 3a53a660da2257540f23bc6438fc5933e5229c76
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78198057"
---
# <a name="tutorial-ingest-and-query-monitoring-data-in-azure-data-explorer"></a>Öğretici: Azure Veri Gezgini'nde izleme verilerini alma ve sorgulama 

Bu öğretici, tanılama ve etkinlik günlüklerinden kod yazmadan bir Azure Veri Gezgini kümesine nasıl veri alacağınızı öğretir. Bu basit yutma yöntemiyle, veri çözümlemesi için Azure Veri Gezgini'ni sorgulamaya hızla başlayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Veri Gezgini veritabanında tablolar ve alma eşlemeleri oluşturun.
> * Bir güncelleştirme ilkesi kullanarak yutulan verileri biçimlendirin.
> * Bir [olay hub'ı](/azure/event-hubs/event-hubs-about) oluşturun ve Azure Veri Gezgini'ne bağlayın.
> * Azure Monitor [tanılama ölçümlerinden, günlüklerinden ve](/azure/azure-monitor/platform/diagnostic-settings) [etkinlik günlüklerinden](/azure/azure-monitor/platform/activity-logs-overview)verileri bir olay merkezine aktarın.
> * Yutulan verileri Azure Veri Gezgini'ni kullanarak sorgula.

> [!NOTE]
> Tüm kaynakları aynı Azure konumuveya bölgesinde oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Azure Veri Gezgini kümesi ve veritabanı.](create-cluster-database-portal.md) Bu öğreticide, veritabanı adı *TestDatabase'dir.*

## <a name="azure-monitor-data-provider-diagnostic-metrics-and-logs-and-activity-logs"></a>Azure Monitor veri sağlayıcısı: tanılama ölçümleri ve günlükleri ve etkinlik günlükleri

Aşağıdaki Azure Monitor tanı ölçümleri ve günlükleri ve etkinlik günlükleri tarafından sağlanan verileri görüntüleyin ve anlayın. Bu veri şemalarına dayalı bir yutma ardışık hattı oluşturacaksınız. Günlükteki her olayın bir dizi kaydı olduğunu unutmayın. Bu kayıt dizisi daha sonra öğreticide bölünür.

### <a name="examples-of-diagnostic-metrics-and-logs-and-activity-logs"></a>Tanılama ölçümleri ve günlükleri ve etkinlik günlükleri örnekleri

Azure tanı ölçümleri, günlükleri ve etkinlik günlükleri bir Azure hizmeti tarafından yayımlanır ve bu hizmetin çalışması hakkında veri sağlar. 

# <a name="diagnostic-metrics"></a>[Tanısal ölçümler](#tab/diagnostic-metrics)
#### <a name="example"></a>Örnek

Tanısal ölçümler 1 dakikalık bir zaman dilimi ile toplanır. Aşağıda, sorgu süresine ilişkin bir Azure Veri Gezgini metrik olay şemasına bir örnek verilmiştir:

```json
{
    "records": [
    {
        "count": 14,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/<subscriptionID>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/<cluster-name>",
        "time": "2018-12-20T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    },
    {
        "count": 12,
        "total": 0,
        "minimum": 0,
        "maximum": 0,
        "average": 0,
        "resourceId": "/SUBSCRIPTIONS/<subscriptionID>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/<cluster-name>",
        "time": "2018-12-21T17:00:00.0000000Z",
        "metricName": "QueryDuration",
        "timeGrain": "PT1M"
    }
    ]
}
```

# <a name="diagnostic-logs"></a>[Tanılama günlükleri](#tab/diagnostic-logs)
#### <a name="example"></a>Örnek

Aşağıda, Azure Veri Gezgini [tanılama günlüğüne](using-diagnostic-logs.md#diagnostic-logs-schema)bir örnek verilmiştir:

```json
{
    "time": "2019-08-26T13:22:36.8804326Z",
    "resourceId": "/SUBSCRIPTIONS/<subscriptionID>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/<cluster-name>",
    "operationName": "MICROSOFT.KUSTO/CLUSTERS/INGEST/ACTION",
    "operationVersion": "1.0",
    "category": "FailedIngestion",
    "resultType": "Failed",
    "correlationId": "d59882f1-ad64-4fc4-b2ef-d663b6cc1cc5",
    "properties": {
        "OperationId": "00000000-0000-0000-0000-000000000000",
        "Database": "Kusto",
        "Table": "Table_13_20_prod",
        "FailedOn": "2019-08-26T13:22:36.8804326Z",
        "IngestionSourceId": "d59882f1-ad64-4fc4-b2ef-d663b6cc1cc5",
        "Details":
        {
            "error": 
            {
                "code": "BadRequest_DatabaseNotExist",
                "message": "Request is invalid and cannot be executed.",
                "@type": "Kusto.Data.Exceptions.DatabaseNotFoundException",
                "@message": "Database 'Kusto' was not found.",
                "@context": 
                {
                    "timestamp": "2019-08-26T13:22:36.7179157Z",
                    "serviceAlias": "<cluster-name>",
                    "machineName": "KEngine000001",
                    "processName": "Kusto.WinSvc.Svc",
                    "processId": 5336,
                    "threadId": 6528,
                    "appDomainName": "Kusto.WinSvc.Svc.exe",
                    "clientRequestd": "DM.IngestionExecutor;a70ddfdc-b471-4fc7-beac-bb0f6e569fe8",
                    "activityId": "f13e7718-1153-4e65-bf82-8583d712976f",
                    "subActivityId": "2cdad9d0-737b-4c69-ac9a-22cf9af0c41b",
                    "activityType": "DN.AdminCommand.DataIngestPullCommand",
                    "parentActivityId": "2f65e533-a364-44dd-8d45-d97460fb5795",
                    "activityStack": "(Activity stack: CRID=DM.IngestionExecutor;a70ddfdc-b471-4fc7-beac-bb0f6e569fe8 ARID=f13e7718-1153-4e65-bf82-8583d712976f > DN.Admin.Client.ExecuteControlCommand/5b764b32-6017-44a2-89e7-860eda515d40 > P.WCF.Service.ExecuteControlCommandInternal..IAdminClientServiceCommunicationContract/c2ef9344-069d-44c4-88b1-a3570697ec77 > DN.FE.ExecuteControlCommand/2f65e533-a364-44dd-8d45-d97460fb5795 > DN.AdminCommand.DataIngestPullCommand/2cdad9d0-737b-4c69-ac9a-22cf9af0c41b)"
                },
                "@permanent": true
            }
        },
        "ErrorCode": "BadRequest_DatabaseNotExist",
        "FailureStatus": "Permanent",
        "RootActivityId": "00000000-0000-0000-0000-000000000000",
        "OriginatesFromUpdatePolicy": false,
        "ShouldRetry": false,
        "IngestionSourcePath": "https://c0skstrldkereneus01.blob.core.windows.net/aam-20190826-temp-e5c334ee145d4b43a3a2d3a96fbac1df/3216_test_3_columns_invalid_8f57f0d161ed4a8c903c6d1073005732_59951f9ca5d143b6bdefe52fa381a8ca.zip"
    }
}
```
# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
#### <a name="example"></a>Örnek

Azure etkinlik günlükleri, aboneliğinizdeki kaynaklarüzerinde gerçekleştirilen işlemlerhakkında bilgi sağlayan abonelik düzeyinde günlüklerdir. Aşağıda erişimi denetlemek için bir etkinlik günlüğü olayı örneği verilmiştir:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/<subscriptionID>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/<subscriptionID>/RESOURCEGROUPS/<resource-group>/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```
---

## <a name="set-up-an-ingestion-pipeline-in-azure-data-explorer"></a>Azure Veri Gezgini'nde bir temizleme ardışık hattı ayarlama

Azure Veri Gezgini ardışık bir aygıt kurma, [tablo oluşturma ve veri alma](/azure/data-explorer/ingest-sample-data#ingest-data)gibi birkaç adım içerir. Ayrıca verileri işleyebilir, eşleyebilir ve güncelleştirebilirsiniz.

### <a name="connect-to-the-azure-data-explorer-web-ui"></a>Azure Veri Gezgini Web UI'sine bağlanma

Azure Veri Gezgini *Test Veritabanı* veritabanınızda, Azure Veri Gezgini Web UI'sini açmak için **Sorgu'yu** seçin.

![Sorgu sayfası](media/ingest-data-no-code/query-database.png)

### <a name="create-the-target-tables"></a>Hedef tabloları oluşturma

Azure Monitor günlüklerinin yapısı tabular değildir. Verileri manipüle edecek ve her olayı bir veya daha fazla kayda genişletin. Ham veriler, etkinlik günlükleri için *ActivityLogsRawRecords* adlı bir ara tabloya ve tanılama ölçümleri ve günlükler için *DiagnosticRawRecords'a* yutulacaktır. O zaman, veriler manipüle edilecek ve genişletilecektir. Bir güncelleştirme ilkesi kullanılarak, genişletilmiş veriler etkinlik günlükleri için *ActivityLogs* tablosuna, tanılama ölçümleri için *Tanısal Ölçümlere* ve tanılama günlükleri için *Tanılama Günlükleri'ne* yutulacaktır. Bu, etkinlik günlüklerini sindiren iki ayrı tablo ve tanılama ölçümleri ve günlükleri sindirmek için üç ayrı tablo oluşturmanız gerektiği anlamına gelir.

Azure Veri Gezgini veritabanında hedef tabloları oluşturmak için Azure Veri Gezgini Web UI'sini kullanın.

# <a name="diagnostic-metrics"></a>[Tanısal ölçümler](#tab/diagnostic-metrics)
#### <a name="create-tables-for-the-diagnostic-metrics"></a>Tanılama ölçümleri için tablolar oluşturma

1. *TestDatabase* veritabanında, tanılama ölçümlerini depolamak için *DiagnosticMetrics* adında bir tablo oluşturun. Aşağıdaki `.create table` denetim komutunu kullanın:

    ```kusto
    .create table DiagnosticMetrics (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Tablooluşturmak için **Çalıştır'ı** seçin.

    ![Sorgu çalıştırma](media/ingest-data-no-code/run-query.png)

1. Aşağıdaki sorguyu kullanarak veri işleme için *TestDatabase* veritabanında *DiagnosticRawRecords* adlı ara veri tablosunu oluşturun. Tablooluşturmak için **Çalıştır'ı** seçin.

    ```kusto
    .create table DiagnosticRawRecords (Records:dynamic)
    ```

1. Ara tablo için sıfır [bekletme ilkesini](/azure/kusto/management/retention-policy) ayarlayın:

    ```kusto
    .alter-merge table DiagnosticRawRecords policy retention softdelete = 0d
    ```

# <a name="diagnostic-logs"></a>[Tanılama günlükleri](#tab/diagnostic-logs)
#### <a name="create-tables-for-the-diagnostic-logs"></a>Tanılama günlükleri için tablolar oluşturma 

1. *TestDatabase* veritabanında, tanılama günlüğü kayıtlarını depolamak için *DiagnosticLogs* adında bir tablo oluşturun. Aşağıdaki `.create table` denetim komutunu kullanın:

    ```kusto
    .create table DiagnosticLogs (Timestamp:datetime, ResourceId:string, OperationName:string, Result:string, OperationId:string, Database:string, Table:string, IngestionSourceId:string, IngestionSourcePath:string, RootActivityId:string, ErrorCode:string, FailureStatus:string, Details:string)
    ```

1. Tablooluşturmak için **Çalıştır'ı** seçin.

1. Aşağıdaki sorguyu kullanarak veri işleme için *TestDatabase* veritabanında *DiagnosticRawRecords* adlı ara veri tablosunu oluşturun. Tablooluşturmak için **Çalıştır'ı** seçin.

    ```kusto
    .create table DiagnosticRawRecords (Records:dynamic)
    ```

1. Ara tablo için sıfır [bekletme ilkesini](/azure/kusto/management/retention-policy) ayarlayın:

    ```kusto
    .alter-merge table DiagnosticRawRecords policy retention softdelete = 0d
    ```

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
#### <a name="create-tables-for-the-activity-logs"></a>Etkinlik günlükleri için tablolar oluşturma 

1. Etkinlik günlüğü kayıtlarını almak için *TestDatabase* veritabanında *ActivityLogs* adında bir tablo oluşturun. Tabloyu oluşturmak için aşağıdaki Azure Veri Gezgini sorgusunu çalıştırın:

    ```kusto
    .create table ActivityLogs (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Veri işleme için *TestDatabase* veritabanında *ActivityLogsRawRecords* adlı ara veri tablosunu oluşturun:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

1. Ara tablo için sıfır [bekletme ilkesini](/azure/kusto/management/retention-policy) ayarlayın:

    ```kusto
    .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    ```
---

### <a name="create-table-mappings"></a>Tablo eşlemeleri oluşturma

 Veri biçimi olduğundan, `json`veri eşleme gereklidir. Eşleme, `json` her json yolunu bir tablo sütun adı ile eşler.

# <a name="diagnostic-metrics--diagnostic-logs"></a>[Tanılama ölçümleri / Tanılama günlükleri](#tab/diagnostic-metrics+diagnostic-logs) 
#### <a name="map-diagnostic-metrics-and-logs-to-the-table"></a>Tanılama ölçümlerini ve günlükleri tabloya eşle

Tanılama metrik ve günlük verilerini tabloyla eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table DiagnosticRawRecords ingestion json mapping 'DiagnosticRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
#### <a name="map-activity-logs-to-the-table"></a>Harita etkinlik günlükleri tabloya

Etkinlik günlüğü verilerini tabloyla eşlemek için aşağıdaki sorguyu kullanın:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[{"column":"Records","path":"$.records"}]'
```
---

### <a name="create-the-update-policy-for-metric-and-log-data"></a>Metrik ve günlük verileri için güncelleştirme ilkesini oluşturma

# <a name="diagnostic-metrics"></a>[Tanısal ölçümler](#tab/diagnostic-metrics)
#### <a name="create-data-update-policy-for-diagnostics-metrics"></a>Tanılama ölçümleri için veri güncelleştirme ilkesi oluşturma

1. Tanılama metrik kayıtlarının koleksiyonunu genişleten bir [işlev](/azure/kusto/management/functions) oluşturun, böylece koleksiyondaki her değer ayrı bir satır alır. İşleç kullanın: [`mv-expand`](/azure/kusto/query/mvexpandoperator)
     ```kusto
    .create function DiagnosticMetricsExpand() {
        DiagnosticRawRecords
        | mv-expand events = Records
        | where isnotempty(events.metricName)
        | project
            Timestamp = todatetime(events['time']),
            ResourceId = tostring(events.resourceId),
            MetricName = tostring(events.metricName),
            Count = toint(events['count']),
            Total = todouble(events.total),
            Minimum = todouble(events.minimum),
            Maximum = todouble(events.maximum),
            Average = todouble(events.average),
            TimeGrain = tostring(events.timeGrain)
    }
    ```

2. Güncelleştirme [ilkesini](/azure/kusto/concepts/updatepolicy) hedef tabloya ekleyin. Bu ilke, *diagnosticRawRecords* ara veri tablosunda yeni alınan herhangi bir veri üzerinde sorguyu otomatik olarak çalıştıracak ve sonuçlarını *DiagnosticMetrics* tablosuna kaydeder:

    ```kusto
    .alter table DiagnosticMetrics policy update @'[{"Source": "DiagnosticRawRecords", "Query": "DiagnosticMetricsExpand()", "IsEnabled": "True", "IsTransactional": true}]'
    ```

# <a name="diagnostic-logs"></a>[Tanılama günlükleri](#tab/diagnostic-logs)
#### <a name="create-data-update-policy-for-diagnostics-logs"></a>Tanılama günlükleri için veri güncelleştirme ilkesi oluşturma

1. Tanılama günlükleri kayıtlarının koleksiyonunu genişleten bir [işlev](/azure/kusto/management/functions) oluşturun, böylece koleksiyondaki her değer ayrı bir satır alır. Bir Azure Veri Gezgini kümesinde giriş günlüklerini etkinleştirin ve [yutma günlükleri şemasını kullanırsınız.](/azure/data-explorer/using-diagnostic-logs#diagnostic-logs-schema) Başarılı ve başarısız olan alım için bir tablo oluşturursunuz, bazı alanlar başarılı alma için boş olur (örneğin Hata Kodu). İşleç kullanın: [`mv-expand`](/azure/kusto/query/mvexpandoperator)

    ```kusto
    .create function DiagnosticLogsExpand() {
        DiagnosticRawRecords
        | mv-expand events = Records
        | where isnotempty(events.operationName)
        | project
            Timestamp = todatetime(events['time']),
            ResourceId = tostring(events.resourceId),
            OperationName = tostring(events.operationName),
            Result = tostring(events.resultType),
            OperationId = tostring(events.properties.OperationId),
            Database = tostring(events.properties.Database),
            Table = tostring(events.properties.Table),
            IngestionSourceId = tostring(events.properties.IngestionSourceId),
            IngestionSourcePath = tostring(events.properties.IngestionSourcePath),
            RootActivityId = tostring(events.properties.RootActivityId),
            ErrorCode = tostring(events.properties.ErrorCode),
            FailureStatus = tostring(events.properties.FailureStatus),
            Details = tostring(events.properties.Details)
    }
    ```

2. Güncelleştirme [ilkesini](/azure/kusto/concepts/updatepolicy) hedef tabloya ekleyin. Bu ilke, *diagnosticRawRecords* ara veri tablosunda yeni alınan herhangi bir veri üzerinde sorguyu otomatik olarak çalıştıracak ve sonuçlarını *DiagnosticLogs* tablosuna kaydeder:

    ```kusto
    .alter table DiagnosticLogs policy update @'[{"Source": "DiagnosticRawRecords", "Query": "DiagnosticLogsExpand()", "IsEnabled": "True", "IsTransactional": true}]'
    ```

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
#### <a name="create-data-update-policy-for-activity-logs"></a>Etkinlik günlükleri için veri güncelleştirme ilkesi oluşturma

1. Koleksiyondaki her değerin ayrı bir satır alması için etkinlik günlüğü kayıtlarının koleksiyonunu genişleten bir [işlev](/azure/kusto/management/functions) oluşturun. İşleç kullanın: [`mv-expand`](/azure/kusto/query/mvexpandoperator)

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mv-expand events = Records
        | project
            Timestamp = todatetime(events['time']),
            ResourceId = tostring(events.resourceId),
            OperationName = tostring(events.operationName),
            Category = tostring(events.category),
            ResultType = tostring(events.resultType),
            ResultSignature = tostring(events.resultSignature),
            DurationMs = toint(events.durationMs),
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events.location),
            Level = tostring(events.level)
    }
    ```

2. Güncelleştirme [ilkesini](/azure/kusto/concepts/updatepolicy) hedef tabloya ekleyin. Bu ilke, *sorguyu ActivityLogsRawRecords* ara veri tablosunda yeni alınan herhangi bir veri üzerinde otomatik olarak çalıştıracak ve sonuçlarını *ActivityLogs* tablosuna kaydeder:

    ```kusto
    .alter table ActivityLogs policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True", "IsTransactional": true}]'
    ```
---

## <a name="create-an-azure-event-hubs-namespace"></a>Azure Etkinlik Hub'ları ad alanı oluşturma

Azure tanı ayarları, ölçümlerin ve günlüklerin bir depolama hesabına veya bir etkinlik merkezine dışa aktarılmasını sağlar. Bu öğreticide, ölçümleri ve günlükleri bir etkinlik merkezi üzerinden yönlendireceğiz. Aşağıdaki adımlardaki tanılama ölçümleri ve günlükleri için bir Olay Hub'ları ad alanı ve olay hub'ı oluşturursunuz. Azure Monitor, etkinlik günlükleri için etkinlik merkezi *öngörüleri-operasyonel günlükleri* oluşturur.

1. Azure portalında bir Azure Kaynak Yöneticisi şablonu kullanarak bir etkinlik merkezi oluşturun. Bu makaledeki diğer adımları izlemek için **Azure'a Dağıt** düğmesini sağ tıklatın ve ardından **yeni pencerede Aç'ı**seçin. **Azure'a Dağıt** düğmesi sizi Azure portalına götürür.

    [![Azure düğmesine dağıt](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Tanılama günlükleri için bir Olay Hub'ları ad alanı ve olay merkezi oluşturun.

    ![Olay göbeği oluşturma](media/ingest-data-no-code/event-hub.png)

1. Formu aşağıdaki bilgilerle doldurun. Aşağıdaki tabloda listelenmemiş tüm ayarlar için varsayılan değerleri kullanın.

    **Ayar** | **Önerilen değer** | **Açıklama**
    |---|---|---|
    | **Abonelik** | *Aboneliğiniz* | Olay hub'ınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | **Kaynak grubu** | *test-resource-group* | Yeni bir kaynak grubu oluşturun. |
    | **Konum** | İhtiyaçlarınızı en iyi karşılayan bölgeyi seçin. | Olay Hub'ları ad alanını diğer kaynaklarla aynı konumda oluşturun.
    | **Ad alanı adı** | *Azureİzleme Verileri* | Ad alanınızı tanımlayan benzersiz bir ad seçin.
    | **Olay hub adı** | *Tanılama Verileri* | Olay hub'ı benzersiz bir kapsayıcı kapsamı sunan ad alanında bulunur. |
    | **Tüketici grubu adı** | *adxpipeline* | Bir tüketici grubu adı oluşturun. Tüketici grupları birden fazla tüketici uygulamasının ayrı olay akışı görünümüne sahip olmasını sağlar. |
    | | |

## <a name="connect-azure-monitor-metrics-and-logs-to-your-event-hub"></a>Azure Monitör ölçümleri ve günlükleri etkinlik merkezinize bağlayın

Artık tanılama ölçümlerinizi, günlüklerinizi ve etkinlik günlüklerinizi etkinlik merkezine bağlamanız gerekir.

# <a name="diagnostic-metrics--diagnostic-logs"></a>[Tanılama ölçümleri / Tanılama günlükleri](#tab/diagnostic-metrics+diagnostic-logs) 
### <a name="connect-diagnostic-metrics-and-logs-to-your-event-hub"></a>Tanılama ölçümlerini ve günlükleri etkinlik merkezinize bağlama

Ölçümleri dışa aktarmak için bir kaynak seçin. Olay Hub'ları ad alanı, Azure Key Vault, Azure IoT Hub ve Azure Veri Gezgini kümeleri dahil olmak üzere çeşitli kaynak türleri tanı verisi dışa aktarmayı destekler. Bu öğreticide, kaynağımız olarak bir Azure Veri Gezgini kümesi kullanacağız, sorgu performans ölçümlerini ve yutma sonuçları günlüklerini gözden geçireceğiz.

1. Azure portalında Kusto kümenizi seçin.
1. **Tanılama ayarlarını**seçin ve ardından **tanılama** bağlantısını aç bağlantısını seçin. 

    ![Tanılama ayarları](media/ingest-data-no-code/diagnostic-settings.png)

1. **Tanılama ayarları** bölmesi açılır. Aşağıdaki adımları uygulayın:
   1. Tanılama günlüğü verilerine *ADXExportedData*adını verin.
   1. **LOG**altında, hem **Başarılı Alma** hem de **FailedIngestion** onay kutularını seçin.
   1. **METRIC**altında, **Sorgu performans** onay kutusunu seçin.
   1. Olay **hub'ına Akış'ı** seçin onay kutusunu seçin.
   1. **Yapılandır**'ı seçin.

      ![Tanılama ayarları bölmesi](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Olay **hub'ını Seç** bölmesinde, tanılama günlüklerinden oluşturduğunuz olay merkezine veri verme yöntemini nasıl yapılandırın:
    1. Olay **hub'ı seç** listesinde *AzureMonitoringData'yı*seçin.
    1. Olay **hub adı** listesini seç'te *DiagnosticData'yı*seçin.
    1. Olay **hub ilke adı** listesinde **RootManagerSharedAccessKey'i**seçin.
    1. **Tamam'ı**seçin.

1. **Kaydet'i**seçin.

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
### <a name="connect-activity-logs-to-your-event-hub"></a>Etkinlik günlüklerini etkinlik merkezinize bağlama

1. Azure portalının sol menüsünde **Etkinlik günlüğü'nü**seçin.
1. **Etkinlik günlüğü** penceresi açılır. **Etkinlik Hub'ına Dışa Aktar'ı**seçin.

    ![Etkinlik günlüğü penceresi](media/ingest-data-no-code/activity-log.png)

1. **Dışa** Aktarma etkinlik günlüğü penceresi açılır:
 
    ![Etkinlik günlüğü pencereyi dışa aktarma](media/ingest-data-no-code/export-activity-log.png)

1. **Dışa Aktarma etkinlik günlüğü** penceresinde aşağıdaki adımları izleyin:
      1. Aboneliğinizi seçin.
      1. **Bölgeler** listesinde **tümünü seç'i**seçin.
      1. Olay **hub'ına Dışa** Aktar'ı seçin onay kutusunu seçin.
      1. **Etkinlik hub'ını seç** bölmesini açmak için servis veri aracı ad **alanı** seçin'i seçin.
      1. Etkinlik **hub'ını Seç** bölmesinde aboneliğinizi seçin.
      1. Olay **hub'ı seç** listesinde *AzureMonitoringData'yı*seçin.
      1. Olay **merkezi ilke adı** listesinde varsayılan olay merkezi ilkesi adını seçin.
      1. **Tamam'ı**seçin.
      1. Pencerenin sol üst köşesinde **Kaydet'i**seçin.
   Ad *öngörüleri-operasyonel günlükleri* içeren bir olay merkezi oluşturulur.
---

### <a name="see-data-flowing-to-your-event-hubs"></a>Etkinlik hub'larınıza akan verileri görme

1. Bağlantı tanımlanana ve etkinlik merkezine yapılan etkinlik günlüğü dışa aktarAna kadar birkaç dakika bekleyin. Oluşturduğunuz etkinlik hub'larını görmek için Olay Hub'ları ad alanınıza gidin.

    ![Oluşturulan olay hub'ları](media/ingest-data-no-code/event-hubs-created.png)

1. Etkinlik merkezinize akan verileri görün:

    ![Olay merkezinin verileri](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-an-event-hub-to-azure-data-explorer"></a>Etkinlik merkezini Azure Veri Gezgini'ne bağlama

Şimdi tanılama ölçümleri ve günlükleri ve etkinlik günlükleri için veri bağlantıları oluşturmanız gerekir.

### <a name="create-the-data-connection-for-diagnostic-metrics-and-logs-and-activity-logs"></a>Tanılama ölçümleri, günlükler ve etkinlik günlükleri için veri bağlantısı oluşturma

1. *Kustodocs*adlı Azure Veri Gezgini kümenizde sol menüde **Veritabanları'nı** seçin.
1. **Veritabanları** penceresinde, *TestDatabase* veritabanınızı seçin.
1. Sol menüde **Veri alımını**seçin.
1. Veri **alma** penceresinde **+ Veri Bağlantısı Ekle'yi**tıklatın.
1. Veri **bağlantısı** penceresinde aşağıdaki bilgileri girin:

    ![Olay merkezi veri bağlantısı](media/ingest-data-no-code/event-hub-data-connection.png)

# <a name="diagnostic-metrics--diagnostic-logs"></a>[Tanılama ölçümleri / Tanılama günlükleri](#tab/diagnostic-metrics+diagnostic-logs) 

1. **Veri Bağlantısı** penceresinde aşağıdaki ayarları kullanın:

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Veri bağlantısı adı** | *DiagnosticsLogsBağlantı* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | **Olay merkezi ad alanı** | *Azureİzleme Verileri* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | **Olay hub'ı** | *Tanılama Verileri* | Oluşturduğunuz olay hub'ı. |
    | **Tüketici grubu** | *adxpipeline* | Oluşturduğunuz olay hub'ında tanımlanan tüketici grubu. |
    | | |

    Hedef tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğretici için, tablo adını, veri biçimini ve eşlemi belirttiğiniz statik yönlendirmeyi (varsayılan) kullanırsınız. **Verilerimde yönlendirme bilgileri var** seçeneğini işaretlemeyin.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Tablo** | *TeşhisHam Kayıtlar* | *TestDatabase* veritabanında oluşturduğunuz tablo. |
    | **Veri biçimi** | *Json* | Tabloda kullanılan biçim. |
    | **Sütun eşleme** | *TeşhisRawRecordsMapping* | *TestDatabase* veritabanında oluşturduğunuz ve gelen JSON verilerini *DiagnosticRawRecords* tablosunun sütun adları ve veri türlerine eşleyen eşleme.|
    | | |

1. **Oluştur'u**seçin.  

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)

1. **Veri Bağlantısı** penceresinde aşağıdaki ayarları kullanın:

    Veri kaynağı:

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Veri bağlantısı adı** | *ActivityLogsBağlantı* | Azure Veri Gezgini'nde oluşturmak istediğiniz bağlantının adı.|
    | **Olay merkezi ad alanı** | *Azureİzleme Verileri* | Önceden seçtiğiniz ve ad alanınızı tanımlayan ad. |
    | **Olay hub'ı** | *insights-operasyonel-günlükleri* | Oluşturduğunuz olay hub'ı. |
    | **Tüketici grubu** | *$Default* | Varsayılan tüketici grubu. Gerekirse, farklı bir tüketici grubu oluşturabilirsiniz. |
    | | |

    Hedef tablo:

    İki yönlendirme seçeneği vardır: *statik* ve *dinamik*. Bu öğretici için, tablo adını, veri biçimini ve eşlemi belirttiğiniz statik yönlendirmeyi (varsayılan) kullanırsınız. **Verilerimde yönlendirme bilgileri var** seçeneğini işaretlemeyin.

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | **Tablo** | *ActivityLogsRawRecords* | *TestDatabase* veritabanında oluşturduğunuz tablo. |
    | **Veri biçimi** | *Json* | Tabloda kullanılan biçim. |
    | **Sütun eşleme** | *ActivityLogsRawRecordsMapping* | *TestDatabase* veritabanında oluşturduğunuz ve gelen JSON verilerini *ActivityLogsRawRecords* tablosunun sütun adları ve veri türlerine eşleyen eşleme.|
    | | |

1. **Oluştur'u**seçin.  
---

## <a name="query-the-new-tables"></a>Yeni tabloları sorgula

Artık veri akan bir boru hattınız var. Küme üzerinden alım varsayılan olarak 5 dakika sürer, bu nedenle sorgulamaya başlamadan önce verilerin birkaç dakika akmasına izin verin.

# <a name="diagnostic-metrics"></a>[Tanısal ölçümler](#tab/diagnostic-metrics)
### <a name="query-the-diagnostic-metrics-table"></a>Tanılama ölçümleri tablosunu sorgula

Aşağıdaki sorgu, Azure Veri Gezgini'ndeki tanılama metrik kayıtlarından sorgu süresi verilerini analiz eder:

```kusto
DiagnosticMetrics
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Sorgu sonuçları:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

# <a name="diagnostic-logs"></a>[Tanılama günlükleri](#tab/diagnostic-logs)
### <a name="query-the-diagnostic-logs-table"></a>Tanılama günlükleri tablosunu sorgula

Bu ardışık işlem, bir olay merkezi üzerinden alımlar üretir. Bu alımların sonuçlarını gözden geçireceksiniz.
Aşağıdaki sorgu, bir örneğini `Database` `Table` ve her aralık `IngestionSourcePath` için bir dakika içinde kaç yutma tahakkuk ettiğini analiz eder:

```kusto
DiagnosticLogs
| where Timestamp > ago(15m) and OperationName has 'INGEST'
| summarize count(), any(Database, Table, IngestionSourcePath) by bin(Timestamp, 1m)
```

Sorgu sonuçları:

|   |   |
| --- | --- |
|   |  count_ | any_Database | any_Table | any_IngestionSourcePath
|   | 00:06.156 | TestDatabase | TeşhisHam Kayıtlar | https://rtmkstrldkereneus00.blob.core.windows.net/20190827-readyforaggregation/1133_TestDatabase_DiagnosticRawRecords_6cf02098c0c74410bd8017c2d458b45d.json.zip
| | |

# <a name="activity-logs"></a>[Etkinlik günlükleri](#tab/activity-logs)
### <a name="query-the-activity-logs-table"></a>Etkinlik günlükleri tablosunu sorgula

Aşağıdaki sorgu, Azure Veri Gezgini'ndeki etkinlik günlüğü kayıtlarından gelen verileri analiz eder:

```kusto
ActivityLogs
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Sorgu sonuçları:

|   |   |
| --- | --- |
|   |  avg(Sürems) |
|   | 768.333 |
| | |

---

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini için Yazma sorgularını](write-queries.md)kullanarak Azure Veri Gezgini'nden çıkardığınız verilere daha birçok sorgu yazmayı öğrenin.
* [Tanılama günlüklerini kullanarak Azure Veri Gezgini işlemlerine izleme](using-diagnostic-logs.md)
* [Kümelerin sistem durumunu izlemek için ölçümleri kullanma](using-metrics.md)
