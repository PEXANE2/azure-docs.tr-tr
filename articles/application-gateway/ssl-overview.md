---
title: Azure Uygulama Ağ Geçidi'nde TLS'nin sonuna kadar etkinleştirme
description: Bu makale, TLS desteğini sona erdirmek için Uygulama Ağ Geçidi'nin sonuna genel bir bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: ae80b49c3bfb40743665768622d3f4a8a6990c12
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311859"
---
# <a name="overview-of-tls-termination-and-end-to-end-tls-with-application-gateway"></a>Uygulama Ağ Geçidi ile TLS sonlandırma ve bitiş tls genel bakış

Daha önce Güvenli Soket katmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS), bir web sunucusu ve tarayıcı arasında şifreli bir bağlantı kurmak için standart güvenlik teknolojisidir. Bu bağlantı, web sunucusu ve tarayıcılar arasında geçirilen tüm verilerin gizli ve şifreli kalmasını sağlar. Uygulama ağ geçidi, hem ağ geçidinde HEM TLS sonlandırmayı hem de UC'dan uca TLS şifrelemesini destekler.

## <a name="tls-termination"></a>TLS sonlandırma

Uygulama Ağ Geçidi, trafiğin genellikle arka uç sunucularına şifrelenmemiş olarak aktığı ağ geçidinde TLS sonlandırmayı destekler. Uygulama ağ geçidinde TLS sonlandırma yapmanın birçok avantajı vardır:

- **Geliştirilmiş performans** – TLS şifre çözme yaparken en büyük performans ilk el sıkışmadır. Performansı artırmak için, şifre çözme işini yapan sunucu TLS oturum ii'lerini önbelleğe alıyor ve TLS oturum biletlerini yönetir. Bu uygulama ağ geçidinde yapılırsa, aynı istemciden gelen tüm istekler önbelleğe alınan değerleri kullanabilir. Arka uç sunucularında yapılıyorsa, istemcinin istekleri her farklı sunucuya her gidişinde istemcinin yeniden kimlik doğrulaması gerekir. TLS biletlerinin kullanımı bu sorunu azaltmaya yardımcı olabilir, ancak tüm istemciler tarafından desteklenmez ve yapılandırmak ve yönetmek zor olabilir.
- **Arka uç sunucularının daha iyi kullanımı** – SSL/TLS işleme çok CPU yoğun, ve anahtar boyutları arttıkça daha yoğun hale geliyor. Bu çalışmayı arka uç sunucularından kaldırmak, içerik sunarak en verimli oldukları şeye odaklanmalarını sağlar.
- **Akıllı yönlendirme** – Uygulama ağ geçidi trafiğin şifresini çözerek üstbilgi, URI gibi istek içeriğine erişebilir ve istekleri yönlendirmek için bu verileri kullanabilir.
- **Sertifika yönetimi** – Sertifikaların yalnızca satın alınması ve uygulama ağ geçidine yüklenmesi gerekir ve tüm arka uç sunucularında değil. Bu hem zaman hem de para tasarrufu sağlar.

TLS sonlandırma yapılandırması için, uygulama ağ geçidinin TLS/SSL protokol belirtimine göre simetrik bir anahtar elde etmesini sağlamak için dinleyiciye bir TLS/SSL sertifikası eklenmesi gerekir. Simetrik anahtar daha sonra ağ geçidine gönderilen trafiği şifrelemek ve çözmek için kullanılır. TLS/SSL sertifikasının Kişisel Bilgi Alışverişi (PFX) formatında olması gerekir. Bu dosya biçimi, trafiğin şifreleme ve şifre çözme gerçekleştirmek için uygulama ağ geçidi tarafından gerekli olan özel anahtarı dışa aktarmanızı sağlar.

> [!NOTE] 
>
> Uygulama ağ geçidi, yeni bir sertifika oluşturmak veya sertifika yetkilisine sertifika isteği göndermek için herhangi bir yetenek sağlamaz.

TLS bağlantısının çalışması için TLS/SSL sertifikasının aşağıdaki koşulları karşıladığından emin olmanız gerekir:

- Geçerli tarih ve saatin sertifikadaki "Geçerliden" ve "Geçerliye" tarih aralığında olması.
- Sertifikanın "Ortak Ad" (CN) değeri, istekteki ana bilgisayar üst bilgisiyle eşleşiyor. Örneğin istemci isteğinin hedefi `https://www.contoso.com/` ise CN `www.contoso.com` olmalıdır.

### <a name="certificates-supported-for-tls-termination"></a>TLS sonlandırma için desteklenen sertifikalar

Uygulama ağ geçidi aşağıdaki sertifika türlerini destekler:

