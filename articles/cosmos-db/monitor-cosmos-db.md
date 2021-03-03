---
title: İzleyici Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB performansını ve kullanılabilirliğini izlemeyi öğrenin.
author: SnehaGunda
services: cosmos-db
ms.service: cosmos-db
ms.topic: how-to
ms.date: 12/01/2020
ms.author: sngun
ms.custom: subject-monitoring
ms.openlocfilehash: e23318684294a833c59fd5666db8c0a14efde890
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658512"
---
# <a name="monitor-azure-cosmos-db"></a>Azure Cosmos DB'yi izleme
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure kaynaklarına bağlı kritik Uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakları kullanılabilirlik, performans ve işlem için izlemek istersiniz. Bu makalede, Azure Cosmos veritabanları tarafından oluşturulan izleme verileri ve bu verileri çözümlemek ve uyarmak için Azure Izleyici 'nin özelliklerini nasıl kullanabileceğiniz açıklanır.

Verilerinizi, istemci tarafı ve sunucu tarafı ölçümleriyle izleyebilirsiniz. Sunucu tarafı ölçümlerini kullanırken, Azure Cosmos DB ' de depolanan verileri aşağıdaki seçeneklerle izleyebilirsiniz:

* **Azure Cosmos DB portalından izleme:** Azure Cosmos hesabının **ölçümler** sekmesinde bulunan ölçülerle izleyebilirsiniz. Bu sekmedeki ölçümler aktarım hızı, depolama, kullanılabilirlik, gecikme süresi, tutarlılık ve sistem düzeyi ölçümlerini içerir. Varsayılan olarak, bu ölçümler 7 günlük bir bekletme dönemine sahiptir. Daha fazla bilgi edinmek için bu makalenin [Azure Cosmos DB toplanan izleme verileri](#monitoring-from-azure-cosmos-db) bölümüne bakın.

* **Azure izleyici 'de ölçümlerle izleme:** Azure Cosmos hesabınızın ölçümlerini izleyebilir ve Azure Izleyici 'den panolar oluşturabilirsiniz. Azure Izleyici, varsayılan olarak Azure Cosmos DB ölçümleri toplar, açıkça hiçbir şey yapılandırmamanız gerekmez. Bu ölçümler bir dakikalık ayrıntı düzeyi ile toplanır ve ayrıntı düzeyi, seçtiğiniz ölçüme göre farklılık gösterebilir. Varsayılan olarak, bu ölçümler 30 günlük bir bekletme dönemine sahiptir. Önceki seçeneklerden kullanılabilen ölçümlerin çoğu bu ölçümlerde de mevcuttur. Kapsayıcı adı gibi ölçümler için boyut değerleri büyük/küçük harfe duyarlıdır. Bu nedenle, bu boyut değerlerinde dize karşılaştırmaları yaparken büyük/küçük harfe duyarsız karşılaştırma kullanmanız gerekir. Daha fazla bilgi edinmek için bu makalenin [ölçüm verilerini çözümleme](#analyze-metric-data) bölümüne bakın.

* **Azure izleyici 'de tanılama günlükleri Ile izleme:** Azure Cosmos hesabınızın günlüklerini izleyebilir ve Azure Izleyici 'den panolar oluşturabilirsiniz. İkinci bir ayrıntı düzeyinde gerçekleşen olaylar ve izlemeler gibi telemetri Günlükler olarak depolanır. Örneğin, bir kapsayıcının üretilen işi değişirse, Cosmos hesabının özellikleri değiştirilir ve bu olaylar Günlükler içinde yakalar. Toplanan verilerde sorgular çalıştırarak bu günlükleri analiz edebilirsiniz. Daha fazla bilgi edinmek için bu makalenin [günlük verilerini çözümleme](#analyze-log-data) bölümüne bakın.

* **SDK 'ları kullanarak program aracılığıyla izleme:** .NET, Java, Python, Node.js SDK 'Ları ve REST API üst bilgilerini kullanarak Azure Cosmos hesabınızı programlama yoluyla izleyebilirsiniz. Daha fazla bilgi edinmek için bu makaledeki [izleme Azure Cosmos DB Programlama](#monitor-cosmosdb-programmatically) bölümüne bakın.

Aşağıdaki görüntüde Azure Cosmos DB hesabı Azure portal aracılığıyla izlemek için kullanabileceğiniz farklı seçenekler gösterilmektedir:

:::image type="content" source="media/monitor-cosmos-db/monitoring-options-portal.png" alt-text="Azure portal 'de kullanılabilen izleme seçenekleri" border="false":::

Azure Cosmos DB kullanırken, istemci tarafında, oluşabilecek herhangi bir sorunu gidermek için istek ücreti, etkinlik KIMLIĞI, özel durum/yığın izleme bilgileri, HTTP durumu/alt durum kodu, tanılama dizesi ayrıntılarını toplayabilirsiniz. Bu bilgiler, Azure Cosmos DB destek ekibine erişmeniz gerekiyorsa de gereklidir. 

## <a name="monitor-overview"></a>İzlemeye genel bakış

Her Azure Cosmos DB hesabı için Azure portal **genel bakış** sayfası, Toplam İstekler, belırlı bir http durum kodu ile sonuçlanan istekler ve saatlik faturalandırma gibi kaynak kullanımının kısa bir görünümünü içerir. Bu bilgiler faydalı olur, ancak bu bölmeden yalnızca küçük miktarda izleme verisi bulunabilir. Bu verilerden bazıları otomatik olarak toplanır ve kaynağı oluşturur almaz analiz için kullanılabilir. Bazı yapılandırma ile ek veri koleksiyonu türlerini etkinleştirebilirsiniz.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?

Azure Cosmos DB, Azure 'da, diğer bulutlardaki ve Şirket içindeki kaynaklara ek olarak Azure kaynaklarınızı izlemeye yönelik eksiksiz bir özellik kümesi sunan Azure [izleyici](../azure-monitor/overview.md) 'yi kullanarak izleme verileri oluşturur.

Azure hizmetlerini izleme konusunda bilgi sahibi değilseniz, Azure [izleyici Ile Azure kaynaklarını izleme](../azure-monitor/essentials/monitor-azure-resource.md) makalesini aşağıdaki kavramları açıklayan makalede başlatın:

* Azure İzleyici nedir?
* İzleme ile ilişkili maliyetler
* Azure 'da toplanan verileri izleme
* Veri toplamayı yapılandırma
* İzleme verilerini analiz etmek ve uyarı vermek için Azure 'da standart araçlar

Aşağıdaki bölümler, Azure Cosmos DB toplanan belirli verileri açıklayarak ve veri toplamayı yapılandırmaya ve bu verileri Azure araçlarıyla çözümlemeye yönelik örnekler sunarak bu makaleye yöneliktir.

## <a name="azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Izleyici

Azure Cosmos DB için Azure Izleyici, [Azure izleyici 'nin çalışma kitapları özelliğine](../azure-monitor/visualize/workbooks-overview.md) dayalıdır ve aşağıdaki bölümlerde açıklanan Azure Cosmos DB için toplanan izleme verilerini kullanır. Birleşik etkileşimli bir deneyime sahip tüm Azure Cosmos DB kaynaklarınızın genel performans, başarısızlık, kapasite ve işletimsel sistem durumunun bir görünümü için Azure Izleyici 'yi kullanın ve ayrıntılı analiz ve uyarı için Azure Izleyici 'nin diğer özelliklerinden yararlanın. Daha fazla bilgi edinmek için bkz. [Azure Cosmos DB Için Azure Izleyicisini keşfet](../azure-monitor/insights/cosmosdb-insights-overview.md) makalesi.

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

## <a name="monitoring-data"></a><a id="monitoring-from-azure-cosmos-db"></a> İzleme verileri 

Azure Cosmos DB, [Azure kaynaklarından gelen verileri izleme](../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data)bölümünde açıklanan diğer Azure kaynaklarıyla aynı türde izleme verilerini toplar. Azure Cosmos DB tarafından oluşturulan günlüklere ve ölçümlere ilişkin ayrıntılı bir başvuru için bkz. [Azure Cosmos DB izleme verileri başvurusu](monitor-cosmos-db-reference.md) .

Her Azure Cosmos veritabanı için Azure portal **genel bakış** sayfası, isteği ve saatlik faturalandırma kullanımı dahil olmak üzere veritabanı kullanımının kısa bir görünümünü içerir. Bu yararlı bir bilgi olmakla kalmaz, yalnızca küçük miktarda izleme verisi kullanılabilir. Bu verilerden bazıları otomatik olarak toplanır ve analiz için kullanılabilir, ancak bazı yapılandırma ile ek veri toplamayı etkinleştirebilirsiniz.

:::image type="content" source="media/monitor-cosmos-db/overview-page.png" alt-text="Genel bakış sayfası":::

## <a name="collection-and-routing"></a>Toplama ve yönlendirme

Platform ölçümleri ve etkinlik günlüğü otomatik olarak toplanır ve depolanır, ancak bir tanılama ayarı kullanılarak başka konumlara yönlendirilebilir.  

Kaynak günlükleri, bir tanılama ayarı oluşturup bunları bir veya daha fazla konuma yönlendirene kadar toplanmaz ve depolanmaz.

Azure portal ve bazı tanılama sorgu örneklerini kullanarak bir tanılama ayarı oluşturmaya yönelik ayrıntılı süreç için [Azure 'da platform günlüklerini ve ölçümleri toplamak üzere tanılama ayarı oluşturma](cosmosdb-monitor-resource-logs.md) konusuna bakın. Bir tanılama ayarı oluşturduğunuzda hangi günlük kategorilerinin toplanacağını belirlersiniz.

Toplayacağınız ölçümler ve Günlükler aşağıdaki bölümlerde ele alınmıştır.

## <a name="analyzing-metrics"></a><a id="analyze-metric-data"></a> Ölçümler çözümleniyor

Azure Cosmos DB ölçümler ile çalışmak için özel bir deneyim sağlar. **Azure izleyici** menüsünden **ölçümler** ' i açarak Ölçüm Gezgini 'ni kullanarak diğer Azure hizmetlerinden ölçümlerle Azure Cosmos DB için ölçümleri çözümleyebilirsiniz. Bu aracı kullanma hakkında ayrıntılı bilgi için bkz. [Azure Ölçüm Gezgini](../azure-monitor/essentials/metrics-getting-started.md) kullanmaya başlama. Ayrıca, Azure Cosmos DB kaynaklarınız için [sunucu tarafı gecikme süresinin](monitor-server-side-latency.md), [istek birimi kullanımının](monitor-request-unit-usage.md)ve [normalleştirilmiş istek birimi kullanımının](monitor-normalized-request-units.md) nasıl izleneceğini de kullanıma alabilirsiniz.

Azure Cosmos DB için toplanan platform ölçümlerinin bir listesi için bkz. [Azure Cosmos DB veri başvuru ölçümleri izleme](monitor-cosmos-db-reference.md#metrics) makalesi.

Tüm Azure Cosmos DB ölçümleri **Standart ölçümlerde Cosmos DB** ad alanıdır. Bir grafiğe filtre eklerken bu ölçümler ile aşağıdaki boyutları kullanabilirsiniz:

* CollectionName
* DatabaseName
* OperationType
* Region
* Durum

Başvuru için, [Azure izleyici 'de desteklenen tüm kaynak ölçümlerinin](../azure-monitor/essentials/metrics-supported.md)bir listesini görebilirsiniz.

### <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Azure Cosmos DB için işlem düzeyi ölçümlerini görüntüleyin

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Sol taraftaki Gezinti çubuğundan **izleyici** ' yi seçin ve **ölçümler**' i seçin.

   :::image type="content" source="./media/monitor-cosmos-db/monitor-metrics-blade.png" alt-text="Azure Izleyici 'de ölçümler bölmesi":::

1. **Ölçümler** bölmesinden > **bir kaynak seçin** > gerekli **aboneliği** ve **kaynak grubunu** seçin. **Kaynak türü** için **Azure Cosmos DB hesapları**' nı seçin, mevcut Azure Cosmos hesaplarınızdan birini seçin ve **Uygula**' yı seçin.

   :::image type="content" source="./media/monitor-cosmos-db/select-cosmosdb-account.png" alt-text="Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin":::

1. Ardından, kullanılabilir ölçümler listesinden bir ölçüm seçebilirsiniz. Talep birimleri, depolama, gecikme, kullanılabilirlik, Cassandra ve diğer kullanıcılara özgü ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümler](monitor-cosmos-db-reference.md) makalesine bakın. Bu örnekte, **istek birimlerini** ve toplama değeri olarak **Ort** ' i seçelim.

   Bu ayrıntılara ek olarak, ölçümlerin **zaman aralığını** ve **zaman parçalı yapısını** da seçebilirsiniz. En fazla, son 30 güne ait ölçümleri görüntüleyebilirsiniz.  Filtreyi uyguladıktan sonra filtreniz temelinde bir grafik görüntülenir. Seçili dönem için dakika başına tüketilen ortalama istek birimi sayısını görebilirsiniz.  

   :::image type="content" source="./media/monitor-cosmos-db/metric-types.png" alt-text="Azure portal bir ölçüm seçin":::

### <a name="add-filters-to-metrics"></a>Ölçümlere filtre ekleme

Ayrıca ölçümleri ve belirli bir **CollectionName**, **DatabaseName**, **OperationType**, **Region** ve **StatusCode** tarafından görüntülenmiş grafik için filtre uygulayabilirsiniz. Ölçümleri filtrelemek için, **Filtre Ekle** ' yi seçin ve **OperationType** gibi gerekli özelliği seçin ve **sorgu** gibi bir değer seçin. Grafik daha sonra seçili dönem için sorgu işlemi için kullanılan istek birimlerini görüntüler. Saklı yordam aracılığıyla yürütülen işlemler, OperationType ölçümü altında kullanılamayacak şekilde günlüğe kaydedilmez.

:::image type="content" source="./media/monitor-cosmos-db/add-metrics-filter.png" alt-text="Ölçüm ayrıntı düzeyini seçmek için filtre ekleyin":::

**Bölmeyi Uygula** seçeneğini kullanarak ölçümleri gruplandırabilirsiniz. Örneğin, her işlem türü için istek birimlerini gruplandırabilir ve aşağıdaki görüntüde gösterildiği gibi tüm işlemlerin tek seferde grafiğini görüntüleyebilirsiniz:

:::image type="content" source="./media/monitor-cosmos-db/apply-metrics-splitting.png" alt-text="Uygulama bölme filtresi ekle":::

## <a name="analyzing-logs"></a><a id="analyze-log-data"></a> Günlükler çözümleniyor

Azure Izleyici günlüklerindeki veriler, her tablonun kendine ait benzersiz özellikler kümesine sahip olduğu tablolarda depolanır.

Azure Izleyici 'deki tüm kaynak günlüklerine aynı alanlar ve hizmete özgü alanlar gelir. Ortak şema, [Azure izleyici kaynak günlüğü şemasında](../azure-monitor/essentials/resource-logs-schema.md#top-level-common-schema)özetlenmiştir. Azure Cosmos DB için toplanan kaynak günlüklerinin türlerinin listesi için bkz. [izleme Azure Cosmos DB veri başvurusu](monitor-cosmos-db-reference.md#resource-logs).

[Etkinlik günlüğü](../azure-monitor/essentials/activity-log.md) , abonelik düzeyindeki olaylara ilişkin Öngörüler sağlayan bir Azure platformu oturum açma işlemi. Bunu bağımsız olarak görüntüleyebilir veya Azure Izleyici günlüklerine yönlendirebilirsiniz, burada Log Analytics kullanarak çok daha karmaşık sorgular yapabilirsiniz.  

Azure Cosmos DB, verileri aşağıdaki tablolarda depolar.

| Tablo | Açıklama |
|:---|:---|
| AzureDiagnostics | Kaynak günlüklerini depolamak için birden çok hizmet tarafından kullanılan ortak tablo. Azure Cosmos DB kaynak günlükleri ile tanımlanabilir `MICROSOFT.DOCUMENTDB` .   |
| AzureActivity    | Etkinlik günlüğünden tüm kayıtları depolayan ortak tablo.

### <a name="sample-kusto-queries"></a>Örnek kusto sorguları

> [!IMPORTANT]
> Azure Cosmos DB menüsünden **Günlükler** ' i seçtiğinizde, Log Analytics sorgu kapsamı geçerli Azure Cosmos DB hesabına ayarlanmış olarak açılır. Bu, günlük sorgularının yalnızca bu kaynaktaki verileri dahil olacağı anlamına gelir. Diğer hesaplardan veya diğer Azure hizmetlerinden verileri içeren bir sorgu çalıştırmak istiyorsanız, **Azure izleyici** menüsünden **Günlükler** ' i seçin. Ayrıntılar için bkz. [Azure izleyici 'de günlük sorgusu kapsamı ve zaman aralığı Log Analytics](../azure-monitor/logs/scope.md) .

Azure Cosmos kaynaklarınızı izlemenize yardımcı olması için **günlük araması** arama çubuğuna girebileceğiniz bazı sorgular aşağıda verilmiştir. Bu sorgular [Yeni dille](../azure-monitor/logs/log-query-overview.md)çalışır.

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

## <a name="alerts"></a>Uyarılar

İzleme verilerinizde önemli koşullar bulunduğunda Azure Izleyici uyarıları size önceden bildirimde bulunur. Bunlar, müşterilerinizin sorunları fark etmeden önce sisteminizdeki sorunları tanımlamanızı ve ele belirlemenizi sağlar. [Ölçümler](../azure-monitor/alerts/alerts-metric-overview.md), [Günlükler](../azure-monitor/alerts/alerts-unified-log.md)ve [etkinlik günlüğü](../azure-monitor/alerts/activity-log-alerts.md)hakkında uyarı ayarlayabilirsiniz. Farklı uyarı türlerinin avantajları ve dezavantajları vardır

Örneğin, aşağıdaki tabloda kaynaklarınız için birkaç uyarı kuralı listelenmektedir. Azure portal uyarı kurallarının ayrıntılı bir listesini bulabilirsiniz. Daha fazla bilgi için bkz. [Uyarıları yapılandırma](create-alerts.md) makalesi.  

| Uyarı türü | Koşul | Açıklama  |
|:---|:---|:---|
|İstek birimlerindeki hız sınırlaması (ölçüm uyarısı) |Boyut adı: StatusCode, Işleç: Equals, boyut değerleri: 429  | Kapsayıcı veya veritabanı sağlanan aktarım hızı sınırını aşarsa uyarır. |
|Yük devredilen bölge |İşleç: büyüktür, toplama türü: Count, eşik değeri: 1 | Tek bir bölgenin yük devretme durumunda. Otomatik yük devretmeyi etkinleştirmezseniz bu uyarı yararlı olur. |
| Anahtarları Döndür (etkinlik günlüğü uyarısı)| Olay düzeyi: bilgilendirici, durum: başlatıldı| Hesap anahtarları döndürüldüğünde uyarır. Uygulamanızı yeni anahtarlarla güncelleştirebilirsiniz. |

## <a name="monitor-azure-cosmos-db-programmatically"></a><a id="monitor-cosmosdb-programmatically"></a> Program aracılığıyla Azure Cosmos DB izleme

Portalda kullanılabilen hesap düzeyi ölçümleri, hesap depolama kullanımı ve toplam istek gibi SQL API 'Leri aracılığıyla kullanılamaz. Ancak, SQL API 'Lerini kullanarak koleksiyon düzeyinde kullanım verilerini alabilirsiniz. Koleksiyon düzeyindeki verileri almak için aşağıdakileri yapın:

* REST API kullanmak için, [koleksiyonda BIR get işlemi gerçekleştirin](/rest/api/cosmos-db/get-a-collection). Koleksiyonun kota ve kullanım bilgileri, yanıttaki x-MS-Resource-Quota ve x-MS-Resource-Usage üst bilgilerinde döndürülür.

* .NET SDK 'yı kullanmak için, **Collectionsizeusage**, **databaseusage**, **documentusage** gibi birçok kullanım özelliği içeren bir [resourceres,](/dotnet/api/microsoft.azure.documents.client.resourceresponse-1) döndüren [documentclient. readdocumentcollectionasync](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync) yöntemini kullanın.

Ek ölçümlere erişmek için [Azure izleyici SDK 'sını](https://www.nuget.org/packages/Microsoft.Azure.Insights)kullanın. Kullanılabilir Ölçüm tanımları, şu çağırarak alınabilir:

```http
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01
```

Bireysel ölçümleri almak için aşağıdaki biçimi kullanın:

```http
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/providers/microsoft.insights/metrics?timespan={StartTime}/{EndTime}&interval={AggregationInterval}&metricnames={MetricName}&aggregation={AggregationType}&`$filter={Filter}&api-version=2018-01-01
```

Daha fazla bilgi için bkz. [Azure izleme REST API](../azure-monitor/essentials/rest-api-walkthrough.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Cosmos DB tarafından oluşturulan günlüklerin ve ölçümlerin bir başvurusu için bkz. [Azure Cosmos DB izleme verileri başvurusu](monitor-cosmos-db-reference.md) .
* Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../azure-monitor/essentials/monitor-azure-resource.md) .