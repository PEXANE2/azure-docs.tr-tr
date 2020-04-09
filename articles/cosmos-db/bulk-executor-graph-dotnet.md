---
title: Azure Cosmos DB Gremlin API ile grafik toplu uygulayıcısı .NET kitaplığını kullanma
description: Grafik verilerini azure cosmos DB Gremlin API kapsayıcısına büyük ölçüde almak için toplu yürütme kitaplığını nasıl kullanacağınızı öğrenin.
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: lbosq
ms.reviewer: sngun
ms.openlocfilehash: adf512fc521ef553f0bbd6ef6dd8ee19e398b37b
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80982712"
---
# <a name="using-the-graph-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB Gremlin API'de toplu işlemler gerçekleştirmek için grafik toplu uygulayıcısı .NET kitaplığını kullanma

Bu öğretici, grafik nesnelerini bir Azure Cosmos DB Gremlin API kapsayıcısına almak ve güncelleştirmek için Azure CosmosDB'nin toplu uygulayıcısı .NET kitaplığını kullanma yla ilgili yönergeler sağlar. Bu işlem, vertex ve Edge nesnelerini programlı bir şekilde oluşturmak ve ağ isteği başına birden fazla eklemek için [toplu yürütme kitaplığındaki](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-overview) Grafik sınıfını kullanır. Bu davranış, hem veritabanı hem de yerel bellek kaynaklarını en iyi şekilde kullanmak için toplu yürütme kitaplığı aracılığıyla yapılandırılabilir.

