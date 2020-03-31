---
title: Azure için vCPU kotaları
description: Azure için vCPU kotaları hakkında bilgi edinin.
keywords: ''
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: 20017f42031e126bbdae40f66bf4c36efd12f17c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75893457"
---
# <a name="virtual-machine-vcpu-quotas"></a>Sanal makine vCPU kotaları

Sanal makineler ve sanal makine ölçek kümeleri için vCPU kotaları, her bölgedeki her abonelik için iki katmanda düzenlenir. Birinci katman Total Regional vCPUs'dur, ikinci katman ise D serisi vCPUs'lar gibi çeşitli VM boyutundaki aile çekirdekleridir. VM için vCPUs'lar yeni bir VM dağıtıldığında, VM boyutu ailesi veya toplam bölgesel vCPU kotası için vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmeyecektir. Bölgedeki toplam sanal makine sayısı için de bir kota var. Bu kotaların her biriyle ilgili ayrıntılar [Azure portalındaki](https://portal.azure.com) **Abonelik** sayfasının Kullanım **+ kotaları** bölümünde görülebilir veya PowerShell kullanarak değerleri sorgulayabilirsiniz.

   
 
## <a name="check-usage"></a>Kullanımı denetleme

Kota kullanımınızı kontrol etmek için [Get-AzVMUsage](https://docs.microsoft.com/powershell/module/az.compute/get-azvmusage) cmdlet'i kullanabilirsiniz.

```azurepowershell-interactive
Get-AzVMUsage -Location "East US"
```

Çıktı şuna benzer:

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count
```


## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
VM boyutu esnekliği olmadan tek bir aboneliğe kapsamına giren Ayrılmış VM Örnekleri, vCPU kotalarına yeni bir boyut katar. Bu değerler, belirtilen boyutun abonelikte dağıtılabilen örnek sayısını açıklar. Ayrılmış VM örneklerinin abonelikte dağıtılabilir olmasını sağlamak için kotanın rezerve edilmesini sağlamak için kota sisteminde yer tutucu olarak çalışırlar. Örneğin, belirli bir aboneliğin 10 Standard_D1 ayrılmış VM örneği varsa, ayrılmış Standard_D1 VM örneklerinin kullanım sınırı 10 olacaktır. Bu, Azure'un Standard_D1 örneğinde kullanılacak Toplam Bölgesel vCPUs kotasında her zaman en az 10 vCPUs bulunmasını ve standart D Ailesi vCPU kotasında Standard_D1 örnek için kullanılmak üzere en az 10 vCPUs bulunmasını sağlamasına neden olur.

Tek Abonelik RI satın almak için kota artışı gerekiyorsa, aboneliğinizde [kota artışı talep](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Faturalandırma ve kotalar hakkında daha fazla bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits?toc=/azure/billing/TOC.json)bölümüne bakın.
