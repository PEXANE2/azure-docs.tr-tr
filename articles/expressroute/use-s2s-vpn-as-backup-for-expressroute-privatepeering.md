---
title: Azure ExpressRoute Özel Eşleme için yedekleme olarak S2S VPN kullanma | Microsoft Dokümanlar
description: Bu sayfa, Azure ExpressRoute özel s2S VPN ile yedeklemek için mimari öneriler sağlar.
services: networking
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 02/05/2020
ms.author: rambala
ms.openlocfilehash: a6a22b667bc66d6ee69bfbd7ad1db88f72d8df0e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687861"
---
# <a name="using-s2s-vpn-as-a-backup-for-expressroute-private-peering"></a>ExpressRoute özel eşleme için yedek olarak S2S VPN kullanma

[ExpressRoute özel bakışile olağanüstü durum kurtarma tasarımı][DR-PP]başlıklı makalede, bir ExpressRoute özel bakan bağlantı için yedek bağlantı çözümü için ihtiyaç ve nasıl amaç için coğrafi-yedekli ExpressRoute devreleri kullanmak tartışıldı. Bu makalede, expressroute özel bakış için bir geri olarak siteden siteye (S2S) VPN kaldıraç ve korumak nasıl düşünelim. 

Coğrafi yedekli ExpressRoute devrelerinin aksine, ExpressRoute-VPN olağanüstü durum kurtarma birleşimini yalnızca etkin-pasif modda kullanabilirsiniz. Pasif modda herhangi bir yedekleme ağı bağlantısı kullanmanın en büyük zorluğu, pasif bağlantının genellikle birincil bağlantının yanında başarısız olmasıdır. Pasif bağlantı hatalarının yaygın nedeni etkin bakım eksikliğidir. Bu nedenle, bu makalede, expressroute özel bir bakış yedekleme s2S VPN bağlantısı doğrulamak ve aktif olarak korumak için nasıl odaklanalım.

>[!NOTE] 
>Belirli bir rota hem ExpressRoute hem de VPN üzerinden duyurulduğunda, Azure ExpressRoute üzerinden yönlendirmeyi tercih eder.  
>

Bu makalede, hem Azure perspektifinden hem de müşteri yan ağ kenarı perspektifinden bağlantının nasıl doğrulaşdırılabildiğini görelim. Microsoft ağ varlıklarıyla eşleyen müşteri yan ağ aygıtlarını yönetip yönetmediğinize bakılmaksızın, her iki uçtan da doğrulama olanağı yardımcı olur. 

## <a name="example-topology"></a>Örnek topoloji

Kurulumumuzda, hem ExpressRoute devresi hem de S2S VPN bağlantısı üzerinden Azure hub VNet'e bağlı bir şirket içi ağımız vardır. Azure hub'ı VNet, aşağıdaki diyagramda gösterildiği gibi, sırayla bir vnet'e bakar:

![1][1]

Kurulumda, ExpressRoute devresi şirket içi "Customer Edge" (CE) yönlendiricileri bir çift sonlandırılır. Şirket içi LAN, lider takipçi modunda çalışan bir çift güvenlik duvarı aracılığıyla CE yönlendiricilerine bağlanır. S2S VPN doğrudan güvenlik duvarlarında sonlandırılır.

Aşağıdaki tabloda topolojinin anahtar IP önekleri listelenebedilir:

| **Varlık** | **Ön ek** |
| --- | --- |
| Şirket içi LAN | 10.1.11.0/25 |
| Azure Hub VNet | 10.17.11.0/25 |
| Azure VNet konuştu | 10.17.11.128/26 |
| Şirket içi test sunucusu | 10.1.11.10 |
| Kollu VNet testi VM | 10.17.11.132 |
| ExpressRoute birincil bağlantı p2p alt ağ | 192.168.11.16/30 |
| ExpressRoute ikincil bağlantı p2p subnet | 192.168.11.20/30 |
| VPN ağ geçidi birincil BGP eş IP | 10.17.11.76 |
| VPN ağ geçidi ikincil BGP eş IP | 10.17.11.77 |
| Şirket içi güvenlik duvarı VPN BGP eş IP | 192.168.11.88 |
| Güvenlik duvarı IP'sine doğru birincil CE yönlendirici i/f | 192.168.11.0/31 |
| Birincil CE yönlendirici IP'ye doğru güvenlik duvarı i/f | 192.168.11.1/31 |
| Güvenlik duvarı IP'sine doğru ikincil CE yönlendirici i/f | 192.168.11.2/31 |
| İkincil CE yönlendirici IP'ye doğru güvenlik duvarı i/f | 192.168.11.3/31 |


