---
title: 'Azure ön kapı: sık sorulan sorular'
description: Bu sayfa, Azure ön kapısının Standart/Premium hakkında sık sorulan soruların yanıtlarını sağlar.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: 6f6d71dec9726f009ab9a56e0a49ba21f5d218fd
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102181032"
---
# <a name="frequently-asked-questions-for-azure-front-door-standardpremium-preview"></a>Azure ön kapısı Standart/Premium (Önizleme) için sık sorulan sorular

Bu makalede, Azure ön kapı özellikleri ve işlevselliği hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="general"></a>Genel

### <a name="what-is-azure-front-door"></a>Azure Front Door nedir?

Azure ön kapısı, akıllı tehdit koruması olan hızlı, güvenilir ve güvenli bir modern bulut CDN 'dir. Akıllı tehdit koruması olan genel hiper ölçekli uygulamalarınız, API 'Ler, Web siteleri ve bulut hizmetleriniz için statik ve dinamik içerik hızlandırma, küresel yük dengelemesi ve gelişmiş güvenlik sağlar.

### <a name="what-features-does-azure-front-door-support"></a>Azure ön kapısı hangi özellikleri destekler?

Azure ön kapısı şunları destekler:

* Hem statik içerik hem de dinamik uygulama hızlandırma
* TLS/SSL boşaltma ve uçtan uca TLS
* Web Uygulaması Güvenlik Duvarı
* Tanımlama bilgisi tabanlı oturum benzeşimi
* URL yolu tabanlı yönlendirme
* Ücretsiz sertifikalar ve birden çok etki alanı yönetimi

Desteklenen özelliklerin tam listesi için bkz. [Azure ön kapısına genel bakış](overview.md).

### <a name="what-is-the-difference-between-azure-front-door-and-azure-application-gateway"></a>Azure ön kapısı ve Azure Application Gateway arasındaki fark nedir?

Hem ön kapı hem de Application Gateway katman 7 (HTTP/HTTPS) yük dengeleyiciler olsa da, birincil fark ön kapısının küresel bir hizmettir. Application Gateway bölgesel bir hizmettir. Ön kapı, bölgelere göre farklı ölçek birimleriniz/kümeleriniz/damga birimleri arasında yük dengeleyebilmesini sağlarken, Application Gateway ölçek birimi içindeki sanal makinelerleriniz/kapsayıcılarınız arasında yük dengelemenize olanak tanır.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Ön kapı arkasında Application Gateway ne zaman dağıtıyoruz?

Birinci kapıdan Application Gateway kullanılması gereken önemli senaryolar şunlardır:

* Ön kapı, yol tabanlı yük dengelemeyi yalnızca genel düzeyde yapabilir, ancak biri sanal ağı (VNET) içinde daha da fazla yük dengelemek istiyorsa, Application Gateway kullanmaları gerekir.
* Ön kapı bir VM/kapsayıcı düzeyinde çalışmadığından bağlantı boşaltma yapılamaz. Ancak Application Gateway bağlantı boşaltma yapmanıza olanak sağlar. 
* Ön kapıdan bir Application Gateway, biri %100 TLS/SSL yük boşaltma elde edebilir ve yalnızca kendi sanal ağı (VNET) içinde HTTP isteklerini yönlendirebilir.
* Ön kapı ve Application Gateway her ikisi de oturum benzeşimini destekler. Ön kapı, trafiği bir kullanıcı oturumundan, belirli bir bölgedeki aynı kümeye veya arka uca yönlendirebilir. Application Gateway, trafiği küme içindeki aynı sunucuya yönlendirebilir.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Ön kapıdan Azure Load Balancer dağıtabilir.

Azure ön kapısının trafiği yönlendirmek için genel bir VIP veya genel olarak kullanılabilir bir DNS adı olması gerekir. Ön kapıdan Azure Load Balancer dağıtım yaygın kullanım durumdur.

### <a name="what-protocols-does-azure-front-door-support"></a>Azure ön kapısı hangi protokolleri destekler?

Azure ön kapısı, HTTP, HTTPS ve HTTP/2 destekler.

