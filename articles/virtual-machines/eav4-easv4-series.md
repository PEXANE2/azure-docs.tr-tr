---
title: Eav4-Series ve Easv4 serisi-Azure sanal makineleri
description: Eav4 ve Easv4 serisi VM 'Ler için Özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 96cd7b4f4627e7c0335df9c745d8193a3dac7044
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77493780"
---
# <a name="eav4-and-easv4-series"></a>Eav4 ve Easv4 serisi

Eav4-Series ve Easv4 serisi, en fazla 256 MB boyutlu önbellek kullanan çok iş parçacıklı bir yapılandırmada AMD 'nin 2.35 GHz EPYıC<sup>TM</sup> 7452 işlemcisini kullanır ve en fazla bellek için iyileştirilmiş iş yüklerini çalıştırmaya yönelik seçenekleri artırır. Eav4-Series ve Easv4 serisi, Ev3 & Esv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

## <a name="eav4-series"></a>Eav4 serisi

ACU: 230-260

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Eav4 serisi Boyutlar, 3.35 GHz 'nin en yüksek frekansını ve Premium SSD 'yi kullanmayı sağlayan 2.35 GHz AMD EPIC<sup>TM</sup> 7452 işlemcisini temel alır. Eav4 serisi Boyutlar, bellek açısından yoğun kurumsal uygulamalar için idealdir. Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium SSD 'yi kullanmak için Easv4-Series boyutlarını kullanın. Easv4 boyutları için fiyatlandırma ve faturalandırma ölçümleri Eav3 serisi ile aynıdır.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | En fazla NIC/beklenen ağ bant genişliği (MBps) |
| -----|-----|-----|-----|-----|-----|-----|
| Standart\_E2a\_v4|2|16|50|4|3000/46/23|2 / 1000 |
| Standart\_E4a\_v4|4|32|100|8|6000/93/46|2 / 2000 |
| Standart\_E8a\_v4|8|64|200|16|12000/187/93|4 / 4000 |
| Standart\_E16a\_v4|16|128|400|32|24000/375/187|8 / 8000 |
| Standart\_E20a\_v4|20|160|500|32|30000/468/234|8 / 10000 |
| Standart\_E32a\_v4|32|256|800|32|48000/750/375|8 / 16000 |
| Standart\_E48a\_v4 <sup>**</sup> |48|384|1200|32| | |
| Standart\_E64a\_v4 <sup>**</sup> |64|512|1600|32| | |
| Standart\_E96a\_v4 <sup>**</sup> |96|672|2400|32| | |

<sup>**</sup>  Bu boyutlar önizlemededir. Bu büyük boyutları denemek istiyorsanız [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)' de kaydolun.

## <a name="easv4-series"></a>Easv4 serisi

ACU: 230-260

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Easv4 serisi Boyutlar, 3.35 GHz 'nin en yüksek frekansını ve Premium SSD 'yi kullanmayı sağlayan 2.35 GHz AMD EPIC<sup>TM</sup> 7452 işlemcisini temel alır. Easv4 serisi Boyutlar, bellek açısından yoğun kurumsal uygulamalar için idealdir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (MBps) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_E2as_v4|2|16|32|4|4000/32 (50)|3200/48|2 / 1000 |
| Standard_E4as_v4|4|32|64|8|8000/64 (100)|6400/96|2 / 2000 |
| Standard_E8as_v4|8|64|128|16|16000/128 (200)|12800/192|4 / 4000 |
| Standard_E16as_v4|16|128|256|32|32000/255 (400)|25600/384|8 / 8000 |
| Standard_E20as_v4|20|160|320|32|40000/320 (500)|32000/480|8 / 10000 |
| Standard_E32as_v4|32|256|512|32|64000/510 (800)|51200/768|8 / 16000 |
| Standard_E48as_v4 <sup>**</sup> |48|384|768|32|  | |
| Standard_E64as_v4 <sup>**</sup> |64|512|1024|32| | |
| Standard_E96as_v4 <sup>**</sup> |96|672|1344|32| | |  

<sup>**</sup>  Bu boyutlar önizlemededir. Bu büyük boyutları denemek istiyorsanız [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)' de kaydolun.

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