Gremlin sorgularını komutun değerlendirilip teker teker yürütüldüğü bir veritabanına göndermek yerine, toplu yürütme kitaplığını kullanmak yerine nesneleri yerel olarak oluşturmayı ve doğrulamayı gerektirir. Kitaplık, nesneleri oluşturduktan sonra grafik nesnelerini veritabanı hizmetine sıralı bir şekilde göndermenizi sağlar. Bu yöntemle veri alımı hızları 100 kata kadar artırılabilir ve bu da ilk veri geçişi veya düzenli veri taşıma işlemleri için ideal bir yöntem olmasını sağlar. [Azure Cosmos DB Graph toplu yürütme veya örnek uygulamasının](https://github.com/Azure-Samples/azure-cosmosdb-graph-bulkexecutor-dotnet-getting-started)GitHub sayfasını ziyaret ederek daha fazla bilgi edinin.

## <a name="bulk-operations-with-graph-data"></a>Grafik verileriyle toplu işlemler

[Toplu yürütücü kitaplığı,](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph?view=azure-dotnet) grafik nesneleri oluşturmak ve almak için işlevsellik sağlamak için bir `Microsoft.Azure.CosmosDB.BulkExecutor.Graph` ad alanı içerir. 

Aşağıdaki işlem, veri geçişinin bir Gremlin API kapsayıcısı için nasıl kullanılabileceğini göstermektedir:
1. Kayıtları veri kaynağından alın.
2. Alınan kayıtlardan `GremlinVertex` ve `GremlinEdge` nesnesini oluşturun ve bunları bir `IEnumerable` veri yapısına ekleyin. Uygulamanın bu bölümünde veri kaynağının grafik veritabanı olmaması halinde ilişki algılama ve ekleme mantığının uygulanması gerekir.
3. Grafik nesnelerini koleksiyona eklemek için [Grafik BulkImportAsync metodunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph.graphbulkexecutor.bulkimportasync?view=azure-dotnet) kullanın.

Bu mekanizma, Gremlin istemcisi kullanmaya kıyasla veri geçişinin verimliliğini artırır. Bu geliştirme, verilerin Gremlin ile eklenmesi için uygulamanın doğrulanması, değerlendirilmesi ve ardından veri oluşturmak için yürütülmesi gereken sorguları tek tek gönderilmesi gerektiğindendir. Toplu yürütücü kitaplığı, uygulamadaki doğrulamayı işler ve her ağ isteği için aynı anda birden çok grafik nesnesi gönderir.

### <a name="creating-vertices-and-edges"></a>Köşe ve kenar oluşturma

`GraphBulkExecutor`, `Microsoft.Azure.CosmosDB.BulkExecutor.Graph.Element` ad alanında tanımlı `GremlinVertex` veya `GremlinEdge` nesnelerinin `IEnumerable` listesini gerektiren `BulkImportAsync` metodunu sunar. Örnekte kenarları ve köşeleri iki BulkExecutor içeri aktarma görevine ayırdık. Aşağıdaki örneğe bakın:

```csharp

IBulkExecutor graphbulkExecutor = new GraphBulkExecutor(documentClient, targetCollection);

BulkImportResponse vResponse = null;
BulkImportResponse eResponse = null;

try
{
    // Import a list of GremlinVertex objects
    vResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateVertices(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token);

    // Import a list of GremlinEdge objects
    eResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateEdges(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token);
}
catch (DocumentClientException de)
{
    Trace.TraceError("Document client exception: {0}", de);
}
catch (Exception e)
{
    Trace.TraceError("Exception: {0}", e);
}
```

Toplu yürütme kitaplığı parametreleri hakkında daha fazla bilgi için [Azure Cosmos DB konusuna Toplu Alma Verileri'ne](bulk-executor-dot-net.md#bulk-import-data-to-an-azure-cosmos-account)bakın.

Yükün `GremlinVertex` ve `GremlinEdge` nesnelerinde oluşturulması gerekir. Bu nesneler şu şekilde oluşturulabilir:

**Köşeler**:
```csharp
// Creating a vertex
GremlinVertex v = new GremlinVertex(
    "vertexId",
    "vertexLabel");

// Adding custom properties to the vertex
v.AddProperty("customProperty", "value");

// Partitioning keys must be specified for all vertices
v.AddProperty("partitioningKey", "value");
```

**Kenarlar**:
```csharp
// Creating an edge
GremlinEdge e = new GremlinEdge(
    "edgeId",
    "edgeLabel",
    "targetVertexId",
    "sourceVertexId",
    "targetVertexLabel",
    "sourceVertexLabel",
    "targetVertexPartitioningKey",
    "sourceVertexPartitioningKey");

// Adding custom properties to the edge
e.AddProperty("customProperty", "value");
```

> [!NOTE]
> Toplu yürütme yardımcı programı, Kenarlar eklemeden önce varolan Vertices'leri otomatik olarak denetlemez. Bu durumun BulkImport görevlerini çalıştırmadan önce uygulamada doğrulanması gerekir.

## <a name="sample-application"></a>Örnek uygulama

### <a name="prerequisites"></a>Ön koşullar
* Azure geliştirme iş yüküyle Visual Studio 2019. [Visual Studio 2019 Community Edition'a](https://visualstudio.microsoft.com/downloads/) ücretsiz olarak ulaşabilirsiniz.
* Azure aboneliği. [Buradan ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cosmos-db) oluşturabilirsiniz. Alternatif olarak, Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz](https://azure.microsoft.com/try/cosmosdb/) olarak deneyin ile bir Cosmos veritabanı hesabı oluşturabilirsiniz.
* **Sınırsız koleksiyona** sahip Azure Cosmos DB Gremlin API veritabanı. Bu kılavuz, [.NET ile Azure Cosmos DB Gremlin API'yi](https://docs.microsoft.com/azure/cosmos-db/create-graph-dotnet) kullanmaya başlamayı göstermektedir.
* Git. Daha fazla bilgi için [Git indirme sayfasına](https://git-scm.com/downloads) bakın.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama
Bu öğreticide, GitHub'da barındırılan Azure [Cosmos DB Graph toplu yürütme örneğini](https://github.com/Azure-Samples/azure-cosmosdb-graph-bulkexecutor-dotnet-getting-started) kullanarak başlamak için gereken adımları izleyeceğiz. Bu uygulama, rastgele köşe ve kenar nesneleri oluşturup belirtilen grafik veritabanı hesabına toplu ekleme yapan bir .NET çözümü içerir. Uygulamayı almak için aşağıdaki `git clone` komutunu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/azure-cosmosdb-graph-bulkexecutor-dotnet-getting-started.git
```

Bu depo, aşağıdaki dosyalara sahip olan GraphBulkExecutor örneğini içerir:

Dosya|Açıklama
---|---
`App.config`|Uygulama ve veritabanına özgü parametreler burada belirtilir. Hedef veritabanına ve koleksiyonlara bağlanmak için bu dosyanın değiştirilmesi gerekir.
`Program.cs`| Bu dosya, `DocumentClient` toplama oluşturma, temizleme işleme ve toplu yürütme istekleri gönderme arkasındaki mantığı içerir.
`Util.cs`| Bu dosya, test verileri oluşturmaya ek olarak veritabanı ve koleksiyonların var olup olmadığını kontrol eden mantığı içeren yardımcı sınıfı içerir.

`App.config` dosyasında aşağıdaki yapılandırma değerleri sağlanabilir:

Ayar|Açıklama
---|---
`EndPointUrl`|Bu, Azure Cosmos DB Gremlin API veritabanı hesabınızın Genel Bakış dikey penceresinde bulunan **.NET SDK uç noktanızdır**. `https://your-graph-database-account.documents.azure.com:443/` biçimine sahiptir
`AuthorizationKey`|Bu, Azure Cosmos DB hesabınızda listelenen Birincil veya İkincil anahtardır. [Azure Cosmos DB verilerine güvenli erişim sağlama](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#master-keys) hakkında daha fazla bilgi edinin
`DatabaseName`, `CollectionName`|Bunlar **hedef veritabanı ve koleksiyon adlarıdır**. `ShouldCleanupOnStart`, `true` olarak ayarlandığında bu değerler `CollectionThroughput` ile birlikte bunları bırakmaya ek olarak yeni bir veritabanı ve koleksiyon oluşturmak için kullanılır. Benzer şekilde `ShouldCleanupOnFinish`, `true` olarak ayarlandığında alma işlemi sona erdiğinde veritabanını silmek için kullanılır. Hedef koleksiyonun **sınırsız koleksiyon** olması gerektiğini unutmayın.
`CollectionThroughput`|Bu, `ShouldCleanupOnStart` seçeneği `true` olarak ayarlandığında yeni koleksiyon oluşturmak için kullanılır.
`ShouldCleanupOnStart`|Bu işlem program çalıştırılmadan önce veritabanı hesabını ve koleksiyonlarını bırakır ve ardından `DatabaseName`, `CollectionName` ve `CollectionThroughput` değerleriyle yenilerini oluşturur.
`ShouldCleanupOnFinish`|Bu değer program çalıştırıldıktan sonra belirtilen `DatabaseName` ve `CollectionName` değerleriyle veritabanı hesabını ve koleksiyonlarını bırakır.
`NumberOfDocumentsToImport`|Bu değer örnekte oluşturulacak test köşesi ve kenarı sayısını belirler. Bu sayı hem köşeler hem de kenarlar için geçerlidir.
`NumberOfBatches`|Bu değer örnekte oluşturulacak test köşesi ve kenarı sayısını belirler. Bu sayı hem köşeler hem de kenarlar için geçerlidir.
`CollectionPartitionKey`|Bu değer bu özelliğin otomatik olarak atanacağı test köşelerini ve kenarlarını oluşturmak için kullanılır. Bu değer aynı zamanda `ShouldCleanupOnStart` seçeneği `true` olarak ayarlandığında veritabanını ve koleksiyonları yeniden oluşturmak için kullanılır.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

1. Belirli veritabanı yapılandırma parametrelerinizi `App.config` içine ekleyin. Bu değer bir DocumentClient örneği oluşturmak için kullanılır. Veritabanı ve kapsayıcı henüz oluşturulmadıysa otomatik olarak oluşturulur.
2. Uygulamayı çalıştırın. Biri Köşeleri, biri Kenarları içeri aktarmak için olmak üzere `BulkImportAsync` iki kez çağrılır. Ekleme sırasında hata oluşturan nesneler `.\BadVertices.txt` veya `.\BadEdges.txt` içine eklenir.
3. Grafik veritabanını sorgulayarak sonuçları değerlendirin. `ShouldCleanupOnFinish` seçeneği true olarak ayarlanırsa veritabanı otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar
* Nuget paket ayrıntıları ve toplu uygulayıcı .NET kitaplığı sürüm notları hakkında bilgi edinmek için [toplu uygulayıcı SDK ayrıntılarına](sql-api-sdk-bulk-executor-dot-net.md)bakın. 
* Toplu uygulayıcının kullanımını daha da optimize etmek için [Performans İpuçlarına](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-dot-net#performance-tips) göz atın.
* Bu ad alanında tanımlanan sınıflar ve metotlar hakkında ayrıntılı bilgi için [BulkExecutor.Graph Başvurusu makalesini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph?view=azure-dotnet) gözden geçirin.
