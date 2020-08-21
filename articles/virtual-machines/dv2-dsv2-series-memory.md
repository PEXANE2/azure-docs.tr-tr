---
title: Bellek için iyileştirilmiş dv2 ve DSv2 serisi VM 'Ler-Azure sanal makineleri
description: Dv2 ve DSv2 serisi VM 'Ler için Özellikler.
author: joelpelley
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: aee88420fe8b88de9af9a2397316dd065144e610
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88685298"
---
# <a name="memory-optimized-dv2-and-dsv2-series"></a>Bellek için iyileştirilmiş dv2 ve Dsv2 serisi

Özgün D serisi için bir takip olan dv2 ve Dsv2 serisi, daha güçlü bir CPU sunar. DSv2 serisi boyutlar Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel 2,3® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

## <a name="dv2-series-11-15"></a>Dv2 serisi 11-15

Dv2 serisi boyutlar Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel 2,3® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır.

ACU: 210 - 250

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC|Beklenen ağ bant genişliği (MB/sn) |
|---|---|---|---|---|---|---|---|
| Standard_D11_v2 | 2  | 14  | 100 | 6000/93/46    | 8/8x500   | 2|1500  |
| Standard_D12_v2 | 4  | 28  | 200 | 12000/187/93  | 16/16x500 | 4|3000  |
| Standard_D13_v2 | 8  | 56  | 400 | 24000/375/187 | 32/32x500 | 8|6000  |
| Standard_D14_v2 | 16 | 112 | 800 | 48000/750/375 | 64/64x500 | 8|12000 |
| Standard_D15_v2 <sup>1</sup> | 20 | 140 | 1000 | 60000/937/468 | 64/64x500 | 8|25000 <sup>2</sup> |

<sup>1</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.
Hızlandırılmış ağ ile <sup>2</sup> 25000 Mbps.

## <a name="dsv2-series-11-15"></a>DSv2 serisi 11-15

DSv2 serisi boyutlar Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel 2,3® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır.

ACU: 210-250 <sup>1</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC|Beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |---|
| Standard_DS11_v2 <sup>3</sup> | 2  | 14  | 28  | 8  | 8000/64 (72)    | 6400/96   | 2|1500  |
| Standard_DS12_v2 <sup>3</sup> | 4  | 28  | 56  | 16 | 16000/128 (144) | 12800/192 | 4|3000  |
| Standard_DS13_v2 <sup>3</sup> | 8  | 56  | 112 | 32 | 32000/256 (288) | 25600/384 | 8|6000  |
| Standard_DS14_v2 <sup>3</sup> | 16 | 112 | 224 | 64 | 64000/512 (576) | 51200/768 | 8|12000 |
| Standard_DS15_v2 <sup>2</sup> | 20 | 140 | 280 | 64 | 80000/640 (720) | 64000/960 | 8|25000 <sup>4</sup> |

<sup>1</sup> DSV2 serisi VM ile olası maksimum disk aktarım hızı (IOPS veya MB/sn), eklenen disklerin sayısı, boyutu ve şeritle sınırlı olabilir.  Ayrıntılar için bkz. [yüksek performans Için tasarlama](./premium-storage-performance.md).
<sup>2</sup>  örnek, Intel Haswell tabanlı donanıma yalıtılmış ve tek bir müşteriye ayrılmıştır.  
<sup>3</sup> kısıtlı çekirdek boyutu var.  
Hızlandırılmış ağ ile <sup>4</sup> 25000 Mbps.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Diğer boyutlar ve bilgiler

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

Fiyatlandırma Hesaplayıcı: [Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)

Disk türleri hakkında daha fazla bilgi: [disk türleri](./disks-types.md#ultra-disk)


## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.