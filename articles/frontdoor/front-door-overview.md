---
title: Azure ön kapısı | Microsoft Docs
description: Bu makalede Azure Front Door’a genel bir bakış sağlanır. Uygulamanız için Yük Dengeleme Kullanıcı trafiği için doğru seçim olup olmadığını öğrenin.
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/23/2019
ms.author: sharadag
ms.openlocfilehash: 081305170020aa35511ad574bd8c5575c192d577
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85322286"
---
# <a name="what-is-azure-front-door"></a>Azure Front Door nedir?
Azure ön kapısı, yüksek kullanılabilirlik için en iyi performansı ve hızlı genel yük devretmeyi iyileştirerek Web trafiğiniz için küresel yönlendirmeyi tanımlamanıza, yönetmenize ve izlemenize olanak sağlar. Ön kapı sayesinde küresel (çok bölgeli) tüketici ve kurumsal uygulamalarınızı, Azure ile küresel bir hedef kitleye ulaşan sağlam, yüksek performanslı, kişiselleştirilmiş modern uygulamalar, API 'Ler ve içeriğe dönüştürebilirsiniz.

Front Door 7. Katmanda veya HTTP/HTTPS katmanında çalışır ve genel bağlantıyı geliştirmek için bölünmüş TCP ile her noktaya yayın protokolünü ve Microsoft'un genel ağını kullanır. Bu nedenle, yapılandırmada yaptığınız yönlendirme yöntemi seçimine göre, Front Door’un istemci isteklerini en hızlı ve en kullanılabilir uygulama arka ucuna yönlendirdiğinden emin olabilirsiniz. Uygulama arka ucu, Azure'un içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Front Door, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](front-door-routing-methods.md) ve [arka uç durumunu izleme seçenekleri](front-door-health-probes.md) sunar. [Traffic Manager](../traffic-manager/traffic-manager-overview.md) gibi Front Door da bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. DNS tabanlı genel yönlendirme istiyorsanız ve Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği başına uygulama katmanı işleme özellikleri gereksinimleriniz **yoksa**, [Traffic Manager](../traffic-manager/traffic-manager-overview.md)'ı inceleyin. Bölgenizdeki sunucular arasında yük dengeleme istiyorsanız, uygulama katmanı için [Application Gateway](../application-gateway/application-gateway-introduction.md)’i, ağ katmanı yük dengelemesi için de [Load Balancer](../load-balancer/load-balancer-overview.md)’ı gözden geçirin. Uçtan uca senaryolarınızda bu çözümleri bir arada da kullanabilirsiniz.
>
> Azure yük dengeleme seçenekleri karşılaştırması için bkz. [Azure 'da Yük Dengeleme seçeneklerine genel bakış](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

Front Door aşağıdaki özellikleri içerir:

## <a name="accelerate-application-performance"></a>Uygulama performansını hızlandırma
Front Door, bölünmüş TCP tabanlı her noktaya yayın protokolü kullanarak son kullanıcılarınızın en yakın Front Door POP’a (Varlık Noktası) hemen bağlanmasını sağlar. Front Door POP’larından uygulamanızın arka uçlarına bağlantı için Microsoft’un genel ağı kullanıldığında, bir yandan performansı korurken, diğer yandan da daha yüksek kullanılabilirlik ve güvenilirlik elde edin. Arka ucunuza yönelik bu bağlantı aynı zamanda en düşük gecikme süresi tabanlıdır. [Bölünmüş TCP](front-door-routing-architecture.md#splittcp) ve [Her noktaya yayın protokolü](front-door-routing-architecture.md#anycast) gibi Front Door yönlendirme teknikleri hakkında bilgi edinin.

## <a name="increase-application-availability-with-smart-health-probes"></a>Akıllı durum yoklamalarıyla uygulama kullanılabilirliğini artırma

Ön kapı, akıllı sistem durumu araştırmalarını kullanarak kritik uygulamalarınız için yüksek oranda kullanılabilirlik sağlar, arka uçlarınızı hem gecikme hem de kullanılabilirlik için izler ve arka uç kaldığında hızlı otomatik yük devretme sağlar. Bu nedenle, kesinti yaşamadan uygulamalarınızda planlı bakım çalışmaları yürütebilirsiniz. Front Door bakım sürerken trafiği alternatif arka uçlara yönlendirir.

## <a name="url-based-routing"></a>URL tabanlı yönlendirme
URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL yollarına göre arka uç havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı arka uç havuzlarına yönlendirmektir.

Örneğin, `http://www.contoso.com/users/*` için istekler UserProfilePool’a ve `http://www.contoso.com/products/*` için istekler ProductInventoryPool’a yönlendirilir.  Front Door en iyi eşleşme algoritmasını kullanarak daha karmaşık yol eşleştirme senaryolarına bile olanak tanır. Dolayısıyla, yol desenlerinden hiçbiri eşleşmiyorsa `http://www.contoso.com/*` için varsayılan yönlendirme kuralınız seçilir ve trafik varsayılan tümünü yakalama yönlendirme kuralına yönlendirilir. [Yol Eşleştirme](front-door-route-matching.md) altında daha fazla bilgi edinebilirsiniz.

## <a name="multiple-site-hosting"></a>Birden çok site barındırma
Birden çok site barındırma, aynı Front Door yapılandırmasında birden fazla web sitesi yapılandırmanızı sağlar. Bu özellik, tek bir Front Door yapılandırmasına farklı web siteleri ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Uygulamanızın mimarisine bağlı olarak, her bir Web sitesini kendi arka uç havuzuna yönlendirmek için Azure ön kapısını yapılandırabilir veya aynı arka uç havuzuna yönlendirilmiş çeşitli web sitelerine sahip olursunuz. Örneğin, Front Door `images.contoso.com` ve `videos.contoso.com` için ImagePool ve VideoPool olarak bilinen iki arka uç havuzundan trafik hizmeti verebilir. Alternatif olarak, her iki ön uç konağını da trafiği MediaPool adlı tek bir arka uç havuzuna yönlendirmek üzere yapılandırabilirsiniz.

Benzer biçimde, aynı Front Door’da yapılandırılmış iki farklı etki alanınız (`www.contoso.com` ve `www.fabrikam.com`) olabilir.

## <a name="session-affinity"></a>Oturum benzeşimi
Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı uygulama arka ucunda tutmak istediğinizde kullanışlıdır. Front Door tarafından yönetilen tanımlama bilgilerini kullanarak, kullanıcı oturumundan gelen sonraki trafik işlenmek üzere aynı uygulama arka ucuna yönlendirilir. Bu özellik, bir kullanıcı oturumu için oturum durumunun arka uca yerel olarak kaydedildiği durumlarda önemlidir.

## <a name="tls-termination"></a>TLS sonlandırma
Ön kapı, en kenarda TLS sonlandırmasını destekler, bireysel kullanıcılar uygulama arka ucuna uzun mesafe bağlantıları üzerinden kurmak yerine ön kapı ortamlarıyla bir TLS bağlantısı ayarlayabilir. Front Door bunun yanı sıra Front Door ortamlarıyla arka uçlarınız arasında hem HTTP hem de HTTPS bağlantısını destekler. Bu nedenle, uçtan uca TLS şifrelemesini de ayarlayabilirsiniz. Örneğin, Front Door uygulama yükünüz için bir dakikada 5000'den fazla istek alırsa, etkin hizmetler için sıcak bağlantının yeniden kullanımından dolayı arka ucunuzla diyelim ki yalnızca 500 bağlantı kurar; bu şekilde arka uçlarınızın yükünü önemli ölçüde azaltır.

## <a name="custom-domains-and-certificate-management"></a>Özel etki alanları ve sertifika yönetimi
İçerik ulaştırmak için Front Door kullandığınızda, Front Door URL’nizde kendi etki alanı adınızın görünmesini istiyorsanız özel bir etki alanı gereklidir. Görünür bir etki alanınızın olması, müşterileriniz için kolaylık sağlar ve markalama için faydalıdır.
Front Door özel etki alanı adlarında HTTPS’yi de destekler. Bu özelliği, trafiğiniz için ön kapıya yönetilen sertifikaları seçerek veya kendi özel TLS/SSL sertifikanızı karşıya yükleyerek kullanın.

## <a name="application-layer-security"></a>Uygulama katmanı güvenliği
Azure ön kapısı, HTTP/HTTPS iş yükünüzü istemci IP adresleri, ülke kodu ve http parametrelerine göre kullanım açısından korumak için erişim denetimi için özel Web uygulaması güvenlik duvarı (WAF) kuralları yazmanıza izin verir. Buna ek olarak, Front Door kötü amaçlı bot trafiğiyle mücadele etmek için hız sınırlaması kuralları oluşturmanızı da sağlar. Web uygulaması güvenlik duvarı hakkında daha fazla bilgi için bkz. [Azure Web uygulaması güvenlik duvarı nedir?](../web-application-firewall/overview.md)

Front Door platformunun kendisi Temel [Azure DDoS Koruması](../virtual-network/ddos-protection-overview.md) ile korunur. Korumayı artırmak için, sanal ağlarınızda Azure DDoS Koruması Standart etkinleştirilebilir ve kaynakları ağ katmanı (TCP/UDP) saldırılarına karşı otomatik ayar ve risk azaltma yoluyla koruma altına alır. Front Door bir 7. katman ters ara sunucudur; web trafiğinin yalnızca arka uçlardan geçmesine izin verir ve diğer trafik türlerini varsayılan olarak engeller.

## <a name="url-redirection"></a>URL yeniden yönlendirme
Yalnızca güvenli iletişimi desteklemeye yönelik güçlü sektör gönderimi sayesinde, Web uygulamalarının HTTP trafiğinin otomatik olarak HTTPS 'ye yönlendirilmesini bekleniyor. Bu, kullanıcılar ve uygulama arasındaki tüm iletişimin şifreli bir yol üzerinde gerçekleşmesini sağlar. 

Geleneksel olarak, uygulama sahipleri, tek amacı HTTP üzerinde aldığı istekleri HTTPS 'ye yeniden yönlendirmek üzere olan adanmış bir hizmet oluşturarak bu gereksinimle ilgilendi. Azure ön kapısı, trafiği HTTP 'den HTTPS 'ye yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Azure ön kapıdan URL yeniden yönlendirme, tek başına HTTP ile, aynı zamanda farklı bir ana bilgisayar adına yeniden yönlendirme, farklı bir yola yönlendirme veya hatta URL 'de yeni bir sorgu dizesine yönlendirme için sınırlı değildir.

Daha fazla bilgi için bkz. Azure ön kapılı [trafiği yeniden yönlendirme](front-door-url-redirect.md) .

## <a name="url-rewrite"></a>URL yeniden yazma
Front Door, isteği arka uca yönlendirecek şekilde hazırlarken isteğe bağlı bir Özel İletme Yolu yapılandırmanıza olanak tanıyarak [URL yeniden yazma](front-door-url-rewrite.md) desteği sağlar. Front Door, istek arka ucunuza iletildiğinde gönderilecek Host üst bilgisini yapılandırmanıza da izin verir.

## <a name="protocol-support---ipv6-and-http2-traffic"></a>Protokol desteği - IPv6 ve HTTP/2 trafiği
Azure Front Door yerel olarak uçtan uca IPv6 bağlantısını ve HTTP/2 protokolünü destekler. 

HTTP/2 protokolü, uzun süre çalışan bir TCP bağlantısı üzerinden uygulama arka uçlarıyla bir istemci arasında tam çift yönlü iletişimi etkinleştirir. HTTP/2, arka uçla istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. HTTP/2 protokolü HTTP’den farklı olarak düşük yüke sahiptir ve kaynakların daha verimli kullanılması için aynı TCP bağlantısını birden fazla istek veya yanıt için kullanabilir. [Azure ön kapılarında http/2 desteği](front-door-http2.md)hakkında daha fazla bilgi edinin.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Front Door Fiyatlandırması](https://azure.microsoft.com/pricing/details/frontdoor/).

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
