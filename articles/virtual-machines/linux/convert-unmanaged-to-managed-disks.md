---
title: Yönetilen disklerden yönetilen disklere Linux VM dönüştürme
description: Azure CLI kullanarak yönetilen disklerden yönetilen disklere Linux VM dönüştürme.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 12/15/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 2774dcbd5fc5b01627b965c2c02d870412c8bf77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969686"
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Yönetilen disklerden yönetilen disklere Linux sanal makinesini dönüştürme

Yönetilmeyen diskler kullanan mevcut Linux sanal makineleriniz (VM'ler) varsa, Sanal Kullanımlı [Diskleri](../linux/managed-disks-overview.md)kullanmak için Sanal M'leri dönüştürebilirsiniz. Bu işlem hem işletim sistemi diskini hem de ekteki tüm veri disklerini dönüştürür.

Bu makalede, Azure CLI'yi kullanarak VM'leri nasıl dönüştürersiniz gösterilmektedir. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın. 

## <a name="before-you-begin"></a>Başlamadan önce
* [Yönetilen Disklere geçiş le ilgili SSS'yi gözden geçirin.](faq-for-disks.md#migrate-to-managed-disks)

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]

* Özgün VHD’ler ve dönüştürme öncesinde VM tarafından kullanılan depolama hesabı silinmez. Ücretler uygulanmaya devam eder. Bunlar için ücret alınmasını önlemek istiyorsanız, dönüştürmenin tamamlandığını doğruladıktan sonra özgün VHD bloblarını silin. Bu eklenmemiş diskleri silmek için bulmanız gerekiyorsa, makalemize bakın [Eklenmemiş Azure yönetilen ve yönetilmeyen diskleri bulun ve silin.](find-unattached-disks.md)

## <a name="convert-single-instance-vms"></a>Tek örnekli VM'leri dönüştürme
Bu bölümde, tek örnekli Azure VM'lerin yönetilen disklerden yönetilen disklere nasıl dönüştürülileceği ele alilmektedir. (VM'leriniz kullanılabilirlik kümesindeyse, bir sonraki bölüme bakın.) Bu işlemi, VM'leri premium (SSD) yönetilmeyen disklerden premium yönetilen disklere veya standart (HDD) yönetilmeyen disklerden standart yönetilen disklere dönüştürmek için kullanabilirsiniz.

1. Deallocate [az vm deallocate](/cli/azure/vm)kullanarak VM buluntun. Aşağıdaki örnek, kaynak grubunda adı `myVM` geçen VM'yi yerle `myResourceGroup`bir eder:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Az vm convert kullanarak VM'yi yönetilen disklere [dönüştürün.](/cli/azure/vm) Aşağıdaki işlem, işletim sistemi `myVM`diski ve tüm veri diskleri de dahil olmak üzere VM adlı dönüştürür:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Az vm başlat'ı kullanarak yönetilen disklere dönüştürmeden sonra [VM'yi başlatın.](/cli/azure/vm) Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adı geçen VM'yi başlatır.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Kullanılabilirlik kümesinde VM'leri dönüştürme

Yönetilen disklere dönüştürmek istediğiniz VM'ler kullanılabilirlik kümesindeyse, öncelikle kullanılabilirlik kümesini yönetilen kullanılabilirlik kümesine dönüştürmeniz gerekir.

Kullanılabilirlik kümesindeki tüm VM'ler, kullanılabilirlik kümesini dönüştürmeden önce devre denilmelidir. Kullanılabilirlik kümesi nin kendisi yönetilen kullanılabilirlik kümesine dönüştürüldükten sonra tüm VM'leri yönetilen disklere dönüştürmeyi planlayın. Ardından, tüm VM'leri başlatın ve normal şekilde çalışmaya devam edin.

1. [Az vm kullanılabilirlik ayar listesini](/cli/azure/vm/availability-set)kullanarak bir kullanılabilirlik kümesindeki tüm VM'leri listele. Aşağıdaki örnekte, adlı `myAvailabilitySet` `myResourceGroup`kaynak grubunda adı geçen kullanılabilirlik kümesindeki tüm VM'ler listelenir:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Deallocate [az vm deallocate](/cli/azure/vm)kullanarak tüm VM'ler. Aşağıdaki örnek, kaynak grubunda adı `myVM` geçen VM'yi yerle `myResourceGroup`bir eder:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. [Az vm kullanılabilirlik kümesi ni](/cli/azure/vm/availability-set)kullanarak ayarlanan kullanılabilirliği dönüştürün. Aşağıdaki örnek, adlı `myAvailabilitySet` `myResourceGroup`kaynak grubunda adı geçen kullanılabilirlik kümesini dönüştürür:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Az vm convert kullanarak tüm VM'leri yönetilen disklere [dönüştürün.](/cli/azure/vm) Aşağıdaki işlem, işletim sistemi `myVM`diski ve tüm veri diskleri de dahil olmak üzere VM adlı dönüştürür:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. [Az vm başlat'ı](/cli/azure/vm)kullanarak yönetilen disklere dönüştürmeden sonra tüm VM'leri başlatın. Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adı geçen VM'yi başlatır:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-using-the-azure-portal"></a>Azure portalını kullanarak dönüştürme

Ayrıca, Azure portalını kullanarak yönetilmeyen diskleri yönetilen disklere dönüştürebilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Portaldaki VM listesinden VM'yi seçin.
3. VM için bıçakta menüden **Diskler'i** seçin.
4. **Diskler** bıçağının üst kısmında, **yönetilen disklere Geçir'i**seçin.
5. VM'niz bir kullanılabilirlik kümesindeyse, yönetilen **disklere Geçir** bilgili bıçak ta önce kullanılabilirlik kümesini dönüştürmeniz gerektiğine dair bir uyarı olacaktır. Uyarı, kullanılabilirlik kümesini dönüştürmek için tıklatabileceğiniz bir bağlantıya sahip olmalıdır. Kullanılabilirlik kümesi dönüştürüldükten veya VM'niz kullanılabilirlik kümesinde değilse, disklerinizi yönetilen disklere geçirme işlemini başlatmak için **Geçir'i** tıklatın.

Geçiş tamamlandıktan sonra VM durdurulur ve yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar

Depolama seçenekleri hakkında daha fazla bilgi için [Azure Yönetilen Diskler'e genel bakış](../windows/managed-disks-overview.md)bilgisi ne bürünmüştür.
