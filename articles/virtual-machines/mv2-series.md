---
title: Mv2 serisi - Azure Sanal Makineler
description: Mv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 04/07/2020
ms.author: lahugh
ms.openlocfilehash: 764dc93608ae3b8882b7048a722c6d3415cbc644
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885795"
---
# <a name="mv2-series"></a>Mv2 serisi

Mv2 serisi, 2,5 GHz'lik tüm çekirdek taban frekansına ve 3,8 GHz maksimum turbo frekansına sahip, hiper dişli Intel® Xeon® Platinum 8180M 2.5GHz (Skylake) işlemciüzerinde çalışan yüksek iş çıkışlı, düşük gecikmelik li platforma sahiptir. Tüm Mv2 serisi sanal makine boyutları hem standart hem de premium kalıcı diskler kullanabilir. Mv2 serisi örnekler, ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi analizler için ideal olan yüksek bellek-CPU oranıyla, büyük bellek içi veritabanlarını ve iş yüklerini desteklemek için benzersiz hesaplama performansı sağlayan bellek en iyi duruma getirilmiş VM boyutlarıdır.

Mv2 serisi VM özelliği Intel® Hiper-Threading Teknolojisi

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

Yazma Hızlandırıcısı: [Desteklenen](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

|Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC ' ler / Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000 / 800 (7040) | 40000 / 1000 | 8 / 16000 |
| Standard_M416ms_v2<sup>1</sup> | 416 | 11400 | 8192 | 64 | 250000 / 1600 (14080) | 80000 / 2000 | 8 / 32000 |
| Standard_M416s_v2<sup>1</sup> | 416 | 5700 | 8192 | 64 | 250000 / 1600 (14080) | 80000 / 2000 | 8 / 32000 |

<sup>1</sup> Mv2 serisi VM yalnızca nesil 2'dir. Linux kullanıyorsanız, bir resmi nasıl bulup seçacağınız hakkında talimatlar için [Azure'daki 2 nesil VM'ler için Destek'e](./linux/generation-2.md) bakın. Windows kullanıyorsanız, bir resmi nasıl bulup seçacağınız hakkında talimatlar için [Azure'da nesil 2 VM'ler için Destek'e](./windows/generation-2.md) bakın. Dört farklı Mv2 serisi VM türü listesi için gerekli minimum işletim sistemi bültenleri:

- Windows Server 2019 veya sonrası
- SUSE Linux Enterprise Server 12 SP4 ve sonrası veya SUSE Linux Enterprise Server 15 SP1 ve sonrası
- Red Hat Enterprise Linux 7.6, 7.7, 8.1 veya sonrası 
- Oracle Enterprise Linux 7.7 veya sonrası



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
