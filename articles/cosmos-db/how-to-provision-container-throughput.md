---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure portalı, CLI, PowerShell ve diğer çeşitli SDK'ları kullanarak Azure Cosmos DB'deki kapsayıcı düzeyinde nasıl iş elde edebilirsiniz öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: mjbrown
ms.openlocfilehash: e416501cb3c532b3ba0a262442b35b236875a463
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273287"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı üzerinde sağlama iş ortası

Bu makalede, Azure Cosmos DB'deki bir kapsayıcıda (koleksiyon, grafik veya tablo) nasıl sağlanıyoraçıklanmıştır. Tek bir kapsayıcıda iş elde edilebilir veya [veritabanında tedarik iş lenebilir](how-to-provision-database-throughput.md) ve veritabanı içindeki kapsayıcılar arasında paylaşabilirsiniz. Azure portalı, Azure CLI veya Azure Cosmos DB SDK'ları kullanarak bir kapsayıcıda iş ortası sağlayabilirsiniz.

## <a name="azure-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya varolan bir Azure Cosmos hesabını seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Koleksiyon'u**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı oluşturup oluşturmadığınızı veya varolan bir veritabanını kullanıp kullanmadığınızı belirtin.
   * Kapsayıcı (veya Tablo veya Grafik) Kimliği girin.
   * Bir bölüm anahtar değeri girin `/userid`(örneğin, ).
   * Sağlamak istediğiniz bir iş mesuliyet girin (örneğin, 1000 RUs).
   * **Tamam'ı**seçin.

    ![Yeni Koleksiyon vurgulanmış Data Explorer ekran görüntüsü](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="azure-cli-or-powershell"></a>Azure CLI veya PowerShell

Özel iş bünyesine sahip bir kapsayıcı oluşturmak için,

* [Azure CLI kullanarak kapsayıcı oluşturma](manage-with-cli.md#create-a-container)
* [Powershell kullanarak bir kapsayıcı oluşturma](manage-with-powershell.md#create-container)

> [!Note]
> MongoDB için Azure Cosmos DB API ile yapılandırılan bir Azure Cosmos hesabında bir `/myShardKey` kapsayıcıda iş ortası oluşturuyorsanız, bölme anahtarı yolunu kullanın. Cassandra API ile yapılandırılan bir Azure Cosmos hesabında bir kapsayıcıda `/myPrimaryKey` iş ortası oluşturuyorsanız, bölme anahtarı yolunu kullanın.

## <a name="net-sdk"></a>.NET SDK

> [!Note]
> Cassandra API hariç tüm Cosmos DB API'leri için iş elde etmek için SQL API için Cosmos SDK'ları kullanın.

### <a name="sql-mongodb-gremlin-and-table-apis"></a><a id="dotnet-most"></a>SQL, MongoDB, Gremlin ve Tablo API'leri
### <a name="net-v2-sdk"></a>.Net V2 SDK

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

### <a name="net-v3-sdk"></a>.Net V3 SDK

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/ContainerDocsSampleCode.cs?name=ContainerCreateWithThroughput)]

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

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API

Benzer komutlar herhangi bir CQL uyumlu sürücü aracılığıyla verilebilir.

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 400 RU/s
session.Execute("CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=400");

```
### <a name="alter-or-change-throughput-for-cassandra-table"></a>Cassandra tablosu için iş akışını değiştirme veya değiştirme

```csharp
// Altering the throughput too can be done through code by issuing following command
session.Execute("ALTER TABLE myKeySpace.myTable WITH cosmosdb_provisioned_throughput=5000");
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de iş ortası sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Veritabanında iş oluşturma sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
