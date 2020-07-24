---
title: Birçok özdeş yönetilen disk (Linux) yapmak için VHD anlık görüntüsü-PowerShell
description: Azure PowerShell Betiği Örneği - Kısa sürede birden fazla aynı yönetilen disk oluşturmak için VHD’den anlık görüntü oluşturma
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 8a80056ed3368f6a2a5c4ad7fa31424524e824b2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079936"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell-linux"></a>PowerShell (Linux) ile kısa sürede birden fazla aynı yönetilen disk oluşturmak için VHD 'den anlık görüntü oluşturma

Bu betik, aynı veya farklı abonelikteki bir depolama hesabında VHD dosyasından bir anlık görüntü oluşturur. Özelleştirilmiş (genelleştirilmiş/sysprepped uygulanmış olmayan) bir VHD’yi bir anlık görüntüye içeri aktarmak ve sonra anlık görüntüyü kullanarak kısa sürede birden fazla aynı yönetilen diskler oluşturmak için bu betiği kullanın. Bir veri VHD’sini anlık görüntüye içeri aktarmak ve sonra anlık görüntüyü kullanarak kısa sürede birden fazla yönetilen disk oluşturmak için de bunu kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, farklı abonelikteki bir VHD’den yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Disk oluşturmak için kullanılan disk yapılandırmasını oluşturur. Üst VHD’nin depolandığı depolama hesabının depolama türünü, konumunu, kaynak kimliğini ve üst VHD’nin VHD URI’sini içerir. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Parametre olarak geçirilen disk yapılandırmasını, disk adını ve kaynak grubu adını kullanarak bir disk oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden yönetilen disk oluşturma](virtual-machines-linux-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
