---
title: Azure tanılama ayarlarını kullanarak Azure Cosmos DB verilerini izleme
description: Azure Cosmos DB depolanan verilerin performansını ve kullanılabilirliğini izlemek için Azure tanılama ayarlarını nasıl kullanacağınızı öğrenin
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: sngun
ms.openlocfilehash: b1a507c54c6a6555fc945dd35ee6e54d37d49bfd
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857578"
---
# <a name="monitor-azure-cosmos-db-data-by-using-diagnostic-settings-in-azure"></a>Azure 'da tanılama ayarlarını kullanarak Azure Cosmos DB verilerini izleme

Azure 'daki Tanılama ayarları, kaynak günlüklerini toplamak için kullanılır. Azure Kaynak günlükleri bir kaynak tarafından dağıtılır ve bu kaynağın çalışması hakkında zengin ve sık veriler sağlar. Bu Günlükler istek başına yakalanır ve ayrıca "veri düzlemi günlükleri" olarak anırlar. Veri düzlemi işlemlerine bazı örnekler silme, ekleme ve readFeed içerir. Bu günlüklerin içeriği kaynak türüne göre değişir.

Platform ölçümleri ve etkinlik günlükleri otomatik olarak toplanır, ancak kaynak günlüklerini toplamak veya Azure Izleyici dışında iletmek için bir tanılama ayarı oluşturmanız gerekir. Aşağıdaki adımları kullanarak Azure Cosmos hesapları için tanılama ayarını açabilirsiniz:

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Azure Cosmos hesabınıza gidin. **Tanılama ayarları** bölmesini açın ve ardından **Tanılama ayarı Ekle** seçeneğini belirleyin.

1. **Tanılama ayarları** bölmesinde, formu aşağıdaki ayrıntılarla doldurabilirsiniz: 

    * **Ad**: oluşturulacak Günlükler için bir ad girin.

    * **Bir depolama hesabına arşivlemek**, **bir olay hub 'ına akış** yapmak veya **Log Analytics göndermek** için günlükleri kaydedebilirsiniz

1. Bir tanılama ayarı oluşturduğunuzda toplanacak günlüklerin kategorisini belirtirsiniz. Azure Cosmos DB tarafından desteklenen günlüklerin kategorileri aşağıda listelenen örnek günlük ile birlikte aşağıda listelenmiştir:

 * **Dataplanerequests**: arka uç isteklerini SQL, Graph, MongoDB, Cassandra ve Azure Cosmos db tablo API'si hesaplarını Içeren tüm API 'lere kaydetmek için bu seçeneği belirleyin. Notun önemli özellikleri şunlardır `Requestcharge`:, `statusCode`, `clientIPaddress`, ve. `partitionID`

    ```json
    { "time": "2019-04-23T23:12:52.3814846Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "DataPlaneRequests", "operationName": "ReadFeed", "properties": {"activityId": "66a0c647-af38-4b8d-a92a-c48a805d6460","requestResourceType": "Database","requestResourceId": "","collectionRid": "","statusCode": "200","duration": "0","userAgent": "Microsoft.Azure.Documents.Common/2.2.0.0","clientIpAddress": "10.0.0.24","requestCharge": "1.000000","requestLength": "0","responseLength": "372","resourceTokenUserRid": "","region": "East US","partitionId": "062abe3e-de63-4aa5-b9de-4a77119c59f8","keyType": "PrimaryReadOnlyMasterKey","databaseName": "","collectionName": ""}}
    ```

* **Mongorequests**: Ön uçtaki kullanıcı tarafından başlatılan istekleri günlüğe kaydetmek için bu seçeneği belirleyin. Bu seçenek, Istekleri MongoDB IÇIN Azure Cosmos DB API 'sine istemcilere sunar. Bu günlük türü diğer API hesapları için kullanılamaz. Notun önemli özellikleri şunlardır: `Requestcharge`,. `opCode` Tanılama günlüklerinde MongoRequests 'i etkinleştirdiğinizde, DataPlaneRequests 'yi etkinleştirdiğinizden emin olun. API üzerinde yapılan her istek için bir günlük görürsünüz.

    ```json
    { "time": "2019-04-10T15:10:46.7820998Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "MongoRequests", "operationName": "ping", "properties": {"activityId": "823cae64-0000-0000-0000-000000000000","opCode": "MongoOpCode_OP_QUERY","errorCode": "0","duration": "0","requestCharge": "0.000000","databaseName": "admin","collectionName": "$cmd","retryCount": "0"}}
    ```

