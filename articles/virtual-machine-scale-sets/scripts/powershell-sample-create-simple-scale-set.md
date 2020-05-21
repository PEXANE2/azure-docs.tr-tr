---
title: Azure PowerShell örnekleri-temel bir sanal makine ölçek kümesi oluşturma
description: Bu betik, RDP aracılığıyla erişebileceğiniz Windows Server 2016 çalıştıran bir Azure sanal makine ölçek kümesi oluşturur.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: powershell
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 3d49212e6d35e2a348a7e31ff7d2f0ad49eabfc2
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83699508"
---
# <a name="create-a-basic-virtual-machine-scale-set-with-powershell"></a>PowerShell ile temel bir sanal makine ölçek kümesi oluşturma
Bu betik, Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur. Betiği çalıştırdıktan sonra sanal makine örneklerine RDP üzerinden erişebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik


[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.ps1 "Create a simple virtual machine scale set")]

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
