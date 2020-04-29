---
title: Azure Application Gateway uçtan uca TLS 'yi etkinleştirme
description: Bu makale, uçtan uca TLS desteğine Application Gateway bir genel bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: ae80b49c3bfb40743665768622d3f4a8a6990c12
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311859"
---
# <a name="overview-of-tls-termination-and-end-to-end-tls-with-application-gateway"></a>Application Gateway ile TLS sonlandırmasına ve uçtan uca TLS 'ye Genel Bakış

Daha önce Güvenli Yuva Katmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS), bir Web sunucusu ve tarayıcı arasında şifrelenmiş bir bağlantı kurmak için standart güvenlik teknolojisidir. Bu bağlantı, Web sunucusu ve tarayıcıları arasında geçirilen tüm verilerin özel ve şifreli kalmasını sağlar. Application Gateway, ağ geçidinde hem TLS sonlandırmasını hem de uçtan uca TLS şifrelemesini destekler.

## <a name="tls-termination"></a>TLS sonlandırma

Application Gateway, ağ geçidinde TLS sonlandırmasını destekler, bu da akış genellikle arka uç sunucularına şifrelenmemiş olarak akar. Uygulama ağ geçidinde TLS sonlandırmanın çeşitli avantajları vardır:

- **İyileştirilmiş performans** : TLS şifre çözme işlemi gerçekleştirirken en büyük performans isabet ilk anlaşma olur. Performansı artırmak için şifre çözme yapan sunucu TLS oturum kimliklerini önbelleğe alır ve TLS oturum biletlerini yönetir. Uygulama ağ geçidinde Bu yapıldığında, aynı istemciden gelen tüm istekler önbelleğe alınmış değerleri kullanabilir. Arka uç sunucuları üzerinde yapıldıysa, istemci istekleri her seferinde farklı bir sunucuya her gitilişinde, istemcinin kimliği yeniden doğrulanır. TLS biletleri kullanımı bu sorunu azaltmaya yardımcı olabilir, ancak tüm istemciler tarafından desteklenmez ve yapılandırılması ve yönetilmesi zor olabilir.
- **Arka uç sunucularının daha iyi kullanımı** – SSL/TLS Işleme çok CPU yoğun ve anahtar boyutları artdıkça daha yoğun hale geliyor. Bu çalışmanın arka uç sunucularından kaldırılması, içerik teslimi için en verimli olduklarında odaklanmalarını sağlar.
- **Akıllı yönlendirme** : trafiğin şifresini çözerek, uygulama ağ geçidinin üst BILGILER, URI gibi istek içeriğine erişimi vardır ve bu verileri istekleri yönlendirmek için kullanabilir.
- **Sertifika yönetimi** – sertifikaların yalnızca uygulama ağ geçidinde satın alınması ve yüklenmesi gerekir ve bu, arka uç sunucuları için kullanılamaz. Bu hem zaman hem de para tasarrufu sağlar.

TLS sonlandırmayı yapılandırmak için, uygulama ağ geçidinin, TLS/SSL protokol belirtimine göre bir simetrik anahtar türemesini sağlamak üzere dinleyiciye bir TLS/SSL sertifikası eklenmesi gerekir. Daha sonra simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. TLS/SSL sertifikasının kişisel bilgi değişimi (PFX) biçiminde olması gerekir. Bu dosya biçimi, trafiğin şifrelemesini ve şifresini çözmeyi gerçekleştirmek üzere uygulama ağ geçidi için gereken özel anahtarı dışarı aktarma olanağı sağlar.

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

Bazı müşteriler arka uç sunucularıyla şifrelenmemiş iletişim olmayabilir. Bunun nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Uygulama ağ geçidi, bu tür uygulamalarda uçtan uca TLS şifrelemesini destekler.

Uçtan uca TLS, önemli verileri arka uca şifrelenmiş olarak güvenli bir şekilde iletmenizi sağlarken, Application Gateway 'in sağladığı katman 7 Yük Dengeleme özelliklerinin avantajlarından faydalanarak devam edebilir. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway, uçtan uca TLS iletişim modu ile yapılandırıldığında, ağ geçidinde TLS oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir TLS bağlantısı başlatır ve isteği arka uca iletmeden önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler. Uçtan uca TLS, [arka uç http ayarında](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) protokol ayarı https olarak ayarlanarak etkinleştirilir ve bu daha sonra arka uç havuzuna uygulanır.

TLS ilkesi hem ön uç hem de arka uç trafiği için geçerlidir. Ön uçta, Application Gateway sunucu olarak davranır ve ilkeyi uygular. Arka uçta Application Gateway istemci olarak davranır ve TLS el sıkışması sırasında iletişim kuralı/şifreleme bilgilerini tercih olarak gönderir.

Application Gateway yalnızca uygulama ağ geçidi ile sertifikası olan veya sertifikaları, sertifika CN 'nin HTTP arka uç ayarlarındaki ana bilgisayar adıyla eşleştiği bilinen CA yetkilileri tarafından imzalanmış olan arka uç örnekleriyle iletişim kurar. Bunlar Azure App Service Web Apps ve Azure API Management gibi güvenilir Azure hizmetlerini içerir.

Arka uç havuzundaki üyelerin sertifikaları iyi bilinen CA yetkilileri tarafından imzalanmamışsa, arka uç havuzundaki her bir örnek için uçtan uca TLS etkinleştirilir, güvenli iletişime izin veren bir sertifikayla yapılandırılmalıdır. Sertifikayı eklemek, Application Gateway 'in yalnızca bilinen arka uç örnekleriyle iletişim kuracağını sağlar. Bu, uçtan uca iletişimin güvenliğini sağlar.

