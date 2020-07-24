---
title: Azure PowerShell örnekleri-veri disklerini Iliştirme ve kullanma
description: Bu betik bir Azure sanal makine ölçek kümesi oluşturur ve PowerShell kullanarak veri disklerini iliştirir ve hazırlar.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 06/25/2020
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: f67df4fa654c8b78e66f008d67ac8386f427b343
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87046239"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-powershell"></a>PowerShell ile sanal makine ölçek kümesi kullanarak veri diskleri ekleme ve kullanma
Bu betik, sanal makine ölçek kümesi oluşturur ve veri diskleri ekleyip hazırlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.ps1 "Create a virtual machine scale set with data disks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Sanal makine ölçek kümesi ve sanal ağ, yük dengeleyici ve NAT kurallarının dahil olduğu tüm destekleyici kaynakları oluşturur. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Sanal makine ölçek kümesi hakkında bilgi alır. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Özel Betik için sanal makine uzantısı ekleyerek temel bir web uygulaması yükler. |
| [Güncelleştirme-AzVmss](/powershell/module/az.compute/update-azvmss) | Sanal makine uzantısını uygulamak için sanal makine ölçek kümesi modelini güncelleştirir. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).
