---
title: Özel uç nokta kullanarak bir Azure Web uygulamasına özel olarak bağlanma
description: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 07/07/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.custom: fasttrack-edit, references_regions
ms.openlocfilehash: fdad2f7c2ce4f82529866b4235ebebab8da664d3
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86054585"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Azure Web App için özel uç noktaları kullanma (Önizleme)

> [!Note]
> Önizleme yenilemesinde, veri Sızdırma Koruması özelliğini yayımladık.
>
> Önizleme, PremiumV2 Windows ve Linux Web Apps ve elastik Premium Işlevleri için tüm genel bölgelerde kullanılabilir. 

Özel ağınızda bulunan istemcilerin uygulamaya özel bağlantı üzerinden güvenli bir şekilde erişmesini sağlamak için, Azure Web uygulamanız için özel uç nokta kullanabilirsiniz. Özel uç nokta, Azure VNet adres alanınızda bir IP adresi kullanır. Özel ağınızdaki bir istemci ile Web uygulaması arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel Internet 'ten etkilenme olasılığını ortadan kaldırır.

Web uygulamanız için özel uç nokta kullanımı şunları yapmanızı sağlar:

- Özel uç noktayı yapılandırarak Web uygulamanızın güvenliğini sağlayın ve genel pozlamayı ortadan kaldırın.
- VPN veya ExpressRoute özel eşlemesi kullanarak VNet 'e bağlanan şirket içi ağlardan Web uygulamasına güvenli bir şekilde bağlanın.
- VNet 'inizden herhangi bir veri ayıklanmasını önleyin. 

VNet ve Web uygulamanız arasında güvenli bir bağlantıya ihtiyacınız varsa, en basit çözüm bir hizmet uç noktasıdır. Ayrıca, bir Azure ağ geçidi, bölgesel olarak eşlenmiş VNet veya genel olarak eşlenmiş VNet aracılığıyla Web uygulamasına Şirket içinden erişmeniz gerekiyorsa, Özel uç nokta çözümdür.  

Daha fazla bilgi için bkz. [hizmet uç noktaları][serviceendpoint].

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel uç nokta, sanal ağınızdaki (VNet) bir alt ağda bulunan Azure Web uygulamanız için özel bir ağ arabirimi (NIC).
Web uygulamanız için özel bir uç nokta oluşturduğunuzda, özel ağınızdaki istemciler ve Web uygulamanız arasında güvenli bağlantı sağlar. Özel uç noktaya sanal Ağınızın IP adresi aralığından bir IP adresi atanır.
Özel uç nokta ve Web uygulaması arasındaki bağlantı güvenli bir [özel bağlantı][privatelink]kullanır. Özel uç nokta yalnızca Web uygulamanıza gelen akışlar için kullanılır. Giden akışlar bu özel uç noktayı kullanmaz, ancak giden akışları ağınıza [VNET tümleştirme özelliği][vnetintegrationfeature]aracılığıyla farklı bir alt ağda ekleyebilirsiniz.

Özel uç noktayı taktığınız alt ağ, içindeki diğer kaynaklara sahip olabilir, ayrılmış boş bir alt ağa gerek kalmaz.
Özel uç noktayı Web uygulamasından farklı bir bölgede da dağıtabilirsiniz. 

> [!Note]
>VNet tümleştirme özelliği özel uç nokta ile aynı alt ağı kullanamaz, bu, VNet tümleştirme özelliğinin bir sınırlamasıdır.

Bir güvenlik perspektifinden:

- Web uygulamanıza özel uç noktaları etkinleştirdiğinizde, tüm genel erişimi devre dışı bırakabilirsiniz.
- Diğer bölgelerde VNET 'ler de dahil olmak üzere, diğer ağlardaki ve alt ağlarda bulunan birden çok özel uç noktayı etkinleştirebilirsiniz.
- Özel uç nokta NIC 'in IP adresi dinamik olmalıdır, ancak özel uç nokta silinene kadar aynı kalır.
- Özel uç noktanın NIC 'inde ilişkili bir NSG olamaz.
- Özel uç noktayı barındıran alt ağın ilişkili bir NSG 'si olabilir, ancak özel uç nokta için ağ ilkeleri zorlamayı devre dışı bırakmanız gerekir: bkz. [Özel uç noktalar için ağ Ilkelerini devre dışı bırakma][disablesecuritype]. Sonuç olarak, Özel uç noktanıza erişimi herhangi bir NSG 'ye göre filtreleyemezsiniz.
- Web uygulamanıza özel uç noktayı etkinleştirdiğinizde, Web uygulamasının [erişim kısıtlamaları][accessrestrictions] yapılandırması değerlendirilmez.
- Hedefin Internet veya Azure hizmetleri olduğu tüm NSG kurallarını kaldırarak VNet 'ten veri kaybı riskini ortadan kaldırabilirsiniz. Bir Web uygulaması için özel bir uç nokta dağıttığınızda, bu belirli Web uygulamasına yalnızca özel uç nokta üzerinden ulaşabilirsiniz. Başka bir Web uygulamanız varsa, bu Web uygulaması için başka bir özel uç nokta dağıtmanız gerekir.

Web uygulamanızın Web HTTP günlüklerinde istemci kaynak IP 'sini bulacaksınız. Bu özellik, istemci IP özelliğini Web uygulamasına ileten TCP proxy protokolü kullanılarak uygulanır. Daha fazla bilgi için bkz. [TCP proxy v2 kullanarak bağlantı bilgilerini alma][tcpproxy].


  > [!div class="mx-imgBorder"]
  > ![Web uygulaması özel uç nokta genel bakış](media/private-endpoint/global-schema-web-app.png)

