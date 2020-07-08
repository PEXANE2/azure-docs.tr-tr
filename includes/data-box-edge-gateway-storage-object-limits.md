---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188824"
---
Yazıılabilecek Azure nesnelerinin boyutları aşağıda verilmiştir. Karşıya yüklenen tüm dosyaların bu sınırlara uygun olduğundan emin olun.

| Azure nesne türü | Karşıya yükleme sınırı                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blobu        | ~ 4,75 TB                                                 |
| Sayfa Blobu         | 1 TB <br> Sayfa Blobu biçiminde karşıya yüklenen her dosya 512 bayt hizalı (tamsayı birden çok) olmalıdır, aksi takdirde karşıya yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı. |
| Azure Dosyaları         | 1 TB <br> Sayfa Blobu biçiminde karşıya yüklenen her dosya 512 bayt hizalı (tamsayı birden çok) olmalıdır, aksi takdirde karşıya yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı. |

> [!IMPORTANT]
> Dosyaların oluşturulmasına (depolama türünden bağımsız olarak) 5 TB 'a kadar izin verilir. Ancak, boyutu önceki tabloda tanımlanan karşıya yükleme sınırından daha büyük olan bir dosya oluşturursanız dosya karşıya yüklenemez. Alanı geri kazanmak için dosyayı el ile silmeniz gerekir.