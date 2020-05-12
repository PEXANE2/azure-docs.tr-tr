---
title: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
description: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 05/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.custom: fasttrack-edit
ms.openlocfilehash: 6a95c021153a458a4e3f804e64724b73ea1f1937
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198814"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Azure Web App için özel uç noktaları kullanma (Önizleme)

> [!Note]
> Önizleme, tüm PremiumV2 Windows ve Linux Web Apps ve elastik Premium Işlevleri için Doğu ABD ve Batı ABD 2 bölgelerinde kullanılabilir. 

Özel ağınızda bulunan istemcilerin uygulamaya özel bağlantı üzerinden güvenli bir şekilde erişmesini sağlamak için, Azure Web uygulamanız için özel uç nokta kullanabilirsiniz. Özel uç nokta, Azure VNet adres alanınızda bir IP adresi kullanır. Özel ağınızdaki bir istemci ile Web uygulaması arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel Internet 'ten etkilenme olasılığını ortadan kaldırır.

Web uygulamanız için özel uç nokta kullanımı şunları yapmanızı sağlar:

- Özel uç noktayı yapılandırarak Web uygulamanızın güvenliğini sağlayın ve genel pozlamayı ortadan kaldırın.
- VPN veya ExpressRoute özel eşlemesi kullanarak VNet 'e bağlanan şirket içi ağlardan Web uygulamasına güvenli bir şekilde bağlanın.

VNet ve Web uygulamanız arasında güvenli bir bağlantıya ihtiyacınız varsa, en basit çözüm bir hizmet uç noktasıdır. Ayrıca, bir Azure ağ geçidi, bölgesel olarak eşlenmiş VNet veya genel olarak eşlenmiş VNet aracılığıyla Web uygulamasına Şirket içinden erişmeniz gerekiyorsa, Özel uç nokta çözümdür.  

Daha fazla bilgi için bkz. [hizmet uç noktaları][serviceendpoint].

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel uç nokta, sanal ağınızdaki (VNet) bir alt ağda bulunan Azure Web uygulamanız için özel bir ağ arabirimi (NIC).
Web uygulamanız için özel bir uç nokta oluşturduğunuzda, özel ağınızdaki istemciler ve Web uygulamanız arasında güvenli bağlantı sağlar. Özel uç noktaya sanal Ağınızın IP adresi aralığından bir IP adresi atanır.
Özel uç nokta ve Web uygulaması arasındaki bağlantı güvenli bir [özel bağlantı][privatelink]kullanır. Özel uç nokta yalnızca Web uygulamanıza gelen akışlar için kullanılır. Giden akışlar bu özel uç noktayı kullanmaz, ancak giden akışları ağınıza [VNET tümleştirme özelliği][vnetintegrationfeature]aracılığıyla farklı bir alt ağda ekleyebilirsiniz.

Özel uç noktayı taktığınız alt ağ, içindeki diğer kaynaklara sahip olabilir, ayrılmış boş bir alt ağa gerek kalmaz.
Özel uç noktayı Web uygulamasından farklı bir bölgede da dağıtabilirsiniz. 

> [!Note]
>VNet tümleştirme özelliği özel uç noktadan aynı alt ağı kullanamaz, bu, VNet tümleştirme özelliğinin bir sınırlamasıdır.

Bir güvenlik perspektifinden:

- Web uygulamanıza özel uç noktaları etkinleştirdiğinizde, tüm genel erişimi devre dışı bırakabilirsiniz.
- Diğer bölgelerde VNET 'ler de dahil olmak üzere, diğer ağlardaki ve alt ağlarda bulunan birden çok özel uç noktayı etkinleştirebilirsiniz.
- Özel uç nokta NIC 'in IP adresi dinamik olmalıdır, ancak özel uç nokta silinene kadar aynı kalır.
- Özel uç noktanın NIC 'inde ilişkili bir NSG olamaz.
- Özel uç noktayı barındıran alt ağın ilişkili bir NSG 'si olabilir, ancak özel uç nokta için ağ ilkeleri zorlamayı devre dışı bırakmanız gerekir: bkz. [Özel uç noktalar için ağ Ilkelerini devre dışı bırakma][disablesecuritype]. Sonuç olarak, Özel uç noktanıza erişimi herhangi bir NSG 'ye göre filtreleyemezsiniz.
- Web uygulamanıza özel uç noktayı etkinleştirdiğinizde, Web uygulamasının [erişim kısıtlamaları][accessrestrictions] yapılandırması değerlendirilmez.
- Hedefin Internet veya Azure hizmetleri olduğu tüm NSG kurallarını kaldırarak VNet 'ten veri kaybı riskini azaltabilirsiniz. Ancak alt ağınızdaki bir Web uygulaması özel uç noktası eklemek, aynı dağıtım damgasında barındırılan ve Internet 'e açık olan herhangi bir Web uygulamasına ulaşmanıza imkan tanır.

Web uygulamanızın Web HTTP günlüklerinde istemci kaynak IP 'sini bulacaksınız. Bu, istemci IP özelliğini Web uygulamasına ileten TCP proxy protokolü kullanılarak uygulanır. Daha fazla bilgi için bkz. [TCP proxy v2 kullanarak bağlantı bilgilerini alma][tcpproxy].


  > [!div class="mx-imgBorder"]
  > ![Web uygulaması özel uç nokta genel bakış](media/private-endpoint/global-schema-web-app.png)

## <a name="dns"></a>DNS

Bu özellik önizlemede olduğundan, önizleme sırasında DNS girişini değiştirmedik. Özel DNS sunucunuzdaki DNS girişini yönetmeniz veya özel bölge Azure DNS kendiniz yapmanız gerekir.
Özel bir DNS adı kullanmanız gerekiyorsa, Web uygulamanıza özel adı eklemeniz gerekir. Önizleme sırasında, özel ad genel DNS çözümlemesi kullanılarak herhangi bir özel ad gibi doğrulanması gerekir. Daha fazla bilgi için bkz. [özel DNS doğrulaması][dnsvalidation].

Kudu konsolunu veya kudu REST API (örneğin, Azure DevOps şirket içinde barındırılan aracılarla dağıtım) kullanmanız gerekiyorsa, Azure DNS özel bölgeniz veya özel DNS sunucunuzda iki kayıt oluşturmanız gerekir. 
- PrivateEndpointIP yourwebappname.azurewebsites.net 
- PrivateEndpointIP yourwebappname.scm.azurewebsites.net 

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması][pricing].

## <a name="limitations"></a>Sınırlamalar

Azure Işlevi 'ni özel uç nokta ile elastik Premium planda kullandığınızda, işlevi Azure Web portalında çalıştırmak veya yürütmek için doğrudan ağ erişiminizin olması gerekir veya bir HTTP 403 hatası alırsınız. Diğer bir deyişle, tarayıcınız Azure Web portalından işlevi yürütmek için özel uç noktaya ulaşabilmelidir. 

Önizleme sırasında yalnızca üretim yuvası özel uç noktasının arkasında kullanıma sunulduğunu, diğer yuvaların yalnızca ortak uç nokta tarafından erişilebilir olması gerekir.

Özel bağlantı özelliğini ve özel uç noktayı düzenli olarak geliştirdik, sınırlamalar hakkında güncel bilgiler için [Bu makaleye][pllimitations] bakın.

## <a name="next-steps"></a>Sonraki adımlar

Portal aracılığıyla Web uygulamanız için özel uç nokta dağıtmak için bkz. [Web uygulamasına özel olarak bağlanma][howtoguide]




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
[howtoguide]: https://docs.microsoft.com/azure/private-link/create-private-endpoint-webapp-portal
