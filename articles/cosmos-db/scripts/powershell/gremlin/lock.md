---
title: Azure Cosmos Gremlin API veritabanı ve Graf için kaynak kilidi oluşturmak üzere PowerShell betiği
description: Azure Cosmos Gremlin API veritabanı ve Graf için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: 69b8642c84dcb360e5ad162dbe708243a31b79e2
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93085369"
---
# <a name="create-a-resource-lock-for-azure-cosmos-gremlin-api-database-and-graph-using-azure-powershell"></a>Azure Cosmos Gremlin API veritabanı ve Azure PowerShell kullanarak grafik için bir kaynak kilidi oluşturma
[!INCLUDE[appliesto-gremlin-api](../../../includes/appliesto-gremlin-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Kaynak kilitleri, Cosmos DB hesap ilk olarak etkinleştirilmiş özelliği ile kilitlenmediği takdirde, herhangi bir Gremlin SDK veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/gremlin/ps-gremlin-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure kaynağı**| |
| [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) | Kaynak kilidi oluşturur. |
| [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock) | Bir kaynak kilidi alır veya kaynak kilitlerini listeler. |
| [Remove-AzResourceLock](/powershell/module/az.resources/remove-azresourcelock) | Kaynak kilidini kaldırır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).