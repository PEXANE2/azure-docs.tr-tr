---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188824"
---
Azure nesnelerinin yazılabilir boyutları aşağıda verebilirsiniz. Yüklenen tüm dosyaların bu sınırlara uyduklarına emin olun.

| Azure nesne türü | Yükleme sınırı                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blob        | ~ 4.75 TB                                                 |
| Sayfa Blob         | 1 TB <br> Sayfa Blob formatında yüklenen her dosya 512 bayt hizalanmış olmalıdır (integral bir çoklu) aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalanmış. |
| Azure Dosyaları         | 1 TB <br> Sayfa Blob formatında yüklenen her dosya 512 bayt hizalanmış olmalıdır (integral bir çoklu) aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalanmış. |

> [!IMPORTANT]
> Dosyaların oluşturulmasına (depolama türüne bakılmaksızın) en fazla 5 TB'ye kadar izin verilir. Ancak, boyutu önceki tabloda tanımlanan yükleme sınırından büyük olan bir dosya oluşturursanız, dosya yüklenmez. Alanı geri almak için dosyayı el ile silmeniz gerekir.