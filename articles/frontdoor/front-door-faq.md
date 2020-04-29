---
title: Azure ön kapısı-sık sorulan sorular
description: Bu sayfa, Azure ön kapısı hakkında sık sorulan soruların yanıtlarını sağlar
services: frontdoor
documentationcenter: ''
author: sohamnchatterjee
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2020
ms.author: sohamnc
ms.openlocfilehash: e2785baab27f5bfc996b57607816062195a19b2b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81313765"
---
# <a name="frequently-asked-questions-for-azure-front-door"></a>Azure ön kapısı hakkında sık sorulan sorular

Bu makalede, Azure ön kapı özellikleri ve işlevselliği hakkında sık sorulan sorular yanıtlanmaktadır. Sorunuzun yanıtını görmüyorsanız, aşağıdaki kanallarla (yürüyen sırada) bizimle iletişim kurmanız gerekir:

1. Bu makalenin açıklamalar bölümü.
2. [Azure ön kapı UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Microsoft desteği:** Yeni bir destek isteği oluşturmak için, Azure portal **Yardım** sekmesinde **Yardım + Destek** düğmesini seçin ve ardından **Yeni destek isteği**' ni seçin.

## <a name="general"></a>Genel

### <a name="what-is-azure-front-door"></a>Azure Front Door nedir?

Azure ön kapısı, uygulamalarınız için çeşitli katman 7 yük dengeleme özellikleri sunan bir hizmet olarak Uygulama Teslim Ağı (ADN). Neredeyse gerçek zamanlı yük devretme ile genel yük dengeleme ile birlikte dinamik site hızlandırma (DSA) sağlar. Azure tarafından tam olarak yönetilen, yüksek oranda kullanılabilir ve ölçeklenebilir bir hizmettir.

### <a name="what-features-does-azure-front-door-support"></a>Azure ön kapısı hangi özellikleri destekler?

Azure ön kapısının dinamik site hızlandırma (DSA), TLS/SSL boşaltma ve uçtan uca TLS, Web uygulaması güvenlik duvarı, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı yönlendirme, ücretsiz sertifikalar ve birden çok etki alanı yönetimi ve diğerleri desteklenir. Desteklenen özelliklerin tam listesi için bkz. [Azure ön kapısına genel bakış](front-door-overview.md).

### <a name="what-is-the-difference-between-azure-front-door-and-azure-application-gateway"></a>Azure ön kapısı ve Azure Application Gateway arasındaki fark nedir?

Hem ön kapı hem de Application Gateway katman 7 (HTTP/HTTPS) yük dengeleyiciler olsa da, birincil fark ön kapısının küresel bir hizmettir, ancak Application Gateway bölgesel bir hizmettir. Ön kapı, bölgelere göre farklı ölçek birimleriniz/kümeleriniz/damga birimleri arasında yük dengeleyebilmesini sağlarken, Application Gateway sanal makinelerleriniz/kapsayıcılarınız arasında (ölçek birimi içinde) yük dengelemesi yapmanıza olanak sağlar.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Ön kapı arkasında Application Gateway ne zaman dağıtıyoruz?

Birinci kapıdan Application Gateway kullanılması gereken önemli senaryolar şunlardır:

- Ön kapı, yalnızca genel düzeyde yol tabanlı yük dengelemesi gerçekleştirebilir, ancak biri sanal ağı (VNET) içinde daha da fazla yük dengelemek isterse, Application Gateway kullanmaları gerekir.
- Ön kapı bir VM/kapsayıcı düzeyinde çalışmadığından bağlantı boşaltma yapılamaz. Ancak Application Gateway bağlantı boşaltma yapmanıza olanak sağlar. 
- AFD 'ın arkasındaki Application Gateway, biri %100 TLS/SSL yük boşaltma elde edebilir ve yalnızca kendi sanal ağı (VNET) içinde HTTP isteklerini yönlendirebilir.
- Ön kapı ve Application Gateway her ikisi de oturum benzeşimini destekler. Ön kapı sonraki trafiği bir kullanıcı oturumundan, belirli bir bölgedeki aynı kümeye veya arka uca yönlendirebilir. Application Gateway, trafiği küme içindeki aynı sunucuya yönlendirebilir.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Ön kapıdan Azure Load Balancer dağıtabilir.

Azure ön kapısının trafiği yönlendirmek için genel bir VIP veya genel olarak kullanılabilir bir DNS adı olması gerekir. Ön kapıdan Azure Load Balancer dağıtım yaygın kullanım durumdur.

### <a name="what-protocols-does-azure-front-door-support"></a>Azure ön kapısı hangi protokolleri destekler?

Azure ön kapısı, HTTP, HTTPS ve HTTP/2 destekler.

### <a name="how-does-azure-front-door-support-http2"></a>Azure ön kapısının HTTP/2 desteği nasıl desteklenir?

HTTP/2 protokolü desteği yalnızca Azure ön kapısına bağlanan istemciler tarafından kullanılabilir. Arka uç havuzundaki arka uçlarla iletişim HTTP/1.1 üzerinden yapılır. HTTP/2 desteği varsayılan olarak etkindir.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Bugün arka uç havuzunun bir parçası olarak hangi kaynaklar desteklenir?

Arka uç havuzları depolama, Web uygulaması, Kubernetes örnekleri veya ortak bağlantısı olan diğer özel ana bilgisayar adından oluşabilir. Azure ön kapısı, arka uçların genel bir IP veya genel olarak çözümlenebilen bir DNS ana bilgisayar aracılığıyla tanımlanmasını gerektirir. Arka uç havuzlarının üyeleri, genel bağlantı sahibi oldukları sürece bölgeler, bölgeler veya Azure dışında olabilir.

### <a name="what-regions-is-the-service-available-in"></a>Hizmet hangi bölgelerde kullanılabilir?

Azure ön kapısı küresel bir hizmettir ve belirli bir Azure bölgesine bağlı değildir. Ön kapı oluştururken belirtmeniz gereken tek konum, temel olarak kaynak grubu için meta verilerin depolanacağı yeri belirleyen kaynak grubu konumudur. Ön kapı kaynağı bir genel kaynak olarak oluşturulur ve yapılandırma tüm pop 'Lara (varlık noktası) genel olarak dağıtılır. 

### <a name="what-are-the-pop-locations-for-azure-front-door"></a>Azure ön kapısının POP konumları nelerdir?

Azure ön kapısının, Microsoft 'tan Azure CDN aynı POP (varlık noktası) konumları listesi vardır. Pop 'larımızın tüm listesi için, lütfen [Microsoft 'tan Azure CDN pop konumlarına](https://docs.microsoft.com/azure/cdn/cdn-pop-locations)başvurur.

### <a name="is-azure-front-door-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Azure ön kapısı, Uygulamam için adanmış bir dağıtım mi yoksa müşteriler arasında mi paylaşılıyor?

Azure ön kapısı, küresel olarak dağıtılmış çok kiracılı bir hizmettir. Bu nedenle, ön kapıya ait altyapı tüm müşterileri arasında paylaşılır. Bununla birlikte, ön kapılı bir profil oluşturarak uygulamanız için gerekli olan özel yapılandırmayı tanımlarsınız ve ön kapıda diğer ön kapı yapılandırmalarında hiçbir değişiklik yapılmaz.

### <a name="is-http-https-redirection-supported"></a>HTTP->HTTPS yeniden yönlendirmesi destekleniyor mu?

Evet. Azure ön kapısının yanı sıra ana bilgisayar, yol ve sorgu dizesi yeniden yönlendirmeyi ve URL yeniden yönlendirme 'nin bir parçasını destekler. [URL yeniden yönlendirme](front-door-url-redirect.md)hakkında daha fazla bilgi edinin. 

### <a name="in-what-order-are-routing-rules-processed"></a>Yönlendirme kuralları hangi sırada işlendi?

Ön kapılarınızın rotaları sıralı değildir ve en iyi eşleşme temelinde belirli bir rota seçilir. [Ön kapısının bir yönlendirme kuralına yönelik Istekleri nasıl eşleştirtiği](front-door-route-matching.md)hakkında daha fazla bilgi edinin.

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door"></a>Nasıl yaparım? arka ucuma erişimi yalnızca Azure ön kapısına kilitler mi?

Uygulamanızı yalnızca belirli ön kapıdan gelen trafiği kabul edecek şekilde kilitlemek için, arka ucunuz için IP ACL 'Lerini ayarlamanız ve ardından arka ucunuzdaki trafiği ön kapıya göre gönderilen ' X-Azure-FDıD ' üstbilgisinin belirli bir değeriyle kısıtlamanız gerekir. Bu adımlar aşağıdaki şekilde ayrıntılı olarak verilmiştir:

- Arka uçlarınızın IP adresini, Azure ön kapısından gelen trafiği ve yalnızca Azure 'un altyapı hizmetlerini kabul edecek şekilde yapılandırın. Arka ucunuzu hızlandırma için aşağıdaki IP ayrıntılarına bakın:
 
    - Ön kapıdaki IPv4 arka uç IP adresi aralığı için [Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com/download/details.aspx?id=56519) bölümündeki *azurefrontkapısı. arka* uç bölümüne bakın veya Ayrıca, [ağ güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) veya [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/service-tags)Ile *azurefrontkapısı. arka uç* hizmet etiketini de kullanabilirsiniz.
    - Hizmet etiketinde kapsanan ön kapıdaki **IPv6** arka uç IP alanı, Azure IP aralıkları json dosyasında listelenmez. Açık IPv6 adres aralığı arıyorsanız şu anda şu şekilde sınırlıdır`2a01:111:2050::/44`
    - Sanallaştırılmış ana bilgisayar IP adresleri aracılığıyla Azure 'un [temel altyapı hizmetleri](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) : `168.63.129.16` ve`169.254.169.254`

    > [!WARNING]
    > Ön kapısının arka uç IP alanı daha sonra değişebilir, ancak [Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com/download/details.aspx?id=56519)ile tümleştirilebilmemiz için bu işlem yapılmadan önce bu şekilde daha önce de olur. Herhangi bir değişiklik veya güncelleştirme için [Azure IP aralıklarına ve hizmet etiketlerine](https://www.microsoft.com/download/details.aspx?id=56519) abone olmanızı öneririz.

-    API sürümü `2020-01-01` veya üzeri Ile ön kapıda bir get işlemi gerçekleştirin. API çağrısında `frontdoorID` alan ara. '**X-Azure-FDıD**' gelen başlığına, ön kapıdan arka ucunuza, alanın `frontdoorID`değerine göre değer ile gönderilen filtre uygulayın. 

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Her noktaya yayın IP 'nin ön kapımın ömrü boyunca değiştirilmesini mi?

Ön kapılarınızın ön uç noktaya yayın IP 'si genellikle değişmemelidir ve ön kapısının ömrü boyunca statik kalabilir. Ancak, aynı için **garanti yoktur** . Lütfen IP üzerinde hiçbir doğrudan bağımlılık almaz.

### <a name="does-azure-front-door-support-static-or-dedicated-ips"></a>Azure ön kapısı statik veya ayrılmış IP 'Leri destekliyor mu?

Hayır, Azure ön kapısı şu anda statik veya adanmış ön uç her noktaya IP 'Leri desteklemiyor. 

### <a name="does-azure-front-door-support-x-forwarded-for-headers"></a>Azure ön kapısının x-iletilmiş-üstbilgiler için destek ister misiniz?

Evet, Azure ön kapısı X-Iletilen-for, X-Iletilmiş-Host ve X-Iletilmiş-proto üst bilgilerini destekler. X-Iletilen için-üst bilgi zaten mevcutsa, ön kapı istemci yuvasını IP 'ye ekler. Aksi takdirde, üst bilgiyi istemci yuva IP 'si ile ilgili değer olarak ekler. X-Iletilen-Host ve X-Iletilen-proto için değer geçersiz kılınır.

[Ön kapı desteklenen http üstbilgileri](front-door-http-headers-protocol.md)hakkında daha fazla bilgi edinin.  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-does-my-front-door-still-work-when-being-updated"></a>Azure ön kapısının dağıtılması ne kadar sürer? Ön kapım güncelleştirilirken çalışmaya devam ediyor mu?

Yeni bir ön kapı oluşturma veya var olan bir ön kapıdaki herhangi bir güncelleştirme, genel dağıtım için yaklaşık 3 ila 5 dakika sürer. Bu, yaklaşık 3 ila 5 dakika içinde, ön kapı yapılandırmanızın tüm pop 'larda küresel olarak dağıtılmasını sağlar.

Note-özel TLS/SSL sertifika güncelleştirmelerinin küresel olarak dağıtılması yaklaşık 30 dakika sürer.

Yolların veya arka uç havuzlarındaki veya her türlü güncelleştirme sorunsuz ve sıfır kapalı kalma süresine neden olur (yeni yapılandırma doğruysa). Sertifika güncelleştirmeleri de atomik olur ve ' AFD yönetilen ' değerinden ' kendi sertifikanızı kullanın ' veya bunun tersini yapmadan hiçbir kesinti olmaz.


## <a name="configuration"></a>Yapılandırma

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Azure ön kapısının yük dengelemesi yapabilir veya trafiği bir sanal ağ içinde yönlendirebilir mi?

Azure ön kapısı (AFD), trafiği yönlendirmek için genel IP veya genel olarak çözümlenebilen bir DNS adı gerektirir. Bu nedenle, yanıt hiçbir AFD doğrudan bir sanal ağ içinde yönlendirilemez, ancak arasında bir Application Gateway veya Azure Load Balancer kullanmak bu senaryoyu çözmeyecektir.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door"></a>Azure ön kapısının çeşitli zaman aşımları ve limitleri nelerdir?

[Azure ön kapısının tüm belgelenmiş zaman aşımları ve limitleri](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-front-door-service-limits)hakkında bilgi edinin.

## <a name="performance"></a>Performans

### <a name="how-does-azure-front-door-support-high-availability-and-scalability"></a>Azure ön kapısının yüksek kullanılabilirliği ve ölçeklenebilirliği nasıl destekler?

Azure ön kapısı, uygulamanızın ölçeklenebilirlik ihtiyaçlarını karşılamak için çok büyük hacimli kapasiteye sahip, küresel olarak dağıtılmış çok kiracılı bir platformdur. Microsoft 'un küresel ağı kenarından sunulan ön kapı, tüm uygulamanızı veya hatta tek tek mikro hizmetleri bölge veya farklı bulutlar üzerinde yük devredebilmeniz için genel yük dengeleme özelliği sunar.

## <a name="tls-configuration"></a>TLS yapılandırması

### <a name="what-tls-versions-are-supported-by-azure-front-door"></a>Azure ön kapısının hangi TLS sürümleri desteklenir?

Eylül 2019 ' den sonra oluşturulan tüm ön kapılı profiller varsayılan en düşük düzeyde TLS 1,2 ' i kullanır.

Ön kapı 1,0, 1,1 ve 1,2 TLS sürümlerini destekler. TLS 1,3 henüz desteklenmiyor.

### <a name="what-certificates-are-supported-on-azure-front-door"></a>Azure ön kapıda hangi sertifikalar destekleniyor?

HTTPS protokolünü bir ön kapı özel etki alanında güvenli bir şekilde teslim etmek üzere etkinleştirmek için, Azure ön kapısının yönettiği veya kendi sertifikanızı kullanan bir sertifikayı kullanmayı seçebilirsiniz.
Ön kapı yönetimli seçeneği, DigiCert aracılığıyla standart bir TLS/SSL sertifikası sağlar ve ön kapı Key Vault depolanır. Kendi sertifikanızı kullanmayı seçerseniz, desteklenen bir CA 'dan bir sertifika ekleyebilirsiniz ve standart bir TLS, genişletilmiş doğrulama sertifikası ya da bir joker karakter sertifikası olabilir. Otomatik olarak imzalanan sertifikalar desteklenmez. [Özel bir etki alanı IÇIN https 'yi etkinleştirmeyi](https://aka.ms/FrontDoorCustomDomainHTTPS)öğrenin.

### <a name="does-front-door-support-autorotation-of-certificates"></a>Ön kapı, sertifikaların oto döndürmesini destekliyor mu?

Ön kapı tarafından yönetilen sertifika seçeneği için sertifikalar ön kapıya göre oto döndürülür. Ön kapılı yönetilen bir sertifika kullanıyorsanız ve sertifikanın süre sonu tarihinin 60 günden daha az olduğunu görürseniz, bir destek bileti dosyası kullanın.
</br>Kendi özel TLS/SSL sertifikanız için, oto döndürme desteklenmez. Belirli bir özel etki alanı için ilk kez ayarlamaya benzer şekilde, ön kapıyı Key Vault doğru sertifika sürümüne işaret etmeniz ve ön kapıdaki hizmet sorumlusunun Key Vault hala erişime sahip olduğundan emin olmanız gerekir. Bu sertifika dağıtım işlemi ön kapıya göre güncelleştirilmiş olduğundan, sertifika için konu adı veya SAN sağlanmadığından hiçbir üretim etkisi olmaz.

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door"></a>Azure ön kapısının desteklediği geçerli şifre paketleri nelerdir?

TLS 1.2 için aşağıdaki şifre paketleri desteklenir

TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 TLS_DHE_RSA_WITH_AES_128_GCM_SHA256

TLS 1.0/1.1 özellikli özel etki alanları kullanılırken aşağıdaki şifre paketleri desteklenir:

- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384

### <a name="can-i-configure-tls-policy-to-control-tls-protocol-versions"></a>TLS protokolü sürümlerini denetlemek için TLS ilkesini yapılandırabilir miyim?

Azure ön kapıdaki en düşük TLS sürümünü, Azure portal veya [azure REST API](https://docs.microsoft.com/rest/api/frontdoorservice/frontdoor/frontdoors/createorupdate#minimumtlsversion)aracılığıyla özel etkı alanı https ayarlarında yapılandırabilirsiniz. Şu anda 1,0 ve 1,2 arasında seçim yapabilirsiniz.

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Ön kapıyı yalnızca belirli şifre paketlerini destekleyecek şekilde yapılandırabilir miyim?

Hayır, belirli şifre paketleri için ön kapıyı yapılandırma desteklenmiyor. Bununla birlikte, Sertifika yetkilinizden (Verisign, Entrust veya DigiCert) kendi özel TLS/SSL sertifikanızı alabilir ve oluşturduğunuz sırada sertifika üzerinde işaretlenmiş belirli şifre paketlerine sahip olabilirsiniz. 

### <a name="does-front-door-support-ocsp-stapling"></a>Ön kapı, OCSP zımbalamayı destekliyor mu?

Evet, OCSP zımbalama, ön kapıya göre varsayılan olarak desteklenir ve herhangi bir yapılandırma gerekmez.

### <a name="does-azure-front-door-also-support-re-encryption-of-traffic-to-the-backend"></a>Azure ön kapısının arka uca trafiği yeniden şifrelemeyi de destekliyor mu?

Evet, Azure ön kapısı TLS/SSL yük boşaltma 'yı destekler ve arka uca trafiği yeniden şifreler. Aslında, arka uca bağlantıların ortak IP 'si üzerinden gerçekleşmesi nedeniyle, ön Kapıınızı iletme protokolü olarak HTTPS kullanacak şekilde yapılandırmanız önerilir.

### <a name="does-front-door-support-self-signed-certificates-on-the-backend-for-https-connection"></a>Ön kapı, HTTPS bağlantısı için arka uçta otomatik olarak imzalanan sertifikaları destekliyor mu?

Hayır, kendinden imzalı sertifikalar ön kapıda desteklenmez ve kısıtlama her ikisi için de geçerlidir:

1. **Backends**: trafiği, önbelleğe alma etkin olan yönlendirme KURALLARı için HTTPS veya https sistem durumu araştırmaları olarak iletirken veya önbellekten çıkış kaynağı olarak doldurarak otomatik olarak imzalanan sertifikalar kullanamazsınız.
2. **Ön uç**: özel etkı alanında https 'yi etkinleştirmek için kendı özel TLS/SSL sertifikanızı kullanırken otomatik olarak imzalanan sertifikalar kullanamazsınız.

### <a name="why-is-https-traffic-to-my-backend-failing"></a>Arka ucumun HTTPS trafiği neden başarısız oluyor?

Arka ucunuza, sistem durumu araştırmalarının veya isteklerin iletilmesi için başarılı HTTPS bağlantıları sağlamak için, HTTPS trafiğinin başarısız olmasının iki nedeni olabilir:

1. **Sertifika konu adı uyumsuzluğu**: ön kapı, HTTPS bağlantıları için arka uç ana bilgisayar adı ile eşleşen GEÇERLI bir CA 'dan sertifika sunuyor. Örnek olarak, arka uç ana bilgisayar adı olarak ayarlanmışsa `myapp-centralus.contosonews.net` ve TLS el sıkışması sırasında arka ucunuzun temsil etmediği sertifika, ilgili `myapp-centralus.contosonews.net` ada `*myapp-centralus*.contosonews.net` sahip değilse, ön kapı bağlantıyı reddeder ve bir hatayla sonuçlanır. 
    1. **Çözüm**: bir uyumluluk açısından önerilmemekle karşı, ön kapılarınız için sertifika konu adı denetimini devre dışı bırakarak bu hatayı geçici olarak yapabilirsiniz. Bu, Azure portal ayarları altında ve API 'deki BackendPoolsSettings altında bulunur.
2. **GEÇERSIZ CA 'Dan arka uç barındırma sertifikası**: yalnızca [geçerli CA](/azure/frontdoor/front-door-troubleshoot-allowed-ca) 'Lardan sertifikalar, ön kapılı arka uçta kullanılabilir. İç CA 'Ların veya otomatik olarak imzalanan sertifikaların sertifikalara izin verilmez.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door"></a>Azure ön kapısının hangi türlerde ölçüm ve Günlükler vardır?

Günlükler ve diğer tanılama özellikleri hakkında bilgi için bkz. [ön kapıda izleme ölçümleri ve günlükleri](front-door-diagnostics.md).

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Tanılama günlüklerinde bekletme ilkesi nedir?

Tanılama günlükleri, müşteriler depolama hesabına akar ve müşteriler, saklama ilkesini tercihe göre ayarlayabilir. Tanılama günlükleri, bir olay hub 'ına veya Azure Izleyici günlüklerine de gönderilebilir. Daha fazla bilgi için bkz. [Azure ön kapı tanılama](front-door-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door"></a>Azure ön kapısına yönelik denetim günlüklerini almak Nasıl yaparım? mı?

Azure ön kapısına yönelik denetim günlükleri mevcuttur. Portalda, ön kapılarınızın menü dikey penceresinde **etkinlik günlüğü** ' ne tıklayarak denetim günlüğüne erişin. 

### <a name="can-i-set-alerts-with-azure-front-door"></a>Azure ön kapısına sahip uyarıları ayarlayabilir miyim?

Evet, Azure ön kapısı uyarıları destekler. Uyarılar, ölçümler üzerinde yapılandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
