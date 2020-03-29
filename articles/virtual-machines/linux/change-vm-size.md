---
title: Azure CLI ile Bir Linux VM yeniden boyutlandırma
description: VM boyutunu değiştirerek bir Linux sanal makineyi nasıl ölçeklendirecek veya küçültün.
author: mikewasson
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 02/10/2017
ms.author: mwasson
ms.openlocfilehash: 20e7db80b55347c4a4a76b7c95d4d8bec368abda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969271"
---
# <a name="resize-a-linux-virtual-machine-using-azure-cli"></a>Azure CLI kullanarak Linux Sanal Makinesini yeniden boyutlandırma 

Sanal bir makine (VM) temin ettikten sonra, [VM boyutunu][vm-sizes]değiştirerek VM'yi yukarı veya aşağı ölçeklendirebilirsiniz. Bazı durumlarda, önce VM'nin yerini tespit etmelidir. VM'yi barındıran donanım kümesinde istenilen boyut yoksa VM'yi bulmanız gerekir. Bu makalede, Azure CLI ile bir Linux VM yeniden boyutlandırma küçlük. 

## <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma
Bir VM'yi yeniden boyutlandırmak için, [az giriş](/cli/azure/reference-index)kullanarak en son [Azure CLI'nin](/cli/azure/install-az-cli2) yüklü ve azure hesabına giriş yapmanız gerekir.

1. VM az [vm list-vm-resize-seçenekleri](/cli/azure/vm)ile barındırılan donanım kümesinde kullanılabilir VM boyutlarının listesini görüntüleyin. Aşağıdaki örnekte, kaynak grubu `myVM` `myResourceGroup` bölgesinde adı geçen VM için VM boyutları listelenebilmiştir:
   
    ```azurecli
    az vm list-vm-resize-options --resource-group myResourceGroup --name myVM --output table
    ```

2. İstenilen VM boyutu listelenirse, [VM'yi az vm yeniden boyutlandırarak yeniden boyutlandırın.](/cli/azure/vm) Aşağıdaki örnek, adı verilen `myVM` VM'yi `Standard_DS3_v2` boyuta yeniden boyutlandırıyor:
   
    ```azurecli
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    ```
   
    Bu işlem sırasında VM yeniden başlatılır. Yeniden başlatmadan sonra, varolan işletim sistemi ve veri diskleriniz yeniden eşlenir. Geçici diskteki her şey kaybolur.

3. İstenilen VM boyutu listelenmemişse, öncelikle Az [vm deallocate](/cli/azure/vm)ile VM'yi bulmanız gerekir. Bu işlem, VM'nin daha sonra bölgenin desteklediği ve daha sonra başlatıldıdığı herhangi bir boyuta yeniden boyutlandırılmasına olanak tanır. Aşağıdaki adımlar, aşağıdaki leri yeniden boyutlandırmayı ve `myVM` ardından kaynak `myResourceGroup`grubunda adı geçen VM'yi başlatın:
   
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm resize --resource-group myResourceGroup --name myVM --size Standard_DS3_v2
    az vm start --resource-group myResourceGroup --name myVM
    ```
   
   > [!WARNING]
   > VM'nin ayrılması, VM'ye atanan dinamik IP adreslerini de serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez.

## <a name="next-steps"></a>Sonraki adımlar
Ek ölçeklenebilirlik için birden çok VM örneği çalıştırın ve ölçeklendirin. Daha fazla bilgi için bkz. [Sanal Makine Ölçeği Kümesi'ndeki Linux makinelerini otomatik olarak ölçeklendirin.][scale-set] 

<!-- links -->
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]:sizes.md
