---
title: Av2 Serisi
description: AV2 serisi VM 'Ler için Özellikler.
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: cdcc26a8a22e9a1dc7af75667cdb33bb044c7858
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288586"
---
# <a name="av2-series"></a>Av2 Serisi

AV2 serisi VM 'Ler, çeşitli donanım türlerinde ve işlemcilerde dağıtılabilir. AV2 serisi VM 'Lerde geliştirme ve test gibi giriş düzeyi iş yükleri için en uygun CPU performansı ve bellek yapılandırması vardır. Bu boyut, dağıtılan donanımdan bağımsız olarak, çalışan örnek için tutarlı işlemci performansı sunacak şekilde kısıtlanıyor. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın. Bazı örnek kullanım örnekleri arasında geliştirme ve test sunucuları, düşük trafikli web sunucuları, küçük ve orta ölçekli veritabanları, kavram kanıtı ve kod depoları bulunur.

ACU: 100

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir


| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC | Beklenen ağ bant genişliği (MB/sn)
|---|---|---|---|---|---|---|---|
| Standard_A1_v2  | 1 | 2  | 10 | 1000/20/10  | 2/2x500   | 2 | 250  |
| Standard_A2_v2  | 2 | 4  | 20 | 2000/40/20  | 4/4x500   | 2 | 500  |
| Standard_A4_v2  | 4 | 8  | 40 | 4000/80/40  | 8/8x500   | 4 | 1000 |
| Standard_A8_v2  | 8 | 16 | 80 | 8000/160/80 | 16/16x500 | 8 | 2000 |
| Standard_A2m_v2 | 2 | 16 | 20 | 2000/40/20  | 4/4x500   | 2 | 500  |
| Standard_A4m_v2 | 4 | 32 | 40 | 4000/80/40  | 8/8x500   | 4 | 1000 |
| Standard_A8m_v2 | 8 | 64 | 80 | 8000/160/80 | 16/16x500 | 8 | 2000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Diğer boyutlar ve bilgiler

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

Fiyatlandırma Hesaplayıcı: [Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)

Disk türleri hakkında daha fazla bilgi: [disk türleri](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types#ultra-ssd-preview/)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