> [!NOTE] 
>
> Azure App Service Web Apps ve Azure API Management gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekli değildir.

> [!NOTE] 
>
> Arka uç sunucularının kimliğini doğrulamak için **arka uç http ayarına** eklenen sertifika, Application Gateway 'de TLS sonlandırma için **dinleyiciye** eklenen sertifikayla aynı veya geliştirilmiş güvenlik için farklı olabilir.

![uçtan uca TLS senaryosu][1]

Bu örnekte, TLS 1.2 kullanan istekler, uçtan uca TLS kullanılarak Pool1 içinde arka uç sunucularına yönlendirilir.

## <a name="end-to-end-tls-and-whitelisting-of-certificates"></a>Uçtan uca TLS ve sertifikaların beyaz listesi

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bu tür sertifikalar, kullanılmadan önce önceki adımlarda açıklandığı şekilde uygulama ağ geçidiyle beyaz listeye eklenmelidir.

> [!NOTE]
> Azure App Service gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekli değildir.

## <a name="end-to-end-tls-with-the-v2-sku"></a>V2 SKU 'SU ile uçtan uca TLS

Kimlik doğrulama sertifikaları kullanım dışı bırakılmıştır ve Application Gateway v2 SKU 'sunda güvenilen kök sertifikalarla değiştirilmiştir. Bunlar benzer şekilde, birkaç temel farklılık ile kimlik doğrulama sertifikalarına benzer şekilde çalışır:

- HTTP arka uç ayarlarındaki ana bilgisayar adıyla eşleşen ve bilinen CA yetkilileri tarafından imzalanan sertifikaların, uçtan uca TLS 'nin çalışması için ek bir adım gerekmez. 

   Örneğin, arka uç sertifikaları tanınmış bir CA tarafından verildiyse ve bir contoso.com CN 'si varsa ve arka uç http ayarının ana bilgisayar alanı da contoso.com olarak ayarlanmışsa ek bir adım gerekmez. Arka uç http ayar protokolünü HTTPS olarak ayarlayabilirsiniz ve hem durum araştırması hem de veri yolu TLS etkin olur. Arka ucunuz olarak Azure App Service veya diğer Azure Web hizmetlerini kullanıyorsanız, bunlar da örtülü olarak güvenilirdir ve uçtan uca TLS için başka bir adım gerekmez.
   
> [!NOTE] 
>
> Bir TLS/SSL sertifikasının güvenilir olması için, arka uç sunucusunun bu sertifikasının Application Gateway güvenilen deposunda bulunan bir CA tarafından verilmiş olması gerekir. sertifika, güvenilir bir CA tarafından verilmediği zaman, Application Gateway veren CA sertifikasının güvenilen bir CA tarafından verildiğini ve bu şekilde güvenilen bir CA 'nın bulunana (güvenilen, güvenli bağlantı kurulabileceği) veya güvenilir bir CA 'nın bulununcaya kadar (Bu noktada Application Gateway arka ucunu sağlıksız olarak işaretleyecek) göz atın. Bu nedenle, arka uç sunucu sertifikasının hem kök hem de ara CA 'Ları içermesi önerilir.

- Sertifika kendinden imzalıysa veya bilinmeyen aracılar tarafından imzalanmışsa, v2 SKU 'sunda uçtan uca TLS 'yi etkinleştirmek için güvenilen bir kök sertifika tanımlanmalıdır. Application Gateway, yalnızca sunucu sertifikasının kök sertifikası, havuzla ilişkili arka uç http ayarındaki güvenilen kök sertifikalar listesinden biriyle eşleşen arka uçları ile iletişim kurar.

> [!NOTE] 
>
> Otomatik olarak imzalanan sertifika, bir Sertifika zincirinin parçası olmalıdır. V2 SKU 'sunda, zinciri olmayan tek bir otomatik olarak imzalanan sertifika desteklenmez.

- Kök sertifika eşleştirmesinin yanı sıra Application Gateway, arka uç http ayarında belirtilen ana bilgisayar ayarının, arka uç sunucusunun TLS/SSL sertifikası tarafından sunulan ortak ad (CN) ile eşleşip eşleşmediğini da doğrular. Arka uca bir TLS bağlantısı kurmaya çalışırken Application Gateway, arka uç http ayarında belirtilen konağa Sunucu Adı Belirtme (SNı) uzantısını ayarlar.
- Arka uç http ayarında konak alanı yerine **arka uç adresinden seçim ana bilgisayar adı** seçilirse, SNI üstbilgisi her zaman arka uç havuzu FQDN 'sine ayarlanır ve arka uç sunucusu TLS/SSL sertifikasındaki CN 'nın FQDN 'siyle eşleşmesi gerekir. Bu senaryoda, IP ile arka uç havuzu üyeleri desteklenmez.
- Kök sertifika, arka uç sunucu sertifikalarından gelen Base64 kodlamalı bir kök sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

Uçtan uca TLS hakkında bilgi aldıktan sonra uçtan uca TLS kullanarak bir uygulama ağ geçidi oluşturmak için [PowerShell ile Application Gateway kullanarak uçtan uca TLS 'Yi yapılandırma](application-gateway-end-to-end-ssl-powershell.md) bölümüne gidin.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
