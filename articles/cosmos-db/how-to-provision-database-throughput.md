---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure portal, CLı, PowerShell ve çeşitli diğer SDK 'Ları kullanarak Azure Cosmos DB veritabanı düzeyinde aktarım hızı sağlamayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: 63f8d408d76cfce86c254ad3840c2f0eefb09fd4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85263648"
---
# <a name="provision-standard-manual-throughput-on-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB bir veritabanında standart (el ile) aktarım hızı sağlama

Bu makalede, Azure Cosmos DB bir veritabanında standart (el ile) işleme sağlama açıklanır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Kapsayıcı düzeyi ve veritabanı düzeyi işleme ne zaman kullanacağınızı öğrenmek için bkz. [kapsayıcılar ve veritabanlarında üretilen iş sağlama Için kullanım örnekleri](set-throughput.md) makalesi. Azure portal veya Azure Cosmos DB SDK 'larını kullanarak veritabanı düzeyinde aktarım hızı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

### <a name="sql-core-api"></a><a id="portal-sql"></a>SQL (Core) API'si

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni veritabanı**' nı seçin. Aşağıdaki ayrıntıları sağlayın:

   * Bir veritabanı KIMLIĞI girin.
   * **Sağlama işleme**' yı seçin.
   * Bir üretilen iş girin (örneğin, 1000 ru).
   * **Tamam**’ı seçin.

    :::image type="content" source="./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png" alt-text="Yeni veritabanı iletişim kutusunun ekran görüntüsü":::

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Azure CLı veya PowerShell kullanarak verimlilik sağlama

Paylaşılan verimlilik içeren bir veritabanı oluşturmak için bkz.

* [Azure CLı kullanarak veritabanı oluşturma](manage-with-cli.md#create-a-database-with-shared-throughput)
* [PowerShell kullanarak veritabanı oluşturma](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Tüm API 'Ler için üretilen iş sağlamak üzere SQL API için Cosmos SDK 'larını kullanabilirsiniz. İsteğe bağlı olarak, Cassandra API için aşağıdaki örneği de kullanabilirsiniz.

### <a name="all-apis"></a><a id="dotnet-all"></a>Tüm API’ler

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

---

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API’si

Benzer komut, tüm CQL uyumlu sürücüler aracılığıyla yürütülebilir.

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute("CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400");
```
 
## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ' de sağlanan aktarım hızı hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Bir kapsayıcı için standart (el ile) üretilen iş sağlama](how-to-provision-container-throughput.md)
* [Bir kapsayıcı için otomatik ölçeklendirme üretilen işi sağlama](how-to-provision-autoscale-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
