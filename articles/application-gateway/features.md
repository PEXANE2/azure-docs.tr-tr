---
title: Azure Application Gateway özellikleri
description: Azure Application Gateway özellikleri hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 550d9f4f5396b2165260e39cd28222b083dd6756
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78373629"
---
# <a name="azure-application-gateway-features"></a>Azure Application Gateway özellikleri

[Azure Application Gateway](overview.md), web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir.

![Application Gateway kavramsal](media/overview/figure1-720.png)

Application Gateway aşağıdaki özellikleri içerir:

- [Güvenli Yuva Katmanı (SSL/TLS) sonlandırma](#secure-sockets-layer-ssltls-termination)
- [Otomatik ölçeklendirme](#autoscaling)
- [Bölge artıklığı](#zone-redundancy)
- [Statik VIP](#static-vip)
- [Web uygulaması güvenlik duvarı](#web-application-firewall)
- [AKS için giriş denetleyicisi](#ingress-controller-for-aks)
- [URL tabanlı yönlendirme](#url-based-routing)
- [Birden çok site barındırma](#multiple-site-hosting)
- [Yönlendirme](#redirection)
- [Oturum benzeşimi](#session-affinity)
- [WebSocket ve HTTP/2 trafiği](#websocket-and-http2-traffic)
- [Bağlantı boşaltma](#connection-draining)
- [Özel hata sayfaları](#custom-error-pages)
- [HTTP üstbilgilerini yeniden yaz](#rewrite-http-headers)
- [Boyutlandırma](#sizing)

## <a name="secure-sockets-layer-ssltls-termination"></a>Güvenli Yuva Katmanı (SSL/TLS) sonlandırma

Application Gateway, ağ geçidinde SSL/TLS sonlandırmasını destekler ve bu sayede trafik genellikle arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak bazen sunucularla şifrelenmemiş iletişim kabul edilebilir bir seçenek değildir. Bunun nedeni güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bir bağlantıyı kabul etmesi olabilir. Bu uygulamalar için Application Gateway, uçtan uca SSL/TLS şifrelemesini destekler.

Daha fazla bilgi için bkz. [Application Gateway Ile SSL sonlandırmasına ve uçtan uca SSL 'ye genel bakış](ssl-overview.md)

## <a name="autoscaling"></a>Otomatik ölçeklendirme

Application Gateway Standard_v2 otomatik ölçeklendirmeyi destekler ve değişen trafik yükü desenlerine göre ölçeği değiştirebilir veya azaltabilirsiniz. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. 

Application Gateway Standard_v2 özellikleri hakkında daha fazla bilgi için bkz. [Otomatik ölçeklendirme v2 SKU 'su](application-gateway-autoscaling-zone-redundant.md).

## <a name="zone-redundancy"></a>Bölge artıklığı

Standard_v2 Application Gateway birden fazla Kullanılabilirlik Alanları yayarak, daha iyi hata dayanıklılığı sunarak her bölgede ayrı uygulama ağ geçitleri sağlama ihtiyacını ortadan kaldırabilir.

## <a name="static-vip"></a>Statik VIP

Uygulama ağ geçidi Standard_v2 SKU 'SU yalnızca statik VIP türünü destekler. Bu, uygulama ağ geçidi ile ilişkili VIP 'nin Application Gateway ömrü boyunca bile değişmemesini sağlar.

## <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web uygulaması güvenlik duvarı (WAF), Web uygulamalarınızın genel güvenlik açıklarından ve güvenlik açıklarından merkezi bir şekilde korunmasını sağlayan bir hizmettir. WAF, [OWASP (açık Web uygulaması güvenlik projesi) çekirdek kural kümeleri](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3,1 (yalnızca WAF_v2), 3,0 ve 2.2.9 kurallarını temel alır. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Mevcut uygulama ağ geçitleri, Web uygulaması güvenlik duvarı etkin bir Application Gateway 'e kolayca dönüştürülebilir.

Daha fazla bilgi için bkz. [Azure Web uygulaması güvenlik duvarı nedir?](../web-application-firewall/overview.md).

## <a name="ingress-controller-for-aks"></a>AKS için Giriş Denetleyicisi
Application Gateway giriş denetleyicisi (AGIC), bir [Azure Kubernetes hizmeti (AKS)](https://azure.microsoft.com/services/kubernetes-service/) kümesi için giriş olarak Application Gateway kullanmanıza olanak sağlar. 

Giriş denetleyicisi AKS kümesinde bir pod olarak çalışır ve [Kubernetes giriş kaynaklarını](https://kubernetes.io/docs/concepts/services-networking/ingress/) kullanır ve ağ geçidinin Kubernetes pods 'ye trafik yükünü dengelemeye olanak tanıyan bir Application Gateway yapılandırmasına dönüştürür. Giriş denetleyicisi yalnızca Application Gateway Standard_v2 ve WAF_v2 SKU 'Larını destekler. 

Daha fazla bilgi için bkz. [Application Gateway giriş denetleyicisi (AGIC)](ingress-controller-overview.md).

## <a name="url-based-routing"></a>URL tabanlı yönlendirme

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı havuzlara yönlendirmektir.

Örneğin, `http://contoso.com/video/*` için istekler VideoServerPool’a ve `http://contoso.com/images/*` için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

Daha fazla bilgi için bkz. [URL yolu tabanlı yönlendirmeye genel bakış](url-route-overview.md).

## <a name="multiple-site-hosting"></a>Birden çok site barındırma

Birden çok site barındırma, aynı uygulama ağ geçidi örneğinde birden fazla web sitesi yapılandırmanızı sağlar. Bu özellik, bir Application Gateway (en iyi performans için) en fazla 100 web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi kendi havuzuna yönlendirilebilir. Örneğin, uygulama ağ geçidi ContosoServerPool ve FabrikamServerPool adlı iki sunucu havuzundan `contoso.com` ve `fabrikam.com` için trafik sunabilir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı üst etki alanının iki alt etki alanı, aynı uygulama ağ geçidi dağıtımında barındırılabilir. Alt etki alanı kullanım örnekleri, tek bir uygulama ağ geçidi dağıtımında barındırılan `http://blog.contoso.com` ve `http://app.contoso.com` öğelerini içerebilir.

Daha fazla bilgi için bkz. [birden çok site barındırma Application Gateway](multiple-site-overview.md).

## <a name="redirection"></a>Yönlendirme

Birçok web uygulaması için yaygın bir senaryo, bir uygulama ile kullanıcıları arasındaki tüm iletişimin şifrelenmiş bir yol üzerinden yapıldığından emin olmak için otomatik HTTP’yi HTTPS’ye dönüştürme yeniden yönlendirmesini desteklemektir.

Geçmişte, tek amacı, HTTP üzerinde aldığı istekleri HTTPS 'ye yeniden yönlendirmek üzere olan adanmış havuz oluşturma gibi teknikler kullanmış olabilirsiniz. Uygulama ağ geçidi, Application Gateway’de trafiği yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Application Gateway yeniden yönlendirme desteği, tek başına HTTP ile HTTPS yönlendirmesi ile sınırlı değildir. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Ayrıca, bir dış siteye yönlendirmeyi de destekler.

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

- Ağ Geçidi üzerindeki bir bağlantı noktasından başka bir bağlantı noktasına genel yeniden yönlendirme. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- Yol tabanlı yönlendirme. Bu tür yeniden yönlendirmeler, HTTP’den HTTPS’ye yeniden yönlendirmeyi yalnızca belirli bir site alanında (örneğin `/cart/*` tarafından belirtilen bir alışveriş sepetinde) etkinleştirir.
- Dış siteye yeniden yönlendirme.

Daha fazla bilgi için bkz. [Application Gateway yeniden yönlendirmeye genel bakış](redirect-overview.md).

## <a name="session-affinity"></a>Oturum benzeşimi

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

Daha fazla bilgi için bkz. [Application Gateway 'In nasıl çalıştığı](how-application-gateway-works.md#modifications-to-the-request).

## <a name="websocket-and-http2-traffic"></a>Websocket ve HTTP/2 trafiği

Application Gateway, WebSocket ve HTTP/2 protokolleri için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

WebSocket ve HTTP/2 protokolleri, uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ile bir istemci arasında tam çift yönlü iletişimi etkinleştirir. Bu, web sunucusu ile istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. Bu protokollerin, HTTP 'den farklı olarak düşük bir yükü vardır ve birden çok istek/yanıt için aynı TCP bağlantısını daha verimli bir kaynak kullanımına neden olabilir. Bu protokoller, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.

Daha fazla bilgi için bkz. [WebSocket desteği](application-gateway-websocket.md) ve [http/2 desteği](configuration-overview.md#http2-support).

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar bir arka uç http ayarıyla etkinleştirilir ve kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulanabilir. Application Gateway etkinleştirildikten sonra, bir arka uç havuzunun tüm kaydını kaldırmak örneklerinin yeni bir istek almamasını sağlarken, mevcut isteklerin yapılandırılmış bir süre sınırı içinde tamamlanmasını sağlar. Bu, arka uç havuzundan bir Kullanıcı yapılandırma değişikliği tarafından açıkça kaldırılan arka uç örnekleri için ve sistem durumu araştırmaları tarafından belirlendiği şekilde sağlıksız olarak bildirilen arka uç örnekleri için geçerlidir. Bunun tek istisnası, ağ geçidi ile yönetilen oturum benzeşimi nedeniyle açıkça kaydı yapılmış olan ve kaydını kaldırmak örneklerine yönelik olarak devam eden kaydını kaldırmak örneklerine yönelik isteklerdir.

Daha fazla bilgi için bkz. [Application Gateway yapılandırmasına genel bakış](configuration-overview.md#connection-draining).

## <a name="custom-error-pages"></a>Özel hata sayfaları

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Daha fazla bilgi için bkz. [özel hatalar](custom-error.md).

## <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

HTTP üstbilgileri, istemci ve sunucunun istek veya Yanıt ile ek bilgi geçmesine izin verir. Bu HTTP üstbilgilerini yeniden yazmak, aşağıdaki gibi çeşitli önemli senaryolar gerçekleştirmenize yardımcı olur:

- HSTS/X-XSS-Protection gibi güvenlikle ilgili üst bilgi alanları ekleme.
- Hassas bilgileri açığa çıkartan yanıt üst bilgisi alanlarını kaldırma.
- Üst bilgiler Için X-Iletilen bağlantı noktası bilgilerini yakdırın.

Application Gateway, HTTP isteği ve yanıt üst bilgilerini ekleme, kaldırma veya güncelleştirme özelliğini destekler, ancak istek ve yanıt paketleri istemci ile arka uç havuzları arasında taşınır. Ayrıca, belirtilen üstbilgilerin yalnızca belirli koşullar karşılandığında yeniden yazılabilir olmasını sağlamak için koşullar ekleme özelliği de sağlar.

Daha fazla bilgi için bkz. [HTTP üstbilgilerini yeniden yazma](rewrite-http-headers.md).

## <a name="sizing"></a>Boyutlandırma

Application Gateway Standard_v2, otomatik ölçeklendirme veya sabit boyutlu dağıtımlar için yapılandırılabilir. Bu SKU farklı örnek boyutları sunmaz. V2 performansı ve fiyatlandırması hakkında daha fazla bilgi için bkz. [Otomatik ölçeklendirme v2 SKU 'su](application-gateway-autoscaling-zone-redundant.md#pricing).

Application Gateway standart üç boyutta sunulur: **küçük**, **Orta**ve **büyük**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Aşağıdaki tabloda SSL boşaltması etkin olan her bir Application Gateway v1 örneği için ortalama performans performansı gösterilmektedir:

| Ortalama arka uç sayfa yanıt boyutu | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- Application Gateway nasıl çalıştığını öğrenin- [uygulama ağ geçidi nasıl kullanılır](how-application-gateway-works.md) ?