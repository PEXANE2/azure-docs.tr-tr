---
title: Anlık görüntüden yönetilen disk oluşturma-PowerShell örneği
description: Azure PowerShell Betiği Örneği - Anlık görüntüden yönetilen disk oluşturma
services: virtual-machines
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.subservice: disks
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: f3a890083e4763bcff95a1361bed69907b80e033
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323199"
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>PowerShell ile anlık görüntüden yönetilen disk oluşturma 

Bu betik bir anlık görüntüden yönetilen disk oluşturur. İşletim sistemi anlık görüntülerinden ve veri disklerinden bir sanal makineyi geri yüklemek için bu betiği kullanın. İlgili anlık görüntülerden işletim sistemi ve veri diskleri oluşturun ve sonra yönetilen diskleri ekleyerek yeni bir sanal makine oluşturun. Ayrıca, anlık görüntülerden oluşturulan veri disklerini ekleyerek mevcut bir VM'nin veri disklerini geri yükleyebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir anlık görüntüden yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | Anlık görüntü özelliklerini alır.  |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Disk oluşturmak için kullanılan disk yapılandırmasını oluşturur. Üst anlık görüntünün kaynak kimliğini, üst anlık görüntünün konumuyla aynı olan konumu ve depolama türünü içerir.  |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Parametre olarak geçirilen disk yapılandırmasını, disk adını ve kaynak grubu adını kullanarak bir disk oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
