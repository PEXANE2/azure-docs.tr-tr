---
title: Azure Tanılama ayarlarını kullanarak Azure Cosmos DB verilerini izleme
description: Azure Cosmos DB'de depolanan verilerin performansını ve kullanılabilirliğini izlemek için Azure Tanılama ayarlarını nasıl kullanacağınızı öğrenin
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: sngun
ms.openlocfilehash: f5a0b0f71a72ea76940450f73354fda230e09c5c
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521041"
---
# <a name="monitor-azure-cosmos-db-data-by-using-diagnostic-settings-in-azure"></a>Azure'da tanılama ayarlarını kullanarak Azure Cosmos DB verilerini izleme

Azure'daki tanılama ayarları kaynak günlüklerini toplamak için kullanılır. Azure kaynak Günlükleri bir kaynak tarafından yayımlanır ve bu kaynağın çalışması hakkında zengin ve sık veri sağlar. Bu günlükler istek başına yakalanır ve "veri düzlemi günlükleri" olarak da adlandırılır. Veri düzlemi işlemlerine bazı örnekler sil, ekle ve readFeed'dir. Bu günlüklerin içeriği kaynak türüne göre değişir.

Platform ölçümleri ve Etkinlik günlükleri otomatik olarak toplanır, ancak kaynak günlüklerini toplamak veya Azure Monitor'un dışına iletmek için bir tanılama ayarı oluşturmanız gerekir. Aşağıdaki adımları kullanarak Azure Cosmos hesapları için tanı ayarını açabilirsiniz:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure Cosmos hesabınıza gidin. **Tanılama ayarları** bölmesini açın ve **tanılama ayarını ekle seçeneğini belirleyin.**

1. **Tanılama ayarları** bölmesinde, formu aşağıdaki ayrıntılarla doldurun: 

    * **Adı**: Oluşturmak için günlükleri için bir ad girin.

    * **Günlükleri Arşiv'de bir depolama hesabına,** **akışla etkinlik merkezine** veya Günlük **Analitiğine Gönder'de** saklayabilirsiniz

1. Tanılama ayarı oluşturduğunuzda, hangi günlük kategorisinin toplandığını belirtirsiniz. Azure Cosmos DB tarafından desteklenen günlük kategorileri, kendileri tarafından toplanan örnek günlüklerle birlikte aşağıda listelenmiştir:

 * **DataPlaneRequests**: Azure Cosmos DB'deki SQL, Graph, MongoDB, Cassandra ve Table API hesaplarını içeren tüm API'lere arka uç isteklerini günlüğe kaydetmek için bu seçeneği belirleyin. Unutulmaması gereken temel `Requestcharge` `statusCode`özellikler `clientIPaddress`şunlardır: , , , ve `partitionID`.

    ```json
    { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
    ```

* **MongoRequests**: Azure Cosmos DB'nin MongoDB için API'sine istekler sunmak için kullanıcı tarafından başlatılan istekleri ön uçtan günlüğe kaydetmek için bu seçeneği belirleyin. Bu günlük türü diğer API hesapları için kullanılamaz. Dikkat edilmesi gereken `Requestcharge`önemli `opCode`özellikler şunlardır: , . Tanılama günlüklerinde MongoRequests'u etkinleştirdiğinizde, DataPlaneRequests'i kapattığınızdan emin olun. API'de yapılan her istek için bir günlük görürsünüz.

    ```json
    { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
    ```

* **CassandraRequests**: Azure Cosmos DB'nin Cassandra api'sine istekler sunmak için kullanıcı tarafından başlatılan istekleri ön uçtan günlüğe kaydetmek için bu seçeneği belirleyin. Bu günlük türü diğer API hesapları için kullanılamaz. Unutulmaması gereken temel `operationName` `requestCharge`özellikler `piiCommandText`, , . Tanılama günlüklerinde CassandraRequests'ı etkinleştirdiğinizde, DataPlaneRequests'i kapattığınızdan emin olun. API'de yapılan her istek için bir günlük görürsünüz.

   ```json
   { "time": "2020-03-30T23:55:10.9579593Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "CassandraRequests", "operationName": "QuerySelect", "properties": {"activityId": "6b33771c-baec-408a-b305-3127c17465b6","opCode": "<empty>","errorCode": "-1","duration": "0.311900","requestCharge": "1.589237","databaseName": "system","collectionName": "local","retryCount": "<empty>","authorizationTokenType": "PrimaryMasterKey","address": "104.42.195.92","piiCommandText": "{"request":"SELECT key from system.local"}","userAgent": """"}}
   ```

* **QueryRuntimeStatistics**: Yürütülen sorgu metnini günlüğe kaydetmek için bu seçeneği seçin. Bu günlük türü yalnızca SQL API hesapları için kullanılabilir.

    ```json
    { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
    ```

