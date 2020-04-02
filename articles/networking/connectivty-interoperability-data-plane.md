---
title: "Azure'da birlikte çalışabilirlik : Veri düzlemi analizi"
description: Bu makalede, siteden siteye VPN olan ExpressRoute ile Azure'da sanal ağ eşlemesi arasında birlikte çalışabilirliği analiz etmek için kullanabileceğiniz test kurulumunun veri düzlemi çözümlemesi sağlanmaktadır.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: fe7b74b0d4d065d4f222fefbbdc4a1d434d1163b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80518257"
---
# <a name="interoperability-in-azure--data-plane-analysis"></a>Azure'da birlikte çalışabilirlik : Veri düzlemi analizi

Bu makalede, [test kurulumu][Setup]veri düzlemi analizi açıklanır. Ayrıca [test kurulumu yapılandırmasını][Configuration] ve test kurulumukontrol [düzlemi çözümlemesi][Control-Analysis] gözden geçirebilirsiniz.

Veri düzlemi çözümlemesi, bir yerel ağdan (LAN veya sanal ağ) bir topoloji içinde başka bir ağa geçen paketler tarafından alınan yolu inceler. İki yerel ağ arasındaki veri yolu mutlaka simetrik değildir. Bu nedenle, bu makalede, yerel bir ağdan ters yoldan ayrı başka bir ağa iletme yolunu çözümliyoruz.

## <a name="data-path-from-the-hub-vnet"></a>VNet hub'ından veri yolu

### <a name="path-to-the-spoke-vnet"></a>Konuşan VNet'e giden yol

Sanal ağ (VNet) bakan iki VNets arasında ağ köprü işlevselliğini taklit eder. Bir hub VNet'ten vnet'e konuşan VNet'teki VM'ye traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

Aşağıdaki şekilde, Hub VNet'in ve konuşan VNet'in grafik bağlantı görünümünü Azure Ağ İzleyicisi perspektifinden gösterir:


![1][1]

### <a name="path-to-the-branch-vnet"></a>VNet şubesine giden yol

VNet şubesindeki bir hub VNet'ten VNet'e izleme rotası çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Bu izleme rotasında, ilk atlama, VNet hub'ının Azure VPN Ağ Geçidi'ndeki VPN ağ geçididir. İkinci atlama şube VNet VPN ağ geçididir. VNet şubesinin VPN ağ geçidinin IP adresi vnet hub'ında ilan edilemiyor. Üçüncü atlama, VNet şubesindeki VM'dir.

Aşağıdaki şekilde Hub VNet ve şube VNet'in grafik bağlantı görünümünü Network Watcher perspektifinden gösterir:

![2][2]

Aynı bağlantı için, aşağıdaki şekil Ağ İzleyicisi'nde ızgara görünümünü gösterir:

![3][3]

### <a name="path-to-on-premises-location-1"></a>Şirket içi Konum 1'e giden yol

Şirket içi Konum 1'de bir hub VNet'ten VM'ye izleme rotası çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Bu izleme rotasında, ilk atlama, Microsoft Enterprise Edge Router'a (MSEE) giden Azure ExpressRoute ağ geçidi tüneli bitiş noktasıdır. İkinci ve üçüncü atlamalar müşteri kenarı (CE) yönlendiricive şirket içi Konum 1 LAN IP'leridir. Bu IP adresleri hub VNet'te reklamı yapılan değildir. Dördüncü atlama, şirket içi Konum 1'deki VM'dir.


### <a name="path-to-on-premises-location-2"></a>Şirket içi Konum 2'ye giden yol

Şirket içi Konum 2'de bir hub VNet'ten VM'ye izleme rotası çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

Bu izleme rotasında, ilk atlama, Bir MSEE'nin ExpressRoute ağ geçidi tüneli bitiş noktasıdır. İkinci ve üçüncü atlamalar CE yönlendirici ve şirket içi Konum 2 LAN IP'leridir. Bu IP adresleri hub VNet'te reklamı yapılan değildir. Dördüncü atlama, şirket içi Konum 2'deki VM'dir.

