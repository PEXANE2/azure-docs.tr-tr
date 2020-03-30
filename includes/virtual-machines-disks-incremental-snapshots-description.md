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
ms.openlocfilehash: 4d2c0a02a48c0e04b501f136f66c28b3f532e2a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79485993"
---
Artımlı anlık görüntüler, alındığında yalnızca son anlık görüntüden bu yana yapılan tüm değişikliklerden oluşan yönetilen diskler için zaman yedeklemeleri noktasındadır. Artımlı anlık görüntü indirmeye veya başka bir şekilde kullanmaya çalıştığınızda, tam VHD kullanılır. Yönetilen disk anlık görüntüleri için bu yeni özellik, istediğiniz sürece tüm diski her bir anlık görüntüyle depolamanız gerekmediğinden, bunların daha uygun maliyetli olmasını sağlar. Normal anlık görüntüler gibi, artımlı anlık görüntüler de tam yönetilen bir disk oluşturmak veya normal anlık görüntü oluşturmak için kullanılabilir.

Artımlı anlık görüntü ile normal anlık görüntü arasında birkaç fark vardır. Artımlı anlık görüntüler, diskin depolama türüne bakılmaksızın her zaman standart HDD depolama kullanırken, normal anlık görüntüler premium SSD'leri kullanabilir. VM dağıtımlarını büyütmek için Premium Depolama'da düzenli anlık görüntüler kullanıyorsanız, Paylaşılan [Resim Galerisi'ndeki](../articles/virtual-machines/linux/shared-image-galleries.md)standart depolama alanında özel görüntüler kullanmanızı öneririz. Daha düşük maliyetle daha büyük bir ölçek elde yardımcı olacaktır. Ayrıca, artımlı anlık [görüntüler, bölge yedekli depolama](../articles/storage/common/storage-redundancy-zrs.md) (ZRS) ile daha iyi güvenilirlik sağlar. Seçilen bölgede ZRS varsa, artımlı anlık görüntü ZRS'yi otomatik olarak kullanır. Bölgede ZRS kullanılamıyorsa, anlık görüntü varsayılan [olarak yerel olarak yedekli depolama](../articles/storage/common/storage-redundancy-lrs.md) (LRS) olarak tanımlanır. Bu davranışı geçersiz kılabilir ve el ile birini seçebilirsiniz, ancak bunu önermiyoruz.

Artımlı anlık görüntüler, yalnızca yönetilen diskler için kullanılabilen bir diferansiyel yeteneği de sunar. Bunlar, aynı yönetilen disklerin iki artımlı anlık görüntüsü arasındaki değişiklikleri blok düzeyine kadar almanızı sağlar. Bölgeler arasında anlık görüntüleri kopyalarken veri ayak izinizi azaltmak için bu özelliği kullanabilirsiniz.  Örneğin, ilk artımlı anlık görüntünün başka bir bölgedeki taban blob'u olarak indirebilirsiniz. Sonraki artımlı anlık görüntüler için, yalnızca temel blob için son anlık görüntü beri değişiklikleri kopyalayabilirsiniz. Değişiklikleri kopyaladıktan sonra, başka bir bölgedeki diskin zaman yedeklemesi noktanızı temsil eden temel blob'da anlık görüntü alabilirsiniz. Diskinizi temel blob'dan veya başka bir bölgedeki temel blob'daki anlık görüntüden geri yükleyebilirsiniz.

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="Bölgeler arasında kopyalanan artımlı anlık görüntüleri gösteren diyagram. Anlık görüntüler, her anlık görüntü için sayfa lekeleri oluşturana kadar çeşitli API çağrıları yapar.":::