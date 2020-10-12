---
title: Aynı hesapta bir VHD dosyasından yönetilen disk oluşturma-CLı örneği
description: Azure CLI Betik Örneği - Aynı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturma
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.subservice: disks
ms.devlang: azurecli
ms.topic: sample
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: d0795f67390faec4a08882adb1f2b15efcbc6619
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91274886"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli-linux"></a>CLı ile aynı abonelikte bir depolama hesabındaki bir VHD dosyasından yönetilen disk oluşturma (Linux)

Bu betik aynı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturur. Bir sanal makine oluşturmak üzere yönetilen işletim sistemi diskine özelleştirilmiş (genelleştirilmemiş/sysprep uygulanmamış) bir VHD’yi almak için bu betiği kullanın. Veya yönetilen veri diskine bir veri VHD’si almak için kullanın. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir VHD’den yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk create](/cli/azure/disk) | Aynı abonelikteki bir depolama hesabında bir VHD URI’sini kullanarak yönetilen disk oluşturur |

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
