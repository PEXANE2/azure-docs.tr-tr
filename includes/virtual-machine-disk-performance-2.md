---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 09/25/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: e5a6dae98e786bf55dc17d8fabe42f84e9927442
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91605898"
---
![Dsv3 belgeleri](media/vm-disk-performance/dsv3-documentation.jpg)

**Önbelleğe** alınmamış maksimum disk aktarım hızı, sanal makinenin işleyebilmesi için varsayılan depolama üst sınırı sınırıdır. Ana bilgisayar önbelleğe almayı etkinleştirdiğinizde, en fazla **önbelleğe alınmış** depolama aktarım hızı sınırı ayrı bir sınır olur. Ana bilgisayar önbelleğe alma, yazılabilir veya hızlı bir şekilde okunabilen sanal makineye daha yakın bir depolama alanı ile çalışmaya devam edebilir. Konak önbelleğe alma için VM tarafından kullanılabilen depolama miktarı belgelerde bulunur. Örneğin, Standard_D8s_v3, önbellek depolamanın 200 GiB ile geldiğini görebilirsiniz.

Konak önbelleğe alma özelliğinin etkinleştirilmesi, sanal makineniz oluşturulurken ve diskler iliştirilirken yapılabilir. Ayrıca, mevcut bir VM 'de disklerinizi önbelleğe alma özelliğini açmak ve kapatmak için ayarlayabilirsiniz.

![Konak önbelleğe alma](media/vm-disk-performance/host-caching.jpg)

Konak önbelleğe alma, her bir disk için iş yükü gereksinimlerinize uyacak şekilde ayarlanabilir. Yalnızca okuma ve yazma işlemlerinin bir dengesini sağlayan iş yükleri için okuma/yazma işlemleri yapan iş yükleri için konak önbelleğe alma işlemini Salt okunabilir olacak şekilde ayarlayabilirsiniz. İş yükünüz Bu desenlerden birini izmezse, ana bilgisayar önbelleğe alma kullanılması önerilmez. 

Farklı ana bilgisayar önbelleği ayarlarına birkaç örnek ile çalıştıralım ve veri akışını ve performansını nasıl etkiler, görelim. Bu ilk örnekte, ana bilgisayar önbelleğe alma ayarı **salt okunurdur**olarak ayarlandığında GÇ istekleriyle ne olduğunu göz atacağız.

Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınan ıOPS: 16.000
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 veri diski 
    - IOPS: 5.000
    - **Konak önbelleğe alma: salt okunurdur** 

Okuma işlemi gerçekleştirildiğinde ve istenen veriler önbellekte kullanılabilir olduğunda, önbellek istenen verileri döndürür ve diskten okunmalıdır. Bu okuma işlemi, sanal makinenin önbelleğe alınmış sınırlarına doğru sayılır.

![Ana bilgisayar önbelleğe alma okuma Isabeti](media/vm-disk-performance/host-caching-read-hit.jpg)

Bir okuma işlemi gerçekleştirildiğinde ve istenen veriler **önbellekte yoksa,** okuma isteği daha sonra hem önbellekte hem de VM 'de yüzey oluşturan diske gönderilir. Bu okuma işlemi, sanal makinenin önbelleğe alınmamış sınırına ve VM 'nin önbelleğe alınmış sınırına doğru sayılır.

![Ana bilgisayar önbelleğe alma okuma isabetsizliği](media/vm-disk-performance/host-caching-read-miss.jpg)

Bir yazma işlemi gerçekleştirildiğinde, yazma işlemi tamamlanmadan önce önbelleğin ve diskin üzerine yazılması gerekir. Bu yazma işlemi, sanal makinenin önbelleğe alınmamış sınırına ve VM 'nin önbelleğe alınmış sınırına doğru sayılır.

![Konak önbelleği yazma Işlemini oku](media/vm-disk-performance/host-caching-write.jpg)

Bu sonraki örnekte, ana bilgisayar önbelleği ayarı **okuma/yazma**olarak ayarlandığında GÇ istekleriyle ne olduğunu göz atalım.

Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınan ıOPS: 16.000
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 veri diski 
    - IOPS: 5.000
    - **Konak önbelleğe alma: okuma/yazma** 

