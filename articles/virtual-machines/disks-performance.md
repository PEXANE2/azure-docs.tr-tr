---
title: Sanal makine ve disk performansı
description: Sanal makinelerin ve bağlı disklerinin performans için birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin.
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 5e9f6ecc733eccf317e3013752ee2f5b0586ea78
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540762"
---
# <a name="virtual-machine-and-disk-performance"></a>Sanal makine ve disk performansı
[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Önbelleğe alınmamış ve önbelleğe alınan sanal makine sınırları
Hem Premium Depolama hem de Premium depolama önbelleği için etkinleştirilen sanal makinelerin iki farklı depolama bant genişliği sınırı vardır. Örnek olarak Standard_D8s_v3 sanal makinesine göz atalım. [Dsv3-Series](dv3-dsv3-series.md) ve Standard_D8s_v3 ile ilgili belgeler aşağıda verilmiştir:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-2.md)]

Bu sanal makine ve GÇ etkinliği oluşturan disk birleşimi üzerinde bir sınama testi çalıştıralım. Azure 'da depolama GÇ 'yi kıyaslama hakkında bilgi edinmek için bkz. [Azure disk depolama uygulamanızı kıyaslama](disks-benchmarks.md). Sınama aracı ' ndan, VM ve disk birleşiminin 22.800 ıOPS ile elde olduğunu görebilirsiniz:

[!INCLUDE [VM and Disk Performance](../../includes/virtual-machine-disk-performance-3.md)]
