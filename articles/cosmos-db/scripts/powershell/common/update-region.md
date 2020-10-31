---
title: Azure Cosmos hesabının bölgelerini güncelleştirmek için PowerShell betiği
description: Azure PowerShell betik örneği-Azure Cosmos hesabının bölgelerini güncelleştirme
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/01/2020
ms.author: mjbrown
ms.openlocfilehash: 496e9ba375e1c83b0e5c06d5bd746ea7e02b3b14
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93099496"
---
# <a name="update-an-azure-cosmos-accounts-regions-using-powershell"></a>PowerShell kullanarak bir Azure Cosmos hesabının bölgelerini güncelleştirme
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Aynı işlemdeki bölgeleri değiştiremez ve diğer Cosmos hesabı özelliklerini değiştiremezsiniz. Bunların iki ayrı işlem olarak yapılması gerekir.
> [!NOTE]
> Bu örnekte bir SQL (Core) API hesabı kullanılması gösterilmektedir. Bu örneği diğer API 'Ler için kullanmak üzere ilgili özellikleri kopyalayın ve API 'ye özel betiğe uygulayın.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-update-region.ps1 "Update Azure Cosmos account regions")]

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
| [Get-AzCosmosDBAccount](/powershell/module/az.cosmosdb/get-azcosmosdbaccount) | Cosmos DB hesaplarını listeler veya belirtilen bir Cosmos DB hesabı alır. |
| [New-AzCosmosDBLocationObject](/powershell/module/az.cosmosdb/new-azcosmosdblocationobject) | Update-AzCosmosDBAccountRegion için parametre olarak kullanılacak PSLocation türünde bir nesne oluşturur. |
| [Update-AzCosmosDBAccountRegion](/powershell/module/az.cosmosdb/update-azcosmosdbaccountregion) | Cosmos DB hesabının bölgelerini güncelleştirin. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).