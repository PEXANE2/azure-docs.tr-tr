---
title: 'Azure ExpressRoute: ARP tabloları - Sorun giderme'
description: Bu sayfa, ExpressRoute devresi için ARP tabloları alma hakkında talimatlar sağlar
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: b31e9eb852c69d5f02eb855e319d2a4901942994
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618533"
---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Kaynak Yöneticisi dağıtım modelinde ARP tabloları alma
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makalede, ExpressRoute devreniz için ARP tablolarını öğrenmek için aşağıdaki adımlar size yol gösterir.

> [!IMPORTANT]
> Bu belge, basit sorunları tanılamanıza ve düzeltmenize yardımcı olmak için tasarlanmıştır. Microsoft desteğinin yerine geçmesi amaçlanmamıştır. Aşağıda açıklanan kılavuzu kullanarak sorunu çözemiyorsanız, [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ile bir destek bileti açmanız gerekir.
> 
> 

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
Adres Çözümleme Protokolü (ARP), [RFC 826'da](https://tools.ietf.org/html/rfc826)tanımlanan katman 2 protokolüdür. ARP, Ethernet adresini (MAC adresi) ip adresiyle eşlemek için kullanılır.

ARP tablosu, belirli bir eşleme için ipv4 adresinin ve MAC adresinin eşlenemesini sağlar. ExpressRoute devre si leşme için ARP tablosu her arabirim için aşağıdaki bilgileri sağlar (birincil ve ikincil)

1. Şirket içi yönlendirici arabirimi ip adresinin MAC adresine eşleneme
2. ExpressRoute yönlendirici arabirimi ip adresinin MAC adresine eşleneme
3. Haritalama yaşı

ARP tabloları katman 2 yapılandırmayı doğrulamaya ve temel katman 2 bağlantı sorunlarını gidermede yardımcı olabilir. 

Örnek ARP tablosu: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


Aşağıdaki bölümde, ExpressRoute kenar yönlendiricileri tarafından görülen ARP tablolarını nasıl görüntülediğiniz hakkında bilgi verilmektedir. 

## <a name="prerequisites-for-learning-arp-tables"></a>ARP tablolarını öğrenmek için ön koşullar
İlerlemeden önce aşağıdakileri aldığınızdan emin olun

* En az bir eşleme ile yapılandırılan Geçerli Bir ExpressRoute devresi. Devre tamamen bağlantı sağlayıcısı tarafından yapılandırılmalıdır. Siz (veya bağlantı sağlayıcınız) bu devredeki eşlemelerden (Azure özel, Azure geneli ve Microsoft) en az birini yapılandırmış olmalısınız.
* Eşlemeleri yapılandırmak için kullanılan IP adresi aralıkları (Azure özel, Azure genel ve Microsoft). Ip adreslerinin sizin tarafınızdaki ve ExpressRoute tarafındaki arabirimlere nasıl eşlendirildiğini anlamak için [ExpressRoute yönlendirme gereksinimleri sayfasındaki](expressroute-routing.md) ip adresi atama örneklerini gözden geçirin. [ExpressRoute peering yapılandırma sayfasını](expressroute-howto-routing-arm.md)inceleyerek eşleme yapılandırması hakkında bilgi alabilirsiniz.
* Bu IP adresleriile kullanılan arabirimlerin MAC adreslerindeki ağ ekibinizden / bağlantı sağlayıcınızdan gelen bilgiler.
* Azure için en son PowerShell modülüne sahip olmalısınız (sürüm 1.50 veya daha yeni).

> [!NOTE]
> Katman 3 servis sağlayıcı tarafından sağlanıyorsa ve Aşağıdaki portal/çıktıda ARP tabloları boşsa, portaldaki yenileme düğmesini kullanarak Devre yapılandırmasını yenileyin. Bu işlem, devrenize doğru yönlendirme yapılandırmasını uygular. 
>
>

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>ExpressRoute devreniz için ARP tablolarını alma
Bu bölümde, PowerShell kullanarak her eşleme başına ARP tablolarını nasıl görüntülediğinize ilişkin yönergeler verilmektedir. Siz veya bağlantı sağlayıcınız daha fazla ilerlemeden önce eşlemi yapılandırmış olmalıdır. Her devrenin iki yolu vardır (birincil ve ikincil). Her yol için ARP tablosunu bağımsız olarak denetleyebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>Azure özel eşleme için ARP tabloları
Aşağıdaki cmdlet, Azure özel eşleme için ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Örnekler, yollardan biri için aşağıda gösterilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1   ffff.eeee.dddd
          0 Microsoft         10.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure genel ekimi için ARP tabloları
Aşağıdaki cmdlet, Azure genel ekimi için ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Örnekler, yollardan biri için aşağıda gösterilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1   ffff.eeee.dddd
          0 Microsoft         64.0.0.2   aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft'un bakışları için ARP tabloları
Aşağıdaki cmdlet Microsoft'un bakış için ARP tabloları sağlar

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secondary path
        Get-AzExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Örnekler, yollardan biri için aşağıda gösterilmiştir

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgilerin nasıl kullanılacağı
Bir eşlemenin ARP tablosu, katman 2 yapılandırmasını ve bağlantısını doğrulamak için kullanılabilir. Bu bölümde, ARP tablolarının farklı senaryolar altında nasıl görüneceğine genel bir bakış sağlanmıştır.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Bir devre çalışma durumundayken ARP tablosu (beklenen durum)
* ARP tablosu, geçerli bir IP adresi ve MAC adresi ve Microsoft tarafı için benzer bir giriş ile şirket içi yan için bir giriş olacaktır. 
* Şirket içi ip adresinin son sekizlisi her zaman tek bir sayı olacaktır.
* Microsoft ip adresinin son sekizlisi her zaman çift sayı olacaktır.
* Aynı MAC adresi tüm 3 eşleme (birincil / ikincil) için Microsoft tarafında görünür. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1   ffff.eeee.dddd
          0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>Şirket içi / bağlantı sağlayıcı tarafında sorunlar olduğunda ARP tablosu
Şirket içi veya bağlantı sağlayıcısıyla ilgili sorunlar varsa, ARP tablosunda yalnızca bir girişin görüneceğini veya şirket içi MAC adresinin eksik göstereceğini görebilirsiniz. Bu, Mac adresi ile Microsoft tarafında kullanılan IP adresi arasındaki eşleşmeyi gösterir. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc

or
       
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------   
         0 On-Prem           65.0.0.1   Incomplete
         0 Microsoft         65.0.0.2   aaaa.bbbb.cccc


> [!NOTE]
> Bu tür sorunları hata ayıklamak için bağlantı sağlayıcınızla bir destek isteği açın. ARP tablosunda MAC adreslerine eşlenen arabirimlerin IP adresleri yoksa, aşağıdaki bilgileri gözden geçirin:
> 
> 1. MSEE-PR ve MSEE arasındaki bağlantı için atanan /30 alt netin ilk IP adresi MSEE-PR'ın arayüzünde kullanılırsa. Azure her zaman MSEE'ler için ikinci IP adresini kullanır.
> 2. Müşteri (C-Tag) ve hizmet (S-Tag) VLAN etiketlerinin hem MSEE-PR hem de MSEE çifti yle eşleşip eşleşmeyin.
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>Microsoft tarafında sorunlar olduğunda ARP tablosu
* Microsoft tarafında sorunlar varsa, bir bakış için gösterilen bir ARP tablosu görmezsiniz. 
* Microsoft desteği ile bir destek bileti [açın.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Katman 2 bağlantısıyla ilgili bir sorununuzun olduğunu belirtin. 

## <a name="next-steps"></a>Sonraki Adımlar
* ExpressRoute devreniz için Katman 3 yapılandırmalarını doğrulayın
  * BGP oturumlarının durumunu belirlemek için rota özeti ni alın 
  * ExpressRoute'da hangi öneklerin reklamının verilebilen rota tablosunu alın
* /out baytlarını gözden geçirerek veri aktarımLarını doğrulayın
* Sorun yaşamaya devam ediyorsanız [Microsoft desteğiyle](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bir destek bileti açın.

