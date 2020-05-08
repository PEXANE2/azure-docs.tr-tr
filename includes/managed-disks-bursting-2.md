---
title: include dosyası
description: include dosyası
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 04/27/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 850ace7af15ab37ab9a4a124d20ed4588771f4d4
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594543"
---
## <a name="common-scenarios"></a>Yaygın senaryolar
Aşağıdaki senaryolar büyük ölçüde patlanabilir:
- **Önyükleme zamanlarını iyileştirme** : tekrarlaması sayesinde, örneğiniz önemli ölçüde daha hızlı bir hızda önyüklenecektir. Örneğin, Premium etkin VM 'Ler için varsayılan işletim sistemi diski, 120 ıOPS ve 25 MB/sn için sağlanan bir performans olan P4 disktir. Patlama ile P4, bir önyükleme süresinin 6X ile hızlanmasına izin veren 3500 ıOPS ve 170 MB/sn 'ye kadar sürebilir.
- **Toplu Işleri işleme** – bazı uygulamaların iş yükleri doğası açısından döngüsel olur ve çoğu zaman için bir temel performans gerektirir ve kısa bir süre için daha yüksek performans gerektirir. Bu işlemin bir örneği, az miktarda disk trafiği gerektiren işlemleri günlük olarak işleyen bir muhasebe programıdır. Daha sonra ayın sonunda, çok daha yüksek miktarda disk trafiğine ihtiyacı olan raporların uzlaştırılıyor.
- **Trafik ani artışları Için hazırlık** : Web sunucuları ve uygulamaları, her zaman trafik dalgalanmalarına karşılaşabilir. Web sunucunuz, patlama kullanılarak VM 'Ler veya diskler tarafından desteklenmişse, sunucular trafik artışlarını işlemek için daha iyi şekilde donatılmıştır. 

## <a name="bursting-flow"></a>Patlama akışı
Gereksiz kredi sistemi, hem sanal makine düzeyinde hem de disk düzeyinde aynı şekilde geçerlidir. Kaynağınız, bir VM ya da disk, tamamen stoksiz kredilerle başlar. Bu krediler, en fazla patlama hızında 30 dakika boyunca veri bloğu yapmanıza olanak sağlayacak. Patlama kredileri, kaynağınız performans disk depolama sınırları altında çalışırken biriktirir. Kaynağınızın, kaynağın altında kullandığı tüm ıOPS ve MB/sn 'ler için kredileri biriktirmek için başlarsınız. Kaynağınız, patlama için kullanılmak üzere tahakkuk eden krediler içeriyorsa ve iş yükünüzün ek performansa ihtiyacı varsa, kaynağınız bu kredileri kullanarak, talebi karşılamak için ihtiyaç duyması gereken disk GÇ performansını sağlamak için bu kredilerin performans sınırınızı üzerine gidebilirler.

![Patlama demeti diyagramı](media/managed-disks-bursting/bucket-diagram.jpg)

Veri bloğu birikmesi konusunda bir şey, kullanılmayan ıOPS 'yi temel alan ve performans tutarlarının altındaki MB/s değerlerini temel aldığı için her kaynak için farklı olmasıdır. Bu, daha yüksek temel performans ürünlerinin, en düşük temel performanslı ürünlerden daha hızlı bir şekilde patlama miktarlarını tahakkuk alabileceği anlamına gelir. Örneğin, hiçbir etkinlik olmadan bir P1 disk kimliği, bir P20 disk 2.300 tahakkuk ederken saniyede 120 ıOPS tahakkuk eder.

## <a name="bursting-states"></a>Gereksiz durumlar
Kaynağınız, yazılabilir özelliği etkin olan üç durum vardır:
- **Tahakkuk** : kaynağın GÇ trafiği performans hedefinden daha az kullanılıyor. IOPS için patlama kredisi ve MB/sn birbirinden farklı bir şekilde gerçekleştirilir. Kaynağınız ıOPS kredileri tahakkuk edebilir ve 50/s kredisi ya da tam tersi olabilir.
- **Burdımı** : kaynağın trafiği performans hedefinden daha fazla kullanılıyor. Veri bloğu trafiği ıOPS 'den veya bant genişliğinden bağımsız olarak kredileri kullanacaktır.
- **Sabit** : kaynağın trafiği tam olarak performans hedefidir.

## <a name="examples-of-bursting"></a>Burte örnek
Aşağıdaki örneklerde, birden fazla sanal makine ve disk birleşimleriyle birlikte nasıl çalıştığı gösterilmektedir. Örneklerin daha kolay olmasını sağlamak için MB/sn 'ye odaklanacağız, ancak aynı mantık ıOPS 'ye bağımsız olarak uygulanır.

