---
title: Aynı abonelikteki bir depolama hesabındaki bir VHD dosyasından yönetilen bir disk oluşturma - CLI Sample
description: Azure CLI Betik Örneği - Aynı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturma
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
ms.openlocfilehash: 9f7126d98b5b769ecf87608861b43af8af9dcaa4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75375840"
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>CLI kullanarak aynı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturma

Bu betik aynı abonelikteki bir depolama hesabında VHD dosyasından yönetilen disk oluşturur. Bir sanal makine oluşturmak üzere yönetilen işletim sistemi diskine özelleştirilmiş (genelleştirilmemiş/sysprep uygulanmamış) bir VHD’yi almak için bu betiği kullanın. Veya yönetilen veri diskine bir veri VHD’si almak için kullanın.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir VHD’den yönetilen disk oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk create](https://docs.microsoft.com/cli/azure/disk) | Aynı abonelikteki bir depolama hesabında bir VHD URI’sini kullanarak yönetilen disk oluşturur |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine ve yönetilen diskler CLI komut dosyası örnekleri [Azure Windows VM belgelerinde](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bulunabilir.
