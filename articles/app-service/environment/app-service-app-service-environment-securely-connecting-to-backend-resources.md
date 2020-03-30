---
title: Arka uca bağlan v1
description: Bir Uygulama Hizmet Ortamından arka uç kaynaklarına güvenli bir şekilde nasıl bağlanabilirsiniz hakkında bilgi edinin. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 03f773e286697a12188f238cf2f422a18a20054f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687304"
---
# <a name="connect-securely-to-back-end-resources-from-an-app-service-environment"></a>Bir Uygulama Hizmeti ortamından arka uç kaynaklara güvenli bir şekilde bağlanma
Bir Uygulama Hizmet Ortamı her zaman **bir** Azure Kaynak Yöneticisi sanal ağında **veya** klasik dağıtım modeli [sanal ağında][virtualnetwork]oluşturulduğundan, Bir Uygulama Hizmet Ortamından diğer arka uç kaynaklarına giden bağlantılar yalnızca sanal ağ üzerinden akabilir.  Haziran 2016'da yapılan yeni bir değişiklikle, AS'ler genel adres aralıklarını veya RFC1918 adres alanlarını (örn. özel adresler) kullanan sanal ağlara da dağıtılabilir.  

Örneğin, bağlantı noktası 1433 kilitli sanal makineler kümesi üzerinde çalışan bir SQL Sunucusu olabilir.  Bitiş noktası, yalnızca aynı sanal ağdaki diğer kaynaklardan erişime izin vermek için ACLd olabilir.  

Başka bir örnek olarak, hassas uç noktalar şirket içinde çalışabilir ve [Site'den Siteye][SiteToSite] veya [Azure ExpressRoute][ExpressRoute] bağlantıları üzerinden Azure'a bağlanabilir.  Sonuç olarak, yalnızca Siteden Siteye veya ExpressRoute tünellerine bağlı sanal ağlardaki kaynaklar şirket içi uç noktalara erişebilir.

Tüm bu senaryolar için, Bir Uygulama Hizmeti Ortamında çalışan uygulamalar çeşitli sunuculara ve kaynaklara güvenli bir şekilde bağlanabilecektir.  Bir Uygulama Hizmet Ortamında çalışan uygulamalardan aynı sanal ağdaki (veya aynı sanal ağa bağlı) özel uç noktalarına giden trafik yalnızca sanal ağ üzerinden akar.  Özel uç noktalara giden trafik genel Internet üzerinden akmaz.

Bir uyarı, sanal ağ daki uç noktalara Bir Uygulama Hizmet Ortamı'ndan giden trafik için geçerlidir.  Uygulama Hizmet Ortamları, Uygulama Hizmet Ortamı ile **aynı** alt ağda bulunan sanal makinelerin uç noktalarına ulaşamaz.  Uygulama Hizmet Ortamları yalnızca App Service Environment tarafından özel kullanım için ayrılmış bir alt ağa dağıtılmaya başlanmış olduğu sürece, bu normalde sorun olmamalıdır.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden Bağlantı ve DNS Gereksinimleri
Bir Uygulama Hizmet Ortamının düzgün çalışması için, çeşitli uç noktalara giden erişim gerektirir. Bir ASE tarafından kullanılan harici uç noktaların tam [listesi, ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi için Ağ Yapılandırması'nın "Gerekli Ağ Bağlantısı" bölümünde yer almaktadır.

Uygulama Hizmet Ortamları, sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Herhangi bir nedenle, Bir Uygulama Hizmet Ortamı oluşturulduktan sonra DNS yapılandırması değiştirilirse, geliştiriciler yeni DNS yapılandırmasını almaya bir Uygulama Hizmet Ortamı zorlayabilir.  Portaldaki App Service Environment management blade'in üst kısmında bulunan "Yeniden Başlat" simgesini kullanarak yuvarlanma ortamını yeniden başlatmak, ortamın yeni DNS yapılandırmasını almasını sağlar.

Ayrıca, vnet'teki özel DNS sunucularının Bir Uygulama Hizmet Ortamı oluşturmadan önce kuruluma önceden kurulması önerilir.  Bir Uygulama Hizmet Ortamı oluşturulurken sanal ağın DNS yapılandırması değiştirilirse, bu Uygulama Hizmeti Ortamı oluşturma işleminin başarısız olmasına neden olur.  Benzer bir şekilde, VPN ağ geçidinin diğer ucunda özel bir DNS sunucusu varsa ve DNS sunucusuna erişilemez veya kullanılamıyorsa, Uygulama Hizmeti Ortamı oluşturma işlemi de başarısız olur.

## <a name="connecting-to-a-sql-server"></a>SQL Server'a bağlanma
Ortak bir SQL Server yapılandırması port 1433 üzerinde bir bitiş noktası dinleme vardır:

![SQL Server Bitiş Noktası][SqlServerEndpoint]

Trafiği bu bitiş noktasıyla sınırlamak için iki yaklaşım vardır:

* [Ağ Erişim Denetim Listeleri][NetworkAccessControlLists] (Ağ AÇ'ları)
* [Ağ Güvenlik Grupları][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Ağ ACL ile Erişimi Kısıtlama
Bağlantı noktası 1433 bir ağ erişim denetim listesi kullanılarak güvenli hale alınabilir.  Aşağıdaki örnek, istemci adreslerini sanal bir ağın içinden gelen beyaz listeler ve diğer tüm istemcilere erişimi engeller.

![Ağ Erişim Denetim Listesi Örneği][NetworkAccessControlListExample]

App Service Environment'da SQL Server ile aynı sanal ağda çalışan tüm uygulamalar, SQL Server sanal makinesinin **VNet dahili** IP adresini kullanarak SQL Server örneğine bağlanabilecektir.  

Aşağıdaki örnek bağlantı dizesi, özel IP adresini kullanarak SQL Server'a başvurur.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Sanal makinenin de ortak bir bitiş noktası olmasına rağmen, ortak IP adresini kullanan bağlantı girişimleri ağ ACL nedeniyle reddedilir. 

## <a name="restricting-access-with-a-network-security-group"></a>Ağ Güvenlik Grubuyla Erişimi Kısıtlama
Erişimi güvence altına almak için alternatif bir yaklaşım bir ağ güvenlik grubu ile.  Ağ güvenlik grupları tek tek sanal makinelere veya sanal makineler içeren bir alt ağa uygulanabilir.

Önce bir ağ güvenlik grubu oluşturulması gerekir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Bir ağ güvenlik grubuyla yalnızca VNet iç trafiğine erişimi kısıtlamak çok basittir.  Ağ güvenlik grubundaki varsayılan kurallar yalnızca aynı sanal ağdaki diğer ağ istemcilerinden erişime izin verir.

Sonuç olarak, SQL Server'a erişimi kilitlemek, varsayılan kuralları olan bir ağ güvenlik grubunu SQL Server çalıştıran sanal makinelere veya sanal makineleri içeren alt ağlara uygulamak kadar basittir.

Aşağıdaki örnek, bir ağ güvenlik grubunu içeren alt ağa uygular:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Sonuç, VNet dahili erişimine izin verirken dış erişimi engelleyen bir dizi güvenlik kuralıdır:

![Varsayılan Ağ Güvenlik Kuralları][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamına Giriş][IntroToAppServiceEnvironment]

Uygulama Servis Ortamınıza gelen trafiği denetleme yle ilgili ayrıntılar için, [Uygulama Servis Ortamına gelen trafiği denetleme][ControlInboundASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