* **Cassandrarequests**: Kullanıcı tarafından başlatılan Istekleri Cassandra IÇIN Azure Cosmos DB API 'sine istemcilere sunacak şekilde ön uca kaydetmek için bu seçeneği belirleyin. Bu günlük türü diğer API hesapları için kullanılamaz. Aklınızda bulunan anahtar özellikler `operationName`, `requestCharge`,,. `piiCommandText` Tanılama günlüklerinde CassandraRequests 'yi etkinleştirdiğinizde, DataPlaneRequests 'yi etkinleştirdiğinizden emin olun. API üzerinde yapılan her istek için bir günlük görürsünüz.

   ```json
   { "time": "2020-03-30T23:55:10.9579593Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "CassandraRequests", "operationName": "QuerySelect", "properties": {"activityId": "6b33771c-baec-408a-b305-3127c17465b6","opCode": "<empty>","errorCode": "-1","duration": "0.311900","requestCharge": "1.589237","databaseName": "system","collectionName": "local","retryCount": "<empty>","authorizationTokenType": "PrimaryMasterKey","address": "104.42.195.92","piiCommandText": "{"request":"SELECT key from system.local"}","userAgent": """"}}
   ```

* **QueryRuntimeStatistics**: yürütülen sorgu metnini günlüğe kaydetmek için bu seçeneği belirleyin. Bu günlük türü yalnızca SQL API hesapları için kullanılabilir.

    ```json
    { "time": "2019-04-14T19:08:11.6353239Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "QueryRuntimeStatistics", "properties": {"activityId": "278b0661-7452-4df3-b992-8aa0864142cf","databasename": "Tasks","collectionname": "Items","partitionkeyrangeid": "0","querytext": "{"query":"SELECT *\nFROM c\nWHERE (c.p1__10 != true)","parameters":[]}"}}
    ```

