---
title: Fsv2 serisi - Azure Sanal Makineler
description: Fsv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: brbell
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: f28c6b61aee3c8cbc078db1c2cfb48ed1fba4554
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164857"
---
# <a name="fsv2-series"></a>Fsv2 serisi

Fsv2 serisi Intel® Xeon® Platinum 8168 işlemci dayanmaktadır. Tüm çekirdekli Turbo saat hızı 3.4 GHz ve maksimum tek çekirdekli turbo frekansı 3.7 GHz'dir. Intel® AVX-512 talimatları Intel Ölçeklenebilir İşlemciler yeni. Bu yönergeler, hem tek hem de çift hassas kayan nokta işlemlerinde vektör işleme iş yüklerine 2 kata kadar performans artışı sağlar. Başka bir deyişle, herhangi bir hesaplama iş yükü için gerçekten hızlı.

Fsv2 serisi VM'ler Intel® Hiper-Threading Teknolojisine sahiptir.

ACU: 195 - 210

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

| Boyut | vCPU'nun | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama işliği: IOPS/MBps (GiB'de önbellek boyutu) | Maksimum cached disk işçıktısı: IOPS/MBps | Max NIC'ler/Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_F2s_v2  | 2  | 4   | 16  | 4  | 4000/31 (32)       | 3200/47    | 2/875   |
| Standard_F4s_v2  | 4  | 8   | 32  | 8  | 8000/63 (64)       | 6400/95    | 2/1750  |
| Standard_F8s_v2  | 8  | 16  | 64  | 16 | 16000/127 (128)    | 12800/190  | 4/3500  |
| Standard_F16s_v2 | 16 | 32  | 128 | 32 | 32000/255 (256)    | 25600/380  | 4/7000  |
| Standard_F32s_v2 | 32 | 64  | 256 | 32 | 64000/512 (512)    | 51200/750  | 8/14000 |
| Standard_F48s_v2 | 48 | 96  | 384 | 32 | 96000/768 (768)    | 76800/1100 | 8/21000 |
| Standard_F64s_v2 | 64 | 128 | 512 | 32 | 128000/1024 (1024) | 80000/1100 | 8/28000 |
| Standard_F72s_v2<sup>1, 2</sup> | 72 | 144 | 576 | 32 | 144000/1152 (1520) | 80000/1100 | 8/30000 |

<sup>1</sup> 64 vCPU'dan fazla vCPU kullanımı için şu desteklenen konuk işletim sistemlerinden biri gerekir:

- Windows Server 2016 veya sonrası
- Ubuntu 16.04 LTS veya daha sonraki, Azure ayarlı çekirdekli (4,15 çekirdek veya daha sonraki)
- SLES 12 SP2 veya sonrası
- Microsoft tarafından sağlanan LIS paketi 4.3.1 (veya daha sonra) yüklü olan RHEL veya CentOS sürüm 6.7 ile 6.10
- Microsoft tarafından sağlanan LIS paketi 4.2.1 (veya daha sonra) yüklü OLAN RHEL veya CentOS sürüm 7.3
- RHEL veya CentOS sürüm 7.6 veya sonrası
- UEK4 veya sonrası ile Oracle Linux
- Debian 9 backports çekirdek, Debian 10 veya daha sonra
- CoreOS 4.14 çekirdek veya daha sonra

<sup>2</sup> Örnek, tek bir müşteriye adanmış donanıma yalıtılır.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
