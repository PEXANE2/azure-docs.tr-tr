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
ms.openlocfilehash: 1f5b253dbc2161892bcc2aee07c8cd3788fa5fb5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83200024"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi için doğru hata etki alanı sayısını seçme
Sanal Makine Ölçek Kümeleri, Azure bölgelerinde hiçbir bölge olmadan varsayılan olarak beş hata etki alanıyla oluşturulur. Sanal makine ölçek kümelerinin zeli dağıtımını destekleyen bölgeler için ve bu seçenek belirlendiğinde, her bir bölge için varsayılan hata etki alanı sayısı 1 ' dir. Bu durumda FD = 1, ölçek kümesine ait sanal makine örneklerinin, en iyi çaba temelinde birçok rafta yayılacaı anlamına gelir.

Ayrıca, ölçek kümesi hata etki alanlarının sayısını yönetilen disk sayısı hata etki alanları ile hizalamayı göz önünde bulundurun. Bu hizalama, tüm yönetilen diskler hata etki alanının kapanması durumunda çekirdek kaybını önlemeye yardımcı olabilir. FD sayısı, her bölgede bulunan yönetilen disk hata etki alanı sayısına eşit veya ondan daha düşük bir değere ayarlanabilir. Bölgeye göre yönetilen disk hata etki alanı sayısı hakkında bilgi edinmek için bu [belgeye](../virtual-machines/windows/manage-availability.md) başvurun.

## <a name="rest-api"></a>REST API
Özelliğini `properties.platformFaultDomainCount` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 5) olarak ayarlayabilirsiniz. [REST API için](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate)belgelere bakın.

## <a name="azure-cli"></a>Azure CLI
Parametresini `--platform-fault-domain-count` 1, 2 veya 3 (belirtilmemişse, varsayılan olarak 5) olarak ayarlayabilirsiniz. [Burada](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create)Azure CLI belgelerine bakın.

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
