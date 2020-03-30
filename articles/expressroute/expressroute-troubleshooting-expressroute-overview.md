---
title: 'Azure ExpressRoute: Bağlantıyı Doğrula - Sorun Giderme Kılavuzu'
description: Bu sayfa, bir ExpressRoute devresinin uçtan uca bağlantısını gidermek ve sorun giderme hakkında yönergeler sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 10/31/2019
ms.author: rambala
ms.custom: seodec18
ms.openlocfilehash: 58ae39e8dfdf918ae14ca9bb8dac28405828999e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330966"
---
# <a name="verifying-expressroute-connectivity"></a>ExpressRoute bağlantısını doğrulama
Bu makale, ExpressRoute bağlantısını doğrulamanıza ve sorun gidermenize yardımcı olur. ExpressRoute, bir bağlantı sağlayıcısı tarafından yaygın olarak kolaylaştırılanan özel bir bağlantı üzerinden şirket içi ağı Microsoft bulutuna genişletir. ExpressRoute bağlantısı geleneksel olarak aşağıdaki gibi üç farklı ağ bölgesi içerir:

-   Müşteri Ağı
-   Sağlayıcı Ağı
-   Microsoft Veri Merkezi

> [!NOTE]
> ExpressRoute doğrudan bağlantı modelinde (10/100 Gbps bant genişliğinde sunulur), müşteriler doğrudan Microsoft Enterprise Edge (MSEE) yönlendiricilerin bağlantı noktasına bağlanabilir. Bu nedenle, doğrudan bağlantı modelinde yalnızca müşteri ve Microsoft ağ bölgeleri vardır.
>


Bu belgenin amacı, kullanıcının bir bağlantı sorununun olup olmadığını ve nerede olduğunu belirlemesine yardımcı olmaktır. Bu nedenle, bir sorunu çözmek için uygun ekipten destek istemeye yardımcı olmak için. Bir sorunu gidermek için Microsoft desteği gerekiyorsa, [Microsoft Desteği][Support]ile bir destek bileti açın.

> [!IMPORTANT]
> Bu belge, basit sorunları tanılama ve düzeltmeye yardımcı olmak için tasarlanmıştır. Microsoft desteğinin yerine geçmesi amaçlanmamıştır. Sağlanan kılavuzu kullanarak sorunu çözemiyorsanız [Microsoft Destek][Support] ile bir destek bileti açın.
>
>

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagram, ExpressRoute kullanarak bir müşteri ağının Microsoft ağına mantıksal bağlantısını gösterir.
[![1]][1]

Önceki diyagramda, sayılar anahtar ağ noktalarını gösterir. Bu ağ noktaları, bu makalede zaman zaman ilişkili sayılarına göre başvurulmaktadır. ExpressRoute bağlantı modeline bağlı olarak --Cloud Exchange Co-location, Point-to-Any Ethernet Connection veya Any-to-any (IPVPN)-- ağ noktaları 3 ve 4'lü anahtarlar (Katman 2 aygıtları) veya yönlendiriciler (Katman 3 aygıtları) olabilir. Doğrudan bağlantı modelinde 3 ve 4 ağ noktası yoktur; bunun yerine CEs (2) doğrudan koyu lif ile MSEEs bağlı. Gösterilen önemli ağ noktaları aşağıdaki gibidir:

1.  Müşteri bilgi işlem aygıtı (örneğin, bir sunucu veya PC)
2.  CEs: Müşteri kenar yönlendiricileri 
3.  P'ler (CE bakan): Müşteri kenarı yönlendiricilerine bakan sağlayıcı kenar yönlendiricileri/anahtarları. Bu belgede PE-C'ler olarak anılacaktır.
4.  PEs (MSEE facing): MSEEs karşı karşıya sağlayıcı kenar yönlendiriciler / anahtarları. Bu belgede PE-MSEEs olarak anılacaktır.
5.  MSEEs: Microsoft Enterprise Edge (MSEE) ExpressRoute yönlendiricileri
6.  Sanal Ağ (VNet) Ağ Geçidi
7.  Azure VNet'te işlem aygıtı

Bulut Exchange Ortak konumu, Noktadan Noktaya Ethernet veya doğrudan bağlantı modelleri kullanılırsa, CEs (2) MSEE'lerle BGP'yi kurar (5). 

Any-to-any (IPVPN) bağlantı modeli kullanılırsa, PE-MSEEs (4) MSEEs (5) ile BGP peering kurmak. PE-MSEE'ler, IPVPN servis sağlayıcı ağı üzerinden Microsoft'tan alınan yolları müşteri ağına geri yayır.

