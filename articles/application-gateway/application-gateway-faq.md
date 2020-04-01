---
title: Azure Uygulama Ağ Geçidi hakkında sık sorulan sorular
description: Azure Uygulama Ağ Geçidi hakkında sık sorulan soruların yanıtlarını bulun.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: victorh
ms.openlocfilehash: 290467d5d20a74f8b8b2c23f6da0dcadfd74cc56
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411023"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Uygulama Ağ Geçidi hakkında sık sorulan sorular

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Uygulama Ağ Geçidi hakkında sorulan sık sorulan sorular şunlardır.

## <a name="general"></a>Genel

### <a name="what-is-application-gateway"></a>Application Gateway Nedir?

Azure Application Gateway, hizmet olarak bir uygulama teslim denetleyicisi (ADC) sağlar. Uygulamalarınız için çeşitli katman 7 yük dengeleme özellikleri sunar. Bu hizmet yüksek kullanılabilir, ölçeklenebilir ve Azure tarafından tam olarak yönetilebilir.

### <a name="what-features-does-application-gateway-support"></a>Application Gateway hangi özellikleri destekler?

Uygulama Ağ Geçidi otomatik ölçekleme, SSL boşaltma ve uçtan uca SSL, bir web uygulama güvenlik duvarı (WAF), çerez tabanlı oturum afinite, URL yol tabanlı yönlendirme, çok siteli barındırma ve diğer özellikleri destekler. Desteklenen özelliklerin tam listesi için [bkz.](application-gateway-introduction.md)

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>Uygulama Ağ Geçidi ve Azure Yük Dengeleyicisi'nin farkı nedir?

Uygulama Ağ Geçidi, yalnızca web trafiğiyle (HTTP, HTTPS, WebSocket ve HTTP/2) çalıştığı anlamına gelen bir katman 7 yük dengeleyicisidir. SSL sonlandırma, çerez tabanlı oturum yakınlığı ve yük dengeleme trafiği için yuvarlak robin gibi yetenekleri destekler. Yük Dengeleyici yük-4 katmanı (TCP veya UDP) de trafik dengeler.

### <a name="what-protocols-does-application-gateway-support"></a>Application Gateway hangi protokolleri destekler?

Uygulama Ağ Geçidi HTTP, HTTPS, HTTP/2 ve WebSocket'i destekler.

### <a name="how-does-application-gateway-support-http2"></a>Uygulama Ağ Geçidi HTTP/2'yi nasıl destekler?

[HTTP/2 desteğine](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support)bakın.

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>Arka uç havuzunun bir parçası olarak hangi kaynaklar desteklenir?

[Desteklenen arka uç kaynaklarına](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pools)bakın.

### <a name="in-what-regions-is-application-gateway-available"></a>Application Gateway hangi bölgelerde kullanılabilir?

