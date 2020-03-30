---
title: Azure için vCPU kotaları
description: Azure için vCPU kotaları hakkında bilgi edinin.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: c194dbeb0183e64535342f8aaf9a770a93b3e332
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75896176"
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

Sanal makineler ve sanal makine ölçek kümeleri için vCPU kotaları, her bölgedeki her abonelik için iki katmanda düzenlenir. Birinci katman Total Regional vCPUs'dur, ikinci katman ise D serisi vCPUs'lar gibi çeşitli VM boyutundaki aile çekirdekleridir. VM için vCPUs'lar yeni bir VM dağıtıldığında, VM boyutu ailesi veya toplam bölgesel vCPU kotası için vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmeyecektir. Bölgedeki toplam sanal makine sayısı için de bir kota var. Bu kotaların her biriyle ilgili ayrıntılar [Azure portalındaki](https://portal.azure.com) **Abonelik** sayfasının Kullanım **+ kotaları** bölümünde görülebilir veya Azure CLI'yi kullanarak değerleri sorgulayabilirsiniz.


## <a name="check-usage"></a>Kullanımı denetleme

[Kota kullanımınızı az vm list-kullanımını](/cli/azure/vm)kullanarak kontrol edebilirsiniz.

```azurecli-interactive
az vm list-usage --location "East US" -o table
```

Çıkış aşağıdakine benzer olmalıdır:


```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard A8-A11 Family vCPUs                   0      100
Standard DS Family vCPUs                       0      100
Standard G Family vCPUs                        0      100
Standard GS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```

## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
VM boyutu esnekliği olmadan tek bir aboneliğe kapsamına giren Ayrılmış VM Örnekleri, vCPU kotalarına yeni bir boyut katar. Bu değerler, belirtilen boyutun abonelikte dağıtılabilen örnek sayısını açıklar. Azure rezervasyonlarının abonelikte dağıtılabilir olmasını sağlamak için kotanın rezerve edilmesini sağlamak için kota sisteminde yer tutucu olarak çalışırlar. Örneğin, belirli bir abonelikte 10 Standard_D1 rezervasyon varsa, Standard_D1 rezervasyonları için kullanım sınırı 10 olacaktır. Bu, Azure'un Standard_D1 örneğinde kullanılacak Toplam Bölgesel vCPUs kotasında her zaman en az 10 vCPUs bulunmasını ve standart D Ailesi vCPU kotasında Standard_D1 örnek için kullanılmak üzere en az 10 vCPUs bulunmasını sağlamasına neden olur.

Tek Abonelik RI satın almak için kota artışı gerekiyorsa, aboneliğinizde [kota artışı talep](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Faturalandırma ve kotalar hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits?toc=/azure/billing/TOC.json)bölümüne bakın.
