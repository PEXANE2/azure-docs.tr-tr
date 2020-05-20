---
title: Azure Application Gateway uçtan uca TLS 'yi etkinleştirme
description: Bu makale, uçtan uca TLS desteğine Application Gateway bir genel bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 5/13/2020
ms.author: victorh
ms.openlocfilehash: adaf3dea5855a4af75977cb820ae12675c7f2ced
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648126"
---
# <a name="overview-of-tls-termination-and-end-to-end-tls-with-application-gateway"></a>Application Gateway ile TLS sonlandırmasına ve uçtan uca TLS 'ye Genel Bakış

Daha önce Güvenli Yuva Katmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS), bir Web sunucusu ve tarayıcı arasında şifrelenmiş bir bağlantı kurmak için standart güvenlik teknolojisidir. Bu bağlantı, Web sunucusu ve tarayıcıları arasında geçirilen tüm verilerin özel ve şifreli kalmasını sağlar. Application Gateway, ağ geçidinde hem TLS sonlandırmasını hem de uçtan uca TLS şifrelemesini destekler.

## <a name="tls-termination"></a>TLS sonlandırma

Application Gateway, ağ geçidinde TLS sonlandırmasını destekler, bu da akış genellikle arka uç sunucularına şifrelenmemiş olarak akar. Uygulama ağ geçidinde TLS sonlandırmanın çeşitli avantajları vardır:

- **İyileştirilmiş performans** : TLS şifre çözme işlemi gerçekleştirirken en büyük performans isabet ilk anlaşma olur. Performansı artırmak için şifre çözme yapan sunucu TLS oturum kimliklerini önbelleğe alır ve TLS oturum biletlerini yönetir. Uygulama ağ geçidinde Bu yapıldığında, aynı istemciden gelen tüm istekler önbelleğe alınmış değerleri kullanabilir. Arka uç sunucuları üzerinde yapıldıysa, istemci istekleri her seferinde farklı bir sunucuya her gitilişinde, istemcinin kimliği yeniden doğrulanır. TLS biletleri kullanımı bu sorunu azaltmaya yardımcı olabilir, ancak tüm istemciler tarafından desteklenmez ve yapılandırılması ve yönetilmesi zor olabilir.
- **Arka uç sunucularının daha iyi kullanımı** – SSL/TLS Işleme çok CPU yoğun ve anahtar boyutları artdıkça daha yoğun hale geliyor. Bu çalışmanın arka uç sunucularından kaldırılması, içerik teslimi için en verimli olduklarında odaklanmalarını sağlar.
- **Akıllı yönlendirme** : trafiğin şifresini çözerek, uygulama ağ geçidinin üst BILGILER, URI gibi istek içeriğine erişimi vardır ve bu verileri istekleri yönlendirmek için kullanabilir.
- **Sertifika yönetimi** – sertifikaların yalnızca uygulama ağ geçidinde satın alınması ve yüklenmesi gerekir ve bu, arka uç sunucuları için kullanılamaz. Bu hem zaman hem de para tasarrufu sağlar.

TLS sonlandırmayı yapılandırmak için Application Gateway bir TLS/SSL sertifikasının, bir simetrik anahtarı TLS/SSL protokolü belirtimine göre türemesini sağlamak için dinleyiciye eklenmesi gerekir. Daha sonra simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. TLS/SSL sertifikasının kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, trafiğin şifrelemesini ve şifresini çözmeyi gerçekleştirmek üzere uygulama ağ geçidi için gereken özel anahtarı dışarı aktarma olanağı sağlar.

> [!IMPORTANT] 
> Lütfen dinleyicide bulunan sertifikanın tüm Sertifika zincirinin karşıya yüklenmesini gerektirdiğini unutmayın. 


> [!NOTE] 
>
> Application Gateway, yeni bir sertifika oluşturmak veya bir sertifika yetkilisine sertifika isteği göndermek için herhangi bir özellik sağlamaz.

TLS bağlantısının çalışması için TLS/SSL sertifikasının aşağıdaki koşulları karşıladığından emin olmanız gerekir:

- Geçerli tarih ve saatin, sertifikadaki "geçerlilik tarihi" ve "geçerli bitiş" tarih aralığı içinde olması.
- Sertifikanın "Ortak Ad" (CN) değeri, istekteki ana bilgisayar üst bilgisiyle eşleşiyor. Örneğin istemci isteğinin hedefi `https://www.contoso.com/` ise CN `www.contoso.com` olmalıdır.