### <a name="path-to-the-remote-vnet"></a>Uzak VNet'e giden yol

Bir hub VNet'ten uzak VNet'teki VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

Bu izleme rotasında, ilk atlama, Bir MSEE'nin ExpressRoute ağ geçidi tüneli bitiş noktasıdır. İkinci atlama uzak VNet ağ geçidi IP'si. İkinci atlama IP aralığı hub VNet'te reklamı yapılmaz. Üçüncü atlama uzak VNet VM olduğunu.

## <a name="data-path-from-the-spoke-vnet"></a>Konuşan VNet'ten veri yolu

Konuşan VNet, hub VNet'in ağ görünümünü paylaşır. VNet'in bakışları aracılığıyla, konuşan VNet hub VNet'in uzak ağ geçidi bağlantısını, sanki doğrudan konuşan VNet'e bağlıymuş gibi kullanır.

### <a name="path-to-the-hub-vnet"></a>Hub VNet'e giden yol

Hub VNet'te konuşan VNet'ten VNet'e traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet"></a>VNet şubesine giden yol

VNet şubesindeki vnet'ten VNet'e traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

Bu izleme rotasında, ilk atlama hub VNet VPN ağ geçididir. İkinci atlama şube VNet VPN ağ geçididir. VNet şubesinin VPN ağ geçidinin IP adresi hub/spoke VNet içinde ilan edilemez. Üçüncü atlama, VNet şubesindeki VM'dir.

### <a name="path-to-on-premises-location-1"></a>Şirket içi Konum 1'e giden yol

Şirket içi Konum 1'de konuşan VNet'ten VM'ye traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

Bu izleme rotasında, ilk atlama bir MSEE hub VNet's ExpressRoute ağ geçidi bitiş noktasıdır. İkinci ve üçüncü atlamalar CE yönlendirici ve şirket içi Konum 1 LAN IP'leridir. Bu IP adresleri hub/spoke VNet'te reklamı yapılan bir adres değildir. Dördüncü atlama, şirket içi Konum 1'deki VM'dir.

### <a name="path-to-on-premises-location-2"></a>Şirket içi Konum 2'ye giden yol

Şirket içi Konum 2'de konuşan VNet'ten VM'ye traceroute çıkışı burada gösterilmiştir:


    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    76 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

Bu izleme rotasında, ilk atlama bir MSEE hub VNet's ExpressRoute ağ geçidi bitiş noktasıdır. İkinci ve üçüncü atlamalar CE yönlendirici ve şirket içi Konum 2 LAN IP'leridir. Bu IP adresleri hub/spoke VNets'te tanıtılamıyor. Dördüncü atlama, şirket içi Konum 2'deki VM'dir.

### <a name="path-to-the-remote-vnet"></a>Uzak VNet'e giden yol

Konuşan VNet'ten uzak VNet'teki vm'ye traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

Bu izleme rotasında, ilk atlama bir MSEE hub VNet's ExpressRoute ağ geçidi bitiş noktasıdır. İkinci atlama uzak VNet ağ geçidi IP'si. İkinci atlama IP aralığı hub/spoke VNet'te reklamı yapılmaz. Üçüncü atlama uzak VNet VM olduğunu.

## <a name="data-path-from-the-branch-vnet"></a>VNet dalından veri yolu

### <a name="path-to-the-hub-vnet"></a>Hub VNet'e giden yol

VNet şubesinden vnet hub'ındaki vm'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

Bu izleme rotasında, ilk atlama vnet şubesinin VPN ağ geçididir. İkinci atlama hub VNet VPN ağ geçididir. Hub VNet'in VPN ağ geçidinin IP adresi uzak VNet'te ilan edilemez. Üçüncü atlama, VNet'in hub'ındaki VM'dir.

### <a name="path-to-the-spoke-vnet"></a>Konuşan VNet'e giden yol

