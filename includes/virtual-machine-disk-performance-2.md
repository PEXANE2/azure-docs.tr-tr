---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 65f6c239f34775efff6a2ea2e399064a7702606a
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89664660"
---
![Dsv3 belgeleri](media/vm-disk-performance/dsv3-documentation.jpg)

**Önbelleğe** alınmamış maksimum disk aktarım hızı, sanal makinenin işleyebilmesi için varsayılan depolama üst sınırı sınırıdır. Ana bilgisayar önbelleğe almayı etkinleştirdiğinizde, en fazla **önbelleğe alınmış** depolama aktarım hızı sınırı ayrı bir sınır olur. Konak önbelleğe alma özelliğinin etkinleştirilmesi, sanal makineniz oluşturulurken ve diskler iliştirilirken yapılabilir. Ayrıca, mevcut bir VM 'de disklerinizi önbelleğe alma özelliğini açmak ve kapatmak için ayarlayabilirsiniz:

![Konak önbelleğe alma](media/vm-disk-performance/host-caching.jpg)

Konak önbelleğe alma, her bir disk için iş yükü gereksinimlerinize uyacak şekilde ayarlanabilir. Yalnızca okuma ve yazma işlemlerinin bir dengesini sağlayan iş yükleri için okuma/yazma işlemleri yapan iş yükleri için konak önbelleğe alma işlemini Salt okunabilir olacak şekilde ayarlayabilirsiniz. İş yükünüz Bu desenlerden birini izmezse, ana bilgisayar önbelleğe alma özelliğini kullanamazsınız. 

Standard_D8s_v3 sanal makinemizi bir örnekle devam edelim. Bu süre dışında, diskler üzerinde ana bilgisayar önbelleğe almayı etkinleştireceğiz ve artık VM 'nin ıOPS sınırının 16.000 ıOPS olması gerekir. VM 'ye bağlı üç temel P30 disk olan 5.000 ıOPS 'yi işleyebilir.

Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınan ıOPS: 16.000
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 işletim sistemi diski 
    - IOPS: 5.000
    - Konak önbelleğe alma etkin 
- 2 P30 veri diski
    - IOPS: 5.000
    - Konak önbelleğe alma etkin

![Konak önbelleğe alma örneği](media/vm-disk-performance/host-caching-example-without-remote.jpg)

Bu durumda, önbelleğe alma özelliği etkinken bu Standard_D8s_v3 sanal makineyi kullanan uygulama 15.000 ıOPS için bir istek yapar. Bu istekler, eklenen her bir diske 5.000 ıOPS olarak bölünür ve performans dönüşü gerçekleşmez.

## <a name="combined-uncached-and-cached-limits"></a>Önbelleğe alınmamış ve önbelleğe alınmış limitlerin birleştirilmesi

Bir sanal makinenin önbelleğe alma sınırları, önbelleğe alınmamış limitlerden ayrıdır. Diğer bir deyişle, sanal makinelerinizin önbelleğe alınmış sınırın toplam depolama ıO 'sunu ve önbelleğe alınmamış sınırı almasını sağlamak için, bir VM 'ye bağlı disklerde ana bilgisayar önbelleğe almayı etkinleştirebilmeniz gerekir. Bu limitlerin birlikte nasıl çalıştığını göz atalım ve Standard_D8s_v3 sanal makine ve Premium diskler ekli yapılandırmasına devam edeceğiz.

Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınan ıOPS: 16.000
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 işletim sistemi diski 
    - IOPS: 5.000
    - Konak önbelleğe alma etkin 
- 2 P30 veri diski X 2
    - IOPS: 5.000
    - Konak önbelleğe alma etkin
- 2 P30 veri diski X 2
    - IOPS: 5.000
    - Konak önbelleğe alma devre dışı

![Uzak depolama Ile konak önbelleğe alma örneği](media/vm-disk-performance/host-caching-example-with-remote.jpg)

Artık, Standard_D8s_v3 sanal makinede çalışan uygulama 25.000 ıOPS için bir istek yapar. Bu, bu disklerin 3 ' ün ana bilgisayar önbelleğe alma işlemini kullandığı ve disklerin 2 ' nin olmadığı, bağlı her diske 5.000 ıOPS olarak bölünür. Ana bilgisayar önbelleğe almayı kullanan 3, 16.000 ' in önbelleğe alınmış sınırları dahilinde olduğundan, bu istekler başarıyla tamamlanır ve depolama performansı dönüşü gerçekleşmez. Ayrıca, ana bilgisayar önbelleğe almayı kullanmayan 2 disk, 12.800 olan önbelleğe alınmamış limitlerin içinde olduğundan, bu istekler de başarılı bir şekilde tamamlanır ve hiçbir zaman gerçekleşmez.

