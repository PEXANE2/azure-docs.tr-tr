---
title: Otomatik ölçeklendirme ile Azure Cosmos MongoDB API veritabanı ve koleksiyonu oluşturmak için PowerShell betiği
description: Azure PowerShell betiği-otomatik ölçeklendirme ile Azure Cosmos MongoDB API veritabanı ve koleksiyonu oluşturma
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 07/30/2020
ms.author: mjbrown
ms.openlocfilehash: bd455af8b8a9a8a293165616e490d4b4d1c79f7a
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93075493"
---
# <a name="create-a-database-and-collection-with-autoscale-for-azure-cosmos-db---mongodb-api"></a>Azure Cosmos DB-MongoDB API 'SI için otomatik ölçeklendirmeyi olan bir veritabanı ve koleksiyon oluşturma
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-autoscale.ps1 "Create a database and collection with autoscale for MongoDB API")]

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
| [New-AzCosmosDBMongoDBDatabase](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbdatabase) | MongoDB API veritabanı oluşturur. |
| [New-AzCosmosDBMongoDBIndex](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbindex) | MongoDB API dizini oluşturur. |
| [New-AzCosmosDBMongoDBCollection](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbcollection) | MongoDB API 'SI koleksiyonu oluşturur. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).