VNet dalından vnet'e konuşan VNet'teki VM'ye traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

Bu izleme rotasında, ilk atlama vnet şubesinin VPN ağ geçididir. İkinci atlama hub VNet VPN ağ geçididir. Hub VNet'in VPN ağ geçidinin IP adresi uzak VNet'te ilan edilemez. Üçüncü atlama vm konuştu VNet olduğunu.

### <a name="path-to-on-premises-location-1"></a>Şirket içi Konum 1'e giden yol

Şirket içi Konum 1'deki VNet dalından VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

Bu izleme rotasında, ilk atlama vnet şubesinin VPN ağ geçididir. İkinci atlama hub VNet VPN ağ geçididir. Hub VNet'in VPN ağ geçidinin IP adresi uzak VNet'te ilan edilemez. Üçüncü atlama birincil CE yönlendirici üzerinde VPN tünel sonlandırma noktasıdır. Dördüncü atlama, şirket içi Konum 1'in dahili IP adresidir. Bu LAN IP adresi CE yönlendiricidışında ilan edilmez. Beşinci atlama, şirket içi Konum 1'deki hedef VM'dir.

### <a name="path-to-on-premises-location-2-and-the-remote-vnet"></a>Şirket içi Konum 2'ye ve uzak VNet'e giden yol

Kontrol düzlemi analizinde de ele aldığımız gibi, VNet şubesinin ağ yapılandırması başına şirket içi Konum 2'ye veya uzak VNet'e görünürlüğü yoktur. Aşağıdaki ping sonuçları onaylar: 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

## <a name="data-path-from-on-premises-location-1"></a>Şirket içi Konum 1'den veri yolu

### <a name="path-to-the-hub-vnet"></a>Hub VNet'e giden yol

Şirket içi Konum 1'den VNet hub'ındaki VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

Bu izleme rotasında, ilk iki atlama şirket içi ağın bir parçasıdır. Üçüncü atlama, CE yönlendiricisi ile karşı karşıya olan birincil MSEE arabirimidir. Dördüncü atlama, VNet hub'ının ExpressRoute ağ geçididir. Hub VNet'in ExpressRoute ağ geçidinin IP aralığı şirket içi ağa reklam vermez. Beşinci atlama hedef VM olduğunu.

Ağ İzleyicisi yalnızca Azure merkezli bir görünüm sağlar. Şirket içi bir bakış açısı için Azure Ağ Performans Monitörü'ü kullanıyoruz. Ağ Performans İzleyicisi, veri yolu çözümlemesi için Azure dışındaki ağlardaki sunucularda yükleyebileceğiniz aracılar sağlar.

Aşağıdaki şekil, ExpressRoute üzerinden VNet hub'ındaki VM'ye şirket içi Konum 1 VM bağlantısının topoloji görünümünü gösterir:

![4][4]

Daha önce de belirtildiği gibi, test kurulumu, şirket içi Konum 1 ve hub VNet arasında ExpressRoute için yedekleme bağlantısı olarak siteden siteye VPN kullanır. Yedekleme veri yolunu sınamak için, şirket içi Konum 1 birincil CE yönlendiricisi ile ilgili MSEE arasında bir ExpressRoute bağlantı hatası na neden olalım. ExpressRoute bağlantı hatasına neden olmak için, MSEE ile karşı karşıya olan CE arabirimini kapatın:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

Aşağıdaki şekil, ExpressRoute bağlantısı düştüğünde siteden siteye VPN bağlantısı aracılığıyla hub VNet'teki VM'ye şirket içi Konum 1 VM bağlantısının topoloji görünümünü gösterir:

![5][5]

### <a name="path-to-the-spoke-vnet"></a>Konuşan VNet'e giden yol

Şirket içi Konum 1'den vnet'teki VM'ye izleme yolu çıkışı burada gösterilmiştir:

Söze bağlı VNet'e yönelik veri yolu çözümlemesi yapmak için ExpressRoute birincil bağlantısını geri getirelim:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Veri yolu çözümlemesi geri kalanı için birincil ExpressRoute 1 bağlantısını getirin.

