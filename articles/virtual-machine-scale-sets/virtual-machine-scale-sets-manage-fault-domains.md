---
title: Azure sanal makine ölçek kümelerinde hata etki alanlarını yönetme
description: Sanal makine ölçeği kümesi oluştururken doğru sayıda FD'yi nasıl seçeceğinizi öğrenin.
author: rajsqr
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: drewm
ms.openlocfilehash: 297837354cea4bb5ccdcc03261810dcffd144243
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275719"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi için doğru hata etki alanı sayısını seçme
Sanal makine ölçek kümeleri, bölgeleri olmayan Azure bölgelerinde varsayılan olarak beş hata etki alanıyla oluşturulur. Sanal makine ölçek kümelerinin bölge dağıtımını destekleyen ve bu seçenek seçilen bölgeler için, hata etki alanı sayısının varsayılan değeri bölgelerin her biri için 1'dir. Bu durumda FD=1, ölçek kümesine ait VM örneklerinin en iyi çaba temelinde birçok rafa yayılacağı anlamına gelir.

Ayrıca, ölçek kümesi hata etki alanlarının sayısını Yönetilen Diskler hata etki alanlarının sayısıyla hizalamayı da düşünebilirsiniz. Bu hizalama, Yönetilen Diskler hata etki alanının tamamı çökerse, çoğunluk kaybını önlemeye yardımcı olabilir. FD sayısı, her bölgede bulunan Yönetilen Diskler hata etki alanlarının sayısından daha az veya eşit olarak ayarlanabilir. Bölgeye göre Yönetilen Diskler hata etki alanlarının sayısı hakkında bilgi edinmek için bu [belgeye](../virtual-machines/windows/manage-availability.md) bakın.

## <a name="rest-api"></a>REST API
Özelliği `properties.platformFaultDomainCount` 1, 2 veya 3 olarak ayarlayabilirsiniz (belirtilmemişse varsayılan 5' tir). REST API için [belgelere buradan](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)bakın.

## <a name="azure-cli"></a>Azure CLI
Parametreyi `--platform-fault-domain-count` 1, 2 veya 3 olarak ayarlayabilirsiniz (belirtilmemişse varsayılan 5 varsayılan). Azure [CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)için belgelere buradan bakın.

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
- Azure ortamları için [kullanılabilirlik ve artıklık özellikleri](../virtual-machines/windows/availability.md) hakkında daha fazla bilgi edinin.