## <a name="metrics-for-disk-performance"></a>Disk performansı ölçümleri
Azure 'da, sanal makinelerinizin ve disklerinizin nasıl çalıştığı hakkında öngörü sağlayan ölçümler sunuyoruz. Bu ölçümler Azure portal aracılığıyla görsel olarak görüntülenebilir veya bir API çağrısıyla alınabilir. Ölçümler, bir dakikalık aralıklar üzerinden hesaplanır. VM ve disk GÇ ve verimlilik performansı hakkında bilgi almak için aşağıdaki ölçümler mevcuttur:
- **Işletim sistemi diski sıra derinliği** : işletim sistemi diskine okunmayı bekleyen veya bu diske YAZıLAN geçerli GÇ isteklerinin sayısı.
- **Işletim sistemi diski okuma bayt/sn** – işletim sistemi diskinden saniye cinsinden okunan bayt sayısı.
- **Işletim sistemi diski okuma işlemi/sn** – işletim sistemi diskinden saniye cinsinden okunan giriş işlemlerinin sayısı.
- **Işletim sistemi diski yazma bayt/sn** – işletim sistemi diskinden saniye olarak yazılan bayt sayısı.
- **Işletim sistemi diski yazma işlemi/sn** – işletim sistemi diskinden saniye olarak yazılan çıkış işlemlerinin sayısı.
- **Veri diski sıra derinliği** : veri diskine okunmayı veya veri diskine yazılmasını bekleyen GEÇERLI bekleyen GÇ isteği sayısı.
- **Veri diski okuma bayt/sn** : saniye cinsinden veri disklerden okunan bayt sayısı.
- **Veri diski okuma işlemi/sn** : saniye içinde bir veri diskinden okunan giriş işlemlerinin sayısı.
- **Veri diski yazma bayt/sn** – veri disklerinin bir saniyede yazılan bayt sayısı.
- **Veri diski yazma işlemi/sn** : bir veri diskinden saniye içinde yazılan çıkış işlemlerinin sayısı.
- **Disk okuma bayt/sn** : bir VM 'ye bağlı tüm disklerden saniye cinsinden okunan toplam bayt sayısı.
- **Disk okuma işlemi/sn** : bir VM 'ye bağlı tüm disklerden saniye cinsinden okunan giriş işlemlerinin sayısı.
- **Disk yazma bayt/sn** : bir VM 'ye bağlı tüm disklerden saniye cinsinden yazılan bayt sayısı.
- **Disk yazma işlemi/sn** : bir VM 'ye bağlı tüm disklerden saniye cinsinden yazılan çıkış işlemlerinin sayısı.

## <a name="storage-io-utilization-metrics"></a>Depolama GÇ kullanım ölçümleri
Disk GÇ dönüşü tanılamaya yardımcı olan ölçümler:
- **Veri DISKI IOPS tüketilen yüzde** -veri diski IOPS tarafından hesaplanan yüzde, sağlanan veri diski IOPS üzerinden tamamlandı. Bu miktar %100 ise, çalışan uygulamanız veri diskinizin ıOPS sınırından itibaren GÇ üzerinden yapılır.
- **Veri diski kullanılan bant genişliği yüzdesi** -veri diski işleme tarafından hesaplanan yüzde, sağlanan veri diski aktarım hızı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız veri diskinizin bant genişliği sınırından itibaren GÇ üzerinden alınır.
- **Işletim sistemi DISK IOPS yüzdesi** -işletim sistemi diski IOPS tarafından hesaplanan yüzde, sağlanan işletim sistemi disk IOPS 'si üzerinden tamamlandı. Bu miktar %100 ise, uygulamanızın çalışır olması, işletim sistemi diskinizin ıOPS sınırından itibaren GÇ olacaktır.
- **Işletim sistemi disk bant genişliği yüzdesi** -işletim sistemi disk aktarım hızı tarafından hesaplanan yüzde, sağlanan işletim sistemi disk aktarım hızı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız işletim sistemi diskinizin bant genişliği sınırından itibaren GÇ üzerinden yapılır.