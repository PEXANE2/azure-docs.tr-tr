---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 9000cab026ec08bd78d8a8f334c8967a8a747c95
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202300"
---
Şimdilik yalnızca Premium SSD 'Ler Paylaşılan diskleri etkinleştirebilir. Bu özelliği destekleyen disk boyutları P15 ve daha büyüktür. Farklı disk boyutları farklı bir `maxShares` sınırına sahip olabilir ve bu, `maxShares` değerini ayarlarken aşamazsınız.

Her disk için, diski eşzamanlı olarak paylaşabilen en fazla düğüm sayısını temsil eden bir `maxShares` değeri tanımlayabilirsiniz. Örneğin, 2 düğümlü bir yük devretme kümesi ayarlamayı planlıyorsanız `maxShares=2`ayarlamanız gerekir. En büyük değer, üst sınırdır. Düğüm sayısı belirtilen `maxShares` değerinden düşük olduğu sürece, düğümler kümeye katılabilir veya kümeyi bırakabilir (diski bağlama veya çıkarma).

> [!NOTE]
> `maxShares` değeri yalnızca disk tüm düğümlerden ayrıldığında ayarlanabilir veya düzenlenebilir.

Aşağıdaki tabloda, disk boyutuna göre `maxShares` için izin verilen en büyük değerler gösterilmektedir:

|Disk boyutları  |maxShares sınırı  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Bir diskin ıOPS ve bant genişliği sınırları `maxShares` değerinden etkilenmez. Örneğin, P15 diskinin maksimum ıOPS değeri, maxShares = 1 veya maxShares > 1 ' in 1100 ' dir.