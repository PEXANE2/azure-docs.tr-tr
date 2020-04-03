---
title: 'Azure ExpressRoute: ARP tabloları - Sorun giderme: klasik'
description: Bu sayfa, bir ExpressRoute devresi için ARP tabloları almak için talimatlar sağlar - klasik dağıtım modeli.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: article
ms.date: 01/30/2017
ms.author: charwen
ms.openlocfilehash: a7a24fc6674adca21e01d2502263c9767510469e
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618635"
---
# <a name="getting-arp-tables-in-the-classic-deployment-model"></a>Klasik dağıtım modelinde ARP tabloları alma
> [!div class="op_single_selector"]
> * [PowerShell - Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell - Klasik](expressroute-troubleshooting-arp-classic.md)
> 
> 

Bu makale, Azure ExpressRoute devreniz için Adres Çözümleme Protokolü (ARP) tablolarını alma adımlarında size yol gösterir.

> [!IMPORTANT]
> Bu belge, basit sorunları tanılamanıza ve düzeltmenize yardımcı olmak için tasarlanmıştır. Microsoft desteğinin yerine geçmesi amaçlanmamıştır. Aşağıdaki kılavuzu kullanarak sorunu çözemezseniz, [Microsoft Azure Yardım+desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)yle bir destek isteği açın.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adres Çözümleme Protokolü (ARP) ve ARP tabloları
ARP, [RFC 826'da](https://tools.ietf.org/html/rfc826)tanımlanan bir Katman 2 protokolüdür. ARP, bir Ethernet adresini (MAC adresi) bir IP adresiyle eşlemek için kullanılır.

ARP tablosu, belirli bir eşleme için IPv4 adresinin ve MAC adresinin eşlenemesini sağlar. ExpressRoute devre si timasyonu için ARP tablosu her arabirim (birincil ve ikincil) için aşağıdaki bilgileri sağlar:

1. Şirket içi yönlendirici arabirimi IP adresinin MAC adresine eşleneme
2. ExpressRoute yönlendirici arabirimi IP adresinin MAC adresine eşleneme
3. Haritalama yaşı

ARP tabloları Katman 2 yapılandırmalarını doğrulamaya ve temel Katman 2 bağlantı sorunlarını gidermede yardımcı olabilir.

Aşağıda bir ARP tablosu örneği verilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Aşağıdaki bölümde, ExpressRoute kenar yönlendiricileri tarafından görülen ARP tablolarının nasıl görüntüleneği hakkında bilgi verilmektedir.

## <a name="prerequisites-for-using-arp-tables"></a>ARP tablolarını kullanmak için ön koşullar
Devam etmeden önce aşağıdakileri yaptığınızdan emin olun:

* En az bir bakışla yapılandırılan geçerli bir ExpressRoute devresi. Devre tamamen bağlantı sağlayıcısı tarafından yapılandırılmalıdır. Siz (veya bağlantı sağlayıcınız) bu devredeki eşlemelerden (Azure özel, Azure geneli veya Microsoft) en az birini yapılandırmanız gerekir.
* Eşlemeleri yapılandırmak için kullanılan IP adresi aralıkları (Azure özel, Azure genel ivedi ve Microsoft). IP adreslerinin sizin tarafınızdaki ve ExpressRoute tarafındaki arabirimlere nasıl eşlendirildiğini anlamak için [ExpressRoute yönlendirme gereksinimleri sayfasındaki](expressroute-routing.md) IP adresi atama örneklerini gözden geçirin. [ExpressRoute peering yapılandırma sayfasını](expressroute-howto-routing-classic.md)inceleyerek eşleme yapılandırması hakkında bilgi alabilirsiniz.
* Ağ ekibinizden veya bağlantı sağlayıcınızdan, bu IP adresleriyle birlikte kullanılan arabirimlerin MAC adresleri hakkında bilgiler.
* Azure için en son Windows PowerShell modülü (sürüm 1.50 veya sonraki sürüm).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ExpressRoute devreniz için ARP tabloları
Bu bölümde PowerShell kullanarak her bir eşleme türü için ARP tablolarının nasıl görüntüleneği hakkında talimatlar verilmektedir. Devam etmeden önce, sizin veya bağlantı sağlayıcınızın eşlemayı yapılandırması gerekir. Her devrenin iki yolu vardır (birincil ve ikincil). Her yol için ARP tablosunu bağımsız olarak denetleyebilirsiniz.

### <a name="arp-tables-for-azure-private-peering"></a>Azure özel eşleme için ARP tabloları
Aşağıdaki cmdlet, Azure özel eşlemeiçin ARP tabloları sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Aşağıdaki yollardan biri için örnek çıktı:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure genel bakışları için ARP tabloları:
Aşağıdaki cmdlet, Azure genel ekimi için ARP tabloları sağlar:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Aşağıdaki yollardan biri için örnek çıktı:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Aşağıdaki yollardan biri için örnek çıktı:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft'un bakışları için ARP tabloları
Aşağıdaki cmdlet Microsoft'un bakışları için ARP tabloları sağlar:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Örnek çıktı yollardan biri için aşağıda gösterilmiştir:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Bu bilgilerin nasıl kullanılacağı
Bir eşlemenin ARP tablosu Katman 2 yapılandırmasını ve bağlantısını doğrulamak için kullanılabilir. Bu bölümde, ARP tablolarının farklı senaryolarda nasıl göründüğüne genel bir bakış sağlanmıştır.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Bir devre operasyonel (beklenen) durumdayken ARP tablosu
* ARP tablosunda, geçerli bir IP ve MAC adresine sahip şirket içi taraf için bir giriş ve Microsoft tarafı için de benzer bir giriş vardır.
* Şirket içi IP adresinin son sekizlisi her zaman tek bir sayıdır.
* Microsoft IP adresinin son sekizlisi her zaman çift sayıdır.
* Aynı MAC adresi, üç eşleme (birincil/ikincil) için Microsoft tarafında görünür.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Şirket içi olduğunda veya bağlantı sağlayıcı tarafında sorun olduğunda ARP tablosu
 ARP tablosunda yalnızca bir giriş görünür. MAC adresi ile Microsoft tarafında kullanılan IP adresi arasındaki eşlemi gösterir.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Böyle bir sorunla karşılaşırsanız, gidermek için bağlantı sağlayıcınızla bir destek isteği açın.
> 
> 

### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Microsoft tarafında sorunlar olduğunda ARP tablosu
* Microsoft tarafında sorunlar varsa, bir bakış için gösterilen bir ARP tablosu görmezsiniz.
* [Microsoft Azure Yardım+desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)yle bir destek isteği açın. Katman 2 bağlantısıyla ilgili bir sorununuzun olduğunu belirtin.

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute devreniz için Katman 3 yapılandırmalarını doğrulayın:
  * BGP oturumlarının durumunu belirlemek için bir rota özeti alın.
  * ExpressRoute'da hangi öneklerin reklamının verilebilen bir rota tablosu alın.
* Baytları girip çıkarak veri aktarımLarını doğrulayın.
* Sorunlar yaşamaya devam [ediyorsanız, Microsoft Azure Yardım+desteğiyle](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bir destek isteği açın.

