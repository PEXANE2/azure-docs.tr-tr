---
title: Linux sanal makineleri için yakınlık Yerleşim grupları önizlemesini kullanma | Microsoft Docs
description: Azure 'da Linux sanal makineleri için yakınlık yerleşimi grupları oluşturma ve kullanma hakkında bilgi edinin.
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: a3009c9863d8322e3fe34bd99d64b93f0aa3d858
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385734"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Önizleme: Azure CLı kullanarak yakınlık yerleşimi gruplarına VM 'Leri dağıtma

Olası en düşük gecikme süresini elde etmek üzere VM 'Leri olabildiğince yakın bir şekilde almak için, bunları bir [yakınlık yerleşimi grubu](co-location.md#preview-proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimi grupları, düşük gecikme süresinin gereksinim olduğu iş yükleri için faydalıdır.

> [!IMPORTANT]
> Yakınlık yerleşimi grupları Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Önizleme sırasında yakınlık yerleşimi grupları bu bölgelerde kullanılamaz: **Japonya Doğu**, **Avustralya Doğu** ve **Hindistan Orta**.

## <a name="create-the-proximity-placement-group"></a>Yakınlık yerleşimi grubunu oluşturma
Kullanarak [`az ppg create`](/cli/azure/ppg#az-ppg-create)bir yakınlık yerleşimi grubu oluşturun. 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Yakınlık yerleşimi gruplarını Listele

[Az PPG List](/cli/azure/ppg#az-ppg-list)kullanarak tüm yakınlık yerleşimi gruplarınızı listeleyebilirsiniz.

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>VM oluşturma

[Yeni az VM](/cli/azure/vm#az-vm-create)kullanarak yakınlık yerleşimi grubu IÇINDE bir VM oluşturun.
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

VM 'yi, [az PPG Show](/cli/azure/ppg#az-ppg-show)kullanarak yakınlık yerleşimi grubunda görebilirsiniz.

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Kullanılabilirlik Kümeleri
Ayrıca, yakınlık yerleşimi grubunuzda bir kullanılabilirlik kümesi oluşturabilirsiniz. Kullanılabilirlik kümesi oluşturmak `--ppg` için [az VM AVAILABILITY-set create](/cli/azure/vm/availability-set#az-vm-availability-set-create) ile aynı parametreyi kullanın ve kullanılabilirlik kümesindeki tüm VM 'ler aynı yakınlık yerleşimi grubunda de oluşturulur.

## <a name="scale-sets"></a>Ölçek kümeleri

Ayrıca, yakınlık yerleştirme grubunuzda bir ölçek kümesi de oluşturabilirsiniz. Bir ölçek kümesi `--ppg` oluşturmak için [az VMSS Create](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create) ile aynı parametreyi kullanın ve tüm örnekler aynı yakınlık yerleşimi grubunda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Yakınlık yerleşimi grupları için [Azure CLI](/cli/azure/ppg) komutları hakkında daha fazla bilgi edinin.