### <a name="path-to-the-branch-vnet"></a>VNet şubesine giden yol

Şirket içi Konum 1'den VNet şubesindeki VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

### <a name="path-to-on-premises-location-2"></a>Şirket içi Konum 2'ye giden yol

[Kontrol düzlemi analizinde][Control-Analysis]tartıştığımız gibi, şirket içi Konum 1'in ağ yapılandırması başına şirket içi Konum 2'ye görünürlüğü yoktur. Aşağıdaki ping sonuçları onaylar: 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

### <a name="path-to-the-remote-vnet"></a>Uzak VNet'e giden yol

Şirket içi Konum 1'den uzak VNet'teki VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

## <a name="data-path-from-on-premises-location-2"></a>Şirket içi Konum 2'den veri yolu

### <a name="path-to-the-hub-vnet"></a>Hub VNet'e giden yol

Şirket içi Konum 2'den VNet hub'ındaki VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Konuşan VNet'e giden yol

Şirket içi Konum 2'den vnet'teki VM'ye traceroute çıkışı burada gösterilmiştir:

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-on-premises-location-1-and-the-remote-vnet"></a>Şube VNet'e giden yol, şirket içi Konum 1 ve uzak VNet

[Kontrol düzlemi analizinde][Control-Analysis]tartıştığımız gibi, şirket içi Konum 1'in VNet şubesini, şirket içi Konum 1'i veya ağ yapılandırması başına uzak VNet'i görünürlüğü yoktur. 

## <a name="data-path-from-the-remote-vnet"></a>Uzak VNet'ten veri yolu

### <a name="path-to-the-hub-vnet"></a>Hub VNet'e giden yol

Uzak VNet'ten vnet hub'ındaki bir VM'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

### <a name="path-to-the-spoke-vnet"></a>Konuşan VNet'e giden yol

Uzak VNet'ten vnet'e giden traceroute çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-the-branch-vnet-and-on-premises-location-2"></a>Şube VNet ve şirket içi Konum 2 yolu

[Kontrol düzlemi analizinde][Control-Analysis]tartıştığımız gibi, uzak VNet'in vnet şubesiveya ağ yapılandırması başına şirket içi Konum 2'ye görünürlüğü yoktur. 

### <a name="path-to-on-premises-location-1"></a>Şirket içi Konum 1'e giden yol

Uzak VNet'ten şirket içi Konum 1'deki vm'ye izleme yolu çıkışı burada gösterilmiştir:

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>ExpressRoute ve siteden siteye VPN bağlantısı birlikte

###  <a name="site-to-site-vpn-over-expressroute"></a>ExpressRoute üzerinden siteden siteye VPN

ExpressRoute Microsoft'u kullanarak şirket içi ağınızla Azure VNet'leriniz arasında özel olarak veri alışverişi yaparak siteden siteye VPN yapılandırabilirsiniz. Bu yapılandırma ile, gizlilik, özgünlük ve bütünlük ile veri alışverişi yapabilirsiniz. Veri alışverişi de anti-replay olduğunu. ExpressRoute Microsoft'u kullanarak tünel modunda siteden siteye IPsec VPN'i nasıl yapılandırılabilen hakkında daha fazla bilgi için [ExpressRoute Microsoft'un üzerinden Siteden siteye VPN'e][S2S-Over-ExR]bakın. 

Microsoft'un eşlemasını kullanan siteden siteye VPN yapılandırmanın birincil sınırlaması iş bölümüdür. IPsec tüneli üzerindeki iş çıkışı VPN ağ geçidi kapasitesiyle sınırlıdır. VPN ağ geçidi iş girişi ExpressRoute iş lerinden daha düşüktür. Bu senaryoda, son derece güvenli trafik için IPsec tünelini kullanmak ve diğer tüm trafik için özel bakışkullanmak ExpressRoute bant genişliği kullanımını optimize etmesine yardımcı olur.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>ExpressRoute için güvenli bir arıza yolu olarak siteden siteye VPN