### <a name="certificates-supported-for-tls-termination"></a>TLS sonlandırma için desteklenen sertifikalar

Application Gateway aşağıdaki sertifika türlerini destekler:

- CA (sertifika yetkilisi) sertifikası: CA sertifikası, bir sertifika yetkilisi (CA) tarafından verilen dijital bir sertifikadır
- EV (Genişletilmiş Doğrulama) sertifikası: EV sertifikası, sektör standardı sertifika yönergelerine uygun bir sertifikadır. Bu, tarayıcı Konumlandırıcı çubuğunun yeşil olacağını açıp şirket adını da yayımlayacak.
- Joker karakter sertifikası: Bu sertifika, alt etki alanınızın * yerini alacak *. site.com bağlı olarak herhangi bir sayıda alt etki alanını destekler. Ancak, site.com desteği yoktur, bu nedenle kullanıcıların Web sitenize önde gelen "www" yazısı olmadan erişmesi durumunda joker karakter sertifikası bunu kapsamaz.
- Otomatik olarak Imzalanan Sertifikalar: Istemci tarayıcıları bu sertifikalara güvenmez ve sanal hizmetin sertifikasının bir güven zincirinin parçası olmadığı konusunda kullanıcıyı uyarır. Otomatik olarak imzalanan sertifikalar, yöneticilerin istemcileri denetladığı ve tarayıcının güvenlik uyarılarını güvenle atlayabileceği test veya ortamlar için uygundur. Üretim iş yükleri hiçbir şekilde otomatik olarak imzalanan sertifikalar kullanmamalıdır.

