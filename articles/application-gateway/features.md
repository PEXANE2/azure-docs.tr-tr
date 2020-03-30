---
title: Azure Uygulama Ağ Geçidi özellikleri
description: Azure Uygulama Ağ Geçidi özellikleri hakkında bilgi edinin
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 550d9f4f5396b2165260e39cd28222b083dd6756
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279994"
---
# <a name="azure-application-gateway-features"></a>Azure Uygulama Ağ Geçidi özellikleri

[Azure Application Gateway](overview.md), web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir.

![Uygulama Ağ Geçidi kavramsal](media/overview/figure1-720.png)

Uygulama Ağ Geçidi aşağıdaki özellikleri içerir:

- [Güvenli Soket Katmanı (SSL/TLS) sonlandırma](#secure-sockets-layer-ssltls-termination)
- [Otomatik ölçeklendirme](#autoscaling)
- [Bölge artıklığı](#zone-redundancy)
- [Statik VIP](#static-vip)
- [Web Uygulama Güvenlik Duvarı](#web-application-firewall)
- [AKS için Giriş Denetleyicisi](#ingress-controller-for-aks)
- [URL tabanlı yönlendirme](#url-based-routing)
- [Birden çok site barındırma](#multiple-site-hosting)
- [Yönlendirme](#redirection)
- [Oturum yakınlığı](#session-affinity)
- [Websocket ve HTTP/2 trafiği](#websocket-and-http2-traffic)
- [Bağlantı boşaltma](#connection-draining)
- [Özel hata sayfaları](#custom-error-pages)
- [HTTP üst bilgilerini yeniden üretme](#rewrite-http-headers)
- [Boyutlandırma](#sizing)

## <a name="secure-sockets-layer-ssltls-termination"></a>Güvenli Soket Katmanı (SSL/TLS) sonlandırma

Uygulama ağ geçidi, ağ geçidinde SSL/TLS sonlandırmayı destekler ve ardından trafik genellikle arka uç sunucularına şifrelenmemiş olarak akar. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. Ancak bazen sunuculara şifrelenmemiş iletişim kabul edilebilir bir seçenek değildir. Bunun nedeni güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bir bağlantıyı kabul edebileceği olabilir. Bu uygulamalar için uygulama ağ geçidi, ssl/TLS şifrelemeuçtan uca destekler.

Daha fazla bilgi için, [Uygulama Ağ Geçidi ile SSL sonlandırma ve bitiş sonu SSL Genel Bakış](ssl-overview.md) bakın

## <a name="autoscaling"></a>Otomatik ölçeklendirme

Uygulama Ağ Geçidi Standard_v2 otomatik ölçeklendirmeyi destekler ve değişen trafik yükü desenlerine göre ölçeklenebilir veya küçültebilir. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. 

Uygulama Ağ Geçidi Standard_v2 özellikleri hakkında daha fazla bilgi için [v2 SKU'yu otomatik olarak ölçekleme](application-gateway-autoscaling-zone-redundant.md)bilgisine bakın.

## <a name="zone-redundancy"></a>Bölge artıklığı

Standard_v2 Uygulama Ağ Geçidi, daha iyi hata esnekliği sunarak ve her bölgede ayrı Uygulama Ağ Geçitleri sağlama gereksinimini ortadan kaldırarak birden çok Kullanılabilirlik Bölgesi'ni kapsayabilir.

## <a name="static-vip"></a>Statik VIP

SKUStandard_v2 uygulama ağ geçidi yalnızca statik VIP türünü destekler. Bu, uygulama ağ geçidiyle ilişkili VIP'nin Uygulama Ağ Geçidi'nin ömrü boyunca bile değişmemesini sağlar.

## <a name="web-application-firewall"></a>Web Uygulaması Güvenlik Duvarı

Web Uygulaması Güvenlik Duvarı (WAF), web uygulamalarınızın yaygın açıklardan ve güvenlik açıklarına karşı merkezi koruma sağlayan bir hizmettir. WAF, [OWASP (Open Web Application Security Project) temel kuralı 3,1](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) (yalnızca WAF_v2), 3,0 ve 2,9'daki kurallara dayanır. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Varolan uygulama ağ geçitleri, uygulama ağ geçidini etkinleştirilen bir Web Uygulaması Güvenlik Duvarı'na kolayca dönüştürülebilir.

Daha fazla bilgi için Azure [Web Uygulaması Güvenlik Duvarı nedir?](../web-application-firewall/overview.md)

## <a name="ingress-controller-for-aks"></a>AKS için Giriş Denetleyicisi
Uygulama Ağ Geçidi Denetleyicisi (AGIC), Bir Azure [Kubernetes Hizmeti (AKS)](https://azure.microsoft.com/services/kubernetes-service/) kümesi için giriş olarak Uygulama Ağ Geçidi'ni kullanmanıza olanak tanır. 

Giriş denetleyicisi AKS kümesi içinde bir bölme olarak çalışır ve [Kubernetes Giriş Kaynaklarını](https://kubernetes.io/docs/concepts/services-networking/ingress/) tüketir ve bunları Kubernetes bölmelerine yük bakiyetrafiğini sağlayan bir Uygulama Ağ Geçidi yapılandırmasına dönüştürür. Giriş denetleyicisi yalnızca Uygulama Ağ Geçidi Standard_v2 ve WAF_v2 SNU'ları destekler. 

Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC)](ingress-controller-overview.md).

## <a name="url-based-routing"></a>URL tabanlı yönlendirme

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı havuzlara yönlendirmektir.

Örneğin, `http://contoso.com/video/*` için istekler VideoServerPool’a ve `http://contoso.com/images/*` için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

Daha fazla bilgi için [URL Yol Tabanlı Yönlendirme genel görünümüne](url-route-overview.md)bakın.

## <a name="multiple-site-hosting"></a>Birden çok site barındırma

Birden çok site barındırma, aynı uygulama ağ geçidi örneğinde birden fazla web sitesi yapılandırmanızı sağlar. Bu özellik, 100'e kadar web sitesi bir Uygulama Ağ Geçidine ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanızı sağlar (en iyi performans için). Her web sitesi kendi havuzuna yönlendirilebilir. Örneğin, uygulama ağ geçidi ContosoServerPool ve FabrikamServerPool adlı iki sunucu havuzundan `contoso.com` ve `fabrikam.com` için trafik sunabilir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı üst etki alanının iki alt etki alanı, aynı uygulama ağ geçidi dağıtımında barındırılabilir. Alt etki alanı kullanım örnekleri, tek bir uygulama ağ geçidi dağıtımında barındırılan `http://blog.contoso.com` ve `http://app.contoso.com` öğelerini içerebilir.

Daha fazla bilgi için Uygulama [Ağ Geçidi birden çok site barındırma](multiple-site-overview.md)bakın.

## <a name="redirection"></a>Yönlendirme

Birçok web uygulaması için yaygın bir senaryo, bir uygulama ile kullanıcıları arasındaki tüm iletişimin şifrelenmiş bir yol üzerinden yapıldığından emin olmak için otomatik HTTP’yi HTTPS’ye dönüştürme yeniden yönlendirmesini desteklemektir.

Geçmişte, tek amacı HTTP'den aldığı istekleri HTTPS'ye yönlendirmek olan özel havuz oluşturma gibi teknikler kullanmış olabilirsiniz. Uygulama ağ geçidi, Application Gateway’de trafiği yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Uygulama Ağ Geçidi yeniden yönlendirme desteği yalnızca HTTP ile HTTPS yeniden yönlendirmesi ile sınırlı değildir. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Ayrıca, bir dış siteye yönlendirmeyi de destekler.

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

- Ağ Geçidi üzerindeki bir bağlantı noktasından başka bir bağlantı noktasına genel yeniden yönlendirme. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- Yol tabanlı yönlendirme. Bu tür yeniden yönlendirmeler, HTTP’den HTTPS’ye yeniden yönlendirmeyi yalnızca belirli bir site alanında (örneğin `/cart/*` tarafından belirtilen bir alışveriş sepetinde) etkinleştirir.
- Dış siteye yeniden yönlendirme.

Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yeniden yönlendirme.](redirect-overview.md)

## <a name="session-affinity"></a>Oturum benzeşimi

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

Daha fazla bilgi için, [uygulama ağ geçidinin nasıl çalıştığını](how-application-gateway-works.md#modifications-to-the-request)görün.

## <a name="websocket-and-http2-traffic"></a>Websocket ve HTTP/2 trafiği

Application Gateway, WebSocket ve HTTP/2 protokolleri için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

WebSocket ve HTTP/2 protokolleri, uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ile bir istemci arasında tam çift yönlü iletişimi etkinleştirir. Bu, web sunucusu ile istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. Bu protokoller, HTTP'nin aksine düşük ek yüküne sahiptir ve aynı TCP bağlantısını birden çok istek/yanıt için yeniden kullanabilir ve bu da daha verimli bir kaynak kullanımıyla sonuçlanır. Bu protokoller, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.

Daha fazla bilgi için [WebSocket desteği](application-gateway-websocket.md) ve [HTTP/2 desteğine](configuration-overview.md#http2-support)bakın.

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar bir arka uç http ayarıyla etkinleştirilir ve kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulanabilir. Etkinleştirildiğinde, Uygulama Ağ Geçidi, varolan isteklerin yapılandırılmış bir zaman sınırı içinde tamamlanmasına izin verirken, arka uç havuzunun tüm kayıt silme örneklerinin yeni bir istek almamasını sağlar. Bu, kullanıcı yapılandırma sıcağı değişikliği yle arka uç havuzundan açıkça kaldırılan arka uç örnekleri ve sistem durumu sondaları tarafından belirlendiği gibi sağlıksız olarak bildirilen arka uç örnekleri için de geçerlidir. Bunun tek istisnası, ağ geçidi tarafından yönetilen oturum yakınlığı nedeniyle açıkça kayıt dışı bırakılmış olan ve kayıt silme örneklerine yakın olmaya devam eden örnekleri silme isteğidir.

Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi Yapılandırmagenel Bakış.](configuration-overview.md#connection-draining)

## <a name="custom-error-pages"></a>Özel hata sayfaları

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

Daha fazla bilgi için [Özel Hatalar'a](custom-error.md)bakın.

## <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

HTTP üstbilgileri istemci ve sunucu istek veya yanıt ile ek bilgi aktarmak için izin verir. Bu HTTP üstbilgilerinin yeniden yazılması, şu gibi birkaç önemli senaryo gerçekleştirmenize yardımcı olur:

- HSTS/ X-XSS-Protection gibi güvenlikle ilgili üstbilgi alanları ekleme.
- Hassas bilgileri ortaya çıkarabilecek yanıt üstbilgisi alanlarını kaldırma.
- X-Forwarded-For başlıklarından bağlantı noktası bilgilerini sıyırma.

Uygulama Ağ Geçidi, HTTP istek ve yanıt üstbilgilerini ekleme, kaldırma veya güncelleştirme özelliğini desteklerken, istek ve yanıt paketleri istemci ve arka uç havuzları arasında taşınır. Ayrıca, belirtilen üstbilginin yalnızca belirli koşullar karşılandığında yeniden yazılmasını sağlamak için koşullar ekleme olanağı da sağlar.

Daha fazla bilgi için [http üstbilgilerini yeniden yazın'](rewrite-http-headers.md)a bakın.

## <a name="sizing"></a>Boyutlandırma

Uygulama Ağ Geçidi Standard_v2 otomatik ölçekleme veya sabit boyutlu dağıtımlar için yapılandırılabilir. Bu SKU farklı örnek boyutları sunmuyor. v2 performansı ve fiyatlandırması hakkında daha fazla bilgi için [v2 SKU'yı Otomatik lendirme](application-gateway-autoscaling-zone-redundant.md#pricing)ye bakın.

Uygulama Ağ Geçidi Standardı üç boyutta sunulur: **Küçük,** **Orta**ve **Büyük.** Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Aşağıdaki tablo, SSL boşaltma etkin olan her uygulama ağ geçidi v1 örneği için ortalama bir performans iş ortası gösterir:

| Ortalama arka uç sayfa yanıt boyutu | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mb/sn |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama Ağ Geçidi'nin nasıl çalıştığını öğrenin - [Uygulama ağ geçidi nasıl çalışır?](how-application-gateway-works.md)