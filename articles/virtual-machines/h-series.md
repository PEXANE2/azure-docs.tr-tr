---
title: H serisi-Azure sanal makineleri
description: H serisi VM 'Ler için belirtim.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/04/2020
ms.author: lahugh
ms.openlocfilehash: 6654506a1e53165ef0891ba0de32a7937c21c904
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78164823"
---
# <a name="h-series"></a>H Serisi

H serisi VM 'Ler, yüksek CPU sıklıklarca veya çekirdek gereksinimlerine göre büyük bellek kullanan uygulamalar için iyileştirilmiştir. H serisi VM 'Ler özelliği 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdekleri, CPU çekirdeği başına en fazla 14 GB RAM ve hiper iş parçacığı yok. Uyumlu RDMA performansı için engelleyici olmayan bir FAT ağacı yapılandırmasında 56 GB/sn Mellanox FDR InfiniBand içindeki H Serisi özellikleri. H serisi VM 'Ler Intel MPı 5. x ve MS-MPı 'yi destekler.

ACU: 290-300

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek bant genişliği GB/sn | Taban CPU sıklığı (GHz) | Tüm çekirdekler sıklığı (GHz, tepe) | Tek çekirdekli sıklık (GHz, tepe) | RDMA performansı (GB/sn) | MPı desteği | Geçici depolama (GB) | Maksimum veri diskleri | En fazla Ethernet NIC |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8   | 8  | Intel Xeon E5 2667 v3 | 56 | 40 | 3,2 | 3.3 | 3.6 | - | Intel 5. x, MS-MPı | 1000 | 32 | 2 |
| Standard_H16  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3,2 | 3.3 | 3.6 | - | Intel 5. x, MS-MPı | 2000 | 64 | 4 |
| Standard_H8m  | 8  | Intel Xeon E5 2667 v3 | 112 | 40 | 3,2 | 3.3 | 3.6 | - | Intel 5. x, MS-MPı | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3,2 | 3.3 | 3.6 | - | Intel 5. x, MS-MPı | 2000 | 64 | 4 |
| Standard_H16r <sup>1</sup>  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3,2 | 3.3 | 3.6 | 56 | Intel 5. x, MS-MPı | 2000 | 64 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3,2 | 3.3 | 3.6 | 56 | Intel 5. x, MS-MPı | 2000 | 64 | 4 |

<sup>1</sup> MPI uygulamaları için, adanmış RDMA arka uç ağı, FDR InfiniBand ağı tarafından etkinleştirilir.

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