ExpressRoute, yüksek kullanılabilirlik sağlamak için yedekli bir devre çifti olarak hizmet vermektedir. Coğrafi yedekli ExpressRoute bağlantısını farklı Azure bölgelerinde yapılandırabilirsiniz. Ayrıca, bir Azure bölgesinde test kurulumumuzda gösterildiği gibi, ExpressRoute bağlantınız için bir arıza yolu oluşturmak için siteden siteye VPN kullanabilirsiniz. Aynı önekler hem ExpressRoute hem de siteden siteye VPN üzerinden duyurulduğunda, Azure ExpressRoute'a öncelik verir. ExpressRoute ile siteden siteye VPN arasında asimetrik yönlendirmeyi önlemek için, şirket içi ağ yapılandırması, siteden siteye VPN bağlantısını kullanmadan önce ExpressRoute bağlantısını kullanarak da karşılık vermelidir.

ExpressRoute ve siteden siteye VPN için birlikte varolan bağlantıları niçin yapılandırılabilen ler hakkında daha fazla bilgi için [ExpressRoute ve siteden siteye birlikte yaşama][ExR-S2S-CoEx]bilgi sini görün.

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Arka uç bağlantısını konuşan VNet'lere ve şube konumlarına genişletin

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>VNet eşleme kullanarak Kollu VNet bağlantısı

Hub ve kollu VNet mimarisi yaygın olarak kullanılır. Hub, Azure'da, konuştuğunuz VNet'leriniz ve şirket içi ağınız arasında merkezi bir bağlantı noktası görevi gören bir VNet'tir. Sözcüler, hub'a göre eşleyen ve iş yüklerini yalıtmak için kullanabileceğiniz VNet'lerdir. Bir ExpressRoute veya VPN bağlantısı üzerinden şirket içi veri merkezi ve hub arasında trafik akışı. Mimari hakkında daha fazla bilgi için [bkz.][Hub-n-Spoke]

VNet'in bir bölge içinde ki bakışlarında, kollu VNet'ler uzak ağlarla iletişim kurmak için hub VNet ağ geçitlerini (hem VPN hem de ExpressRoute ağ geçitleri) kullanabilir.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Siteden siteye VPN kullanarak Şube VNet bağlantısı

Farklı bölgelerde bulunan şube VNet'lerinin ve şirket içi ağların bir hub VNet üzerinden birbirleriyle iletişim kurmasını isteyebilirsiniz. Bu yapılandırma için yerel Azure çözümü, VPN kullanarak siteden siteye VPN bağlantısıdır. Bir alternatif hub yönlendirme için bir ağ sanal cihaz (NVA) kullanmaktır.

Daha fazla bilgi için VPN Ağ [Deploy a highly available NVA][Deploy-NVA] [Geçidi nedir?][VPN]


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler için [ExpressRoute SSS'sine][ExR-FAQ] bakın:
-   Bir ExpressRoute ağ geçidine kaç ExpressRoute devresi bağlanabileceğinizi öğrenin.
-   Bir ExpressRoute devresine kaç ExpressRoute ağ geçidi bağlanabileceğinizi öğrenin.
-   ExpressRoute'un diğer ölçek sınırları hakkında bilgi edinin.


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "Ağ İzleyicisi'nin hub VNet'ten konuşan bir VNet'e bağlantı görünümü"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "Ağ İzleyicisi bir hub VNet'ten bir dal VNet'e bağlantı görünümü"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "Hub VNet'ten bir dal VNet'e bağlantının Ağ İzleyici ızgara görünümü"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "ExpressRoute 1 ile Konum 1 VM'den hub VNet'e bağlantının Ağ Performans Monitörü görünümü"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "Siteden siteye VPN aracılığıyla Konum 1 VM'den hub VNet'e bağlantının Ağ Performans Monitörü görünümü"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


