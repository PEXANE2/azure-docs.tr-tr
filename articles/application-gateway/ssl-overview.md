---
title: Azure Uygulama Ağ Geçidi'nde SSL'nin uçmasını etkinleştirme
description: Bu makale, SSL desteğini sona erdirmek için Uygulama Ağ Geçidi'nin sonuna genel bir bakıştır.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 3/19/2019
ms.author: victorh
ms.openlocfilehash: 9c4e6124acdbb35233f8e829f43d2665fd4a5176
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284815"
---
# <a name="overview-of-ssl-termination-and-end-to-end-ssl-with-application-gateway"></a>Uygulama Ağ Geçidi ile SSL sonlandırma ve bitiş sonu SSL genel bakış

Secure Sockets Layer (SSL), bir web sunucusu ve tarayıcı arasında şifreli bir bağlantı kurmak için standart güvenlik teknolojisidir. Bu bağlantı, web sunucusu ve tarayıcılar arasında geçirilen tüm verilerin gizli ve şifreli kalmasını sağlar. Uygulama ağ geçidi, ağ geçidinde hem SSL sonlandırmayı hem de sona erme SSL şifrelemesini destekler.

## <a name="ssl-termination"></a>SSL sonlandırma

Application Gateway, ağ geçidinde SSL sonlandırmasını destekler. Bu sonlandırmanın ardından, trafik genelde arka uç sunucularına şifrelenmemiş olarak akar. Uygulama ağ geçidinde SSL sonlandırma yapmanın birkaç avantajı vardır:

- **Geliştirilmiş performans** – SSL şifre çözme yaparken en büyük performans hit ilk el sıkışma olduğunu. Performansı artırmak için, şifre çözme işini yapan sunucu SSL oturum iliklerini önbelleğe alıyor ve TLS oturum biletlerini yönetir. Bu uygulama ağ geçidinde yapılırsa, aynı istemciden gelen tüm istekler önbelleğe alınan değerleri kullanabilir. Arka uç sunucularında yapılıyorsa, istemcinin istekleri her farklı sunucuya her gidişinde istemcinin yeniden kimlik doğrulaması gerekir. TLS biletlerinin kullanımı bu sorunu azaltmaya yardımcı olabilir, ancak tüm istemciler tarafından desteklenmez ve yapılandırmak ve yönetmek zor olabilir.
- **Arka uç sunucularının daha iyi kullanımı** – SSL/TLS işleme çok CPU yoğun, ve anahtar boyutları arttıkça daha yoğun hale geliyor. Bu çalışmayı arka uç sunucularından kaldırmak, içerik sunarak en verimli oldukları şeye odaklanmalarını sağlar.
- **Akıllı yönlendirme** – Uygulama ağ geçidi trafiğin şifresini çözerek üstbilgi, URI gibi istek içeriğine erişebilir ve istekleri yönlendirmek için bu verileri kullanabilir.
- **Sertifika yönetimi** – Sertifikaların yalnızca satın alınması ve uygulama ağ geçidine yüklenmesi gerekir ve tüm arka uç sunucularında değil. Bu hem zaman hem de para tasarrufu sağlar.

SSL sonlandırma yapılandırmak için, uygulama ağ geçidinin SSL protokol belirtimi uyarınca simetrik bir anahtar elde etmesini sağlamak için dinleyiciye bir SSL sertifikası eklenmesi gerekir. Simetrik anahtar daha sonra ağ geçidine gönderilen trafiği şifrelemek ve çözmek için kullanılır. SSL sertifikasının Kişisel Bilgi Alışverişi (PFX) formatında olması gerekir. Bu dosya biçimi, trafiğin şifreleme ve şifre çözme gerçekleştirmek için uygulama ağ geçidi tarafından gerekli olan özel anahtarı dışa aktarmanızı sağlar.

