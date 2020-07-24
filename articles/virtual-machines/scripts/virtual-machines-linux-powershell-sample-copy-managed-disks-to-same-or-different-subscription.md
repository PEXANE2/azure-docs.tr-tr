---
title: Yönetilen diskleri aboneliğe kopyalama (Linux)-PowerShell örneği
description: Azure PowerShell betik örneği-yönetilen diskleri aynı veya farklı bir aboneliğe kopyalama (veya taşıma)
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
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: ca594801c0886903f395dddb7ccbfccb42bff15f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87069380"
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell-linux"></a>Yönetilen diskleri aynı abonelikte veya PowerShell ile farklı bir abonelikte kopyalama (Linux)

Bu betik, mevcut bir yönetilen diskin aynı abonelikte veya farklı abonelikte bir kopyasını oluşturur. Yeni disk, ana yönetilen diskle aynı bölgede oluşturulur.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak yönetilen diskin kimliğini kullanarak hedef abonelikte yeni bir yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Disk oluşturmak için kullanılan disk yapılandırmasını oluşturur. Ana diskin kaynak kimliğini ve üst disk konumuyla aynı konumu içerir.  |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Parametre olarak geçirilen disk yapılandırmasını, disk adını ve kaynak grubu adını kullanarak bir disk oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
