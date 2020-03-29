---
title: Mv2 serisi - Azure Sanal Makineler
description: Mv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 6fb0f93d3ac124b21bbc52ddc57bc720de6406e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163905"
---
# <a name="mv2-series"></a>Mv2 serisi

Mv2 serisi, 2,5 GHz'lik tüm çekirdek taban frekansına ve 3,8 GHz maksimum turbo frekansına sahip, hiper dişli Intel® Xeon® Platinum 8180M 2.5GHz (Skylake) işlemciüzerinde çalışan yüksek iş çıkışlı, düşük gecikmelik li platforma sahiptir. Tüm Mv2 serisi sanal makine boyutları hem standart hem de premium kalıcı diskler kullanabilir. Mv2 serisi örnekler, ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi için ideal olan yüksek bellek-CPU oranıyla, büyük bellek veritabanlarını ve iş yüklerini desteklemek için benzersiz hesaplama performansı sağlayan bellek en iyi duruma getirilmiş VM boyutlarıdır Analytics.

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
| Standard_M416ms_v2<sup>1, 2</sup> | 416 | 11400 | 8192 | 64 | 250000 / 1600 (14080) | 80000 / 2000 | 8 / 32000 |
| Standard_M416s_v2<sup>1, 2</sup> | 416 | 5700 | 8192 | 64 | 250000 / 1600 (14080) | 80000 / 2000 | 8 / 32000 |

<sup>1</sup> Mv2 serisi VM yalnızca nesil 2'dir. Linux kullanıyorsanız, bir resmi nasıl bulup seçacağınız hakkında talimatlar için [Azure'daki 2 nesil VM'ler için Destek'e](./linux/generation-2.md) bakın.

<sup>2</sup> M416ms_v2 ve M416s_v2 boyutları için, yalnızca aşağıdaki görüntü için ilk desteğin olduğunu unutmayın: "GEN2: SUSE Linux Enterprise Server (SLES) SAP Uygulamaları için 12 SP4."

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
