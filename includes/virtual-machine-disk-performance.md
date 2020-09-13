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
ms.openlocfilehash: 66380e0bab0a83d086ebebb5e595908cecd11643
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89664653"
---
Bu makale, Azure sanal makinelerini ve Azure disklerini birleştirdiğinizde disk performansını ve bunların nasıl çalıştığını açıklığa kavuşturmanıza yardımcı olur. Ayrıca, disk GÇ ve performans için iyileştirmek üzere yapabileceğiniz değişiklikler için performans sorunlarını nasıl tanılabileceğinizi açıklar.

## <a name="how-does-disk-performance-work"></a>Disk performansı nasıl çalışır?
Azure sanal makineleri, sanal makine türü ve boyutuna bağlı olarak ıOPS ve aktarım hızı performans sınırlarına sahiptir. Sanal makinelere eklenebilecek işletim sistemi diskleri ve veri diskleri, kendi IOPS ve aktarım hızı sınırlarına sahip olabilir. Sanal makinelerinizde çalışan uygulamanız, sanal makine veya eklenen diskler için ayrılan miktardan daha fazla ıOPS veya aktarım hızı istediğinde, uygulamanızın performansı alınır. Bu durumda, uygulama, en iyi performans performansına neden olur ve artan gecikme süresi gibi bazı olumsuz sonuçlara yol açabilir. Bunun için birkaç örnek üzerinden çalıştırılalım. Bu örneklerin izlenmesi kolay hale getirmek için yalnızca IOPS 'ye bakacağız ancak aynı mantığın de üretilen iş için de geçerli olduğu görülüyor.

## <a name="disk-io-capping"></a>Disk GÇ dönüşü
Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınmamış ıOPS: 12.800
- E30 işletim sistemi diski
    - IOPS: 500 
- 2 E30 veri diski
    - IOPS: 500

![Disk düzeyi dönüşü](media/vm-disk-performance/disk-level-throttling.jpg)

Sanal makinede çalışan uygulama, sanal makinede 10.000 IOPS gerektiren bir istek oluşturur. Standard_D8s_v3 sanal makine en fazla 12.800 IOPS yürütebildiğinden VM tarafından kullanılabilir. Bu 10.000 IOPS istekleri daha sonra farklı disklere üç farklı isteğe bölünür. 1.000 IOPS, işletim sistemi diskinde istenir ve her bir veri diski için 4.500 IOPS istenir. İliştirilmiş tüm diskler E30 diskler olduğundan ve yalnızca 500 IOPS 'yi işleyebildiği için, her biri 500 IOPS ile yeniden yanıt verir. Daha sonra uygulamanın performansı, eklenen diskler tarafından belirlenir ve yalnızca 1.500 IOPS 'yi işleyebilir. Premium SSD P30 diskler gibi daha iyi çalışan diskler kullanılmışsa 10.000 ıOPS 'de en yüksek performans üzerinde çalışıyor olabilir.

## <a name="virtual-machine-io-capping"></a>Sanal makine GÇ GÇ
Ayarla:
- Standard_D8s_v3 
    - Önbelleğe alınmamış ıOPS: 12.800
- P30 işletim sistemi diski
    - IOPS: 5.000 
- 2 P30 veri diski 
    - IOPS: 5.000

![Sanal makine düzeyi dönüşü](media/vm-disk-performance/vm-level-throttling.jpg)

Sanal makinede çalışan uygulama, 15.000 IOPS gerektiren bir istek yapar. Ne yazık ki Standard_D8s_v3 sanal makine yalnızca 12.800 IOPS 'yi işleyecek şekilde sağlanır. Bundan sonra uygulama, sanal makine sınırları tarafından belirlenir ve ardından ayrılan 12.800 IOPS 'yi ayırmalıdır. İstenen 12.800 IOPS, farklı disklere üç farklı isteğe bölünmüştür. 4.267 IOPS, işletim sistemi diskinde istenir ve her bir veri diski için 4.266 IOPS istenir. İliştirilmiş tüm diskler, 5.000 IOPS 'yi işleyebilen P30 diskleri olduğundan, istenen tutarlarla yeniden yanıt verir.