## <a name="dns"></a>DNS

Web uygulaması için özel uç noktası kullandığınızda, istenen URL, Web uygulamanızın adı ile aynı olmalıdır. Varsayılan olarak mywebappname.azurewebsites.net.

Varsayılan olarak, Özel uç nokta olmadan Web uygulamanızın genel adı kümeye kurallı bir addır.
Örneğin, ad çözümlemesi şu şekilde olacaktır:

|Name |Tür |Değer |
|-----|-----|------|
|mywebapp.azurewebsites.net|CNAME|clustername.azurewebsites.windows.net|
|clustername.azurewebsites.windows.net|CNAME|cloudservicename.cloudapp.net|
|cloudservicename.cloudapp.net|A|40.122.110.154| 


Özel bir uç nokta dağıttığınızda, DNS girişini mywebapp.privatelink.azurewebsites.net kurallı adını gösterecek şekilde güncelleştiririz.
Örneğin, ad çözümlemesi şu şekilde olacaktır:

|Name |Tür |Değer |Görüyorum |
|-----|-----|------|-------|
|mywebapp.azurewebsites.net|CNAME|mywebapp.privatelink.azurewebsites.net|
|mywebapp.privatelink.azurewebsites.net|CNAME|clustername.azurewebsites.windows.net|
|clustername.azurewebsites.windows.net|CNAME|cloudservicename.cloudapp.net|
|cloudservicename.cloudapp.net|A|40.122.110.154|<--bu genel IP özel uç noktanız değil, 403 hatası alırsınız|

Özel bir DNS sunucusu veya Azure DNS özel bölgesi oluşturmanız gerekir, testler için test makinenizin ana bilgisayar girdisini değiştirebilirsiniz.
Oluşturmanız gereken DNS bölgesi: **Privatelink.azurewebsites.net**. Web uygulamanızın kaydını bir kayıt ve özel uç nokta IP 'si ile kaydedin.
Örneğin, ad çözümlemesi şu şekilde olacaktır:

|Name |Tür |Değer |Görüyorum |
|-----|-----|------|-------|
|mywebapp.azurewebsites.net|CNAME|mywebapp.privatelink.azurewebsites.net|
|mywebapp.privatelink.azurewebsites.net|A|10.10.10.8|<--bu girişi, Özel uç nokta IP adresinizi işaret etmek üzere DNS sisteminizde yönetirsiniz|

Bu DNS yapılandırmasından sonra, Web uygulamanıza mywebappname.azurewebsites.net varsayılan adıyla özel olarak ulaşabilirsiniz.


Özel bir DNS adı kullanmanız gerekiyorsa, Web uygulamanıza özel adı eklemeniz gerekir. Önizleme sırasında, özel ad genel DNS çözümlemesi kullanılarak herhangi bir özel ad gibi doğrulanması gerekir. Daha fazla bilgi için bkz. [özel DNS doğrulaması][dnsvalidation].

Kudu konsolu veya kudu REST API (örneğin, Azure DevOps kendiliğinden konak aracılarıyla dağıtım) için, Azure DNS özel bölgeniz veya özel DNS sunucunuzda iki kayıt oluşturmanız gerekir. 

| Name | Tür | Değer |
|-----|-----|-----|
| mywebapp.privatelink.azurewebsites.net | A | PrivateEndpointIP | 
| mywebapp.scm.privatelink.azurewebsites.net | A | PrivateEndpointIP | 



## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması][pricing].

## <a name="limitations"></a>Sınırlamalar

Azure Işlevi 'ni özel uç nokta ile elastik Premium planda kullandığınızda, işlevi Azure Web portalında çalıştırmak veya yürütmek için doğrudan ağ erişiminizin olması gerekir veya bir HTTP 403 hatası alırsınız. Diğer bir deyişle, tarayıcınızın Azure Web portalından işlevi yürütmek için özel uç noktaya erişebilmesi gerekir. 

Önizleme sırasında, Özel uç noktanın arkasında yalnızca üretim yuvası sunulur, diğer yuvaların ortak uç nokta ile ulaşması gerekir.

Özel bağlantı özelliğini ve özel uç noktayı düzenli olarak geliştirdik, sınırlamalar hakkında güncel bilgiler için [Bu makaleye][pllimitations] bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Portal aracılığıyla Web uygulamanız için özel uç nokta dağıtmak üzere, bkz [. Portal Ile Web uygulamasına özel olarak bağlanma][howtoguide1]
- Azure CLı kullanarak Web uygulamanıza özel uç nokta dağıtmak için bkz. [Azure CLI Ile Web uygulamasına özel olarak bağlanma][howtoguide2]
- PowerShell kullanarak Web uygulamanız için özel uç nokta dağıtmak üzere, bkz. [PowerShell Ile Web uygulamasına özel olarak bağlanma][howtoguide3]



<!--Links-->
[serviceendpoint]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
[privatelink]: https://docs.microsoft.com/azure/private-link/private-link-overview
[vnetintegrationfeature]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[disablesecuritype]: https://docs.microsoft.com/azure/private-link/disable-private-endpoint-network-policy
[accessrestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[tcpproxy]: ../../private-link/private-link-service-overview.md#getting-connection-information-using-tcp-proxy-v2
[dnsvalidation]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain
[pllimitations]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview#limitations
[pricing]: https://azure.microsoft.com/pricing/details/private-link/
[howtoguide1]: https://docs.microsoft.com/azure/private-link/create-private-endpoint-webapp-portal
[howtoguide2]: https://docs.microsoft.com/azure/app-service/scripts/cli-deploy-privateendpoint
[howtoguide3]: https://docs.microsoft.com/azure/app-service/scripts/powershell-deploy-private-endpoint
