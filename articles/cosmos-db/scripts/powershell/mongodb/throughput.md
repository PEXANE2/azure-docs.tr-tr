---
title: MongoDB için Azure Cosmos DBs API 'SI için üretilen iş (RU/s) işlemleri için PowerShell betikleri
description: MongoDB için Azure Cosmos DBs API 'SI için üretilen iş (RU/s) işlemleri için PowerShell betikleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 10/07/2020
ms.author: mjbrown
ms.openlocfilehash: 253b604c540d4ef903b3769ad90ac500e4b832d3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98677890"
---
# <a name="throughput-rus-operations-with-powershell-for-a-database-or-collection-for-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API 'sine yönelik bir veritabanı veya koleksiyon için PowerShell ile işleme (RU/s) işlemleri
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Bu örnek Azure PowerShell az 5.4.0 veya üstünü gerektirir. `Get-Module -ListAvailable Az`Hangi sürümlerin yüklü olduğunu görmek için ' i çalıştırın.
Yüklemeniz gerekiyorsa bkz. [ınstall Azure PowerShell Module](/powershell/azure/install-az-ps).

Azure 'da oturum açmak için [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu çalıştırın.

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
| [Get-Azcosmosdbmongodbdatabaseverimini](/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabasethroughput) | MongoDB veritabanı için belirtilen Azure Cosmos DB API 'sinin aktarım hızı değerini alır. |
| [Get-Azcosmosdbmongodbcollectionverimlilik](/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollectionthroughput) | MongoDB koleksiyonu için belirtilen Azure Cosmos DB API 'sinin aktarım hızı değerini alır. |
| [Update-Azcosmosdbmongodbdatabaseverimini](/powershell/module/az.cosmosdb/update-azcosmosdbmongodbdatabasethroughput) | MongoDB veritabanı için Azure Cosmos DB API 'sinin üretilen iş değerini güncelleştirir. |
| [Update-Azcosmosdbmongodbcollectionverimini](/powershell/module/az.cosmosdb/update-azcosmosdbmongodbcollectionthroughput) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin üretilen iş değerini güncelleştirir. |
| [Invoke-AzCosmosDBMongoDBDatabaseThroughputMigration](/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbdatabasethroughputmigration) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin aktarım hızını geçirin. |
| [Invoke-AzCosmosDBMongoDBCollectionThroughputMigration](/powershell/module/az.cosmosdb/invoke-azcosmosdbmongodbcollectionthroughputmigration) | MongoDB koleksiyonu için Azure Cosmos DB API 'sinin aktarım hızını geçirin. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).