---
title: Azure Cosmos DB'yi Izleme | Microsoft Dokümanlar
description: Azure Cosmos DB'nin performansını ve kullanılabilirliğini nasıl izleyeceğinizi öğrenin.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: b9b66c379714c2f4fa2421876fda3bdb500ce6c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250350"
---
# <a name="monitoring-azure-cosmos-db"></a>Azure Cosmos DB'nin İzlenmesi
Azure kaynaklarına dayanan kritik uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakların kullanılabilirliği, performansı ve işleyişi için izlemek istersiniz. Bu makalede, Azure Cosmos veritabanları tarafından oluşturulan izleme verileri ve bu verileri analiz etmek ve uyarmak için Azure Monitor'un özelliklerini nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
Azure Cosmos DB, Azure'da tam bir yığın izleme hizmeti olan [Azure Monitor'u](../azure-monitor/overview.md) kullanarak izleme verileri oluşturur ve diğer bulutlardaki ve şirket içi kaynakların yanı sıra Azure kaynaklarınızı izlemek için eksiksiz bir özellik kümesi sağlar. 

Azure hizmetlerini izlemeyi zaten bilmiyorsanız, [Azure Monitor ile Azure kaynaklarını izleme](../azure-monitor/insights/monitor-azure-resource.md) makalesine başlayın: Aşağıdakileri açıklar:

- Azure İzleyici nedir?
- İzleme ile ilgili maliyetler
- Azure'da toplanan verileri izleme
- Veri toplamayı yapılandırma
- Verileri analiz etmek ve uyarmak için Azure'da standart araçlar

Aşağıdaki bölümler, Azure Cosmos DB'den toplanan belirli verileri tanımlayarak ve veri toplamayı yapılandırmak ve bu verileri Azure araçlarıyla çözümleme için örnekler sağlayarak bu makaleye dayanmaktadır.

