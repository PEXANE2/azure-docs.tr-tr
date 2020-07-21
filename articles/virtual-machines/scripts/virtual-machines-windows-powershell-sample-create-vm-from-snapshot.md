---
title: Anlık görüntüden VM oluşturma (Windows)-PowerShell örneği
description: Azure PowerShell betik örneği-anlık görüntüden VM oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 7f8e7f5e758c916cf7e6b96ab38607ee722152b1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509567"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell-windows"></a>PowerShell ile anlık görüntüden sanal makine oluşturma (Windows)

Bu betik bir işletim sistemi diskinin anlık görüntüsünden sanal makine oluşturur. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, anlık görüntü özelliklerini almak, anlık görüntüden yönetilen disk oluşturmak ve VM oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | Anlık görüntü adını kullanarak bir anlık görüntü alır. |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Disk yapılandırması oluşturur. Bu yapılandırma, disk oluşturma işlemiyle birlikte kullanılır. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Yönetilen bir disk oluşturur. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) | Yönetilen diski işletim sistemi diski olarak sanal makineye iliştirir |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-Aznetworkınterface](/powershell/module/az.network/new-aznetworkinterface) | Ağ arabirimi oluşturur. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Bir sanal makine oluşturur. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
