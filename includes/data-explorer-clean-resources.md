---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
ms.date: 11/28/2019
ms.author: lugoldbe
ms.openlocfilehash: 9598724db630db7fc48545b58f2b0c1cad464aa4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74667838"
---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin. 

### <a name="clean-up-resources-using-the-azure-portal"></a>Azure portalını kullanarak kaynakları temizleme

[Kaynakları temizleme](../articles/data-explorer/create-cluster-database-portal.md#clean-up-resources)adımlarını izleyerek Azure portalındaki kaynakları silin.

### <a name="clean-up-resources-using-powershell"></a>PowerShell kullanarak kaynakları temizleme

Bulut Kabuğu hala açıksa, ilk satırı kopyalamanız/çalıştırmanız gerekmez (Oku-Ana Bilgisayar).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```