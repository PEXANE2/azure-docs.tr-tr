---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/29/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 871a3edf70690a09d3747703e8bc999dfcce967c
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385189"
---
Disk patlaması premium SSD'ler için desteklenir. Bursting herhangi bir prim SSD disk boyutları <= 512 GiB (P20 veya altında) desteklenir. Bu disk boyutları en iyi çaba temelinde patlama desteği ve patlama yönetmek için bir kredi sistemi kullanmak. Disk trafiği, disk boyutları için sağlanan performans hedefinin altında olduğunda, krediler bir patlama kovasında birikir ve trafik hedefin ötesinde patladığında kredi tüketir. Disk trafiği, sağlanan hedefte hem IOPS hem de bant genişliğine göre izlenir. Disk patlaması, IOPS veya iş çıkışındaki sanal makine (VM) boyut sınırlamalarını atlamaz.

Disk patlaması varsayılan olarak onu destekleyen disk boyutlarının yeni dağıtımlarında etkinleştirilir. Varolan disk boyutları, disk patlamasını destekliyorsa, aşağıdaki yöntemlerden herhangi biri aracılığıyla patlamayı etkinleştirebilir:

- Diski ayırın ve yeniden takın.
- Dur ve VM'yi başlat.

## <a name="burst-states"></a>Patlama durumları

Tüm patlama uygulanabilir disk boyutları, disk Sanal Makine'ye bağlandığında tam bir seri çekim kredi kovasıyla başlar. Patlamanın maksimum süresi, patlama kredi kovasının boyutuna göre belirlenir. Yalnızca kredi kovası boyutuna kadar kullanılmayan kredileri biriktirebilirsiniz. Herhangi bir zamanda, disk patlaması kredi kovanız aşağıdaki üç durumdan birinde olabilir: 

- Disk trafiği, sağlanan performans hedefinden daha az ını kullanıyorsa tahakkuk etme. Disk trafiği IOPS veya bant genişliği hedefleri veya her ikisinin ötesindeyse kredi biriktirebilirsiniz. Tam disk bant genişliği tüketirken hala IO kredisi biriktirebilirsiniz, tam tersi.  

- Disk trafiği, sağlanan performans hedefinden daha fazlasını kullanıyorsa azalan. Patlama trafiği, IOPS veya bant genişliğinden bağımsız olarak kredi tüketir. 

- Disk trafiği tam olarak sağlanan performans hedefinde olduğunda sabit kalır. 

Patlama belirtimleriyle birlikte patlama desteği sağlayan disk boyutları aşağıdaki tabloda özetlenmiştir.

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Disk patlaması, Genel Bulut'taki tüm bölgelerde kullanılabilir.

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

## <a name="example-scenarios"></a>Örnek senaryolar

Bunun nasıl çalıştığı hakkında size daha iyi bir fikir vermek için birkaç örnek senaryo aşağıda verilmiştir:

- Disk patlamasından yararlanabilecek yaygın bir senaryo, işletim sistemi disklerinde daha hızlı VM önyükleme ve uygulama başlatmadır. Örnek olarak 8 GiB işletim sistemi görüntüsüne sahip bir Linux VM alın. İşletim sistemi diski olarak P2 disk kullanırsak, hedeflenen hedef 120 IOPS ve 25 MBps'dir. VM başladığında, önyükleme dosyalarını yükleyen işletim sistemi diskinde bir okuma sıçraması olacaktır. Patlamanın piyasaya sürülmesiyle, maksimum 3500 IOPS ve 170 MBps patlama hızında okuyarak yükleme süresini en az 6 kat hızlandırabilirsiniz. VM önyüklemeden sonra, uygulama tarafından gerçekleştirilen çoğu veri işlemi ekli veri disklerine karşı olacağından, işletim sistemi diskindeki trafik düzeyi genellikle düşüktür. Trafik sağlanan hedefin altındaysa, kredi biriktirirsiniz.

- Uzaktan Sanal Masaüstü ortamıbarındırıyorsanız, etkin bir kullanıcı AutoCAD gibi bir uygulama başlattığında, işletim sistemi diskine olan okuma trafiği önemli ölçüde artar. Bu durumda, patlama trafiği birikmiş kredileri tüketir, böylece sağlanan hedefin ötesine geçmenizi sağlar ve uygulamayı çok daha hızlı başlatabilirsiniz.

- Bir P1 diskinin 120 IOPS ve 25 MBps'lik bir hedefi vardır. Diskteki gerçek trafik son 1 saniye aralığında 100 IOPS ve 20 MBps ise, kullanılmayan 20 IOs ve 5 MB diskin seri kovasına yatırılır. Patlama kovasındaki krediler daha sonra trafik hedeflenen hedefi aştığında, maksimum patlama sınırına kadar kullanılabilir. Maksimum seri patlama sınırı, tüketecek patlama kredilerine sahip olsanız bile disk trafiğinin tavanını tanımlar. Bu durumda, kredi kovasında 10.000 IO olsa bile, bir P1 diski saniyede 3.500 IO'luk maksimum patlamadan fazlasını veremez.  
