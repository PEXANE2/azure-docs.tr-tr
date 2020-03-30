---
title: Azure Ön Kapı - Sık Sorulan Sorular
description: Bu sayfa, Azure Ön Kapı hakkında sık sorulan soruların yanıtlarını sağlar
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2019
ms.author: sharadag
ms.openlocfilehash: 1cfee9749bf2eb30799efb05ac875843bcde6651
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372627"
---
# <a name="frequently-asked-questions-for-azure-front-door"></a>Azure Ön Kapı için sık sorulan sorular

Bu makalede, Azure Ön Kapı özellikleri ve işlevleri yle ilgili sık sorulan sorular yanıtlanmaktadır. Sorunuzun cevabını göremiyorsanız, aşağıdaki kanallardan (artan sırada) bizimle iletişime geçebilirsiniz:

1. Bu makalenin yorum bölümü.
2. [Azure Ön Kapı UserVoice](https://feedback.azure.com/forums/217313-networking?category_id=345025).
3. **Microsoft Desteği:** **Azure portalında, Yardım** sekmesinde yeni bir destek isteği oluşturmak için Yardım + **destek** düğmesini seçin ve ardından Yeni **destek isteğini**seçin.

## <a name="general"></a>Genel

### <a name="what-is-azure-front-door"></a>Azure Front Door nedir?

Azure Ön Kapı, uygulamalarınız için çeşitli katman 7 yük dengeleme özellikleri sunan bir Uygulama Dağıtım Ağıdır (ADN). Dinamik alan ivmesi (DSA) ve neredeyse gerçek zamanlı arıza ile küresel yük dengeleme sağlar. Azure tarafından tamamen yönetilen yüksek kullanılabilir ve ölçeklenebilir bir hizmettir.

### <a name="what-features-does-azure-front-door-support"></a>Azure Ön Kapı hangi özellikleri destekler?

Azure Ön Kapı dinamik site ivmesini (DSA), SSL boşaltma ve bitiş sonuna Kadar SSL, Web Application Güvenlik Duvarı, çerez tabanlı oturum yakınlığı, url yol tabanlı yönlendirme, ücretsiz sertifikalar ve birden çok etki alanı yönetimi ve diğerleri destekler. Desteklenen özelliklerin tam listesi için Azure [Ön Kapısı'na Genel Bakış'a](front-door-overview.md)bakın.

### <a name="what-is-the-difference-between-azure-front-door-and-azure-application-gateway"></a>Azure Ön Kapı ile Azure Uygulama Ağ Geçidi arasındaki fark nedir?

Hem Ön Kapı hem de Uygulama Ağ Geçidi katman 7 (HTTP/HTTPS) yük dengeleyicileri olsa da, birincil fark, Ön Kapı'nın küresel bir hizmet, uygulama ağ geçidinin ise bölgesel bir hizmet olmasıdır. Ön Kapı bölgeler arasında farklı ölçek birimleriniz/kümeleriniz/damga birimleriniz arasındaki dengeyi yükleyebilirken, Application Gateway ölçek birimi içinde bulunan VM'leriniz/kaplarınız vb. arasındaki dengeyi yüklemenize olanak tanır.

### <a name="when-should-we-deploy-an-application-gateway-behind-front-door"></a>Ön Kapının arkasına ne zaman uygulama ağ geçidi konuşlandırmalıyız?

Ön Kapı arkasında Uygulama Ağ Geçidi kullanmanız gereken temel senaryolar şunlardır:

- Ön Kapı sadece küresel düzeyde yol tabanlı yük dengeleme gerçekleştirebilir ancak sanal ağ (VNET) içinde denge trafiğini daha da yüklemek istiyorsa, uygulama ağ geçidini kullanmalıdır.
- Ön Kapı VM/konteyner düzeyinde çalışmadığından, Bağlantı Boşaltma yapamaz. Ancak, Uygulama Ağ Geçidi Bağlantı Boşaltma yapmanızı sağlar. 
- AFD'nin arkasındaki Uygulama Ağ Geçidi ile %100 SSL boşaltma elde edilebilir ve sanal ağları (VNET) içinde yalnızca HTTP isteklerini yönlendirebilirsiniz.
- Ön Kapı ve Uygulama Ağ Geçidi hem destek oturum afinite. Ön Kapı, kullanıcı oturumundan sonraki trafiği aynı kümeye veya belirli bir bölgeye yönlendirebilirken, Application Gateway trafiği küme içindeki aynı sunucuya yönlendirebilir.  

### <a name="can-we-deploy-azure-load-balancer-behind-front-door"></a>Azure Yük Bakiyesi'ni Ön Kapının arkasına dağıtabilir miyiz?

Azure Ön Kapı'nın trafiği yönlendirmek için herkese açık bir VIP'ye veya herkese açık bir DNS adına ihtiyacı vardır. Ön Kapı arkasında bir Azure Yük Dengeleyicisi dağıtmak yaygın bir kullanım örneğidir.

### <a name="what-protocols-does-azure-front-door-support"></a>Azure Ön Kapı hangi protokolleri destekler?

Azure Ön Kapı HTTP, HTTPS ve HTTP/2'yi destekler.

### <a name="how-does-azure-front-door-support-http2"></a>Azure Ön Kapı HTTP/2'yi nasıl destekler?

HTTP/2 protokol desteği yalnızca Azure Ön Kapı'ya bağlanan istemciler tarafından kullanılabilir. Arka uç havuzunda arka uçlara iletişim HTTP/1.1 üzerindedir. HTTP/2 desteği varsayılan olarak etkinleştirilir.

### <a name="what-resources-are-supported-today-as-part-of-backend-pool"></a>Arka uç havuzunun bir parçası olarak bugün hangi kaynaklar desteklenmektedir?

Arka uç havuzları Depolama, Web Uygulaması, Kubernetes örneklerinden veya genel bağlantıya sahip diğer özel ana bilgisayar adlarından oluşabilir. Azure Ön Kapı, arka uçların genel bir IP veya genel olarak çözülebilir bir DNS ana bilgisayar adı aracılığıyla tanımlanmasını gerektirir. Arka uç havuzlarının üyeleri, genel bağlantıya sahip oldukları sürece bölgeler, bölgeler ve hatta Azure dışında olabilir.

### <a name="what-regions-is-the-service-available-in"></a>Hizmet hangi bölgelerde kullanılabilir?

Azure Ön Kapı genel bir hizmettir ve belirli bir Azure bölgesine bağlı değildir. Ön Kapı oluştururken belirtmeniz gereken tek konum, kaynak grubu için meta verilerin nerede depolanacağını temel olarak belirten kaynak grubu konumudur. Ön Kapı kaynağının kendisi küresel bir kaynak olarak oluşturulur ve yapılandırma tüm POP'lara (Varoluş Noktası) genel olarak dağıtılır. 

### <a name="what-are-the-pop-locations-for-azure-front-door"></a>Azure Ön Kapı için POP konumları nelerdir?

Azure Ön Kapı, Microsoft'tan gelen Azure CDN ile aynı POP (Point of Presence) konumları listesine sahiptir. POP'larımızın tam listesi [için, Microsoft'un Azure CDN POP konumlarını](https://docs.microsoft.com/azure/cdn/cdn-pop-locations)lütfen incelikle incebaşvurun.

### <a name="is-azure-front-door-a-dedicated-deployment-for-my-application-or-is-it-shared-across-customers"></a>Azure Ön Kapı, uygulamam için özel bir dağıtım mı yoksa müşteriler arasında mı paylaşılıyor?

Azure Ön Kapı, dünya çapında dağıtılan çok kiracılı bir hizmettir. Bu nedenle, Ön Kapı altyapısı tüm müşterileri arasında paylaşılır. Ancak, bir Ön Kapı profili oluşturarak, uygulamanız için gereken özel yapılandırmayı tanımlarsınız ve Ön Kapınızda yapılan hiçbir değişiklik diğer Ön Kapı yapılandırmalarını etkilemez.

### <a name="is-http-https-redirection-supported"></a>HTTP->HTTPS yeniden yönlendirmesi desteklendi mi?

Evet. Aslında, Azure Ön Kapı ana bilgisayar, yol ve sorgu dize yönlendirmesinin yanı sıra URL yeniden yönlendirmesinin bir parçasını destekler. [URL yeniden yönlendirmesi](front-door-url-redirect.md)hakkında daha fazla bilgi edinin. 

### <a name="in-what-order-are-routing-rules-processed"></a>Yönlendirme kuralları hangi sırayla işlenir?

Ön Kapınız için rotalar sipariş edilmez ve en iyi eşleşmeye göre belirli bir rota seçilir. Ön Kapı istekleri yönlendirme [kuralıyla nasıl eşleştme](front-door-route-matching.md)hakkında daha fazla bilgi edinin.

### <a name="how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door"></a>Arka ucuma erişimi yalnızca Azure Ön Kapı'ya nasıl kilitlerim?

Yalnızca belirli Ön Kapınızdan trafiği kabul etmek için başvurunuzu kilitlemek için arka uçiçin IP ALA'ları ayarlamanız ve ardından arka ponponunızdaki trafiği Ön Kapı tarafından gönderilen üstbilginin 'X-Azure-FDID' başlığının belirli değeriyle sınırlamanız gerekir. Bu adımlar aşağıdaki gibi ayrıntılı olarak açıklanmıştır:

- Arka uçlarınız için IP YAPıŞTıRmayı yapılandırın ve yalnızca Azure Ön Kapı'nın arka uç IP adres alanından ve Azure'un altyapı hizmetlerinden gelen trafiği kabul edin. Arka uçunuzu karıştırmak için aşağıdaki IP ayrıntılarına bakın:
 
    - Ön Kapının IPv4 arka uç IP adresi aralığı için [Azure IP Aralıkları ve Hizmet Etiketleri](https://www.microsoft.com/download/details.aspx?id=56519) bölümünde *AzureFrontDoor.Backend* bölümüne bakın veya [ağ güvenlik gruplarınızda](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) veya [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/service-tags)ile *AzureFrontDoor.Backend* hizmet etiketini de kullanabilirsiniz.
    - Servis etiketinde kaplanmış ken Front Door'un **IPv6** arka uç IP alanı, Azure IP aralıkları JSON dosyasında listelenmez. Açık IPv6 adres aralığı arıyorsanız, şu anda`2a01:111:2050::/44`
    - Azure'un sanallaştırılmış ana bilgisayar IP adresleri `168.63.129.16` aracılığıyla temel altyapı [hizmetleri:](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) ve`169.254.169.254`

    > [!WARNING]
    > Front Door'un arka uç IP alanı daha sonra değişebilir, ancak bu olmadan önce [Azure IP Aralıkları ve Hizmet Etiketleri](https://www.microsoft.com/download/details.aspx?id=56519)ile entegre olacağımızı garanti edeceğiz. Herhangi bir değişiklik veya güncelleme için [Azure IP Aralıkları ve Hizmet](https://www.microsoft.com/download/details.aspx?id=56519) Etiketleri'ne abone olmamızı öneririz.

-    API sürümü `2020-01-01` veya daha yüksek olan Ön Kapınızda GET işlemi gerçekleştirin. API aramasında, alan `frontdoorID` arayın. Gelen üstbilgideki '**X-Azure-FDID**' i ön kapı ile arka uca gönderilen `frontdoorID`üstbilgideki filtreleme, alanın değeri yle birlikte. 

### <a name="can-the-anycast-ip-change-over-the-lifetime-of-my-front-door"></a>Herhangi bir IP benim Ön Kapı ömrü boyunca değişebilir mi?

Ön Kapınızın ön uç anycast IP'si genellikle değişmemelidir ve Ön Kapının ömrü boyunca statik kalabilir. Ancak, aynı garanti **yoktur.** Lütfen IP üzerinde herhangi bir doğrudan bağımlılık almayın.

### <a name="does-azure-front-door-support-static-or-dedicated-ips"></a>Azure Ön Kapı statik veya özel IP'leri destekliyor mu?

Hayır, Azure Ön Kapı şu anda statik veya özel ön uç herhangi bir ip'leri desteklemez. 

### <a name="does-azure-front-door-support-x-forwarded-for-headers"></a>Azure Ön Kapı x iletili üstbilgi desteği veriyor mu?

Evet, Azure Ön Kapı X-Forwarded-For, X-Forwarded-Host ve X-Forwarded-Proto üstlelerini destekler. X-Forwarded-For için başlık zaten mevcutsa, Ön Kapı istemci soket IP'sini ona ekler. Aksi takdirde, değer olarak istemci soket IP ile üstbilgi ekler. X-Forwarded-Host ve X-Forwarded-Proto için değer geçersiz kılınır.

Ön Kapı [desteklenen HTTP başlıkları](front-door-http-headers-protocol.md)hakkında daha fazla bilgi edinin.  

### <a name="how-long-does-it-take-to-deploy-an-azure-front-door-does-my-front-door-still-work-when-being-updated"></a>Azure Ön Kapı'nın dağıtılması ne kadar sürer? Güncellenirken Ön Kapım hala çalışıyor mu?

Yeni bir Ön Kapı oluşturma veya varolan bir Ön Kapı herhangi bir güncelleştirme küresel dağıtım için yaklaşık 3 ila 5 dakika sürer. Bu, yaklaşık 3 ila 5 dakika içinde Ön Kapı yapılandırmanızın tüm KÜRESEL POP'larımıza dağıtılacayaçalışacağı anlamına gelir.

Not - Özel SSL sertifika güncelleştirmelerinin genel olarak dağıtılması yaklaşık 30 dakika sürer.

Rotalar veya arka uç havuzları vb. için yapılan tüm güncellemeler sorunsuzdur ve (yeni yapılandırma doğruysa) sıfır kapalı kalma süresine neden olur. Sertifika güncelleştirmeleri de atomiktir ve 'AFD Yönetilen'den 'Kendi sertifikanızı kullanın' veya tam tersi bir geçiş yapmadıkça herhangi bir kesintiye neden olmaz.


## <a name="configuration"></a>Yapılandırma

### <a name="can-azure-front-door-load-balance-or-route-traffic-within-a-virtual-network"></a>Azure Ön Kapı sanal ağdaki trafiği yükleyebilir veya yönlendirebilir mi?

Azure Ön Kapı (AFD), trafiği yönlendirmek için genel bir IP veya genel olarak çözülebilir bir DNS adı gerektirir. Bu nedenle, cevap hiçbir AFD doğrudan bir sanal ağ içinde yönlendiremez, ancak arada bir Uygulama Ağ Geçidi veya Azure Yük Dengeleyici kullanarak bu senaryoyu çözecektir.

### <a name="what-are-the-various-timeouts-and-limits-for-azure-front-door"></a>Azure Ön Kapı için çeşitli zaman zaman ları ve sınırlamaları nelerdir?

Azure Ön Kapı için belgelenen tüm [zaman zaman ları ve sınırlamaları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-front-door-service-limits)hakkında bilgi edinin.

## <a name="performance"></a>Performans

### <a name="how-does-azure-front-door-support-high-availability-and-scalability"></a>Azure Ön Kapı yüksek kullanılabilirliği ve ölçeklenebilirliği nasıl destekler?

Azure Ön Kapı, uygulamanızın ölçeklenebilirlik gereksinimlerini karşılamak için büyük hacimlerde kapasiteye sahip, küresel olarak dağıtılan çok kiracılı bir platformdur. Microsoft'un küresel ağının kenarından teslim edilen Front Door, tüm uygulamanız ve bölgeler veya farklı bulutlar arasında tek tek mikro hizmetler üzerinde başarısız olmamanızı sağlayan küresel yük dengeleme özelliği sağlar.

## <a name="ssl-configuration"></a>SSL yapılandırması

### <a name="what-tls-versions-are-supported-by-azure-front-door"></a>Azure Ön Kapı tarafından hangi TLS sürümleri desteklenir?

Eylül 2019'dan sonra oluşturulan tüm Ön Kapı profilleri varsayılan minimum tls 1.2'yi kullanır.

Ön Kapı TLS versiyonları 1.0, 1.1 ve 1.2 destekler. TLS 1.3 henüz desteklenmedi.

### <a name="what-certificates-are-supported-on-azure-front-door"></a>Azure Ön Kapı'da hangi sertifikalar desteklenir?

Ön Kapı özel etki alanında güvenli bir şekilde içerik sunmak için HTTPS protokolünü etkinleştirmek için Azure Ön Kapı tarafından yönetilen bir sertifika yı veya kendi sertifikanızı kullanmayı seçebilirsiniz.
Ön Kapı yönetilen seçenek, Digicert üzerinden standart bir SSL sertifikası sunar ve Front Door'un Anahtar Kasası'nda saklanır. Kendi sertifikanızı kullanmayı seçerseniz, desteklenen bir CA'dan bir sertifikaya binebilirsiniz ve standart bir SSL, genişletilmiş doğrulama sertifikası ve hatta joker karakter sertifikası olabilir. Kendi imzalı sertifikalar desteklenmez. [Özel bir etki alanı için HTTPS'yi nasıl etkinleştireceklerini](https://aka.ms/FrontDoorCustomDomainHTTPS)öğrenin.

### <a name="does-front-door-support-autorotation-of-certificates"></a>Ön Kapı sertifikaların otomatik olarak döndürülmesini destekliyor mu?

Ön Kapı yönetilen sertifika seçeneği için, sertifikalar Ön Kapı tarafından otomatik olarak döndürülür. Bir Ön Kapı yönetilen sertifikası kullanıyorsanız ve sertifikanın son kullanma tarihinin 60 günden az olduğunu görüyorsanız, bir destek bileti gönderin.
</br>Kendi özel SSL sertifikanız için otomatik döndürme desteklenmez. Belirli bir özel etki alanı için ilk kez nasıl ayarlandığının benzer şekilde, Ön Kapı'yı Key Vault'unuzdadoğru sertifika sürümüne doğru şekilde ayarlamanız ve Ön Kapı servis müdürünün hala Key Vault'a erişebilmesini sağlamanız gerekir. Front Door tarafından güncelleştirilen bu güncelleştirilmiş sertifika kullanıma alma işlemi atomiktir ve sertifikanın özne adı veya SAN'ın değişmemesi koşuluyla herhangi bir üretim etkisine neden olmaz.

### <a name="what-are-the-current-cipher-suites-supported-by-azure-front-door"></a>Azure Ön Kapı tarafından desteklenen mevcut şifreleme paketleri nelerdir?

Azure Ön Kapı tarafından desteklenen mevcut şifreleme paketleri şunlardır:

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

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>SSL Protokolü sürümlerini denetlemek için SSL ilkesini yapılandırabilir miyim?

Azure Portalı veya [Azure REST API](https://docs.microsoft.com/rest/api/frontdoorservice/frontdoor/frontdoors/createorupdate#minimumtlsversion)üzerinden özel etki alanı HTTPS ayarlarında Azure Ön Kapı'da minimum TLS sürümünü yapılandırabilirsiniz. Şu anda, 1.0 ve 1.2 arasında seçim yapabilirsiniz.

### <a name="can-i-configure-front-door-to-only-support-specific-cipher-suites"></a>Front Door'u yalnızca belirli şifre süitlerini destekleyecek şekilde yapılandırabilir miyim?

Hayır, belirli şifre paketleri için Ön Kapı yapılandırma sı desteklenmez. Ancak, Sertifika Yetkilisinizden kendi özel SSL sertifikanızı (Verisign, Entrust veya Digicert deyin) alabilir ve sertifikayı oluşturduğunuzda belirli şifre paketleriişaretleyebilirsiniz. 

### <a name="does-front-door-support-ocsp-stapling"></a>Ön Kapı OCSP zımbalatması destekliyor mu?

Evet, OCSP zımbalaması Varsayılan olarak Ön Kapı tarafından desteklenir ve yapılandırma gerekmez.

### <a name="does-azure-front-door-also-support-re-encryption-of-traffic-to-the-backend"></a>Azure Ön Kapı, trafiğin arka uçta yeniden şifrelemesini de destekliyor mu?

Evet, Azure Ön Kapı SSL boşaltmayı destekler ve trafiği arka uca yeniden şifreleyen SSL'yi uçtan uca destekler. Aslında, arka uç bağlantıları genel IP üzerinden gerçekleştiğinden, ön kapınızı https'yi yönlendirme protokolü olarak kullanacak şekilde yapılandırmanız önerilir.

### <a name="does-front-door-support-self-signed-certificates-on-the-backend-for-https-connection"></a>Front Door, HTTPS bağlantısı için arka uçta kendi imzalı sertifikaları destekliyor mu?

Hayır, kendi imzalı sertifikalar Ön Kapı'da desteklenmez ve kısıtlama her ikisi için de geçerlidir:

1. **Arka Uçlar**: Trafiği HTTPS veya HTTPS sistem durumu sondası olarak iletirken veya önbelleğe alma kuralları önbelleğe alınmış olarak yönlendirme kuralları için önbelleği kaynaktan doldururken kendi imzalı sertifikaları kullanamazsınız.
2. **Frontend**: Özel etki alanınızda HTTPS'yi etkinleştirmek için kendi özel SSL sertifikanızı kullanırken kendi imzalı sertifikaları kullanamazsınız.

### <a name="why-is-https-traffic-to-my-backend-failing"></a>Arka uça https trafiği neden başarısız oluyor?

İster sistem sondaları ister yönlendirme istekleri için olsun, arka uca başarılı HTTPS bağlantılarına sahip olmak için, HTTPS trafiğinin başarısız olmasının iki nedeni olabilir:

1. **Sertifika konusu adı uyuşmazlığı**: HTTPS bağlantıları için, Ön Kapı arka uçünüzün arka uç ana bilgisayar adı ile eşleşen konu adı(lar) içeren geçerli bir CA'dan sertifika sunmasını bekler. Örnek olarak, arka uç ana adınızı `myapp-centralus.contosonews.net` n için ayarlanmışsa ve SSL el sıkışması `*myapp-centralus*.contosonews.net` sırasında arka uçünüzün sunduğu sertifikada ne konu adı varsa, `myapp-centralus.contosonews.net` Ön Kapı bağlantıyı reddeder ve bir hataya neden olur. 
    1. **Çözüm**: Uyumluluk açısından önerilmese de, Ön Kapınız için sertifika özne adı denetimini devre dışı bırakarak bu hatayı geçici olarak çözebilirsiniz. Bu, Azure portalındaki Ayarlar ve API'deki BackendPoolsSettings altında bulunur.
2. **Geçersiz CA'dan arka uç barındırma sertifikası**: Ön Kapı ile arka uçta yalnızca geçerli [CA'lardan](/azure/frontdoor/front-door-troubleshoot-allowed-ca) alınan sertifikalar kullanılabilir. İç CA'lardan veya kendi imzalı sertifikalardan sertifikalara izin verilmez.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-metrics-and-logs-are-available-with-azure-front-door"></a>Azure Ön Kapı'da ne tür ölçümler ve günlükler kullanılabilir?

Günlükler ve diğer tanılama özellikleri hakkında bilgi için, [Ön Kapı için Izleme ölçümleri ve günlükleri](front-door-diagnostics.md)bakın.

### <a name="what-is-the-retention-policy-on-the-diagnostics-logs"></a>Tanılama günlüklerinde bekletme ilkesi nedir?

Tanılama günlükleri müşteri depolama hesabına akar ve müşteriler bekletme ilkesini tercihlerine göre ayarlayabilir. Tanılama günlükleri bir Olay Hub'ına veya Azure Monitör günlüklerine de gönderilebilir. Daha fazla bilgi için [Azure Ön Kapı Tanılama'ya](front-door-diagnostics.md)bakın.

### <a name="how-do-i-get-audit-logs-for-azure-front-door"></a>Azure Ön Kapı için denetim günlüklerini nasıl alabilirim?

Azure Ön Kapı için denetim günlükleri kullanılabilir. Portalda, denetim günlüğüne erişmek için Ön Kapınızın menü kutusunda **Etkinlik Günlüğü'nü** tıklatın. 

### <a name="can-i-set-alerts-with-azure-front-door"></a>Azure Ön Kapı ile uyarılar ayarlayabilir miyim?

Evet, Azure Ön Kapı destek uyarıları yapar. Uyarılar ölçümlere göre yapılandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
