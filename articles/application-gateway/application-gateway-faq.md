---
title: Azure Application Gateway hakkında sık sorulan sorular
description: Azure Application Gateway hakkında sık sorulan soruların yanıtlarını bulun.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/01/2020
ms.author: victorh
ms.openlocfilehash: 8acf04d26d0c69c426eb86f8a26a9f243c6f5ad2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82136677"
---
# <a name="frequently-asked-questions-about-application-gateway"></a>Application Gateway hakkında sık sorulan sorular

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure Application Gateway ile ilgili sık sorulan sorular aşağıda verilmiştir.

## <a name="general"></a>Genel

### <a name="what-is-application-gateway"></a>Application Gateway Nedir?

Azure Application Gateway, bir hizmet olarak uygulama teslim denetleyicisi (ADC) sağlar. Uygulamalarınız için çeşitli katman 7 yük dengeleme özellikleri sunar. Bu hizmet yüksek oranda kullanılabilir, ölçeklenebilir ve Azure tarafından tam olarak yönetilir.

### <a name="what-features-does-application-gateway-support"></a>Application Gateway hangi özellikleri destekler?

Application Gateway otomatik ölçeklendirmeyi, TLS yük boşaltma ve uçtan uca TLS, bir Web uygulaması güvenlik duvarı (WAF), tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı yönlendirme, çoklu site barındırma ve diğer özellikleri destekler. Desteklenen özelliklerin tam listesi için bkz. [tanıtım Application Gateway](application-gateway-introduction.md).

### <a name="how-do-application-gateway-and-azure-load-balancer-differ"></a>Application Gateway ve Azure Load Balancer nasıl farklıdır?

Application Gateway, yalnızca Web trafiğiyle (HTTP, HTTPS, WebSocket ve HTTP/2) çalıştığı anlamına gelen bir katman 7 yük dengeleyicidir. TLS sonlandırma, tanımlama bilgisi tabanlı oturum benzeşimi ve yük dengeleme trafiği için hepsini bir kez deneme gibi özellikleri destekler. Load Balancer yük-trafiği katman 4 ' te (TCP veya UDP) dengeler.

### <a name="what-protocols-does-application-gateway-support"></a>Application Gateway hangi protokolleri destekler?

Application Gateway HTTP, HTTPS, HTTP/2 ve WebSocket 'i destekler.

### <a name="how-does-application-gateway-support-http2"></a>Application Gateway HTTP/2 ' ye nasıl destekler?

