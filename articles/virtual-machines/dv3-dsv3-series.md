---
title: Dv3 ve Dsv3 serisi - Azure Sanal Makineler
description: Dv3 ve Dsv3 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: joelpelley
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 9ea5811fa20ea4866655de74d79ff3905ba03f16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164415"
---
# <a name="dv3-and-dsv3-series"></a>Dv3 ve Dsv3 serisi

Dv3 serisi Intel® Xeon® 8171M 2.1GHz (Skylake), Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemciler üzerinde çalışır, en genel için daha iyi bir değer teklifi sağlayan amaç iş yükleri. Bellek genişletildi (~3.5 GiB/vCPU'dan 4 GiB/vCPU'ya) disk ve ağ sınırları ise hiperiş parçacığına geçmek için çekirdek bazında ayarlandı. Dv3 serisi artık D/Dv2 serisi yüksek bellek VM boyutlarına sahip, bu bellek optimize [Ev3 ve Esv3 serisi](ev3-esv3-series.md)taşındı .

Örnek D serisi kullanım örnekleri, kurumsal dereceli uygulamaları, ilişkisel veritabanlarını, bellek içi önbelleğe alma ve analizleri içerir.

## <a name="dv3-series"></a>Dv3 serisi

Dv3 serisi boyutları Intel Turbo Boost Technology 2.0 ile Intel® Xeon® 8171M 2.1GHz (Skylake), Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcilerinde çalışır. Dv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için Dsv3 boyutlarını kullanın. Dsv3 boyutları için fiyatlandırma ve faturalandırma oranları Dv3 serisi ile aynıdır.

Dv3 serisi VM'ler Intel® Hiper-Threading Teknolojisine sahiptir.

ACU: 160-190

Premium Depolama: Desteklenmiyor

Premium Depolama önbelleğe alma: Desteklenmiyor

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama iş boku: IOPS/Okuma MBps/Yazma MBps | Max NIC'ler/Ağ bant genişliği |
|---|---|---|---|---|---|---|
| Standard_D2_v3  | 2  | 8   | 50   | 4  | 3000/46/23     | 2/1000  |
| Standard_D4_v3  | 4  | 16  | 100  | 8  | 6000/93/46     | 2/2000  |
| Standard_D8_v3  | 8  | 32  | 200  | 16 | 12000/187/93   | 4/4000  |
| Standard_D16_v3 | 16 | 64  | 400  | 32 | 24000/375/187  | 8/8000  |
| Standard_D32_v3 | 32 | 128 | 800  | 32 | 48000/750/375  | 8/16000 |
| Standard_D48_v3 | 48 | 192 | 1200 | 32 | 96000/1000/500 | 8/24000 |
| Standard_D64_v3 | 64 | 256 | 1600 | 32 | 96000/1000/500 | 8/30000 |

## <a name="dsv3-series"></a>Dsv3 serisi

Dsv3 serisi boyutları Intel® Xeon® 8171M 2.1GHz (Skylake), Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcileri intel Turbo Boost Technology 2.0 ile çalışır ve premium depolama kullanır. Dsv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.

Dsv3 serisi VM'ler Intel® Hiper İş Parçacığı Teknolojisine sahiptir.

ACU: 160-190

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama işliği: IOPS/MBps (GiB'de önbellek boyutu) | Maksimum cached disk işçıktısı: IOPS/MBps | Max NIC'ler/Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_D2s_v3  | 2  | 8   | 16  | 4  | 4000/32 (50)       | 3200/48    | 2/1000  |
| Standard_D4s_v3  | 4  | 16  | 32  | 8  | 8000/64 (100)      | 6400/96    | 2/2000  |
| Standard_D8s_v3  | 8  | 32  | 64  | 16 | 16000/128 (200)    | 12800/192  | 4/4000  |
| Standard_D16s_v3 | 16 | 64  | 128 | 32 | 32000/256 (400)    | 25600/384  | 8/8000  |
| Standard_D32s_v3 | 32 | 128 | 256 | 32 | 64000/512 (800)    | 51200/768  | 8/16000 |
| Standard_D48s_v3 | 48 | 192 | 384 | 32 | 96000/768 (1200)   | 76800/1152 | 8/24000 |
| Standard_D64s_v3 | 64 | 256 | 512 | 32 | 128000/1024 (1600) | 80000/1200 | 8/30000 |

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
