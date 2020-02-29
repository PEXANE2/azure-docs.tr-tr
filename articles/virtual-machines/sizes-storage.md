---
title: Azure VM boyutları-depolama | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilen farklı depolama için iyileştirilmiş boyutları listeler. Bu serideki boyutlarda sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: sasha-melamed
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 5a611dc288fc18a14f6000689f9f9b49d4b3feb8
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77913345"
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
