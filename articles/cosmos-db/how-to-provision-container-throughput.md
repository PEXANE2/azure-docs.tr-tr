---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure portal, CLı, PowerShell ve çeşitli diğer SDK 'Ları kullanarak Azure Cosmos DB kapsayıcı düzeyinde aktarım hızı sağlamayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 12/13/2019
ms.author: mjbrown
ms.custom: devx-track-javascript, devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: cf2bd616fc739d233826769baaa14298d0fcec2d
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017133"
---
# <a name="provision-standard-manual-throughput-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısında standart (el ile) aktarım hızı sağlama

Bu makalede, Azure Cosmos DB bir kapsayıcı (koleksiyon, grafik veya tablo) üzerinde standart (el ile) aktarım hızı sağlama açıklanmaktadır. Tek bir kapsayıcıda üretilen iş sağlama veya [bir veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md) ve veritabanı içindeki kapsayıcılar arasında paylaşma sağlayabilirsiniz. Azure portal, Azure CLı veya Azure Cosmos DB SDK 'Ları kullanarak bir kapsayıcıda aktarım hızı sağlayabilirsiniz.

## <a name="azure-portal"></a>Azure portal

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni koleksiyon**' u seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı mı yoksa var olan bir veritabanını mı kullandığınızı belirtin.
   * Bir kapsayıcı (veya tablo veya grafik) KIMLIĞI girin.
   * Bir bölüm anahtarı değeri girin (örneğin, `/userid` ).
   * Sağlamak istediğiniz bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png" alt-text="Yeni koleksiyonla vurgulanan Veri Gezgini ekran görüntüsü":::

## <a name="azure-cli-or-powershell"></a>Azure CLı veya PowerShell

Adanmış aktarım hızı içeren bir kapsayıcı oluşturmak için bkz.

* [Azure CLI kullanarak kapsayıcı oluşturma](manage-with-cli.md#create-a-container)
* [PowerShell kullanarak kapsayıcı oluşturma](manage-with-powershell.md#create-container)

> [!Note]
> MongoDB için Azure Cosmos DB API 'siyle yapılandırılmış bir Azure Cosmos hesabındaki kapsayıcıda aktarım hızı sağlıyorsanız, `/myShardKey` bölüm anahtarı yolu için kullanın. Cassandra API ile yapılandırılmış bir Azure Cosmos hesabındaki kapsayıcıda aktarım hızı sağlıyorsanız, `/myPrimaryKey` bölüm anahtarı yolu için kullanın.

## <a name="net-sdk"></a>.NET SDK

> [!Note]
> Cassandra ve MongoDB API hariç tüm Cosmos DB API 'Leri için üretilen iş sağlamak üzere SQL API için Cosmos SDK 'larını kullanın.

### <a name="sql-gremlin-and-table-apis"></a><a id="dotnet-most"></a>SQL, Gremlin ve tablo API 'Leri

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
// Create a container with a partition key and provision throughput of 400 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 400 });
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/ContainerDocsSampleCode.cs?name=ContainerCreateWithThroughput)]

---

## <a name="javascript-sdk"></a>JavaScript SDK'sı

```javascript
// Create a new Client
const client = new CosmosClient({ endpoint, key });

// Create a database
const { database } = await client.databases.createIfNotExists({ id: "databaseId" });

// Create a container with the specified throughput
const { resource } = await database.containers.createIfNotExists({
id: "containerId",
throughput: 1000
});

// To update an existing container or databases throughput, you need to user the offers API
// Get all the offers
const { resources: offers } = await client.offers.readAll().fetchAll();

// Find the offer associated with your container or the database
const offer = offers.find((_offer) => _offer.offerResourceId === resource._rid);

// Change the throughput value
offer.content.offerThroughput = 2000;

// Replace the offer.
await client.offer(offer.id).replace(offer);
```

### <a name="mongodb-api"></a><a id="dotnet-mongodb"></a>MongoDB API’si

```csharp
// refer to MongoDB .NET Driver
// https://docs.mongodb.com/drivers/csharp

// Create a new Client
String mongoConnectionString = "mongodb://DBAccountName:Password@DBAccountName.documents.azure.com:10255/?ssl=true&replicaSet=globaldb";
mongoUrl = new MongoUrl(mongoConnectionString);
mongoClientSettings = MongoClientSettings.FromUrl(mongoUrl);
mongoClient = new MongoClient(mongoClientSettings);

// Change the database name
mongoDatabase = mongoClient.GetDatabase("testdb");

// Change the collection name, throughput value then update via MongoDB extension commands
// https://docs.microsoft.com/en-us/azure/cosmos-db/mongodb-custom-commands#update-collection

var result = mongoDatabase.RunCommand<BsonDocument>(@"{customAction: ""UpdateCollection"", collection: ""testcollection"", offerThroughput: 400}");
```

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API’si

Benzer komutlar, CQL uyumlu herhangi bir sürücü aracılığıyla verilebilir.

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 400 RU/s
session.Execute("CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=400");

```
### <a name="alter-or-change-throughput-for-cassandra-table"></a>Cassandra tablosu için işleme değiştirme veya değişiklik

```csharp
// Altering the throughput too can be done through code by issuing following command
session.Execute("ALTER TABLE myKeySpace.myTable WITH cosmosdb_provisioned_throughput=5000");
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ' de üretilen iş sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Bir veritabanında standart (el ile) işleme sağlama](how-to-provision-database-throughput.md)
* [Veritabanında otomatik ölçeklendirme üretilen işi sağlama](how-to-provision-autoscale-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
