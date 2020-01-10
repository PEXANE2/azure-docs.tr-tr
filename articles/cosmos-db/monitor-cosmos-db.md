---
title: İzleme Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB performansını ve kullanılabilirliğini izlemeyi öğrenin.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: c166811bbfd27691f9a01a944d304d06560b0232
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445189"
---
# <a name="monitoring-azure-cosmos-db"></a>İzleme Azure Cosmos DB
Azure kaynaklarına bağlı kritik Uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakları kullanılabilirlik, performans ve işlem için izlemek istersiniz. Bu makalede, Azure Cosmos veritabanları tarafından oluşturulan izleme verileri ve bu verileri çözümlemek ve uyarmak için Azure Izleyici 'nin özelliklerini nasıl kullanabileceğiniz açıklanır.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
Azure Cosmos DB, Azure 'da, diğer bulutlardaki ve Şirket içindeki kaynaklara ek olarak Azure kaynaklarınızı izlemeye yönelik eksiksiz bir özellik kümesi sunan Azure [izleyici](../azure-monitor/overview.md) 'yi kullanarak izleme verileri oluşturur. 

Azure hizmetlerini izleme konusunda bilginiz yoksa, aşağıdakileri açıklayan Azure [izleyici Ile Azure kaynaklarını izleme](../azure-monitor/insights/monitor-azure-resource.md) makalesindeki makaleye başlayın:

- Azure İzleyici nedir?
- İzleme ile ilişkili maliyetler
- Azure 'da toplanan verileri izleme
- Veri toplamayı yapılandırma
- İzleme verilerini analiz etmek ve uyarı vermek için Azure 'da standart araçlar

Aşağıdaki bölümler, Azure Cosmos DB toplanan belirli verileri açıklayarak ve veri toplamayı yapılandırmaya ve bu verileri Azure araçlarıyla çözümlemeye yönelik örnekler sunarak bu makaleye yöneliktir.

