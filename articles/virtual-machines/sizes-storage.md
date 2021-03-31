---
title: Azure VM boyutları-depolama | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilen farklı depolama için iyileştirilmiş boyutları listeler. Bu serideki boyutlarda sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
ms.subservice: vm-sizes-storage
documentationcenter: ''
author: sasha-melamed
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: d97fe6cda1134d45468e257965fd5e28fe170e6f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102561033"
---
# <a name="storage-optimized-virtual-machine-sizes"></a>Depolama için iyileştirilmiş sanal makine boyutları

Depolama için iyileştirilmiş VM boyutları, yüksek disk verimlilik ve GÇ sağlar ve büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için idealdir.  Cassandra, MongoDB, Cloudera ve Redsıs örnekleri sayılabilir. Bu makalede, her iyileştirilmiş boyut için sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra yerel depolama alanı işleme ve ağ bant genişliği hakkında bilgi sağlanır.

[Lsv2-Series](lsv2-series.md) , yüksek performans, düşük gecikme süresi ve [AMD Epic<sup>TM</sup> 7551 işlemcisi](https://www.amd.com/en/products/epyc-7000-series) üzerinde çalışan yerel NVMe depolama alanını, 2.55 GHz 'nin tüm çekirdek artışı ve en fazla 3,0 GHz sürümü ile birlikte sunar. Lsv2 serisi VM 'Ler, eşzamanlı bir çoklu iş parçacığı yapılandırmasında 8 ile 80 vCPU boyutunda olacak şekilde gelir.  Her vCPU için 8 GiB bellek ve 8 vCPU başına, L80s v2 'de bulunan 19.2 TB 'a kadar (10 x 1.92 TB) bir 1.92 TB NVMe SSD M. 2 cihaz vardır.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

[Windows](windows/storage-performance.md) veya [Linux](linux/storage-performance.md)için Lsv2 serisi sanal makinelerde performansı en üst düzeye çıkarmak hakkında bilgi edinin.

Azure 'un VM 'lerini nasıl adlandırmasının hakkında daha fazla bilgi için bkz. [Azure sanal makine boyutları adlandırma kuralları](./vm-naming-conventions.md).