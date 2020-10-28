---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 10599b47d55a0b4ea1eb79cda3c31a2479efb7c0
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755452"
---
Bir Azure sanal makinesine çok sayıda veri diski ekleyebilirsiniz. Bir sanal makinenin veri disklerinin ölçeklenebilirlik ve performans hedeflerine bağlı olarak, performans ve kapasite gereksinimlerinizi karşılamak için gereken disk sayısını ve türünü belirleyebilirsiniz.

> [!IMPORTANT]
> En iyi performans için, olası azaltmayı önlemek için sanal makineye bağlı olan yüksek oranda kullanılan disklerin sayısını sınırlayın. Tüm eklenen diskler aynı anda yüksek oranda kullanılıyorsa, sanal makine daha fazla sayıda diski destekleyebilir.

**Azure yönetilen diskler için:**

Aşağıdaki tabloda, abonelik başına bölge başına düşen kaynak sayısı için varsayılan ve en fazla limit gösterilmektedir. Sınırlar, platform tarafından yönetilen anahtarlarla veya müşteri tarafından yönetilen anahtarlarla şifrelenen disklere bakılmaksızın aynı kalır. Kaynak grubu başına yönetilen disk sayısı, anlık görüntüler ve görüntüler için bir sınır yoktur.  

> | Kaynak | Sınır |
> | --- | --- |
> | Standart yönetilen diskler | 50,000 |
> | Standart SSD yönetilen diskler | 50,000 |
> | Premium yönetilen diskler | 50,000 |
> | Standard_LRS anlık görüntüler | 50,000 |
> | Standard_ZRS anlık görüntüler | 50,000 |
> | Yönetilen görüntü | 50,000 |

**Standart depolama hesapları için:** Standart depolama hesabında en fazla 20.000 ıOPS toplam istek oranı vardır. Standart depolama hesabındaki tüm sanal makine disklerinizdeki toplam ıOPS, bu sınırı aşmamalıdır.
  
    You can roughly calculate the number of highly utilized disks supported by a single Standard storage account based on the request rate limit. For example, for a Basic tier VM, the maximum number of highly utilized disks is about 66, which is 20,000/300 IOPS per disk. The maximum number of highly utilized disks for a Standard tier VM is about 40, which is 20,000/500 IOPS per disk. 

**Premium Depolama hesapları için:** Premium Depolama hesabında en fazla 50 Gbps toplam verimlilik ücreti vardır. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