Bkz. [http/2 desteği](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

### <a name="what-resources-are-supported-as-part-of-a-backend-pool"></a>Arka uç havuzunun bir parçası olarak hangi kaynaklar desteklenir?

[Desteklenen arka uç kaynaklarına](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#backend-pools)bakın.

### <a name="in-what-regions-is-application-gateway-available"></a>Application Gateway hangi bölgelerde kullanılabilir?

Application Gateway tüm küresel Azure bölgelerinde kullanılabilir. [Azure Çin 21Vianet](https://www.azure.cn/) ve [Azure Kamu](https://azure.microsoft.com/overview/clouds/government/)'da da kullanılabilir.

### <a name="is-this-deployment-dedicated-for-my-subscription-or-is-it-shared-across-customers"></a>Bu dağıtım Aboneliğim için ayrılmıştır veya müşteriler arasında paylaşılıyor mu?

Application Gateway, sanal ağınızdaki adanmış bir dağıtımdır.

### <a name="does-application-gateway-support-http-to-https-redirection"></a>Application Gateway HTTP-HTTPS yeniden yönlendirmeyi destekliyor mu?

Yeniden yönlendirme destekleniyor. Bkz. [Application Gateway yeniden yönlendirmeye genel bakış](application-gateway-redirect-overview.md).

### <a name="in-what-order-are-listeners-processed"></a>Dinleyicilerinin hangi sırada işlendiği?

[Dinleyici işleme sırasına](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-listeners)bakın.

### <a name="where-do-i-find-the-application-gateway-ip-and-dns"></a>IP ve DNS Application Gateway nerede bulabilirim?

Uç nokta olarak genel bir IP adresi kullanıyorsanız, IP ve DNS bilgilerini genel IP adresi kaynağında bulabilirsiniz. Veya Portal 'da, uygulama ağ geçidinin Genel Bakış sayfasında bulabilirsiniz. İç IP adresleri kullanıyorsanız Genel Bakış sayfasındaki bilgileri bulun.

V2 SKU 'SU için genel IP kaynağını açın ve **yapılandırma**' yı seçin. DNS ad **etiketi (isteğe bağlı)** alanı DNS adını yapılandırmak için kullanılabilir.

### <a name="what-are-the-settings-for-keep-alive-timeout-and-tcp-idle-timeout"></a>Etkin tutma zaman aşımı ve TCP boşta kalma zaman aşımı ayarları nelerdir?

*Canlı tutma zaman aşımı* , Application Gateway bir istemcinin yeniden kullanmadan veya kapatmadan önce kalıcı bir bağlantıda başka bir http isteği göndermesini ne kadar bekleyeceğini yönetir. *TCP boşta kalma zaman aşımı* , bir TCP bağlantısının bir etkinlik olmaması durumunda açık tutulacağı süreyi yönetir. 

Application Gateway v1 SKU 'sunda *canlı tut zaman aşımı süresi* 120 saniyedir ve v2 sku 'sunda 75 saniyedir. *TCP boşta kalma zaman aşımı* , Application Gateway hem v1 hem de v2 SKU 'SU sanal IP 'SINDE (VIP) 4 dakikalık bir varsayılandır. Bu değerleri değiştiremezsiniz.

### <a name="does-the-ip-or-dns-name-change-over-the-lifetime-of-the-application-gateway"></a>Uygulama ağ geçidinin ömrü boyunca IP veya DNS adı değişsin mi?

Application Gateway v1 SKU 'sunda, uygulama ağ geçidini durdurup başlatırsanız VIP değişebilir. Ancak uygulama ağ geçidiyle ilişkilendirilen DNS adı ağ geçidinin kullanım ömrü boyunca değişmez. DNS adı değişmediği için bir CNAME diğer adı kullanmalı ve uygulama ağ geçidinin DNS adresine işaret etmelisiniz. Application Gateway v2 SKU 'sunda IP adresini statik olarak ayarlayabilirsiniz, bu nedenle IP ve DNS adı, uygulama ağ geçidinin kullanım ömrü boyunca değişmeyecektir. 

### <a name="does-application-gateway-support-static-ip"></a>Application Gateway statik IP 'yi destekliyor mu?

Evet, Application Gateway v2 SKU 'SU statik genel IP adreslerini destekler. V1 SKU 'SU statik iç IP 'Leri destekler.

### <a name="does-application-gateway-support-multiple-public-ips-on-the-gateway"></a>Application Gateway ağ geçidinde birden çok genel IP 'yi destekliyor mu?

Uygulama ağ geçidi yalnızca bir genel IP adresini destekler.

### <a name="how-large-should-i-make-my-subnet-for-application-gateway"></a>Alt ağumu Application Gateway için ne kadar büyük hale yapmam gerekir?

Bkz. [Application Gateway alt ağ boyutu konuları](https://docs.microsoft.com/azure/application-gateway/configuration-overview#size-of-the-subnet).

### <a name="can-i-deploy-more-than-one-application-gateway-resource-to-a-single-subnet"></a>Tek bir alt ağa birden fazla Application Gateway kaynağı dağıtabilir miyim?

Evet. Belirli bir Application Gateway dağıtımının birden çok örneğine ek olarak, farklı bir Application Gateway kaynağı içeren mevcut bir alt ağa başka bir benzersiz Application Gateway kaynağı sağlayabilirsiniz.

Tek bir alt ağ hem Standard_v2 hem de standart Application Gateway birlikte destekleyemez.

### <a name="does-application-gateway-v2-support-user-defined-routes-udr"></a>Application Gateway V2 Kullanıcı tanımlı yolları (UDR) destekliyor mu?

Evet, ancak yalnızca belirli senaryolar. Daha fazla bilgi için bkz. [Application Gateway yapılandırmasına genel bakış](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet).

### <a name="does-application-gateway-support-x-forwarded-for-headers"></a>Üst bilgiler için x-iletilmiş Application Gateway destekler mi?

Evet. [Bir Istekteki değişikliklere](https://docs.microsoft.com/azure/application-gateway/how-application-gateway-works#modifications-to-the-request)bakın.

### <a name="how-long-does-it-take-to-deploy-an-application-gateway-will-my-application-gateway-work-while-its-being-updated"></a>Uygulama ağ geçidini dağıtmak ne kadar sürer? Uygulama ağ geçim güncelleştirilirken çalışacak mı?

Yeni Application Gateway v1 SKU dağıtımları, sağlanması 20 dakikaya kadar sürebilir. Örnek boyutu veya sayımla yapılan değişiklikler karışıklığa neden olmaz ve ağ geçidi bu süre içinde etkin kalır.

V2 SKU 'sunu kullanan dağıtımların çoğu, sağlanması 6 dakika sürer. Ancak, dağıtımın türüne bağlı olarak daha uzun sürebilir. Örneğin, çok sayıda örneğe sahip birden fazla Kullanılabilirlik Alanları dağıtımı 6 dakikadan fazla sürebilir. 

### <a name="can-i-use-exchange-server-as-a-backend-with-application-gateway"></a>Exchange Server 'ı Application Gateway arka uç olarak kullanabilir miyim?

Hayır. Application Gateway, SMTP, IMAP ve POP3 gibi e-posta protokollerini desteklemez.

### <a name="is-there-guidance-available-to-migrate-from-the-v1-sku-to-the-v2-sku"></a>V1 SKU 'sunda v2 SKU 'suna geçiş yapmak için kullanılabilecek bir kılavuz var mı?

Evet. Ayrıntılar için bkz: [Azure Application Gateway ve Web uygulaması güvenlik duvarını v1 'den v2 'ye geçirme](migrate-v1-v2.md).

### <a name="will-the-application-gateway-v1-sku-continue-to-be-supported"></a>Application Gateway v1 SKU 'SU desteklenmeye devam edecek mi?

Evet. Application Gateway v1 SKU 'SU desteklenmeye devam edecektir. Ancak, bu SKU 'daki özellik güncelleştirmelerinden yararlanmak için v2 'ye geçmeniz önemle tavsiye edilir. Daha fazla bilgi için bkz. [Otomatik ölçeklendirme ve bölge yedekli Application Gateway v2](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-application-gateway-v2-support-proxying-requests-with-ntlm-authentication"></a>Application Gateway v2, NTLM kimlik doğrulamasıyla proxy isteklerini destekler mi?

Hayır. Application Gateway v2, kimlik doğrulama isteklerini henüz NTLM kimlik doğrulamasıyla desteklemez.

### <a name="does-application-gateway-affinity-cookie-support-samesite-attribute"></a>Application Gateway benzeşim tanımlama bilgisi SameSite özniteliğini destekliyor mu?
Evet, [Kmıum Browser](https://www.chromium.org/Home) [V80 Update](https://chromiumdash.appspot.com/schedule) , SameSite = LAX olarak değerlendirilmeyeceği bir SAMESITE özniteliği olmadan http tanımlama bilgilerinde bir mantarih getirdi. Bu, Application Gateway benzeşim tanımlama bilgisinin tarayıcı tarafından üçüncü taraf bir bağlamda gönderilemeyeceği anlamına gelir. 

Bu senaryoyu desteklemek için, mevcut *Applicationgatewaybenzeşim* tanımlama bilgisine ek olarak *Applicationgatewayaffinitycors* adlı başka bir tanımlama bilgisini Application Gateway çıkartır.  Bu tanımlama bilgileri benzerdir, ancak *Applicationgatewayaffinitycors* tanımlama bilgisinin kendisine eklenmiş iki özniteliği vardır: *SameSite = None; Güvenli*. Bu öznitelikler, çapraz kaynak istekleri için bile yapışkan oturumları korur. Daha fazla bilgi için [tanımlama bilgisi tabanlı benzeşim bölümüne](configuration-overview.md#cookie-based-affinity) bakın.

## <a name="performance"></a>Performans

### <a name="how-does-application-gateway-support-high-availability-and-scalability"></a>Application Gateway yüksek kullanılabilirliği ve ölçeklenebilirliği nasıl destekler?

Application Gateway v1 SKU 'SU, iki veya daha fazla örnek dağıttığınız zaman yüksek kullanılabilirlik senaryolarını destekler. Azure, örnekleri aynı anda başarısız olmamasını sağlamak için bu örnekleri güncelleştirme ve hata etki alanları arasında dağıtır. V1 SKU 'SU, yükü paylaşmak için aynı ağ geçidinin birden fazla örneğini ekleyerek ölçeklenebilirliği destekler.

V2 SKU 'SU, yeni örneklerin hata etki alanları ve güncelleştirme etki alanları arasında yayılmasını otomatik olarak sağlar. Bölge artıklığı ' nı seçerseniz, en yeni örnekler Ayrıca, Kullanılabilirlik alanlarına göre yayarak hata dayanıklılığı sağlar.

### <a name="how-do-i-achieve-a-dr-scenario-across-datacenters-by-using-application-gateway"></a>Application Gateway kullanarak veri merkezleri arasında bir DR senaryosu elde Nasıl yaparım? mı?

Farklı veri merkezlerinde birden çok uygulama ağ geçidi arasında trafik dağıtmak için Traffic Manager kullanın.

### <a name="does-application-gateway-support-autoscaling"></a>Application Gateway otomatik ölçeklendirmeyi destekliyor mu?

Evet, Application Gateway v2 SKU 'SU otomatik ölçeklendirmeyi destekler. Daha fazla bilgi için bkz. [Otomatik ölçeklendirme ve bölge yedekli Application Gateway](application-gateway-autoscaling-zone-redundant.md).

### <a name="does-manual-or-automatic-scale-up-or-scale-down-cause-downtime"></a>El ile veya otomatik ölçek artırma ya da ölçeği azaltma kapalı kalma süresine neden oluyor?

Hayır. Örnekler, yükseltme etki alanları ve hata etki alanları arasında dağıtılır.

### <a name="does-application-gateway-support-connection-draining"></a>Bağlantı boşaltma Application Gateway destekler mi?

Evet. Bir arka uç havuzu içindeki üyeleri kesintiye uğramadan değiştirmek için bağlantı boşaltma ayarı yapabilirsiniz. Daha fazla bilgi için [Application Gateway bağlantı boşaltma bölümüne](features.md#connection-draining)bakın.

### <a name="can-i-change-instance-size-from-medium-to-large-without-disruption"></a>Örnek boyutunu, kesintiye uğramadan büyük olarak değiştirebilir miyim?

Evet.

## <a name="configuration"></a>Yapılandırma

### <a name="is-application-gateway-always-deployed-in-a-virtual-network"></a>Application Gateway her zaman bir sanal ağda dağıtılır mı?

Evet. Application Gateway her zaman bir sanal ağ alt ağında dağıtılır. Bu alt ağ, yalnızca uygulama ağ geçitleri içerebilir. Daha fazla bilgi için bkz. [sanal ağ ve alt ağ gereksinimleri](https://docs.microsoft.com/azure/application-gateway/configuration-overview#azure-virtual-network-and-dedicated-subnet).

### <a name="can-application-gateway-communicate-with-instances-outside-of-its-virtual-network-or-outside-of-its-subscription"></a>Application Gateway sanal ağının dışındaki veya aboneliğin dışında örneklerle iletişim kurabilir mi?

IP bağlantınız olduğu sürece Application Gateway, içinde bulunduğu sanal ağın dışındaki örneklerle iletişim kurabilir. Application Gateway, içindeki aboneliğin dışındaki örneklerle de iletişim kurabilir. İç IP 'Leri arka uç havuzu üyesi olarak kullanmayı planlıyorsanız, [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md) veya [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)kullanın.

### <a name="can-i-deploy-anything-else-in-the-application-gateway-subnet"></a>Application Gateway alt ağında başka bir şey dağıtabilir miyim?

Hayır. Ancak diğer uygulama ağ geçitlerini alt ağda dağıtabilirsiniz.

### <a name="are-network-security-groups-supported-on-the-application-gateway-subnet"></a>Ağ güvenlik grupları, uygulama ağ geçidi alt ağında destekleniyor mu?

[Application Gateway alt ağındaki ağ güvenlik grupları '](https://docs.microsoft.com/azure/application-gateway/configuration-overview#network-security-groups-on-the-application-gateway-subnet)na bakın.

### <a name="does-the-application-gateway-subnet-support-user-defined-routes"></a>Application Gateway alt ağı Kullanıcı tanımlı yolları destekliyor mu?

[Application Gateway alt ağında desteklenen Kullanıcı tanımlı yollara](https://docs.microsoft.com/azure/application-gateway/configuration-overview#user-defined-routes-supported-on-the-application-gateway-subnet)bakın.

### <a name="what-are-the-limits-on-application-gateway-can-i-increase-these-limits"></a>Application Gateway sınırları nelerdir? Bu limitleri artırabilir miyim?

[Application Gateway sınırlara](../azure-resource-manager/management/azure-subscription-service-limits.md#application-gateway-limits)bakın.

### <a name="can-i-simultaneously-use-application-gateway-for-both-external-and-internal-traffic"></a>Hem dış hem de iç trafik için Application Gateway eşzamanlı olarak kullanabilir miyim?

Evet. Application Gateway, her uygulama ağ geçidi için bir iç IP ve bir dış IP 'yi destekler.

### <a name="does-application-gateway-support-virtual-network-peering"></a>Sanal Ağ eşlemesini desteklemek Application Gateway mı?

Evet. Sanal ağ eşlemesi, diğer sanal ağlardaki trafiğin yükünü dengelemeye yardımcı olur.

### <a name="can-i-talk-to-on-premises-servers-when-theyre-connected-by-expressroute-or-vpn-tunnels"></a>ExpressRoute veya VPN tünellerinden bağlandıklarında şirket içi sunucularla iletişim kurabilir miyim?

Evet, trafiğe izin verilen sürece.

### <a name="can-one-backend-pool-serve-many-applications-on-different-ports"></a>Bir arka uç havuzu farklı bağlantı noktalarında birçok uygulamayı sunabilir mi?

Mikro hizmet mimarisi desteklenir. Farklı bağlantı noktalarında araştırma yapmak için birden çok HTTP ayarı yapılandırmanız gerekir.

### <a name="do-custom-probes-support-wildcards-or-regex-on-response-data"></a>Özel yoklamalar, yanıt verilerinde joker karakter veya Regex destekliyor mu?

Hayır. 

### <a name="how-are-routing-rules-processed-in-application-gateway"></a>Yönlendirme kuralları Application Gateway içinde nasıl işlenir?

Bkz. [işleme kuralları sırası](https://docs.microsoft.com/azure/application-gateway/configuration-overview#order-of-processing-rules).

### <a name="for-custom-probes-what-does-the-host-field-signify"></a>Özel yoklamalar için, ana bilgisayar alanı ne işaret eder?

Konak alanı, Application Gateway üzerinde çok siteli yapılandırdığınız zaman, araştırmanın gönderileceği adı belirtir. Aksi takdirde ' 127.0.0.1 ' kullanın. Bu değer, sanal makine ana bilgisayar adından farklıdır. \<Biçimi protokol\>://\<ana bilgisayar\>:\<bağlantı noktası\>\<yoludur\>.

### <a name="can-i-allow-application-gateway-access-to-only-a-few-source-ip-addresses"></a>Yalnızca birkaç kaynak IP adresine Application Gateway erişimine izin verebilir miyim?

Evet. Bkz. [belirli kaynak IP 'lerine erişimi kısıtlama](https://docs.microsoft.com/azure/application-gateway/configuration-overview#allow-application-gateway-access-to-a-few-source-ips).

### <a name="can-i-use-the-same-port-for-both-public-facing-and-private-facing-listeners"></a>Hem genel hem de özel kullanıma yönelik dinleyiciler için aynı bağlantı noktasını kullanabilir miyim?

Hayır.

### <a name="does-application-gateway-support-ipv6"></a>Application Gateway IPv6 'Yı destekliyor mu?

Application Gateway v2 Şu anda IPv6 'Yı desteklemiyor. Yalnızca IPv4 kullanan bir çift yığın VNet 'te çalışabilir, ancak ağ geçidi alt ağının yalnızca IPv4 olması gerekir. Application Gateway v1 çift yığın VNET 'leri desteklemez. 

### <a name="how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address"></a>Application Gateway v2 'yi yalnızca özel ön uç IP adresi ile kullanmak Nasıl yaparım??

Application Gateway v2 Şu anda yalnızca özel IP modunu desteklemiyor. Aşağıdaki birleşimleri destekler
* Özel IP ve genel IP
* Yalnızca genel IP

Ancak Application Gateway v2 'yi yalnızca özel IP ile kullanmak istiyorsanız, aşağıdaki işlemi izleyebilirsiniz:
1. Hem genel hem de özel ön uç IP adresiyle Application Gateway oluşturun
2. Genel ön uç IP adresi için herhangi bir dinleyici oluşturmayın. Application Gateway, kendisi için bir dinleyici oluşturulmadıysa genel IP adresindeki herhangi bir trafiği dinlemez.
3. Öncelik sırasına göre aşağıdaki yapılandırmaya sahip Application Gateway alt ağı için bir [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/security-overview) oluşturun ve ekleyin:
    
    a. Kaynak olarak **Gatewaymanager** hizmet etiketi ve hedef bağlantı **noktası olarak** **65200-65535**olarak gelen trafiğe izin verin. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları sertifika kimlik doğrulaması tarafından korunur (kilitlidir). Ağ Geçidi Kullanıcı yöneticileri de dahil olmak üzere dış varlıklar, uygun sertifikalara sahip olmayan bu uç noktalar üzerinde değişiklik başlatamaz
    
    b. Kaynak **AzureLoadBalancer** hizmet etiketi ve hedef ve hedef bağlantı **noktası gibi kaynaklardan** gelen trafiğe izin ver
    
    c. Kaynaktan **Internet** hizmet etiketi ve hedef ve hedef bağlantı **noktası olarak gelen**tüm trafiği reddetme. Bu kurala gelen kurallarda *En düşük önceliği* verin
    
    d. Özel IP adresine erişimin engellenmemesi için VirtualNetwork Inbound 'e izin verme gibi varsayılan kuralları koruyun
    
    e. Giden internet bağlantısı engellenmiyor. Aksi takdirde, oturum açma, ölçümler vb. ile ilgili sorunlar olur.

Yalnızca özel IP erişimi için örnek NSG yapılandırması: ![yalnızca özel IP erişimi Için Application Gateway v2 NSG yapılandırması](./media/application-gateway-faq/appgw-privip-nsg.png)

## <a name="configuration---tls"></a>Yapılandırma-TLS

### <a name="what-certificates-does-application-gateway-support"></a>Application Gateway hangi sertifikaları destekler?

Application Gateway, otomatik olarak imzalanan sertifikaları, sertifika yetkilisi (CA) sertifikaları, Genişletilmiş Doğrulama (EV) sertifikaları ve joker karakter sertifikalarını destekler.

### <a name="what-cipher-suites-does-application-gateway-support"></a>Hangi şifreleme paketleri Application Gateway destekler?

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

TLS seçeneklerini özelleştirme hakkında daha fazla bilgi için bkz. [APPLICATION Gateway TLS ilkesi sürümlerini ve şifre paketlerini yapılandırma](application-gateway-configure-ssl-policy-powershell.md).

### <a name="does-application-gateway-support-reencryption-of-traffic-to-the-backend"></a>Application Gateway arka uca trafiği yeniden şifrelemeyi destekliyor mu?

Evet. Application Gateway, arka uca trafiği yeniden şifreleyen TLS yük boşaltma ve uçtan uca TLS 'yi destekler.

### <a name="can-i-configure-tls-policy-to-control-tls-protocol-versions"></a>TLS protokolü sürümlerini denetlemek için TLS ilkesini yapılandırabilir miyim?

Evet. Application Gateway, TLS 1.0, TLS 1.1 ve TLS 1.2 'yi reddedecek şekilde yapılandırabilirsiniz. Varsayılan olarak, SSL 2,0 ve 3,0 zaten devre dışıdır ve yapılandırılamaz.

### <a name="can-i-configure-cipher-suites-and-policy-order"></a>Şifre paketlerini ve ilke sırasını yapılandırabilir miyim?

Evet. Application Gateway, [şifre paketlerini yapılandırabilirsiniz](application-gateway-ssl-policy-overview.md). Özel bir ilke tanımlamak için aşağıdaki şifre paketlerinden en az birini etkinleştirin. 

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

Application Gateway, arka uç yönetimi için SHA256 kullanır.

### <a name="how-many-tlsssl-certificates-does-application-gateway-support"></a>Kaç TLS/SSL sertifikası Application Gateway destekler?

Application Gateway en fazla 100 TLS/SSL sertifikasını destekler.

### <a name="how-many-authentication-certificates-for-backend-reencryption-does-application-gateway-support"></a>Arka uç yeniden şifreleme için kaç kimlik doğrulama sertifikası Application Gateway destekler?

Application Gateway en fazla 100 kimlik doğrulama sertifikasını destekler.

### <a name="does-application-gateway-natively-integrate-with-azure-key-vault"></a>Application Gateway yerel olarak Azure Key Vault tümleştirsin mi?

Evet, Application Gateway v2 SKU 'SU Key Vault destekler. Daha fazla bilgi için bkz. [Key Vault sertifikalarla TLS sonlandırma](key-vault-certs.md).

### <a name="how-do-i-configure-https-listeners-for-com-and-net-sites"></a>. Com ve .net siteleri için HTTPS dinleyicileri yapılandırmak Nasıl yaparım?? 

Birden çok etki alanı tabanlı (ana bilgisayar tabanlı) yönlendirme için çoklu site dinleyicileri oluşturabilir, protokol olarak HTTPS kullanan dinleyicileri ayarlayabilir ve dinleyicileri yönlendirme kurallarıyla ilişkilendirebilirsiniz. Daha fazla bilgi için bkz. [Application Gateway kullanarak birden çok site barındırma](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview).

### <a name="can-i-use-special-characters-in-my-pfx-file-password"></a>. Pfx dosya parolamda özel karakterler kullanabilir miyim?

Hayır,. pfx dosya parolanda yalnızca alfasayısal karakterler kullanın.

## <a name="configuration---web-application-firewall-waf"></a>Yapılandırma-Web uygulaması güvenlik duvarı (WAF)

### <a name="does-the-waf-sku-offer-all-the-features-available-in-the-standard-sku"></a>WAF SKU 'SU standart SKU 'da bulunan tüm özellikleri sunuyor mu?

Evet. WAF, standart SKU 'daki tüm özellikleri destekler.

### <a name="how-do-i-monitor-waf"></a>Nasıl yaparım? İzleyicisi WAF mi?

Tanılama günlük kaydı ile WAF 'yi izleyin. Daha fazla bilgi için bkz. [Application Gateway Için tanılama günlüğü ve ölçümleri](application-gateway-diagnostics.md).

### <a name="does-detection-mode-block-traffic"></a>Algılama modu trafiği engelliyor mu?

Hayır. Algılama modu yalnızca bir WAF kuralını tetikleyen trafiği günlüğe kaydeder.

### <a name="can-i-customize-waf-rules"></a>WAF kurallarını özelleştirebilir miyim?

Evet. Daha fazla bilgi için bkz. [WAF kural gruplarını ve kurallarını özelleştirme](application-gateway-customize-waf-rules-portal.md).

### <a name="what-rules-are-currently-available-for-waf"></a>WAF için şu anda hangi kurallar kullanılabilir?

WAF Şu anda sp_configure [2.2.9](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp229), [3,0](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp30)ve [3,1](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md#owasp31)' i desteklemektedir. Bu kurallar, Web uygulaması güvenlik projesini (OWASP) açan en yüksek 10 güvenlik açığına karşı temel güvenlik sağlar: 

* SQL ekleme koruması
* Siteler arası betik koruması
* Komut ekleme, HTTP isteği, HTTP yanıtı bölme ve uzak dosya ekleme saldırısı gibi yaygın web saldırılarına karşı koruma
* HTTP protokolü ihlallerine karşı koruma
* Eksik konak kullanıcısı-aracısı ve kabul üst bilgileri gibi HTTP protokolü anormalliklerine karşı koruma
* Robotlar, gezginler ve tarayıcıları önleme
* Yaygın uygulama yapılandırmalarını algılama (yani, Apache, IIS vb.)

Daha fazla bilgi için bkz. [OWASP Top-10 güvenlik açıkları](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013).

### <a name="does-waf-support-ddos-protection"></a>WAF, DDoS korumasını destekliyor mu?

Evet. DDoS korumasını, uygulama ağ geçidinin dağıtıldığı sanal ağ üzerinde etkinleştirebilirsiniz. Bu ayar, Azure DDoS Koruması hizmetinin Application Gateway sanal IP 'yi (VIP) de korumasını sağlar.

## <a name="configuration---ingress-controller-for-aks"></a>AKS için yapılandırma girişi denetleyicisi

### <a name="what-is-an-ingress-controller"></a>Giriş denetleyicisi nedir?

Kubernetes, `deployment` ve `service` kaynak oluşturulmasına izin verir. Aynı hizmeti dışarıdan göstermek için, Yük Dengeleme [`Ingress`](https://kubernetes.io/docs/concepts/services-networking/ingress/) , TLS sonlandırma ve ad tabanlı sanal barındırma sağlayan bir kaynak tanımlanmıştır.
Bu `Ingress` kaynağı karşılamak için, `Ingress` kaynaklarda yapılan tüm değişiklikleri dinleyen ve yük dengeleyici Ilkelerini yapılandıran bir giriş denetleyicisi gerekir.

Application Gateway giriş denetleyicisi, [azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) 'nin bir aks kümesi olarak da bilinen bir [Azure Kubernetes hizmeti](https://azure.microsoft.com/services/kubernetes-service/) için giriş olarak kullanılmasına izin verir.

### <a name="can-a-single-ingress-controller-instance-manage-multiple-application-gateways"></a>Tek bir giriş denetleyicisi örneği birden çok uygulama ağ geçidini yönetebilir mi?

Şu anda bir giriş denetleyicisi örneği yalnızca bir Application Gateway ilişkilendirilebilir.

## <a name="diagnostics-and-logging"></a>Tanılama ve günlüğe kaydetme

### <a name="what-types-of-logs-does-application-gateway-provide"></a>Application Gateway ne tür Günlükler sağlar?

Application Gateway üç günlük sağlar: 

* **Applicationgatewayaccesslog**: erişim günlüğü, Application Gateway ön ucunda gönderilen her isteği içerir. Veriler çağıranın IP 'sini, istenen URL 'yi, yanıt gecikmesini, dönüş kodunu ve gelen ve giden baytları içerir. Her uygulama ağ geçidi için bir kayıt içerir.
* **ApplicationGatewayPerformanceLog**: performans günlüğü her uygulama ağ geçidi için performans bilgilerini yakalar. Bilgiler bayt cinsinden aktarım hızını, sunulan toplam istek sayısını, başarısız istek sayısını, sağlıklı ve sağlıksız arka uç örnek sayısını içerir.
* **Applicationgatewayfirewalllog**: WAF ile yapılandırdığınız uygulama ağ geçitleri için, güvenlik duvarı günlüğü, algılama modu ya da önleme modu aracılığıyla günlüğe kaydedilen istekleri içerir.

Tüm Günlükler her 60 saniyede toplanır. Daha fazla bilgi için bkz. [Application Gateway Için arka uç sistem durumu, tanılama günlükleri ve ölçümler](application-gateway-diagnostics.md).

### <a name="how-do-i-know-if-my-backend-pool-members-are-healthy"></a>Nasıl yaparım?, arka uç havuzu Üyelerimin sağlıklı olup olmadığını bilmelidir mi?

PowerShell cmdlet 'ini `Get-AzApplicationGatewayBackendHealth` veya portalını kullanarak sistem durumunu doğrulayın. Daha fazla bilgi için bkz. [tanılama Application Gateway](application-gateway-diagnostics.md).

### <a name="whats-the-retention-policy-for-the-diagnostic-logs"></a>Tanılama günlükleri için bekletme ilkesi nedir?

Tanılama günlükleri müşterinin depolama hesabına akar. Müşteriler, saklama ilkesini tercihe göre ayarlayabilir. Tanılama günlükleri, bir olay hub 'ına veya Azure Izleyici günlüklerine de gönderilebilir. Daha fazla bilgi için bkz. [tanılama Application Gateway](application-gateway-diagnostics.md).

### <a name="how-do-i-get-audit-logs-for-application-gateway"></a>Application Gateway için denetim günlükleri Nasıl yaparım? mi?

Portalda, bir uygulama ağ geçidinin menü dikey penceresinde, denetim günlüğüne erişmek için **etkinlik günlüğü** ' nü seçin. 

### <a name="can-i-set-alerts-with-application-gateway"></a>Uyarıları Application Gateway ayarlayabilir miyim?

Evet. Application Gateway, ölçümler üzerinde uyarılar yapılandırılır. Daha fazla bilgi için bkz. [Application Gateway ölçümleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics) ve [uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

### <a name="how-do-i-analyze-traffic-statistics-for-application-gateway"></a>Application Gateway için trafik istatistiklerini analiz Nasıl yaparım??

Erişim günlüklerini çeşitli yollarla görüntüleyebilir ve analiz edebilirsiniz. Azure Izleyici günlüklerini, Excel, Power BI vb. kullanın.

Ayrıca, Application Gateway erişim günlükleri için popüler [Goaccess](https://goaccess.io/) Log Analyzer 'ı yükleyen ve çalıştıran bir kaynak yöneticisi şablonu da kullanabilirsiniz. GoAccess benzersiz ziyaretçiler, istenen dosyalar, konaklar, işletim sistemleri, tarayıcılar ve HTTP durum kodları gibi değerli HTTP trafiği istatistiklerini sağlar. GitHub 'da daha fazla bilgi için, [Kaynak Yöneticisi Şablon klasöründeki Readme dosyasına](https://aka.ms/appgwgoaccessreadme)bakın.

### <a name="what-could-cause-backend-health-to-return-an-unknown-status"></a>Arka uç durumunun bilinmeyen bir durum döndürmesine neden olabilir?

Genellikle, arka uca erişim bir ağ güvenlik grubu (NSG), özel DNS veya uygulama ağ geçidi alt ağında Kullanıcı tanımlı yönlendirme (UDR) tarafından engellendiğinde bilinmeyen bir durum görürsünüz. Daha fazla bilgi için bkz. [Application Gateway Için arka uç sistem durumu, tanılama günlüğü ve ölçümler](application-gateway-diagnostics.md).

### <a name="is-there-any-case-where-nsg-flow-logs-wont-show-allowed-traffic"></a>NSG akış günlüklerinin izin verilen trafiği göstermeyeceği bir durum var mı?

Evet. Yapılandırmanız aşağıdaki senaryoda eşleşiyorsa NSG akış günlüklerinizin izin verilen trafiğini görmezsiniz:
- Application Gateway v2 'yi dağıttıysanız
- Application Gateway alt ağında bir NSG var
- NSG akış günlüklerini bu NSG 'de etkinleştirdiniz

### <a name="how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address"></a>Application Gateway v2 'yi yalnızca özel ön uç IP adresi ile kullanmak Nasıl yaparım??

Application Gateway v2 Şu anda yalnızca özel IP modunu desteklemiyor. Aşağıdaki birleşimleri destekler
* Özel IP ve genel IP
* Yalnızca genel IP

Ancak Application Gateway v2 'yi yalnızca özel IP ile kullanmak istiyorsanız, aşağıdaki işlemi izleyebilirsiniz:
1. Hem genel hem de özel ön uç IP adresiyle Application Gateway oluşturun
2. Genel ön uç IP adresi için herhangi bir dinleyici oluşturmayın. Application Gateway, kendisi için bir dinleyici oluşturulmadıysa genel IP adresindeki herhangi bir trafiği dinlemez.
3. Öncelik sırasına göre aşağıdaki yapılandırmaya sahip Application Gateway alt ağı için bir [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/security-overview) oluşturun ve ekleyin:
    
    a. Kaynak olarak **Gatewaymanager** hizmet etiketi ve hedef bağlantı **noktası olarak** **65200-65535**olarak gelen trafiğe izin verin. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları sertifika kimlik doğrulaması tarafından korunur (kilitlidir). Ağ Geçidi Kullanıcı yöneticileri de dahil olmak üzere dış varlıklar, uygun sertifikalara sahip olmayan bu uç noktalar üzerinde değişiklik başlatamaz
    
    b. Kaynak olarak **AzureLoadBalancer** hizmet etiketi ve hedef bağlantı noktası olarak trafiğe izin **Any** ver
    
    c. Kaynaktaki tüm gelen trafiği **Internet** hizmet etiketi ve hedef bağlantı **noktası olarak reddet.** Bu kurala gelen kurallarda *En düşük önceliği* verin
    
    d. Özel IP adresi erişiminin engellenmemesi için VirtualNetwork Inbound 'e izin verme gibi varsayılan kuralları koruyun
    
    e. Giden internet bağlantısı engellenmiyor. Aksi takdirde, oturum açma, ölçümler vb. ile ilgili sorunlar olur.

Yalnızca özel IP erişimi için örnek NSG yapılandırması: ![yalnızca özel IP erişimi Için Application Gateway v2 NSG yapılandırması](./media/application-gateway-faq/appgw-privip-nsg.png)

### <a name="does-application-gateway-affinity-cookie-support-samesite-attribute"></a>Application Gateway benzeşim tanımlama bilgisi SameSite özniteliğini destekliyor mu?
Evet, [Kmıum Browser](https://www.chromium.org/Home) [V80 Update](https://chromiumdash.appspot.com/schedule) , SameSite = LAX olarak değerlendirilmeyeceği bir SAMESITE özniteliği olmadan http tanımlama bilgilerinde bir mantarih getirdi. Bu, Application Gateway benzeşim tanımlama bilgisinin tarayıcı tarafından üçüncü taraf bir bağlamda gönderilemeyeceği anlamına gelir. Bu senaryoyu desteklemek için, mevcut *Applicationgatewaybenzeşim* tanımlama bilgisine ek olarak *Applicationgatewayaffinitycors* adlı başka bir tanımlama bilgisini Application Gateway çıkartır.  Bu tanımlama bilgileri benzerdir, ancak *Applicationgatewayaffinitycors* tanımlama bilgisinin kendisine eklenmiş iki özniteliği vardır: *SameSite = None; Güvenli*. Bu öznitelikler, çapraz kaynak istekleri için bile yapışkan oturumları korur. Daha fazla bilgi için [tanımlama bilgisi tabanlı benzeşim bölümüne](configuration-overview.md#cookie-based-affinity) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway hakkında daha fazla bilgi edinmek için bkz. [Azure Application Gateway nedir?](overview.md).
