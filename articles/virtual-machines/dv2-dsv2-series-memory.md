---
title: Bellek optimize Dv2 ve DSv2 serisi VM'ler - Azure Sanal Makineler
description: Dv2 ve DSv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: joelpelley
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 66e415070e60676df5602078aff50c7b68920f1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914050"
---
# <a name="memory-optimized-dv2-and-dsv2-series"></a>Bellek optimize Dv2 ve Dsv2 serisi

Orijinal D serisinin devamı olan Dv2 ve Dsv2 serisi daha güçlü bir Işlemciye sahiptir. DSv2 serisi boyutları Intel® Xeon® 8171M 2.1 GHz (Skylake) veya Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcilerinde çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

## <a name="dv2-series-11-15"></a>Dv2 serisi 11-15

Dv2 serisi boyutları Intel® Xeon® 8171M 2.1 GHz (Skylake) veya Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcilerinde çalışır.

ACU: 210 - 250

Premium Depolama: Desteklenmiyor

Premium Depolama önbelleğe alma: Desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama iş boku: IOPS/Okuma MBps/Yazma MBps | Maksimum veri diskleri/iş aktarım: IOPS | Max NIC'ler/Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|
| Standard_D11_v2 | 2  | 14  | 100 | 6000/93/46    | 8/8x500   | 2/1500  |
| Standard_D12_v2 | 4  | 28  | 200 | 12000/187/93  | 16/16x500 | 4/3000  |
| Standard_D13_v2 | 8  | 56  | 400 | 24000/375/187 | 32/32x500 | 8/6000  |
| Standard_D14_v2 | 16 | 112 | 800 | 48000/750/375 | 64/64x500 | 8/12000 |
| Standard_D15_v2 <sup>1</sup> | 20 | 140 | 1000 | 60000/937/468 | 64/64x500 | 8/25000 <sup>2</sup> |

<sup>1</sup> Örnek, tek bir müşteriye adanmış donanıma yalıtılır.
Hızlandırılmış Ağ ile 2 25000 Mbps. <sup>2</sup>

## <a name="dsv2-series-11-15"></a>DSv2 serisi 11-15

DSv2 serisi boyutları Intel® Xeon® 8171M 2.1 GHz (Skylake) veya Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcilerinde çalışır.

ACU: 210 - 250 <sup>1</sup>

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama işliği: IOPS/MBps (GiB'de önbellek boyutu) | Maksimum cached disk işçıktısı: IOPS/MBps | Max NIC'ler/Beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 <sup>3</sup> | 2  | 14  | 28  | 8  | 8000/64 (72)    | 6400/96   | 2/1500  |
| Standard_DS12_v2 <sup>3</sup> | 4  | 28  | 56  | 16 | 16000/128 (144) | 12800/192 | 4/3000  |
| Standard_DS13_v2 <sup>3</sup> | 8  | 56  | 112 | 32 | 32000/256 (288) | 25600/384 | 8/6000  |
| Standard_DS14_v2 <sup>3</sup> | 16 | 112 | 224 | 64 | 64000/512 (576) | 51200/768 | 8/12000 |
| Standard_DS15_v2 <sup>2</sup> | 20 | 140 | 280 | 64 | 80000/640 (720) | 64000/960 | 8/25000 <sup>4</sup> |

<sup>1</sup> DSv2 serisi VM ile mümkün olan maksimum disk verimi (IOPS veya MBps) ekli diskin (ler) sayısı, boyutu ve şeritle sınırlı olabilir.  Ayrıntılar [için, yüksek performans için Tasarım bölümüne](./windows/premium-storage-performance.md)bakın.
<sup>2</sup> Örnek, Intel Haswell tabanlı donanıma yalıtılmış ve tek bir müşteriye adanmıştır.  
<sup>3</sup> Kısıtlı çekirdek boyutları mevcuttur.  
Hızlandırılmış Ağ ile <sup>4</sup> 25000 Mbps.

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
