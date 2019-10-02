---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure Cosmos DB’de kapsayıcı düzeyinde aktarım hızını sağlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 8da27773cc74324c1dde5a95de1abef3256c1f1c
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71811672"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısında üretilen iş sağlama

Bu makalede, Azure Cosmos DB bir kapsayıcı (koleksiyon, grafik veya tablo) üzerinde üretilen iş sağlama açıklanmaktadır. Tek bir kapsayıcıda üretilen iş sağlama veya [bir veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md) ve veritabanı içindeki kapsayıcılar arasında paylaşma sağlayabilirsiniz. Azure portal, Azure CLı veya Azure Cosmos DB SDK 'Ları kullanarak bir kapsayıcıda aktarım hızı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni koleksiyon**' u seçin. Ardından, aşağıdaki ayrıntıları sağlayın:

   * Yeni bir veritabanı mı yoksa var olan bir veritabanını mı kullandığınızı belirtin.
   * Bir kapsayıcı (veya tablo veya grafik) KIMLIĞI girin.
   * Bir bölüm anahtarı değeri girin (örneğin, `/userid`).
   * Sağlamak istediğiniz bir üretilen iş (örneğin, 1000 ru) girin.
   * **Tamam**’ı seçin.

    ![Yeni koleksiyonla vurgulanan Veri Gezgini ekran görüntüsü](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Azure CLı veya PowerShell kullanarak verimlilik sağlama

Adanmış aktarım hızı içeren bir kapsayıcı oluşturmak için bkz.

* [Azure CLı kullanarak kapsayıcı oluşturma](manage-with-cli.md#create-a-container)
* [PowerShell kullanarak kapsayıcı oluşturma](manage-with-powershell.md#create-container)

> [!Note]
> MongoDB için Azure Cosmos DB API 'siyle yapılandırılmış bir Azure Cosmos hesabındaki kapsayıcıda aktarım hızı sağlıyorsanız, bölüm anahtarı yolu için `/myShardKey` kullanın. Cassandra API ile yapılandırılmış bir Azure Cosmos hesabındaki kapsayıcıda aktarım hızı sağlıyorsanız, bölüm anahtarı yolu için `/myPrimaryKey` kullanın.

## <a name="provision-throughput-by-using-net-sdk"></a>.NET SDK kullanarak verimlilik sağlama

> [!Note]
> Cassandra API dışında tüm Cosmos DB API 'Leri için işleme sağlamak üzere SQL API için Cosmos SDK 'larını kullanın.

### <a id="dotnet-most"></a>SQL, MongoDB, Gremlin ve Tablo API'leri
### <a name="net-v2-sdk"></a>.Net v2 SDK

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

### <a name="net-v3-sdk"></a>.Net v3 SDK
[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/ContainerDocsSampleCode.cs?name=ContainerCreateWithThroughput)]

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 400 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=400);
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ' de üretilen iş sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Bir veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
