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
ms.custom: mimckitt
ms.openlocfilehash: 70e1a131daf95a9b0f6ae75c022dec2fdbc394a7
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92366675"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi için doğru hata etki alanı sayısını seçme
Sanal Makine Ölçek Kümeleri, Azure bölgelerinde hiçbir bölge olmadan varsayılan olarak beş hata etki alanıyla oluşturulur. Sanal makine ölçek kümelerinin zeli dağıtımını destekleyen bölgeler için ve bu seçenek belirlendiğinde, her bir bölge için varsayılan hata etki alanı sayısı 1 ' dir. Bu durumda FD = 1, ölçek kümesine ait sanal makine örneklerinin, en iyi çaba temelinde birçok rafta yayılacaı anlamına gelir.

Ayrıca, ölçek kümesi hata etki alanlarının sayısını yönetilen disk sayısı hata etki alanları ile hizalamayı göz önünde bulundurun. Bu hizalama, tüm yönetilen diskler hata etki alanının kapanması durumunda çekirdek kaybını önlemeye yardımcı olabilir. FD sayısı, her bölgede bulunan yönetilen disk hata etki alanı sayısına eşit veya ondan daha düşük bir değere ayarlanabilir. Bölgeye göre yönetilen disk hata etki alanı sayısı hakkında bilgi edinmek için bu [belgeye](../virtual-machines/manage-availability.md) başvurun.

## <a name="rest-api"></a>REST API
Özelliğini `properties.platformFaultDomainCount` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 3) olarak ayarlayabilirsiniz. [REST API için](/rest/api/compute/virtualmachinescalesets/createorupdate)belgelere bakın.

## <a name="azure-cli"></a>Azure CLI
Parametresini `--platform-fault-domain-count` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 3) olarak ayarlayabilirsiniz. [Burada](/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)Azure CLI belgelerine bakın.

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