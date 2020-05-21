---
title: İzleme Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB performansını ve kullanılabilirliğini izlemeyi öğrenin.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: fcdc80e73d11bbc6d94e921548d346ee436b69ac
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715690"
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

## <a name="azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Izleyici

Azure Cosmos DB için Azure Izleyici, [Azure izleyici 'nin çalışma kitapları özelliğine](../azure-monitor/app/usage-workbooks.md) dayalıdır ve aşağıdaki bölümlerde açıklanan Cosmos DB için toplanan izleme verilerini kullanır. Birleşik etkileşimli bir deneyime sahip tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işletimsel sistem durumunun bir görünümü için Azure Izleyici 'yi kullanın ve ayrıntılı analiz ve uyarı için Azure Izleyici 'nin diğer özelliklerinden yararlanın. Daha fazla bilgi edinmek için bkz. [Azure Cosmos DB Için Azure Izleyicisini keşfet](../azure-monitor/insights/cosmosdb-insights-overview.md) makalesi.

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

## <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Azure Cosmos DB için işlem düzeyi ölçümlerini görüntüleyin

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   ![Azure Izleyici 'de ölçümler bölmesi](./media/monitor-cosmos-db/monitor-metrics-blade.png)

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği**ve **kaynak grubunu**seçin. **Kaynak türü**için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.

   ![Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin](./media/monitor-cosmos-db/select-cosmosdb-account.png)

1. Ardından, kullanılabilir ölçümler listesinden bir ölçüm seçebilirsiniz. Talep birimleri, depolama, gecikme, kullanılabilirlik, Cassandra ve diğer kullanıcılara özgü ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte, **istek birimlerini** ve toplama değeri olarak **Ort** ' i seçelim.

   Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   ![Azure portal bir ölçüm seçin](./media/monitor-cosmos-db/metric-types.png)

### <a name="add-filters-to-metrics"></a>Ölçümlere filtre ekleme

Ayrıca ölçümleri ve belirli bir **CollectionName**, **DatabaseName**, **OperationType**, **Region**ve **StatusCode**tarafından görüntülenmiş grafik için filtre uygulayabilirsiniz. Ölçümleri filtrelemek için, **Filtre Ekle** ' yi seçin ve **OperationType** gibi gerekli özelliği seçin ve **sorgu**gibi bir değer seçin. Grafik daha sonra seçili dönem için sorgu işlemi için kullanılan istek birimlerini görüntüler. Saklı yordam aracılığıyla yürütülen işlemler, OperationType ölçümü altında kullanılamayacak şekilde günlüğe kaydedilmez.

![Ölçüm ayrıntı düzeyini seçmek için filtre ekleyin](./media/monitor-cosmos-db/add-metrics-filter.png)

**Bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz. Örneğin, her işlem türü için istek birimlerini gruplandırabilir ve aşağıdaki görüntüde gösterildiği gibi tüm işlemlerin tek seferde grafiğini görüntüleyebilirsiniz:

![Uygulama bölme filtresi ekle](./media/monitor-cosmos-db/apply-metrics-splitting.png)

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
- Durum

## <a name="analyzing-log-data"></a>Günlük verileri çözümleniyor
Azure Izleyici günlüklerindeki veriler, her tablonun kendine ait benzersiz özellikler kümesine sahip olduğu tablolarda depolanır. Azure Cosmos DB, verileri aşağıdaki tablolarda depolar.

| Tablo | Açıklama |
|:---|:---|
| AzureDiagnostics | Kaynak günlüklerini depolamak için birden çok hizmet tarafından kullanılan ortak tablo. Azure Cosmos DB kaynak günlükleri ile tanımlanabilir `MICROSOFT.DOCUMENTDB` .   |
| AzureActivity    | Etkinlik günlüğünden tüm kayıtları depolayan ortak tablo. 


> [!IMPORTANT]
> Azure Cosmos DB menüsünden **Günlükler** ' i seçtiğinizde, Log Analytics geçerli Azure Cosmos veritabanı olarak ayarlanan sorgu kapsamıyla açılır. Bu, günlük sorgularının yalnızca bu kaynaktaki verileri dahil olacağı anlamına gelir. Diğer veritabanlarından veya diğer Azure hizmetlerinden verileri içeren bir sorgu çalıştırmak istiyorsanız, **Azure izleyici** menüsünden **Günlükler** ' i seçin. Ayrıntılar için bkz. [Azure izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics](../azure-monitor/log-query/scope.md) .

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics sorguları Azure Cosmos DB

Azure Cosmos Kapsayıcılarınızı izlemenize yardımcı olması için **günlük araması** arama çubuğuna girebileceğiniz bazı sorgular aşağıda verilmiştir. Bu sorgular [Yeni dille](../log-analytics/log-analytics-log-search-upgrade.md)çalışır.

Azure Cosmos veritabanlarınızı izlemenize yardımcı olması için kullanabileceğiniz sorgular aşağıda verilmiştir.

* Belirli bir süre için Azure Cosmos DB tüm tanılama günlüklerini sorgulamak için:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests"

    ```

* Kaynağa göre gruplandırılan tüm işlemleri sorgulamak için:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* Kaynağa göre gruplandırılan tüm kullanıcı etkinliklerini sorgulamak için:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>Program aracılığıyla Azure Cosmos DB izleme

Portalda kullanılabilen hesap düzeyi ölçümleri, hesap depolama kullanımı ve toplam istek gibi SQL API 'Leri aracılığıyla kullanılamaz. Ancak, SQL API 'Lerini kullanarak koleksiyon düzeyinde kullanım verilerini alabilirsiniz. Koleksiyon düzeyindeki verileri almak için aşağıdakileri yapın:

* REST API kullanmak için, [koleksiyonda BIR get işlemi gerçekleştirin](https://msdn.microsoft.com/library/mt489073.aspx). Koleksiyonun kota ve kullanım bilgileri, yanıttaki x-MS-Resource-Quota ve x-MS-Resource-Usage üst bilgilerinde döndürülür.

* .NET SDK 'yı kullanmak için, **Collectionsizeusage**, **databaseusage**, **documentusage**gibi birçok kullanım özelliği içeren bir [resourceres,](https://msdn.microsoft.com/library/dn799209.aspx) döndüren [documentclient. readdocumentcollectionasync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) yöntemini kullanın.

Ek ölçümlere erişmek için [Azure izleyici SDK 'sını](https://www.nuget.org/packages/Microsoft.Azure.Insights)kullanın. Kullanılabilir Ölçüm tanımları, şu çağırarak alınabilir:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Tek tek ölçümleri almak için sorgular aşağıdaki biçimi kullanır:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>Sonraki adımlar

- Azure Cosmos DB tarafından oluşturulan günlüklerin ve ölçümlerin bir başvurusu için bkz. [Azure Cosmos DB izleme verileri başvurusu](monitor-cosmos-db-reference.md) .
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../azure-monitor/insights/monitor-azure-resource.md) .
