---
title: Azure Web Uygulaması Güvenlik Duvarı - Sık Sorulan Sorular
description: Bu makalede, Azure Ön Kapı'da Web Uygulaması Güvenlik Duvarı hakkında sık sorulan soruların yanıtlarını sağlar
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.devlang: na
ms.topic: article
ms.date: 09/25/2019
ms.author: victorh
ms.openlocfilehash: fefbb038cec0c061f1fd191a77164880372555a0
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314316"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall-on-azure-front-door-service"></a>Azure Ön Kapı Hizmetinde Azure Web Uygulaması Güvenlik Duvarı için sık sorulan sorular

Bu makalede, Azure web uygulaması güvenlik duvarı (WAF) özellikleri ve işlevleri yle ilgili sık sorulan sorular yanıtlanmaktadır. 

## <a name="what-is-azure-waf"></a>Azure WAF nedir?

Azure WAF, web uygulamalarınızı SQL enjeksiyonu, site ler arası komut dosyası uygulama ve diğer web açıkları gibi yaygın tehditlere karşı korumaya yardımcı olan bir web uygulaması güvenlik duvarıdır. Web uygulamalarınıza erişimi denetlemek için özel ve yönetilen kuralların bir birleşiminden oluşan bir WAF ilkesi tanımlayabilirsiniz.

Uygulama Ağ Geçidi'nde veya Azure Ön Kapılar'da barındırılan web uygulamalarına Azure WAF ilkesi uygulanabilir.

## <a name="what-is-waf-on-azure-front-door"></a>Azure Ön Kapı'da WAF nedir? 

Azure Ön Kapı, son derece ölçeklenebilir, küresel olarak dağıtılmış bir uygulama ve içerik dağıtım ağıdır. Azure WAF, Ön Kapı ile entegre edildiğinde, hizmet reddi ve hedeflenen uygulama saldırılarını Azure ağ kenarında durdurur, sanal ağınıza girmeden önce saldırı kaynaklarına yakın olur, performanstan ödün vermeden koruma sağlar.

## <a name="does-azure-waf-support-https"></a>Azure WAF HTTPS'yi destekliyor mu?

Ön Kapı TLS boşaltma sunuyor. WAF, Front Door ile yerel olarak entegre edilmiştir ve bir isteği şifresi çözüldükten sonra inceleyebilir.

## <a name="does-azure-waf-support-ipv6"></a>Azure WAF IPv6'yı destekliyor mu?

Evet. IPv4 ve IPv6 için IP kısıtlaması yapılandırabilirsiniz.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>Yönetilen kural kümeleri ne kadar güncel?

Değişen tehdit ortamına ayak uydurmak için elimizden geleni yapıyoruz. Yeni bir kural güncelleştirildikten sonra, yeni bir sürüm numarasıyla Varsayılan Kural Kümesi'ne eklenir.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>WAF politikamda değişiklik yaparsam yayılma süresi nedir?

Bir WAF ilkesinin küresel olarak dağıtılması genellikle yaklaşık 5 dakika sürer ve genellikle daha erken tamamlar.

## <a name="can-waf-policies-be-different-for-different-regions"></a>WAF ilkeleri farklı bölgeler için farklı olabilir mi?

Front Door ile entegre edildiğinde, WAF küresel bir kaynaktır. Aynı yapılandırma tüm Ön Kapı konumlarında geçerlidir.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>Arka ucuma erişimi yalnızca Ön Kapı'dan olmak için nasıl sınırlandırebilirim?

IP Erişim Kontrol Listesi'ni arka uçunuzda, yalnızca Ön Kapı giden IP adresi aralıklarına izin vermek ve Internet'ten doğrudan erişimi reddetmek için yapılandırabilirsiniz. Hizmet etiketleri, sanal ağınızda kullanmanız için desteklenir. Ayrıca, X-Forwarded-Host HTTP üstbilgi alanının web uygulamanız için geçerli olduğunu da doğrulayabilirsiniz.

## <a name="which-azure-waf-options-should-i-choose"></a>Hangi Azure WAF seçeneklerini seçmeliyim?

Azure'da WAF ilkeleri uygularken iki seçenek vardır. Azure Ön Kapılı WAF, dünya çapında dağıtılan, kenar güvenlik çözümüdür. Waf uygulama ağ geçidi ile bölgesel, özel bir çözümdür. Genel performans ve güvenlik gereksinimlerinize göre bir çözüm seçmenizi öneririz. Daha fazla bilgi için [Azure'un uygulama teslim paketiyle Yük dengeleme'ye](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite)bakın.


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>Tüm entegre platformlarda aynı WAF özelliklerini destekliyor musunuz?

Şu anda, ModSec CRS 2.2.9, CRS 3.0 ve CRS 3.1 kuralları yalnızca Uygulama Ağ Geçidi'ndeki WAF ile desteklenir. Hız sınırlayıcı, coğrafi filtreleme ve Azure yönetilen Varsayılan Kural Kümesi kuralları yalnızca Azure Ön Kapı'daki WAF ile desteklenir.

## <a name="is-ddos-protection-integrated-with-front-door"></a>DDoS koruması Ön Kapı ile entegre midir? 

Azure ağ kenarlarında genel olarak dağıtılan Azure Ön Kapı, büyük hacimli saldırıları emebilir ve coğrafi olarak yalıtabilir. İmzaları bilinen http(ler) saldırılarını otomatik olarak engellemek ve sınırlamak için özel WAF ilkesi oluşturabilirsiniz. Dahası, arka uçlarınızın dağıtıldığı VNet'te DDoS Koruma Standardını etkinleştirebilirsiniz. Azure DDoS Koruma Standardı müşterileri, bir saldırı sırasında anında yardım almak için maliyet koruması, SLA garantisi ve DDoS Hızlı Müdahale Ekibi'nden uzmanlara erişim gibi ek avantajlardan yararlanır.

## <a name="why-do-additional-requests-above-the-threshold-configured-for-my-rate-limit-rule-get-passed-to-my-backend-server"></a>Neden oran sınırı kuralım için yapılandırılan eşiğin üzerindeki ek istekler arka uç sunucuma geçirilir?

Bir oran sınırı kuralı herhangi bir istemci IP adresinden anormal yüksek trafik sınırlayabilir. Bir dakika veya beş dakikalık bir süre boyunca istemci IP adresinden izin verilen web isteklerinin sayısına bir eşik göre yapılandırabilirsiniz. Parçalı hız denetimi için, hız sınırlaması HTTP(S) parametre eşleştirmegibi ek eşleşme koşullarıyla birleştirilebilir. 

Aynı istemciden gelen istekler genellikle aynı Ön Kapı sunucusuna ulaşır. Bu durumda, eşiğin üzerindeki ek isteklerin hemen engellendiğini görürsünüz. 

Ancak, aynı istemciden gelen isteklerin, fiyat sınırı sayacını henüz yenilememiş farklı bir Ön Kapı sunucusuna ulaşması mümkündür. Örneğin, istemci her istek için yeni bir bağlantı açabilir ve eşik düşüktür. Bu durumda, yeni Ön Kapı sunucusuna ilk istek oran sınırı denetimi nden geçer. Bir oran sınırı eşiği genellikle herhangi bir istemci IP adresinden hizmet reddi saldırılarına karşı savunmak için yüksek ayarlanır. Çok düşük bir eşik için, eşiğin üzerinde ek isteklerin geçtiğini görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Web Uygulaması Güvenlik Duvarı](../overview.md)hakkında bilgi edinin.
- [Azure Ön Kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin.
