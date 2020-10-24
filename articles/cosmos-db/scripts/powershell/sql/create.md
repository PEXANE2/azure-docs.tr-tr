---
title: Azure Cosmos DB SQL API veritabanı ve kapsayıcısı oluşturmak için PowerShell betiği
description: Azure PowerShell betiği-Azure Cosmos DB SQL API veritabanı ve kapsayıcısı oluştur
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.openlocfilehash: 3ee4036a605e74ff43b3951b41ee3b4d1325004d
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92482052"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>Azure Cosmos DB-SQL API 'SI için bir veritabanı ve kapsayıcı oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

Bu betik, oturum düzeyi tutarlılığı, bir veritabanı ve bölüm anahtarı, özel dizin oluşturma ilkesi, benzersiz anahtar ilkesi, TTL, adanmış aktarım hızı ve son yazıcı WINS çakışma çözümleme ilkesini, zaman içinde kullanılacak özel bir çakışma çözümü olan bir kapsayıcı olan iki bölgede SQL (Core) API 'SI için Cosmos hesabı oluşturur `multipleWriteLocations=true` .

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
| [New-AzCosmosDBAccount](/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Cosmos DB hesabı oluşturur. |
| [New-AzCosmosDBSqlDatabase](/powershell/module/az.cosmosdb/new-azcosmosdbsqldatabase) | Cosmos DB bir SQL veritabanı oluşturur. |
| [New-AzCosmosDBSqlUniqueKey](/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | New-AzCosmosDBSqlUniqueKeyPolicy için parametre olarak kullanılan bir PSSqlUniqueKey nesnesi oluşturur. |
| [New-AzCosmosDBSqlUniqueKeyPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | New-AzCosmosDBSqlContainer parametresi olarak kullanılan bir PSSqlUniqueKeyPolicy nesnesi oluşturur. |
| [New-AzCosmosDBSqlCompositePath](/powershell/module/az.cosmosdb/new-azcosmosdbsqlcompositepath) | New-AzCosmosDBSqlIndexingPolicy için parametre olarak kullanılan PSCompositePath nesnesi oluşturur. |
| [New-Azcosmosdbsqlıncludedpathındex](/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | New-Azcosmosdbsqlıncludedpath parametresi olarak kullanılan bir Psındexes nesnesi oluşturur. |
| [New-Azcosmosdbsqlıncludedpath](/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | New-AzCosmosDBSqlIndexingPolicy için parametre olarak kullanılan bir Psıncludedpath nesnesi oluşturur. |
| [New-AzCosmosDBSqlIndexingPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | New-AzCosmosDBSqlContainer parametresi olarak kullanılan bir PSSqlIndexingPolicy nesnesi oluşturur. |
| [New-AzCosmosDBSqlConflictResolutionPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | New-AzCosmosDBSqlContainer parametresi olarak kullanılan bir Psssınconflictresolutionpolicy nesnesi oluşturur. |
| [New-AzCosmosDBSqlContainer](/powershell/module/az.cosmosdb/new-azcosmosdbsqlcontainer) | Yeni bir Cosmos DB SQL kapsayıcısı oluşturur. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).