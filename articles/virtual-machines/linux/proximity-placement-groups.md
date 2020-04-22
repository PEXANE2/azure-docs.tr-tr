---
title: Yakınlık yerleşim gruplarını kullanma
description: Azure'daki sanal makineler için yakınlık yerleşim grupları oluşturma ve kullanma hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: cynthn
ms.openlocfilehash: f89b28e7a3c29e45efa2796788e27325c01d7098
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759259"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Azure CLI'yi kullanarak VM'leri yakınlık yerleşim gruplarına dağıtma

VM'leri mümkün olduğunca yakın hale getirmek ve mümkün olan en düşük gecikmeyi elde etmek için, bunları bir [yakınlık yerleşim grubu](co-location.md#proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşim grubu, Azure hesaplama kaynaklarının fiziksel olarak birbirine yakın olduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimgrupları, düşük gecikme nin bir gereklilik olduğu iş yükleri için yararlıdır.


## <a name="create-the-proximity-placement-group"></a>Yakınlık yerleşim grubunu oluşturma
Bir yakınlık yerleşim grubu [`az ppg create`](/cli/azure/ppg#az-ppg-create)kullanarak oluşturun. 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Yakınlık yerleşim gruplarını listele

Az [ppg listesini](/cli/azure/ppg#az-ppg-list)kullanarak tüm yakınlık yerleştirme gruplarınızı listeleyebilirsiniz.

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>VM oluşturma

[Yeni az vm](/cli/azure/vm#az-vm-create)kullanarak yakınlık yerleşim grubu içinde bir VM oluşturun.

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Az [ppg show](/cli/azure/ppg#az-ppg-show)kullanarak yakınlık yerleşim grubunda VM görebilirsiniz.

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Kullanılabilirlik Kümeleri
Ayrıca, yakınlık yerleşim grubunuzun içinde bir kullanılabilirlik kümesi de oluşturabilirsiniz. Kullanılabilirlik `--ppg` kümesi oluşturmak için [az vm kullanılabilirlik kümesi ile](/cli/azure/vm/availability-set#az-vm-availability-set-create) aynı parametreyi kullanın ve kullanılabilirlik kümesindeki tüm VM'ler de aynı yakınlık yerleşim grubunda oluşturulur.

## <a name="scale-sets"></a>Ölçek kümeleri

Ayrıca, yakınlık yerleşim grubunda bir ölçek kümesi de oluşturabilirsiniz. Bir ölçek `--ppg` kümesi oluşturmak için [az vmss oluşturmak](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create) ile aynı parametreyi kullanın ve tüm örnekleri aynı yakınlık yerleşim grubunda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Yakınlık yerleşim grupları için [Azure CLI](/cli/azure/ppg) komutları hakkında daha fazla bilgi edinin.
