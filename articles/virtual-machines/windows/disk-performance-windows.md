---
title: Sanal makine ve disk performansı
description: VM 'Lerin ve bağlı disklerinin performans için birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin
author: albecker1
ms.author: albecker
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 88cf9c28a9d325d617f4b049015f0cd238a2fb31
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016555"
---
# <a name="virtual-machine-and-disk-performance"></a>Sanal makine ve disk performansı
[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance.md)]

## <a name="virtual-machine-uncached-vs-cached-limits"></a>Önbelleğe alınmamış ve önbelleğe alınan sanal makine sınırları
 Hem Premium Storage hem de Premium depolama önbelleği etkin olan sanal makinelerin iki farklı depolama bant genişliği sınırı vardır. Örnek olarak Standard_D8s_v3 sanal makinesine bakmaya devam edelim. İşte [Dsv3-Series](../dv3-dsv3-series.md) ve bu belgede Standard_D8s_v3:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-2.md)]

Bu VM 'de, GÇ etkinliği oluşturmaya yönelik bir sınama testi çalıştıralım ve Azure 'da depolama ıO 'yu [burada](disks-benchmarks.md)nasıl kıyaslama hakkında bilgi edinebilirsiniz. Benchişaretleme aracından, VM 'nin ve disk birleşiminin 22.800 ıOPS elde edebileceğini görebilirsiniz:

[!INCLUDE [VM and Disk Performance](../../../includes/virtual-machine-disk-performance-3.md)]