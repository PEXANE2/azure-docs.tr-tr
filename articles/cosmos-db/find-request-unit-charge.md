---
title: Azure Cosmos DB'de istek birimini (RU) şarj bulma
description: Azure Cosmos kapsayıcısına karşı gerçekleştirilen herhangi bir işlem için istek birimi (RU) ücretini nasıl bulacağınızı öğrenin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: thweiss
ms.openlocfilehash: c5699bb851bd0a818a987228155c62683e93f51a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585909"
---
# <a name="find-the-request-unit-charge-in-azure-cosmos-db"></a>Azure Cosmos DB'de istek birimi ücretini bulma

Bu makalede, Azure Cosmos DB'de bir kapsayıcıya karşı gerçekleştirilen herhangi bir işlem için [istek birimi](request-units.md) (RU) tüketimini bulabileceğiniz farklı yollar sunulur. Şu anda, bu tüketimi yalnızca Azure portalını kullanarak veya Azure Cosmos DB'den SDK'lardan biri aracılığıyla gönderilen yanıtı inceleyerek ölçebilirsiniz.

## <a name="sql-core-api"></a>SQL (Core) API'si

SQL API kullanıyorsanız, bir Azure Cosmos kapsayıcısına karşı bir işlem için RU tüketimini bulmak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda, yalnızca bir SQL sorgusu için Azure portalında istek ücretini bulabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account) ve verileri besleyin veya zaten veri içeren mevcut bir Azure Cosmos hesabını seçin.

1. **Veri Gezgini** bölmesine gidin ve sonra üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. **Yeni SQL Sorgusu**'nu seçin.

1. Geçerli bir sorgu girin ve **sorguyı yürüt'ü**seçin.

1. Yürüttüğün isteğin gerçek istek ücretini görüntülemek için **Sorgu İstatistikleri'ni** seçin.