### <a name="non-burstable-virtual-machine-with-burstable-disks"></a>Burstable diskler içeren, bursuz sanal makine
**VM ve disk birleşimi:** 
- Standard_D8as_v4 
    - Önbelleğe alınmamış MB/s: 192
- P4 işletim sistemi diski
    - Sağlanan MB/s: 25
    - En fazla patlama MB/s: 170 
- 2 P10 veri diski 
    - Sağlanan MB/s: 25
    - En fazla patlama MB/s: 170

 VM önyüklendiğinde, işletim sistemi diskinden veri alır. İşletim sistemi diski, başlangıç yapan bir sanal makinenin parçası olduğundan, işletim sistemi diski, ani kredilerin sonuna kadar olacaktır. Bu krediler, aşağıda görüldüğü gibi, işletim sistemi diskinin başlatılmasını 170 MB/s saniye sonra da sağlar:

![Bursuz sanal makine diski başlatma](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-startup.jpg)

Önyükleme tamamlandıktan sonra, bir uygulama VM üzerinde çalışır ve kritik olmayan bir iş yüküne sahiptir. Bu iş yükü, tüm diskler arasında eşit olarak yayılan 15 MB/S gerektirir:

![Bursuz sanal makine diski boşta](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-idling.jpg)

Sonra uygulamanın 192 MB/s gerektiren bir toplu işi işlemesi gerekir. 2 MB/s, işletim sistemi diski tarafından kullanılır ve REST, veri diskleri arasında eşit olarak bölünür:

![Bursuz sanal makine, disk patlaması](media/managed-disks-bursting/nonbursting-vm-busting-disk/nonbusting-vm-bursting-disk-bursting.jpg)

### <a name="burstable-virtual-machine-with-non-burstable-disks"></a>Bursuz diskler içeren Burstable sanal makine
**VM ve disk birleşimi:** 
- Standard_L8s_v2 
    - Önbelleğe alınmamış MB/s: 160
    - En fazla patlama MB/s: 1.280
- P50 işletim sistemi diski
    - Sağlanan MB/s: 250 
- 2 P10 veri diski 
    - Sağlanan MB/s: 250

 İlk önyüklemeden sonra, sanal makinede bir uygulama çalıştırılır ve kritik olmayan bir iş yüküne sahiptir. Bu iş yükü, tüm diskler arasında eşit olarak yayılan 30 MB/s gerektirir: ![gereksiz VM diske alınmamış disk boşta](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-normal.jpg)

Sonra uygulamanın 600 MB/s gerektiren bir toplu işi işlemesi gerekir. Standard_L8s_v2, bu talebi karşılamak ve ardından disklere yönelik istekler, P50 disklere eşit bir şekilde yayılmak üzere daha fazla bilgi alır:

![Patlama sanal makinesi diske alınmamış disk](media/managed-disks-bursting/bursting-vm-nonbursting-disk/burst-vm-nonbursting-disk-bursting.jpg)
### <a name="burstable-virtual-machine-with-burstable-disks"></a>Burstable disklere sahip Burstable sanal makine
**VM ve disk birleşimi:** 
- Standard_L8s_v2 
    - Önbelleğe alınmamış MB/s: 160
    - En fazla patlama MB/s: 1.280
- P4 işletim sistemi diski
    - Sağlanan MB/s: 25
    - En fazla patlama MB/s: 170 
- 2 P4 veri diski 
    - Sağlanan MB/s: 25
    - En fazla patlama MB/s: 170 

VM önyüklendiğinde, işletim sistemi diskinden 1.280 MB/sn 'ye ait patlama sınırını istemek için veri bloğu, işletim sistemi diski 170 MB/s veri bloğu performansı ile yanıt verir:

![Patlama sanal makinesi diski başlatma](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-startup.jpg)

Ardından, önyükleme tamamlandıktan sonra VM 'de bir uygulama çalıştırılır. Uygulamanın, tüm disklerde eşit olarak yayıldığı 15 MB/s gerektiren kritik olmayan bir iş yükü vardır:

![Patlama sanal makinesi diski boşta](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-idling.jpg)

Sonra uygulamanın 360 MB/s gerektiren bir toplu işi işlemesi gerekir. Standard_L8s_v2, bu talebi karşılamak ve sonra isteklerini karşılar. İşletim sistemi diski için yalnızca 20 MB/sn gerekiyor. Kalan 340 MB/s, burdıya P4 veri diskleri tarafından işlenir:  

![Patlama sanal makinesi disk patlaması](media/managed-disks-bursting/bursting-vm-bursting-disk/burst-vm-burst-disk-bursting.jpg)