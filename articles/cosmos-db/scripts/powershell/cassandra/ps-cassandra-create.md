---
title: Azure Cosmos DB Cassandra API anahtar alanı ve tablo oluşturmak için PowerShell betiği
description: Azure PowerShell betik-Azure Cosmos DB oluştur Cassandra API anahtar alanı ve tablo
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 1fcded19731d412a4892e231f4445c51b330b1b4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80366135"
---
# <a name="create-a-keyspace-and-table-for-azure-cosmos-db---cassandra-api"></a>Azure Cosmos DB Cassandra API için bir anahtar uzayı ve tablo oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/cassandra/ps-cassandra-create.ps1 "Create a keyspace and table for Cassandra API")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure kaynakları**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Bir kaynak oluşturur. |
|**Azure Cosmos DB**| |
| [Set-AzCosmosDBCassandraKeyspace](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbcassandrakeyspace) | Bir Cosmos DB Cassandra API anahtar alanı oluşturur veya güncelleştirir. |
| [New-AzCosmosDBCassandraClusterKey](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraclusterkey) | Yeni bir CosmosDB Cassandra küme anahtarı oluşturur. |
| [New-AzCosmosDBCassandraColumn](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandracolumn) | Yeni bir CosmosDB Cassandra sütunu oluşturur. |
| [New-AzCosmosDBCassandraSchema](https://docs.microsoft.com/powershell/module/az.cosmosdb/new-azcosmosdbcassandraschema) | Yeni bir CosmosDB Cassandra şeması oluşturur. |
| [Set-AzCosmosDBCassandraTable](https://docs.microsoft.com/powershell/module/az.cosmosdb/set-azcosmosdbcassandratable) | Bir Cosmos DB Cassandra API tablosu oluşturur veya güncelleştirir. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.