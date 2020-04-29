---
title: Azure Cosmos hesabının bölgelerini güncelleştirmek için PowerShell betiği
description: Azure PowerShell betik örneği-Azure Cosmos hesabının bölgelerini güncelleştirme
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/21/2020
ms.author: mjbrown
ms.openlocfilehash: 4a8e24b4445e2bf29b35e87600de260c523d4376
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80367577"
---
# <a name="update-an-azure-cosmos-accounts-regions-using-powershell"></a>PowerShell kullanarak bir Azure Cosmos hesabının bölgelerini güncelleştirme

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
|**Azure kaynakları**| |
| [Set-AzResource](https://docs.microsoft.com/powershell/module/az.resources/set-azresource) | Bir kaynağı güncelleştirin. |
|**Azure Kaynak grupları**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Cosmos DB PowerShell betiği örnekleri, [Azure Cosmos DB PowerShell betiklerinde](../../../powershell-samples.md) bulunabilir.