Okumalar salt okunur şekilde tamamen işlenir, yazma/yazma önbelleklemesi ile farklı olan tek şeydir. Ana bilgisayar önbelleğe alma özelliği okuma/yazma olarak ayarlandığında, yazma işleminin yalnızca, tamamlanmış olarak kabul edilmesi için ana bilgisayar önbelleğine yazılması gerekir. Yazma daha sonra geç arka plan işlemi olarak diske yazılır. Bu, yazma işlemlerinin önbelleğe yazıldığı sırada önbelleğe alınmış GÇ 'ye ve geç ne zaman önbelleğe alınmamışsa, önbelleğe alınmamış GÇ 'ye doğru sayılacağını gösterir.

![Okuma/yazma ana bilgisayar önbelleği yazma](media/vm-disk-performance/host-caching-read-write.jpg)

Standard_D8s_v3 sanal makinemizi bir örnekle devam edelim. Bu süre dışında, diskler üzerinde ana bilgisayar önbelleğe almayı etkinleştireceğiz ve artık VM 'nin ıOPS sınırının 16.000 ıOPS olması gerekir. VM 'ye bağlı üç temel P30 disk olan 5.000 ıOPS 'yi işleyebilir.

Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınan ıOPS: 16.000
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 işletim sistemi diski 
    - IOPS: 5.000
    - Konak önbelleğe alma: okuma/yazma 
- 2 P30 veri diski
    - IOPS: 5.000
    - Konak önbelleğe alma: okuma/yazma

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
    - Konak önbelleğe alma: okuma/yazma
- 2 P30 veri diski X 2
    - IOPS: 5.000
    - Konak önbelleğe alma: okuma/yazma
- 2 P30 veri diski X 2
    - IOPS: 5.000
    - Konak önbelleğe alma: devre dışı

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
- **Veri DISKI IOPS tüketilen yüzde** -VERI diski IOPS tarafından hesaplanan yüzde, sağlanan VERI diski IOPS üzerinden tamamlandı. Bu miktar %100 ise, çalışan uygulamanız veri diskinizin ıOPS sınırından itibaren GÇ üzerinden yapılır.
- **Veri diski kullanılan bant genişliği yüzdesi** -veri diski işleme tarafından hesaplanan yüzde, sağlanan veri diski aktarım hızı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız veri diskinizin bant genişliği sınırından itibaren GÇ üzerinden alınır.
- **Işletim sistemi DISK IOPS yüzdesi** -işletim sistemi diski IOPS tarafından hesaplanan yüzde, sağlanan işletim SISTEMI disk IOPS 'si üzerinden tamamlandı. Bu miktar %100 ise, uygulamanızın çalışır olması, işletim sistemi diskinizin ıOPS sınırından itibaren GÇ olacaktır.
- **Işletim sistemi disk bant genişliği yüzdesi** -işletim sistemi disk aktarım hızı tarafından hesaplanan yüzde, sağlanan işletim sistemi disk aktarım hızı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız işletim sistemi diskinizin bant genişliği sınırından itibaren GÇ üzerinden yapılır.

VM GÇ dönüşü tanılamaya yardımcı olan ölçümler:
- **VM önbelleğe ALıNAN IOPS yüzdesi** -toplam IOPS tarafından hesaplanan yüzde, önbelleğe alınmış maksimum sanal makine IOPS sınırı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız VM 'nin önbelleğe alınmış ıOPS sınırından itibaren GÇ üzerinden yapılır.
- **VM önbelleğe alınan bant genişliği yüzdesi** -toplam disk işleme tarafından hesaplanan yüzde, önbelleğe alınan en fazla sanal makine performansı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız VM 'nin önbelleğe alınmış bant genişliği sınırından itibaren GÇ üzerinden yapılır.
- **VM önbelleğe ALıNMAMıŞ IOPS yüzdesi** -bir sanal MAKINEDEKI toplam IOPS tarafından hesaplanan yüzde, önbelleğe alınmamış maksimum sanal makine IOPS sınırı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız VM 'nin önbelleğe alınmamış ıOPS sınırından itibaren GÇ üzerinden yapılır.
- **VM önbelleğe alınmamış bant genişliği yüzdesi** -sanal makinedeki toplam disk aktarım hızı tarafından hesaplanan yüzde, sağlanan en yüksek sanal makine aktarım hızı üzerinden tamamlanır. Bu miktar %100 ise, çalışan uygulamanız, sanal makinenin önbelleğe alınmamış bant genişliği sınırından itibaren GÇ üzerinden yapılır.

