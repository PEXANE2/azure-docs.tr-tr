---
title: Mv2-Series-Azure sanal makineleri
description: Mv2 serisi VM 'Ler için Özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: caa80443a189874bfd699dba412fb749c8711556
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77493403"
---
# <a name="mv2-series"></a>Mv2 serisi

Mv2-Series, bir hiper iş parçacıklı Intel® Xeon® Platinum 81805 2.5 GHz (ufuk Gölü) işlemcisi üzerinde çalışan yüksek performans, 2,5 GHz ve en fazla Turbo frekansı 3,8 GHz ile çalışır. Tüm Mv2 serisi sanal makine boyutları, hem standart hem de Premium kalıcı diskler kullanabilir. Mv2 serisi örnekler, büyük bellek içi veritabanlarını ve iş yüklerini desteklemek için benzersiz işlem performansı sağlayan bellek için iyileştirilmiş VM boyutlarıdır ve ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi için ideal olan yüksek bellekle CPU oranıyla desteklenir analiz.

Mv2 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Yazma Hızlandırıcısı: [destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

|Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |
| Standard_M416ms_v2<sup>1, 2</sup> | 416 | 11400 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 / 32000 |
| Standard_M416s_v2<sup>1, 2</sup> | 416 | 5700 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 / 32000 |

<sup>1</sup> Mv2 serisi VM 'ler yalnızca 2. kuşak. Linux kullanıyorsanız, bir görüntüyü bulma ve seçme hakkında yönergeler için bkz. [Azure 'da 2. nesil sanal makineler Için destek](/linux/generation-2.md) .

<sup>2</sup> M416ms_v2 ve M416s_v2 boyutları için yalnızca şu görüntü için başlangıç desteğinin olduğunu unutmayın: "GEN2: SUSE Linux Enterprise Server (SLES) 12 SP4 for SAP Applications."

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