---
title: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
description: Azure özel uç noktasını kullanarak bir Web uygulamasına özel olarak bağlanma
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: ad7e04d611129766fe9fb72285fe35ccfbb17626
ms.sourcegitcommit: 05a650752e9346b9836fe3ba275181369bd94cf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79136681"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Azure Web App için özel uç noktaları kullanma (Önizleme)

Özel ağınızda bulunan istemcilerin özel bağlantı üzerinden uygulamaya güvenli bir şekilde erişmesini sağlamak için Azure Web uygulamanız için özel uç nokta kullanabilirsiniz. Özel uç nokta, Azure VNet adres alanınızda bir IP adresi kullanır. Özel ağınızdaki ve Web uygulamasındaki istemci arasındaki ağ trafiği, VNET üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel Internet 'ten etkilenme olasılığını ortadan kaldırır.

Web uygulamanız için özel uç nokta kullanımı şunları yapmanızı sağlar:

- Hizmet uç noktasını yapılandırarak Web uygulamanızın güvenliğini sağlayın, genel pozlamayı ortadan kaldırın
- VPN veya ExpressRoute özel eşlemesi kullanarak VNET 'e bağlanan şirket içi ağlardan Web uygulamasına güvenli bir şekilde bağlanın.

VNET ve Web uygulamanız arasında güvenli bir bağlantı gerekiyorsa, hizmet uç noktası en basit çözümdür. Ayrıca, bir Azure ağ geçidi aracılığıyla Şirket içinden Web uygulamasına erişmeniz gerekiyorsa, bölgesel olarak eşlenmiş VNET veya genel eşlenmiş VNet, Özel uç nokta çözümdür.  

[Hizmet uç noktası][serviceendpoint] hakkında daha fazla bilgi için

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel uç nokta, sanal ağınızdaki (VNet) alt ağınızdaki Azure Web uygulamanız için özel bir ağ arabirimi (NIC).
Web uygulamanız için özel bir uç nokta oluşturduğunuzda, özel ağınızdaki istemciler ve Web uygulamanız arasında güvenli bir bağlantı sağlar. Özel uç noktaya sanal Ağınızın IP adresi aralığından bir IP adresi atanır.
Özel uç nokta ve Web uygulaması arasındaki bağlantı güvenli bir [özel bağlantı][privatelink]kullanır. Özel uç nokta yalnızca Web uygulamanıza gelen akışlar için kullanılır. Giden akışlar bu özel uç noktayı kullanmaz, ancak giden akışları ağınıza [VNET tümleştirme özelliği][vnetintegrationfeature]aracılığıyla farklı bir alt ağda ekleyebilirsiniz.

Özel uç noktayı taktığınız alt ağ, içindeki diğer kaynaklara sahip olabilir, ayrılmış boş bir alt ağa gerek kalmaz.
Özel uç noktayı Web uygulamasından farklı bir bölgede dağıtabilirsiniz. 

> [!Note]
>VNET tümleştirme özelliği özel uç noktadan aynı alt ağı kullanamaz, bu, VNET tümleştirme özelliğinin bir kısıtlamasıdır

Güvenlik perspektifinden:

- Web uygulamanıza hizmet uç noktası etkinleştirdiğinizde, tüm genel erişimi devre dışı bırakabilirsiniz
- Diğer bölgelerdeki VNET 'ler dahil, diğer ağlardaki ve alt ağlarda bulunan birden çok özel bitiş noktasını etkinleştirebilirsiniz
- Özel uç nokta NIC 'in IP adresi dinamik olmalıdır, ancak özel uç noktası silinene kadar aynı kalır
- Özel uç noktanın NIC 'inde ilişkili bir NSG olamaz
- Özel uç noktayı barındıran alt ağın ilişkili bir NSG 'si olabilir, ancak özel uç nokta için ağ ilkeleri zorlamayı devre dışı bırakmanız gerekir [Bu makaleye][disablesecuritype]bakın. Sonuç olarak, Özel uç noktanıza erişimi herhangi bir NSG 'ye göre filtreleyemezsiniz
- Web uygulamanıza özel uç noktayı etkinleştirdiğinizde, Web uygulamasının [erişim kısıtlamaları][accessrestrictions] yapılandırması değerlendirilmez.
- Hedefin Internet veya Azure hizmetleri olduğu tüm NSG kurallarını kaldırarak VNET 'ten veri kaybı riskini azaltabilirsiniz. Ancak alt ağınızdaki bir Web App Service uç noktası eklemek, aynı damgada barındırılan ve Internet 'e açık olan herhangi bir Web uygulamasına ulaşmanıza imkan tanır.

Web uygulaması için özel uç nokta katman PremiumV2 için kullanılabilir ve bir dış Ao ile yalıtılmıştır.

Web uygulamanızın Web http günlüklerinde istemci kaynak IP 'sini bulacaksınız. TCP proxy protokolünü uyguladık, Web uygulamasına istemci IP özelliğini ilettik. Daha fazla bilgi için [bu makaleye][tcpproxy] bakın.

![Genel Bakış][1]


## <a name="dns"></a>DNS

Bu özellik önizlemede olduğundan, önizleme sırasında DNS girişini değiştirmedik. Özel DNS sunucunuzdaki veya Azure DNS özel bölgesindeki DNS girişini kendiniz yönetmeniz gerekir. Özel bir DNS adı kullanmanız gerekiyorsa, Web uygulamanıza özel adı eklemeniz gerekir. Önizleme sırasında, özel ad genel DNS çözümlemesi kullanılarak herhangi bir özel ad gibi doğrulanması gerekir. [özel DNS doğrulaması teknik başvurusu][dnsvalidation]

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması][pricing].

## <a name="limitations"></a>Sınırlamalar

Özel bağlantı özelliğini ve özel uç noktayı düzenli olarak geliştirdik, sınırlamalar hakkında güncel bilgiler için [Bu makaleye][pllimitations] bakın.

## <a name="next-steps"></a>Sonraki adımlar

Portal aracılığıyla Web uygulamanız için özel uç nokta dağıtmak için bkz. [Web uygulamasına özel olarak bağlanma][howtoguide]


<!--Image references-->
[1]: ./media/private-endpoint/schemaglobaloverview.png

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
