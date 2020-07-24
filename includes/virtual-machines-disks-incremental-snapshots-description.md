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
ms.openlocfilehash: 4f40a821a0a639fa117dc0844146f28d887166e1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103068"
---
Artımlı anlık görüntüler, alındığı zaman yönetilen diskler için zaman içinde son anlık görüntüden bu yana yapılan değişikliklerden oluşur. Bir diski artımlı bir anlık görüntüden geri yüklediğinizde, sistem Artımlı anlık görüntü çekilirken diskin zaman içindeki noktasını temsil eden tam diski yeniden yükler. Yönetilen disk anlık görüntülerinin bu yeni özelliği, siz tercih etmediğiniz müddetçe, her bir anlık görüntü ile tüm diski depolamanız gerekmediği için büyük olasılıkla daha fazla maliyetli olabilir. Tam anlık görüntülerle olduğu gibi, artımlı anlık görüntüler tam yönetilen disk veya tam anlık görüntü oluşturmak için kullanılabilir.

Artımlı bir anlık görüntü ve tam anlık görüntü arasında birkaç fark vardır. Artımlı anlık görüntüler, diskin depolama türünden bağımsız olarak her zaman standart HDD depolamayı kullanır, ancak tam anlık görüntüler Premium SSD 'Ler kullanabilir. VM dağıtımlarını ölçeklendirmek için Premium depolamada tam anlık görüntü kullanıyorsanız, [paylaşılan görüntü galerisinde](../articles/virtual-machines/linux/shared-image-galleries.md)standart depolamada özel görüntüler kullanmanızı öneririz. Daha düşük maliyetli daha büyük bir ölçek elde etmenize yardımcı olur. Ayrıca, artımlı anlık görüntüler, bölgesel olarak [yedekli depolama](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) ile daha iyi güvenilirlik sunar. ZRS seçili bölgede kullanılabiliyorsa, artımlı bir anlık görüntü, ZRS 'yi otomatik olarak kullanır. ZRS bölgede yoksa, anlık görüntü varsayılan [olarak yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) (LRS) olarak değişir. Bu davranışı geçersiz kılabilir ve bir el ile seçebilirsiniz ancak bunun için önerilmez.

Artımlı anlık görüntüler, yalnızca yönetilen diskler için kullanılabilen bir fark özelliği de sunar. Bunlar, blok düzeyine kadar, aynı yönetilen disklerin iki artımlı anlık görüntüsü arasında değişiklik almanızı sağlar. Bu özelliği, anlık görüntüleri bölgeler arasında kopyalarken veri parmak izini azaltmak için kullanabilirsiniz.  Örneğin, ilk artımlı anlık görüntüyü başka bir bölgeye temel blob olarak indirebilirsiniz. Sonraki artımlı anlık görüntüler için, yalnızca son anlık görüntüden bu yana yapılan değişiklikleri temel bloba kopyalayabilirsiniz. Değişiklikleri kopyaladıktan sonra, temel Blobun üzerinde, diskin zaman içindeki yedeğini başka bir bölgede temsil eden anlık görüntüler alabilirsiniz. Disketinizi temel Blobun ya da başka bir bölgedeki temel Blobun anlık görüntüsünden geri yükleyebilirsiniz.

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="Bölgeler arasında kopyalanmış Artımlı anlık görüntüleri gösteren diyagram. Anlık görüntüler, her anlık görüntü için sayfa Blobları oluşana kadar çeşitli API çağrıları yapar.":::

Artımlı anlık görüntüler yalnızca kullanılan boyut için faturalandırılır. [Azure kullanım raporuna](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)bakarak, anlık görüntülerinizin kullanılan boyutunu bulabilirsiniz. Örneğin, bir anlık görüntünün kullanılan veri boyutu 10 GiB ise **günlük** kullanım raporu, tüketilen miktar olarak 10 GiB/(31 gün) = 0,3226 gösterir.
