---
title: Azure Cosmos DB SQL API veritabanı ve kapsayıcısı oluşturmak için PowerShell betiği
description: Azure PowerShell betiği-Azure Cosmos DB SQL API veritabanı ve kapsayıcısı oluştur
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 03/17/2020
ms.author: mjbrown
ms.openlocfilehash: 719e2cd831a982c62ab965cd7dc8a37c4cb41265
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80365615"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>Azure Cosmos DB-SQL API 'SI için bir veritabanı ve kapsayıcı oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

Bu betik, oturum düzeyi tutarlılığı, bir veritabanı ve bölüm anahtarı, özel dizin oluşturma ilkesi, benzersiz anahtar ilkesi, TTL, adanmış aktarım hızı ve son yazıcı WINS çakışma çözümleme ilkesini, zaman `multipleWriteLocations=true`içinde kullanılacak özel bir çakışma çözümü olan bir kapsayıcı olan ıkı bölgede SQL (Core) API 'Si Için Cosmos hesabı oluşturur.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-create.ps1 "Create an account, database, and container for SQL API")]

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
| [New-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Yeni bir Cosmos DB hesabı oluşturur. |
| [Set-AzCosmosDBSqlDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqldatabase) | Yeni bir Cosmos DB SQL veritabanı oluşturur veya güncelleştirir. |
| [New-AzCosmosDBSqlUniqueKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | Yeni bir Cosmos DB SQL UniqueKey nesnesi oluşturur. |
| [New-AzCosmosDBSqlUniqueKeyPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | Yeni bir Cosmos DB SQL UniqueKeyPolicy nesnesi oluşturur. |
| [New-Azcosmosdbsqlıncludedpathındex](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | Set-Azcosmosdbsqlıncludedpath parametresi olarak kullanılan Psındexes türünde yeni bir nesne oluşturur. |
| [New-Azcosmosdbsqlıncludedpath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | New-AzCosmosDBSqlIndexingPolicy parametresi olarak kullanılan Psıncludedpath türünde yeni bir nesne oluşturur. |
| [New-AzCosmosDBSqlIndexingPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | Set-AzCosmosDBSqlContainer parametresi olarak kullanılan PSSqlIndexingPolicy türünde yeni bir nesne oluşturur. |
| [New-AzCosmosDBSqlConflictResolutionPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | Set-AzCosmosDBSqlContainer parametresi olarak kullanılan Psssınconflictresolutionpolicy türünde yeni bir nesne oluşturur. |
| [Set-AzCosmosDBSqlContainer](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqlcontainer) | Yeni bir Cosmos DB SQL kapsayıcısını oluşturur veya güncelleştirir. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