Uygulama Ağ Geçidi, genel Azure'un tüm bölgelerinde kullanılabilir. Ayrıca Azure China [21Vianet](https://www.azure.cn/) ve [Azure Kamu'da](https://azure.microsoft.com/overview/clouds/government/)da kullanılabilir.

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>Bu dağıtım aboneliğime mi özel, yoksa müşteriler arasında mı paylaşılır?

Uygulama Ağ Geçidi, sanal ağınızda özel bir dağıtımdır.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>Uygulama Ağ Geçidi HTTP-TO-HTTPS yeniden yönlendirmesini destekliyor mu?

Yeniden yönlendirme desteklenir. Bkz. [Uygulama Ağ Geçidi yeniden yönlendirme genel bakış.](application-gateway-redirect-overview.md)

### <a name="in-what-order-are-listeners-processed"></a>Dinleyiciler hangi sırayla işlenir?

Dinleyici [işleme sırasına](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners)bakın.

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>Uygulama Ağ Geçidi IP ve DNS'yi nerede bulabilirim?

Herkese açık bir IP adresini bitiş noktası olarak kullanıyorsanız, genel IP adresi kaynağında IP ve DNS bilgilerini bulabilirsiniz. Veya portalda, uygulama ağ geçidi için genel bakış sayfasında bulabilirsiniz. Dahili IP adreslerini kullanıyorsanız, genel bakış sayfasındaki bilgileri bulun.

v2 SKU için ortak IP kaynağını açın ve **Yapılandırma'yı**seçin. DNS adını yapılandırmak için **DNS ad etiketi (isteğe bağlı)** alanı kullanılabilir.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>Keep-Alive zaman sonu ve TCP boşta zaman sonu ayarları nelerdir?

*Keep-Alive zaman sonu,* Uygulama Ağ Geçidi'nin istemciyi yeniden kullanmadan veya kapatmadan önce kalıcı bir bağlantıya başka bir HTTP isteği göndermesini ne kadar bekleyeceğini yönetir. *TCP boşta zaman alabilen* zaman alakart, herhangi bir etkinlik olmaması durumunda TCP bağlantısının ne kadar süre açık tutulabildiğini yönetir. 

Uygulama Ağ Geçidi v1 SKU'daki *Keep-Alive zaman dilimi* 120 saniye, v2 SKU'da ise 75 saniye. *TCP boşta zaman sonu,* Uygulama Ağ Geçidi'nin hem v1 hem de v2 SKU'nun ön uç sanal IP'si (VIP) üzerinde 4 dakikalık bir varsayılandır. 

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>IP veya DNS adı uygulama ağ geçidinin ömrü boyunca değişir mi?

Uygulama Ağ Geçidi V1 SKU'da, uygulama ağ geçidini durdurur ve çalıştırsanız VIP değişebilir. Ancak uygulama ağ geçidiyle ilişkili DNS adı ağ geçidinin ömrü boyunca değişmez. DNS adı değişmediği için CNAME takma adı kullanmalı ve uygulama ağ geçidinin DNS adresine yönlendirmelisiniz. Uygulama Ağ Geçidi V2 SKU'da IP adresini statik olarak ayarlayabilirsiniz, böylece IP ve DNS adı uygulama ağ geçidinin ömrü boyunca değişmez. 

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway statik IP'yi destekliyor mu?

Evet, Application Gateway v2 SKU statik genel IP adreslerini destekler. v1 SKU statik iç IP'leri destekler.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Uygulama Ağ Geçidi ağ geçidinde birden çok genel IP'yi destekliyor mu?

Uygulama ağ geçidi yalnızca bir genel IP adresini destekler.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Uygulama Ağ Geçidi için alt ağımı ne kadar büyütmem gerekir?

Bkz. [Uygulama Ağ Geçidi alt ağ boyutu hususlar.](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet)

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>Tek bir alt ağa birden fazla Uygulama Ağ Geçidi kaynağı dağıtabilir miyim?

Evet. Belirli bir Uygulama Ağ Geçidi dağıtımının birden çok örneğine ek olarak, farklı bir Uygulama Ağ Geçidi kaynağı içeren varolan bir alt ağa başka bir benzersiz Uygulama Ağ Geçidi kaynağı sağlayabilirsiniz.

Tek bir alt ağ hem Standard_v2 hem de Standart Uygulama Ağ Geçidi'ni birlikte destekleyebilir.

### <a name="does-application-gateway-v2-support-user-defined-routes-udr"></a>Application Gateway v2 kullanıcı tanımlı yolları (UDR) destekliyor mu?

Evet, ama sadece belirli senaryolar. Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yapılandırmagenel bakışı.](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet)

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Uygulama Ağ Geçidi x iletilen üstbilgi desteği mi?

Evet. Bkz. [Bir istekte yapılan değişiklikler.](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request)

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>Bir uygulama ağ geçidini dağıtmak ne kadar sürer? Uygulama ağ geçidim güncelleştirilirken çalışacak mı?

Yeni Uygulama Ağ Geçidi v1 SKU dağıtımlarının sağlanması 20 dakika kadar sürebilir. Örnek boyutu veya sayısındaki değişiklikler kesintiye uğratmaz ve ağ geçidi bu süre boyunca etkin kalır.

