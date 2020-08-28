---
title: Yönetilen diskleri bir abonelik-CLı örneğine kopyalama
description: Azure CLı betik örneği-yönetilen diskleri aynı veya farklı bir aboneliğe kopyalama (veya taşıma)
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.devlang: azurecli
ms.topic: sample
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ed593b37de556d5b62cfec6b726322d12f4d8104
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89052285"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>CLI ile yönetilen diskleri aynı veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diski aynı bölgedeki aynı veya farklı bir aboneliğe kopyalar. Kopyalama yalnızca abonelikler aynı Azure AD kiracısının parçası olduğunda işe yarar.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak yönetilen diskin kimliğini kullanarak hedef abonelikte yeni bir yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk show](/cli/azure/disk) | Yönetilen diskin kaynak ve grup özelliklerini kullanarak tüm özelliklerini alır. Yönetilen diski farklı aboneliğe kopyalamak için kimlik özelliği kullanılır.  |
| [az disk create](/cli/azure/disk) | Üst yönetilen diskin kimliği ve adı ile farklı abonelikte yeni bir yönetilen disk oluşturarak yönetilen diski kopyalar.  |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
