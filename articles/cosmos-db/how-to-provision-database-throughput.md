---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure Cosmos DB’de aktarım hızını veritabanı düzeyinde sağlamayı öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: rimman
ms.openlocfilehash: 29bc65c8afaa1fe4bdc39923bd2219184e8b3a96
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70093009"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB bir veritabanında üretilen iş sağlama

Bu makalede, Azure Cosmos DB bir veritabanında işleme sağlama açıklanmaktadır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Kapsayıcı düzeyinde ve veritabanı düzeyinde üretilen iş verimini ne zaman kullanacağınızı öğrenmek için bkz. [kapsayıcılar ve veritabanlarında üretilen iş sağlama Için kullanım örnekleri](set-throughput.md) makalesi. Azure portal veya Azure Cosmos DB SDK 'larını kullanarak veritabanı düzeyinde aktarım hızı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

### <a id="portal-sql"></a>SQL (Core) API'si

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturun](create-sql-api-dotnet.md#create-account)veya mevcut bir Azure Cosmos hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni veritabanı**' nı seçin. Aşağıdaki ayrıntıları sağlayın:

   * Bir veritabanı KIMLIĞI girin. 
   * **Sağlama işleme**' yı seçin.
   * Bir üretilen iş girin (örneğin, 1000 ru).
   * **Tamam**’ı seçin.

![Yeni veritabanı iletişim kutusunun ekran görüntüsü](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)


## <a name="provision-throughput-using-azure-cli"></a>Azure CLI kullanarak aktarım hızı sağlama

```azcli-interactive
az cosmosdb database create --db-name
                            [--key]
                            [--name]
                            [--resource-group-name]
                            [--subscription]
                            [--throughput]
                            [--url-connection]
```




## <a name="provision-throughput-using-powershell"></a>PowerShell kullanarak üretilen iş sağlama

```azurepowershell-interactive
# Create a database and provision throughput of 400 RU/s
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$databaseResourceName = $accountName + "/sql/" + $databaseName

$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"= 400 }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties
```

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Tüm API 'Ler için üretilen iş sağlamak üzere SQL API için Cosmos SDK 'larını kullanabilirsiniz. İsteğe bağlı olarak, Cassandra API için aşağıdaki örneği de kullanabilirsiniz.

### <a id="dotnet-all"></a>Tüm API’ler
### <a name="net-v2-sdk"></a>.Net v2 SDK

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

### <a name="net-v3-sdk"></a>.Net v3 SDK
[!code-csharp[](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos/tests/Microsoft.Azure.Cosmos.Tests/SampleCodeForDocs/DatabaseDocsSampleCode.cs?name=DatabaseCreateWithThroughput)]

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 400 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=400);
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ' de sağlanan aktarım hızı hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Küresel olarak sağlanan verimlilik ölçeği](scaling-throughput.md)
* [Kapsayıcılar ve veritabanları üzerinde üretilen iş sağlama](set-throughput.md)
* [Kapsayıcı için aktarım hızı sağlama](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