![Azure portalında bir SQL sorgu isteği ücretinin ekran görüntüsü](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma
### <a name="net-v2-sdk"></a>.Net V2 SDK

[.NET SDK v2'den](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) döndürülen nesneler `RequestCharge` bir özelliği ortaya çıkarır:

```csharp
ResourceResponse<Document> fetchDocumentResponse = await client.ReadDocumentAsync(
    UriFactory.CreateDocumentUri("database", "container", "itemId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
var requestCharge = fetchDocumentResponse.RequestCharge;

StoredProcedureResponse<string> storedProcedureCallResponse = await client.ExecuteStoredProcedureAsync<string>(
    UriFactory.CreateStoredProcedureUri("database", "container", "storedProcedureId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
requestCharge = storedProcedureCallResponse.RequestCharge;

IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri("database", "container"),
    "SELECT * FROM c",
    new FeedOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    }).AsDocumentQuery();
while (query.HasMoreResults)
{
    FeedResponse<dynamic> queryResponse = await query.ExecuteNextAsync<dynamic>();
    requestCharge = queryResponse.RequestCharge;
}
```

### <a name="net-v3-sdk"></a>.Net V3 SDK

[.NET SDK v3'ten](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) döndürülen nesneler `RequestCharge` bir özelliği ortaya çıkarır:

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/CustomDocsSampleCode.cs?name=GetRequestCharge)]

Daha fazla bilgi için [Bkz. Quickstart: Azure Cosmos DB'de bir SQL API hesabı kullanarak bir .NET web uygulaması oluşturun.](create-sql-api-dotnet.md)

### <a name="use-the-java-sdk"></a>Java SDK'yı kullanma

[Java SDK'dan](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) döndürülen nesneler `getRequestCharge()` bir yöntem ortaya çıkarır:

```java
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<ResourceResponse<Document>> readDocumentResponse = client.readDocument(String.format("/dbs/%s/colls/%s/docs/%s", "database", "container", "itemId"), requestOptions);
readDocumentResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

Observable<StoredProcedureResponse> storedProcedureResponse = client.executeStoredProcedure(String.format("/dbs/%s/colls/%s/sprocs/%s", "database", "container", "storedProcedureId"), requestOptions, null);
storedProcedureResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

FeedOptions feedOptions = new FeedOptions();
feedOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<FeedResponse<Document>> feedResponse = client
    .queryDocuments(String.format("/dbs/%s/colls/%s", "database", "container"), "SELECT * FROM c", feedOptions);
feedResponse.forEach(result -> {
    double requestCharge = result.getRequestCharge();
});
```

Daha fazla bilgi için [Bkz. Quickstart: Azure Cosmos DB SQL API hesabı kullanarak bir Java uygulaması oluşturun.](create-sql-api-java.md)

### <a name="use-the-nodejs-sdk"></a>Düğüm.js SDK kullanın

[Node.js SDK'dan](https://www.npmjs.com/package/@azure/cosmos) döndürülen nesneler, `headers` temel http API tarafından döndürülen tüm başlıkları eşleyen bir alt nesne ortaya çıkarır. İstek ücreti `x-ms-request-charge` anahtar altında mevcuttur:

```javascript
const item = await client
    .database('database')
    .container('container')
    .item('itemId', 'partitionKey')
    .read();
var requestCharge = item.headers['x-ms-request-charge'];

const storedProcedureResult = await client
    .database('database')
    .container('container')
    .storedProcedure('storedProcedureId')
    .execute({
        partitionKey: 'partitionKey'
    });
requestCharge = storedProcedureResult.headers['x-ms-request-charge'];

const query = client.database('database')
    .container('container')
    .items
    .query('SELECT * FROM c', {
        partitionKey: 'partitionKey'
    });
while (query.hasMoreResults()) {
    var result = await query.executeNext();
    requestCharge = result.headers['x-ms-request-charge'];
}
```

Daha fazla bilgi için [Bkz. Quickstart: Azure Cosmos DB SQL API hesabı kullanarak bir Düğüm.js uygulaması oluşturun.](create-sql-api-nodejs.md) 

### <a name="use-the-python-sdk"></a>Python SDK'yı kullan

Python `CosmosClient` [SDK'daki](https://pypi.org/project/azure-cosmos/) nesne, `last_response_headers` yürütülen son işlem için temel HTTP API tarafından döndürülen tüm başlıkları eşleyen bir sözlük ortaya çıkarır. İstek ücreti `x-ms-request-charge` anahtar altında mevcuttur:

```python
response = client.ReadItem(
    'dbs/database/colls/container/docs/itemId', {'partitionKey': 'partitionKey'})
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure(
    'dbs/database/colls/container/sprocs/storedProcedureId', None, {'partitionKey': 'partitionKey'})
request_charge = client.last_response_headers['x-ms-request-charge']
```

Daha fazla bilgi için [Bkz. Quickstart: Azure Cosmos DB SQL API hesabı kullanarak bir Python uygulaması oluşturun.](create-sql-api-python.md) 

## <a name="azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API

RU charge, özel bir veritabanı `getLastRequestStatistics` [komutu](https://docs.mongodb.com/manual/reference/command/) tarafından ortaya çıkarır. Komut, yürütülen son işlemin adını, istek ücretini ve süresini içeren bir belge döndürür. MongoDB için Azure Cosmos DB API kullanıyorsanız, RU ücretini almak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda, yalnızca bir sorgu için Azure portalında istek ücretini bulabilirsiniz.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-mongodb-dotnet.md#create-a-database-account) ve verilerle besleyin veya zaten veri içeren varolan bir hesabı seçin.

1. **Veri Gezgini** bölmesine gidin ve sonra üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. **Yeni Sorgu**'yu seçin.

1. Geçerli bir sorgu girin ve **sorguyı yürüt'ü**seçin.

1. Yürüttüğün isteğin gerçek istek ücretini görüntülemek için **Sorgu İstatistikleri'ni** seçin.

![Azure portalında MongoDB sorgu isteği ücretinin ekran görüntüsü](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>MongoDB .NET sürücüsünü kullanın

[Resmi MongoDB .NET sürücüsünü](https://docs.mongodb.com/ecosystem/drivers/csharp/)kullandığınızda, bir `RunCommand` `IMongoDatabase` nesne üzerinde yöntemi çağırarak komutları çalıştırabilirsiniz. Bu yöntem `Command<>` soyut sınıfın uygulanmasını gerektirir:

```csharp
class GetLastRequestStatisticsCommand : Command<Dictionary<string, object>>
{
    public override RenderedCommand<Dictionary<string, object>> Render(IBsonSerializerRegistry serializerRegistry)
    {
        return new RenderedCommand<Dictionary<string, object>>(new BsonDocument("getLastRequestStatistics", 1), serializerRegistry.GetSerializer<Dictionary<string, object>>());
    }
}

Dictionary<string, object> stats = database.RunCommand(new GetLastRequestStatisticsCommand());
double requestCharge = (double)stats["RequestCharge"];
```

Daha fazla bilgi için [Quickstart: MongoDB için Azure Cosmos DB API kullanarak bir .NET web uygulaması oluşturun.](create-mongodb-dotnet.md)

### <a name="use-the-mongodb-java-driver"></a>MongoDB Java sürücüsünü kullanma


[Resmi MongoDB Java sürücüsünü](https://mongodb.github.io/mongo-java-driver/)kullandığınızda, `runCommand` bir `MongoDatabase` nesne üzerinde yöntemi arayarak komutları çalıştırabilirsiniz:

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

Daha fazla bilgi için [Quickstart: MongoDB ve Java SDK için Azure Cosmos DB API'sini kullanarak bir web uygulaması oluşturun.](create-mongodb-java.md)

### <a name="use-the-mongodb-nodejs-driver"></a>MongoDB Düğümü.js sürücüsünü kullanın

[Resmi MongoDB Node.js sürücüsünü](https://mongodb.github.io/node-mongodb-native/)kullandığınızda, bir `command` `db` nesne üzerinde yöntemi arayarak komutları çalıştırabilirsiniz:

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

Daha fazla bilgi için [Bkz. Quickstart: Mevcut bir MongoDB Node.js web uygulamasını Azure Cosmos DB'ye geçirin.](create-mongodb-nodejs.md)

## <a name="cassandra-api"></a>Cassandra API’si

Azure Cosmos DB Cassandra API'ye karşı işlemler gerçekleştirdiğinizde, RU ücreti gelen yükte bir alan olarak döndürülür. `RequestCharge` RU ücretini almak için çeşitli seçenekleriniz vardır.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

[.NET SDK'yı](https://www.nuget.org/packages/CassandraCSharpDriver/)kullandığınızda, gelen yükü bir `Info` `RowSet` nesnenin özelliği altından alabilirsiniz:

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"].Reverse().ToArray(), 0);
```

Daha fazla bilgi için [,.NET SDK ve Azure Cosmos DB'yi kullanarak Bir Cassandra uygulaması oluşturun.](create-cassandra-dotnet.md)

### <a name="use-the-java-sdk"></a>Java SDK'yı kullanma

[Java SDK'yı](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core)kullandığınızda, bir `getExecutionInfo()` `ResultSet` nesne üzerinde yöntemi arayarak gelen yükü alabilirsiniz:

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

Daha fazla bilgi için [Bkz. Quickstart: Java SDK ve Azure Cosmos DB'yi kullanarak bir Cassandra uygulaması oluşturun.](create-cassandra-java.md)

## <a name="gremlin-api"></a>Gremlin API

Gremlin API'sini kullandığınızda, Bir Azure Cosmos kapsayıcısına karşı yapılan bir işlem için RU tüketimini bulmak için birden çok seçeneğiniz vardır. 

### <a name="use-drivers-and-sdk"></a>Sürücüleri ve SDK'yı kullanma

Gremlin API tarafından döndürülen başlıklar, şu anda Gremlin .NET ve Java SDK tarafından su yüzüne çıkan özel durum özelliklerine eşlenir. İstek ücreti anahtarın `x-ms-request-charge` altında mevcuttur.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

[SDK Gremlin.NET](https://www.nuget.org/packages/Gremlin.Net/)kullandığınızda, durum öznitelikleri `StatusAttributes` `ResultSet<>` nesnenin özelliği altında kullanılabilir:

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

Daha fazla bilgi için [Bkz. Quickstart: Bir Azure Cosmos DB Gremlin API hesabı kullanarak bir .NET Framework veya Core uygulaması oluşturun.](create-graph-dotnet.md)

### <a name="use-the-java-sdk"></a>Java SDK'yı kullanma

[Gremlin Java SDK'yı](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver)kullandığınızda, nesne üzerindeki `statusAttributes()` yöntemi arayarak durum özniteliklerini `ResultSet` alabilirsiniz:

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

Daha fazla bilgi için [Bkz. Quickstart: Java SDK'yı kullanarak Azure Cosmos DB'de bir grafik veritabanı oluşturun.](create-graph-java.md)

## <a name="table-api"></a>Tablo API’si

Şu anda, tablo işlemleri için RU ücretini döndüren tek SDK [.NET Standart SDK'dır.](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) Nesne, `TableResult` Azure `RequestCharge` Cosmos DB Tablo API'sine karşı kullandığınızda SDK tarafından doldurulan bir özelliği ortaya çıkarır:

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

Daha fazla bilgi için [,.NET SDK ve Azure Cosmos DB'yi kullanarak Hızlı Başlangıç: Tablo API uygulaması oluşturun.](create-table-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

RU tüketiminizi en iyi duruma getirmek hakkında bilgi edinmek için şu makalelere bakın:

* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
* [Azure Cosmos DB’de sağlanan işlem hızını iyileştirme](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu maliyetini iyileştirme](optimize-cost-queries.md)
* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Bir konteyner için sağlama iş](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki ölçümlerle izleme ve hata ayıklama](use-metrics.md)
