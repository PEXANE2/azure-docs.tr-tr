---
title: Eav4 serisi ve Easv4 serisi - Azure Sanal Makineler
description: Eav4 ve Easv4 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: migerdes
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: e05a2b97a25ce4cab7d619f4b62d405765cb08db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164398"
---
# <a name="eav4-and-easv4-series"></a>Eav4 ve Easv4 serisi

Eav4 serisi ve Easv4 serisi, AMD'nin 2.35Ghz EPYC<sup>TM</sup> 7452 işlemcisini 256 MB L3 önbelleğe sahip çok iş parçacığı yapılandırmasında kullanarak en çok bellek için optimize edilmiş iş yükünü çalıştırma seçeneklerini artırır. Eav4 serisi ve Easv4 serisi, Ev3 & Esv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

## <a name="eav4-series"></a>Eav4 serisi

ACU: 230 - 260

Premium Depolama: Desteklenmiyor

Premium Depolama önbelleğe alma: Desteklenmiyor

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

Eav4 serisi boyutları 3.35GHz artırılmış maksimum frekans elde edebilir ve prim SSD kullanabilirsiniz 2.35Ghz AMD EPYC<sup>TM</sup> 7452 işlemci dayanmaktadır. Eav4 serisi boyutları bellek yoğun kurumsal uygulamalar için idealdir. Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium SSD kullanmak için Easv4 serisi boyutlarını kullanın. Easv4 boyutları için fiyatlandırma ve faturalandırma sayaçları Eav3 serisi ile aynıdır.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Max NIC ' ler / Beklenen ağ bant genişliği (MBps) |
| -----|-----|-----|-----|-----|-----|-----|
| Standart\_E2a\_v4|2|16|50|4|3000/46/23|2 / 1000 |
| Standart\_E4a\_v4|4|32|100|8|6000/93/46|2 / 2000 |
| Standart\_E8a\_v4|8|64|200|16|12000/187/93|4 / 4000 |
| Standart\_E16a\_v4|16|128|400|32|24000/375/187|8 / 8000 |
| Standart\_E20a\_v4|20|160|500|32|30000 / 468 / 234|8 / 10000 |
| Standart\_E32a\_v4|32|256|800|32|48000/750/375|8 / 16000 |
| Standart\_E48a\_v4<sup>**</sup> |48|384|1200|32| | |
| Standart\_E64a\_v4<sup>**</sup> |64|512|1600|32| | |
| Standart\_E96a\_v4<sup>**</sup> |96|672|2400|32| | |

<sup>**</sup>Bu boyutlar Önizleme'de bulunmaktadır. Bu büyük boyutları denemek istiyorsanız, kaydolun. [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)

## <a name="easv4-series"></a>Easv4 serisi

ACU: 230 - 260

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

Easv4 serisi boyutları 3.35GHz artırılmış maksimum frekans elde edebilir ve prim SSD kullanabilirsiniz 2.35Ghz AMD EPYC<sup>TM</sup> 7452 işlemci dayanmaktadır. Easv4 serisi boyutlar bellek yoğun kurumsal uygulamalar için idealdir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC ' ler / Beklenen ağ bant genişliği (MBps) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_E2as_v4|2|16|32|4|4000 / 32 (50)|3200 / 48|2 / 1000 |
| Standard_E4as_v4|4|32|64|8|8000 / 64 (100)|6400 / 96|2 / 2000 |
| Standard_E8as_v4|8|64|128|16|16000 / 128 (200)|12800 / 192|4 / 4000 |
| Standard_E16as_v4|16|128|256|32|32000 / 255 (400)|25600 / 384|8 / 8000 |
| Standard_E20as_v4|20|160|320|32|40000 / 320 (500)|32000 / 480|8 / 10000 |
| Standard_E32as_v4|32|256|512|32|64000 / 510 (800)|51200 / 768|8 / 16000 |
| Standard_E48as_v4<sup>**</sup> |48|384|768|32|  | |
| Standard_E64as_v4<sup>**</sup> |64|512|1024|32| | |
| Standard_E96as_v4<sup>**</sup> |96|672|1344|32| | |  

<sup>**</sup>Bu boyutlar Önizleme'de bulunmaktadır. Bu büyük boyutları denemek istiyorsanız, kaydolun. [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)

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
