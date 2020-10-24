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
ms.openlocfilehash: 8eff9da82fdfa5749fd1c2bc04652d5c8ce8dfd2
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92518090"
---
![R = 22.8 k vurgulanmasını gösteren f ı o çıkışının ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/fio-output.jpg)

Standard_D8s_v3 toplam 28.600 ıOPS elde edebilir. Ölçümleri kullanarak, neler olduğunu araştıralım ve depolama GÇ darboğazımuzu nasıl tanımlayacağım. Sol bölmede **ölçümler**' i seçin:

![Sol bölmede vurgulanan ölçümleri gösteren ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/metrics-menu.jpg)

İlk olarak, **VM 'Nin önbelleğe alınmış IOPS yüzde** ölçüsünü göz atalım:

![V M önbelleğe alınan g/ç 'yi gösteren ekran görüntüsü yüzdesi.](media/vm-disk-performance/utilization-metrics-example/vm-cached.jpg)

Bu ölçüm, sanal makine üzerindeki önbelleğe alınmış ıOPS 'ye ayrılan 16.000 ıOPS 'nin %61 ' u olduğunu söyler. Bu yüzde, depolama GÇ sorununa %100 ' de olmadığı için önbelleğe alınan diskler olmadığı anlamına gelir. Şimdi **VM 'Nin önbelleğe ALıNMAMıŞ IOPS yüzdesi** ölçümünü inceleyelim:

![V M önbelleğe alınmamış g/ç 'Leri tüketilen ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/vm-uncached.jpg)

Bu ölçüm %100 ' dir. Bu, VM 'deki önbelleğe alınmamış ıOPS 'ye ayrılan tüm 12.800 ıOPS 'nin kullanıldığını söyler. Bu sorunu düzelttiğimiz yollardan biri, VM 'imizin boyutunu ek GÇ 'yi işleyebileceğiniz daha büyük bir boyuta değiştirmek olabilir. Ancak bunu yapmadan önce, kaç ıOPS gördüğünü öğrenmek için eklenen diske göz atalım. İşletim **sistemi DISK IOPS tüketilen yüzdeyi**inceleyerek Işletim sistemi diskini kontrol edin:

![O 'un tükettiği disk g/ç yüzdesini gösteren ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/os-disk.jpg)

Bu ölçüm, bu P30 işletim sistemi diski için sağlanan 5.000 ıOPS 'nin yaklaşık %90 ' üne sahip olduğunu bize söyler. Bu yüzde, işletim sistemi diskinde performans sorunu olmadığı anlamına gelir. Şimdi, VM 'ye bağlı veri disklerini, **tüketilen veri DISKI IOPS yüzdesine**göz atalım.

![Veri diski g/ç yüzdesini gösteren ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/data-disks-no-splitting.jpg)

Bu ölçüm, eklenen tüm diskler genelinde yüzde 42 ' ün altında olan ortalama ıOPS 'nin yüzdesini söyler. Bu yüzde, diskler tarafından kullanılan ve konak önbelleğinden sunulmayan ıOPS temel alınarak hesaplanır. Bu ölçülere *bölme* ve LUN değerine göre bölme uygulayarak bu ölçüme daha ayrıntılı bir bakalım:

![Veri diski g/ç 'Leri bölme ile tüketilen ekran görüntüsü.](media/vm-disk-performance/utilization-metrics-example/data-disks-splitting.jpg)

Bu ölçüm, 1. LUN 'a eklenen veri disklerini ve 2 ' nin sağlanan ıOPS 'nin %85 ' de kullandığını söyler. Bu, GÇ 'nin VM ve diskler mimarisinden nasıl görüneceğine ilişkin bir diyagram aşağıda verilmiştir:

![Depolama g/ç ölçümleri örneği diyagramı.](media/vm-disk-performance/utilization-metrics-example/metrics-diagram.jpg)
