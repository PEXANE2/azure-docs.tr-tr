---
title: Azure CLI Betik Örneği - İşletim sistemi diskini bağlama
description: Azure CLI Betik Örneği - İşletim sistemi diskini bağlama
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 0e5cabdf9c69ead9a2d7f497aceec183cf0be900
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509673"
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>VM işletim sistemi diski ile ilgili sorun giderme

Bu betik, hatalı veya sorunlu bir sanal makinenin işletim sistemi diskini ikinci bir sanal makineye veri diski olarak bağlar. Disk sorunlarını giderirken veya verileri kurtarırken bu yöntem kullanışlı olabilir.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az vm show](/cli/azure/vm) | Sanal makinelerin listesini döndürür. Bu örnekte sanal makine işletim sistemi diskini döndürmek için sorgu seçeneği kullanılır. Bu değer daha sonra 'uri' değişken adına eklenir. |
| [az vm delete](/cli/azure/vm) | Bir sanal makineyi siler. |
| [az vm create](/cli/azure/vm) | Bir sanal makine oluşturur.  |
| [az vm disk attach](/cli/azure/vm/disk) | Bir diski sanal makineye ekler. |
| [az vm list-ip-addresses](/cli/azure/vm) | Bir sanal makinenin IP adreslerini döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
