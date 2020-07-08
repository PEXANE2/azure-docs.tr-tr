---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/06/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0b185d545e129c941d5df2e8ce86ee684174b666
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81008376"
---
Şimdilik yalnızca Ultra diskler ve Premium SSD 'Ler Paylaşılan diskleri etkinleştirebilir. Farklı disk boyutları farklı bir sınıra sahip olabilir `maxShares` ve bu değer ayarlanırken aşamazsınız `maxShares` . Premium SSD 'Ler için, disklerinin paylaşımını destekleyen disk boyutları P15 ve daha büyüktür.

Her disk için, `maxShares` diski eşzamanlı olarak paylaşabilen en fazla düğüm sayısını temsil eden bir değer belirleyebilirsiniz. Örneğin, 2 düğümlü bir yük devretme kümesi ayarlamayı planlıyorsanız, ayarlanır `maxShares=2` . En büyük değer, üst sınırdır. Düğüm sayısı belirtilen değerden düşük olduğu sürece, düğümler kümeye katılabilir veya kümeyi bırakabilir (diski bağlama veya çıkarma) `maxShares` .

> [!NOTE]
> `maxShares`Değer yalnızca disk tüm düğümlerden ayrıldığında ayarlanabilir veya düzenlenebilir.

### <a name="premium-ssd-ranges"></a>Premium SSD aralıkları

Aşağıdaki tabloda Premium disk boyutları için izin verilen en büyük değerler gösterilmektedir `maxShares` :

|Disk boyutları  |maxShares sınırı  |
|---------|---------|
|P15, P20     |2         |
|P30, P40, P50     |5         |
|P60, P70, P80     |10         |

Bir diskin ıOPS ve bant genişliği sınırları bu `maxShares` değerden etkilenmez. Örneğin, P15 diskinin maksimum ıOPS değeri, maxShares = 1 veya maxShares > 1 ' in 1100 ' dir.

### <a name="ultra-disk-ranges"></a>Ultra disk aralıkları

En küçük `maxShares` değer 1 ' dir, ancak en büyük `maxShares` değer 5 ' tir. Ultra disklerde herhangi bir boyut kısıtlaması yoktur, tüm boyuttaki Ultra Disk `maxShares` , en fazla değeri de dahil olmak üzere herhangi bir değeri kullanabilir.