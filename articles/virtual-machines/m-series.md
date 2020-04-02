---
title: M serisi - Azure Sanal Makineler
description: M serisi VM'ler için teknik özellikler.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 03/31/2020
ms.author: jushiman
ms.openlocfilehash: 3e0552570d5bdb7f812852cd058710b833b7fdd4
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521337"
---
# <a name="m-series"></a>M serisi

M serisi yüksek vCPU sayısı (128 vCPUs'a kadar) ve büyük miktarda bellek (3,8 TiB'ye kadar) sunar. Ayrıca, yüksek vCPU sayılarından ve büyük miktarda bellekten yararlanan son derece büyük veritabanları veya diğer uygulamalar için de idealdir. M&reg; serisi boyutları hem Intel Xeon&reg; Işlemci E7-8890 v3 @ 2.50GHz&reg; ve Intel&reg; Xeon Platinum 8280M 2.7GHz (Cascade Lake) üzerinde desteklenir.

M-serisi VM özelliği&reg; Intel Hyper-Threading Teknolojisi.

ACU: 160-180

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

Yazma Hızlandırıcısı: [Desteklenen](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama işliği: IOPS/MBps (GiB'de önbellek boyutu) | Maksimum cached disk işçıktısı: IOPS/MBps | Max NIC'ler/Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_M8ms <sup>2</sup>       | 8   | 218.75 | 256   | 8  | 10000/100 (793)     | 5000/125   | 4/2000  |
| Standard_M16ms <sup>2</sup>      | 16  | 437.5  | 512   | 16 | 20000/200 (1587)    | 10000/250  | 8/4000  |
| Standard_M32ts                   | 32  | 192    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M32ls                   | 32  | 256    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M32ms <sup>2</sup>      | 32  | 875    | 1024  | 32 | 40000/400 (3174)    | 20000/500  | 8/8000  |
| Standard_M64s                    | 64  | 1024   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M64ls                   | 64  | 512    | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M64ms <sup>3</sup>      | 64  | 1792   | 2048  | 64 | 80000/800 (6348)    | 40000/1000 | 8/16000 |
| Standard_M128s <sup>1</sup>      | 128 | 2048   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8/30000 |
| Standard_M128ms <sup>1,2,3</sup> | 128 | 3892   | 4096  | 64 | 160000/1600 (12696) | 80000/2000 | 8/30000 |
| Standard_M64                     | 64  | 1024   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8/16000 |
| Standard_M64m                    | 64  | 1792   | 7168  | 64 | 80000/800 (1228)    | 40000/1000 | 8/16000 |
| Standard_M128 <sup>1</sup>       | 128 | 2048   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8/32000 |
| Standard_M128m <sup>1</sup>      | 128 | 3892   | 14336 | 64 | 250000/1600 (2456)  | 80000/2000 | 8/32000 |

<sup>1</sup> 64'ten fazla vCPU şu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya LIS 4.2.1 ile Oracle Linux 7.3.

<sup>2</sup> Kısıtlı çekirdek boyutları mevcuttur.

<sup>3</sup> Örnek, tek bir müşteriye adanmış donanıma yalıtılır.

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
