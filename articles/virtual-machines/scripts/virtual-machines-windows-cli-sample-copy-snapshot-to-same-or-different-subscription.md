---
title: Yönetilen diskin anlık görüntüsünü bir abonelik-CLı örneğine kopyalama, Windows VM
description: Azure CLı betik örneği-bir Windows sanal makinesinde yönetilen bir diskin aynı veya farklı bir aboneliğe sahip olarak kopyasını kopyalama (taşıma)
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 05b73bf62bfce456e65fde153379690bc9823e6a
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88056746"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli-on-a-windows-vm"></a>Bir Windows VM 'de yönetilen diskin anlık görüntüsünü CLı ile aynı veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diskin anlık görüntüsünü aynı veya farklı bir aboneliğe kopyalar. Aşağıdaki senaryolar için bu betiği kullanın:

1. Maliyetinizi azaltmak için Premium depolamada (Premium_LRS) bir anlık görüntüyü standart depolamaya (Standard_LRS veya Standard_ZRS) geçirin.
1. ZRS depolamanın daha yüksek güvenilirliğine faydalanmak için yerel olarak yedekli depolama alanından (Premium_LRS, Standard_LRS) bir anlık görüntüyü bölge yedekli depolama alanına (Standard_ZRS) geçirin.
1. Daha uzun bekletme için bir anlık görüntüyü aynı bölgedeki farklı bir aboneliğe taşıyın.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az snapshot show](/cli/azure/snapshot) | Anlık görüntünün kaynak ve grup özelliklerini kullanarak tüm özelliklerini alır. Anlık görüntüyü farklı aboneliğe kopyalamak için kimlik özelliği kullanılır.  |
| [az snapshot create](/cli/azure/snapshot) | Üst anlık görüntünün kimlik ve adı ile farklı bir abonelikte anlık görüntü oluşturarak anlık görüntüyü kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine ve yönetilen diskler CLı betiği örnekleri, [Azure WINDOWS VM belgelerinde](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bulunabilir.
