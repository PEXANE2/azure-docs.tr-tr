---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure portalı, CLI, PowerShell ve diğer çeşitli SDK'ları kullanarak Azure Cosmos DB'deki veritabanı düzeyinde nasıl veri metot sağlarınızda bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/28/2019
ms.author: mjbrown
ms.openlocfilehash: ef7d06dfb074a3453f5589284cbdaf079c48d111
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78933778"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB'deki bir veritabanında sağlama iş ortası

Bu makalede, Azure Cosmos DB'deki bir veritabanında iş ortası nasıl sağlanıyor açıklanmaktadır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Kapsayıcı düzeyi ve veritabanı düzeyi iş parçacığının ne zaman kullanılacağını öğrenmek [için, kapsayıcılar ve veritabanları makalesinde iş parçacığı sağlamak için Kullanım örneklerine](set-throughput.md) bakın. Azure portalını veya Azure Cosmos DB SDK'larını kullanarak veritabanı düzeyinde iş oluşturma yı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

### <a name="sql-core-api"></a><a id="portal-sql"></a>SQL (Core) API

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya varolan bir Azure Cosmos hesabını seçin.

1. Veri **Gezgini** bölmesini açın ve **Yeni Veritabanı'nı**seçin. Aşağıdaki ayrıntıları sağlayın:

   * Bir veritabanı kimliği girin.
   * **Tedarik iş bürüncülünü**seçin.
   * Bir iş mesuliyeti girin (örneğin, 1000 RUs).
   * **Tamam'ı**seçin.

    ![Yeni Veritabanı iletişim kutusunun ekran görüntüsü](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli-or-powershell"></a>Azure CLI veya PowerShell kullanarak sağlama girdisi

Paylaşılan iş tamtisi ile bir veritabanı oluşturmak için bkz.

* [Azure CLI'yi kullanarak veritabanı oluşturma](manage-with-cli.md#create-a-database-with-shared-throughput)
* [Powershell'i kullanarak veritabanı oluşturma](manage-with-powershell.md#create-db-ru)

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Tüm API'ler için iş bitimi sağlamak için SQL API için Cosmos SDK'ları kullanabilirsiniz. İsteğe bağlı olarak Cassandra API için aşağıdaki örneği de kullanabilirsiniz.

### <a name="all-apis"></a><a id="dotnet-all"></a>Tüm API’ler

### <a name="net-v2-sdk"></a>.Net V2 SDK

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

### <a name="net-v3-sdk"></a>.Net V3 SDK

[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

### <a name="cassandra-api"></a><a id="dotnet-cassandra"></a>Cassandra API
Benzer komut herhangi bir CQL uyumlu sürücü aracılığıyla yürütülebilir. 
```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute("CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400");
```
 
## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de sağlanan iş ortası hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md)
* [Kapsayıcı için aktarım hızı sağlama](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