> [!NOTE]
>Yüksek kullanılabilirlik için Microsoft, MSEE'ler (5) ve PE-MSEEs (4) çiftleri arasında tamamen gereksiz bir paralel bağlantı kurar. Tamamen yedekli paralel ağ yolu da müşteri ağı ve PE-C'ler çifti arasında teşvik edilir. Yüksek kullanılabilirlik ile ilgili daha fazla bilgi için [ExpressRoute ile yüksek kullanılabilirlik için Tasarım][HA] makalesine bakın
>
>

ExpressRoute devresi ile ilgili sorun gidermede mantıksal adımlar şunlardır:

* [Devre sağlama ve durumu doğrulayın](#verify-circuit-provisioning-and-state)
  
* [Eşleme Yapılandırması'nı doğrula](#validate-peering-configuration)
  
* [ARP'yi Doğrula](#validate-arp)
  
* [MSEE'de BGP'yi ve rotaları doğrulayın](#validate-bgp-and-routes-on-the-msee)
  
* [Trafik akışını onaylama](#confirm-the-traffic-flow)


## <a name="verify-circuit-provisioning-and-state"></a>Devre sağlama ve durumu doğrulayın
ExpressRoute devresi sağlanması, CEs/PE-MSEEs (2)/(4) ve MSEE'ler (5) arasında yedek bir Katman 2 bağlantısı kurar. Bir ExpressRoute devresinin nasıl oluşturulup değiştirilen, sağlanıp doğrulanıncaya kadar olduğu hakkında daha fazla bilgi için [ExpressRoute devresi oluştur ve değiştirmakaleye][CreateCircuit]bakın.

>[!TIP]
>Bir hizmet anahtarı, expressroute devresi benzersiz olarak tanımlar. ExpressRoute sorununu gidermek için Microsoft'tan veya bir ExpressRoute iş ortağından yardıma ihtiyacınız olursa, devreyi kolayca tanımlamak için hizmet anahtarını sağlayın.
>
>

### <a name="verification-via-the-azure-portal"></a>Azure portalı üzerinden doğrulama
Azure portalında ExpressRoute devre bıçağını açın. Bıçağın ![3][3] bölümünde, ExpressRoute temelleri aşağıdaki ekran görüntüsünde gösterildiği gibi listelenir:

![4][4]    

ExpressRoute Essentials'da *Devre durumu,* Microsoft tarafındaki devrenin durumunu gösterir. *Sağlayıcı durumu,* devrenin servis sağlayıcı tarafında *sağlanmış/sağlanmadığını* gösterir. 

Bir ExpressRoute devresinin çalışır durumda olması için *Devre durumu* *etkinleştirilmeli* ve *Sağlayıcı durumu* *sağlanmalıdır.*

> [!NOTE]
> ExpressRoute devresini yapılandırdıktan sonra, *Devre durumu* etkin olmayan durumda vurulsa, [Microsoft Destek'e][Support]başvurun. Diğer taraftan, Sağlayıcı *statüsü* sağlanmamış bir durumda vurulsa, servis sağlayıcınıza başvurun.
>
>

### <a name="verification-via-powershell"></a>PowerShell ile Doğrulama
Kaynak Grubu'ndaki tüm ExpressRoute devrelerini listelemek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Bir kaynak grubunun adını arıyorsanız, *get-AzResourceGroup* komutunu kullanarak aboneliğinizdeki tüm kaynak gruplarını listeleyerek bu grubu alabilirsiniz.
>


Kaynak Grubunda belirli bir ExpressRoute devresini seçmek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Örnek bir yanıt:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

ExpressRoute devresinin çalışır durumda olup olmadığını doğrulamak için aşağıdaki alanlara özellikle dikkat edin:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

> [!NOTE]
> ExpressRoute devresini yapılandırdıktan sonra, *Devre durumu* etkin olmayan durumda vurulsa, [Microsoft Destek'e][Support]başvurun. Diğer taraftan, Sağlayıcı *statüsü* sağlanmamış bir durumda vurulsa, servis sağlayıcınıza başvurun.
>
>

## <a name="validate-peering-configuration"></a>Eşleme Yapılandırması'nı doğrula
Servis sağlayıcı ExpressRoute devresinin sağlanmasını tamamladıktan sonra, CEs/MSEE-PEs (2)/(4) ve MSEEs (5) arasındaki ExpressRoute devresi üzerinden birden fazla eBGP tabanlı yönlendirme yapılandırması oluşturulabilir. Her ExpressRoute devresi olabilir: Azure özel bakış (Azure'daki özel sanal ağlara trafik) ve/veya Microsoft'a bakış (PaaS ve SaaS'in ortak uç noktalarına giden trafik). Yönlendirme yapılandırmasını nasıl oluşturup değiştirebilirsiniz hakkında daha fazla bilgi için [ExpressRoute devresi için yönlendirme oluştur ve değiştir'][CreatePeering]makalesine bakın.

### <a name="verification-via-the-azure-portal"></a>Azure portalı üzerinden doğrulama

> [!NOTE]
> IPVPN bağlantı modelinde, hizmet sağlayıcılar eşlemeleri (katman 3 hizmetleri) yapılandırma sorumluluğunu ele alar. Böyle bir modelde, servis sağlayıcı bir eşleme yapılandırdıktan sonra ve portalda eşleme boşsa, portaldaki yenileme düğmesini kullanarak devre yapılandırmasını yenilemeyi deneyin. Bu işlem, geçerli yönlendirme yapılandırmasını devrenizden çeker. 
>

Azure portalında, ExpressRoute devresi izleme durumunu ExpressRoute devre siilesi altında niçin kontrol edilebilir. Bıçağın ![3][3] bölümünde, ExpressRoute eşlemeleri aşağıdaki ekran görüntüsünde gösterildiği gibi listelenir:

![5][5]

Önceki örnekte, belirtildiği gibi Azure özel eşlemeleri sağlanmış, Azure genel ve Microsoft eşlemeleri ise sağlanmaz. Başarılı bir şekilde sağlanan bir eşleme bağlamı da birincil ve ikincil noktadan noktaya alt ağları listelenir. /30 alt ağlar, MSEE'lerin ve CE'lerin/PE-MSEE'lerin arayüz IP adresi için kullanılır. Sağlanan eşlemeler için, liste yapılandırmayı en son kimin değiştirdiğini de gösterir. 

> [!NOTE]
> Bir eşleme etkinleştirme başarısız olursa, atanan birincil ve ikincil alt ağların bağlı CE/PE-MSEE'deki yapılandırmayla eşleşip eşleşmeyini kontrol edin. Ayrıca doğru *VlanId*, *AzureASN*ve *PeerASN'nin* MSEE'lerde kullanIlip kullanılmayadiğini ve bu değerlerin bağlı CE/PE-MSEE'de kullanılan değerlerle eşleyip eşlenmediğini de kontrol edin. MD5 karmaseçilirse, paylaşılan anahtar MSEE ve PE-MSEE/CE çiftinde aynı olmalıdır. Önceden yapılandırılmış paylaşılan anahtar güvenlik nedenleriyle görüntülenmez. Bir MSEE yönlendiricisindeki bu yapılandırmalardan herhangi birini değiştirmeniz gerekiyorsa, [ExpressRoute devresi için yönlendirme oluştur ve değiştir'e][CreatePeering]bakın.  
>

> [!NOTE]
> Arabirim için atanan bir /30 alt ağda, Microsoft MSEE arabirimi için alt netin ikinci kullanılabilir IP adresini seçer. Bu nedenle, alt netin ilk kullanılabilir IP adresinin eşlenen CE/PE-MSEE'ye atandığından emin olun.
>


### <a name="verification-via-powershell"></a>PowerShell ile Doğrulama
Azure özel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Başarılı bir şekilde yapılandırılmış özel bir bakış için örnek bir yanıt:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : 123##
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 200
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Başarılı bir şekilde etkinleştirilmiş bir eşleme bağlamı, birincil ve ikincil adres önekleri listelenir. /30 alt ağlar, MSEE'lerin ve CE'lerin/PE-MSEE'lerin arayüz IP adresi için kullanılır.

Azure genel bakış yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt

Microsoft eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
     Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Bir eşleme yapılandırılmamışsa, bir hata iletisi olacaktır. Belirtilen eşleme (bu örnekteki Azure Genel ibaresi) devre içinde yapılandırılmamışsa, örnek bir yanıt:

    Get-AzExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


> [!NOTE]
> Bir eşleme etkinleştirme başarısız olursa, atanan birincil ve ikincil alt ağların bağlı CE/PE-MSEE'deki yapılandırmayla eşleşip eşleşmeyini kontrol edin. Ayrıca doğru *VlanId*, *AzureASN*ve *PeerASN'nin* MSEE'lerde kullanIlip kullanılmayadiğini ve bu değerlerin bağlı CE/PE-MSEE'de kullanılan değerlerle eşleyip eşlenmediğini de kontrol edin. MD5 karmaseçilirse, paylaşılan anahtar MSEE ve PE-MSEE/CE çiftinde aynı olmalıdır. Önceden yapılandırılmış paylaşılan anahtar güvenlik nedenleriyle görüntülenmez. Bir MSEE yönlendiricisindeki bu yapılandırmalardan herhangi birini değiştirmeniz gerekiyorsa, [ExpressRoute devresi için yönlendirme oluştur ve değiştir'e][CreatePeering]bakın.  
>
>

> [!NOTE]
> Arabirim için atanan bir /30 alt ağda, Microsoft MSEE arabirimi için alt netin ikinci kullanılabilir IP adresini seçer. Bu nedenle, alt netin ilk kullanılabilir IP adresinin eşlenen CE/PE-MSEE'ye atandığından emin olun.
>

## <a name="validate-arp"></a>ARP'yi Doğrula

ARP tablosu, belirli bir eşleme için IP adresinin ve MAC adresinin eşlenemesini sağlar. ExpressRoute devre si timasyonu için ARP tablosu her arabirim (birincil ve ikincil) için aşağıdaki bilgileri sağlar:
* Şirket içi yönlendirici arabirimi ip adresinin MAC adresine eşleneme
* ExpressRoute yönlendirici arabirimi ip adresinin MAC adresine eşleneme
* Eşleme li ARP tablolarının yaşı katman 2 yapılandırmasını doğrulamaya ve temel katman 2 bağlantı sorunlarını gidermede yardımcı olabilir.


[Bkz. Kaynak Yöneticisi dağıtım modeli belgesinde ARP tabloları][ARP] alma, ExpressRoute eşlemesinin ARP tablosunu nasıl görüntülenebilmek ve katman 2 bağlantı sorununu gidermek için bilgilerin nasıl kullanılacağı nasıI.


## <a name="validate-bgp-and-routes-on-the-msee"></a>MSEE'de BGP'yi ve rotaları doğrulayın

*Özel* yönlendirme bağlamı için *Birincil* yolda MSEE yönlendirme tablosu almak için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuitRouteTable -DevicePath Primary -ExpressRouteCircuitName ******* -PeeringType AzurePrivatePeering -ResourceGroupName ****

Örnek bir yanıt:

    Network : 10.1.0.0/16
    NextHop : 10.17.17.141
    LocPrf  : 
    Weight  : 0
    Path    : 65515

    Network : 10.1.0.0/16
    NextHop : 10.17.17.140*
    LocPrf  : 
    Weight  : 0
    Path    : 65515

    Network : 10.2.20.0/25
    NextHop : 172.16.0.1
    LocPrf  : 
    Weight  : 0
    Path    : 123##


> [!NOTE]
> Bir MSEE ile CE/PE-MSEE arasındaki eBGP'nin durumu Etkin veya Boşta ise, atanan birincil ve ikincil eş alt ağlarının bağlı CE/PE-MSEE'deki yapılandırmayla eşleşip eşleşmeyini kontrol edin. Ayrıca doğru *VlanId*, *AzureAsn*ve *PeerAsn'ın* MSEE'lerde kullanIlip kullanılmayadiğini ve bu değerlerin bağlı PE-MSEE/CE'de kullanılan değerlerle eşleyip eşleşmediğini de kontrol edin. MD5 karmaseçilirse, paylaşılan anahtar MSEE ve CE/PE-MSEE çiftinde aynı olmalıdır. Bir MSEE yönlendiricisindeki bu yapılandırmalardan herhangi birini değiştirmeniz gerekiyorsa, [ExpressRoute devresi için yönlendirme oluştur ve değiştir'e][CreatePeering]bakın.
>


> [!NOTE]
> Belirli hedeflere bir bakanlama üzerinden erişilemiyorsa, ilgili eşleme bağlamı için MSE'lerin rota tablosunu denetleyin. Yönlendirme tablosunda eşleşen bir önek (NATed IP olabilir) varsa, trafiği engelleyen yolda güvenlik duvarları/NSG/ACLs olup olmadığını kontrol edin.
>


Aşağıdaki örnek, var olmayan bir eşleme için komutun yanıtını gösterir:

    Get-AzExpressRouteCircuitRouteTable : The BGP Peering AzurePublicPeering with Service Key ********************* is not found.
    StatusCode: 400

## <a name="confirm-the-traffic-flow"></a>Trafik akışını onaylama
Birleştirilmiş birincil ve ikincil yol trafik istatistiklerini (baytlar bir eşleme bağlamına girip çıkar) almak için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuitStats -ResourceGroupName $RG -ExpressRouteCircuitName $CircuitName -PeeringType 'AzurePrivatePeering'

Komutun örnek çıktısı:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Var olmayan bir bakış için komutun örnek çıktısı:

    Get-AzExpressRouteCircuitRouteTable : The BGP Peering AzurePublicPeering with Service Key ********************* is not found.
    StatusCode: 400

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi veya yardım için aşağıdaki bağlantılara göz atın:

- [Microsoft Destek][Support]
- [ExpressRoute bağlantı hattı oluşturma ve değiştirme][CreateCircuit]
- [Bir ExpressRoute bağlantı hattı için yönlendirmeyi oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Mantıksal Ekspres Rota Bağlantısı"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Tüm kaynaklar simgesi"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Genel bakış simgesi"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials örnek ekran görüntüsü"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials örnek ekran görüntüsü"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager
[HA]: https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute
[DR-Pvt]: https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering





