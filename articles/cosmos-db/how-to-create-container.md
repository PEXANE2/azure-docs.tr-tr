---
title: Azure Cosmos DB'de kapsayıcı oluşturma
description: Azure portal, .net, Java, Python, Node.js ve diğer SDK 'Ları kullanarak Azure Cosmos DB bir kapsayıcı oluşturmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 04/24/2020
ms.author: mjbrown
ms.openlocfilehash: 22c51497a9c9a331f1337134fbaf7c781b9c8ba7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390899"
---
# <a name="create-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı oluşturma

Bu makalede, Azure Cosmos kapsayıcısı (koleksiyon, tablo veya grafik) oluşturmanın farklı yolları açıklanmaktadır. Bu için Azure portal, Azure CLı veya desteklenen SDK 'lar kullanabilirsiniz. Bu makalede bir kapsayıcı oluşturma, Bölüm anahtarını belirtme ve üretilen iş sağlama işlemlerinin nasıl yapılacağı gösterilir.

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

## <a name="create-a-container-using-azure-portal"></a>Azure portalını kullanarak kapsayıcı oluşturma

### <a name="sql-api"></a><a id="portal-sql"></a>SQL APı 'SI

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya var olan bir hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **yeni kapsayıcı**' yı seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı mı yoksa var olan bir veritabanını mı kullandığınızı belirtin.
   * Bir kapsayıcı KIMLIĞI girin.
   * Bir bölüm anahtarı girin.
   * Sağlanacak bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-sql.png" alt-text="Yeni kapsayıcı vurgulanmış şekilde Veri Gezgini bölmesinin ekran görüntüsü":::

### <a name="azure-cosmos-db-api-for-mongodb"></a><a id="portal-mongodb"></a>MongoDB için Azure Cosmos DB API

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-mongodb-dotnet.md#create-a-database-account)veya var olan bir hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **yeni kapsayıcı**' yı seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı mı yoksa var olan bir veritabanını mı kullandığınızı belirtin.
   * Bir kapsayıcı KIMLIĞI girin.
   * Parça anahtarı girin.
   * Sağlanacak bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-mongodb.png" alt-text="MongoDB için Azure Cosmos DB API 'nin ekran görüntüsü, kapsayıcı ekleme iletişim kutusu":::

### <a name="cassandra-api"></a><a id="portal-cassandra"></a>Cassandra API’si

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-cassandra-dotnet.md#create-a-database-account)veya var olan bir hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni tablo**' yı seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir keyspace mi, yoksa var olanı mi kullandığınızı belirtin.
   * Tablo adı girin.
   * Özellikleri girin ve bir birincil anahtar belirtin.
   * Sağlanacak bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-cassandra.png" alt-text="Cassandra API, tablo Ekle iletişim kutusunun ekran görüntüsü":::

> [!NOTE]
> Cassandra API'si için, bölüm anahtarı birincil anahtar olarak kullanılır.

### <a name="gremlin-api"></a><a id="portal-gremlin"></a>Gremlin API

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-graph-dotnet.md#create-a-database-account)veya var olan bir hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **yeni grafik**' i seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı mı yoksa var olan bir veritabanını mı kullandığınızı belirtin.
   * Bir grafik KIMLIĞI girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Köşeler için bir bölüm anahtarı girin.
   * Sağlanacak bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-gremlin.png" alt-text="Gremlin API ekran görüntüsü, Graf Ekle iletişim kutusu":::

### <a name="table-api"></a><a id="portal-table"></a>Tablo API’si

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-table-dotnet.md#create-a-database-account)veya var olan bir hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni tablo**' yı seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Tablo KIMLIĞI girin.
   * Sağlanacak bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-table.png" alt-text="Tablo API'si, tablo Ekle iletişim kutusunun ekran görüntüsü":::

> [!Note]
> Tablo API'si için, her yeni satır eklediğinizde bölüm anahtarı belirtilir.

## <a name="create-a-container-using-azure-cli"></a>Azure CLı kullanarak kapsayıcı oluşturma<a id="cli-sql"></a><a id="cli-mongodb"></a><a id="cli-cassandra"></a><a id="cli-gremlin"></a><a id="cli-table"></a>

Aşağıdaki bağlantılar, Azure CLı kullanarak Azure Cosmos DB için kapsayıcı kaynakları oluşturmayı gösterir.

Tüm Azure Cosmos DB API 'lerde tüm Azure CLı örneklerinin listesi için bkz. [SQL API](cli-samples.md), [Cassandra API](cli-samples-cassandra.md), [MongoDB API](cli-samples-mongodb.md), [Gremlin API](cli-samples-gremlin.md)ve [tablo API'si](cli-samples-table.md)

* [Azure CLı ile kapsayıcı oluşturma](manage-with-cli.md#create-a-container)
* [Azure CLı ile MongoDB API 'SI için Azure Cosmos DB bir koleksiyon oluşturma](./scripts/cli/mongodb/create.md)
* [Azure CLı ile Cassandra tablosu oluşturma](./scripts/cli/cassandra/create.md)
* [Azure CLı ile Gremlin grafiği oluşturma](./scripts/cli/gremlin/create.md)
* [Azure CLı ile Tablo API'si tablosu oluşturma](./scripts/cli/table/create.md)

## <a name="create-a-container-using-powershella-idps-mongodba-idps-gremlin"></a>PowerShell kullanarak kapsayıcı oluşturma<a id="ps-sql"></a><a id="ps-mongodb"><a id="ps-cassandra"></a><a id="ps-gremlin"><a id="ps-table"></a>

Aşağıdaki bağlantılar, PowerShell kullanarak Azure Cosmos DB için kapsayıcı kaynaklarının nasıl oluşturulacağını gösterir.

Tüm Azure Cosmos DB API 'lerde tüm Azure CLı örneklerinin listesi için bkz. [SQL API](powershell-samples-sql.md), [Cassandra API](powershell-samples-cassandra.md), [MongoDB API](powershell-samples-mongodb.md), [Gremlin API](powershell-samples-gremlin.md)ve [tablo API'si](powershell-samples-table.md)

* [PowerShell ile kapsayıcı oluşturma](manage-with-powershell.md#create-container)
* [PowerShell ile MongoDB API 'SI için Azure Cosmos DB bir koleksiyon oluşturma](./scripts/powershell/mongodb/ps-mongodb-create.md)
* [PowerShell ile Cassandra tablosu oluşturma](./scripts/powershell/cassandra/ps-cassandra-create.md)
* [PowerShell ile Gremlin grafiği oluşturma](./scripts/powershell/gremlin/ps-gremlin-create.md)
* [PowerShell ile Tablo API'si tablosu oluşturma](./scripts/powershell/table/ps-table-create.md)

## <a name="create-a-container-using-net-sdk"></a>.NET SDK'sını kullanarak kapsayıcı oluşturma

Bir koleksiyon oluştururken zaman aşımı özel durumuyla karşılaşırsanız, koleksiyonun başarıyla oluşturulduğunu doğrulamak için bir okuma işlemi yapın. Okuma işlemi, koleksiyon oluşturma işlemi başarılı olana kadar bir özel durum oluşturur. Oluşturma işlemi tarafından desteklenen durum kodlarının listesi için [Azure Cosmos DB Için http durum kodları](/rest/api/cosmos-db/http-status-codes-for-cosmosdb) makalesine bakın.

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
> MongoDB tel protokolü, [Istek birimi](request-units.md)kavramını anlamıyor. Üzerinde üretilen işleme ile yeni bir koleksiyon oluşturmak için SQL API için Azure portal veya Cosmos DB SDK 'larını kullanın.

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API’si

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](account-overview.md)
