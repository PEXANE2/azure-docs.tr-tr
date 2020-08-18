---
title: Mv2-Series-Azure sanal makineleri
description: Mv2 serisi VM 'Ler için Özellikler.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: f6dc732a512368b007a289c3aaabc3123a696d96
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88505266"
---
# <a name="mv2-series"></a>Mv2 serisi

Mv2-Series, bir hiper iş parçacıklı Intel® Xeon® Platinum 81805 2.5 GHz (ufuk Gölü) işlemcisi üzerinde çalışan yüksek performans, 2,5 GHz ve en fazla Turbo frekansı 3,8 GHz ile çalışır. Tüm Mv2 serisi sanal makine boyutları, hem standart hem de Premium kalıcı diskler kullanabilir. Mv2 serisi örnekler, ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi analiz için ideal olan yüksek bellek içi veritabanlarını ve iş yüklerini desteklemek üzere benzersiz işlem performansı sağlayan bellek için iyileştirilmiş VM boyutlarıdır.

Mv2 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

Yazma Hızlandırıcısı: [destekleniyor](./how-to-enable-write-accelerator.md)

|Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC | Beklenen ağ bant genişliği (MB/sn) |
|---|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M416ms_v2<sup>1</sup> | 416 | 11400 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |
| Standard_M416s_v2<sup>1</sup> | 416 | 5700 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |

<sup>1</sup> Mv2 serisi VM 'ler yalnızca 2. nesil ve 2. nesil desteklenen görüntülerin bir alt kümesini destekler. Mv2-Series için desteklenen görüntülerin tam listesi için lütfen aşağıya bakın. Linux kullanıyorsanız, bir görüntüyü bulma ve seçme hakkında yönergeler için bkz. [Azure 'da 2. nesil sanal makineler Için destek](./linux/generation-2.md) . Windows kullanıyorsanız, bir görüntüyü bulma ve seçme hakkında yönergeler için bkz. [Azure 'da 2. nesil sanal makineler Için destek](./windows/generation-2.md) . 

- Windows Server 2019 veya üzeri
- SUSE Linux Enterprise Server 12 SP4 ve üzeri ya da SUSE Linux Enterprise Server 15 SP1 ve üzeri
- Red Hat Enterprise Linux 7,6, 7,7, 8,1 veya üzeri 
- Oracle Enterprise Linux 7,7 veya üzeri



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Diğer boyutlar ve bilgiler

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

Fiyatlandırma Hesaplayıcı: [Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)

Disk türleri hakkında daha fazla bilgi: [disk türleri](./linux/disks-types.md#ultra-disk)


## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.