### <a name="how-does-azure-front-door-support-http2"></a>Azure ön kapısının HTTP/2 desteği nasıl desteklenir?

HTTP/2 protokolü desteği yalnızca Azure ön kapısına bağlanan istemciler tarafından kullanılabilir. Arka uç havuzundaki arka uçlarla iletişim HTTP/1.1 üzerinden yapılır. HTTP/2 desteği varsayılan olarak etkindir.

### <a name="what-resources-are-supported-today-as-part-of-origin-group"></a>Kaynak grubunun parçası olarak bugün hangi kaynaklar desteklenir?

Kaynak grubu depolama, Web uygulaması, Kubernetes örnekleri veya ortak bağlantısı olan diğer özel ana bilgisayar adından oluşabilir. Azure ön kapısının kaynakları genel bir IP veya genel olarak çözümlenebilen bir DNS ana bilgisayar adı ile tanımlanmasını gerektirir. Kaynak grubunun üyeleri, genel bağlantı sahibi oldukları sürece bölgeler, bölgeler ve hatta Azure dışında olabilir.

### <a name="what-regions-is-the-service-available-in"></a>Hizmet hangi bölgelerde kullanılabilir?

Azure ön kapısı küresel bir hizmettir ve belirli bir Azure bölgesine bağlı değildir. Ön kapı oluştururken belirtmeniz gereken tek konum, kaynak grubu içindir. Bu konum temelde kaynak grubu için meta verilerin depolanacağı yeri belirtmektir. Ön kapı kaynağı bir genel kaynak olarak oluşturulur ve yapılandırma tüm pop 'Lara (varlık noktası) genel olarak dağıtılır. 

### <a name="what-are-the-pop-locations-for-azure-front-door"></a>Azure ön kapısının POP konumları nelerdir?

