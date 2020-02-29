---
title: Dav4 ve Dasv4 serisi-Azure sanal makineleri
description: Dav4 ve Dasv4 serisi VM 'Ler için Özellikler.
services: virtual-machines
author: migerdes
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 809843bddc4216df4dab44330172d36778248b1a
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78162188"
---
# <a name="dav4-and-dasv4-series"></a>Dav4 ve Dasv4 serisi

Dav4-Series ve Dasv4 serisi, çok iş parçacıklı bir yapılandırmada, 128 MB 'a kadar L3 önbellek kullanan<sup>yeni 7452</sup> boyutlardır ve her 8 çekirdeğe, genel amaçlı iş yüklerini çalıştırmaya yönelik müşteri seçeneklerini artırarak, en fazla 8 ' 256 e varan bir yapılandırma. Dav4-Series ve Dasv4 serisi, D & Dsv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

## <a name="dav4-series"></a>Dav4 serisi

ACU: 230-260

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

Dav4 serisi Boyutlar, 3.35 GHz 'nin en yüksek frekansını elde etmeye yönelik 2.35 GHz AMD EPıC<sup>TM</sup> 7452 işlemcisini temel alır. Dav4 serisi boyutları, üretim iş yüklerinin çoğu için vCPU, bellek ve geçici depolama alanı birleşimini sunar. Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium SSD 'yi kullanmak için Dasv4 boyutlarını kullanın. Dasv4 boyutları için fiyatlandırma ve faturalandırma ölçümleri Dav4 serisi ile aynıdır.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | En fazla NIC/beklenen ağ bant genişliği (MBps) |
|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000/46/23   | 2 / 1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000/93/46   | 2 / 2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000/187/93 | 4 / 4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000/375/187 |8 / 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000/750/375 |8 / 16000 |
| Standard_D48a_v4 <sup>**</sup> | 48 | 192| 1200 | 32 | | |
| Standard_D64a_v4 <sup>**</sup> | 64 | 256 | 1600 | 32 | | |
| Standard_D96a_v4 <sup>**</sup> | 96 | 384 | 2400 | 32 | | |

<sup>**</sup> Bu boyutlar önizlemededir.  Bu büyük boyutları denemek istiyorsanız [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)' de kaydolun.

## <a name="dasv4-series"></a>Dasv4 serisi

ACU: 230-260

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

Dasv4 serisi Boyutlar, 3.35 GHz 'nin en yüksek frekansını ve Premium SSD 'yi kullanmayı sağlayan 2.35 GHz AMD EPIC<sup>TM</sup> 7452 işlemcisini temel alır. Dasv4 serisi boyutları, üretim iş yüklerinin çoğu için vCPU, bellek ve geçici depolama alanı birleşimini sunar.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (MBps) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000/32 (50)|3200/48|2 / 1000 |
| Standard_D4as_v4|4|16|32|8|8000/64 (100)|6400/96|2 / 2000 |
| Standard_D8as_v4|8|32|64|16|16000/128 (200)|12800/192|4 / 4000 |
| Standard_D16as_v4|16|64|128|32|32000/255 (400)|25600/384|8 / 8000 |
| Standard_D32as_v4|32|128|256|32|64000/510 (800)|51200/768|8 / 16000 |
| Standard_D48as_v4 <sup>**</sup>|48|192|384|32| | | 
| Standard_D64as_v4 <sup>**</sup>|64|256|512|32| | | 
| Standard_D96as_v4 <sup>**</sup>|96|384|768|32| | | 

<sup>**</sup> Bu boyutlar önizlemededir.  Bu büyük boyutları denemek istiyorsanız [https://aka.ms/AzureAMDLargeVMPreview](https://aka.ms/AzureAMDLargeVMPreview)' de kaydolun.

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
