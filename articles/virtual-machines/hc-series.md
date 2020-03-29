---
title: HC serisi - Azure Sanal Makineler
description: HC serisi VM'ler için teknik özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: cc25fb9b21d535ef07bcfae673be48216427b370
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164789"
---
# <a name="hc-series"></a>HC serisi

HC serisi VM'ler, örtük sonlu elemananalizi, moleküler dinamikler ve hesaplamalı kimya gibi yoğun hesaplamayla yönlendirilen uygulamalar için optimize edilmiştir. HC VM'lerde 44 Intel Xeon Platinum 8168 işlemci çekirdeği, CPU çekirdeği başına 8 GB RAM ve hiperiş ipliği yoktur. Intel Xeon Platinum platformu, Intel Math Kernel Library gibi yazılım araçlarından oluşan zengin ekosistemini destekler.

ACU: 297-315

Premium Depolama: Desteklenen

Premium Depolama Önbelleğe Alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/s | Temel CPU frekansı (GHz) | Tüm çekirdek frekansı (GHz, tepe) | Tek çekirdekli frekans (GHz, tepe) | RDMA performansı (Gb/s) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | Maksimum Ethernet NIC'leri |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platin 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | Tümü | 700 | 4 | 1 |

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