## <a name="azure-monitor-for-cosmos-db-preview"></a>Cosmos DB için Azure Izleyici (Önizleme)
[Azure Cosmos DB Için Azure izleyici](../azure-monitor/insights/cosmosdb-insights-overview.md) , [Azure izleyici 'nin çalışma kitapları özelliğine](../azure-monitor/app/usage-workbooks.md) dayalıdır ve aşağıdaki bölümlerde açıklanan Cosmos DB için toplanan izleme verilerini kullanır. Birleşik etkileşimli bir deneyime sahip tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işletimsel sistem durumunun bir görünümü için bu aracı kullanın ve ayrıntılı analiz ve uyarı için Azure Izleyici 'nin diğer özelliklerinden yararlanın. 

![Cosmos DB için Azure Izleyici](media/monitor-cosmos-db/azure-monitor-cosmos-db.png)

## <a name="monitoring-data-collected-from-azure-cosmos-db"></a>Azure Cosmos DB toplanan verileri izleme

Azure Cosmos DB, [Azure kaynaklarından gelen verileri izleme](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data)bölümünde açıklanan diğer Azure kaynaklarıyla aynı türde izleme verilerini toplar. Azure Cosmos DB tarafından oluşturulan günlüklere ve ölçümlere ilişkin ayrıntılı bir başvuru için bkz. [Azure Cosmos DB izleme verileri başvurusu](monitor-cosmos-db-reference.md) .

Her Azure Cosmos veritabanı için Azure portal **genel bakış** sayfası, isteği ve saatlik faturalandırma kullanımı dahil olmak üzere veritabanı kullanımının kısa bir görünümünü içerir. Bu yararlı bir bilgi olmakla kalmaz, yalnızca küçük miktarda izleme verisi kullanılabilir. Bu verilerden bazıları otomatik olarak toplanır ve analiz için kullanılabilir, ancak bazı yapılandırma ile ek veri toplamayı etkinleştirebilirsiniz.

![Genel Bakış sayfası](media/monitor-cosmos-db/overview-page.png)

## <a name="analyzing-metric-data"></a>Ölçüm verileri çözümleniyor

Azure Cosmos DB ölçümler ile çalışmak için özel bir deneyim sağlar. Bu deneyimi kullanmayla ilgili ayrıntılar ve farklı Azure Cosmos DB senaryoları çözümlemek için bkz. [Azure izleyici 'de Azure Cosmos DB ölçümleri izleme ve hata ayıklama](cosmos-db-azure-monitor-metrics.md) .

**Azure izleyici** menüsünden **ölçümler** ' i açarak Ölçüm Gezgini 'ni kullanarak diğer Azure hizmetlerinden ölçümlerle Azure Cosmos DB için ölçümleri çözümleyebilirsiniz. Bu aracı kullanma hakkında ayrıntılı bilgi için bkz. [Azure Ölçüm Gezgini](../azure-monitor/platform/metrics-getting-started.md) kullanmaya başlama. Tüm Azure Cosmos DB ölçümleri **Standart ölçümlerde Cosmos DB**ad alanıdır. Bir grafiğe filtre eklerken bu ölçümler ile aşağıdaki boyutları kullanabilirsiniz:

- CollectionName
- DatabaseName
- OperationType
- Bölge
- StatusCode


## <a name="analyzing-log-data"></a>Günlük verileri çözümleniyor
Azure Izleyici günlüklerindeki veriler, her tablonun kendine ait benzersiz özellikler kümesine sahip olduğu tablolarda depolanır. Azure Cosmos DB, verileri aşağıdaki tablolarda depolar.

| Tablo | Açıklama |
|:---|:---|
| AzureDiagnostics | Kaynak günlüklerini depolamak için birden çok hizmet tarafından kullanılan ortak tablo. Azure Cosmos DB kaynak günlükleri `MICROSOFT.DOCUMENTDB`ile tanımlanabilir.   |
| AzureActivity    | Etkinlik günlüğünden tüm kayıtları depolayan ortak tablo. 


> [!IMPORTANT]
> Azure Cosmos DB menüsünden **Günlükler** ' i seçtiğinizde, Log Analytics geçerli Azure Cosmos veritabanı olarak ayarlanan sorgu kapsamıyla açılır. Bu, günlük sorgularının yalnızca bu kaynaktaki verileri dahil olacağı anlamına gelir. Diğer veritabanlarından veya diğer Azure hizmetlerinden verileri içeren bir sorgu çalıştırmak istiyorsanız, **Azure izleyici** menüsünden **Günlükler** ' i seçin. Ayrıntılar için bkz. [Azure izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics](../azure-monitor/log-query/scope.md) .

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics sorguları Azure Cosmos DB

Azure Cosmos Kapsayıcılarınızı izlemenize yardımcı olması için **günlük araması** arama çubuğuna girebileceğiniz bazı sorgular aşağıda verilmiştir. Bu sorguları çalışmak [yeni dil](../log-analytics/log-analytics-log-search-upgrade.md).

Azure Cosmos veritabanlarınızı izlemenize yardımcı olması için kullanabileceğiniz sorgular aşağıda verilmiştir.

* Belirtilen bir süre için tüm Azure Cosmos DB'den tanılama günlükleri sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"

    ```

* 10 en son sorgulamak için olayları günlüğe kaydedilir:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* İşlem türüne göre gruplandırılan tüm işlemler için sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* Kaynağa göre gruplandırılan tüm işlemleri sorgulamak için:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* Kaynağa göre gruplandırılmış tüm kullanıcı etkinliklerini sorgulamak için:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
* Data **Planerequests** ve **QueryRunTimeStatistics**'ten alınan 100 ru 'dan büyük tüm sorguları almak için.

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

* Sorgu işlemlerde 3 milisaniyeden uzun almak için:

    ```Kusto
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* İşlemler için hangi aracıyı çalıştıran sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* Uzun süre çalışan işlemleri gerçekleştirildiğinde sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | summarize count() by bin(TimeGenerated, 5s)
    | render timechart
    ```
    
* Veritabanı hesabı için ilk 3 bölümde eğriliği değerlendirmek üzere bölüm anahtarı istatistiklerini almak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB program aracılığıyla izleyin
Hesap düzeyindeki ölçümleri hesap depolama kullanım ve toplam istekleri gibi Portalı'nda SQL API'leri üzerinden kullanılabilir değil. Ancak, SQL API'leri kullanarak koleksiyon düzeyinde kullanım verileri alabilir. Koleksiyon düzeyi verileri almak için aşağıdakileri yapın:

* REST API'sini kullanmayı [koleksiyonunda bir GET gerçekleştirmek](https://msdn.microsoft.com/library/mt489073.aspx). Koleksiyon kotası ve kullanım bilgileri yanıtındaki x-ms-resource-quota ve x-ms-resource-kullanım üstbilgileri döndürülür.
* .NET SDK'yı kullanmak için [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) döndüren yöntemi bir [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) gibi çeşitli kullanım özellikleri içeren  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**ve daha fazlası.

Ek ölçümlere erişmek için kullanmanız [Azure İzleyici SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Insights). Kullanılabilir ölçüm tanımlarını çağrılarak alınabilir:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Tek tek ölçümleri almak için sorgu aşağıdaki biçimi kullanın:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB tarafından oluşturulan günlüklerin ve ölçümlerin bir başvurusu için bkz. [Azure Cosmos DB izleme verileri başvurusu](monitor-cosmos-db-reference.md) .
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../azure-monitor/insights/monitor-azure-resource.md) .
