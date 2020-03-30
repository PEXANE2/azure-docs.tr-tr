---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 34699ed89e79448d66343021dd624cb872d0172d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77471729"
---
Şimdilik, paylaşılan diskleri yalnızca premium SSD'ler etkinleştirebilir. Bu özelliği destekleyen disk boyutları P15 ve daha büyüktür. Farklı disk boyutları, değeri `maxShares` ayarlarken aşamadığınız farklı `maxShares` bir sınıra sahip olabilir.

Her disk için, diski aynı anda paylaşabilecek en fazla düğüm sayısını temsil eden bir `maxShares` değer tanımlayabilirsiniz. Örneğin, 2 düğümlü bir başarısız küme ayarlamayı planlıyorsanız, `maxShares=2`. Maksimum değer bir üst sınırdır. Düğüm sayısı belirtilen `maxShares` değerden daha düşük olduğu sürece düğümler kümeye katılabilir veya kümeden çıkabilir (diski monte edebilir veya açabilir).

> [!NOTE]
> Değer `maxShares` yalnızca disk tüm düğümlerden ayrıldığında ayarlanabilir veya düzenlenebilir.

Aşağıdaki tablo, disk boyutuna `maxShares` göre izin verilen maksimum değerleri gösterir:

|Disk boyutları  |maxShares sınırı  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Bir diskiçin IOPS ve bant genişliği sınırları `maxShares` değerden etkilenmez. Örneğin, bir P15 diskinin maksimum IOPS 1100 olup maxShares = 1 veya maxShares > 1 vardır.