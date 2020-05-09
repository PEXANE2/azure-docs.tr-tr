---
title: İstek birimi (RU) ücreti Azure Cosmos DB bulun
description: Azure Cosmos kapsayıcısına karşı yürütülen herhangi bir işlem için istek birimi (RU) ücreti bulmayı öğrenin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: thweiss
ms.openlocfilehash: e5420b9b765fffcf7b4ccd6775d05795b1b13871
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872235"
---
# <a name="find-the-request-unit-charge-in-azure-cosmos-db"></a>İstek Birim ücreti Azure Cosmos DB bulun

Bu makalede, Azure Cosmos DB bir kapsayıcıya karşı yürütülen herhangi bir işlem için [istek birimi](request-units.md) (ru) tüketimini bulmanın farklı yolları sunulmaktadır. Şu anda, bu tüketimi yalnızca Azure portal kullanarak veya SDK 'lardan biri aracılığıyla Azure Cosmos DB geri gönderilen yanıtı inceleyerek ölçebilirsiniz.

## <a name="sql-core-api"></a>SQL (Core) API'si

SQL API 'sini kullanıyorsanız, bir Azure Cosmos kapsayıcısına karşı bir işlemin RU tüketimini bulmak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda, yalnızca bir SQL sorgusu için Azure portal istek ücreti bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account) ve verilerle birlikte akışı yapın veya zaten veri içeren mevcut bir Azure Cosmos hesabı seçin.

1. **Veri Gezgini** bölmesine gidin ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. **Yeni SQL Sorgusu**'nu seçin.

1. Geçerli bir sorgu girin ve sonra **sorguyu Yürüt**' ü seçin.

1. Yürüttüğünüz istek için gerçek istek ücreti göstermek üzere **sorgu istatistikleri** ' ni seçin.

![Azure portal bir SQL sorgu isteği ücreti ekran görüntüsü](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

[.NET SDK v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) 'den döndürülen nesneler bir `RequestCharge` özelliği kullanıma sunar:

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

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[.NET SDK V3](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) 'den döndürülen nesneler bir `RequestCharge` özelliği kullanıma sunar:

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/CustomDocsSampleCode.cs?name=GetRequestCharge)]

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Cosmos DB ' de BIR SQL API hesabı kullanarak .NET Web uygulaması oluşturma](create-sql-api-dotnet.md).

---

### <a name="use-the-java-sdk"></a>Java SDK 'sını kullanma

[Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) 'dan döndürülen nesneler bir `getRequestCharge()` yöntemi kullanıma sunar:

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

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Cosmos db SQL API hesabı kullanarak Java uygulaması oluşturma](create-sql-api-java.md).

### <a name="use-the-nodejs-sdk"></a>Node. js SDK 'sını kullanma

[Node. js SDK](https://www.npmjs.com/package/@azure/cosmos) 'dan döndürülen nesneler, temeldeki HTTP API 'si `headers` tarafından döndürülen tüm üst bilgileri eşleyen bir alt nesne sunar. İstek ücreti şu `x-ms-request-charge` anahtar altında kullanılabilir:

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

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Cosmos db SQL API hesabı kullanarak Node. js uygulaması oluşturma](create-sql-api-nodejs.md). 

### <a name="use-the-python-sdk"></a>Python SDK'yı kullanma

`CosmosClient` [Python SDK](https://pypi.org/project/azure-cosmos/) 'daki nesne, yürütülen son işlem `last_response_headers` için temeldeki HTTP API 'si tarafından döndürülen tüm üstbilgileri eşleyen bir sözlük sunar. İstek ücreti şu `x-ms-request-charge` anahtar altında kullanılabilir:

```python
response = client.ReadItem(
    'dbs/database/colls/container/docs/itemId', {'partitionKey': 'partitionKey'})
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure(
    'dbs/database/colls/container/sprocs/storedProcedureId', None, {'partitionKey': 'partitionKey'})
request_charge = client.last_response_headers['x-ms-request-charge']
```

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Cosmos db SQL API hesabı kullanarak bir Python uygulaması oluşturma](create-sql-api-python.md). 

## <a name="azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API

