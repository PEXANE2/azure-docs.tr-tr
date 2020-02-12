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
ms.openlocfilehash: b819264895e35c6ef4fe9dc5263444dcac17eaa2
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74935799"
---
Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

- , Bölge başına değişen sayıda kullanılabilirlik bölgesi ile aşağıdaki bölgelerde desteklenir:
    - Doğu ABD 2
    - Doğu ABD
    - Batı ABD 2
    - Güneydoğu Asya
    - Kuzey Avrupa
    - Batı Avrupa
    - Güney Birleşik Krallık 
- Yalnızca kullanılabilirlik alanları ile kullanılabilir (bölge dışındaki kullanılabilirlik kümeleri ve tek VM dağıtımları, bir ultra disk iliştirebilme özelliğine sahip olmaz)
- Yalnızca şu VM serisinde desteklenir:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, tüm desteklenen bölgelerde Ultra disklerle kullanılamaz
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler. Ultra diskin 4K yerel sektör boyutu nedeniyle, Ultra disklerle uyumlu olmayacak bazı uygulamalar vardır. Oracle Database bir örnek, Ultra diskleri desteklemek için sürüm 12,2 veya sonraki bir sürümü gerektirir.  
- Yalnızca boş disk olarak oluşturulabilir  
- Disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini ve Azure disk şifrelemesini henüz desteklemez
- Azure Backup veya Azure Site Recovery tümleştirmeyi henüz desteklemez
- GA VM 'lerde ıOPS için geçerli en büyük sınır 80.000 ' dir.
- Ultra disklerle 160.000 ıOPS gerçekleştirebilecek bir sanal makinenin sınırlı önizlemesine katılmak istiyorsanız lütfen e-posta UltraDiskFeedback@microsoft. com