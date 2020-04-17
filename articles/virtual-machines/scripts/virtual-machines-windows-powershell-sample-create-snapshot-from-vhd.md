---
title: Birden çok aynı yönetilen disk oluşturmak için Bir VHD'den anlık görüntü oluşturma - PowerShell Sample
description: Azure PowerShell Betiği Örneği - Kısa sürede birden fazla aynı yönetilen disk oluşturmak için VHD’den anlık görüntü oluşturma
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 9da8adb786baebcb2e798c7ffb5998aca0f68265
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459262"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>PowerShell ile kısa sürede birden fazla aynı yönetilen disk oluşturmak için VHD’den anlık görüntü oluşturma

Bu betik, aynı veya farklı abonelikteki bir depolama hesabında VHD dosyasından bir anlık görüntü oluşturur. Özelleştirilmiş (genelleştirilmiş/sysprepped uygulanmış olmayan) bir VHD’yi bir anlık görüntüye içeri aktarmak ve sonra anlık görüntüyü kullanarak kısa sürede birden fazla aynı yönetilen diskler oluşturmak için bu betiği kullanın. Bir veri VHD’sini anlık görüntüye içeri aktarmak ve sonra anlık görüntüyü kullanarak kısa sürede birden fazla yönetilen disk oluşturmak için de bunu kullanın. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden yönetilen disk oluşturma](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
