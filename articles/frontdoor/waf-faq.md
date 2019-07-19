---
title: Azure Web uygulaması güvenlik duvarı-sık sorulan sorular
description: Bu sayfa, Azure ön kapı hizmeti hakkında sık sorulan soruların yanıtlarını sağlar
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: c993e465bc439ff52cba3241dbff64b7655d1f12
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849170"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall"></a>Azure Web uygulaması güvenlik duvarı hakkında sık sorulan sorular

Bu makalede, Azure Web uygulaması güvenlik duvarı (WAF) özellikleri ve işlevleri hakkında sık sorulan sorular yanıtlanmaktadır. 

## <a name="what-is-azure-waf"></a>Azure WAF nedir?

Azure WAF, Web uygulamalarınızı SQL ekleme, siteler arası komut dosyası ve diğer web siteleri gibi yaygın tehditlere karşı korumanıza yardımcı olan bir Web uygulaması güvenlik duvarıdır. Web uygulamalarınıza erişimi denetlemek için özel ve yönetilen kuralların birleşiminden oluşan bir WAF ilkesi tanımlayabilirsiniz.

Azure WAF ilkesi, Application Gateway veya Azure ön kapılarında barındırılan Web uygulamalarına uygulanabilir.

## <a name="what-is-waf-for-azure-front-door-service"></a>Azure ön kapı hizmeti için WAF nedir? 

Azure ön kapısı, yüksek düzeyde ölçeklenebilir, global olarak dağıtılmış bir uygulama ve içerik teslim ağı. Azure WAF, ön kapıya tümleştirildiğinde hizmet reddi ve hedeflenen uygulama saldırılarını, Sanal ağınızı girmeden önce, saldırı kaynaklarına yakın bir şekilde kapatır ve performanstan ödün vermeden koruma sunar.

## <a name="does-azure-waf-support-https"></a>Azure WAF, HTTPS 'yi destekliyor mu?

Ön kapı hizmeti, SSL boşaltma sağlar. WAF, ön kapıya yerel olarak tümleşiktir ve şifresi çözüledikten sonra bir isteği inceleyebilir.

## <a name="does-azure-waf-support-ipv6"></a>Azure WAF, IPv6 'Yı destekliyor mu?

Evet. IPv4 ve IPv6 için IP kısıtlaması yapılandırabilirsiniz.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>Yönetilen kural kümeleri güncel değil mi?

Tehdit Yatayı değiştirmekle en iyi şekilde haberdar ediyoruz. Yeni bir kural güncelleştirildikten sonra, varsayılan kural kümesine yeni bir sürüm numarasıyla eklenir.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>WAF ilkenizde bir değişiklik yaptım zaman yayma süresi nedir?

Bir WAF ilkesini küresel olarak dağıtmak yaklaşık 5 dakika sürer ve genellikle daha erken tamamlanır.

## <a name="can-waf-policies-be-different-for-different-regions"></a>WAF ilkeleri farklı bölgeler için farklı olabilir mi?

Ön kapı hizmeti ile tümleştirildiğinde WAF genel bir kaynaktır. Tüm ön kapı konumlarında aynı yapılandırma geçerlidir.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>Nasıl yaparım? arka uca erişimi yalnızca ön kapıdan olacak şekilde sınırlayın mi?

Arka uçınızdan IP Access Control listesini, yalnızca ön kapı giden IP adresi aralıklarına izin vermek ve Internet 'ten doğrudan erişimi reddetmek için yapılandırabilirsiniz. Sanal ağınızda kullanabilmeniz için hizmet etiketleri desteklenir. Ayrıca, X ile Iletilen ana bilgisayar HTTP üst bilgisi alanının Web uygulamanız için geçerli olduğunu doğrulayabilirsiniz.




## <a name="which-azure-waf-options-should-i-choose"></a>Hangi Azure WAF seçeneklerini seçmem gerekir?

Azure 'da WAF İlkeleri uygulanırken iki seçenek vardır. Azure ön kapısına sahip WAF, küresel olarak dağıtılmış bir uç güvenlik çözümüdür. Application Gateway ile WAF, bölgesel ve adanmış bir çözümdür. Genel performans ve güvenlik gereksinimlerinize göre bir çözüm seçmenizi öneririz. Daha fazla bilgi için bkz. [Azure 'un uygulama teslim paketiyle Yük Dengeleme](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite).


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>Tümleştirilen tüm platformlarda aynı WAF özelliklerini destekliyor musunuz?

Şu anda ModSec, 2.2.9 ve yukarı 3,0 kuralları yalnızca Application Gateway 'de WAF ile desteklenir. Hız sınırlaması, coğrafi filtreleme ve Azure tarafından yönetilen varsayılan kural kümesi kuralları yalnızca Azure ön kapıda WAF ile desteklenir.

## <a name="is-ddos-protection-integrated-with-front-door"></a>DDoS koruması ön kapıla tümleşiktir mı? 

Azure ağ kenarlarında küresel olarak dağıtılan Azure ön kapısı, büyük hacimli saldırıları artışlarını devralarak ve coğrafi olarak yalıtabilir. Bilinen imzaları olan http (ler) saldırılarını otomatik olarak engellemek ve derecelendirmek için özel WAF ilkesi oluşturabilirsiniz. Daha fazla bilgi için arka uçlarınızın dağıtıldığı VNet üzerinde DDoS koruma standardını etkinleştirebilirsiniz. Azure DDoS koruması standart müşterileri, bir saldırı sırasında anında yardım almak için maliyet koruması, SLA garantisi ve DDoS hızlı yanıt ekibinden uzmanlara erişim gibi ek avantajlar sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Web uygulaması güvenlik duvarı](waf-overview.md)hakkında bilgi edinin.
- [Azure ön kapısı](front-door-overview.md)hakkında daha fazla bilgi edinin.
