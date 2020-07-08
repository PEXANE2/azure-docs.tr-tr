---
title: Arka uç v1 'ye bağlanma
description: App Service Ortamı arka uç kaynaklarına güvenli bir şekilde bağlanma hakkında bilgi edinin. Bu belge yalnızca eski v1 Ao kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 68667908d25813b61b6a725fddce9ab438a248d8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85833129"
---
# <a name="connect-securely-to-back-end-resources-from-an-app-service-environment"></a>App Service ortamından arka uç kaynaklarına güvenli bir şekilde bağlanma
Bir App Service Ortamı her **zaman bir Azure Resource Manager** sanal ağda **veya** klasik dağıtım modeli [sanal ağında][virtualnetwork]oluşturulduğundan, bir App Service ortamı giden bağlantılar sanal ağ üzerinden özel olarak akabilir. Haziran 2016 itibariyle, ASE 'ler, ortak adres aralıkları veya RFC1918 adres alanları (özel adresler) kullanan sanal ağlara da dağıtılabilir.  

Örneğin, 1433 numaralı bağlantı noktasına sahip bir sanal makine kümesinde çalışan bir SQL Server olabilir.  Uç nokta, yalnızca aynı sanal ağdaki diğer kaynaklardan erişime izin vermek için ACLd olabilir.  

Diğer bir örnek olarak, hassas uç noktalar şirket içinde çalışabilir ve Azure 'a [siteden siteye][SiteToSite] veya [Azure ExpressRoute][ExpressRoute] bağlantıları aracılığıyla bağlı olabilir.  Sonuç olarak, yalnızca siteden siteye veya ExpressRoute tünellerini bağlı sanal ağlardaki kaynaklar şirket içi uç noktalara erişebilir.

Bu senaryoların tümünde, bir App Service Ortamı çalışan uygulamalar çeşitli sunuculara ve kaynaklara güvenli bir şekilde bağlanabilir. Uygulamalardan giden trafik aynı sanal ağdaki özel uç noktalara App Service Ortamı çalışıyorsa veya aynı sanal ağa bağlı ise, yalnızca sanal ağ üzerinden akar.  Özel uç noktalara giden trafik, genel Internet üzerinden akamaz.

Bir sorun, bir App Service Ortamı bir sanal ağ içindeki uç noktalara giden trafik için geçerlidir. App Service ortamlar, App Service Ortamı **aynı** alt ağda bulunan sanal makinelerin uç noktalarına ulaşamıyor. App Service ortamları, yalnızca App Service Ortamı tarafından kullanılmak üzere ayrılmış bir alt ağa dağıtılmışsa, bu sınırlama normalde bir sorun olması gerekmez.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden Bağlantı ve DNS Gereksinimleri
App Service Ortamı düzgün bir şekilde çalışması için, çeşitli uç noktalara giden erişim gerekir. AX tarafından kullanılan dış uç noktaların tam listesi [ExpressRoute Için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesinin "gerekli ağ bağlantısı" bölümünde bulunur.

App Service ortamları, sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Bir App Service Ortamı oluşturulduktan sonra DNS yapılandırması değiştirilirse, geliştiriciler bir App Service Ortamı yeni DNS yapılandırmasını çekmeye zorlayabilir. Portalda App Service Ortamı Management dikey penceresinin üst kısmında, **Yeniden Başlat** simgesini seçerek bir çalışan ortamın yeniden başlatılmasını tetikleyin, bu da ORTAMıN yeni DNS yapılandırmasını almasına neden olur.

Ayrıca, VNET üzerindeki özel DNS sunucularının bir App Service Ortamı oluşturmadan önce zaman ayarlaması önerilir.  Bir App Service Ortamı oluşturma sırasında bir sanal ağın DNS yapılandırması değiştirilirse, bu, App Service Ortamı oluşturma işleminin başarısız olmasına neden olur. VPN ağ geçidinin diğer ucunda, erişilemeyen veya kullanılamayan özel bir DNS sunucusu varsa App Service Ortamı oluşturma işlemi de başarısız olur.

## <a name="connecting-to-a-sql-server"></a>SQL Server bağlanma
Ortak bir SQL Server yapılandırmasında 1433 numaralı bağlantı noktasında dinleme yapan bir uç nokta vardır:

![SQL Server uç noktası][SqlServerEndpoint]

Bu uç noktayla trafiği kısıtlamak için iki yaklaşım vardır:

* [Ağ Access Control listeleri][NetworkAccessControlLists] (ağ ACL 'leri)
* [Ağ güvenlik grupları][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Ağ ACL 'SI Ile erişimi kısıtlama
Bağlantı noktası 1433, bir ağ erişim denetimi listesi kullanılarak güvenli hale getirilir.  Aşağıdaki örnek, istemci adreslerinin bir sanal ağ içinden kaynaklandığı atama izinlerine ekler ve diğer tüm istemcilere erişimi engeller.

![Ağ Access Control listesi örneği][NetworkAccessControlListExample]

SQL Server ile aynı sanal ağda App Service Ortamı çalıştıran tüm uygulamalar SQL Server örneğine bağlanabilir. SQL Server sanal makinesi için **VNET iç** IP adresini kullanın.  

Aşağıdaki örnek bağlantı dizesi, kendi özel IP adresini kullanarak SQL Server başvurur.

`Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient`

Sanal makinede ortak bir uç nokta de olsa da, ağ ACL 'si nedeniyle genel IP adresini kullanmaya yönelik bağlantı girişimleri reddedilir. 

## <a name="restricting-access-with-a-network-security-group"></a>Ağ güvenlik grubuyla erişimi kısıtlama
Erişimin güvenliğini sağlamaya yönelik alternatif bir yaklaşım da ağ güvenlik grubudur.  Ağ güvenlik grupları ayrı sanal makinelere veya sanal makineler içeren bir alt ağa uygulanabilir.

İlk olarak, bir ağ güvenlik grubu oluşturmanız gerekir:

```azurepowershell-interactive
New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"
```

Ağ güvenlik grubuyla yalnızca VNet iç trafiğine erişimi kısıtlamak basittir.  Bir ağ güvenlik grubundaki varsayılan kurallar yalnızca aynı sanal ağdaki diğer ağ istemcilerinden erişime izin verir.

Sonuç olarak, SQL Server erişimini kilitlemek basittir. SQL Server çalıştıran sanal makinelere ya da sanal makineleri içeren alt ağa varsayılan kuralları ile bir ağ güvenlik grubu uygulamanız yeterlidir.

Aşağıdaki örnek, kapsayan alt ağa bir ağ güvenlik grubu uygular:

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
```

Nihai sonuç, dış erişimi engelleyen bir güvenlik kuralları kümesidir ve VNet iç erişimine izin verir:

![Varsayılan ağ güvenlik kuralları][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Başlarken
App Service ortamlarıyla çalışmaya başlamak için bkz. [giriş App Service ortamı][IntroToAppServiceEnvironment]

App Service Ortamı gelen trafiği denetleme hakkında daha fazla bilgi için bkz. [bir App Service ortamı gelen trafiği denetleme][ControlInboundASE]

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
