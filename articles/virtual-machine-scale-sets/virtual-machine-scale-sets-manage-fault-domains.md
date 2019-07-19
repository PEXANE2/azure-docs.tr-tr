---
title: Azure sanal makine ölçek kümelerinde hata etki alanlarını yönetme | Microsoft Docs
description: Sanal makine ölçek kümesi oluştururken doğru sayıda FDs seçme hakkında bilgi edinin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: drewm
ms.openlocfilehash: f97c7e6971fb9c58a3f08959c00c84e64e160916
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67871948"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Sanal makine ölçek kümesi için hata etki alanlarının doğru sayısını seçme
Sanal Makine Ölçek Kümeleri, Azure bölgelerinde hiçbir bölge olmadan varsayılan olarak beş hata etki alanıyla oluşturulur. Sanal makine ölçek kümelerinin zeli dağıtımını destekleyen bölgeler için, hata etki alanı sayısı varsayılan değeri her bir bölgenin 1 ' dir. Bu durumda FD = 1, ölçek kümesine ait sanal makine örneklerinin, en iyi çaba temelinde birçok rafta yayılacaı anlamına gelir.

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
