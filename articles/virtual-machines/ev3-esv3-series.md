---
title: Ev3-Series ve Esv3 serisi-Azure sanal makineleri
description: Ev3 ve Esv3 serisi VM 'Ler için Özellikler.
services: virtual-machines
author: joelpelley
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: aad4610a44c59ed95cf1ad9777329097886c5bed
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78164381"
---
# <a name="ev3-and-esv3-series"></a>Ev3 ve Esv3 serisi

Ev3 ve Esv3 serisi özelliği, bir hiper iş yükleri için Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel® Xeon® E5-2673 v4 2,3 GHz (geniş kapsamlı) işlemciyi, genel amaçlı iş yükleri için daha iyi bir değer teklifi sağlar ve Ev3 diğer bulutların çoğu için genel amaçlı VM 'lerle hizalı olarak.  Bellek ve ağ sınırları, hiper iş parçacığına geçme ile hizalanmak üzere bir çekirdek temelinde ayarlandığı sürece (7 GiB/vCPU 'dan 8 GiB/vCPU 'ya kadar) bellek genişletilir. Ev3, D/dv2 ailelerinin yüksek bellek sanal makine boyutlarına göre takip edilir.

## <a name="ev3-series"></a>Ev3 serisi

Ev3 serisi örnekler Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel® Xeon® E5-2673 v4 2,3 GHz (geniş) işlemci ve Intel Turbo Boost Technology 2,0 özelliğini temel alır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır.

Ev3 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®.

ACU: 160-190

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|---|---|---|---|---|---|---|
| Standard_E2_v3  | 2  | 16  | 50   | 4  | 3000/46/23     | 2/1000  |
| Standard_E4_v3  | 4  | 32  | 100  | 8  | 6000/93/46     | 2/2000  |
| Standard_E8_v3  | 8  | 64  | 200  | 16 | 12000/187/93   | 4/4000  |
| Standard_E16_v3 | 16 | 128 | 400  | 32 | 24000/375/187  | 8/8000  |
| Standard_E20_v3 | 20 | 160 | 500  | 32 | 30000/469/234  | 8/10000 |
| Standard_E32_v3 | 32 | 256 | 800  | 32 | 48000/750/375  | 8/16000 |
| Standard_E48_v3 | 48 | 384 | 1200 | 32 | 96000/1000/500 | 8/24000 |
| Standard_E64_v3 | 64 | 432 | 1600 | 32 | 96000/1000/500 | 8/30000 |
| Standard_E64i_v3 <sup>1, 2</sup> | 64 | 432 | 1600 | 32 | 96000/1000/500 | 8/30000 |

<sup>1</sup> kısıtlı çekirdek boyutu var.

<sup>2</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

## <a name="esv3-series"></a>Esv3 serisi

Esv3 serisi örnekler, Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel® Xeon® E5-2673 v4 2,3 GHz (geniş) işlemci, Intel Turbo Boost Technology 2,0 özelliğini temel alır ve Premium depolamayı kullanır. Esv3 serisi örnekler, bellek açısından yoğun kurumsal uygulamalar için idealdir.

Esv3 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®.

ACU: 160-190

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_E2s_v3 | 2 | 16 | 32 | 4 | 4000/32 (50) | 3200/48 | 2/1000 |
| Standard_E4s_v3 <sup>1</sup>  | 4  | 32  | 64  | 8  | 8000/64 (100)   | 6400/96   | 2/2000 |
| Standard_E8s_v3 <sup>1</sup>  | 8  | 64  | 128 | 16 | 16000/128 (200) | 12800/192 | 4/4000 |
| Standard_E16s_v3 <sup>1</sup> | 16 | 128 | 256 | 32 | 32000/256 (400) | 25600/384 | 8/8000 |
| Standard_E20s_v3 | 20 | 160 | 320 | 32 | 40000/320 (400) | 32000/480 | 8/10000 |
| Standard_E32s_v3 <sup>1</sup>  | 32 | 256 | 512 | 32 | 64000/512 (800)    | 51200/768  | 8/16000 |
| Standard_E48s_v3 <sup>1</sup>  | 48 | 384 | 768 | 32 | 96000/768 (1200)   | 76800/1152 | 8/24000 |
| Standard_E64s_v3 <sup>1</sup>  | 64 | 432 | 864 | 32 | 128000/1024 (1600) | 80000/1200 | 8/30000 |
| Standard_E64is_v3 <sup>2</sup> | 64 | 432 | 864 | 32 | 128000/1024 (1600) | 80000/1200 | 8/30000 |

<sup>1</sup> kısıtlı çekirdek boyutu var.

<sup>2</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
