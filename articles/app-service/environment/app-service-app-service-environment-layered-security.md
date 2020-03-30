---
title: Katmanlı güvenlik v1
description: Uygulama Hizmeti ortamınızda katmanlı bir güvenlik mimarisini nasıl uygulayacağınızı öğrenin. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: a8920e97d315dc7bfd0ba22386b8b637afb7c05e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74688793"
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Uygulama Hizmet Ortamları ile Katmanlı Güvenlik Mimarisi Uygulama
Uygulama Hizmet Ortamları sanal ağa dağıtılmış yalıtılmış bir çalışma zamanı ortamı sağladığından, geliştiriciler her fiziksel uygulama katmanı için farklı ağ erişim düzeyleri sağlayan katmanlı bir güvenlik mimarisi oluşturabilir.

Yaygın bir istek, API arka uçlarını genel Internet erişiminden gizlemek ve yalnızca API'ların upstream web uygulamaları tarafından çağrılmasını sağlamaktır.  ApI uygulamalarına genel erişimi kısıtlamak için Uygulama Hizmet Ortamları içeren alt ağlarda [ağ güvenlik grupları (NSG'ler)][NetworkSecurityGroups] kullanılabilir.

Aşağıdaki diyagramda, Bir Uygulama Hizmet Ortamı'nda dağıtılan WebAPI tabanlı bir uygulama içeren örnek bir mimari gösterilmektedir.  Üç ayrı Uygulama Hizmeti Ortamı'nda dağıtılan üç ayrı web uygulaması örneği, aynı WebAPI uygulamasına arka uç aramaları yapar.

![Kavramsal Mimari][ConceptualArchitecture] 

Yeşil artı işaretleri, alt ağüzerinde "apiaz" içeren ağ güvenlik grubunun upstream web uygulamalarından gelen aramaları ve kendisinden gelen aramaları sağladığını gösterir.  Ancak aynı ağ güvenlik grubu, Internet'ten gelen genel trafiğe erişimi açıkça reddeder. 

Bu makalenin geri kalanı, "apiaze" içeren alt ağdaki ağ güvenlik grubunu yapılandırmak için gereken adımları gözden geçirir.

## <a name="determining-the-network-behavior"></a>Ağ Davranışını Belirleme
Hangi ağ güvenliği kurallarının gerekli olduğunu bilmek için, API uygulamasını içeren Uygulama Hizmet Ortamı'na hangi ağ istemcilerinin erişmesine izin verilip verilmeyeceğini ve hangi istemcilerin engelleneceğini belirlemeniz gerekir.

[Ağ güvenlik grupları (NSG'ler)][NetworkSecurityGroups] alt ağlara uygulandığından ve Uygulama Hizmet Ortamları alt ağlara dağıtılıolduğundan, NSG'de yer alan kurallar Bir Uygulama Hizmet Ortamında çalışan **tüm** uygulamalar için geçerlidir.  Bu makalenin örnek mimarisi kullanılarak, "apiaz" içeren alt ağa bir ağ güvenlik grubu uygulandıktan sonra, "apiase" App Service Environment'da çalışan tüm uygulamalar aynı güvenlik kuralları kümesi tarafından korunur. 

* **Yukarı akım arayanların giden IP adresini belirleyin:**  Upstream arayanların IP adresi veya adresleri nedir?  Bu adreslerin NSG'ye açıkça erişilmesine izin verilmesi gerekir.  Uygulama Hizmet Ortamları arasındaki aramalar "Internet" çağrıları olarak kabul edilir olduğundan, üç yukarı Uygulama Hizmeti Ortamının her birine atanan giden IP adresinin "apiase" alt ağı için NSG'ye erişmesine izin verilmesi gerekir.   Bir Uygulama Hizmet Ortamında çalışan uygulamaların giden IP adresini belirleme hakkında daha fazla bilgi için [Ağ Mimarisine][NetworkArchitecture] Genel Bakış makalesine bakın.
* **Arka uç API uygulamasının kendisini araması gerekecek mi?**  Bazen gözden kaçan ve ince bir nokta, arka uç uygulamasının kendisini araması gereken senaryodur.  Bir Uygulama Hizmet Ortamındaki bir arka uç API uygulamasının kendisini araması gerekiyorsa, bu çağrı "Internet" çağrısı olarak da değerlendirilir.  Örnek mimaride, bu da "apiase" App Service Environment giden IP adresinden erişim eizin gerektirir.

## <a name="setting-up-the-network-security-group"></a>Ağ Güvenlik Grubu'nu ayarlama
Giden IP adresleri kümesi bilindikten sonra, bir sonraki adım bir ağ güvenlik grubu oluşturmaktır.  Ağ güvenlik grupları hem Kaynak Yöneticisi tabanlı sanal ağlar hem de klasik sanal ağlar için oluşturulabilir.  Aşağıdaki örnekler, Powershell kullanarak klasik bir sanal ağda NSG oluşturmayı ve yapılandırmayı göstermektedir.

Örnek mimarisi için ortamlar Güney Orta ABD'de bulunur, bu nedenle bu bölgede boş bir NSG oluşturulur:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Önce, Azure yönetim altyapısı için, Uygulama Hizmeti Ortamları için [gelen trafik][InboundTraffic] le ilgili makalede belirtildiği gibi açık bir izin kuralı eklenir.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Daha sonra, ilk yukarı Uygulama Hizmet Ortamından HTTP ve HTTPS çağrılarına izin vermek için iki kural eklenir ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

İkinci ve üçüncü yukarı Uygulama Hizmet Ortamlarını ("fe2ase"ve "fe3ase") durulayın ve tekrarlayın.

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Son olarak, arka uç API'nin App Service Environment'ının giden IP adresine erişim izni vererek kendi içine geri çağrıyapabilsin.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Her NSG varsayılan olarak Internet'ten gelen erişimi engelleyen bir varsayılan kural kümesi olduğundan, başka ağ güvenliği kuralı gerekmez.

Ağ güvenlik grubundaki kuralların tam listesi aşağıda gösterilmiştir.  Vurgulanan son kuralın, açıkça erişim izni verilmiş arayanlar dışında tüm arayanlardan gelen erişimi nasıl engellediğini unutmayın.

![NSG Yapılandırması][NSGConfiguration] 

Son adım, NSG'yi "apiase" Uygulama Hizmet Ortamını içeren alt ağa uygulamaktır.

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Alt ağa uygulanan NSG ile, yalnızca üç yukarı Uygulama Hizmet Ortamı ve API arka ucunu içeren App Service Environment'ın "apiaz" ortamına çağrı yapmasına izin verilir.

## <a name="additional-links-and-information"></a>Ek Linkler ve Bilgiler
Ağ [güvenlik grupları](../../virtual-network/security-overview.md)hakkında bilgi.

[Giden IP adreslerini][NetworkArchitecture] ve Uygulama Hizmet Ortamlarını anlama.

App Service Environments tarafından kullanılan [ağ bağlantı noktaları.][InboundTraffic]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  app-service-app-service-environment-network-architecture-overview.md
[InboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
