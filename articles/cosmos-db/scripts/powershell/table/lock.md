---
title: Azure Cosmos Tablo API'si tablosu için kaynak kilidi oluşturmak üzere PowerShell betiği
description: Azure Cosmos Tablo API'si tablosu için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: a56846e3bfbc9e5392c509ff1f484b8109ce9eac
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098799"
---
# <a name="create-a-resource-lock-for-azure-cosmos-table-api-table-using-azure-powershell"></a>Azure PowerShell kullanarak Azure Cosmos Tablo API'si tablosu için kaynak kilidi oluşturma
[!INCLUDE[appliesto-table-api](../../../includes/appliesto-table-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-PowerShell-install](../../../../../includes/sample-PowerShell-install-no-ssh.md)]

> [!IMPORTANT]
> Kaynak kilitleri, Cosmos DB hesabı etkinleştirilmiş özellik ile ilk kez kilitlenmediği takdirde, herhangi bir Cosmos DB SDK 'yı, hesap anahtarları aracılığıyla bağlanan araçları veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-PowerShell[main](../../../../../PowerShell_scripts/cosmosdb/table/ps-table-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```PowerShell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure kaynağı**| |
| [New-AzResourceLock](/PowerShell/module/az.resources/new-azresourcelock) | Kaynak kilidi oluşturur. |
| [Get-AzResourceLock](/PowerShell/module/az.resources/get-azresourcelock) | Bir kaynak kilidi alır veya kaynak kilitlerini listeler. |
| [Remove-AzResourceLock](/PowerShell/module/az.resources/remove-azresourcelock) | Kaynak kilidini kaldırır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/PowerShell/).