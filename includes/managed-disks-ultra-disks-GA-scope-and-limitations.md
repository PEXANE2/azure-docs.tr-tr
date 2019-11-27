---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/18/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: df488c2a3033015175943f7d092beaaa41e34046
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260908"
---
Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

- , Bölge başına değişen sayıda kullanılabilirlik bölgesi ile aşağıdaki bölgelerde desteklenir:
    - Doğu ABD 2
    - Doğu ABD
    - Batı ABD 2
    - Güneydoğu Asya
    - Kuzey Avrupa
    - Batı Avrupa
    - Birleşik Krallık Güney 
- Yalnızca kullanılabilirlik alanları ile kullanılabilir (bölge dışındaki kullanılabilirlik kümeleri ve tek VM dağıtımları, bir ultra disk iliştirebilme özelliğine sahip olmaz)
- Yalnızca şu VM serisinde desteklenir:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, Ultra disklerle desteklenen her bölgede kullanılabilir değildir.
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler  
- Yalnızca boş disk olarak oluşturulabilir  
- Disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini ve Azure disk şifrelemesini henüz desteklemez
- Azure Backup veya Azure Site Recovery tümleştirmeyi henüz desteklemez
- GA VM 'lerde ıOPS için geçerli en büyük sınır 80.000 ' dir.
- Ultra disklerle 160.000 ıOPS gerçekleştirebilecek bir sanal makinenin sınırlı önizlemesine katılmak istiyorsanız lütfen e-posta UltraDiskFeedback@microsoft. com