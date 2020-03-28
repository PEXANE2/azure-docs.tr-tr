---
title: Azure Cosmos DB SQL API veritabanı ve kapsayıcı oluşturmak için PowerShell komut dosyası
description: Azure PowerShell komut dosyası - Azure Cosmos DB SQL API veritabanı ve kapsayıcı oluşturmak
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 03/17/2020
ms.author: mjbrown
ms.openlocfilehash: 719e2cd831a982c62ab965cd7dc8a37c4cb41265
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365615"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>Azure Cosmos DB için veritabanı ve kapsayıcı oluşturma - SQL API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası, oturum düzeyi tutarlılığı, bir veritabanı ve bir bölüm anahtarı, özel dizin ilkesi, benzersiz anahtar ilkesi, TTL, özel iş bölümü ile çakışma çözümlemesi ilkesi kazanır ve `multipleWriteLocations=true`son yazar zaman kullanılacak özel bir çakışma çözümlemesi yolu ile iki bölgede SQL (Core) API için bir Cosmos hesabı oluşturur.

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
| [Yeni-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Yeni bir Cosmos DB Hesabı oluşturur. |
| [Set-AzCosmosDBSqlDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqldatabase) | Yeni bir cosmos DB SQL Veritabanı oluşturur veya güncelleştirir. |
| [Yeni-AzCosmosDBSqlUniqueKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | Yeni bir Cosmos DB SQL UniqueKey nesnesi oluşturur. |
| [Yeni-AzCosmosDBSqlUniqueKeyPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | Yeni bir Cosmos DB SQL UniqueKeyPolicy nesnesi oluşturur. |
| [Yeni-AzCosmosDBSqlIncludedPathIndex](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | Set-AzCosmosDBSqlIncludedPath için parametre olarak kullanılan PSIndexes türünde yeni bir nesne oluşturur. |
| [Yeni-AzCosmosDBSqlIncludedPath](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | New-AzCosmosDBSqlIndexingPolicy için parametre olarak kullanılan PSIncludedPath türünde yeni bir nesne oluşturur. |
| [Yeni-AzCosmosDBSqlIndexingPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | Set-AzCosmosDBSqlContainer için parametre olarak kullanılan PSSqlIndexingPolicy türünde yeni bir nesne oluşturur. |
| [Yeni-AzCosmosDBSqlConflictResolutionPolicy](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | Set-AzCosmosDBSqlContainer için parametre olarak kullanılan PSSqlConflictResolutionPolicy türünde yeni bir nesne oluşturur. |
| [Set-AzCosmosDBSqlContainer](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbsqlcontainer) | Yeni bir cosmos DB SQL Kapsayıcısı oluşturur veya güncelleştirir. |
|**Azure Kaynak Grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