v2 SKU kullanan dağıtımların çoğunun sağlanması yaklaşık 6 dakika sürer. Ancak dağıtım türüne bağlı olarak daha uzun sürebilir. Örneğin, birden çok örnekiçeren birden çok Kullanılabilirlik Bölgesi'nde dağıtımlar 6 dakikadan fazla sürebilir. 

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>Exchange Server'ı Application Gateway ile arka uç olarak kullanabilir miyim?

Hayır. Uygulama Ağ Geçidi, SMTP, IMAP ve POP3 gibi e-posta protokollerini desteklemez. 

## <a name="performance"></a>Performans

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Application Gateway yüksek kullanılabilirliği ve ölçeklenebilirliği nasıl destekler?

Uygulama Ağ Geçidi v1 SKU, iki veya daha fazla örnek dağıttığınızda yüksek kullanılabilirlik senaryolarını destekler. Azure, bu örnekleri güncelleştirme ve hata etki alanlarında dağıtan bu örnekleri, örneklerin aynı anda başarısız olmamasını sağlar. v1 SKU, yükü paylaşmak için aynı ağ geçidinin birden çok örneğini ekleyerek ölçeklenebilirliği destekler.

v2 SKU, yeni örneklerin hata etki alanları arasında yayılmasını ve etki alanlarını güncellemesini otomatik olarak sağlar. Bölge artıklığını seçerseniz, en yeni örnekler de bölge hatası esnekliği sunmak için kullanılabilirlik bölgeleri arasında yayılır.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>Uygulama Ağ Geçidi'ni kullanarak veri merkezlerinde DR senaryosuna nasıl ulaşabilirim?

Trafiği farklı veri merkezlerinde birden çok uygulama ağ geçidine dağıtmak için Trafik Yöneticisi'ni kullanın.

### <a name="does-application-gateway-support-autoscaling"></a>Uygulama Ağ Geçidi otomatik küçültmeyi destekliyor mu?

Evet, Uygulama Ağ Geçidi v2 SKU otomatik ölçekleme destekler. Daha fazla bilgi için bkz: [Otomatik ölçekleme ve Bölge yedekli Uygulama Ağ Geçidi.](application-gateway-autoscaling-zone-redundant.md)

### <a name="does-manual-or-automatic-scale-up-or-scale-down-cause-downtime"></a>Manuel veya otomatik ölçeklendirme veya küçültme çalışmamaz neden olur mu?

Hayır. Örnekler yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

### <a name="does-application-gateway-support-connection-draining"></a>Uygulama Ağ Geçidi bağlantının boşalmasını destekliyor mu?

Evet. Bir arka uç havuzundaki üyeleri kesintiye uğramadan değiştirmek için bağlantı drenajı ayarlayabilirsiniz. Daha fazla bilgi için, [Uygulama Ağ Geçidi'nin bağlantı boşaltma bölümüne](features.md#connection-draining)bakın.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Örnek boyutunu kesintiye uğramadan ortadan büyüğe değiştirebilir miyim?

Evet.

## <a name="configuration"></a>Yapılandırma

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Uygulama Ağ Geçidi her zaman sanal ağda mı dağıtılır?

Evet. Uygulama Ağ Geçidi her zaman sanal ağ alt ağında dağıtılır. Bu alt ağ yalnızca uygulama ağ geçitleri içerebilir. Daha fazla bilgi için [sanal ağ ve alt ağ gereksinimlerine](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet)bakın.

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>Application Gateway, sanal ağının dışındaki veya aboneliği dışındaki örneklerle iletişim kurabilir mi?

IP bağlantınız olduğu sürece, Application Gateway içinde olduğu sanal ağın dışındaki örneklerle iletişim kurabilir. Uygulama Ağ Geçidi, içinde olduğu aboneliğin dışındaki örneklerle de iletişim kurabilir. Dahili IP'leri arka uç havuzu üyesi olarak kullanmayı planlıyorsanız, [sanal ağ eşlemesini](../virtual-network/virtual-network-peering-overview.md) veya [Azure VPN Ağ Geçidi'ni](../vpn-gateway/vpn-gateway-about-vpngateways.md)kullanın.

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Uygulama ağ geçidi alt ağına başka bir şey dağıtabilir miyim?

Hayır. Ancak alt ağdaki diğer uygulama ağ geçitlerini dağıtabilirsiniz.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Ağ güvenlik grupları uygulama ağ geçidi alt ağında desteklenir mi?