Daha fazla bilgi için bkz. [Application Gateway Ile TLS sonlandırmayı yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="size-of-the-certificate"></a>Sertifikanın boyutu
Desteklenen en yüksek TLS/SSL sertifika boyutunu bildirmek için [Application Gateway sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#application-gateway-limits) bölümünü denetleyin.

## <a name="end-to-end-tls-encryption"></a>Uçtan uca TLS şifrelemesi

Arka uç sunucuları için şifrelenmemiş iletişim istemiyor olabilirsiniz. Güvenlik gereksinimleriniz, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bir bağlantı kabul etmesi olabilir. Azure Application Gateway, bu gereksinimleri desteklemek için uçtan uca TLS şifrelemesi içerir.

Uçtan uca TLS, Application Gateway katman 7 Yük Dengeleme özelliklerini kullanırken hassas verileri, arka uca şifrelemeyi ve güvenli bir şekilde aktarmanıza olanak tanır. Bu özellikler, tanımlama bilgisi tabanlı oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği, X-Iletilen-* üst bilgilerini yeniden yazma veya ekleme olanağı içerir.

Uçtan uca TLS iletişim modu ile yapılandırıldığında, Application Gateway ağ geçidinde TLS oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Application Gateway, arka uç sunucusuna yeni bir TLS bağlantısı başlatır ve isteği arka uca iletmeden önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler. Uçtan uca TLS, [arka uç http ayarında](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) protokol ayarı https olarak ayarlanarak etkinleştirilir ve bu daha sonra arka uç havuzuna uygulanır.

Application Gateway ve WAF v1 SKU 'SU için TLS ilkesi hem ön uç hem de arka uç trafiği için geçerlidir. Ön uçta, Application Gateway sunucu olarak davranır ve ilkeyi uygular. Arka uçta Application Gateway istemci olarak davranır ve TLS el sıkışması sırasında iletişim kuralı/şifreleme bilgilerini tercih olarak gönderir.

Application Gateway ve WAF v2 SKU 'SU için, TLS ilkesi yalnızca ön uç trafiği için geçerlidir ve tüm şifrelemeler, el sıkışma sırasında belirli şifreleme ve TLS sürümünü seçme denetimi olan arka uç sunucusuna sunulur.

Application Gateway, yalnızca Application Gateway sertifikası olan veya sertifikaları iyi bilinen CA yetkilileri tarafından imzalanmış veya sertifikanın CN 'si, HTTP arka uç ayarlarındaki ana bilgisayar adıyla eşleşen arka uç sunucularıyla iletişim kurar. Bunlara Azure App Service/Web Apps ve Azure API Management gibi güvenilir Azure hizmetleri dahildir.

Arka uç havuzundaki üyelerin sertifikaları iyi bilinen CA yetkilileri tarafından imzalanmamışsa, arka uç havuzundaki her bir örnek için uçtan uca TLS etkinleştirilir, güvenli iletişime izin veren bir sertifikayla yapılandırılmalıdır. Sertifikayı eklemek, Application Gateway 'in yalnızca bilinen arka uç örnekleriyle iletişim kuracağını sağlar. Bu, uçtan uca iletişimin güvenliğini sağlar.

> [!NOTE] 
>
> Arka uç sunucularının kimliğini doğrulamak için **arka uç http ayarına** eklenen sertifika, Application Gateway 'de TLS sonlandırma için **dinleyiciye** eklenen sertifikayla aynı veya geliştirilmiş güvenlik için farklı olabilir.

![uçtan uca TLS senaryosu][1]

Bu örnekte, TLS 1.2 kullanan istekler, uçtan uca TLS kullanılarak Pool1 içinde arka uç sunucularına yönlendirilir.

## <a name="end-to-end-tls-and-whitelisting-of-certificates"></a>Uçtan uca TLS ve sertifikaların beyaz listesi

Application Gateway, yalnızca uygulama ağ geçidi ile sertifikaları beyaz listelenmiş bilinen arka uç örnekleriyle iletişim kurar. Uçtan uca TLS kurulum işleminde kullanılan Application Gateway sürümüne göre bazı farklılıklar vardır. Aşağıdaki bölümde ayrı ayrı açıklanmaktadır.

## <a name="end-to-end-tls-with-the-v1-sku"></a>V1 SKU 'SU ile uçtan uca TLS

Arka uç sunucularıyla uçtan uca TLS 'yi etkinleştirmek ve istekleri bunlara yönlendirmek için Application Gateway için, sistem durumu araştırmalarının başarılı olması ve sağlıklı yanıt döndürmesi gerekir.

HTTPS sistem durumu araştırmaları için Application Gateway v1 SKU 'SU, HTTP ayarlarına yüklenecek kimlik doğrulama sertifikasıyla (kök sertifika için ortak anahtar) tam eşleşmeyi kullanır.

Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geri kalan arka uçlar sistem durumu araştırmalarının sağlıksız olduğu kabul edilir. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bu tür sertifikalar, kullanılmadan önce önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle beyaz listeye eklenmelidir.

> [!NOTE]
> Azure App Service gibi güvenilir Azure hizmetleri için kimlik doğrulaması ve güvenilen kök sertifika kurulumu gerekli değildir. Bunlar varsayılan olarak güvenilir olarak değerlendirilir.

## <a name="end-to-end-tls-with-the-v2-sku"></a>V2 SKU 'SU ile uçtan uca TLS

Kimlik doğrulama sertifikaları kullanım dışı bırakılmıştır ve Application Gateway v2 SKU 'sunda güvenilen kök sertifikalarla değiştirilmiştir. Bunlar benzer şekilde, birkaç temel farklılık ile kimlik doğrulama sertifikalarına benzer şekilde çalışır:

- HTTP arka uç ayarlarındaki ana bilgisayar adıyla eşleşen ve bilinen CA yetkilileri tarafından imzalanan sertifikaların, uçtan uca TLS 'nin çalışması için ek bir adım gerekmez. 

   Örneğin, arka uç sertifikaları tanınmış bir CA tarafından verildiyse ve bir contoso.com CN 'si varsa ve arka uç http ayarının ana bilgisayar alanı da contoso.com olarak ayarlanmışsa ek bir adım gerekmez. Arka uç http ayar protokolünü HTTPS olarak ayarlayabilirsiniz ve hem durum araştırması hem de veri yolu TLS etkin olur. Arka ucunuz olarak Azure App Service veya diğer Azure Web hizmetlerini kullanıyorsanız, bunlar da örtülü olarak güvenilirdir ve uçtan uca TLS için başka bir adım gerekmez.
   
> [!NOTE] 
>
> Bir TLS/SSL sertifikasının güvenilir olması için, arka uç sunucusunun bu sertifikasının iyi bilinen bir CA tarafından verilmiş olması gerekir. Sertifika, güvenilir bir CA tarafından yayımlanmazsa, uygulama ağ geçidi, sertifika veren CA sertifikasının güvenilen bir CA tarafından verildiğini ve bu şekilde güvenilen bir CA 'nın bulunana (güvenilir, güvenli bağlantı kurulabileceği) veya güvenilir bir CA 'nın bulununcaya kadar (Bu noktada, Application Gateway 'in arka ucu sağlıksız olduğunu işaretleyecek şekilde) kontrol eder. Bu nedenle, arka uç sunucu sertifikasının hem kök hem de ara CA 'Ları içermesi önerilir.

- Sertifika kendinden imzalıysa veya bilinmeyen aracılar tarafından imzalanmışsa, v2 SKU 'sunda uçtan uca TLS 'yi etkinleştirmek için güvenilen bir kök sertifika tanımlanmalıdır. Application Gateway yalnızca, sunucu sertifikasının kök sertifikası, havuzla ilişkili arka uç http ayarındaki güvenilen kök sertifikalar listesinden biriyle eşleşen arka uçları ile iletişim kurar.

- Kök sertifika eşleştirmesinin yanı sıra, Application Gateway v2, arka uç http ayarında belirtilen ana bilgisayar ayarının, arka uç sunucusunun TLS/SSL sertifikası tarafından sunulan ortak ad (CN) ile eşleşip eşleşmediğini da doğrular. Application Gateway v2, arka uca bir TLS bağlantısı kurmaya çalışırken, arka uç http ayarında belirtilen konağa Sunucu Adı Belirtme (SNı) uzantısını ayarlar.

- Arka uç http ayarında konak alanı yerine **arka uç adresinden seçim ana bilgisayar adı** seçilirse, SNI üstbilgisi her zaman arka uç havuzu FQDN 'sine ayarlanır ve arka uç sunucusu TLS/SSL sertifikasındaki CN 'nın FQDN 'siyle eşleşmesi gerekir. Bu senaryoda, IP ile arka uç havuzu üyeleri desteklenmez.

- Kök sertifika, arka uç sunucu sertifikalarından gelen Base64 kodlamalı bir kök sertifikadır.

## <a name="sni-differences-in-the-v1-and-v2-sku"></a>V1 ve v2 SKU 'sunda SNı farkları

Daha önce bahsedildiği gibi, Application Gateway Application Gateway dinleyicisinden istemciden TLS trafiğini sonlandırır (ön uç bağlantısını çağıralım), trafiğin şifresini çözer, isteğin iletilmesi için gereken arka uç sunucusunu belirleyen gerekli kuralları uygular ve arka uç sunucusuyla yeni bir TLS oturumu kurar (bunu arka uç bağlantısı arayalım).

Aşağıdaki tablolarda, ön uç ve arka uç bağlantıları bakımından v1 ve v2 SKU 'SU arasındaki SNı arasındaki farklar ana hatlarıyla verilmiştir.

### <a name="frontend-tls-connection-client-to-application-gateway"></a>Ön uç TLS bağlantısı (istemciden uygulama ağ geçidine)

---
Senaryo | v1 | v2 |
| --- | --- | --- |
| İstemci SNı üstbilgisini belirtiyorsa ve tüm çoklu site dinleyicileri "SNı gerektir" bayrağıyla etkinleştirilirse | Uygun sertifikayı döndürün ve site yoksa (server_name göre), bağlantı sıfırlanır. | Varsa uygun sertifikayı döndürür, aksi takdirde yapılandırılan ilk HTTPS dinleyicisinin sertifikasını döndürür (sırasıyla)|
| İstemci bir SNı üstbilgisi belirtmezse ve tüm çoklu site başlıkları "SNı gerektir" ile etkinleştirildiyse | Bağlantıyı sıfırlar | Yapılandırılan ilk HTTPS dinleyicisinin sertifikasını döndürür (sırasıyla)
| İstemci SNı üstbilgisini belirtmezse ve bir sertifikayla yapılandırılmış temel bir dinleyici varsa | İstemciye temel dinleyicide yapılandırılan sertifikayı döndürür (varsayılan veya geri dönüş sertifikası) | Yapılandırılan ilk HTTPS dinleyicisinin sertifikasını döndürür (sırasıyla) |

### <a name="backend-tls-connection-application-gateway-to-the-backend-server"></a>Arka uç TLS bağlantısı (arka uç sunucusuna uygulama ağ geçidi)

#### <a name="for-probe-traffic"></a>Araştırma trafiği için

---
Senaryo | v1 | v2 |
| --- | --- | --- |
| SNı (server_name) TLS anlaşması sırasında FQDN olarak üst bilgi | Arka uç havuzundan FQDN olarak ayarlayın. [RFC 6066](https://tools.ietf.org/html/rfc6066)' ye kadar, SNI ana bilgisayar adı 'Nda değişmez IPv4 ve IPv6 adreslerine izin verilmez. <br> **Note:** Arka uç havuzundaki FQDN DNS, arka uç sunucusunun IP adresine (genel veya özel) çözümlenmelidir | SNı üstbilgisi (server_name), HTTP ayarlarına eklenen özel araştırmanın ana bilgisayar adı olarak ayarlanır (yapılandırıldıysa), aksi takdirde, arka uç havuzunda belirtilen FQDN 'den Aksi halde HTTP ayarlarında belirtilen ana bilgisayar adı. Öncelik sırası, HTTP ayarları > arka uç havuzu > özel araştırmanız. <br> **Note:** HTTP ayarlarında yapılandırılan ana bilgisayar adları ve özel yoklama farklıysa, önceliğe göre SNı, özel araştırmadaki ana bilgisayar adı olarak ayarlanır.
| Arka uç havuzu adresi bir IP adresi (v1) ise veya özel araştırma ana bilgisayar adı IP adresi (v2) olarak yapılandırılmışsa | SNı (server_name) ayarlanmayacak. <br> **Note:** Bu durumda, arka uç sunucusu bir varsayılan/geri dönüş sertifikası döndürebilmelidir ve bu, kimlik doğrulama sertifikası altındaki HTTP ayarları 'nda beyaz listeye eklenmelidir. Arka uç sunucusunda yapılandırılmış bir varsayılan/geri dönüş sertifikası yoksa ve SNı bekleniyorsa, sunucu bağlantıyı sıfırlayabilir ve araştırma hatalarına neden olur | Daha önce bahsedilen öncelik sırasına göre IP adresi ana bilgisayar adı ise, SNı, [RFC 6066](https://tools.ietf.org/html/rfc6066)' e göre ayarlanmayacaktır. <br> **Note:** Özel bir araştırma yapılandırılmamışsa ve HTTP ayarları veya arka uç havuzunda ana bilgisayar adı ayarlanmamışsa SNı v2 araştırmasına de ayarlanmayacaktır |

> [!NOTE] 
> Özel bir araştırma yapılandırılmamışsa Application Gateway bu biçimde varsayılan bir araştırma gönderir- \< Protokol \> ://127.0.0.1: \< bağlantı noktası \> /. Örneğin, varsayılan bir HTTPS araştırması için, olarak gönderilir https://127.0.0.1:443/ . Burada bahsedilen 127.0.0.1 yalnızca HTTP ana bilgisayar üst bilgisi olarak kullanıldığını ve RFC 6066 ' e göre, SNı üstbilgisi olarak kullanılmayacağını unutmayın. Durum araştırma hataları hakkında daha fazla bilgi için [arka uç sistem durumu sorun giderme kılavuzunu](application-gateway-backend-health-troubleshooting.md)denetleyin.

#### <a name="for-live-traffic"></a>Canlı trafik için

---
Senaryo | v1 | v2 |
| --- | --- | --- |
| SNı (server_name) TLS anlaşması sırasında FQDN olarak üst bilgi | Arka uç havuzundan FQDN olarak ayarlayın. [RFC 6066](https://tools.ietf.org/html/rfc6066)' ye kadar, SNI ana bilgisayar adı 'Nda değişmez IPv4 ve IPv6 adreslerine izin verilmez. <br> **Note:** Arka uç havuzundaki FQDN DNS, arka uç sunucusunun IP adresine (genel veya özel) çözümlenmelidir | SNı üstbilgisi (server_name), HTTP ayarlarından ana bilgisayar adı olarak ayarlanır, aksi halde *Pickhostnamefrombackendadddress* seçeneği seçilirse veya hiçbir ana bilgisayar adı belirtilmemişse, arka uç havuzu yapılandırmasında FQDN olarak ayarlanır.
| Arka uç havuzu adresi bir IP adresi veya ana bilgisayar adı HTTP ayarları 'nda ayarlanmamışsa | Arka uç havuz girişi bir FQDN değilse, SNı, [RFC 6066](https://tools.ietf.org/html/rfc6066) ' a göre ayarlanmayacaktır | SNı, istemciden giriş FQDN 'sinden ana bilgisayar adı olarak ayarlanacak ve arka uç sertifikasının bu ana bilgisayar adıyla eşleşmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca TLS hakkında bilgi aldıktan sonra uçtan uca TLS kullanarak bir uygulama ağ geçidi oluşturmak için [PowerShell ile Application Gateway kullanarak uçtan uca TLS 'Yi yapılandırma](application-gateway-end-to-end-ssl-powershell.md) bölümüne gidin.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
