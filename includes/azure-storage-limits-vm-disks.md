---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: c3028ed7629c41eece354dd2554ede9249bac4f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334964"
---
Bir Azure sanal makinesine çok sayıda veri diski ekleyebilirsiniz. VM'nin veri disklerinin ölçeklenebilirlik ve performans hedeflerine bağlı olarak, performans ve kapasite gereksinimlerinizi karşılamak için gereken disk sayısını ve türünü belirleyebilirsiniz.

> [!IMPORTANT]
> En iyi performans için, olası azaltmayı önlemek için sanal makineye bağlı yüksek oranda kullanılan disk sayısını sınırlayın. Ekli tüm diskler aynı anda yüksek oranda kullanılmıyorsa, sanal makine daha fazla disk sayısını destekleyebilir.

**Azure yönetilen diskler için:**

Aşağıdaki tablo, abonelik başına bölge başına kaynak sayısının varsayılan ve maksimum sınırlarını göstermektedir. Yönetilen Disk sayısı, anlık görüntü ve kaynak grubu başına görüntü sayısı için bir sınır yoktur.  

> | Kaynak | Sınır |
> | --- | --- |
> | Standart yönetilen diskler | 50.000 |
> | Standart SSD yönetilen diskler | 50.000 |
> | Premium yönetilen diskler | 50.000 |
> | Standard_LRS anlık görüntüler | 50.000 |
> | Standard_ZRS anlık görüntüler | 50.000 |
> | Yönetilen görüntü | 50.000 |

* **Standart depolama hesapları için:** Standart depolama hesabının maksimum toplam istek oranı 20.000 IOPS'dir. Standart depolama hesabındaki tüm sanal makine disklerinizdeki toplam IOPS bu sınırı aşmamalıdır.
  
    İstek oranı sınırını temel alarak tek bir Standart depolama hesabı tarafından desteklenen yüksek kullanılan disk lerin sayısını kabaca hesaplayabilirsiniz. Örneğin, temel katman VM için, yüksek oranda kullanılan disk sayısı yaklaşık 66'dır ve bu da disk başına 20.000/300 IOPS'dir. Standart seviye VM için en fazla kullanılan disk sayısı yaklaşık 40'tır ve bu da disk başına 20.000/500 IOPS'dir. 

* **Premium depolama hesapları için:** Premium depolama hesabının toplam toplam verim oranı 50 Gbps'dir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

