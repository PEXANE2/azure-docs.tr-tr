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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74935799"
---
Şimdilik, ultra diskler ek sınırlamalar var, onlar aşağıdaki gibidir:

- Bölge başına değişen sayıda kullanılabilirlik bölgesiyle aşağıdaki bölgelerde desteklenir:
    - Doğu ABD 2
    - Doğu ABD
    - Batı ABD 2
    - Güneydoğu Asya
    - Kuzey Avrupa
    - Batı Avrupa
    - Güney Birleşik Krallık 
- Yalnızca kullanılabilirlik bölgeleriyle kullanılabilir (kullanılabilirlik kümeleri ve bölgelerin dışındaki tek VM dağıtımları ultra disk ekleme yeteneğine sahip olmayacaktır)
- Yalnızca aşağıdaki VM serilerinde desteklenir:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, desteklenen her bölgede ultra disklerle kullanılamaz
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler. Ultra Disk'in 4K yerel sektör boyutu nedeniyle, ultra disklerle uyumlu olmayacak bazı uygulamalar vardır. Bir örnek, ultra diskleri desteklemek için sürüm 12.2 veya daha sonra gerektiren Oracle Veritabanı olacaktır.  
- Yalnızca boş diskler olarak oluşturulabilir  
- Disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini ve Azure disk şifrelemeyi henüz desteklemeyin
- Azure Yedekleme veya Azure Site Kurtarma ile tümleştirmeyi henüz desteklemeyin
- GA VM'lerde IOPS için geçerli maksimum sınır 80.000'dir.
- Ultra disklerle 160.000 IOPS gerçekleştirebilen bir VM'nin sınırlı bir önizlemesine UltraDiskFeedback@microsoft katılmak isterseniz, lütfen .com adresine e-posta gönderin.