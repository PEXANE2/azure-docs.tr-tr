---
title: 'Bağlantıyı doğrulama-ExpressRoute sorun giderme kılavuzu: Azure | Microsoft Docs'
description: Bu sayfa, bir ExpressRoute devresine yönelik uçtan uca bağlantıyı gidermeye ve doğrulamaya ilişkin yönergeler sağlar.
services: expressroute
author: rambk
ms.service: expressroute
ms.topic: article
ms.date: 09/26/2017
ms.author: rambala
ms.custom: seodec18
ms.openlocfilehash: 026900e3dcbf7c20750bb8e17e44ba64897c9a30
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71123455"
---
# <a name="verifying-expressroute-connectivity"></a>ExpressRoute bağlantısını doğrulama
Bu makale, ExpressRoute bağlantısını doğrulamanıza ve sorunlarını gidermenize yardımcı olur. ExpressRoute, bir bağlantı sağlayıcısı tarafından kolaylaştırılıan özel bir bağlantı üzerinden şirket içi ağı Microsoft bulutuna genişleten, aşağıdaki üç farklı ağ bölgesini içerir:

-   Müşteri ağı
-   Sağlayıcı ağı
-   Microsoft veri merkezi

Bu belgenin amacı, kullanıcının nerede (veya hatta) bir bağlantı sorunu olduğunu ve hangi bölgede olduğunu belirlemesine yardımcı olmak ve bu sayede sorunu çözmek için uygun takımdan yardım almak sağlamaktır. Bir sorunu çözmek için Microsoft desteği gerekiyorsa [Microsoft desteği][Support]bir destek bileti açın.

> [!IMPORTANT]
> Bu belge basit sorunları tanılamaya ve düzeltmeye yardımcı olmaya yöneliktir. Microsoft desteği için bir değişiklik olması amaçlanmamıştır. Belirtilen Kılavuzu kullanarak sorunu çözemediğiniz bir destek bileti [Microsoft desteği][Support] açın.
>
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagramda, ExpressRoute kullanarak bir müşteri ağının Microsoft Network 'e mantıksal bağlantısı gösterilmektedir.
[![1]][1]

Yukarıdaki diyagramda, numaralar anahtar ağ noktalarını gösterir. Ağ noktalarına genellikle ilgili numara ile bu makale aracılığıyla başvurulur.

ExpressRoute bağlantı modeline (bulut Exchange ortak konumu, noktadan noktaya Ethernet bağlantısı veya herhangi birine (ıPVPN)) bağlı olarak, ağ noktaları 3 ve 4 olabilir (katman 2 cihazları). Gösterilen anahtar ağ noktaları aşağıdaki gibidir:

