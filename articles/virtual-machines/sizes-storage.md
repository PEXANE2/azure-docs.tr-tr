---
title: Azure VM boyutları - Depolama | Microsoft Dokümanlar
description: Azure'daki sanal makineler için kullanılabilen farklı depolama en iyi duruma getirilmiş boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77913345"
---
# <a name="storage-optimized-virtual-machine-sizes"></a>Depolama optimize edilmiş sanal makine boyutları

Depolama optimize vm boyutları yüksek disk veri ve IO sunar ve Büyük Veri, SQL, NoSQL veritabanları, veri ambarı ve büyük işlem veritabanları için idealdir.  Örnekler Cassandra, MongoDB, Cloudera ve Redis içerir. Bu makalede, her optimize edilmiş boyut için vCPUs, veri diskleri ve NIC'lerin sayısının yanı sıra yerel depolama veri metodu ve ağ bant genişliği hakkında bilgi verilmektedir.

[Lsv2 serisi](lsv2-series.md) yüksek iş kolu, düşük gecikme, doğrudan 2.55GHz tüm çekirdek desteği ve 3.0GHz maksimum destek ile [AMD EPYC<sup>TM</sup> 7551 işlemci](https://www.amd.com/en/products/epyc-7000-series) üzerinde çalışan yerel NVMe depolama eşlenen özellikleri. Lsv2 serisi VM'ler aynı anda çoklu iş parçacığı yapılandırmasında 8 ile 80 vCPU arasında boyutlarda gelir.  VCPU başına 8 GiB bellek ve 8 vCPUs başına bir 1.92TB NVMe SSD M.2 cihazı vardır ve L80s v2'de 19.2 TB'ye (10x1.92TB) kadar bellek mevcuttur.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.

[Windows](windows/storage-performance.md) veya [Linux](linux/storage-performance.md)için Lsv2 serisi sanal makinelerde performansı nasıl optimize edebilirsiniz öğrenin.
