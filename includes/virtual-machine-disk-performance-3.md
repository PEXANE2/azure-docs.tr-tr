---
title: include dosyası
description: include dosyası
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 10/12/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 14e74bfbcd087ccc1d8c5f2f10a8e44ed37cce84
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016595"
---
![Ölçümler menüsü](media/vm-disk-performance/utilization-metrics-example/fio-output.jpg)

Ancak, Standard_D8s_v3 toplam 28.600 IOPS elde edebilir. ölçümler kullanılarak, neler olduğunu araştırıp depolama GÇ darboğazımızın belirlenmesi sağlanır. İlk olarak, ölçüm düğmesine sol taraftaki menüyü bulun ve seçin:

![Ölçümler menüsü](media/vm-disk-performance/utilization-metrics-example/metrics-menu.jpg)

İlk olarak, **VM 'Nin önbelleğe alınmış IOPS yüzde** ölçüsünü göz atalım:

![VM önbelleğe alınan ıOPS yüzdesi](media/vm-disk-performance/utilization-metrics-example/vm-cached.jpg)

Bu ölçüm, VM 'deki önbelleğe alınmış IOPS 'ye ayrılan 16.000 IOPS 'yi söylemekte, %61 kullanılıyor. Bu, depolama GÇ darboğazın %100 ' de olmadığı için önbelleğe alınmış disklere sahip olmadığı anlamına gelir. Şimdi, **VM 'Nin önbelleğe ALıNMAMıŞ IOPS yüzde** ölçüsünü göz atalım:

![VM önbelleğe alınmamış ıOPS yüzde yüzdesi](media/vm-disk-performance/utilization-metrics-example/vm-uncached.jpg)

Bu ölçüm %100 ' de olduğundan, VM 'deki önbelleğe alınmamış IOPS 'ye ayrılan tüm 12.800 IOPS 'nin kullanıldığını bize söyleyerek. Bunu düzeltebilmemiz için, VM 'imizin boyutunu ek GÇ 'yi işleyebilen daha büyük bir boyutla değiştirerek yapabilirsiniz. Ancak bunu yapmadan önce, kaç IOPS gördüğünü görmek için eklenen diske göz atalım. İlk olarak işletim **sistemi DISK IOPS tüketilen yüzdeye**bakarak Işletim sistemi diskine göz atalım:

![İşletim sistemi diski ıOPS tüketilen yüzde](media/vm-disk-performance/utilization-metrics-example/os-disk.jpg)

Bu ölçüm, bu P30 işletim sistemi diski için sağlanan 5.000 IOPS 'nin %90 ' nin üzerinde olduğunu bize bildiriyor. Bu, burada işletim sistemi diskinde bir performans sorunu olmadığı anlamına gelir. Şimdi, **veri DISKI IOPS tüketilen yüzdeye**bakarak VM 'ye bağlı veri disklerine göz atalım:

![Veri diski ıOPS tüketilen yüzde](media/vm-disk-performance/utilization-metrics-example/data-disks-no-splitting.jpg)

Bu ölçüm, bağlı olan tüm diskler genelinde yüzde 42 ' ün altında bulunan ortalama IOPS 'nin olduğunu bize bildiriyor. Bu yüzde, diskler tarafından kullanılan ve ana bilgisayar önbelleğinden sunulmayan IOPS temel alınarak hesaplanır. Bu ölçüm hakkında daha ayrıntılı bilgi vererek bu ölçümleri **bölmek** ve LUN değerine göre bölmek için:

![Veri diski ıOPS değeri bölme Ile tüketildi](media/vm-disk-performance/utilization-metrics-example/data-disks-splitting.jpg)

Bu ölçüm, LUN 3 ' te takılı olan veri disklerinin ve 2 ' nin sağlanan IOPS 'nin %85 ' de kullanıldığı konusunda bize söylemekte. Bu, GÇ 'nin VM ve disk mimarisine nasıl benzediklerinin bir diyagramı aşağıda verilmiştir:

![Storage ıO ölçümleri örnek diyagramı](media/vm-disk-performance/utilization-metrics-example/metrics-diagram.jpg)
