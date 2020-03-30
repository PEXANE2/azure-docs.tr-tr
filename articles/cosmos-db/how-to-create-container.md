---
title: Azure Cosmos DB'de kapsayıcı oluşturma
description: Azure portalı, .Net, Java, Python, Node.js ve diğer SDK'ları kullanarak Azure Cosmos DB'de nasıl bir kapsayıcı oluşturabilirsiniz öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 4eaa2974817bfcd8bef83e5139d75a2d4c2ec107
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873718"
---
# <a name="create-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı oluşturma

Bu makalede, bir Azure Cosmos kapsayıcı (koleksiyon, tablo veya grafik) oluşturmak için farklı yollar açıklanmaktadır. Bunun için Azure portalLarını, Azure CLI'yi veya desteklenen SDK'ları kullanabilirsiniz. Bu makalede, kapsayıcı oluşturmak için nasıl gösterir, bölüm anahtarı belirtin ve madde iş parçacığı.

## <a name="create-a-container-using-azure-portal"></a>Azure portalını kullanarak kapsayıcı oluşturma

### <a name="sql-api"></a><a id="portal-sql"></a>SQL API’si

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Kapsayıcı'yı**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı oluşturup oluşturmadığınızı veya varolan bir veritabanını kullanıp kullanmadığınızı belirtin.
   * Bir kapsayıcı kimliği girin.
   * Bir bölüm anahtarı girin.
   * (örneğin, 1000 RUs) sağlanacak bir iş elde etme girin.
   * **Tamam'ı**seçin.

    ![Yeni Kapsayıcı vurgulanmış Veri Gezgini bölmesinin ekran görüntüsü](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a name="azure-cosmos-db-api-for-mongodb"></a><a id="portal-mongodb"></a>MongoDB için Azure Cosmos DB API

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-mongodb-dotnet.md#create-a-database-account)veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Kapsayıcı'yı**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı oluşturup oluşturmadığınızı veya varolan bir veritabanını kullanıp kullanmadığınızı belirtin.
   * Bir kapsayıcı kimliği girin.
   * Bir parça tuşu girin.
   * (örneğin, 1000 RUs) sağlanacak bir iş elde etme girin.
   * **Tamam'ı**seçin.

    ![MongoDB için Azure Cosmos DB API ekran görüntüsü, Kapsayıcı ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a name="cassandra-api"></a><a id="portal-cassandra"></a>Cassandra API

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-cassandra-dotnet.md#create-a-database-account)veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Tablo'yu**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir anahtar alanı mı oluşturduğunızı veya varolan bir anahtar alanı kullanıp kullanmadığınızı belirtin.
   * Tablo adı girin.
   * Özellikleri girin ve birincil anahtar belirtin.
   * (örneğin, 1000 RUs) sağlanacak bir iş elde etme girin.
   * **Tamam'ı**seçin.

    ![Cassandra API ekran görüntüsü, Tablo ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Cassandra API'si için, bölüm anahtarı birincil anahtar olarak kullanılır.

### <a name="gremlin-api"></a><a id="portal-gremlin"></a>Gremlin API

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-graph-dotnet.md#create-a-database-account)veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Grafik'i**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı oluşturup oluşturmadığınızı veya varolan bir veritabanını kullanıp kullanmadığınızı belirtin.
   * Grafik Kimliği girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Vertices için bir bölüm anahtarı girin.
   * (örneğin, 1000 RUs) sağlanacak bir iş elde etme girin.
   * **Tamam'ı**seçin.

    ![Gremlin API ekran görüntüsü, Grafik ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a name="table-api"></a><a id="portal-table"></a>Tablo API'si

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-table-dotnet.md#create-a-database-account)veya varolan bir hesabı seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Tablo'yu**seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Tablo Kimliği girin.
   * (örneğin, 1000 RUs) sağlanacak bir iş elde etme girin.
   * **Tamam'ı**seçin.

    ![Tablo API ekran görüntüsü, Tablo ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Tablo API'si için, her yeni satır eklediğinizde bölüm anahtarı belirtilir.

## <a name="create-a-container-using-azure-cli"></a>Azure CLI kullanarak kapsayıcı oluşturma<a id="cli-sql"></a><a id="cli-mongodb"></a><a id="cli-cassandra"></a><a id="cli-gremlin"></a><a id="cli-table"></a>

Aşağıdaki bağlantılar, Azure CLI kullanarak Azure Cosmos DB için kapsayıcı kaynaklarının nasıl oluşturulacak olduğunu gösterir.

Tüm Azure Cosmos DB API'lerinde tüm Azure CLI örneklerinin [Cassandra API](cli-samples-cassandra.md)listesi için [MongoDB API](cli-samples-mongodb.md) [bkz.](cli-samples.md) [Gremlin API](cli-samples-gremlin.md) [Table API](cli-samples-table.md)

* [Azure CLI ile kapsayıcı oluşturma](manage-with-cli.md#create-a-container)
* [Azure CLI ile MongoDB API için Azure Cosmos DB için bir koleksiyon oluşturma](./scripts/cli/mongodb/create.md)
* [Azure CLI ile Cassandra tablosu oluşturma](./scripts/cli/cassandra/create.md)
* [Azure CLI ile Gremlin grafiği oluşturma](./scripts/cli/gremlin/create.md)
* [Azure CLI ile Tablo API tablosu oluşturma](./scripts/cli/table/create.md)

## <a name="create-a-container-using-powershella-idps-mongodba-idps-gremlin"></a>PowerShell'i kullanarak konteyner oluşturma<a id="ps-sql"></a><a id="ps-mongodb"><a id="ps-cassandra"></a><a id="ps-gremlin"><a id="ps-table"></a>

Aşağıdaki bağlantılar, PowerShell kullanarak Azure Cosmos DB için kapsayıcı kaynaklarının nasıl oluşturulacak olduğunu göstermektedir.

Tüm Azure Cosmos DB API'lerinde tüm Azure CLI örneklerinin [Cassandra API](powershell-samples-cassandra.md)listesi için [MongoDB API](powershell-samples-mongodb.md) [bkz.](powershell-samples-sql.md) [Gremlin API](powershell-samples-gremlin.md) [Table API](powershell-samples-table.md)

* [Powershell ile bir kapsayıcı oluşturma](manage-with-powershell.md#create-container)
* [Powershell ile MongoDB API için Azure Cosmos DB için bir koleksiyon oluşturma](./scripts/powershell/mongodb/ps-mongodb-create.md)
* [Powershell ile Cassandra tablosu oluşturma](./scripts/powershell/cassandra/ps-cassandra-create.md)
* [Powershell ile Gremlin grafiği oluşturma](./scripts/powershell/gremlin/ps-gremlin-create.md)
* [Powershell ile Tablo API tablosu oluşturma](./scripts/powershell/table/ps-table-create.md)

## <a name="create-a-container-using-net-sdk"></a>.NET SDK'sını kullanarak kapsayıcı oluşturma

### <a name="sql-api-and-gremlin-api"></a><a id="dotnet-sql-graph"></a>SQL API'si ve Gremlin API'si

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a name="azure-cosmos-db-api-for-mongodb"></a><a id="dotnet-mongodb"></a>MongoDB için Azure Cosmos DB API

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB tel protokolü [İstek Birimleri](request-units.md)kavramını anlamıyor. Üzerinde sağlanan iş ortası bulunan yeni bir koleksiyon oluşturmak için SQL API için Azure portalını veya Cosmos DB SDK'larını kullanın.

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek Birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)
