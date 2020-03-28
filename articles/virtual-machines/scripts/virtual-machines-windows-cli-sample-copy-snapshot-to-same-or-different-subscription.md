---
title: Yönetilen bir diskin anlık görüntüsünü aboneye kopyalama - CLI Sample
description: Azure CLI Betik Örneği - CLI ile bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma)
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
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
ms.openlocfilehash: ddcf585af86a265410f9c2d424c85acf5abbc8fd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75375939"
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>CLI ile bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diskin anlık görüntüsünü aynı veya farklı bir aboneliğe kopyalar. Aşağıdaki senaryolar için bu komut dosyasını kullanın:

1. Maliyetinizi azaltmak için Premium depolama alanında (Premium_LRS) standart depolama alanına (Standard_LRS veya Standard_ZRS) anlık görüntü geçirin.
1. ZRS depolamasının daha yüksek güvenilirliğinden yararlanmak için yerel olarak yedekdepolamadan (Premium_LRS, Standard_LRS) bölge yedekdepolamasına (Standard_ZRS) anlık görüntü geçirin.
1. Daha uzun bekletme için anlık görüntünün aynı bölgedeki farklı abonelmeye taşınması.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot) | Anlık görüntünün kaynak ve grup özelliklerini kullanarak tüm özelliklerini alır. Anlık görüntüyü farklı aboneliğe kopyalamak için kimlik özelliği kullanılır.  |
| [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot) | Üst anlık görüntünün kimlik ve adı ile farklı bir abonelikte anlık görüntü oluşturarak anlık görüntüyü kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine ve yönetilen diskler CLI komut dosyası örnekleri [Azure Windows VM belgelerinde](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bulunabilir.