* **PartitionKeyStatistics**: Bölüm anahtarlarının istatistiklerini günlüğe kaydetmek için bu seçeneği seçin. Bu şu anda bölüm anahtarlarının depolama boyutu (KB) ile temsil edilir. Bu makalenin [Azure Tanılama sorguları bölümünü kullanarak Sorun Giderme sorunlarına](#diagnostic-queries) bakın. Örneğin "PartitionKeyStatistics" kullanan sorgular. Günlük, çoğu veri depolama alanı nın içinde olan ilk üç bölüm anahtarına karşı yayılır. Bu günlük, KB'de abonelik kimliği, bölge adı, veritabanı adı, koleksiyon adı, bölüm anahtarı ve depolama boyutu gibi verileri içerir.

    ```json
    { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
    ```

* **PartitionKeyRUConsumption**: Bu günlük, bölüm tuşlarının saniyede birleştirilmiş RU/s tüketimini bildirir. Şu anda Azure Cosmos DB, yalnızca SQL API hesapları ve nokta okuma/yazma ve depolanan yordam işlemleri için bölüm anahtarlarını rapor eder. diğer API'ler ve işlem türleri desteklenmez. Diğer API'ler için tanılama günlüğü tablosundaki bölüm anahtar sütunu boş olacaktır. Bu günlük, abonelik kimliği, bölge adı, veritabanı adı, koleksiyon adı, bölüm anahtarı, işlem türü ve istek ücreti gibi verileri içerir. Bu makalenin [Azure Tanılama sorguları bölümünü kullanarak Sorun Giderme sorunlarına](#diagnostic-queries) bakın. Örneğin "PartitionKeyRUConsumption" kullanan sorgular. 

* **ControlPlaneRequests**: Bu günlük, hesap oluşturma, bir bölge ekleme veya kaldırma, hesap çoğaltma ayarlarını güncelleme vb. gibi kontrol uçağı işlemleriyle ilgili ayrıntıları içerir. Bu günlük türü, SQL (Core), MongoDB, Gremlin, Cassandra, Tablo API'yi içeren tüm API türleri için kullanılabilir.

* **İstekler**: Azure Cosmos DB'den tanı ayarındaki hedeflere metrik veri toplamak için bu seçeneği belirleyin. Bu, Azure Ölçümleri'nde otomatik olarak toplanan verilerle aynıdır. Her iki veri türüni birlikte analiz etmek ve azure verilerini Azure Monitor'un dışına göndermek için kaynak günlükleriyle metrik veriler toplayın.

Azure portalı, CLI veya PowerShell'i kullanarak tanılama ayarı oluşturma hakkında ayrıntılı bilgi için Azure [makalesinde platform günlükleri ve ölçümleri toplamak için tanı ayarını oluştur'a](../azure-monitor/platform/diagnostic-settings.md) bakın.


## <a name="troubleshoot-issues-with-diagnostics-queries"></a><a id="diagnostic-queries"></a>Tanılama sorgularıyla ilgili sorunları giderme

1. Pahalı sorgular için istek ücretleri nasıl alınır?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 10.0
   | project activityId_g, requestCharge_s
   | join kind= inner (
   AzureDiagnostics
   | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
   | project activityId_g, querytext_s
   ) on $left.activityId_g == $right.activityId_g
   | order by requestCharge_s desc
   | limit 100
   ```

1. HANGI operasyonların en çok RU/s'yi aldığını nasıl bulabilirim?

    ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(responseLength_s), max(requestLength_s), max(requestCharge_s), count = count() by OperationName, requestResourceType_s, userAgent_s, collectionRid_s, bin(TimeGenerated, 1h)
   ```
1. Nasıl farklı işlemler için dağıtım almak için?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize count = count()  by OperationName, requestResourceType_s, bin(TimeGenerated, 1h) 
   ```

1. Bir bölümün tükettiği maksimum iş verimi nedir?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(requestCharge_s) by bin(TimeGenerated, 1h), partitionId_g
   ```

1. Saniyede bölüm tuşları RU / s tüketimi hakkında bilgi almak için nasıl?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s, TimeGenerated 
   | order by TimeGenerated asc 
   ```

1. Belirli bir bölüm anahtarı için istek ücreti nasıl alabilirsiniz?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where parse_json(partitionKey_s)[0] == "2" 
   ```

1. Nasıl en ru / s belirli bir dönemde tüketilen üst bölüm tuşları almak için? 

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where TimeGenerated >= datetime("11/26/2019, 11:20:00.000 PM") and TimeGenerated <= datetime("11/26/2019, 11:30:00.000 PM") 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s 
   | order by total desc
    ```

1. Depolama boyutu 8 GB'tan büyük olan bölüm anahtarlarının günlükleri nasıl elde edilir?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics"
   | where todouble(sizeKb_d) > 800000
   ```

1. Veritabanı hesabı için ilk üç bölüm arasında çarpık değerlendirmek için bölüm Anahtar istatistikleri almak için nasıl?

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB için Azure Monitör](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json)
* [Azure Cosmos DB'deki ölçümlerle izleme ve hata ayıklama](use-metrics.md)
