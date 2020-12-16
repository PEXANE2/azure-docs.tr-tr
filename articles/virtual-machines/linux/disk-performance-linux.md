---
title: Sanal makine ve disk performansı-Linux
description: Sanal makinelerin ve bağlı disklerinin Linux üzerinde performans için birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 2c48672bcfd8c552b36e3ae0807135924669c1d9
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97591863"
---
# <a name="virtual-machine-and-disk-performance-linux"></a>Sanal makine ve disk performansı (Linux)
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Önbelleğe alınmamış ve önbelleğe alınan sanal makine sınırları
Hem Premium Depolama hem de Premium depolama önbelleği için etkinleştirilen sanal makinelerin iki farklı depolama bant genişliği sınırı vardır. Örnek olarak Standard_D8s_v3 sanal makinesine göz atalım. [Dsv3-Series](../dv3-dsv3-series.md) ve Standard_D8s_v3 ile ilgili belgeler aşağıda verilmiştir:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Bu sanal makine ve GÇ etkinliği oluşturan disk birleşimi üzerinde bir sınama testi çalıştıralım. Azure 'da depolama GÇ 'yi kıyaslama hakkında bilgi edinmek için bkz. [Azure disk depolama uygulamanızı kıyaslama](disks-benchmarks.md). Sınama aracı ' ndan, VM ve disk birleşiminin 22.800 ıOPS ile elde olduğunu görebilirsiniz:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
