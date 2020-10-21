---
title: Azure Cosmos MongoDB API veritabanı ve koleksiyonu için kaynak kilidi oluşturmak üzere PowerShell betiği
description: Azure Cosmos MongoDB API veritabanı ve koleksiyonu için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: ac67e7a8e5575bcbca451335fb0ea837fb70d3d3
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92282728"
---
# <a name="create-a-resource-lock-for-azure-cosmos-mongodb-api-database-and-collection-using-azure-powershell"></a>Azure PowerShell kullanarak Azure Cosmos MongoDB API veritabanı ve koleksiyonu için bir kaynak kilidi oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Kaynak kilitleri, hiçbir MongoDB SDK, Mongoshell, herhangi bir araç veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz, çünkü Cosmos DB hesap, etkin özellik ile kilitlenmez `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-lock.ps1 "Create, list, and remove resource locks")]

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
| [New-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcelock) | Kaynak kilidi oluşturur. |
| [Get-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcelock) | Bir kaynak kilidi alır veya kaynak kilitlerini listeler. |
| [Remove-AzResourceLock](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcelock) | Kaynak kilidini kaldırır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).
