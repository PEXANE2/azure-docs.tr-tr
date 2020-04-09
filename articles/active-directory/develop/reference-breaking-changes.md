---
title: Azure Etkin Dizin kırma değişiklikleri başvurusu
description: Azure REKLAM protokollerinde uygulamanızı etkileyebilecek değişiklikler hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 3/13/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 52ede7d66bd657b5002272e34673b4b01c9ab1aa
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883466"
---
# <a name="whats-new-for-authentication"></a>Kimlik doğrulama için yenilikler nelerdir? 

>Bu sayfadaki güncellemeler hakkında bilgilendirilin. Bu [URL'yi](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) RSS özet akışı okuyucunuza eklemeniz gerekiyor.

Kimlik doğrulama sistemi, güvenliği ve standartlara uyumluluğu geliştirmek için sürekli olarak özellikleri değiştirir ve ekler. En son gelişmelerden haberdar olmak için bu makalede, aşağıdaki ayrıntılar hakkında bilgi verilmektedir:

- En yeni özellikler
- Bilinen sorunlar
- Protokol değişiklikleri
- Kullanım dışı işlevsellik

> [!TIP] 
> Bu sayfa düzenli olarak güncellenir, bu nedenle sık sık ziyaret edin. Aksi belirtilmedikçe, bu değişiklikler yalnızca yeni kaydedilmiş uygulamalar için yerine konur.  

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler

Şu anda planlanmış bir şey yok.  Üretime giren veya üretime gelen değişiklikler için lütfen aşağıya bakın. 

## <a name="march-2020"></a>Mart 2020 

### <a name="user-passwords-will-be-restricted-to-256-characters"></a>Kullanıcı parolaları 256 karakterle sınırlandırılacaktır.

**Yürürlük tarihi**: 13 Mart 2020

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen Protokol**: Tüm kullanıcı akışları. 

Parolaları doğrudan Azure AD'de oturum açan 256 karakterden daha uzun olan kullanıcılar (ADFS gibi federe bir IDP'nin aksine) 13 Mart 2020'den itibaren oturum açamaz ve bunun yerine parolalarını sıfırlamaları istenebilir.  Yöneticiler, kullanıcıların parolasını sıfırlamaya yardımcı olmak için istekler alabilir. 

Oturum daki hata AADSTS 50052 olacaktır: GeçersizPasswordExceedsMaxLength

İleti:`The password entered exceeds the maximum length of 256. Please reach out to your admin to reset the password.`

Düzeltme:

Parolaları izin verilen maksimum uzunluğu aştığı için kullanıcı oturum açamıyor. Parolayı sıfırlamak için yöneticilerine başvurmaları gerekir. Kiracıları için SSPR etkinse, "Parolanızı unuttum" bağlantısını izleyerek parolalarını sıfırlayabilirler.



## <a name="february-2020"></a>Şubat 2020 

### <a name="empty-fragments-will-be-appended-to-every-http-redirect-from-the-login-endpoint"></a>Boş parçalar, giriş bitiş noktasından her HTTP yönlendirmesine eklenir. 

**Yürürlük tarihi**: 8 Şubat 2020

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen protokol**: response_type=sorgu kullanan OAuth ve OIDC akışları - bu bazı durumlarda [yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md) ve [örtük akışı](v2-oauth2-implicit-grant-flow.md)kapsar. 

Kimlik doğrulama yanıtı login.microsoftonline.com'dan bir uygulamaya HTTP yönlendirme sn üzerinden gönderildiğinde, hizmet yanıt URL'sine boş bir parça ekler.  Bu, tarayıcının kimlik doğrulama isteğindeki varolan herhangi bir parçayı silmesini sağlayarak bir yeniden yönlendirme saldırısı sınıfının önüne geçimesini sağlar.  Hiçbir uygulamanın bu davranışa bağımlılığı olmamalıdır. 


## <a name="august-2019"></a>Ağustos 2019

