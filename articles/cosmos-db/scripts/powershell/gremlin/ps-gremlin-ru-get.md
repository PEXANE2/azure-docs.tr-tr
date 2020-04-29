---
title: Azure Cosmos DB Gremlin API 'SI için üretilen iş (RU/s) almak için PowerShell betiği
description: Gremlin API 'SI için Azure PowerShell betiği Azure Cosmos DB Get üretilen Iş (RU/s)
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: fa376aff9c127d5792da9e0489079ca3a1c67cf2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80366037"
---
# <a name="get-throughput-rus-for-a-database-or-graph-for-azure-cosmos-db---gremlin-api"></a>Azure Cosmos DB-Gremlin API 'SI için bir veritabanı veya grafik için üretilen Iş (RU/sn) alın

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-ru-get.ps1 "Get throughput on a database or graph for Gremlin API")]

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
| [Get-Azcosmosdbgremlindatabaseüretilen Iş](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlindatabasethroughput) | Belirtilen Gremlin API veritabanının üretilen iş değerini alır. |
| [Get-Azcosmosdbgremlingraphüretilen Iş](https://docs.microsoft.com/powershell/module/az.cosmosdb/get-azcosmosdbgremlingraphthroughput) | Belirtilen Gremlin API grafiğinin üretilen iş değerini alır. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.