> [!NOTE] 
>
> Uygulama ağ geçidi, yeni bir sertifika oluşturmak veya sertifika yetkilisine sertifika isteği göndermek için herhangi bir yetenek sağlamaz.

SSL bağlantısının çalışması için, SSL sertifikasının aşağıdaki koşulları karşıladığından emin olmanız gerekir:

- Geçerli tarih ve saatin sertifikadaki "Geçerliden" ve "Geçerliye" tarih aralığında olması.
- Sertifikanın "Ortak Ad" (CN) değeri, istekteki ana bilgisayar üst bilgisiyle eşleşiyor. Örneğin istemci isteğinin hedefi `https://www.contoso.com/` ise CN `www.contoso.com` olmalıdır.

### <a name="certificates-supported-for-ssl-termination"></a>SSL sonlandırma için desteklenen sertifikalar

Uygulama ağ geçidi aşağıdaki sertifika türlerini destekler:

- CA (Sertifika Yetkilisi) sertifikası: CA sertifikası, sertifika yetkilisi (CA) tarafından verilen dijital sertifikadır
- EV (Genişletilmiş Doğrulama) sertifikası: EV sertifikası, endüstri standardı sertifika yönergelerine uygun bir sertifikadır. Bu tarayıcı bulucu çubuğu yeşil açmak ve şirket adını da yayımlamak.
- Joker Karakter Sertifikası: Bu sertifika, *.site.com'a dayalı olarak alt etki alanınızın *'in yerini alacağı herhangi bir alt etki alanını destekler. Ancak, site.com desteklemez, bu nedenle kullanıcıların önde gelen "www" yazmadan web sitenize erişmesi durumunda, joker karakter sertifikası bunu kapsamaz.
- Kendi İmzalı sertifikalar: İstemci tarayıcılar bu sertifikalara güvenmez ve kullanıcıyı sanal hizmetin sertifikasının güven zincirinin bir parçası olmadığı konusunda uyarır. Kendi imzalı sertifikalar, yöneticilerin istemcileri denetlediği ve tarayıcının güvenlik uyarılarını güvenle atlayabildiği sınama veya ortamlar için iyidir. Üretim iş yükleri hiçbir zaman kendi imzalı sertifikalar kullanmamalıdır.

Daha fazla bilgi için, [uygulama ağ geçidi ile SSL sonlandırma yapılandırma](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal)bakın.

