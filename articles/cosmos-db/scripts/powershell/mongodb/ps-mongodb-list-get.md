---
title: MongoDB için Azure Cosmos DB API 'sindeki işlemleri listelemek ve almak için PowerShell betiği
description: MongoDB API 'SI için Azure PowerShell betiği Azure Cosmos DB listesi ve Get işlemleri
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 686b140915e90adefa15084c2e8317404b91ed09
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80365898"
---
# <a name="list-and-get-databases-and-graphs-for-azure-cosmos-db---mongodb-api"></a>Azure Cosmos DB-MongoDB API 'SI için veritabanları ve grafikler listeleme ve edinme

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-list-get.ps1 "List or get databases or collections for MongoDB API")]

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
| [Get-AzCosmosDBAccount](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Cosmos DB hesaplarını listeler veya belirtilen bir Cosmos DB hesabı alır. |
| [Get-AzCosmosDBMongoDBDatabase](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbdatabase) | Bir hesapta Cosmos DB MongoDB API veritabanlarını listeler veya bir hesapta belirtilen bir Cosmos DB MogoDB API 'SI veritabanını alır. |
| [Get-AzCosmosDBMongoDBCollection](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbmongodbcollection) | Cosmos DB MongoDB API koleksiyonlarını listeler veya bir veritabanında belirtilen bir Cosmos DB MongoDB API 'SI koleksiyonunu alır |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.