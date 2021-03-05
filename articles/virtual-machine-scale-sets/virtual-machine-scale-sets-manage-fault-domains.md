---
title: Azure sanal makine ölçek kümelerinde hata etki alanlarını yönetme
description: Sanal makine ölçek kümesi oluştururken doğru sayıda FDs seçme hakkında bilgi edinin.
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 12/18/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 5a71a6bce6d0e1a41201e0d7395110a6ac64db8c
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102209756"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi için doğru hata etki alanı sayısını seçme
Sanal Makine Ölçek Kümeleri, Azure bölgelerinde hiçbir bölge olmadan varsayılan olarak beş hata etki alanıyla oluşturulur. Sanal makine ölçek kümelerinin zeli dağıtımını destekleyen bölgeler için ve bu seçenek belirlendiğinde, her bir bölge için varsayılan hata etki alanı sayısı 1 ' dir. Bu durumda FD = 1, ölçek kümesine ait sanal makine örneklerinin, en iyi çaba temelinde birçok rafta yayılacaı anlamına gelir.

Ayrıca, ölçek kümesi hata etki alanlarının sayısını yönetilen disk sayısı hata etki alanları ile hizalamayı göz önünde bulundurun. Bu hizalama, tüm yönetilen diskler hata etki alanının kapanması durumunda çekirdek kaybını önlemeye yardımcı olabilir. FD sayısı, her bölgede bulunan yönetilen disk hata etki alanı sayısına eşit veya ondan daha düşük bir değere ayarlanabilir. Bölgeye göre yönetilen disk hata etki alanı sayısı hakkında bilgi edinmek için bu [belgeye](../virtual-machines/manage-availability.md) başvurun.

## <a name="rest-api"></a>REST API
Özelliğini `properties.platformFaultDomainCount` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 3) olarak ayarlayabilirsiniz. [REST API için](/rest/api/compute/virtualmachinescalesets/createorupdate)belgelere bakın.

## <a name="azure-cli"></a>Azure CLI
Parametresini `--platform-fault-domain-count` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 3) olarak ayarlayabilirsiniz. [Burada](/cli/azure/vmss#az-vmss-create)Azure CLI belgelerine bakın.

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
- Azure ortamları için [kullanılabilirlik ve artıklık özellikleri](../virtual-machines/availability.md) hakkında daha fazla bilgi edinin.