- CA (Sertifika Yetkilisi) sertifikası: CA sertifikası, sertifika yetkilisi (CA) tarafından verilen dijital sertifikadır
- EV (Genişletilmiş Doğrulama) sertifikası: EV sertifikası, endüstri standardı sertifika yönergelerine uygun bir sertifikadır. Bu tarayıcı bulucu çubuğu yeşil açmak ve şirket adını da yayımlamak.
- Joker Karakter Sertifikası: Bu sertifika, *.site.com'a dayalı olarak alt etki alanınızın *'in yerini alacağı herhangi bir alt etki alanını destekler. Ancak, site.com desteklemez, bu nedenle kullanıcıların önde gelen "www" yazmadan web sitenize erişmesi durumunda, joker karakter sertifikası bunu kapsamaz.
- Kendi İmzalı sertifikalar: İstemci tarayıcılar bu sertifikalara güvenmez ve kullanıcıyı sanal hizmetin sertifikasının güven zincirinin bir parçası olmadığı konusunda uyarır. Kendi imzalı sertifikalar, yöneticilerin istemcileri denetlediği ve tarayıcının güvenlik uyarılarını güvenle atlayabildiği sınama veya ortamlar için iyidir. Üretim iş yükleri hiçbir zaman kendi imzalı sertifikalar kullanmamalıdır.

Daha fazla bilgi için [tls sonlandırmayı uygulama ağ geçidiyle yapılandırmaya](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)bakın.

### <a name="size-of-the-certificate"></a>Sertifikanın boyutu
Desteklenen maksimum TLS/SSL sertifika boyutunu bilmek için [Uygulama Ağ Geçidi sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#application-gateway-limits) bölümünü kontrol edin.

## <a name="end-to-end-tls-encryption"></a>TLS şifrelemesi sona erecek

Bazı müşteriler arka uç sunucularına şifrelenmemiş iletişim ihya etmek istemeyebilir. Bunun nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Bu tür uygulamalar için uygulama ağ geçidi, TLS şifrelemesini sona erdirmeyi destekler.

Ucto end TLS, uygulama ağ geçidinin sağladığı Katman 7 yük dengeleme özelliklerinden yararlanırken hassas verileri şifreli arka uca güvenli bir şekilde iletmenizi sağlar. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

TlS iletişim modu uçarak yapılandırıldığında, uygulama ağ geçidi ağ geçidindeki TLS oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Uygulama ağ geçidi daha sonra arka uç sunucusuna yeni bir TLS bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler. Ucto end TLS, [backend http ayarı](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) ile HTTPS'ye protokol ayarı ayarlayarak etkinleştirilir ve bu da daha sonra bir arka uç havuzuna uygulanır.

TLS ilkesi hem ön uç hem de arka uç trafiği için geçerlidir. Ön uçta, Application Gateway sunucu gibi davranır ve ilkeyi uygular. Arka uçta, Application Gateway istemci gibi davranır ve TLS el sıkışması sırasında tercih olarak protokol/şifre bilgilerini gönderir.

Uygulama ağ geçidi yalnızca, sertifikalarını uygulama ağ geçidiyle beyaz listeye alan veya sertifikaları CN sertifikasının HTTP arka uç ayarlarında ana bilgisayar adı ile eşleştiği tanınmış CA yetkilileri tarafından imzalanan arka uç örnekleriyle iletişim kurar. Bunlar arasında Azure App hizmeti web uygulamaları ve Azure API Yönetimi gibi güvenilir Azure hizmetleri yer almaktadır.

Arka uç havuzundaki üyelerin sertifikaları iyi bilinen CA yetkilileri tarafından imzalanmıyorsa, arka uç havuzunda uçtan uca TLS etkinleştirilmiş her örnek, güvenli iletişim sağlamak için bir sertifika ile yapılandırılmalıdır. Sertifikanın eklenmesi, uygulama ağ geçidinin yalnızca bilinen arka uç örnekleriyle iletişim kurmasını sağlar. Bu, uçlardan uca iletişimi daha da güvence altına alar.

> [!NOTE] 
>
> Azure App hizmeti web uygulamaları ve Azure API Yönetimi gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekmez.

> [!NOTE] 
>
> Arka uç sunucularının kimliğini doğrulamak için **Backend HTTP Ayarı'na** eklenen sertifika, uygulama ağ geçidinde TLS sonlandırma için **dinleyiciye** eklenen sertifikayla aynı veya gelişmiş güvenlik için farklı olabilir.

![tls senaryosu nun sonuna kadar][1]

Bu örnekte, TLS1.2 kullanan istekler, TLS uçuç kullanılarak Pool1'deki arka uç sunucularına yönlendirilir.

## <a name="end-to-end-tls-and-whitelisting-of-certificates"></a>TLS ve sertifikaların beyaz listesi sona erecek

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bu tür sertifikalar, kullanılmadan önce önceki adımlarda açıklandığı gibi uygulama ağ geçidi ile beyaz listeye alınmalıdır.

> [!NOTE]
> Azure Uygulama Hizmeti gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekmez.

## <a name="end-to-end-tls-with-the-v2-sku"></a>v2 SKU ile TLS'yi sona erdirmek

Kimlik Doğrulama Sertifikaları amortismana uymuş ve Uygulama Ağ Geçidi v2 SKU'daki Güvenilir Kök Sertifikaları ile değiştirilmiştir. Birkaç önemli farkla kimlik doğrulama sertifikalarına benzer şekilde çalışırlar:

- CN'si HTTP arka uç ayarlarındaki ana bilgisayar adı ile eşleşen tanınmış CA yetkilileri tarafından imzalanmış sertifikalar, TLS'nin sona ermesi için ek bir adım gerektirmez. 

   Örneğin, arka uç sertifikaları iyi bilinen bir CA tarafından verilmişse ve cn contoso.com varsa ve arka uç http ayarının ana bilgisayar alanı da contoso.com olarak ayarlanmışsa, ek adım gerekmez. Arka uç http ayar protokolünü HTTPS olarak ayarlayabilirsiniz ve hem sistem durumu sondası hem de veri yolu TLS etkin olacaktır. Azure Uygulama Hizmeti'ni veya diğer Azure web hizmetlerini arka uç olarak kullanıyorsanız, bunlar atüre güvenle de güvenilir ve TLS'nin sona ermesi için başka adım gerekmez.
   
> [!NOTE] 
>
> BIR TLS/SSL sertifikasının güvenilir olması için, arka uç sunucusunun sertifikasının Application Gateway'in güvenilir deposunda bulunan bir CA tarafından verilmiş olması gerekir.Sertifika güvenilir bir CA tarafından verilmediyse, Uygulama Ağ Geçidi daha sonra, verilen CA sertifikasının güvenilir bir CA tarafından verilip verilmeyeceğini ve güvenilir bir CA bulunana (bu noktada güvenilir, güvenli bir bağlantı kurulacak) veya güvenilir bir CA bulunamayana (bu noktada Uygulama Ağ Geçidi'nin arka uç sağlıksız olarak işaretlenebileceği) olup olmadığını denetler. Bu nedenle, arka uç sunucu sertifikası hem kök hem de ara CAs içeren önerilir.

