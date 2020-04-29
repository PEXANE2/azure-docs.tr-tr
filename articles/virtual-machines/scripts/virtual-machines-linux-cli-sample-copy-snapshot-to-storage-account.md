---
title: Bir anlık görüntüyü başka bir bölgedeki bir depolama hesabına kopyalama-CLı örneği
description: Azure CLı betik örneği-aynı veya farklı bölgedeki bir depolama hesabına VHD olarak anlık görüntü gönderme/kopyalama.
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: 42a516f3c52d7548d08f72687e9f233620a39287
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81458629"
---
# <a name="exportcopy-a-snapshot-to-a-storage-account-in-different-region-with-cli"></a>CLı ile farklı bölgedeki bir anlık görüntüyü bir depolama hesabına aktarma/kopyalama

Bu betik farklı bölgedeki bir depolama hesabına yönetilen bir anlık görüntü gönderir. İlk olarak anlık görüntünün SAS URI'sini oluşturur ve sonra onu farklı bölgede bulunan bir depolama hesabına kopyalamak için kullanır. Olağanüstü durum kurtarma amacıyla farklı bölgede bulunan yönetilen disklerinizin yedeğini tutmak için bu betiği kullanın. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen anlık görüntünün SAS URI'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI'sini kullanarak anlık görüntüyü bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az snapshot grant-access](https://docs.microsoft.com/cli/azure/snapshot) | Temel alınan VHD dosyasını bir depolama hesabına kopyalamak veya şirket içine indirmek üzere kullanılan salt okunur SAS oluşturur  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy) | Bir blobu bir depolama hesabından diğerine zaman uyumsuz olarak kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[VHD'den yönetilen disk oluşturma](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
