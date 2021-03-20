---
title: Azure Cosmos DB Cassandra API bir kapsayıcı oluşturma
description: Azure portal, .NET, Java, Python, Node.js ve diğer SDK 'Ları kullanarak Azure Cosmos DB Cassandra API bir kapsayıcı oluşturmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 10/16/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: 01030a563c15eee7786058c2eae30d23803dfe42
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93101655"
---
# <a name="create-a-container-in-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API bir kapsayıcı oluşturma
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Bu makalede, Azure Cosmos DB Cassandra API bir kapsayıcı oluşturmanın farklı yolları açıklanmaktadır. Azure portal, Azure CLı, PowerShell veya desteklenen SDK 'lar kullanılarak nasıl kapsayıcı oluşturulacağını gösterir. Bu makalede bir kapsayıcı oluşturma, Bölüm anahtarını belirtme ve üretilen iş sağlama işlemlerinin nasıl yapılacağı gösterilir.

Bu makalede, Azure Cosmos DB Cassandra API bir kapsayıcı oluşturmanın farklı yolları açıklanmaktadır. Farklı bir API kullanıyorsanız, kapsayıcıyı oluşturmak için bkz. [MongoDB Için API](how-to-create-container-mongodb.md), [gremlin API](how-to-create-container-gremlin.md), [tablo API'si](how-to-create-container-table.md)ve [SQL API](how-to-create-container.md) makaleleri.

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

## <a name="create-using-azure-portal"></a><a id="portal-cassandra"></a>Azure portalını kullanarak oluşturma

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

## <a name="create-using-net-sdk"></a><a id="dotnet-cassandra"></a>.NET SDK kullanarak oluşturma

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

Bir koleksiyon oluştururken zaman aşımı özel durumuyla karşılaşırsanız, koleksiyonun başarıyla oluşturulduğunu doğrulamak için bir okuma işlemi yapın. Okuma işlemi, koleksiyon oluşturma işlemi başarılı olana kadar bir özel durum oluşturur. Oluşturma işlemi tarafından desteklenen durum kodlarının listesi için [Azure Cosmos DB Için http durum kodları](/rest/api/cosmos-db/http-status-codes-for-cosmosdb) makalesine bakın.

## <a name="create-using-azure-cli"></a><a id="cli-mongodb"></a>Azure CLı kullanarak oluşturma

[Azure CLI Ile Cassandra tablosu oluşturun](./scripts/cli/cassandra/create.md). Tüm Azure Cosmos DB API 'lerde tüm Azure CLı örneklerinin listelenmesi için bkz. [Azure Cosmos DB Için Azure CLI örnekleri](cli-samples.md).

## <a name="create-using-powershell"></a>PowerShell kullanarak oluşturma

[PowerShell Ile Cassandra tablosu oluşturun](./scripts/powershell/cassandra/create.md). Tüm Azure Cosmos DB API 'lerde tüm PowerShell örneklerinin listesi için bkz. [PowerShell örnekleri](powershell-samples.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
* [Azure Cosmos DB'de İstek birimleri](request-units.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Azure Cosmos hesabıyla çalışma](./account-databases-containers-items.md)