* **Partitionkeystatistics**: Bölüm anahtarlarının istatistiklerini günlüğe kaydetmek için bu seçeneği belirleyin. Bu, şu anda Bölüm anahtarlarının depolama boyutu (KB) ile temsil edilir. Bu makalenin [Azure tanılama sorguları ' nı kullanarak sorun giderme sorunları](#diagnostic-queries) bölümüne bakın. Örneğin, "PartitionKeyStatistics" kullanan sorgular. Günlük, çoğu veri depolamasını kaplayan ilk üç bölüm anahtarına göre yayılır. Bu günlük abonelik KIMLIĞI, bölge adı, veritabanı adı, koleksiyon adı, bölüm anahtarı ve depolama boyutu gibi verileri KB olarak içerir.

    ```json
    { "time": "2019-10-11T02:33:24.2018744Z", "resourceId": "/SUBSCRIPTIONS/<your_subscription_ID>/RESOURCEGROUPS/<your_resource_group>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<your_database_account>", "category": "PartitionKeyStatistics", "properties": {"subscriptionId": "<your_subscription_ID>","regionName": "West US 2","databaseName": "KustoQueryResults","collectionname": "CapacityMetrics","partitionkey": "["CapacityMetricsPartition.136"]","sizeKb": "2048270"}}
    ```

* **Partitionkeyrutüketim**: Bu günlük, Bölüm anahtarlarının toplam saniyede ru/sn tüketimini bildirir. Şu anda, yalnızca SQL API hesapları ve nokta okuma/yazma ve saklı yordam işlemleri için bölüm anahtarlarını raporlar Azure Cosmos DB. diğer API 'Ler ve işlem türleri desteklenmez. Diğer API 'Ler için, tanılama günlüğü tablosundaki bölüm anahtarı sütunu boş olur. Bu günlük abonelik KIMLIĞI, bölge adı, veritabanı adı, koleksiyon adı, bölüm anahtarı, işlem türü ve istek ücreti gibi verileri içerir. Bu makalenin [Azure tanılama sorguları ' nı kullanarak sorun giderme sorunları](#diagnostic-queries) bölümüne bakın. Örneğin, "Partitionkeyrutüketim" kullanan sorgular. 

* **Controlplanerequests**: Bu günlük, hesap oluşturma, bölge ekleme veya kaldırma, hesap çoğaltma ayarlarını güncelleştirme gibi denetim düzlemi işlemlerine ilişkin ayrıntıları içerir. Bu günlük türü SQL (Core), MongoDB, Gremlin, Cassandra, Tablo API'si içeren tüm API türlerinde kullanılabilir.

* **İstekler**: Azure Cosmos DB ' dan alınan ölçüm verilerini tanılama ayarındaki hedeflere toplamak için bu seçeneği belirleyin. Bu, Azure ölçümlerinde otomatik olarak toplanan verileri de aynı şekilde toplamıştır. Her iki veri türünü birlikte analiz etmek ve ölçüm verilerini Azure Izleyici dışında göndermek için kaynak günlükleriyle ölçüm verileri toplayın.

Azure portal, CLı veya PowerShell kullanarak bir tanılama ayarı oluşturma hakkında ayrıntılı bilgi için bkz. [Azure makalesinde platform günlüklerini ve ölçümlerini toplamak için tanılama ayarı oluşturma](../azure-monitor/platform/diagnostic-settings.md) .


## <a name="troubleshoot-issues-with-diagnostics-queries"></a><a id="diagnostic-queries"></a>Tanılama sorgularıyla ilgili sorunları giderme

1. Çalıştırmak için 3 milisaniyeden daha uzun süren işlemleri sorgulama:

   ```Kusto
   AzureDiagnostics 
   | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
   | summarize count() by clientIpAddress_s, TimeGenerated
   ```

1. İşlemleri çalıştıran Kullanıcı aracısını sorgulama:

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
   | summarize count() by OperationName, userAgent_s
   ```

1. Uzun süre çalışan işlemler için sorgulama:

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
   | project TimeGenerated , duration_s 
   | summarize count() by bin(TimeGenerated, 5s)
   | render timechart
   ```
    
1. Veritabanı hesabı için ilk 3 bölümde eğriliği değerlendirmek üzere bölüm anahtarı istatistiklerini alma:

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
   | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
   ```

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

1. RU/sn 'nin en çok hangi işlemleri ele aldığını nasıl bulabilirim?

    ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(responseLength_s), max(requestLength_s), max(requestCharge_s), count = count() by OperationName, requestResourceType_s, userAgent_s, collectionRid_s, bin(TimeGenerated, 1h)
   ```

1. Data **Planerequests** ve **QueryRunTimeStatistics**'tan daha fazla alan, 100 ru/sn 'den daha fazla alan tüm sorguları alma.

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 100.0
   | project activityId_g, requestCharge_s
   | join kind= inner (
           AzureDiagnostics
           | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
           | project activityId_g, querytext_s
   ) on $left.activityId_g == $right.activityId_g
   | order by requestCharge_s desc
   | limit 100
   ```

1. Farklı işlemler için dağıtım nasıl alınır?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize count = count()  by OperationName, requestResourceType_s, bin(TimeGenerated, 1h) 
   ```

1. Bir bölümün tükettiği en fazla üretilen iş miktarı nedir?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2h) 
   | summarize max(requestCharge_s) by bin(TimeGenerated, 1h), partitionId_g
   ```

1. Saniye başına bölüm anahtarları RU/sn ile ilgili bilgiler nasıl alınır?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s, TimeGenerated 
   | order by TimeGenerated asc 
   ```

1. Belirli bir bölüm anahtarı için istek ücreti alma

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where parse_json(partitionKey_s)[0] == "2" 
   ```

1. En çok RU/sn ile belirli bir dönemde tüketilen en üst bölüm anahtarları nasıl alınır? 

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider == "MICROSOFT.DOCUMENTDB" and Category == "PartitionKeyRUConsumption" 
   | where TimeGenerated >= datetime("11/26/2019, 11:20:00.000 PM") and TimeGenerated <= datetime("11/26/2019, 11:30:00.000 PM") 
   | summarize total = sum(todouble(requestCharge_s)) by databaseName_s, collectionName_s, partitionKey_s 
   | order by total desc
    ```

1. Depolama boyutu 8 GB 'tan büyük olan bölüm anahtarları için Günlükler nasıl alınır?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics"
   | where todouble(sizeKb_d) > 800000
   ```

1. Veritabanı hesabı için ilk üç bölüm arasındaki eğriliği değerlendirmek için bölüm anahtarı istatistikleri nasıl alınır?

   ```Kusto
   AzureDiagnostics 
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
   | project SubscriptionId, regionName_s, databaseName_s, collectionName_s, partitionKey_s, sizeKb_d, ResourceId
   ```

1. İşlemler, istek ücreti veya yanıtın uzunluğu için P99 veya P50 çoğaltma gecikmeleri nasıl alınır?

   ```Kusto
   AzureDiagnostics
   | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
   | where TimeGenerated >= ago(2d)
   | summarize
   percentile(todouble(responseLength_s), 50), percentile(todouble(responseLength_s), 99), max(responseLength_s),
   percentile(todouble(requestCharge_s), 50), percentile(todouble(requestCharge_s), 99), max(requestCharge_s),
   percentile(todouble(duration_s), 50), percentile(todouble(duration_s), 99), max(duration_s),
   count()
   by OperationName, requestResourceType_s, userAgent_s, collectionRid_s, bin(TimeGenerated, 1h)
   ```
 
1. Controldüzlemi günlükleri nasıl alınır?
 
   [anahtar tabanlı meta veri yazma erişimini devre dışı bırak](audit-control-plane-logs.md#disable-key-based-metadata-write-access) makalesinde açıklandığı şekilde bayrak üzerinde geçiş yapın ve Azure POWERSHELL, CLı veya ARM aracılığıyla işlemleri yürütün.
 
   ```Kusto  
   AzureDiagnostics 
   | where Category =="ControlPlaneRequests"
   | summarize by OperationName 
   ```


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB için Azure Izleyici](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/azure/cosmos-db/toc.json)
* [Azure Cosmos DB ölçümlerle izleme ve hata ayıklama](use-metrics.md)
