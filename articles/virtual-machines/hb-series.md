---
title: HB serisi - Azure Sanal Makineler
description: HB serisi VM'ler için teknik özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 600f10e81742e9bb66c800b747fd7b2dc062754d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164840"
---
# <a name="hb-series"></a>HB serisi

HB serisi VM'ler, akışkan dinamiği, açık sonlu elemanlar analizi ve hava durumu modellemesi gibi bellek bant genişliğine bağlı uygulamalar için optimize edilmiştir. HB VM'lerde 60 AMD EPYC 7551 işlemci çekirdeği, CPU çekirdeği başına 4 GB RAM ve eşzamanlı çoklu iş parçacığı yoktur. HB VM 260 GB/sn'ye kadar bellek bant genişliği sağlar.

ACU: 199-216

Premium Depolama: Desteklenen

Premium Depolama Önbelleğe Alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/s | Temel CPU frekansı (GHz) | Tüm çekirdek frekansı (GHz, tepe) | Tek çekirdekli frekans (GHz, tepe) | RDMA performansı (Gb/s) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | Maksimum Ethernet NIC'leri |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2,0 | 2.55 | 2.55 | 100 | Tümü | 700 | 4 | 1 |

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