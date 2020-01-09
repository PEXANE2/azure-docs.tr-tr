---
title: Azure PowerShell örnekleri-ana bilgisayar tabanlı otomatik ölçeklendirmeyi etkinleştir
description: Bu betik, CPU yükü değiştikçe otomatik olarak ölçeklendirmek için Windows Server 2016 çalıştıran ve ana bilgisayar tabanlı ölçümler kullanan bir sanal makine ölçek kümesi oluşturur.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 92411785419a1544b2d172d3a18661d501d4e5cf
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75350831"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-powershell"></a>PowerShell ile sanal makine ölçek kümesini otomatik olarak ölçeklendirme
Bu betik, CPU yükü değiştikçe otomatik olarak ölçeklendirmek için Windows Server 2016 çalıştıran ve ana bilgisayar tabanlı ölçümler kullanan bir sanal makine ölçek kümesi oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik


[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/auto-scale-host-metrics/auto-scale-host-metrics.ps1 "Automatically scale a virtual machine scale set")]

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
| [Güncelleştirme-AzVmss](/powershell/module/az.compute/update-azvmss) | VM uzantısını uygulamak için sanal makine ölçek kümesi modelini güncelleştirir. |
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Yük dengeleyici tarafından kullanılan atanan genel IP adresi hakkında bilgi alır. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.