1.  Müşteri işlem cihazı (örneğin, bir sunucu veya BILGISAYAR)
2.  CEs: müşteri Edge yönlendiricileri 
3.  PEs (CE 'ye yönelik): sağlayıcının Edge yönlendiricileri/ilgili müşteri Edge yönlendiricileriyle anahtarlar. Bu belgede PE-CEs adı verilir.
4.  PEs (MSEE 'e bakan): sağlayıcıya yönelik olan sağlayıcı Edge yönlendiricileri/anahtarları. Bu belgede PE-Mas olarak adlandırılır.
5.  MSEE: Microsoft Enterprise Edge (MSEE) ExpressRoute yönlendiricileri
6.  Sanal ağ (VNet) ağ geçidi
7.  Azure VNet 'te işlem cihazı

Bulut Exchange ortak konumu veya noktadan noktaya Ethernet bağlantı modelleri kullanılıyorsa, müşteri Edge yönlendiricisi (2) MSEE (5) ile BGP eşlemesi kurar. 3 ve 4 ağ noktaları hala mevcuttur ancak katman 2 cihazları olarak saydam olur.

Herhangi bir (ıPVPN) bağlantı modeli kullanılırsa, PEs (MSEE 'e bakan) (4), MSEE (5) ile BGP eşlemesi kurar. Ardından yollar, ıPVPN hizmet sağlayıcısı ağı aracılığıyla müşteri ağına geri yayılır.

> [!NOTE]
>ExpressRoute yüksek kullanılabilirliği için, Microsoft, MSEE (5) ve PE-MSEE (4) arasında fazladan bir BGP oturumu çifti gerektirir. Ek bir ağ yolu çifti de müşteri ağı ile PE 'ler arasında teşvik edilir. Ancak, herhangi bir (ıPVPN) bağlantı modelinde tek bir CE aygıtı (2) bir veya daha fazla PEs 'e (3) bağlı olabilir.
>
>

Bir ExpressRoute devresini doğrulamak için aşağıdaki adımlar ele alınmıştır (ağ noktası ilişkili sayıyla belirtilir):
1. [Devre sağlamayı ve durumu doğrula (5)](#validate-circuit-provisioning-and-state)
2. [En az bir ExpressRoute eşlemesinin yapılandırıldığını doğrula (5)](#validate-peering-configuration)
3. [Microsoft ile hizmet sağlayıcı arasında ARP 'yi doğrula (4 ve 5 arasında bağlantı)](#validate-arp-between-microsoft-and-the-service-provider)
4. [MSEE BGP ve yolları doğrulama (sanal ağ bağlantısı varsa 4 ila 5 arasında BGP ve 5 ila 6 arası)](#validate-bgp-and-routes-on-the-msee)
5. [Trafik Istatistiklerini denetleme (5 üzerinden geçen trafik)](#check-the-traffic-statistics)

Daha sonra daha fazla doğrulama ve denetim eklenecektir, her ay geri çekin!

## <a name="validate-circuit-provisioning-and-state"></a>Devre sağlamayı ve durumu doğrula
Bağlantı modelinden bağımsız olarak, bir ExpressRoute devresi oluşturulması ve bu nedenle devre sağlama için bir hizmet anahtarı oluşturulmalıdır. ExpressRoute devresini sağlamak, PE-MSEE (4) ve MSEE (5) arasında yedekli katman 2 bağlantıları kurar. ExpressRoute bağlantı hattı oluşturma, değiştirme, sağlama ve doğrulama hakkında daha fazla bilgi için, [bir ExpressRoute bağlantı hattı oluşturma ve değiştirme][CreateCircuit]makalesine bakın.

>[!TIP]
>Bir hizmet anahtarı, bir ExpressRoute devresini benzersiz bir şekilde tanımlar. Bu belgede belirtilen PowerShell komutlarının birçoğu için bu anahtar gereklidir. Ayrıca, bir ExpressRoute sorununu gidermek için Microsoft 'tan veya bir ExpressRoute ortağından yardım almanız gerekir, devreyi kolay bir şekilde tanımlamak için hizmet anahtarını sağlayın.
>
>

### <a name="verification-via-the-azure-portal"></a>Azure portal aracılığıyla doğrulama
Azure portal, bir ExpressRoute bağlantı hattının durumu, sol taraftaki çubuk menüsünde ![2][2] ' yi ve ardından ExpressRoute bağlantı hattını seçerek denetlenebilir. "Tüm kaynaklar" altında listelenen bir ExpressRoute devresini seçmek ExpressRoute bağlantı hattı dikey penceresini açar. Dikey pencerenin ![3][3] bölümünde ExpressRoute Essentials aşağıdaki ekran görüntüsünde gösterildiği gibi listelenir:

![4][4]    

ExpressRoute Essentials 'da, *devre durumu* Microsoft tarafında devre 'nın durumunu gösterir. *Sağlayıcı durumu* , devre dışı bırakıldığını/hizmet sağlayıcı tarafında *sağlanıp sağlanmadığını* gösterir. 

Bir ExpressRoute bağlantı hattının işlemsel olması için *devre durumunun* *etkinleştirilmesi* ve *sağlayıcı durumunun* *sağlanması*gerekir.

> [!NOTE]
> *Devre durumu* etkinleştirilmemişse [Microsoft desteği][Support]başvurun. *Sağlayıcı durumu* sağlanmamışsa, hizmet sağlayıcınıza başvurun.
>
>

### <a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Bir kaynak grubundaki tüm ExpressRoute devrelerini listelemek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Azure üzerinden kaynak grubu adınızı alabilirsiniz. Bu belgenin önceki alt bölümüne bakın ve örnek ekran görüntüsünde kaynak grubu adının listelendiğini unutmayın.
>
>

Bir kaynak grubunda belirli bir ExpressRoute devresini seçmek için aşağıdaki komutu kullanın:

    Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

Örnek yanıt:

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

Bir ExpressRoute bağlantı hattının çalışır durumda olup olmadığını doğrulamak için, aşağıdaki alanlara özellikle dikkat edin:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

> [!NOTE]
> *Devresi Provisioningstate* etkinleştirilmemişse, [Microsoft desteği][Support]başvurun. *Serviceproviderprovisioningstate* sağlanmamışsa, hizmet sağlayıcınıza başvurun.
>
>

### <a name="verification-via-powershell-classic"></a>PowerShell aracılığıyla doğrulama (klasik)
Bir abonelik altındaki tüm ExpressRoute devrelerini listelemek için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuit

Belirli bir ExpressRoute devresini seçmek için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

Örnek yanıt:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Bir ExpressRoute bağlantı hattının çalışır durumda olup olmadığını doğrulamak için, aşağıdaki alanlara özellikle dikkat edin: ServiceProviderProvisioningState: sağlanan durum: etkin

> [!NOTE]
> *Durum* etkinleştirilmemişse [Microsoft desteği][Support]başvurun. *Serviceproviderprovisioningstate* sağlanmamışsa, hizmet sağlayıcınıza başvurun.
>
>

## <a name="validate-peering-configuration"></a>Eşleme yapılandırmasını doğrula
Hizmet sağlayıcı ExpressRoute bağlantı hattını sağlamayı tamamladıktan sonra, MSEE-PR 'ler (4) ve MSEE (5) arasındaki ExpressRoute devresi üzerinden bir yönlendirme yapılandırması oluşturulabilir. Her ExpressRoute bağlantı hattı bir, iki veya üç yönlendirme bağlamı etkin olabilir: Azure özel eşlemesi (Azure 'da özel sanal ağlara trafik), Azure genel eşlemesi (Azure 'da genel IP adreslerine giden trafik) ve Microsoft eşlemesi (Office 365 trafiği). Yönlendirme yapılandırması oluşturma ve değiştirme hakkında daha fazla bilgi için, [bir ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme][CreatePeering]makalesine bakın.

### <a name="verification-via-the-azure-portal"></a>Azure portal aracılığıyla doğrulama

> [!NOTE]
> Katman 3 hizmet sağlayıcısı tarafından sağlanmışsa ve eşlemeler portalda boşsa, portalda Yenile düğmesini kullanarak devre yapılandırmasını yenileyin. Bu işlem, devreniz için doğru yönlendirme yapılandırmasını uygular. 
>
>

Azure portal, bir ExpressRoute bağlantı hattının durumu, sol taraftaki çubuk menüsünde ![2][2] ' yi ve ardından ExpressRoute bağlantı hattını seçerek denetlenebilir. "Tüm kaynaklar" altında listelenen bir ExpressRoute devresini seçmek ExpressRoute bağlantı hattı dikey penceresini açar. Dikey pencerenin ![3][3] bölümünde ExpressRoute Essentials aşağıdaki ekran görüntüsünde gösterildiği gibi listelenir:

![5][5]

Yukarıdaki örnekte, belirtilen Azure özel eşleme yönlendirme bağlamı etkinleştirildiğinden Azure genel ve Microsoft eşleme yönlendirme bağlamları etkin değildir. Başarıyla etkinleştirilmiş bir eşleme bağlamı, listelenen birincil ve ikincil noktadan noktaya (BGP için gereklidir) alt ağları da içermelidir. /30 alt ağları, MSEE ve PE-MSEE Arabirim IP adresi için kullanılır. 

> [!NOTE]
> Bir eşleme etkinleştirilmemişse, atanan birincil ve ikincil alt ağların PE-MSEE yapılandırması ile eşleşip eşleşmediğinden emin olun. Aksi takdirde, MSEE yönlendiricilerinde yapılandırmayı değiştirmek için bkz. [ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme][CreatePeering]
>
>

### <a name="verification-via-powershell"></a>PowerShell aracılığıyla doğrulama
Azure özel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Başarılı bir şekilde yapılandırılmış özel eşleme için örnek yanıt:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 Başarıyla etkinleştirilen bir eşleme bağlamı, birincil ve ikincil adres ön eklerinin listelenmesine neden olur. /30 alt ağları, MSEE ve PE-MSEE Arabirim IP adresi için kullanılır.

Azure genel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt

Microsoft eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    $ckt = Get-AzExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
     Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Bir eşleme yapılandırılmamışsa, bir hata iletisi olur. Bir örnek yanıt, belirtilen eşleme (Bu örnekteki Azure ortak eşlemesi) devre içinde yapılandırılmadı:

    Get-AzExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


> [!NOTE]
> Bir eşleme etkinleştirilmemişse, atanan birincil ve ikincil alt ağların bağlı PE-MSEE yapılandırmasıyla eşleşip eşleşmediğinden emin olun. Ayrıca, MSEE doğru *Vlanıd*, *Azureasn*ve *peerasn* 'nın kullanıldığını ve bu değerlerin bağlı PE-MSEE ile eşlenenlere eşlendiğini kontrol edin. MD5 karma seçilirse, paylaşılan anahtar MSEE ve PE-MSEE çiftinde aynı olmalıdır. MSEE yönlendiricilerinde yapılandırmayı değiştirmek için, [bir ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme][CreatePeering]bölümüne bakın.  
>
>

### <a name="verification-via-powershell-classic"></a>PowerShell aracılığıyla doğrulama (klasik)
Azure özel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutu kullanın:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

Başarılı bir şekilde yapılandırılmış özel eşleme için örnek yanıt:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

Başarılı, etkin bir eşleme bağlamı, birincil ve ikincil eşdüzey alt ağlarını listeleyebilir. /30 alt ağları, MSEE ve PE-MSEE Arabirim IP adresi için kullanılır.

Azure genel eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

Microsoft eşleme yapılandırma ayrıntılarını almak için aşağıdaki komutları kullanın:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

> [!IMPORTANT]
> Katman 3 eşayarları hizmet sağlayıcısı tarafından ayarlandıysa, portal veya PowerShell aracılığıyla ExpressRoute eşayarlarını ayarlamak hizmet sağlayıcısı ayarlarının üzerine yazar. Sağlayıcı tarafı eşleme ayarlarını sıfırlamak için hizmet sağlayıcısı desteği gerekir. Yalnızca hizmet sağlayıcının katman 2 hizmetleri sağlaması durumunda ExpressRoute eşayarlarını değiştirin!
>
>

> [!NOTE]
> Eşleme etkinleştirilmediyse, birincil ve ikincil eşdüzey alt ağlarının bağlı PE-MSEE yapılandırma ile eşleşip eşleşmediğinden emin olun. Ayrıca, MSEE doğru *Vlanıd*, *Azureasn*ve *peerasn* 'nın kullanıldığını ve bu değerlerin bağlı PE-MSEE ile eşlenenlere eşlendiğini kontrol edin. MSEE yönlendiricilerinde yapılandırmayı değiştirmek için, [bir ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme][CreatePeering]bölümüne bakın.
>
>

## <a name="validate-arp-between-microsoft-and-the-service-provider"></a>Microsoft ile hizmet sağlayıcı arasında ARP doğrulama
Bu bölümde PowerShell (klasik) komutları kullanılmaktadır. PowerShell Azure Resource Manager komutlarını kullanıyorsanız, aboneliğe yönelik yönetici/ortak yönetici erişimine sahip olduğunuzdan emin olun. Azure Resource Manager komutlarını kullanarak sorun giderme için lütfen [Kaynak Yöneticisi dağıtım modeli BELGESINDE ARP tablolarını alma][ARP] bölümüne bakın.

> [!NOTE]
>ARP 'yi almak için hem Azure portal hem de Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutlarıyla hatalar ile karşılaşılırsa klasik PowerShell komutları, klasik PowerShell komutları Azure Resource Manager ExpressRoute devreleri ile de çalışır.
>
>

Özel eşleme için birincil MSEE yönlendiricisinde ARP tablosunu almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Başarılı senaryoda komut için örnek bir yanıt:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

Benzer şekilde, *özel* /*genel* /*Microsoft* eşlemeleri için *birincil* /*İkincil* yolundaki MSEE ARP tablosunu kontrol edebilirsiniz.

Aşağıdaki örnek, bir eşleme için komutun yanıtı olmadığını gösterir.

    ARP Info:
       
> [!NOTE]
> ARP tablosunun MAC adreslerine eşlenen arabirimlerin IP adresleri yoksa, aşağıdaki bilgileri gözden geçirin:
>1. MSEE-PR ve MSEE arasında bağlantı için atanan/30 alt ağının ilk IP adresi MSEE-PR arabiriminde kullanılırsa. Azure, Mgördüğü için her zaman ikinci IP adresini kullanır.
>2. Müşterinin (C-Tag) ve hizmet (S-Tag) VLAN etiketlerinin hem MSEE-PR hem de MSEE çiftinin ile eşleşip eşleşmediğinden emin olun.
>
>

## <a name="validate-bgp-and-routes-on-the-msee"></a>MSEE BGP ve rotalar doğrulama
Bu bölümde PowerShell (klasik) komutları kullanılmaktadır. PowerShell Azure Resource Manager komutlarını kullanıyorsanız, aboneliğe yönelik yönetici/ortak yönetici erişimine sahip olduğunuzdan emin olun.

> [!NOTE]
>BGP bilgilerini almak için hem Azure portal hem de Azure Resource Manager PowerShell komutları kullanılabilir. Azure Resource Manager PowerShell komutlarıyla hatalar ile karşılaşılırsa klasik PowerShell komutları, klasik PowerShell komutları Azure Resource Manager ExpressRoute devreleri ile de çalışır.
>
>

Belirli bir yönlendirme bağlamı için yönlendirme tablosu (BGP komşusu) özetini almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Örnek yanıt:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

Yukarıdaki örnekte gösterildiği gibi, komut yönlendirme bağlamının ne kadar süreyle kurulabileceğini belirlemede yararlı olur. Ayrıca, eşleme yönlendiricisi tarafından tanıtılan rota önekleri sayısını belirtir.

> [!NOTE]
> Durum etkin veya boşta ise, bağlı PE-MSEE yapılandırma ile ilişkili birincil ve ikincil eş alt ağlarının olup olmadığını denetleyin. Ayrıca, MSEE doğru *Vlanıd*, *Azureasn*ve *peerasn* 'nın kullanıldığını ve bu değerlerin bağlı PE-MSEE ile eşlenenlere eşlendiğini kontrol edin. MD5 karma seçilirse, paylaşılan anahtar MSEE ve PE-MSEE çiftinde aynı olmalıdır. MSEE yönlendiricilerinde yapılandırmayı değiştirmek için, [bir ExpressRoute bağlantı hattı için yönlendirme oluşturma ve değiştirme][CreatePeering]bölümüne bakın.
>
>

> [!NOTE]
> Belirli bir eşleme üzerinden erişilebilir değilse, belirli eşleme bağlamına ait olan MSEE yol tablosunu kontrol edin. Yönlendirme tablosunda eşleşen bir ön ek (IP olabilir) varsa, yolda güvenlik duvarları/NSG/ACL 'ler olup olmadığını ve trafiğe izin verip vermediğinin kontrol edin.
>
>

Belirli bir *özel* yönlendirme bağlamı için *birincil* yolda MSEE tam yönlendirme tablosunu almak için aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Komut için başarılı bir sonuç örneği şunlardır:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

Benzer şekilde,*Microsoft* 'un bir eşleme bağlamı olan *özel* /*genel* / için *birincil* /*İkincil* yolundaki MSEE yönlendirme tablosunu kontrol edebilirsiniz.

Aşağıdaki örnek, bir eşleme için komutun yanıtı olmadığını gösterir:

    Route Table Info:

## <a name="check-the-traffic-statistics"></a>Trafik Istatistiklerini denetleme
Birleşik birincil ve ikincil yol trafiği istatistiklerini almak için, bir eşleme bağlamının içindeki ve çıkan baytlar, aşağıdaki komutu kullanın:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Komutun örnek çıktısı şu şekilde olur:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Mevcut olmayan bir eşleme için komutun örnek çıktısı:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi veya yardım için aşağıdaki bağlantıları inceleyin:

- [Microsoft Desteği][Support]
- [ExpressRoute bağlantı hattı oluşturma ve değiştirme][CreateCircuit]
- [ExpressRoute devresi için yönlendirme oluşturma ve değiştirme][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "mantıksal Express yol bağlantısı"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Tüm kaynaklar simgesi"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Genel Bakış simgesi"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials örnek ekran görüntüsü"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials örnek ekran görüntüsü"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[ARP]: https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






