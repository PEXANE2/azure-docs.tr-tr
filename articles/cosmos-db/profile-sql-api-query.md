---
title: SQL sorgu performansı & yürütme ölçümleri alın
description: Azure Cosmos DB isteklerinin SQL sorgu yürütme ölçümlerini ve profil SQL sorgu performansını nasıl alabildiğini öğrenin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: girobins
ms.openlocfilehash: 48b9a67de5c870a187ee008bd97265760ca6c341
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70998376"
---
# <a name="get-sql-query-execution-metrics-and-analyze-query-performance-using-net-sdk"></a>.NET SDK kullanarak SQL sorgu yürütme ölçümlerini alın ve sorgu performansını analiz edin

Bu makalede, Azure Cosmos DB'de SQL sorgu performansının nasıl profilini nasil açıklanır. Bu profil oluşturma .NET SDK'dan alınan lar kullanılarak `QueryMetrics` yapılabilir ve burada ayrıntılı olarak açıklanır. [QueryMetrics,](https://msdn.microsoft.com/library/microsoft.azure.documents.querymetrics.aspx) arka uç sorgusu yürütmesi hakkında bilgi içeren güçlü bir şekilde yazılan bir nesnedir. Bu [ölçümler, Tune Query Performance](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) makalesinde daha ayrıntılı olarak belgelenmiştir.

## <a name="set-the-feedoptions-parameter"></a>FeedOptions parametresini ayarlama

[DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentquery.aspx) için tüm aşırı yüklemeler isteğe bağlı [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) parametresini alır. Bu seçenek, sorgu yürütmenin ayarlanmasını ve parametrenize getirilmesini sağlar. 

Sql sorgu yürütme ölçümlerini toplamak [için, FeedOptions'daki](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) [PopulateQueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.populatequerymetrics.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.PopulateQueryMetrics) `true`parametresini . Doğru `PopulateQueryMetrics` ayar, ilgili `QueryMetrics`yi `FeedResponse` içerecek şekilde bunu sağlayacaktır. 

## <a name="get-query-metrics-with-asdocumentquery"></a>AsDocumentQuery ile sorgu ölçümlerini alın()
Aşağıdaki kod örneği, [AsDocumentQuery()](https://msdn.microsoft.com/library/microsoft.azure.documents.linq.documentqueryable.asdocumentquery.aspx) yöntemini kullanırken alma ölçümlerinin nasıl yapılacağını gösterir:

```csharp
// Initialize this DocumentClient and Collection
DocumentClient documentClient = null;
DocumentCollection collection = null;

// Setting PopulateQueryMetrics to true in the FeedOptions
FeedOptions feedOptions = new FeedOptions
{
    PopulateQueryMetrics = true
};

string query = "SELECT TOP 5 * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    
    // At this point you have QueryMetrics which you can serialize using .ToString()
    foreach (KeyValuePair<string, QueryMetrics> kvp in partitionIdToQueryMetrics)
    {
        string partitionId = kvp.Key;
        QueryMetrics queryMetrics = kvp.Value;
        
        // Do whatever logging you need
        DoSomeLoggingOfQueryMetrics(query, partitionId, queryMetrics);
    }
}
```
## <a name="aggregating-querymetrics"></a>QueryMetrics toplama

Önceki bölümde, [ExecuteNextAsync](https://msdn.microsoft.com/library/azure/dn850294.aspx) yöntemine birden çok çağrı olduğunu unutmayın. Her çağrı, `FeedResponse` sözlük olan bir `QueryMetrics`nesneyi döndürür; sorgunun her devamı için bir. Aşağıdaki örnek, linq `QueryMetrics` kullanarak bunların nasıl toplanmış olduğunu gösterir:

```csharp
List<QueryMetrics> queryMetricsList = new List<QueryMetrics>();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    queryMetricsList.AddRange(partitionIdToQueryMetrics.Values);
}

// Aggregate the QueryMetrics using the + operator overload of the QueryMetrics class.
QueryMetrics aggregatedQueryMetrics = queryMetricsList.Aggregate((curr, acc) => curr + acc);
Console.WriteLine(aggregatedQueryMetrics);
```

## <a name="grouping-query-metrics-by-partition-id"></a>Sorgu ölçümlerini Bölüm Kimliğine göre gruplandırma

Bölüm Kimliği'ne göre gruplayabilirsin. `QueryMetrics` Partition ID'ye göre gruplandırma, belirli bir Bölümün diğerleriyle karşılaştırıldığında performans sorunlarına neden olup olmadığını görmenizi sağlar. Aşağıdaki örnek, LINQ `QueryMetrics` ile nasıl gruplaşılaşyapılacağını gösterir:

```csharp
List<KeyValuePair<string, QueryMetrics>> partitionedQueryMetrics = new List<KeyValuePair<string, QueryMetrics>>();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary is maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    partitionedQueryMetrics.AddRange(partitionIdToQueryMetrics.ToList());
}

// Now we are able to group the query metrics by partitionId
IEnumerable<IGrouping<string, KeyValuePair<string, QueryMetrics>>> groupedByQueryMetrics = partitionedQueryMetrics
    .GroupBy(kvp => kvp.Key);

// If we wanted to we could even aggregate the groupedby QueryMetrics
foreach(IGrouping<string, KeyValuePair<string, QueryMetrics>> grouping in groupedByQueryMetrics)
{
    string partitionId = grouping.Key;
    QueryMetrics aggregatedQueryMetricsForPartition = grouping
        .Select(kvp => kvp.Value)
        .Aggregate((curr, acc) => curr + acc);
    DoSomeLoggingOfQueryMetrics(query, partitionId, aggregatedQueryMetricsForPartition);
}
```

## <a name="linq-on-documentquery"></a>DocumentQuery üzerinde LINQ

Ayrıca `AsDocumentQuery()` yöntemi kullanarak `FeedResponse` bir LINQ Sorgusu'ndan da alabilirsiniz:

```csharp
IDocumentQuery<Document> linqQuery = client.CreateDocumentQuery(collection.SelfLink, feedOptions)
    .Take(1)
    .Where(document => document.Id == "42")
    .OrderBy(document => document.Timestamp)
    .AsDocumentQuery();
FeedResponse<Document> feedResponse = await linqQuery.ExecuteNextAsync<Document>();
IReadOnlyDictionary<string, QueryMetrics> queryMetrics = feedResponse.QueryMetrics;
```

## <a name="expensive-queries"></a>Pahalı Sorgular

Pahalı sorguları veya yüksek iş elde etme tüketen sorguları araştırmak için her sorgu tarafından tüketilen istek birimlerini yakalayabilirsiniz. [RequestCharge](https://msdn.microsoft.com/library/azure/dn948712.aspx) özelliğini kullanarak istek ücretini `FeedResponse`alabilirsiniz. Azure portalını ve farklı SDK'ları kullanarak istek ücretini nasıl alacağınız hakkında daha fazla bilgi edinmek için, istek birimi ücret lendirme [makalesini bulun.](find-request-unit-charge.md)

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    double requestCharge = feedResponse.RequestCharge
    
    // Log the RequestCharge how ever you want.
    DoSomeLogging(requestCharge);
}
```

## <a name="get-the-query-execution-time"></a>Sorgu yürütme süresini alma

İstemci tarafı sorgusunu yürütmek için gereken süreyi hesaplarken, kod tabanınızın diğer bölümlerini değil, `ExecuteNextAsync` yalnızca yöntemi arama süresini dahil ettiğinizden emin olun. Yalnızca bu çağrılar, sorgu yürütmesinin aşağıdaki örnekte gösterildiği gibi ne kadar sürdüğünü hesaplamanıza yardımcı olur:

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();
Stopwatch queryExecutionTimeEndToEndTotal = new Stopwatch();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    queryExecutionTimeEndToEndTotal.Start();
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    queryExecutionTimeEndToEndTotal.Stop();
}

// Log the elapsed time
DoSomeLogging(queryExecutionTimeEndToEndTotal.Elapsed);
```

## <a name="scan-queries-commonly-slow-and-expensive"></a>Sorguları tarayın (genellikle yavaş ve pahalı)

Tbmks sorgusu, sonuç kümesini döndürmeden önce birçok belgenin yüklendiği dizin tarafından sunulmayan bir sorguyu ifade eder.

Aşağıda bir tazyik sorgusu örneği verilmiştir:

```sql
SELECT VALUE c.description 
FROM   c 
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Bu sorgunun filtresi, dizinden servis edilemeyecek olan ÜST sistem işlevini kullanır. Bu sorguyu büyük bir koleksiyona karşı yürütmek, ilk devamı için aşağıdaki sorgu ölçümlerini üretti:

```
QueryMetrics

Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Sorgu ölçümleri çıktısından aşağıdaki değerleri not edin:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
```

Bu sorgu, toplam 399.998.938 bayt 60.951 belge yükledi. Bu kadar bayt yüklemek yüksek maliyet veya istek birim ücreti ile sonuçlanır. Ayrıca, harcanan toplam süre yle açık olan sorguyu yürütmek de uzun zaman alır:

```
Total Query Execution Time               :        4,500.34 milliseconds
```

Yani sorgunun yürütülmesi 4,5 saniye sürer (ve bu yalnızca bir devamtı).

Bu örnek sorguyu en iyi duruma getirmek için filtrede UPPER kullanımından kaçının. Bunun yerine, belgeler oluşturulduğunda `c.description` veya güncelleştirildiğinde, değerler tüm büyük harflerin karakterlerine eklenmelidir. Sorgu daha sonra olur: 

```sql
SELECT VALUE c.description 
FROM   c 
WHERE c.description = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Bu sorgu artık dizinden sunulabilir.

Sorgu performansını ayarlama hakkında daha fazla bilgi edinmek için [Sorgu Performansı Ayarını Dinle](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) makalesine bakın.

## <a name="references"></a><a id="References"></a>Başvurular

- [Azure Cosmos DB SQL belirtimi](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [Json](https://json.org/)
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu performansını ayarlama](sql-api-query-metrics.md)
- [Dizin oluşturma genel bakış](index-overview.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
