---
title: Azure PowerShell örnekleri-özel bir VM görüntüsü kullanma
description: Bu betik, VM örnekleri kaynağı olarak özel bir VM görüntüsü kullanan bir sanal makine ölçek kümesi oluşturur.
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: sample
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2a272cf534ef8ac9cbc45c05f1b9d3f0fecfa1f3
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76276656"
---
# <a name="create-a-virtual-machine-scale-set-from-a-custom-vm-image-with-powershell"></a>PowerShell ile özel bir VM görüntüsünden sanal makine ölçek kümesi oluşturma
Bu betik, VM örnekleri kaynağı olarak özel bir VM görüntüsü kullanan bir sanal makine ölçek kümesi oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/use-custom-vm-image/use-custom-vm-image.ps1 "Create a virtual machine scale set with a custom VM image")]

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
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.
