---
title: CLI ' kullanarak işletim sistemi diskini Azure VM ile değiştirme
description: CLI'yi kullanarak Azure sanal makinesi tarafından kullanılan işletim sistemi diskini değiştirin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: bfbe47fb68ffe7cee1ee2f9f7b94b418d8da2a1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74035340"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>CLI'yi kullanarak Azure VM tarafından kullanılan işletim sistemi diskini değiştirme


Varolan bir VM'iniz varsa, ancak diski yedek disk veya başka bir işletim sistemi diski ile değiştirmek istiyorsanız, işletim sistemi disklerini değiştirmek için Azure CLI'yi kullanabilirsiniz. VM'yi silmek ve yeniden oluşturmak zorunda değilsiniz. Zaten kullanılmamış olması durumunda yönetilen bir diski başka bir kaynak grubunda bile kullanabilirsiniz.

VM durdurulması\tahsis edilmesi gerekir, sonra yönetilen diskin kaynak kimliği farklı bir yönetilen diskin kaynak kimliği ile değiştirilebilir. 

VM boyutunun ve depolama türünün eklemek istediğiniz diskle uyumlu olduğundan emin olun. Örneğin, kullanmak istediğiniz disk Premium Depolama'daysa, VM'nin Premium Depolama özelliğine sahip olması gerekir (DS serisi boyutu gibi).

Bu makalede, Azure CLI sürümü 2.0.25 veya daha büyük gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 


Kaynak grubunuzdaki disklerin listesini almak için [az disk listesini](/cli/azure/disk) kullanın.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Diskleri değiştirmeden önce VM'yi durdurmak için [az vm stop'u](/cli/azure/vm) kullanın.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Parametre için yeni diskin tam kaynak kimliği `--osdisk` ile [az vm güncellemesini](/cli/azure/vm#az-vm-update) kullanın 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
[Az vm start](/cli/azure/vm)kullanarak VM'yi yeniden başlatın.

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Sonraki adımlar**

Bir diskin kopyasını oluşturmak için, [bir diskin Anlık Görüntüsü'ne](snapshot-copy-managed-disk.md)bakın.
