---
title: Azure Özel Bitiş Noktası'nı kullanarak bir Web Uygulamasına özel bağlanma
description: Azure Özel Bitiş Noktası'nı kullanarak bir Web Uygulamasına özel bağlanma
author: ericgre
ms.assetid: 2dceac28-1ba6-4904-a15d-9e91d5ee162c
ms.topic: article
ms.date: 03/18/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.custom: fasttrack-edit
ms.openlocfilehash: c2717b1f29af39c6fdc4602b11acba131d959f03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534397"
---
# <a name="using-private-endpoints-for-azure-web-app-preview"></a>Azure Web Uygulaması için Özel Uç Noktaları Kullanma (Önizleme)

> [!Note]
> Önizleme, tüm PremiumV2 Windows ve Linux Web Uygulamaları ve Elastik Premium Fonksiyonlar için Doğu ABD ve Batı ABD 2 bölgelerinde mevcuttur. 

Azure Web Uygulamanız için Özel Bitiş Noktası'nı kullanarak özel ağınızda bulunan istemcilerin özel bağlantı üzerinden uygulamaya güvenli bir şekilde erişmesine izin verebilirsiniz. Private Endpoint, Azure VNet adres alanınızdan bir IP adresi kullanır. Özel ağınızdaki bir istemci ile Web Uygulaması arasındaki ağ trafiği, Microsoft omurga ağındaki VNet ve Özel Bağlantı üzerinden geçerek genel Internet'ten açığa yayını ortadan kaldırır.

Web Uygulamanız için Özel Bitiş Noktası'nı kullanmak şunları yapmanızı sağlar:

- Hizmet Bitiş Noktası'nı yapılandırarak ve herkese açık açığa çıkanları ortadan kaldırarak Web Uygulamanızı güvence altına alanın.
- VPN veya ExpressRoute özel eşleme kullanarak VNet'e bağlanan şirket içi ağlardan Web App'e güvenli bir şekilde bağlanın.

VNet'inizle Web Uygulamanız arasında güvenli bir bağlantıya ihtiyacınız varsa, Hizmet Bitiş Noktası en basit çözümdür. Web uygulamasına şirket içinde azure ağ geçidi, bölgesel olarak bakıldığında bir VNet veya küresel olarak bakıldığında bir VNet aracılığıyla da ulaşmanız gerekiyorsa, çözüm Private Endpoint'tir.  

Daha fazla bilgi için [Hizmet Bitiş Noktaları'na][serviceendpoint]bakın.

## <a name="conceptual-overview"></a>Kavramsal genel bakış

Özel Bitiş Noktası, Sanal Ağınızdaki (VNet) bir Alt ağdaki Azure Web Uygulamanız için özel bir ağ arabirimidir (NIC).
Web Uygulamanız için bir Özel Bitiş Noktası oluşturduğunuzda, özel ağınızdaki istemciler ile Web Uygulamanız arasında güvenli bağlantı sağlar. Özel Bitiş Noktası'na VNet'inizin IP adresi aralığından bir IP Adresi atanır.
Özel Bitiş Noktası ve Web Uygulaması arasındaki bağlantı güvenli bir [Özel Bağlantı][privatelink]kullanır. Özel Bitiş Noktası yalnızca Web Uygulamanıza gelen akışlar için kullanılır. Giden akışlar bu Özel Bitiş Noktasını kullanmaz, ancak [VNet tümleştirme özelliği][vnetintegrationfeature]aracılığıyla giden akışları farklı bir alt ağda ağınıza enjekte edebilirsiniz.

Özel Bitiş Noktası'nı taktığınız Alt ağ içinde başka kaynaklar olabilir, özel boş bir Alt ağa ihtiyacınız yoktur.
Özel Bitiş Noktası'nı Web Uygulamasından farklı bir bölgede de dağıtabilirsiniz. 

> [!Note]
>VNet tümleştirme özelliği Özel Bitiş Noktası ile aynı alt ağı kullanamaz, bu VNet tümleştirme özelliğinin bir sınırlamasıdır.

Güvenlik açısından:

- Web Uygulamanıza Özel Uç Noktaları etkinleştirdiğinizde, tüm herkese açık erişimi devre dışı kılmış olursunuz.
- Diğer bölgelerdeki VNets'ler de dahil olmak üzere diğer VNet'lerde ve Alt Ağlar'da birden çok Özel Uç Noktası etkinleştirebilirsiniz.
- Private Endpoint NIC'nin IP adresi dinamik olmalıdır, ancak Siz Özel Bitiş Noktası'nı silene kadar aynı kalır.
- Özel Bitiş Noktası'nın NIC'si ilişkili bir NSG'ye sahip olamaz.
- Özel Bitiş Noktası'nı barındıran Alt Ağ ilişkili bir NSG'ye sahip olabilir, ancak Özel Bitiş Noktası için ağ ilkeleri zorlamasını devre dışı bmelisiniz: [özel uç noktaları için ağ ilkelerini devre dışı kaldır.][disablesecuritype] Sonuç olarak, Herhangi bir NSG tarafından Özel Bitiş Noktası erişimi filtre olamaz.
- Web Uygulamanıza Özel Bitiş Noktası'nı etkinleştirdiğinizde, Web Uygulamasının [erişim kısıtlamaları][accessrestrictions] yapılandırması değerlendirilmez.
- Hedefin Internet veya Azure hizmetlerinin etiketi olduğu tüm NSG kurallarını kaldırarak VNet'ten veri sızma riskini azaltabilirsiniz. Ancak alt ağınıza bir Web Uygulaması Özel Bitiş Noktası eklemek, aynı dağıtım damgasında barındırılan ve Internet'e maruz kalan herhangi bir Web Uygulamasına erişmenizi sağlar.

Web Uygulamanızın Web HTTP günlüklerinde istemci kaynağı IP'yi bulacaksınız. Bu, Istemci IP özelliğini Web Uygulamasına ileterek TCP Proxy protokolü kullanılarak uygulanır. Daha fazla bilgi için Bkz. [TCP Proxy v2 kullanarak bağlantı Bilgileri Alma.][tcpproxy]


  > [!div class="mx-imgBorder"]
  > ![Web App Private Endpoint global genel bakış](media/private-endpoint/global-schema-web-app.png)

## <a name="dns"></a>DNS

Bu özellik önizlemede olduğundan, önizleme sırasında DNS girişini değiştirmeyiz. Özel DNS sunucunuzdaki veya Azure DNS özel bölgenizdeki DNS girişini kendiniz yönetmeniz gerekir.
Özel bir DNS adı kullanmanız gerekiyorsa, Web Uygulamanıza özel adı eklemeniz gerekir. Önizleme sırasında, özel ad, genel DNS çözünürlüğü kullanılarak herhangi bir özel ad gibi doğrulanmalıdır. Daha fazla bilgi için [özel DNS doğrulamaya][dnsvalidation] bakın.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma ayrıntıları için Azure [Özel Bağlantı fiyatlandırması'na][pricing]bakın.

## <a name="limitations"></a>Sınırlamalar

Özel Bağlantı özelliğini ve Private Endpoint özelliğini düzenli olarak geliştiriyoruz, sınırlamalar hakkında güncel bilgiler için [bu makaleyi][pllimitations] kontrol ediyoruz.

## <a name="next-steps"></a>Sonraki adımlar

Portal üzerinden Web Uygulamanız için Özel bitiş noktasını dağıtmak [için bir Web Uygulamasına özel olarak nasıl bağlanabilirsiniz][howtoguide]




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
