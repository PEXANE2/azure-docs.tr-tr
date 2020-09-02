---
title: Bir abonelikte bir depolama hesabındaki VHD dosyasından yönetilen disk oluşturma-PowerShell örneği
description: Azure PowerShell Betiği Örneği - Aynı veya farklı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturma
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.topic: sample
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 1d743d840afc4505cdc39b258b21856b6c3e7aa9
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323195"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>PowerShell kullanarak aynı veya farklı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturma

Bu betik, aynı veya farklı abonelikteki bir depolama hesabında VHD dosyasından bir yönetilen disk oluşturur. Bir sanal makine oluşturmak üzere yönetilen işletim sistemi diskine özelleştirilmiş (genelleştirilmemiş/sysprep uygulanmamış) bir VHD’yi almak için bu betiği kullanın. Veya yönetilen veri diskine bir veri VHD’si içeri aktarmak için bunu kullanın. 

Kısa sürede bir VHD dosyasından birden fazla aynı yönetilen disk oluşturmayın. Bir vhd dosyasından yönetilen diskler oluşturmak için vhd dosyasının blob anlık görüntüsü oluşturulur ve sonra yönetilen diskler oluşturmak için kullanılır. Bir dakika içinde, azaltma nedeniyle disk oluşturma hatalarına neden olan yalnızca bir blob anlık görüntüsü oluşturulabilir. Bu azaltmayı önlemek için, bir [vhd dosyasındaki yönetilen anlık görüntü](virtual-machines-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) oluşturun ve sonra kısa sürede birden fazla yönetilen disk oluşturmak için yönetilen anlık görüntüyü kullanın. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, farklı abonelikteki bir VHD’den yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Disk oluşturmak için kullanılan disk yapılandırmasını oluşturur. Bu, üst VHD 'nin depolandığı depolama hesabının depolama türünü, konumunu, kaynak KIMLIĞINI ve üst VHD 'nin VHD URI 'sini içerir. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Parametre olarak geçirilen disk yapılandırmasını, disk adını ve kaynak grubu adını kullanarak bir disk oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma](virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
