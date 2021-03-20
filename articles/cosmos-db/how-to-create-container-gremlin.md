---
title: Azure Cosmos DB Gremlin API 'de kapsayıcı oluşturma
description: Azure portal, .NET ve diğer SDK 'Ları kullanarak Azure Cosmos DB Gremlin API 'de kapsayıcı oluşturmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
ms.date: 10/16/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: f7e9de1f23ec46af08fe96b5db3170fac9a7eb2e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93101638"
---
# <a name="create-a-container-in-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB Gremlin API 'de kapsayıcı oluşturma
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

Bu makalede, Azure Cosmos DB Gremlin API 'sinde bir kapsayıcı oluşturmanın farklı yolları açıklanmaktadır. Azure portal, Azure CLı, PowerShell veya desteklenen SDK 'lar kullanılarak nasıl kapsayıcı oluşturulacağını gösterir. Bu makalede bir kapsayıcı oluşturma, Bölüm anahtarını belirtme ve üretilen iş sağlama işlemlerinin nasıl yapılacağı gösterilir.

Bu makalede, Azure Cosmos DB Gremlin API 'sinde bir kapsayıcı oluşturmanın farklı yolları açıklanmaktadır. Farklı bir API kullanıyorsanız, kapsayıcıyı oluşturmak için bkz. [MongoDB Için API](how-to-create-container-mongodb.md), [Cassandra API](how-to-create-container-cassandra.md), [tablo API'si](how-to-create-container-table.md)ve [SQL API](how-to-create-container.md) makaleleri.

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

## <a name="create-using-azure-portal"></a><a id="portal-gremlin"></a>Azure portalını kullanarak oluşturma

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

## <a name="create-using-net-sdk"></a><a id="dotnet-sql-graph"></a>.NET SDK kullanarak oluşturma

Bir koleksiyon oluştururken zaman aşımı özel durumuyla karşılaşırsanız, koleksiyonun başarıyla oluşturulduğunu doğrulamak için bir okuma işlemi yapın. Okuma işlemi, koleksiyon oluşturma işlemi başarılı olana kadar bir özel durum oluşturur. Oluşturma işlemi tarafından desteklenen durum kodlarının listesi için [Azure Cosmos DB Için http durum kodları](/rest/api/cosmos-db/http-status-codes-for-cosmosdb) makalesine bakın.

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

## <a name="create-using-azure-cli"></a><a id="cli-mongodb"></a>Azure CLı kullanarak oluşturma

[Azure CLI Ile Gremlin grafiği oluşturun](./scripts/cli/gremlin/create.md). Tüm Azure Cosmos DB API 'lerde tüm Azure CLı örneklerinin listelenmesi için bkz. [Azure Cosmos DB Için Azure CLI örnekleri](cli-samples.md).

## <a name="create-using-powershell"></a>PowerShell kullanarak oluşturma

[PowerShell Ile Gremlin grafiği oluşturun](./scripts/powershell/gremlin/create.md). Tüm Azure Cosmos DB API 'lerde tüm PowerShell örneklerinin listesi için bkz. [PowerShell örnekleri](powershell-samples.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](./account-databases-containers-items.md)