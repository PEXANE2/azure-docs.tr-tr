---
title: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma) | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma)
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
ms.openlocfilehash: f7dbda75eaf403e9bd99dce63e5ed37118fd83ea
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70081374"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>PowerShell ile bir yönetilen diskin anlık görüntüsünü aynı aboneliğe veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diskin anlık görüntüsünü aynı veya farklı bir aboneliğe kopyalar. Aşağıdaki senaryolar için bu betiği kullanın:

1. Maliyetinizi azaltmak için Premium depolamada (Premium_LRS) bir anlık görüntüyü standart depolamaya (Standard_LRS veya Standard_ZRS) geçirin.
1. ZRS depolamanın daha yüksek güvenilirliğine faydalanmak için yerel olarak yedekli depolama alanından (Premium_LRS, Standard_LRS) bir anlık görüntüyü bölge yedekli depolama alanına (Standard_ZRS) geçirin.
1. Daha uzun bekletme için bir anlık görüntüyü aynı bölgedeki farklı bir aboneliğe taşıyın.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzSnapshotConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzSnapshotConfig) | Anlık görüntü oluşturmak için kullanılan anlık görüntü yapılandırmasını oluşturur. Üst anlık görüntünün kaynak kimliğini ve üst anlık görüntünün konumuyla aynı olan konumu içerir.  |
| [New-AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Parametre olarak geçirilen anlık görüntü yapılandırmasını, anlık görüntü adını ve kaynak grubu adını kullanarak bir anlık görüntü oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden sanal makine oluşturma](./virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.