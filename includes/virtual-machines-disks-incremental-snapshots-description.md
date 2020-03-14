---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 1f094cd78e6708fec4bda1e5510379b11df14dcd
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79299218"
---
Artımlı anlık görüntüler, alındığı zaman yönetilen diskler için zaman içinde son anlık görüntüden bu yana yapılan tüm değişikliklerden oluşur. Artımlı bir anlık görüntüyü indirmeye veya başka bir şekilde kullanmaya çalıştığınızda, tam VHD kullanılır. Yönetilen disk anlık görüntülerinin bu yeni özelliği, siz tercih etmediğiniz müddetçe, her bir anlık görüntü ile tüm diski depolamanız gerekmediği için büyük olasılıkla daha fazla maliyetli olabilir. Düzenli anlık görüntüler gibi, artımlı anlık görüntüler tam yönetilen disk oluşturmak veya düzenli bir anlık görüntü oluşturmak için kullanılabilir.

Artımlı anlık görüntü ve normal anlık görüntü arasında birkaç fark vardır. Artımlı anlık görüntüler, diskin depolama türünden bağımsız olarak her zaman standart HDD depolamayı kullanır, ancak normal anlık görüntüler Premium SSD 'Ler kullanabilir. VM dağıtımlarını ölçeklendirmek için Premium depolamada düzenli anlık görüntüler kullanıyorsanız, [paylaşılan görüntü galerisinde](../articles/virtual-machines/linux/shared-image-galleries.md)standart depolamada özel görüntüler kullanmanızı öneririz. Daha düşük maliyetli daha büyük bir ölçek elde etmenize yardımcı olur. Ayrıca, artımlı anlık görüntüler, bölgesel olarak [yedekli depolama](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) ile daha iyi güvenilirlik sunar. ZRS seçili bölgede kullanılabiliyorsa, artımlı bir anlık görüntü, ZRS 'yi otomatik olarak kullanır. ZRS bölgede yoksa, anlık görüntü varsayılan [olarak yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) (LRS) olarak değişir. Bu davranışı geçersiz kılabilir ve bir el ile seçebilirsiniz ancak bunun için önerilmez.

Artımlı anlık görüntüler, yalnızca yönetilen diskler için kullanılabilen bir fark özelliği de sunar. Bunlar, blok düzeyine kadar, aynı yönetilen disklerin iki artımlı anlık görüntüsü arasında değişiklik almanızı sağlar. Bu özelliği, anlık görüntüleri bölgeler arasında kopyalarken veri parmak izini azaltmak için kullanabilirsiniz.
