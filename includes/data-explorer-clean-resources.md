---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
ms.date: 11/28/2019
ms.author: lugoldbe
ms.openlocfilehash: 9598724db630db7fc48545b58f2b0c1cad464aa4
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74667838"
---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. 

### <a name="clean-up-resources-using-the-azure-portal"></a>Azure portal kullanarak kaynakları Temizleme

[Kaynakları Temizleme](../articles/data-explorer/create-cluster-database-portal.md#clean-up-resources)bölümündeki adımları izleyerek Azure Portal kaynakları silin.

### <a name="clean-up-resources-using-powershell"></a>PowerShell kullanarak kaynakları temizleme

Cloud Shell hala açıksa, ilk satırı (Read-Host) kopyalamanız/çalıştırmanız gerekmez.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```