[Uygulama Ağ Geçidi alt ağındaki Ağ güvenlik gruplarına](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet)bakın.

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>Uygulama ağ geçidi alt ağı kullanıcı tanımlı yolları destekliyor mu?

[Uygulama Ağ Geçidi alt netinde desteklenen Kullanıcı tanımlı rotalara](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet)bakın.

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Uygulama Ağ Geçidi'nin sınırları nelerdir? Bu limitleri artırabilir miyim?

Bkz. [Uygulama Ağ Geçidi sınırları.](../azure-resource-manager/management/azure-subscription-service-limits.md#application-gateway-limits)

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>Uygulama Ağ Geçidi'ni hem iç hem de dış trafik için aynı anda kullanabilir miyim?

Evet. Uygulama Ağ Geçidi, uygulama ağ geçidi başına bir dahili IP ve bir harici IP'yi destekler.

### <a name="does-application-gateway-support-virtual-network-peering"></a>Application Gateway sanal ağ akranlarını destekliyor mu?

Evet. Sanal ağ eşleme, diğer sanal ağlardaki yük dengesi trafiğine yardımcı olur.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>ExpressRoute veya VPN tünelleri ile bağlandıklarında şirket içi sunucularla konuşabilir miyim?

Evet, trafiğe izin verildiği sürece.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>Bir arka uç havuzu farklı bağlantı noktalarında birçok uygulamaya hizmet edebilir mi?

Microservice mimarisi desteklenir. Farklı bağlantı noktalarında araştırma yapmak için birden çok HTTP ayarlarını yapılandırmanız gerekir.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>Özel sondalar yanıt verilerinde joker karakterleri veya regex'i destekliyor mu?

Hayır. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>Uygulama Ağ Geçidi'nde yönlendirme kuralları nasıl işlenir?

Bkz. [İşlem kuralları sırası.](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules)

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>Özel sondalar için, Ana Bilgisayar alanı ne anlama geliyor?

Ana Bilgisayar alanı, Application Gateway'de çok siteli yapılandırıldığınızda sondayı gönderecek adı belirtir. Aksi takdirde '127.0.0.1' kullanın. Bu değer, sanal makine ana bilgisayar adından farklıdır. Biçimi \<\>protokol -\<\>ana\<\>\<bilgisayar\>: bağlantı noktası yoludur.

### <a name="can-i-allow-application-gateway-access-to-only-a-few-source-ip-addresses"></a>Uygulama Ağ Geçidi'nin yalnızca birkaç kaynak IP adresine erişmesine izin verebilir miyim?

Evet. Bkz. [belirli kaynak IP'lerine erişimi kısıtlayın.](https://docs.microsoft.com/azure/application-gateway/configuration-overview#allow-application-gateway-access-to-a-few-source-ips)

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>Aynı bağlantı noktasını hem kamuya açık hem de özel dinleyiciler için kullanabilir miyim?

Hayır.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>v1 SKU'dan v2 SKU'ya geçiş için kılavuz var mı?

Evet. Ayrıntılar için bkz: [Azure Uygulama Ağ Geçidi'ni ve Web Uygulama Güvenlik Duvar'ı v1'den v2'ye geçirin.](migrate-v1-v2.md)

### <a name="does-application-gateway-support-ipv6"></a>Uygulama Ağ Geçidi IPv6'yı destekliyor mu?

Uygulama Ağ Geçidi v2 şu anda IPv6'yı desteklemiyor. Yalnızca IPv4 kullanarak çift yığın VNet'te çalışabilir, ancak ağ geçidi alt ağı yalnızca IPv4 olmalıdır. Uygulama Ağ Geçidi v1 çift yığın VNets desteklemez. 

## <a name="configuration---ssl"></a>Yapılandırma - SSL

### <a name="what-certificates-does-application-gateway-support"></a>Application Gateway hangi sertifikaları destekler?

Uygulama Ağ Geçidi, kendi imzalı sertifikaları, sertifika yetkilisi (CA) sertifikalarını, Genişletilmiş Doğrulama (EV) sertifikalarını ve joker karakter sertifikalarını destekler.

### <a name="what-cipher-suites-does-application-gateway-support"></a>Application Gateway hangi şifreleme paketlerini destekler?

Application Gateway aşağıdaki şifre paketlerini destekler. 

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

SSL seçeneklerini özelleştirme hakkında daha fazla bilgi için Uygulama [Ağ Geçidi'ndeki SSL ilke sürümlerini ve şifreleme paketlerini yapılandırın.](application-gateway-configure-ssl-policy-powershell.md)

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>Uygulama Ağ Geçidi, trafiğin arka uçta yeniden şifrelenmesini destekliyor mu?

Evet. Uygulama Ağ Geçidi, trafiği arka uca yeniden şifreleyen SSL boşaltma ve uçtan uca SSL'yi destekler.

### <a name="can-i-configure-ssl-policy-to-control-ssl-protocol-versions"></a>SSL protokol sürümlerini denetlemek için SSL ilkesini yapılandırabilir miyim?

Evet. Uygulama Ağ Geçidi'ni TLS1.0, TLS1.1 ve TLS1.2'yi reddecek şekilde yapılandırabilirsiniz. Varsayılan olarak, SSL 2.0 ve 3.0 zaten devre dışı bırakılmıştır ve yapılandırılamaz.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Şifre paketlerini ve ilke sırasını yapılandırabilir miyim?

Evet. Uygulama Ağ Geçidi'nde, [şifre paketlerini yapılandırabilirsiniz.](application-gateway-ssl-policy-overview.md) Özel bir ilke tanımlamak için aşağıdaki şifreleme paketlerinden en az birini etkinleştirin. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Uygulama Ağ Geçidi arka uç yönetimi için SHA256 kullanır.

### <a name="how-many-ssl-certificates-does-application-gateway-support"></a>Application Gateway kaç SSL sertifikasını destekler?

Uygulama Ağ Geçidi 100 SSL'ye kadar sertifikayı destekler.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>Geri uç yeniden şifrelemesi için kaç kimlik doğrulama sertifikası Application Gateway destekletir?

Uygulama Ağ Geçidi en fazla 100 kimlik doğrulama sertifikasıdestekler.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>Uygulama Ağ Geçidi Azure Anahtar Kasası ile yerel olarak tümleşir mi?

Evet, Application Gateway v2 SKU Key Vault'u destekler. Daha fazla bilgi için [Key Vault sertifikalarıile SSL sonlandırma'ya](key-vault-certs.md)bakın.

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>.com ve .net siteleri için HTTPS dinleyicilerini nasıl yapılandırıyorum? 

Birden çok etki alanı tabanlı (ana bilgisayar tabanlı) yönlendirme için, çok siteli dinleyiciler oluşturabilir, iletişim kuralı olarak HTTPS'yi kullanan dinleyiciler ayarlayabilir ve dinleyicileri yönlendirme kurallarıyla ilişkilendirebilirsiniz. Daha fazla bilgi için, [Uygulama Ağ Geçidi'ni kullanarak birden çok siteyi barındırma'ya](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview)bakın.

### <a name="can-i-use-special-characters-in-my-pfx-file-password"></a>.pfx dosya şifremde özel karakterler kullanabilir miyim?

Hayır, .pfx dosya parolasınızda yalnızca alfasayısal karakterler kullanın.

## <a name="configuration---web-application-firewall-waf"></a>Yapılandırma - web uygulaması güvenlik duvarı (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>WAF SKU, Standart SKU'da bulunan tüm özellikleri sunuyor mu?

Evet. WAF, Standart SKU'daki tüm özellikleri destekler.

### <a name="how-do-i-monitor-waf"></a>WAF'ı nasıl izleyebilirim?

Tanısal günlüğe kaydetme yoluyla WAF'ı izleyin. Daha fazla bilgi için Uygulama [Ağ Geçidi için Tanılama günlüğe kaydetme ve ölçümlere](application-gateway-diagnostics.md)bakın.

### <a name="does-detection-mode-block-traffic"></a>Algılama modu trafiği engelliyor mu?

Hayır. Algılama modu yalnızca WAF kuralını tetikleyen trafiği kaydeder.

### <a name="can-i-customize-waf-rules"></a>WAF kurallarını özelleştirebilir miyim?

Evet. Daha fazla bilgi için [WAF kural gruplarını ve kurallarını özelleştir'e](application-gateway-customize-waf-rules-portal.md)bakın.

### <a name="what-rules-are-currently-available-for-waf"></a>WAF için şu anda hangi kurallar mevcut?

WAF şu anda CRS [2.2.9,](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp229) [3.0](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp30)ve [3.1'i](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp31)destekler. Bu kurallar, Open Web Application Security Project'in (OWASP) tanımladığı en iyi 10 güvenlik açığının çoğuna karşı temel güvenlik sağlar: 

* SQL ekleme koruması
* Site arası komut dosyası koruması
* Komut enjeksiyonu, HTTP istek kaçakçılığı, HTTP yanıt bölme ve uzaktan dosya ekleme saldırısı gibi yaygın web saldırılarına karşı koruma
* HTTP protokolü ihlallerine karşı koruma
* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma
* Robotlar, gezginler ve tarayıcıları önleme
* Yaygın uygulama yanlış yapılandırmalarının algılanması (diğer bir şey, Apache, IIS vb.)

Daha fazla bilgi için Bkz. [OWASP top-10 güvenlik açıkları.](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

### <a name="does-waf-support-ddos-protection"></a>WAF DDoS korumayı destekliyor mu?

Evet. Uygulama ağ geçidinin dağıtıldığı sanal ağda DDoS korumasını etkinleştirebilirsiniz. Bu ayar, Azure DDoS Koruma hizmetinin uygulama ağ geçidi sanal IP'yi (VIP) de korumasını sağlar.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>v1 SKU'dan v2 SKU'ya geçiş için kılavuz var mı?

Evet. Ayrıntılar için bkz: [Azure Uygulama Ağ Geçidi'ni ve Web Uygulama Güvenlik Duvar'ı v1'den v2'ye geçirin.](migrate-v1-v2.md)

## <a name="configuration---ingress-controller-for-aks"></a>Yapılandırma - AKS için giriş denetleyicisi

### <a name="what-is-an-ingress-controller"></a>Giriş Denetleyici nedir?

Kubernetes kümeiçinde `deployment` `service` dahili olarak bir bölme grubu ortaya çıkarmak için kaynak oluşturulmasıve kaynak sağlar. Aynı hizmeti harici olarak ortaya [`Ingress`](https://kubernetes.io/docs/concepts/services-networking/ingress/) çıkarmak için, yük dengeleme, SSL sonlandırma ve ad tabanlı sanal barındırma sağlayan bir kaynak tanımlanır.
Bu `Ingress` kaynağı karşılamak için, kaynaklardaki `Ingress` değişiklikleri dinleyen ve yük dengeleyici ilkelerini yapılandıran bir Giriş Denetleyicisi gereklidir.

Uygulama Ağ Geçidi Giriş Denetleyicisi, [Azure Uygulama Ağ Geçidi'nin](https://azure.microsoft.com/services/application-gateway/) AKS kümesi olarak da bilinen Bir [Azure Kubernetes Hizmeti](https://azure.microsoft.com/services/kubernetes-service/) için giriş olarak kullanılmasına olanak tanır.

### <a name="can-a-single-ingress-controller-instance-manage-multiple-application-gateways"></a>Tek bir giriş denetleyici örneği birden çok Uygulama Ağ Geçidi'ni yönetebilir mi?

Şu anda, Giriş Denetleyicisinin bir örneği yalnızca bir Uygulama Ağ Geçidi ile ilişkilendirilebilir.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-logs-does-application-gateway-provide"></a>Uygulama Ağ Geçidi ne tür günlükler sağlar?

Uygulama Ağ Geçidi üç günlük sağlar: 

* **ApplicationGatewayAccessLog**: Erişim günlüğü, uygulama ağ geçidi ön ucuna gönderilen her isteği içerir. Veriler, arayanın IP'sini, istenen URL'sini, yanıt gecikmesini, iade kodunu ve baytlarını içerir. Uygulama ağ geçidi başına bir kayıt içerir.
* **ApplicationGatewayPerformanceLog**: Performans günlüğü her uygulama ağ geçidi için performans bilgilerini yakalar. Bilgiler, baytlarda elde edilen iş bsayısını, sunulan toplam istekleri, başarısız istek sayısını ve sağlıklı ve sağlıksız arka uç örnek sayısını içerir.
* **ApplicationGatewayFirewallLog**: WAF ile yapılandırdığınız uygulama ağ geçitleri için, güvenlik duvarı günlüğü algılama modu veya önleme modu üzerinden günlüğe kaydedilmiş istekleri içerir.

Tüm günlükler her 60 saniyede bir toplanır. Daha fazla bilgi için, [Bkz. Backend sağlık, tanılama günlükleri ve Uygulama Ağ Geçidi için ölçümler.](application-gateway-diagnostics.md)

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>Arka uç havuz üyelerimin sağlıklı olup olmadığını nasıl anlarım?

PowerShell cmdlet `Get-AzApplicationGatewayBackendHealth` veya portalkullanarak sağlık doğrulayın. Daha fazla bilgi için [Uygulama Ağ Geçidi tanılama](application-gateway-diagnostics.md)bakın.

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>Tanılama günlükleri için bekletme ilkesi nedir?

Tanılama günlükleri müşterinin depolama hesabına akar. Müşteriler bekletme ilkesini tercihlerine göre ayarlayabilir. Tanılama günlükleri bir olay hub'ına veya Azure Monitor günlüklerine de gönderilebilir. Daha fazla bilgi için [Uygulama Ağ Geçidi tanılama](application-gateway-diagnostics.md)bakın.

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Uygulama Ağ Geçidi için denetim günlüklerini nasıl alabilirim?

Portalda, bir uygulama ağ geçidinin menü bıçaklarında denetim günlüğüne erişmek için **Etkinlik Günlüğü'nü** seçin. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Application Gateway ile uyarılar ayarlayabilir miyim?

Evet. Uygulama Ağ Geçidi'nde uyarılar ölçümlere göre yapılandırılır. Daha fazla bilgi için [Uygulama Ağ Geçidi ölçümlerine](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics) bakın ve [uyarı bildirimleri alın.](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Uygulama Ağ Geçidi için trafik istatistiklerini nasıl analiz edebilirim?

Erişim günlüklerini çeşitli şekillerde görüntüleyebilir ve çözümleyebilirsiniz. Azure Monitor günlüklerini, Excel'i, Power BI'yi ve benzeri kullanımları kullanın.

Ayrıca, Uygulama Ağ Geçidi erişim günlükleri için popüler [GoAccess](https://goaccess.io/) günlük çözümleyicisini yükleyen ve çalıştıran bir Kaynak Yöneticisi şablonu da kullanabilirsiniz. GoAccess, benzersiz ziyaretçiler, istenen dosyalar, ana bilgisayarlar, işletim sistemleri, tarayıcılar ve HTTP durum kodları gibi değerli HTTP trafik istatistikleri sağlar. Daha fazla bilgi için GitHub'da [Kaynak Yöneticisi şablon klasöründeki Readme dosyasına](https://aka.ms/appgwgoaccessreadme)bakın.

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>Bilinmeyen bir durumu döndürmek için arka uç sağlığına ne neden olabilir?

Genellikle, arka uçerişimi bir ağ güvenlik grubu (NSG), özel DNS veya kullanıcı tanımlı yönlendirme (UDR) tarafından uygulama ağ geçidi alt ağında engellendiğinde bilinmeyen bir durum görürsünüz. Daha fazla bilgi için, [Bkz. Backend sağlık, tanılama günlüğü ve Uygulama Ağ Geçidi için ölçümler.](application-gateway-diagnostics.md)

### <a name="is-there-any-case-where-nsg-flow-logs-wont-show-allowed-traffic"></a>NSG akış günlüklerinin izin verilen trafiği göstermeyeceği bir durum var mı?

Evet. Yapılandırmanız aşağıdaki senaryoyla eşleşiyorsa, NSG akış günlüklerinizde izin verilen trafiği görmezsiniz:
- Uygulama Ağ Geçidi v2'yi dağıttınız
- Uygulama ağ geçidi alt netinde bir NSG'niz var
- NSG akış günlüklerini etkinleştirdin.

### <a name="how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address"></a>Yalnızca özel ön uç IP adresiyle Application Gateway V2'yi nasıl kullanırım?

Uygulama Ağ Geçidi V2 şu anda yalnızca özel IP modunu desteklemiyor. Aşağıdaki kombinasyonları destekler
* Özel IP ve Genel IP
* Yalnızca genel IP

Ancak Uygulama Ağ Geçidi V2'yi yalnızca özel IP ile kullanmak istiyorsanız, aşağıdaki işlemi takip edebilirsiniz:
1. Hem genel hem de özel ön uç IP adresiyle bir Uygulama Ağ Geçidi Oluşturma
2. Public frontend IP adresi için dinleyici oluşturmayın. Uygulama Ağ Geçidi, bunun için dinleyici oluşturulmazsa, genel IP adresindeki trafiği dinlemez.
3. Uygulama Ağ Geçidi alt ağı için öncelik sırasına göre aşağıdaki yapılandırmayla bir [Ağ Güvenlik Grubu](https://docs.microsoft.com/azure/virtual-network/security-overview) oluşturun ve iliştirin:
    
    a. **65200-65535**olarak **GatewayManager** servis etiketi ve Hedef olarak **Herhangi** ve Hedef bağlantı noktası olarak Kaynak trafik izin verin. Bu bağlantı noktası aralığı Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları sertifika kimlik doğrulaması tarafından korunur (kilitlenir). Ağ Geçidi kullanıcı yöneticileri de dahil olmak üzere dış varlıklar, uygun sertifikalar yerine olmadan bu uç noktalarda değişiklik başlatamaz
    
    b. **AzureLoadBalancer** servis etiketi ve herhangi **bir** olarak hedef bağlantı noktası olarak Kaynak trafiğine izin verin
    
    c. **Kaynak'tan** gelen tüm trafiği Internet servis etiketi ve hedef bağlantı noktası **olarak**herhangi bir şekilde reddet. Bu kurala gelen kurallarda *en az önceliği* verin
    
    d. Özel IP adresine erişimin engellenmemesine izin vermek gibi varsayılan kuralları tutun
    
    e. Giden internet bağlantısı engellenemez. Aksi takdirde, günlük, ölçümler ve benzeri sorunlarla karşı karşıya kalacaktır.

Yalnızca özel IP erişimi için örnek ![NSG yapılandırması: Yalnızca özel IP erişimi için Uygulama Ağ Geçidi V2 NSG Yapılandırması](./media/application-gateway-faq/appgw-privip-nsg.png)

### <a name="does-application-gateway-affinity-cookie-support-samesite-attribute"></a>Uygulama Ağ Geçidi afiyet çerezi SameSite özniteliğini destekliyor mu?
Evet, [Krom tarayıcı](https://www.chromium.org/Home) [v80 güncellemesi](https://chromiumdash.appspot.com/schedule) SameSite= Lax olarak kabul edilecek SameSite özniteliği olmadan HTTP çerezleri bir görev tanıttı. Bu, Uygulama Ağ Geçidi yakınlık çerezinin tarayıcı tarafından üçüncü taraf bağlamında gönderilmeyeceği anlamına gelir. Bu senaryoyu desteklemek için, Application Gateway varolan *ApplicationGatewayAffinity* çerezine ek olarak *ApplicationGatewayAffinityCORS* adlı başka bir çerez enjekte eder.  Bu çerezler benzer, ancak *ApplicationGatewayAffinityCORS* çerez iki öznitelikleri eklendi: *SameSite =None; Güvenli*. Bu öznitelikler, çapraz orijin istekleri için bile yapışkan oturumları korur. Daha fazla bilgi için [çerez tabanlı yakınlık bölümüne](configuration-overview.md#cookie-based-affinity) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi hakkında daha fazla bilgi edinmek için Azure [Uygulama Ağ Geçidi nedir?](overview.md)
