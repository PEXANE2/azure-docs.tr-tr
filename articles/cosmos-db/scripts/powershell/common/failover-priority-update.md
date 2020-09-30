---
title: Tek bir yazma bölgesi olan bir Azure Cosmos hesabının yük devretme önceliğini değiştirmek için PowerShell betiği
description: Azure PowerShell betik örneği-tek bir yazma bölgesi olan bir Azure Cosmos hesabı için yük devretme önceliğini değiştirin veya yük devretmeyi tetikleyin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/18/2020
ms.author: mjbrown
ms.openlocfilehash: 07561de8236a06a3ac34aa8867a07890aaf0656f
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91570591"
---
# <a name="change-failover-priority-or-trigger-failover-for-an-azure-cosmos-account-with-single-write-region-by-using-powershell"></a>PowerShell kullanarak tek bir yazma bölgesi ile bir Azure Cosmos hesabı için yük devretme önceliğini değiştirme veya yük devretmeyi tetikleme

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Bir bölgedeki herhangi bir değişiklik, `failoverPriority=0` el ile yük devretme tetikler ve yalnızca el ile yük devretme için yapılandırılmış bir hesaba yapılabilir. Diğer tüm bölgelerde yapılan değişiklikler yalnızca Cosmos hesabının yük devretme önceliğini değiştirir.
> [!NOTE]
> Bu örnekte bir SQL (Core) API hesabı kullanılması gösterilmektedir. Bu örneği diğer API 'Ler için kullanmak üzere ilgili özellikleri kopyalayın ve API 'ye özel betiğe uygulayın

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-failover-priority-update.ps1 "Update failover priority for an Azure Cosmos account or trigger a manual failover")]

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
| [Update-AzCosmosDBAccountFailoverPriority](https://docs.microsoft.com/powershell/module/az.cosmosdb/update-azcosmosdbaccountfailoverpriority) | Cosmos DB hesabının bölgelerinin yük devretme önceliği sırasını güncelleştirin. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
