---
title: Azure ön kapısı | Microsoft Docs
description: Bu makalede Azure Front Door’a genel bir bakış sağlanır.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2020
ms.author: duau
ms.openlocfilehash: e5b8e5059d4d7df8d7061698ea29d7a6785236f9
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91825636"
---
# <a name="what-is-azure-front-door"></a>Azure Front Door nedir?

Azure ön kapı, hızlı, güvenli ve yaygın olarak ölçeklenebilir Web uygulamaları oluşturmak için Microsoft Global Edge ağını kullanan küresel, ölçeklenebilir bir giriş noktasıdır. Ön kapı sayesinde küresel tüketici ve kurumsal uygulamalarınızı, Azure aracılığıyla küresel bir hedef kitleye ulaşan içerikle sağlam ve yüksek performanslı modern uygulamalara dönüştürebilirsiniz.

<p align="center">
  <img src="./media/front-door-overview/front-door-visual-diagram.png" alt="Front Door architecture" width="600" title="Azure Front Door">
</p>

Ön kapı, genel bağlantıyı geliştirmek için bölünmüş TCP ve Microsoft 'un küresel ağı ile her noktaya yayın protokolünü kullanarak katman 7 ' de (HTTP/HTTPS katmanı) çalışmaktadır. Yönlendirme yönteminizin temel alınarak, ön kapısının istemci isteklerinizi en hızlı ve en uygun uygulama arka ucuna yönlendirdiğinden emin olabilirsiniz. Uygulama arka ucu, Azure'un içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Ön kapı, farklı uygulama ihtiyaçlarına ve otomatik yük devretme senaryolarına uyacak şekilde çeşitli [trafik yönlendirme yöntemleri](front-door-routing-methods.md) ve [arka uç durumu izleme seçenekleri](front-door-health-probes.md) sağlar. [Traffic Manager](../traffic-manager/traffic-manager-overview.md)benzer şekilde, ön kapı, Azure bölgesinin tamamına yönelik hatalarla birlikte hatalara karşı dayanıklıdır.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. 
> * DNS tabanlı küresel yönlendirme yapmak istiyorsanız ve Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma"), HTTP/HTTPS isteği veya uygulama katmanı işleme gereksinimlerine sahip **değilseniz** [Traffic Manager](../traffic-manager/traffic-manager-overview.md)gözden geçirin. 
> * Uygulama katmanındaki bir bölgedeki sunucularınız arasında yük dengelemesi yapmak istiyorsanız, [Application Gateway](../application-gateway/application-gateway-introduction.md) gözden geçirin
> * Ağ katmanı yük dengelemesi yapmak için [Load Balancer](../load-balancer/load-balancer-overview.md)gözden geçirin. 
> 
> Uçtan uca senaryolarınız, bu çözümlerin gerektiğinde birleştirilmesinin avantajlarından yararlanabilir.
> Azure yük dengeleme seçenekleri karşılaştırması için bkz. [Azure 'da Yük Dengeleme seçeneklerine genel bakış](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

## <a name="why-use-azure-front-door"></a>Azure ön kapısı neden kullanılmalıdır?

Ön kapılı ile dinamik Web uygulamanızı ve statik içeriğinizi oluşturabilir, işleyebilir ve ölçeklendirebilirsiniz. Ön kapı, hızlı genel yük devretme ile en üst katman Son Kullanıcı performansını ve güvenilirliğini iyileştirerek Web trafiğiniz için küresel yönlendirmeyi tanımlamanıza, yönetmenize ve izlemenize olanak sağlar.

Ön kapıda bulunan temel özellikler:

* **[Bölünmüş TCP](front-door-routing-architecture.md#splittcp)** tabanlı **[her noktaya yayın protokolünü](front-door-routing-architecture.md#anycast)** kullanarak hızlandırılmış uygulama performansı.

* Arka uç kaynakları için akıllı **[durum araştırma](front-door-health-probes.md)** izlemesi.

*  İstekler için **[URL yolu tabanlı](front-door-route-matching.md)** yönlendirme.

* Verimli uygulama altyapısı için birden çok Web sitesinin barındırılmasına izin sağlar. 

* Tanımlama bilgisi tabanlı **[oturum benzeşimi](front-door-routing-methods.md#affinity)**.

* **[SSL boşaltma](front-door-custom-domain-https.md)** ve sertifika yönetimi.

* Kendi **[özel etki](front-door-custom-domain.md)** alanınızı tanımlayın. 

* Tümleşik  **[Web uygulaması güvenlik duvarı (WAF)](../web-application-firewall/overview.md)** ile uygulama güvenliği.

* HTTP trafiğini **[URL yeniden yönlendirme](front-door-url-redirect.md)** ile https 'ye yönlendirin.

* **[URL yeniden yazma](front-door-url-rewrite.md)** ile özel iletme yolu.

* Uçtan uca IPv6 bağlantısı ve **[http/2 protokolünün](front-door-http2.md)** yerel desteği.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Front Door Fiyatlandırması](https://azure.microsoft.com/pricing/details/frontdoor/). Bkz. [Azure ön kapısı Için SLA](https://azure.microsoft.com/en-us/support/legal/sla/frontdoor/v1_0/).

## <a name="whats-new"></a>Yenilikler

RSS akışına abone olun ve [Azure Updates](https://azure.microsoft.com/updates/?category=networking&query=Azure%20Front%20Door) sayfasında en son Azure ön kapılı Özellik güncelleştirmelerini görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