- Sertifika kendi kendine imzalanmışsa veya bilinmeyen aracılar tarafından imzalanmışsa, v2 SKU'da TLS'nin sona ermesini sağlamak için güvenilir bir kök sertifika tanımlanmalıdır. Uygulama Ağ Geçidi yalnızca, Sunucu sertifikasının kök sertifikası, arka uçtaki güvenilen kök sertifikaları listesindehavuzla ilişkili http ayarındaki biriyle eşleşen arka uçlarla iletişim kurar.

> [!NOTE] 
>
> İmzalanan sertifika, sertifika zincirinin bir parçası olmalıdır. V2 SKU'da zincirsiz tek bir kendi imzalı sertifika desteklenmez.

- Kök sertifika eşleşmesine ek olarak, Uygulama Ağ Geçidi, arka uç http ayarında belirtilen Ana Bilgisayar ayarı, arka uç sunucusunun TLS/SSL sertifikası tarafından sunulan ortak ad (CN) ile eşleşirse de doğrular. Arka uça TLS bağlantısı kurmaya çalışırken, Application Gateway sunucu adı göstergesi (SNI) uzantısını arka uç http ayarında belirtilen Ana Bilgisayar'a ayarlar.
- Arka uç http ayarında Ana Bilgisayar alanı yerine **ana bilgisayar adı** seçilirse, SNI üstbilgisi her zaman arka uç havuzuFQDN'ye ayarlanır ve arka uç sunucusu TLS/SSL sertifikasındaki CN, FQDN ile eşleşmelidir. IP'leri olan arka uç havuz üyeleri bu senaryoda desteklenmez.
- Kök sertifika, arka uç Sunucu sertifikalarından 64 kodlanmış bir kök sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

TLS'nin sonuna kadar olduğunu öğrendikten sonra, [PowerShell ile Uygulama Ağ Geçidi'ni kullanarak TLS'yi](application-gateway-end-to-end-ssl-powershell.md) sonuna kadar yapılandırın.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
