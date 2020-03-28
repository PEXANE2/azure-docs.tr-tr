---
title: Yönetilen bir diskin anlık görüntüsünü aboneye kopyalama - PowerShell Sample
description: Azure PowerShell Komut Dosyası Örneği - Yönetilen bir diskin anlık görüntüsünü aynı veya farklı aboneye kopyalama (veya taşıma)
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 65a3e39206864f10c41e79ba6b3e7a89da99dc6f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75463783"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>PowerShell ile bir yönetilen diskin anlık görüntüsünü aynı aboneliğe veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diskin anlık görüntüsünü aynı veya farklı bir aboneliğe kopyalar. Aşağıdaki senaryolar için bu komut dosyasını kullanın:

1. Maliyetinizi azaltmak için Premium depolama alanında (Premium_LRS) standart depolama alanına (Standard_LRS veya Standard_ZRS) anlık görüntü geçirin.
1. ZRS depolamasının daha yüksek güvenilirliğinden yararlanmak için yerel olarak yedekdepolamadan (Premium_LRS, Standard_LRS) bölge yedekdepolamasına (Standard_ZRS) anlık görüntü geçirin.
1. Daha uzun bekletme için anlık görüntünün aynı bölgedeki farklı abonelmeye taşınması.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Yeni-AzSnapshotConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzSnapshotConfig) | Anlık görüntü oluşturmak için kullanılan anlık görüntü yapılandırmasını oluşturur. Üst anlık görüntünün kaynak kimliğini ve üst anlık görüntünün konumuyla aynı olan konumu içerir.  |
| [Yeni-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Parametre olarak geçirilen anlık görüntü yapılandırmasını, anlık görüntü adını ve kaynak grubu adını kullanarak bir anlık görüntü oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden sanal makine oluşturma](./virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.