### <a name="size-of-the-certificate"></a>Sertifikanın boyutu
Desteklenen maksimum SSL sertifika boyutunu bilmek için [Uygulama Ağ Geçidi sınırları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#application-gateway-limits) bölümünü kontrol edin.

## <a name="end-to-end-ssl-encryption"></a>Uçuca SSL şifreleme

Bazı müşteriler arka uç sunucularına şifrelenmemiş iletişim ihya etmek istemeyebilir. Bunun nedeni, güvenlik gereksinimleri, uyumluluk gereksinimleri veya uygulamanın yalnızca güvenli bağlantı kabul etmesi olabilir. Application Gateway, böyle uygulamalar için uçtan uca SSL şifrelemesini desteklemektedir.

Uçtan uca SSL, gizli verileri şifrelenmiş olarak arka uca aktarmanızı sağlar ve Application Gateway’in sunduğu Katman 7 yük dengeleme özelliklerinin avantajlarından yararlanmaya devam eder. Bu özelliklerin bazıları tanımlama bilgisi temelli oturum benzeşimi, URL tabanlı yönlendirme, sitelere göre yönlendirme desteği veya X-Forwarded-* üst bilgilerini ekleyebilmedir.

Application Gateway uçtan uca SSL iletişimi modu ile yapılandırıldığında, ağ geçidindeki SSL oturumlarını sonlandırır ve kullanıcı trafiğinin şifresini çözer. Ardından trafiğin yönlendirileceği uygun arka uç havuzunu seçmek için yapılandırılan kuralları uygular. Ardından Application Gateway, arka uç sunucusuyla yeni bir SSL bağlantısı başlatır ve isteği arka uca aktarmadan önce arka uç sunucusunun ortak anahtar sertifikasını kullanarak verileri yeniden şifreler. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler. Uçuca SSL, [backend http ayarı](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http-settings) ile HTTPS'ye protokol ayarı ayarlayarak etkinleştirilir ve bu da daha sonra bir arka uç havuzuna uygulanır.

SSL ilkesi hem ön yüz hem de arka uç trafiği için geçerlidir. Ön uçta, Application Gateway sunucu gibi davranır ve ilkeyi uygular. Arka uçta, Application Gateway istemci gibi davranır ve SSL el sıkışması sırasında tercih olarak protokol/şifre bilgilerini gönderir.

Uygulama ağ geçidi yalnızca, sertifikalarını uygulama ağ geçidiyle beyaz listeye alan veya sertifikaları CN sertifikasının HTTP'deki ana bilgisayar adı ile eşleştiği tanınmış CA yetkilileri tarafından imzalanmış olan arka uç örnekleriyle iletişim kurar. arka uç ayarları. Bunlar arasında Azure App hizmeti web uygulamaları ve Azure API Yönetimi gibi güvenilir Azure hizmetleri yer almaktadır.

Arka uç havuzundaki üyelerin sertifikaları iyi bilinen CA yetkilileri tarafından imzalanmıyorsa, arka uç havuzunda uçtan uca SSL etkinleştirilmiş her örnek, güvenli iletişimsağlamak için bir sertifika ile yapılandırılmalıdır. Sertifikanın eklenmesi, uygulama ağ geçidinin yalnızca bilinen arka uç örnekleriyle iletişim kurmasını sağlar. Bu, uçlardan uca iletişimi daha da güvence altına alar.

> [!NOTE] 
>
> Azure App hizmeti web uygulamaları ve Azure API Yönetimi gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekmez.

> [!NOTE] 
>
> Arka uç sunucularının kimliğini doğrulamak için **Backend HTTP Ayarı'na** eklenen sertifika, uygulama ağ geçidinde SSL sonlandırma için **dinleyiciye** eklenen sertifikayla veya gelişmiş güvenlik için farklı olabilir.

![uçtan uca SSL senaryosu][1]

Bu örnekte, TLS1.2 kullanan istekler, uçtan uca SSL kullanılarak Pool1'deki sunuculara yönlendirilir.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Uçtan uca SSL ve sertifikaların güvenilir listeye alınması

Uygulama ağ geçidi, yalnızca sertifikalarını uygulama ağ geçidiyle güvenilir listeye aldırmış, bilinen arka uç örnekleriyle iletişim kurar. Sertifikaların güvenilir listeye alınmasını etkinleştirmek için, arka uç sunucusu sertifikalarının ortak anahtarlarını uygulama ağ geçidine (kök sertifika değil) yüklemeniz gerekir. Bunun ardından, yalnızca bilinen ve güvenilir listeye alınmış arka uçlara yönelik bağlantılara izin verilir. Geriye kalan arka uçlar, ağ geçidi hatasına neden olur. Otomatik olarak imzalanan sertifikalar, yalnızca test amaçlarına yöneliktir ve üretim iş yükleri için önerilmez. Bu tür sertifikalar, kullanılmadan önce önceki adımlarda açıklandığı gibi uygulama ağ geçidi ile beyaz listeye alınmalıdır.

> [!NOTE]
> Azure Uygulama Hizmeti gibi güvenilir Azure hizmetleri için kimlik doğrulama sertifikası kurulumu gerekmez.

## <a name="end-to-end-ssl-with-the-v2-sku"></a>V2 SKU ile Sona SSL

Kimlik Doğrulama Sertifikaları amortismana uymuş ve Uygulama Ağ Geçidi v2 SKU'daki Güvenilir Kök Sertifikaları ile değiştirilmiştir. Birkaç önemli farkla kimlik doğrulama sertifikalarına benzer şekilde çalışırlar:

- CN'si HTTP arka uç ayarlarındaki ana bilgisayar adıile eşleşen tanınmış CA yetkilileri tarafından imzalanan sertifikalar, SSL'nin sona ermesi için ek bir adım gerektirmez. 

   Örneğin, arka uç sertifikaları iyi bilinen bir CA tarafından verilmişse ve cn contoso.com varsa ve arka uç http ayarının ana bilgisayar alanı da contoso.com olarak ayarlanmışsa, ek adım gerekmez. Arka uç http ayar protokolünü HTTPS olarak ayarlayabilirsiniz ve hem sistem durumu sondası hem de veri yolu SSL etkin olacaktır. Azure Uygulama Hizmeti'ni veya diğer Azure web hizmetlerini arka uç olarak kullanıyorsanız, bunlar asımdan da güvenilir ve SSL'nin sona ermesi için başka adım gerekmez.
   
> [!NOTE] 
>
> Bir SSL sertifikasının güvenilir olması için, arka uç sunucusunun sertifikasının Uygulama Ağ Geçidi'nin güvenilir deposunda bulunan bir CA tarafından verilmiş olması gerekir.Sertifika güvenilir bir CA tarafından düzenlenmemişse, Uygulama Ağ Geçidi sağlayan CA sertifikasının güvenilir bir CA tarafından verilip verilmediği ve güvenilir bir CA bulunana (bu noktada güvenilir, güvenli bir bağlantı kurulacak) veya güvenilir bir CA bulunup bulunmadığını görmek için (bu noktada Uygulama Ağ Geçidi arka ucu işaretleyecek sağlıksız). Bu nedenle, arka uç sunucu sertifikası hem kök hem de ara CAs içeren önerilir.

- Sertifika kendi kendine imzalanmışsa veya bilinmeyen aracılar tarafından imzalanmışsa, v2 SKU'da SSL'nin sona ermesini etkinleştirmek için güvenilir bir kök sertifikası tanımlanmalıdır. Uygulama Ağ Geçidi yalnızca, Sunucu sertifikasının kök sertifikası, arka uçtaki güvenilen kök sertifikaları listesindehavuzla ilişkili http ayarındaki biriyle eşleşen arka uçlarla iletişim kurar.

> [!NOTE] 
>
> İmzalanan sertifika, sertifika zincirinin bir parçası olmalıdır. V2 SKU'da zincirsiz tek bir kendi imzalı sertifika desteklenmez.

- Kök sertifika eşleşmesine ek olarak, Uygulama Ağ Geçidi, arka uç http ayarında belirtilen Ana Bilgisayar ayarı, arka uç sunucusunun SSL sertifikası tarafından sunulan ortak ad (CN) ile eşleşirse de doğrular. Arka uça bir SSL bağlantısı kurmaya çalışırken, Application Gateway sunucu adı göstergesi (SNI) uzantısını arka uç http ayarında belirtilen Ana Bilgisayar'a ayarlar.
- Arka uç http ayarında Ana Bilgisayar alanı yerine **ana bilgisayar adı** seçilirse, SNI üstbilgisi her zaman arka uç havuzu FQDN'ye ayarlanır ve arka uç sunucusu SSL sertifikasındaki CN, FQDN ile eşleşmelidir. IP'leri olan arka uç havuz üyeleri bu senaryoda desteklenmez.
- Kök sertifika, arka uç Sunucu sertifikalarından 64 kodlanmış bir kök sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

SSL'nin sonuna kadar öğrenmeyi öğrendikten sonra, [PowerShell ile Birlikte Uygulama Ağ Geçidi'ni kullanarak SSL](application-gateway-end-to-end-ssl-powershell.md) sonuna kadar yapılandırın.

<!--Image references-->

[1]: ./media/ssl-overview/scenario.png
