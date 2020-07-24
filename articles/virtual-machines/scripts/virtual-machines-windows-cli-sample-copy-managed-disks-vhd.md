---
title: Yönetilen diskleri bir depolama hesabına kopyalama-Windows CLı örneği
description: Azure CLı örneği-yönetilen diskleri bir depolama hesabına aktarın veya kopyalayın.
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
ms.date: 09/17/2018
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: 17ca58a8c03def3565bf7e38099d84cf767d333c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87010113"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Azure CLı kullanarak yönetilen bir diski depolama hesabına aktarma/kopyalama

Bu betik yönetilen diskin VHD dosyasını aynı veya farklı bölgede bulunan bir depolama hesabına aktarır. İlk olarak yönetilen diskin SAS URI'sini oluşturur ve sonra onu VHD dosyasını bir depolama hesabına kopyalamak için kullanır. Bu betiği bölgesel genişleme için yönetilen disklerinizi kopyalama amacıyla kullanabilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen diskin SAS URI'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI'sini kullanarak VHD dosyasını bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk grant-access](/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Temel alınan VHD dosyasını bir depolama hesabına kopyalamak veya şirket içine indirmek üzere kullanılan salt okunur SAS oluşturur  |
| [az storage blob copy start](/cli/azure/storage/blob/copy) | Bir blobu bir depolama hesabından diğerine zaman uyumsuz olarak kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[VHD'den yönetilen disk oluşturma](virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine ve yönetilen diskler CLı betiği örnekleri, [Azure WINDOWS VM belgelerinde](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bulunabilir.