Aşağıdaki tabloda topolojinin ASN'leri listelenebilmişve listelenebilmişdir:

| **Özerk sistem** | **ASN** |
| --- | --- |
| Şirket içi | 65020 |
| Microsoft Kurumsal Kenar | 12076 |
| Sanal Ağ GW (ExR) | 65515 |
| Sanal Ağ GW (VPN) | 65515 |

## <a name="high-availability-without-asymmetricity"></a>Asimetri olmadan yüksek kullanılabilirlik

### <a name="configuring-for-high-availability"></a>Yüksek kullanılabilirlik için yapılandırma

[ExpressRoute ve Siteden Siteye birlikte var olan bağlantıları yapılandırın,][Conf-CoExist] birlikte var olan ExpressRoute devresi ve S2S VPN bağlantılarının nasıl yapılandırılabildiğini tartışır. [ExpressRoute ile yüksek kullanılabilirlik için Tasarım][HA]tartışılan gibi , ExpressRoute yüksek kullanılabilirlik geliştirmek için kurulum ağ artıklığı korur (başarısızlık tek bir nokta önler) uç noktalara kadar tüm yol. Ayrıca, ExpressRoute devrelerinin hem birincil hem de ikincil bağlantıları, şirket içi önekleri her iki bağlantıda da aynı şekilde reklam vererek etkin-etkin modda çalışacak şekilde yapılandırılmıştır. 