RU ücreti adlı `getLastRequestStatistics`özel bir [veritabanı komutu](https://docs.mongodb.com/manual/reference/command/) tarafından sunulur. Komutu, yürütülen son işlemin adını, istek ücretine ve süresini içeren bir belge döndürür. MongoDB için Azure Cosmos DB API kullanıyorsanız, RU ücreti almak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda yalnızca bir sorgu için Azure portal istek ücreti bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-mongodb-dotnet.md#create-a-database-account) ve verilerle birlikte akışı yapın veya zaten veri içeren mevcut bir hesabı seçin.

1. **Veri Gezgini** bölmesine gidin ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. **Yeni Sorgu**'yu seçin.

1. Geçerli bir sorgu girin ve sonra **sorguyu Yürüt**' ü seçin.

1. Yürüttüğünüz istek için gerçek istek ücreti göstermek üzere **sorgu istatistikleri** ' ni seçin.

![Azure portal MongoDB sorgu isteği ücreti ekran görüntüsü](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>MongoDB .NET sürücüsünü kullanma

[Resmi MongoDB .net sürücüsünü](https://docs.mongodb.com/ecosystem/drivers/csharp/)kullandığınızda, `RunCommand` yöntemini bir `IMongoDatabase` nesne üzerinde çağırarak komutları çalıştırabilirsiniz. Bu yöntem, `Command<>` soyut sınıfın bir uygulamasını gerektirir:

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

Daha fazla bilgi için bkz. [hızlı başlangıç: MongoDB için Azure Cosmos DB API kullanarak .NET Web uygulaması oluşturma](create-mongodb-dotnet.md).

### <a name="use-the-mongodb-java-driver"></a>MongoDB Java sürücüsünü kullanma


[Resmi MongoDB Java sürücüsünü](https://mongodb.github.io/mongo-java-driver/)kullandığınızda, `runCommand` metodu bir `MongoDatabase` nesne üzerinde çağırarak komutları çalıştırabilirsiniz:

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

Daha fazla bilgi için bkz. [hızlı başlangıç: MongoDB için Azure Cosmos DB API ve Java SDK 'sı kullanarak Web uygulaması oluşturma](create-mongodb-java.md).

### <a name="use-the-mongodb-nodejs-driver"></a>MongoDB Node. js sürücüsünü kullanma

[Resmi MongoDB Node. js sürücüsünü](https://mongodb.github.io/node-mongodb-native/)kullandığınızda, `command` metodu bir `db` nesne üzerinde çağırarak komutları çalıştırabilirsiniz:

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

Daha fazla bilgi için bkz. [hızlı başlangıç: mevcut bir MongoDB Node. js web uygulamasını Azure Cosmos DB 'ye geçirme](create-mongodb-nodejs.md).

## <a name="cassandra-api"></a>Cassandra API’si

Azure Cosmos DB Cassandra API karşı işlemler gerçekleştirdiğinizde RU ücreti, gelen yükünde adlı `RequestCharge`bir alan olarak döndürülür. RU ücretini almak için çeşitli seçenekleriniz vardır.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

[.NET SDK](https://www.nuget.org/packages/CassandraCSharpDriver/)'yı kullandığınızda, bir `Info` `RowSet` nesnenin özelliği altında gelen yükü alabilirsiniz:

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"].Reverse().ToArray(), 0);
```

Daha fazla bilgi için bkz. [hızlı başlangıç: .NET SDK ve Azure Cosmos DB kullanarak Cassandra uygulaması oluşturma](create-cassandra-dotnet.md).

### <a name="use-the-java-sdk"></a>Java SDK 'sını kullanma

[Java SDK 'sını](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core)kullandığınızda, `getExecutionInfo()` yöntemi bir `ResultSet` nesne üzerinde çağırarak gelen yükü alabilirsiniz:

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

Daha fazla bilgi için bkz. [hızlı başlangıç: Java SDK 'sını kullanarak Cassandra uygulaması derleme ve Azure Cosmos DB](create-cassandra-java.md).

## <a name="gremlin-api"></a>Gremlin API

Gremlin API 'yi kullandığınızda, bir Azure Cosmos kapsayıcısına karşı bir işlemin RU tüketimini bulmak için birden çok seçeneğiniz vardır. 

### <a name="use-drivers-and-sdk"></a>Sürücüleri ve SDK 'Yı kullanma

Gremlin API tarafından döndürülen üstbilgiler, şu anda Gremlin .NET ve Java SDK 'Sı tarafından ortaya çıkacak olan özel durum öznitelikleriyle eşlenir. İstek ücreti, `x-ms-request-charge` anahtar altında kullanılabilir.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

[Gremlin.NET SDK 'sını](https://www.nuget.org/packages/Gremlin.Net/)kullandığınızda, durum öznitelikleri `StatusAttributes` `ResultSet<>` nesnenin özelliği altında kullanılabilir:

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

Daha fazla bilgi için bkz. [hızlı başlangıç: Azure Cosmos DB Gremlin API hesabı kullanarak .NET Framework veya çekirdek uygulama oluşturma](create-graph-dotnet.md).

### <a name="use-the-java-sdk"></a>Java SDK 'sını kullanma

[Gremlin Java SDK 'sını](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver)kullandığınızda, `statusAttributes()` `ResultSet` nesne üzerinde yöntemini çağırarak durum özniteliklerini alabilirsiniz:

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

Daha fazla bilgi için bkz. [hızlı başlangıç: Java SDK 'sını kullanarak Azure Cosmos DB grafik veritabanı oluşturma](create-graph-java.md).

## <a name="table-api"></a>Tablo API’si

Şu anda, tablo işlemleri için RU ücreti döndüren tek SDK [.NET Standard SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)olur. `TableResult` Nesnesi, Azure Cosmos db tablo API'si `RequestCharge` karşı kullandığınızda SDK tarafından doldurulan bir özelliği kullanıma sunar:

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

Daha fazla bilgi için bkz. [hızlı başlangıç: .NET SDK ve Azure Cosmos DB kullanarak tablo API'si uygulama oluşturma](create-table-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

RU tüketiminizi en iyi duruma getirme hakkında bilgi edinmek için şu makalelere bakın:

* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
* [Azure Cosmos DB’de sağlanan işlem hızını iyileştirme](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu maliyetini iyileştirme](optimize-cost-queries.md)
* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Bir kapsayıcı için sağlama işleme](how-to-provision-container-throughput.md)
* [Azure Cosmos DB ölçümlerle izleme ve hata ayıklama](use-metrics.md)
