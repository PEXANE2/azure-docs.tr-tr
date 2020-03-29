---
title: HBv2 serisi - Azure Sanal Makineler
description: HBv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: amverma
ms.openlocfilehash: eea649610ca53ccbb98b5ca361555280dcd3dafe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164806"
---
# <a name="hbv2-series"></a>HBv2 serisi

HBv2 serisi VM'ler, akışkandin dinamiği, sonlu elemanlar analizi ve rezervuar simülasyonu gibi bellek bant genişliğine göre yönlendirilen uygulamalar için optimize edilmiştir. HBv2 VM'lerde 120 AMD EPYC 7742 işlemci çekirdeği, CPU çekirdeği başına 4 GB RAM ve eşzamanlı çoklu iş parçacığı yoktur. Her HBv2 VM 340 GB/sn bellek bant genişliği ve 4 teraFLOPS kadar FP64 bilgisayar sağlar.

Premium Depolama: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/s | Temel CPU frekansı (GHz) | Tüm çekirdek frekansı (GHz, tepe) | Tek çekirdekli frekans (GHz, tepe) | RDMA performansı (Gb/s) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | Maksimum Ethernet NIC'leri |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs_v2 | 120 | AMD EPYC 7V12 | 480 | 350 | 2.45 | 3.1 | 3.3 | 200 | Tümü | 480 + 960 | 8 | 1 |


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