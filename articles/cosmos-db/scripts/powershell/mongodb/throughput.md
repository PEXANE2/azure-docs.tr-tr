---
title: MongoDB için Azure Cosmos DBs API 'SI için üretilen iş (RU/s) işlemleri için PowerShell betikleri
description: MongoDB için Azure Cosmos DBs API 'SI için üretilen iş (RU/s) işlemleri için PowerShell betikleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 781d74c2d9617972d70a8d2d3af771589fffdab7
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91844256"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-collection-for-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sine yönelik bir veritabanı veya koleksiyon için PowerShell ile işleme (RU/s) işlemleri

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="get-throughput"></a>Aktarım hızı alma

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-get.ps1 "Get throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="update-throughput"></a>Üretimi Güncelleştir

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-update.ps1 "Update throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="migrate-throughput"></a>Aktarım hızını geçir

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-ru-migrate.ps1 "Migrate between standard and autoscale throughput on a database or collection for Azure Cosmos DB API for MongoDB")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure Cosmos DB**| |
| [Get-Azcosmosdbmongodbdatabaseverimini](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabasethroughput) | MongoDB veritabanı için belirtilen Azure Cosmos DB API 'sinin aktarım hızı değerini alır. |
| [Get-Azcosmosdbmongodbcollectionverimlilik](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollectionthroughput) | MongoDB koleksiyonu için belirtilen Azure Cosmos DB API 'sinin aktarım hızı değerini alır. |
| [Update-Azcosmosdbmongodbdatabaseverimini](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbdatabasethroughput) | MongoDB veritabanı için Azure Cosmos DB API 'sinin üretilen iş değerini güncelleştirir. |
| [Update-Azcosmosdbmongodbcollectionverimini](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbmongodbcollectionthroughput) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin üretilen iş değerini güncelleştirir. |
| [Invoke-AzCosmosDBMongoDBDatabaseThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbdatabasethroughputmigration) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin aktarım hızını geçirin. |
| [Invoke-AzCosmosDBMongoDBCollectionThroughputMigration](https://docs.microsoft.com/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbcollectionthroughputmigration) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin aktarım hızını geçirin. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.