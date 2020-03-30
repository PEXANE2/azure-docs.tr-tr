---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: ef18feb10dabc6a77e6512c6a32ad44b32c6e832
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334698"
---
**Premium yönetilmeyen sanal makine diskleri: Hesap başına limitler**

| Kaynak | Sınır |
| --- | --- |
| Hesap başına toplam disk kapasitesi |35 TB |
| Hesap başına toplam anlık görüntü kapasitesi |10 TB |
| Hesap başına maksimum bant genişliği (giriş + çıkış)<sup>1</sup> |En fazla 50 Gbps |

<sup>1</sup>*Giriş,* bir depolama hesabına gönderilen isteklerden gelen tüm verileri ifade eder. *Çıkış,* bir depolama hesabından alınan yanıtlardan gelen tüm verileri ifade eder.

**Premium yönetilmeyen sanal makine diskleri: Disk başına limitler**

| Premium depolama diski türü | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Disk boyutu |128 GiB |512 GiB |1.024 GiB (1 TB) |2.048 GiB (2 TB)|4.095 GiB (4 TB)|
| Disk başına maksimum IOPS |500 |2.300 |5.000 |7.500 |7.500 |
| Disk başına maksimum iş verimi |100 MB/sn | 150 MB/sn |200 MB/sn |250 MB/sn |250 MB/sn |
| Depolama hesabı başına maksimum disk sayısı |280 |70 |35 | 17 | 8 |

**Premium yönetilmeyen sanal makine diskleri: VM başına limitler**

| Kaynak | Sınır |
| --- | --- |
| VM başına maksimum IOPS |GS5 VM ile 80.000 IOPS |
| VM başına maksimum verim |GS5 VM ile 2.000 MB/sn |

