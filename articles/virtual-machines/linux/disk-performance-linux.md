---
title: Sanal makine ve disk performansı
description: VM 'Lerin ve bağlı disklerinin performans için birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 092368bb66784a00d5116da0b6be6513f8ebb261
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518098"
---
# <a name="virtual-machine-and-disk-performance"></a>Sanal makine ve disk performansı
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Önbelleğe alınmamış ve önbelleğe alınan sanal makine sınırları
Hem Premium Depolama hem de Premium depolama önbelleği için etkinleştirilen sanal makinelerin iki farklı depolama bant genişliği sınırı vardır. Örnek olarak Standard_D8s_v3 sanal makinesine göz atalım. [Dsv3-Series](../dv3-dsv3-series.md) ve Standard_D8s_v3 ile ilgili belgeler aşağıda verilmiştir:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Bu sanal makine ve GÇ etkinliği oluşturan disk birleşimi üzerinde bir sınama testi çalıştıralım. Azure 'da depolama GÇ 'yi kıyaslama hakkında bilgi edinmek için bkz. [Azure disk depolama uygulamanızı kıyaslama](disks-benchmarks.md). Sınama aracı ' ndan, VM ve disk birleşiminin 22.800 ıOPS ile elde olduğunu görebilirsiniz:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]
