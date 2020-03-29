---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "66244622"
---
Azure nesnelerinin yazılabilir boyutları aşağıda verebilirsiniz. Yüklenen tüm dosyaların bu sınırlara uyduklarına emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4.75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> Sayfa blob biçiminde yüklenen her dosya 512 bayt hizalanmış olmalıdır (integral bir çoklu), aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalanmış. |
| Azure Dosyaları        | 1 TiB                                                      |
| Yönetilen diskler     | 4 TiB <br> Boyut ve sınırlar hakkında daha fazla bilgi için bkz: <li>[Standart SSD'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Premium SSD'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Standart HDD'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Yönetilen disklerin fiyatlandırması ve faturalandırılması](../articles/virtual-machines/windows/disks-types.md#billing)</li>  
