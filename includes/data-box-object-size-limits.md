---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: 9805f53d5901226fc9e32b24a323256cd1da6844
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88655014"
---
Yazıılabilecek Azure nesnelerinin boyutları aşağıda verilmiştir. Karşıya yüklenen tüm dosyaların bu sınırlara uygun olduğundan emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> Sayfa Blobu biçiminde karşıya yüklenen her dosya 512 bayt hizalı (tamsayı birden çok) olmalıdır, aksi takdirde karşıya yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı. |
| Azure Dosyaları        | 1 TiB                                                      |
| Yönetilen diskler     | 4 TiB <br> Boyut ve sınırlar hakkında daha fazla bilgi için bkz. <li>[Standart SSD 'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/disks-types.md#standard-ssd)</li><li>[Premium SSDs 'nin ölçeklenebilirlik hedefleri](../articles/virtual-machines/disks-types.md#standard-hdd)</li><li>[Standart HDD 'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/disks-types.md#premium-ssd)</li><li>[Yönetilen disklerin fiyatlandırma ve faturalama](../articles/virtual-machines/disks-types.md#billing)</li>  
