---
title: Azure'da bir VHD anlık görüntüsü oluşturma
description: Yedekleme veya sorun giderme sorunları için Azure'da vhd'nin bir kopyasını nasıl oluşturup oluşturabilirsiniz öğrenin.
author: roygara
manager: twooley
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 07/11/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: f2d905d041198bd35c4657052bf18d981904dc44
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758409"
---
# <a name="create-a-snapshot"></a>Anlık görüntü oluşturma 

Yedekleme için bir işletim sistemi veya veri diskinin anlık görüntüsünü alın veya VM sorunlarını giderin. Anlık görüntü, bir VHD'nin tam, salt okunur kopyasıdır. 

## <a name="use-azure-cli"></a>Azure CLI kullanma 

Aşağıdaki örnek, Cloud [Shell'i](https://shell.azure.com/bash) kullanmanızı veya Azure CLI'sinin yüklenmesini gerektirir.

Aşağıdaki adımlar, **kaynak-disk** parametresi ile **az anlık görüntü oluşturma** komutunu kullanarak anlık görüntü nün nasıl alındığını gösterir. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda *myVM* adında bir VM olduğunu varsayar.

[az vm show](/cli/azure/vm#az-vm-show)kullanarak disk kimliğini alın.

```azurecli-interactive
osDiskId=$(az vm show \
   -g myResourceGroup \
   -n myVM \
   --query "storageProfile.osDisk.managedDisk.id" \
   -o tsv)
```

[Az anlık görüntü oluşturmayı](/cli/azure/snapshot#az-snapshot-create)kullanarak *osDisk yedekleme* adlı bir anlık görüntü alın.

```azurecli-interactive
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDisk-backup
```

> [!NOTE]
> Anlık görüntünüzü bölgeye dayanıklı depolamaalanında depolamak istiyorsanız, anlık durumu [destekleyen](../../availability-zones/az-overview.md) bir bölgede oluşturmanız ve **--sku Standard_ZRS** parametresini eklemeniz gerekir.

[Az anlık görüntü listesini](/cli/azure/snapshot#az-snapshot-list)kullanarak anlık görüntülistesini görebilirsiniz.

```azurecli-interactive
az snapshot list \
   -g myResourceGroup \
   - table
```

## <a name="use-azure-portal"></a>Azure portalı kullanma 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üstten başlayarak kaynak **oluştur'u** tıklatın ve **anlık görüntü**arayın. Arama sonuçlarından **Anlık Görüntü'nü** seçin.
3. Anlık **Görüntü** çubuğunda **Oluştur'u**tıklatın.
4. Anlık görüntü için bir **Ad** girin.
5. Varolan bir kaynak grubu seçin veya yeni bir kaynak için ad yazın. 
7. **Kaynak disk**için, anlık görüntü için yönetilen diski seçin.
8. Anlık görüntüdepolamak için kullanılacak **Hesap türünü** seçin. Yüksek performanslı bir SSD'de depolanmış olması gerekmedikçe **Standart HDD'yi** kullanın.
9. **Oluştur'u**tıklatın.


## <a name="next-steps"></a>Sonraki adımlar

 Anlık görüntüden yönetilen bir disk oluşturup yeni yönetilen diski işletim sistemi diski olarak ekleyerek anlık görüntüden sanal bir makine oluşturun. Daha fazla bilgi için anlık görüntü komut [dosyasından VM oluştur'a](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) bakın.

