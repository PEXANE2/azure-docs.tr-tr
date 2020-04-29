---
title: Azure Cosmos DB SQL API veritabanı veya kapsayıcısı için üretilen iş (RU/s) almak için PowerShell betiği
description: Azure Cosmos DB SQL API veritabanı veya kapsayıcısı için üretilen iş (RU/s) almak için betik Azure PowerShell
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 03/17/2020
ms.author: mjbrown
ms.openlocfilehash: f471b1d41314728a6c6f0c5d2ab981891e2caa87
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80365605"
---
# <a name="get-throughput-rus-for-azure-cosmos-db-sql-api-database-or-container"></a>Azure Cosmos DB SQL API veritabanı veya kapsayıcısı için üretilen iş (RU/s) Al

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-ru-get.ps1 "Get throughput (RU/s) for Azure Cosmos DB SQL API database or container")]

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
| [Get-Azcosmosdbsqldatabaseüretilen Işi](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbsqldatabasethroughput) | Azure Cosmos DB bir SQL API veritabanında sağlanan aktarım hızını alın. |
| [Get-Azcosmosdbsqlcontainerüretilen Işi](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbsqlcontainerthroughput) | Azure Cosmos DB bir SQL API kapsayıcısında sağlanan aktarım hızını alın. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.