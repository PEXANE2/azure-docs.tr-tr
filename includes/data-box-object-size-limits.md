---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "66244622"
---
Yazıılabilecek Azure nesnelerinin boyutları aşağıda verilmiştir. Karşıya yüklenen tüm dosyaların bu sınırlara uygun olduğundan emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> Sayfa Blobu biçiminde karşıya yüklenen her dosya 512 bayt hizalı (tamsayı birden çok) olmalıdır, aksi takdirde karşıya yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı. |
| Azure Dosyaları        | 1 TiB                                                      |
| Yönetilen diskler     | 4 TiB <br> Boyut ve sınırlar hakkında daha fazla bilgi için bkz. <li>[Standart SSD 'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Premium SSDs 'nin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Standart HDD 'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Yönetilen disklerin fiyatlandırma ve faturalama](../articles/virtual-machines/windows/disks-types.md#billing)</li>  
