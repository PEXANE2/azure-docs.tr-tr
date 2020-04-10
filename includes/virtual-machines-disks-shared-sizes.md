---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/06/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0b185d545e129c941d5df2e8ce86ee684174b666
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008376"
---
Şimdilik, yalnızca ultra diskler ve premium SSD'ler paylaşılan diskleri etkinleştirebilir. Farklı disk boyutları, değeri `maxShares` ayarlarken aşamadığınız farklı `maxShares` bir sınıra sahip olabilir. Birinci sınıf SSD'ler için, disklerini paylaşmayı destekleyen disk boyutları P15 ve daha büyüktür.

Her disk için, diski aynı anda paylaşabilecek en fazla düğüm sayısını temsil eden bir `maxShares` değer tanımlayabilirsiniz. Örneğin, 2 düğümlü bir başarısız küme ayarlamayı planlıyorsanız, `maxShares=2`. Maksimum değer bir üst sınırdır. Düğüm sayısı belirtilen `maxShares` değerden daha düşük olduğu sürece düğümler kümeye katılabilir veya kümeden çıkabilir (diski monte edebilir veya açabilir).

> [!NOTE]
> Değer `maxShares` yalnızca disk tüm düğümlerden ayrıldığında ayarlanabilir veya düzenlenebilir.

### <a name="premium-ssd-ranges"></a>Premium SSD aralıkları

Aşağıdaki tablo, premium disk boyutlarına `maxShares` göre izin verilen maksimum değerleri gösterir:

|Disk boyutları  |maxShares sınırı  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Bir diskiçin IOPS ve bant genişliği sınırları `maxShares` değerden etkilenmez. Örneğin, bir P15 diskinin maksimum IOPS 1100 olup maxShares = 1 veya maxShares > 1.

### <a name="ultra-disk-ranges"></a>Ultra disk aralıkları

En `maxShares` küçük değer 1 iken, en büyük `maxShares` değer 5'tir. Ultra disklerde boyut kısıtlaması yoktur, herhangi bir boyut `maxShares`ultra disk için herhangi bir değer kullanabilirsiniz , kadar ve maksimum değer de dahil olmak üzere.