Azure ön kapısının, Microsoft 'tan Azure CDN aynı POP (varlık noktası) konumları listesi vardır. Pop 'larımızın tüm listesi için, lütfen [Microsoft 'tan Azure CDN pop konumlarına](../../cdn/cdn-pop-locations.md)başvurur.

### <a name="is-azure-front-door-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Azure ön kapısı, Uygulamam için adanmış bir dağıtım mi yoksa müşteriler arasında mi paylaşılıyor?

Azure ön kapısı, küresel olarak dağıtılmış çok kiracılı bir hizmettir. Ön kapı altyapısı tüm müşterileri arasında paylaşılır. Ön kapılı bir profil oluşturarak uygulamanız için gerekli olan belirli yapılandırmayı tanımlamanız gerekir. Ön kapıda yapılan değişiklikler diğer ön kapı yapılandırmasını etkilemez.

### <a name="is-http-https-redirection-supported"></a>HTTP->HTTPS yeniden yönlendirmesi destekleniyor mu?

Evet. Aslında Azure ön kapısı, ana bilgisayar, yol, sorgu dizesi yeniden yönlendirmeyi ve URL yeniden yönlendirmenin bir parçasını destekler. [URL yeniden yönlendirme](concept-rule-set-url-redirect-and-rewrite.md)hakkında daha fazla bilgi edinin. 

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door"></a>Nasıl yaparım? arka ucuma erişimi yalnızca Azure ön kapısına kilitler mi?

Uygulamanızı yalnızca belirli ön kapı örneğinden gelen trafiği kabul etmek için kilitlemek için en iyi yol, uygulamanızı özel uç nokta aracılığıyla yayımlamaktır. Ön kapı ve uygulama arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağı üzerinde bir özel bağlantıdan geçer ve bu da genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

[Özel bağlantıyla ön kapıda güvenli başlangıç kaynağı](concept-private-link.md)hakkında daha fazla bilgi edinin.  

Uygulamanızı yalnızca belirli ön kapıdan gelen trafiği kabul etmek için kilitlemek için alternatif yol, arka ucunuz için IP ACL 'Lerini ayarlamanız gerekir. Ardından arka ucunuzun trafiğini ön kapıdan gönderilen ' X-Azure-FDıD ' üstbilgisinin belirli bir değeriyle sınırlayın. Bu adımlar aşağıdaki şekilde ayrıntılı olarak verilmiştir:

* Arka uçlarınızın IP adresini, Azure ön kapısından gelen trafiği ve yalnızca Azure 'un altyapı hizmetlerini kabul edecek şekilde yapılandırın. Arka ucunuzu hızlandırma için aşağıdaki IP ayrıntılarına bakın:
 
    * Ön kapıdaki IPv4 arka uç IP adresi aralığı için [Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com/download/details.aspx?id=56519) bölümündeki *Azurefrontkapısı. arka uç* bölümüne bakın. Ayrıca, [ağ güvenlik gruplarında](../../virtual-network/network-security-groups-overview.md#security-rules) *Azurefrontkapısı. arka ucu* hizmet etiketini de kullanabilirsiniz.
    * Sanallaştırılmış konak IP adresleri aracılığıyla Azure 'un [temel altyapı hizmetleri](../../virtual-network/network-security-groups-overview.md#azure-platform-considerations) : `168.63.129.16` ve `169.254.169.254` .

    > [!WARNING]
    > Ön kapısının arka uç IP alanı daha sonra değişebilir, ancak [Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com/download/details.aspx?id=56519)ile tümleştirilebilmemiz için bu işlem yapılmadan önce bu şekilde daha önce de olur. Herhangi bir değişiklik veya güncelleştirme için [Azure IP aralıklarına ve hizmet etiketlerine](https://www.microsoft.com/download/details.aspx?id=56519) abone olmanızı öneririz.

* API sürümü veya üzeri olan ön kapıda bir GET işlemi yapın `2020-01-01` . API çağrısında `frontdoorID` alan ara. '**X-Azure-FDıD**' gelen başlığına, ön kapıdan, alanın değeri ile arka uca gönderilen filtre uygulayın `frontdoorID` . Ayrıca, `Front Door ID` ön kapı portalı sayfasından genel bakış bölümünde değeri de bulabilirsiniz. 

* Trafiği, ortaya çıkan ' X-Azure-FDıD ' üst bilgi değerine göre kısıtlamak için arka uç Web sunucunuza kural filtrelemesi uygulayın.

  İşte [Microsoft Internet Information Services (IIS)](https://www.iis.net/)için bir örnek:

    ``` xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
        <system.webServer>
            <rewrite>
                <rules>
                    <rule name="Filter_X-Azure-FDID" patternSyntax="Wildcard" stopProcessing="true">
                        <match url="*" />
                        <conditions>
                            <add input="{HTTP_X_AZURE_FDID}" pattern="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" negate="true" />
                        </conditions>
                        <action type="AbortRequest" />
                    </rule>
                </rules>
            </rewrite>
        </system.webServer>
    </configuration>
    ```

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Her noktaya yayın IP 'nin ön kapımın ömrü boyunca değiştirilmesini mi?

Ön kapılarınızın ön uç noktaya yayın IP 'si genellikle değişmemelidir ve ön kapısının ömrü boyunca statik kalabilir. Ancak, aynı için **garanti yoktur** . Burada, IP üzerinde hiçbir doğrudan bağımlılığı olmaz.

### <a name="does-azure-front-door-support-static-or-dedicated-ips"></a>Azure ön kapısı statik veya ayrılmış IP 'Leri destekliyor mu?

Hayır, Azure ön kapısı şu anda statik veya adanmış ön uç her noktaya IP 'Leri desteklemiyor. 

### <a name="does-azure-front-door-support-x-forwarded-for-headers"></a>Azure ön kapısının x-iletilmiş-üstbilgiler için destek ister misiniz?

Evet, Azure ön kapısı X-Iletilen-for, X-Iletilmiş-Host ve X-Iletilmiş-proto üst bilgilerini destekler. X-Iletilen için-üst bilgi zaten mevcutsa, ön kapı istemci yuvasını IP 'ye ekler. Aksi takdirde, üst bilgiyi istemci yuva IP 'si ile ilgili değer olarak ekler. X-Iletilen-Host ve X-Iletilen-proto için değer geçersiz kılınır.  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-does-my-front-door-still-work-when-being-updated"></a>Azure ön kapısının dağıtılması ne kadar sürer? Ön kapım güncelleştirilirken çalışmaya devam ediyor mu?

Yeni bir ön kapı oluşturma veya var olan bir ön kapıdaki herhangi bir güncelleştirme, genel dağıtım için yaklaşık 3 ila 5 dakika sürer. Bu, yaklaşık 3 ila 5 dakika içinde, ön kapı yapılandırmanızın tüm pop 'larda küresel olarak dağıtılmasını sağlar.

Note-özel TLS/SSL sertifika güncelleştirmelerinin küresel olarak dağıtılması yaklaşık 30 dakika sürer.

Yolların veya arka uç havuzlarındaki tüm güncelleştirmeler sorunsuz ve sıfır kapalı kalma süresine neden olur (yeni yapılandırma doğruysa). ' Azure ön kapısının yönetilmediği ' ' kendi sertifikanızı kullanın ' veya başka bir şekilde geçiş yapmadığınız takdirde sertifika güncelleştirmeleri herhangi bir kesinti oluşmasına neden olmaz.


## <a name="configuration"></a>Yapılandırma

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Azure ön kapısının yük dengelemesi yapabilir veya trafiği bir sanal ağ içinde yönlendirebilir mi?

Azure ön kapısı (AFD), trafiği yönlendirmek için genel bir IP veya genel olarak çözümlenebilen bir DNS adı gerektirir. Azure ön kapısı, bir sanal ağdaki kaynaklara doğrudan yönlendirilemiyor. Bu sorunu çözmek için bir Application Gateway veya genel IP ile Azure Load Balancer kullanabilirsiniz.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door"></a>Azure ön kapısının çeşitli zaman aşımları ve limitleri nelerdir?

[Azure ön kapısının tüm belgelenmiş zaman aşımları ve limitleri](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-front-door-service-limits)hakkında bilgi edinin.

### <a name="how-long-does-it-take-for-a-rule-to-take-effect-after-being-added-to-the-front-door-rules-engine"></a>Kural, ön kapı kuralları altyapısına eklendikten sonra etkili olması için ne kadar sürer?

Kural Altyapısı yapılandırması, bir güncelleştirmeyi tamamlamaya yaklaşık 10 ila 15 dakika sürer. Güncelleştirme tamamlandıktan hemen sonra kuralın etkin olmasını sağlayabilirsiniz. 

### <a name="can-i-configure-azure-cdn-behind-my-front-door-profile-or-front-door-behind-my-azure-cdn"></a>Ön kapı profilimi veya Azure CDN arkasındaki ön kapıyı Azure CDN yapılandırabilir miyim?

Her iki hizmet de isteklere yanıt vermediğinde aynı Azure Edge sitelerini kullandığından Azure ön kapısı ve Azure CDN birlikte yapılandırılamaz. 

## <a name="performance"></a>Performans

### <a name="how-does-azure-front-door-support-high-availability-and-scalability"></a>Azure ön kapısının yüksek kullanılabilirliği ve ölçeklenebilirliği nasıl destekler?

Azure ön kapısı, uygulamanızın ölçeklenebilirlik ihtiyaçlarını karşılamak için büyük miktarda kapasiteye sahip, küresel olarak dağıtılmış çok kiracılı bir platformdur. Microsoft 'un küresel ağı kenarından sunulan ön kapı, tüm uygulamanızı veya hatta tek tek mikro hizmetleri bölge veya farklı bulutlar üzerinde yük devredebilmeniz için genel yük dengeleme özelliği sunar.

## <a name="tls-configuration"></a>TLS yapılandırması

### <a name="what-tls-versions-are-supported-by-azure-front-door"></a>Azure ön kapısının hangi TLS sürümleri desteklenir?

Eylül 2019 ' den sonra oluşturulan tüm ön kapılı profiller varsayılan en düşük düzeyde TLS 1,2 ' i kullanır.

Ön kapı 1,0, 1,1 ve 1,2 TLS sürümlerini destekler. TLS 1,3 henüz desteklenmiyor.

### <a name="what-certificates-are-supported-on-azure-front-door"></a>Azure ön kapıda hangi sertifikalar destekleniyor?

Ön kapı özel etki alanında HTTPS protokolünü etkinleştirmek için, Azure ön kapısının yönettiği veya kendi sertifikanızı kullanan bir sertifikayı seçebilirsiniz.
Ön kapı yönetimli seçeneği, DigiCert aracılığıyla standart bir TLS/SSL sertifikası sağlar ve ön kapı Key Vault depolanır. Kendi sertifikanızı kullanmayı seçerseniz, desteklenen bir CA 'dan bir sertifika ekleyebilirsiniz ve standart bir TLS, genişletilmiş doğrulama sertifikası ya da bir joker karakter sertifikası olabilir. Otomatik olarak imzalanan sertifikalar desteklenmez.

### <a name="does-front-door-support-autorotation-of-certificates"></a>Ön kapı, sertifikaların oto döndürmesini destekliyor mu?

Ön kapı tarafından yönetilen sertifika seçeneği için sertifikalar ön kapıya göre oto döndürülür. Ön kapılı yönetilen bir sertifika kullanıyorsanız ve sertifikanın süre sonu tarihinin 60 günden daha az olduğunu görürseniz, bir destek bileti dosyası kullanın.

Kendi özel TLS/SSL sertifikanız için, oto döndürme desteklenmez. Belirli bir özel etki alanı için ilk kez ayarlamaya benzer şekilde, Key Vault ön kapıyı doğru sertifika sürümüne işaret etmeniz gerekir. Ön kapıdaki hizmet sorumlusunun Key Vault hala erişime sahip olduğundan emin olun. Ön kapıya göre güncelleştirilmiş sertifika dağıtım işlemi, sertifika için konu adı veya SAN değişmediğinden hiçbir üretim süresi yapılmasına neden olmaz.

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door"></a>Azure ön kapısının desteklediği geçerli şifre paketleri nelerdir?

TLS 1.2 için aşağıdaki şifre paketleri desteklenir: 

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256

TLS 1.0/1.1 özellikli özel etki alanlarını kullanma aşağıdaki şifre paketleri desteklenir:

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

Azure portal veya [azure REST API](/rest/api/frontdoorservice/frontdoor/frontdoors/createorupdate#minimumtlsversion)kullanarak, özel etkı alanı https ayarlarında Azure ön kapıda en düşük TLS sürümünü yapılandırabilirsiniz. Şu anda 1,0 ve 1,2 arasında seçim yapabilirsiniz.

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Ön kapıyı yalnızca belirli şifre paketlerini destekleyecek şekilde yapılandırabilir miyim?

Hayır, belirli şifre paketlerinin ön kapısını yapılandırma desteklenmiyor. Kendi özel TLS/SSL sertifikanızı, Sertifika yetkilinizden (Verisign, Entrust veya DigiCert) alabilirsiniz. Ardından, oluşturduğunuz sırada sertifika üzerinde işaretlenmiş belirli şifre paketlerine sahip olursunuz. 

### <a name="does-front-door-support-ocsp-stapling"></a>Ön kapı, OCSP zımbalamayı destekliyor mu?

Evet, OCSP zımbalama, ön kapıya göre varsayılan olarak desteklenir ve herhangi bir yapılandırma gerekmez.

### <a name="does-azure-front-door-also-support-re-encryption-of-traffic-to-the-backend"></a>Azure ön kapısının arka uca trafiği yeniden şifrelemeyi de destekliyor mu?

Evet, Azure ön kapısı TLS/SSL yük boşaltma ve uçtan uca TLS 'yi destekler ve bu da arka uca trafiği yeniden şifreler. Arka uca bağlantı genel IP üzerinden gerçekleştiğinden, ön kapıyı iletme protokolü olarak HTTPS kullanacak şekilde yapılandırmanız önerilir.

### <a name="does-front-door-support-self-signed-certificates-on-the-backend-for-https-connection"></a>Ön kapı, HTTPS bağlantısı için arka uçta otomatik olarak imzalanan sertifikaları destekliyor mu?

Hayır, kendinden imzalı sertifikalar ön kapıda desteklenmez ve kısıtlama her ikisi için de geçerlidir:

* Arka **uçlar**: trafiği, önbelleğe alma etkin olan yönlendirme kuralları için, trafiği HTTPS veya https durum araştırmaları olarak iletirken veya önbellekten çıkış kaynağı olarak doldurarak otomatik olarak imzalanan sertifikalar kullanamazsınız.
* **Ön uç**: özel etkı alanında https 'yi etkinleştirmek için kendı özel TLS/SSL sertifikanızı kullanırken otomatik olarak imzalanan sertifikalar kullanamazsınız.

### <a name="why-is-https-traffic-to-my-backend-failing"></a>Arka ucumun HTTPS trafiği neden başarısız oluyor?

Arka ucunuza, sistem durumu araştırmalarının veya isteklerin iletilmesi için başarılı HTTPS bağlantıları sağlamak için, HTTPS trafiğinin başarısız olmasının iki nedeni olabilir:

* **Sertifika konu adı uyumsuzluğu**: ön kapı, HTTPS bağlantıları için arka uç ana bilgisayar adı ile eşleşen GEÇERLI bir CA 'dan sertifika sunuyor. Örnek olarak, arka uç ana bilgisayar adı olarak ayarlanır `myapp-centralus.contosonews.net` ve arka UCUNUZUN TLS el sıkışması sırasında sunduğu sertifika, `myapp-centralus.contosonews.net` konu adına veya bu `*myapp-centralus*.contosonews.net` adı vermez. Ardından ön kapı bağlantıyı reddeder ve hataya neden olur. 
    * **Çözüm**: uyumluluk açısından önerilmez, ancak ön kapılarınız için sertifika konu adı denetimini devre dışı bırakarak bu hatayı çözebilirsiniz. Bu seçeneği, Azure portal ayarlar altında ve API 'de BackendPoolsSettings altında bulabilirsiniz.
* **GEÇERSIZ CA 'Dan arka uç barındırma sertifikası**: yalnızca [geçerli sertifika yetkililerinden](troubleshoot-allowed-certificate-authority.md) sertifikalar, ön kapılı arka uçta kullanılabilir. İç CA 'Ların veya otomatik olarak imzalanan sertifikaların sertifikalara izin verilmez.

### <a name="can-i-use-clientmutual-authentication-with-azure-front-door"></a>Azure ön kapılı istemci/karşılıklı kimlik doğrulaması kullanabilir miyim?

Hayır. Azure ön kapısı, [RFC 5246](https://tools.ietf.org/html/rfc5246)' de istemci/karşılıklı kimlik doğrulaması sunan TLS 1,2 ' i desteklese de şu anda Azure ön kapısının istemci/karşılıklı kimlik doğrulaması desteği yoktur.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door"></a>Azure ön kapısının hangi türlerde ölçüm ve Günlükler vardır?

Günlükler ve diğer tanılama özellikleri hakkında bilgi için bkz. ön kapıda Izleme ölçümleri ve günlükleri.

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Tanılama günlüklerinde bekletme ilkesi nedir?

Tanılama günlükleri, müşteriler depolama hesabına akar ve müşteriler, saklama ilkesini tercihe göre ayarlayabilir. Tanılama günlükleri, bir olay hub 'ına veya Azure Izleyici günlüklerine de gönderilebilir. Daha fazla bilgi için bkz. [Azure ön kapı günlüğü](how-to-logs.md).

### <a name="how-do-i-get-audit-logs-for-azure-front-door"></a>Azure ön kapısına yönelik denetim günlüklerini almak Nasıl yaparım? mı?

Azure ön kapısına yönelik denetim günlükleri mevcuttur. Portalda, ön kapılarınızın menü sayfasında **etkinlik günlüğü** ' nü seçerek denetim günlüğüne erişin. 

### <a name="can-i-set-alerts-with-azure-front-door"></a>Azure ön kapısına sahip uyarıları ayarlayabilir miyim?

Evet, Azure ön kapısı uyarıları destekler. Uyarılar, ölçümler üzerinde yapılandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

[Ön kapı Standart/Premium oluşturmayı](create-front-door-portal.md)öğrenin.