### <a name="post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored"></a>POST formu semantik daha sıkı uygulanacaktır - boşluklar ve tırnak göz ardı edilecektir

**Yürürlük tarihi**: 2 Eylül 2019

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen Protokol**: Anywhere POST kullanılır ([istemci kimlik bilgileri](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow), yetkilendirme kodu [kullanımı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow), [ROPC](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth-ropc), [OBO](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow), ve [yenileme belirteci itfa](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow#refresh-the-access-token))

9/2 haftasından itibaren, POST yöntemini kullanan kimlik doğrulama istekleri daha katı HTTP standartları kullanılarak doğrulanır.  Özellikle, boşluklar ve çift tırnak (") artık istek formu değerlerinden kaldırılmaz. Bu değişikliklerin varolan istemcileri kırması beklenmez ve Azure AD'ye gönderilen isteklerin her seferinde güvenilir bir şekilde işlenmesini sağlar. Gelecekte (yukarıya bakınız) yinelenen parametreleri ayrıca reddetmeyi ve istekler içindeki BOM'u yok saymayı planlıyoruz. 

Örnek:

Bugün, `?e=    "f"&g=h` `?e=f&g=h` aynı şekilde ayrıştırılır `e`- bu yüzden  ==  `f`.  Bu değişiklikle, şimdi ayrıştırılmış `e`  ==  `    "f"` olacak, böylece - bu geçerli bir argüman olması olası değildir ve istek şimdi başarısız olur. 


## <a name="july-2019"></a>Temmuz 2019

### <a name="app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant"></a>Tek kiracılı uygulamalar için yalnızca uygulama belirteçleri yalnızca istemci uygulaması kaynak kiracıda varsa verilir

**Yürürlük tarihi**: 26 Temmuz 2019

**Etkilenen uç noktalar**: Hem [v1.0](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) hem de [v2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)

**Etkilenen protokol**: [İstemci Kimlik Bilgileri (yalnızca uygulama belirteçleri)](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)

Yalnızca uygulama belirteçlerinin (istemci kimlik bilgileri hibesi yoluyla) düzenlenme biçimini değiştiren bir güvenlik değişikliği 26 Temmuz'da canlı olarak gerçekleşti. Daha önce, uygulamaların, kiracıda veya bu uygulama için onay verilen rollerde varlığından bağımsız olarak, başka bir uygulamayı aramak için belirteçler almasına izin verilirdi.  Bu davranış, kaynaklar (bazen Web API'leri olarak da adlandırılır) için tek kiracı (varsayılan) olarak ayarlanmış için istemci uygulamasının kaynak kiracıiçinde bulunması için güncelleştirildi.  İstemci ve API arasında varolan onam hala gerekli olmadığını ve uygulamaların bir `roles` talebin mevcut olduğundan ve API için beklenen değeri içerdiğinden emin olmak için hala kendi yetki denetimlerini yapıyor olması gerektiğini unutmayın.

Bu senaryo için hata iletisi şu anda devletler: 

`The service principal named <appName> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`

Bu sorunu gidermek için, kiracınızda istemci uygulama hizmeti ilkesini oluşturmak için Yönetici Onayı deneyimini kullanın veya el ile oluşturun.  Bu gereksinim, kiracının kiracı içinde faaliyet göstermek için başvuru izni vermiş olmasını sağlar.  

#### <a name="example-request"></a>Örnek istek

`https://login.microsoftonline.com/contoso.com/oauth2/authorize?resource=https://gateway.contoso.com/api&response_type=token&client_id=14c88eee-b3e2-4bb0-9233-f5e3053b3a28&...`Bu örnekte, kaynak kiracı (yetkisini) contoso.com, kaynak uygulaması Contoso kiracı için çağrılan `gateway.contoso.com/api` tek kiracılı bir uygulamadır `14c88eee-b3e2-4bb0-9233-f5e3053b3a28`ve istemci uygulaması .  İstemci uygulamasının Contoso.com içinde bir hizmet ilkesi varsa, bu istek devam edebilir.  Ancak, değilse, istek yukarıdaki hata ile başarısız olur.  

Ancak, Contoso ağ geçidi uygulaması çok kiracılı bir uygulama ysa, istemci uygulamasının Contoso.com içinde bir hizmet ilkesine sahip olmasına bakılmaksızın istek devam eder.  

### <a name="redirect-uris-can-now-contain-query-string-parameters"></a>Yönlendirme IU'ları artık sorgu dize parametreleri içerebilir

**Yürürlük tarihi**: 22 Temmuz 2019

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen Protokol**: Tüm akışlar

[RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2)başına, Azure AD uygulamaları artık OAuth 2.0 istekleri için `https://contoso.com/oauth2?idp=microsoft`statik sorgu parametreleri (örneğin) ile yeniden yönlendirme (yanıt) URI'leri kaydedebilir ve kullanabilir.  Dinamik yeniden yönlendirme UR'leri bir güvenlik riski temsil ettikleri için hala yasaktır ve bu durum bilgilerini `state` kimlik doğrulama isteği nde tutmak için kullanılamaz - bunun için parametreyi kullanın.

Statik sorgu parametresi, uri yönlendirmenin diğer bir bölümü gibi URI'leri yeniden yönlendirmek için dize eşleştirmesine tabidir - URI tarafından çözülmüş redirect_uri eşleşen bir dize kayıtlı değilse, istek reddedilir.  URI uygulama kaydında bulunursa, statik sorgu parametresi de dahil olmak üzere kullanıcıyı yönlendirmek için tüm dize kullanılır. 

Şu anda (Temmuz 2019 sonu) Azure portalındaki uygulama kaydı UX'nin sorgu parametrelerini hala engellediğini unutmayın.  Ancak, sorgu parametreleri eklemek ve bunu uygulamanızda test etmek için uygulama bildirimini el ile dinleyebilirsiniz.  


## <a name="march-2019"></a>Mart 2019

### <a name="looping-clients-will-be-interrupted"></a>Döngü istemcileri kesintiye uğrar

**Yürürlük tarihi**: 25 Mart 2019

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen Protokol**: Tüm akışlar

İstemci uygulamaları bazen yaramazlık yapabilir ve kısa bir süre içinde aynı oturum açma isteğinden yüzlercesini verebilir.  Bu istekler başarılı olabilir veya olmayabilir, ancak bunların tümü, idp için zayıf kullanıcı deneyimine ve artan iş yüklerine katkıda bulunur, tüm kullanıcılar için gecikme süresini artırır ve IDP'nin kullanılabilirliğini azaltır.  Bu uygulamalar normal kullanım sınırları dışında çalışıyor ve doğru şekilde hareket etmek için güncelleştirilmelidir.  

Yinelenen istekleri birden çok kez veren `invalid_grant` istemcilere bir hata gönderilir: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request`. 

Çoğu istemcinin bu hatayı önlemek için davranışını değiştirmesi gerekmez.  Yalnızca yanlış yapılandırılmış istemciler (belirteç önbelleğe almamış olanlar veya zaten istemi döngüleri sergileyenler) bu hatadan etkilenir.  İstemciler, aşağıdaki etkenler üzerinde yerel olarak (çerez yoluyla) her örnek bazında izlenir:

* Varsa kullanıcı ipucu

* İstenilen kapsamlar veya kaynak

* İstemci Kimliği

* Yeniden Yönlendirme URI'si

* Yanıt türü ve modu

Kısa bir süre içinde (5 dakika) birden fazla istekte bulunan `invalid_grant` uygulamalara (5 dakika) döngü yaptıklarını açıklayan bir hata alırsınız.  İstenilen belirteçlerin yeterince uzun ömürlü kullanım ömrü (varsayılan olarak 10 dakika, varsayılan olarak 60 dakika) vardır, bu nedenle bu süre içinde tekrarlanan istekler gereksizdir.  

Tüm uygulamalar, `invalid_grant` sessizce bir belirteç istemek yerine etkileşimli bir komut istemi göstererek ele almalıdır.  Bu hatayı önlemek için, istemciler aldıkları belirteçleri doğru şekilde önbelleğe aldıklarından emin olmalıdır.


## <a name="october-2018"></a>Ekim 2018

### <a name="authorization-codes-can-no-longer-be-reused"></a>Yetkilendirme kodları artık yeniden kullanılamaz

**Yürürlük tarihi**: 15 Kasım 2018

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen protokol**: [Kod akışı](v2-oauth2-auth-code-flow.md)

Azure AD, 15 Kasım 2018'den itibaren uygulamalar için daha önce kullanılmış kimlik doğrulama kodlarını kabul etmeyi durdurur. Bu güvenlik değişikliği, Azure AD'nin OAuth belirtimine uygun hale getirilmesine yardımcı olur ve hem v1 hem de v2 uç noktalarında uygulanır.

Uygulamanız birden çok kaynak için belirteçler almak için yetkilendirme kodlarını yeniden kullanıyorsa, yenileme jetonu almak için kodu kullanmanızı ve ardından diğer kaynaklar için ek belirteçler elde etmek için bu yenileme belirteci'ni kullanmanızı öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden çok kaynak ta birden çok kez kullanılabilir. OAuth kod akışı sırasında kimlik doğrulama kodunu yeniden kullanmaya çalışan herhangi bir yeni uygulama invalid_grant bir hata alır.

Yenileme belirteçleri hakkında daha fazla bilgi için erişim [belirteçlerini yenileme ye](v2-oauth2-auth-code-flow.md#refresh-the-access-token)bakın.  ADAL veya MSAL kullanıyorsanız, bu kitaplık tarafından sizin için ele alınır - 'AcquireTokenByAuthorizationCodeAsync' ikinci örneğini 'AcquireTokenSilentAsync' ile değiştirin. 

## <a name="may-2018"></a>Mayıs 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>OBO akışı için kimlik belirteçleri kullanılamaz

**Tarih**: 1 Mayıs 2018

**Etkilenen uç noktalar**: Hem v1.0 hem de v2.0

**Etkilenen protokoller**: Örtük akış ve [akış adına](v2-oauth2-on-behalf-of-flow.md)

1 Mayıs 2018 tarihinden sonra, id_tokens yeni uygulamalar için OBO akışında iddia olarak kullanılamaz. Erişim belirteçleri, aynı uygulamanın istemcisi ve orta katmanı arasında bile API'leri güvenli hale getirmek için kullanılmalıdır. 1 Mayıs 2018'den önce kayıtlı uygulamalar çalışmaya devam edecek ve erişim jetonu yla id_tokens değiştirebilecek; ancak, bu desen en iyi uygulama olarak kabul edilmez.

Bu değişikliği aşmak için aşağıdakileri yapabilirsiniz:

1. Uygulamanız için bir veya daha fazla kapsam içeren bir Web API'sı oluşturun. Bu açık giriş noktası, daha ince taneli denetim ve güvenlik sağlar.
1. Uygulamanızın manifestosunda, [Azure portalında](https://portal.azure.com) veya [uygulama kayıt portalında,](https://apps.dev.microsoft.com)uygulamanın örtülü akış üzerinden erişim belirteçleri yayınlamasına izin verildiğinden emin olun. Bu `oauth2AllowImplicitFlow` anahtar üzerinden kontrol edilir.
1. İstemci başvurunuz bir `response_type=id_token`id_token istediğinde, yukarıda`response_type=token`oluşturulan Web API'sı için bir erişim jetonu () isteğinde de bulunun. Böylece, v2.0 uç noktası `scope` kullanırken parametre `api://GUID/SCOPE`benzer görünmelidir . v1.0 bitiş noktasında, `resource` parametre web API uygulaması URI olmalıdır.
1. Bu erişim belirteciid_token yerine orta katmana geçirin.  