ExpressRoute devresinin birincil bağlantısı üzerinden birincil CE yönlendiricinin şirket içi rota reklamı aşağıda gösterilmiştir (Junos komutları):

    user@SEA-MX03-01> show route advertising-protocol bgp 192.168.11.18 

    Cust11.inet.0: 8 destinations, 8 routes (7 active, 0 holddown, 1 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

ExpressRoute devresinin ikincil bağlantısı üzerinden ikincil CE yönlendiricinin şirket içi rota reklamı aşağıda gösterilmiştir (Junos komutları):

    user@SEA-MX03-02> show route advertising-protocol bgp 192.168.11.22 

    Cust11.inet.0: 8 destinations, 8 routes (7 active, 0 holddown, 1 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

Yedekleme bağlantısının yüksek kullanılabilirliğini artırmak için, S2S VPN de etkin-aktif modda yapılandırılır. Azure VPN ağ geçidi yapılandırması aşağıda gösterilmiştir. VPN yapılandırması VPN bir parçası olarak ağ geçidiBGP peer IP adresleri -10.17.11.76 ve 10.17.11.77- de listelenir.

![2][2]

Şirket içi rota, güvenlik duvarları tarafından VPN ağ geçidinin birincil ve ikincil BGP eşlerine duyurulur. Rota reklamları aşağıda gösterilmiştir (Junos):

    user@SEA-SRX42-01> show route advertising-protocol bgp 10.17.11.76 

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

    {primary:node0}
    user@SEA-SRX42-01> show route advertising-protocol bgp 10.17.11.77    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
    * 10.1.11.0/25            Self                                    I

>[!NOTE] 
>S2S VPN'in etkin-etkin modda yapılandırılması yalnızca olağanüstü durum kurtarma yedekleme ağ bağlantınıza yüksek kullanılabilirlik sağlamakla kalmıyor, aynı zamanda yedekleme bağlantısına daha yüksek verim sağlar. Başka bir deyişle, S2S VPN'in etkin-etkin modda yapılandırılma, temeltünellerin birden çok oluşturulmasına zorlanması önerilir.
>

### <a name="configuring-for-symmetric-traffic-flow"></a>Simetrik trafik akışı için yapılandırma

Belirli bir şirket içi rota hem ExpressRoute hem de S2S VPN üzerinden duyurulduğunda, Azure'un ExpressRoute yolunu tercih edeceğini kaydettik. Azure'u birlikte var olan ExpressRoute üzerinden S2S VPN yolunu tercih etmeye zorlamak için VPN bağlantısı üzerinden daha spesifik yolların (daha büyük alt ağ maskesi ile daha uzun önek) reklamını yapmanız gerekir. Buradaki amacımız VPN bağlantılarını sadece geri kullanmaktır. Bu nedenle, Azure'un varsayılan yol seçimi davranışı hedefimizle uyumludur. 

Şirket içinde Azure'a giden trafiğin S2S VPN üzerinden ExpressRoute yolunu da tercih etmesini sağlamak bizim sorumluluğumuzdur. Şirket içi kurulumumuzda CE yönlendiricilerinin ve güvenlik duvarlarının varsayılan yerel tercihi 100'dür. Bu nedenle, ExpressRoute özel bakışları üzerinden alınan rotaların yerel tercihini 100'den büyük (örneğin 150) yapılandırarak, Azure'a giden trafiğin sabit durumda ExpressRoute devresini tercih etmesini sağlayabiliriz.

ExpressRoute devresinin birincil bağlantısını sonlandıran birincil CE yönlendiricisinin BGP yapılandırması aşağıda gösterilmiştir. iBGP oturumu üzerinden reklamı yapılan yolların yerel tercihinin değeri 150 olarak yapılandırılmıştır. Benzer şekilde, ExpressRoute devresinin ikincil bağlantısını sonlandıran ikincil CE yönlendiricisinin yerel tercihinin de 150 olarak yapılandırıldığından emin olmamız gerekir.

    user@SEA-MX03-01> show configuration routing-instances Cust11 
    description "Customer 11 VRF";
    instance-type virtual-router;
    interface xe-0/0/0:0.110;
    interface ae0.11;
    protocols {
      bgp {
        group ibgp {
            type internal;
            local-preference 150;
            neighbor 192.168.11.1;
        }
        group ebgp {
            peer-as 12076;
            bfd-liveness-detection {
                minimum-interval 300;
                multiplier 3;
            }
            neighbor 192.168.11.18;
        }
      }
    }

Şirket içi güvenlik duvarlarının yönlendirme tablosu, Azure'a giden şirket içi trafik için tercih edilen yolun sabit durumda ExpressRoute üzerinde olduğunu (aşağıda gösterilmiştir) doğrular.

    user@SEA-SRX42-01> show route table Cust11.inet.0 10.17.11.0/24    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
    + = Active Route, - = Last Active, * = Both

    10.17.11.0/25      *[BGP/170] 2d 00:34:04, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.0 via reth1.11
                          to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 00:34:01, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 21:12:13, localpref 100, from 10.17.11.76
                          AS path: 65515 I, validation-state: unverified
                        > via st0.118
                        [BGP/170] 2d 00:41:51, localpref 100, from 10.17.11.77
                          AS path: 65515 I, validation-state: unverified
                        > via st0.119
    10.17.11.76/32     *[Static/5] 2d 21:12:16
                        > via st0.118
    10.17.11.77/32     *[Static/5] 2d 00:41:56
                        > via st0.119
    10.17.11.128/26    *[BGP/170] 2d 00:34:04, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.0 via reth1.11
                          to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 00:34:01, localpref 150
                          AS path: 12076 I, validation-state: unverified
                        > to 192.168.11.2 via reth2.11
                        [BGP/170] 2d 21:12:13, localpref 100, from 10.17.11.76
                          AS path: 65515 I, validation-state: unverified
                        > via st0.118
                        [BGP/170] 2d 00:41:51, localpref 100, from 10.17.11.77
                          AS path: 65515 I, validation-state: unverified
                        > via st0.119

Yukarıdaki rota tablosunda, hub ve kollu VNet rotaları için--10.17.11.0/25 ve 10.17.11.128/26-- ExpressRoute devresinin VPN bağlantıları na göre tercih edilir. 192.168.11.0 ve 192.168.11.2, CE yönlendiricilere yönelik güvenlik duvarı arabirimindeki IP'lerdir.

## <a name="validation-of-route-exchange-over-s2s-vpn"></a>S2S VPN üzerinden rota değişiminin doğrulanması

Bu makalenin başlarında, güvenlik duvarlarının vpn ağ geçidinin birincil ve ikincil BGP eşlerine şirket içi rota reklamını doğruladık. Ayrıca, VPN ağ geçidinin birincil ve ikincil BGP eşlerinden güvenlik duvarları tarafından alınan Azure rotalarını da doğrulayalım.

    user@SEA-SRX42-01> show route receive-protocol bgp 10.17.11.76 table Cust11.inet.0 

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
      10.17.11.0/25           10.17.11.76                             65515 I
      10.17.11.128/26         10.17.11.76                             65515 I

    {primary:node0}
    user@SEA-SRX42-01> show route receive-protocol bgp 10.17.11.77 table Cust11.inet.0    

    Cust11.inet.0: 14 destinations, 21 routes (14 active, 0 holddown, 0 hidden)
      Prefix                  Nexthop              MED     Lclpref    AS path
      10.17.11.0/25           10.17.11.77                             65515 I
      10.17.11.128/26         10.17.11.77                             65515 I

Benzer şekilde, Azure VPN ağ geçidi tarafından alınan şirket içi ağ rotası önekleri için de doğrulayalım. 

    PS C:\Users\user> Get-AzVirtualNetworkGatewayLearnedRoute -ResourceGroupName SEA-Cust11 -VirtualNetworkGatewayName SEA-Cust11-VNet01-gw-vpn | where {$_.Network -eq "10.1.11.0/25"} | select Network, NextHop, AsPath, Weight

    Network      NextHop       AsPath      Weight
    -------      -------       ------      ------
    10.1.11.0/25 192.168.11.88 65020        32768
    10.1.11.0/25 10.17.11.76   65020        32768
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 192.168.11.88 65020        32768
    10.1.11.0/25 10.17.11.77   65020        32768
    10.1.11.0/25 10.17.11.69   12076-65020  32769
    10.1.11.0/25 10.17.11.69   12076-65020  32769

Yukarıda görüldüğü gibi, VPN ağ geçidi, VPN ağ geçidinin birincil ve ikincil BGP eşleri tarafından alınan rotalara sahiptir. Ayrıca birincil ve ikincil ExpressRoute bağlantıları (12076 ile hazırlanmış AS-path olanlar) üzerinden alınan güzergahlar üzerinde görünürlüğe sahiptir. VPN bağlantıları üzerinden alınan yolları doğrulamak için, bağlantıların şirket içi BGP eş IP'sini bilmemiz gerekir. Göz önünde bulundurulan kurulumumuzda 192.168.11.88 ve biz ondan alınan yolları görüyoruz.

Ardından, Azure VPN ağ geçidi tarafından şirket içi güvenlik duvarı BGP eşine (192.168.11.88) reklamı yapılan yolları doğrulayalım.

    PS C:\Users\user> Get-AzVirtualNetworkGatewayAdvertisedRoute -Peer 192.168.11.88 -ResourceGroupName SEA-Cust11 -VirtualNetworkGatewayName SEA-Cust11-VNet01-gw-vpn |  select Network, NextHop, AsPath, Weight

    Network         NextHop     AsPath Weight
    -------         -------     ------ ------
    10.17.11.0/25   10.17.11.76 65515       0
    10.17.11.128/26 10.17.11.76 65515       0
    10.17.11.0/25   10.17.11.77 65515       0
    10.17.11.128/26 10.17.11.77 65515       0


Rota alışverişinin yapılamaması bağlantı hatasını gösterir. Sorun [Giderme: Azure sitesinden siteye VPN bağlantısı bağlanamaz ve][VPN Troubleshoot] VPN bağlantısını giderme de yardım için çalışmayı durdurur.

## <a name="testing-failover"></a>Test başarısız

Şimdi VPN bağlantısı (kontrol uçağı) üzerinden başarılı rota alışverişi doğruladı, biz VPN bağlantısı ExpressRoute bağlantısı ndan trafik (veri düzlemi) geçiş için ayarlanır. 

>[!NOTE] 
>Üretim ortamlarında arıza testi, servis aksatıcı olabileceğiiçin zamanlanan ağ bakım iş penceresi sırasında yapılmalıdır.
>

Trafik anahtarını yapmak için önce, kurulumumuzdaki geçerli yolu şirket içi test sunucusundan konuşan VNet'teki test VM'ye kadar takip edelim.

    C:\Users\PathLabUser>tracert 10.17.11.132

    Tracing route to 10.17.11.132 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.11.1
      2    <1 ms    <1 ms    11 ms  192.168.11.0
      3    <1 ms    <1 ms    <1 ms  192.168.11.18
      4     *        *        *     Request timed out.
      5     6 ms     6 ms     5 ms  10.17.11.132

    Trace complete.

Kurulumumuzun birincil ve ikincil ExpressRoute noktadan noktaya bağlantı alt ağları sırasıyla 192.168.11.16/30 ve 192.168.11.20/30'dır. Yukarıdaki izleme rotasında, 3. MSEE arabiriminin varlığı, beklendiği gibi mevcut yolumuzun ExpressRoute üzerinde olduğunu doğrulamaktadır.

[Reset ExpressRoute devresi eşlemelerinde][RST]bildirildiği gibi, ExpressRoute devresinin hem birincil hem de ikincil eşlemesini devre dışı bırakmak için aşağıdaki powershell komutlarını kullanalım.

    $ckt = Get-AzExpressRouteCircuit -Name "expressroute name" -ResourceGroupName "SEA-Cust11"
    $ckt.Peerings[0].State = "Disabled"
    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt

Başarısız geçiş süresi BGP yakınsama süresine bağlıdır. Kurulumumuzda, arıza anahtarı birkaç saniye sürer (10'dan az). Geçişten sonra, izleme yolunun tekrarı aşağıdaki yolu gösterir:

    C:\Users\PathLabUser>tracert 10.17.11.132

    Tracing route to 10.17.11.132 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.11.1
      2     *        *        *     Request timed out.
      3     6 ms     7 ms     9 ms  10.17.11.132

    Trace complete.

İzleme rotası sonucu, S2S VPN üzerinden yedekleme bağlantısının etkin olduğunu ve hem birincil hem de ikincil ExpressRoute bağlantılarının başarısız olması durumunda hizmet sürekliliği sağlayabileceğini doğrular. Başarısız lık testini tamamlamak için, aşağıdaki komut kümesini kullanarak ExpressRoute bağlantılarını etkinleştirelim ve trafik akışını normalleştirelim.

    $ckt = Get-AzExpressRouteCircuit -Name "expressroute name" -ResourceGroupName "SEA-Cust11"
    $ckt.Peerings[0].State = "Enabled"
    Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt

Trafiğin ExpressRoute'a geri döndüğünü doğrulamak için, izleme rotasını tekrarlayın ve ExpressRoute özel eşlemelerinden geçtiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute, Microsoft ağında tek bir hata noktası olmadan yüksek kullanılabilirlik için tasarlanmıştır. Yine de bir ExpressRoute devresi tek bir coğrafi bölge ve bir servis sağlayıcı ile sınırlıdır. S2S VPN bir ExpressRoute devre için iyi bir felaket kurtarma pasif yedekleme çözümü olabilir. Güvenilir bir pasif yedekleme bağlantısı çözümü, pasif yapılandırmanın düzenli olarak sürdürülmesi ve periyodik doğrulama için bağlantı önemlidir. VPN yapılandırmasının eskihaline gelmesine izin vermemek ve periyodik olarak (her üç ayda bir söylemek gerekirse) bakım penceresi sırasında bu makalede açıklanan doğrulama ve başarısız test adımlarını yinelemek önemlidir.

VPN ağ geçidi ölçümlerine dayalı izleme ve uyarıları etkinleştirmek için VPN [Ağ Geçidi ölçümlerinde uyarıları ayarlama'ya][VPN-alerts]bakın.

Bir ExpressRoute hatasının ardından BGP yakınsamasını hızlandırmak [için BFD'yi ExpressRoute üzerinden yapılandırın.][BFD]

<!--Image References-->
[1]: ./media/use-s2s-vpn-as-backup-for-expressroute-privatepeering/topology.png "topoloji göz önünde bulunduruluyor"
[2]: ./media/use-s2s-vpn-as-backup-for-expressroute-privatepeering/vpn-gw-config.png "VPN GW yapılandırması"

<!--Link References-->
[DR-PP]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering
[Conf-CoExist]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[VPN Troubleshoot]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-site-to-site-cannot-connect
[VPN-alerts]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric
[BFD]: https://docs.microsoft.com/azure/expressroute/expressroute-bfd
[RST]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-reset-peering