## <a name="azure-monitor-for-cosmos-db-preview"></a>Cosmos DB için Azure Monitörü (Önizleme)
[Azure Cosmos DB için Azure Monitor,](../azure-monitor/insights/cosmosdb-insights-overview.md) [Azure Monitor'un çalışma kitapları özelliğine](../azure-monitor/app/usage-workbooks.md) dayanır ve aşağıdaki bölümlerde açıklanan Cosmos DB için toplanan aynı izleme verilerini kullanır. Bu aracı, tüm Azure Cosmos DB kaynaklarınızın genel performansını, hatalarını, kapasitesini ve operasyonel durumunu birleşik etkileşimli bir deneyimde görüntülemek için kullanın ve ayrıntılı analiz ve uyarı için Azure Monitor'un diğer özelliklerinden yararlanın. 

![Cosmos DB için Azure Monitör](media/monitor-cosmos-db/azure-monitor-cosmos-db.png)

## <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Azure Cosmos DB için işlem düzeyi ölçümlerini görüntüleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Sol daki gezinme çubuğundan **Monitör'ü** seçin ve **Ölçümler'i**seçin.

   ![Azure Monitör'de Ölçümler bölmesi](./media/monitor-cosmos-db/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > Gerekli **aboneliği**ve **kaynak grubunu**seçmek > bir **kaynak** seçin. Kaynak **türü**için **Azure Cosmos DB hesaplarını**seçin, varolan Azure Cosmos hesaplarınızdan birini seçin ve **Uygula'yı**seçin.

   ![Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin](./media/monitor-cosmos-db/select-cosmosdb-account.png)

1. Ardından, kullanılabilir ölçümler listesinden bir metrik seçebilirsiniz. İstek birimleri, depolama, gecikme sonu, kullanılabilirlik, Cassandra ve diğerlerine özgü ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek [için, kategori](monitor-cosmos-db-reference.md) makalesine göre Ölçümler'e bakın. Bu örnekte, toplama değeri olarak **İstek birimlerini** ve **Avg'yi** seçelim.

   Bu ayrıntılara ek olarak, ölçümlerin **Zaman aralığı** ve **Zaman parçalı lığını** da seçebilirsiniz. Maksimum olarak, son 30 gün için ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra, filtrenize göre bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portalından bir metrik seçin](./media/monitor-cosmos-db/metric-types.png)

### <a name="add-filters-to-metrics"></a>Ölçümlere filtre ekleme

Ayrıca ölçümleri ve belirli bir **CollectionName,** **DatabaseName,** **OperationType**, **Bölge**ve **StatusCode**tarafından görüntülenen grafiğe de filtre uygulayabilirsiniz. Ölçümlere filtre uygulayın **Add filter** ve **OperationType** gibi gerekli özelliği seçin ve **Sorgu**gibi bir değer seçin. Grafik daha sonra, seçili dönem için sorgu işlemi için tüketilen istek birimlerini görüntüler. Saklı yordam ı ile yürütülen işlemler günlüğe kaydedilmez, bu nedenle OperationType ölçümü altında kullanılamaz.

![Metrik parçalılığı seçmek için filtre ekleme](./media/monitor-cosmos-db/add-metrics-filter.png)

**Uygula bölme** seçeneğini kullanarak ölçümleri gruplatabilirsiniz. Örneğin, istek birimlerini işlem türüne göre gruplayabilir ve aşağıdaki resimde gösterildiği gibi tüm işlemler için grafiği aynı anda görüntüleyebilirsiniz:

![Uygula bölme filtresi ekle](./media/monitor-cosmos-db/apply-metrics-splitting.png)

Belirli bir veritabanı, kapsayıcı veya işlem için sunucu tarafındaki gecikme ölçümlerini görüntülemek için başka bir örnek aşağıda verilmiştir:

![Sunucu tarafı gecikme ölçümleri](./media/monitor-cosmos-db/serverside-latency-metric.png)

## <a name="monitoring-data-collected-from-azure-cosmos-db"></a>Azure Cosmos DB'den toplanan verileri izleme

Azure Cosmos DB, [Azure kaynaklarından gelen verileri izlemede](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data)açıklanan diğer Azure kaynaklarıyla aynı izleme veritürlerini toplar. Azure Cosmos DB tarafından oluşturulan günlükler ve ölçümlerin ayrıntılı bir başvurusu için [Azure Cosmos DB izleme veri başvurusuna](monitor-cosmos-db-reference.md) bakın.

Her Azure Cosmos veritabanı için Azure portalındaki **Genel Bakış** sayfası, isteği ve saatlik faturalandırma kullanımı da dahil olmak üzere veritabanı kullanımının kısa bir görünümünü içerir. Bu yararlı bilgiler, ancak mevcut izleme verilerinin yalnızca küçük bir miktarıdır. Bu verilerin bir kısmı otomatik olarak toplanır ve veritabanını oluşturur oluşturmaz analiz edilebilebilir ve bazı yapılandırmalarla ek veri toplamayı etkinleştirebilirsiniz.

![Genel bakış sayfası](media/monitor-cosmos-db/overview-page.png)

## <a name="analyzing-metric-data"></a>Metrik verileri çözümleme

Azure Cosmos DB, ölçümlerle çalışmak için özel bir deneyim sağlar. Bu deneyimi kullanma ve farklı Azure Cosmos DB senaryolarını analiz etme hakkında ayrıntılı bilgi için [Azure Monitor'dan Azure Cosmos DB ölçümlerini izleyin](cosmos-db-azure-monitor-metrics.md) ve hata ayıklama bölümüne bakın.

**Azure Monitörü** menüsünden **Ölçümler'i** açarak Metrics explorer'ı kullanarak diğer Azure hizmetlerinden ölçümlerle Azure Cosmos DB ölçümlerini analiz edebilirsiniz. Bu aracı kullanma yla ilgili ayrıntılar için [Azure Ölçümleri Gezgini ile başlarken](../azure-monitor/platform/metrics-getting-started.md) bkz. Azure Cosmos DB için tüm ölçümler ad alanı **Cosmos DB standart ölçümlerindedir.** Grafiğe filtre eklerken aşağıdaki ölçüleri aşağıdaki ölçümlerle kullanabilirsiniz:

- CollectionName
- DatabaseName
- Operationtype
- Bölge
- Statuscode


## <a name="analyzing-log-data"></a>Günlük verilerini çözümleme
Azure Monitör Günlükleri'ndeki veriler, her tablonun kendine özgü özelliklere sahip olduğu tablolarda depolanır. Azure Cosmos DB verileri aşağıdaki tablolarda depolar.

| Tablo | Açıklama |
|:---|:---|
| AzureDiagnostics | Kaynak günlüklerini depolamak için birden çok hizmet tarafından kullanılan ortak tablo. Azure Cosmos DB'deki kaynak günlükleri `MICROSOFT.DOCUMENTDB`.   |
| AzureActivity    | Etkinlik günlüğündeki tüm kayıtları depolayan ortak tablo. 


> [!IMPORTANT]
> Azure Cosmos DB menüsünden **Günlükler'i** seçtiğinizde, Geçerli Azure Cosmos veritabanına ayarlanan sorgu kapsamıyla Log Analytics açılır. Bu, günlük sorgularının yalnızca bu kaynaktan gelen verileri içereceği anlamına gelir. Diğer veritabanlarından veya diğer Azure hizmetlerinden gelen verileri içeren bir sorgu çalıştırmak istiyorsanız, **Azure Monitörü** menüsünden **Günlükler'i** seçin. Ayrıntılar [için Azure Monitor Log Analytics'te Günlük sorgu kapsamı ve zaman aralığına](../azure-monitor/log-query/scope.md) bakın.

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Monitor'da Azure Cosmos DB Günlük Analizi sorguları

Azure Cosmos kapsayıcılarınızı izlemenize yardımcı olmak için **Günlük arama** çubuğuna girebileceğiniz bazı sorgular aşağıda verebilirsiniz. Bu sorgular [yeni dil](../log-analytics/log-analytics-log-search-upgrade.md)ile çalışır.

Azure Cosmos veritabanlarınızı izlemenize yardımcı olmak için kullanabileceğiniz sorgular aşağıda veda eder.

* Azure Cosmos DB'deki tüm tanılama günlüklerini belirli bir süre için sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"

    ```

* En son günlüğe kaydedilmiş 10 olayı sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* İşlem türüne göre gruplanan tüm işlemleri sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* Kaynağa göre gruplanan tüm işlemleri sorgulamak için:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* Kaynağa göre gruplanan tüm kullanıcı etkinliği için sorgu yapmak için:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
* 100'den büyük tüm sorguları almak için **DataPlaneRequests** ve **QueryRunTimeStatistics**verileri ile katıldı.

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

* Hangi işlemlerin 3 milisaniyeden uzun sürdüğünü sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* İşlemleri hangi aracının çalıştırdığısorgulanacak:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* Uzun süren işlemlerin ne zaman gerçekleştirildiyi sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | summarize count() by bin(TimeGenerated, 5s)
    | render timechart
    ```
    
* Veritabanı hesabı için en iyi 3 bölüm arasında çarpık değerlendirmek için Bölüm Anahtar istatistikleri almak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>Azure Cosmos DB'yi programlı olarak izleyin
Portalda bulunan hesap depolama kullanımı ve toplam istekler gibi hesap düzeyi ölçümleri SQL API'leri aracılığıyla kullanılamaz. Ancak, SQL API'lerini kullanarak toplama düzeyinde kullanım verilerini alabilirsiniz. Toplama düzeyi verilerini almak için aşağıdakileri yapın:

* REST API'yi kullanmak için [koleksiyonda GET gerçekleştirin.](https://msdn.microsoft.com/library/mt489073.aspx) Koleksiyoniçin kota ve kullanım bilgileri yanıtolarak x-ms-kaynak kotası ve x-ms-kaynak kullanım üstbilgileri döndürülür.
* .NET SDK'yı kullanmak için, **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**ve daha fazlası gibi bir dizi kullanım özelliği içeren bir [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) döndüren [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) yöntemini kullanın.

Ek ölçümlere erişmek için [Azure MonitörSi SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Insights)kullanın. Kullanılabilir metrik tanımlar arayarak alınabilir:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Tek tek ölçümleri almak için yapılan sorgular aşağıdaki biçimi kullanır:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB tarafından oluşturulan günlükler ve ölçümlere başvurmak için [Azure Cosmos DB izleme veri başvurusuna](monitor-cosmos-db-reference.md) bakın.
- Azure kaynaklarını izleme yle ilgili ayrıntılar için [Azure Kaynaklarını Azure Monitörü ile izleme](../azure-monitor/insights/monitor-azure-resource.md) bilgisine bakın.
