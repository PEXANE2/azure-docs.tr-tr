---
title: Kimlik doğrulama ve yetkilendirme
description: Azure Uygulama Hizmeti ve Azure İşlevleri'nde yerleşik kimlik doğrulama ve yetkilendirme desteği ve uygulamanızın yetkisiz erişime karşı korunmasına nasıl yardımcı olabileceği hakkında bilgi edinin.
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.topic: article
ms.date: 08/12/2019
ms.reviewer: mahender
ms.custom: fasttrack-edit
ms.openlocfilehash: f16b10f13c945dd7f1ae4fdc3f4e02dcd7c5a018
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437953"
---
# <a name="authentication-and-authorization-in-azure-app-service-and-azure-functions"></a>Azure Uygulama Hizmeti ve Azure İşlerinde kimlik doğrulama ve yetkilendirme

> [!NOTE]
> Şu anda Azure [Active Directory v2.0](../active-directory/develop/v2-overview.md) [(MSAL](../active-directory/develop/msal-overview.md)dahil) Azure Uygulama Hizmeti ve Azure İşlevleri için desteklenmez. Güncellemeler için lütfen tekrar kontrol edin.
>

Azure Uygulama Hizmeti yerleşik kimlik doğrulama ve yetkilendirme desteği sağlar, böylece web uygulamanızda, RESTful API'nize ve mobil arka uça ve ayrıca [Azure İşlerinden](../azure-functions/functions-overview.md)en az kod yazarak kullanıcılarda oturum açabilir ve verilere erişebilirsiniz. Bu makalede, App Service'in uygulamanızın kimlik doğrulama ve yetkilendirmeyi basitleştirmeye nasıl yardımcı olduğu açıklanmaktadır.

Güvenli kimlik doğrulama ve yetkilendirme, federasyon, şifreleme, [JSON web belirteçleri (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) yönetimi, [hibe türleri](https://oauth.net/2/grant-types/)vb. dahil olmak üzere derin güvenlik anlayışı gerektirir. App Service, müşterinize iş değeri sağlamak için daha fazla zaman ve enerji harcayabilmeniz için bu yardımcı programları sağlar.

> [!IMPORTANT]
> Bu özelliği kimlik doğrulama ve yetkilendirme için kullanmanız gerekmez. Seçtiğiniz web çerçevenizde birlikte verilen güvenlik özelliklerini kullanabilir veya kendi yardımcı programlarınızı yazabilirsiniz. Ancak, [Chrome 80'in çerezler için SameSite'nin uygulanmasında (yayın](https://www.chromestatus.com/feature/5088147346030592) tarihi Mart 2020 civarında) ve istemci Chrome tarayıcıları güncelleştirildiğinde siteler arası çerez gönderime dayanan özel uzaktan kimlik doğrulama veya diğer senaryolarda çığır açan değişiklikler yaptığını unutmayın. Geçici çözüm, farklı tarayıcılar için farklı SameSite davranışlarını desteklemesi gerektiğinden karmaşıktır. 
>
> ASP.NET Core 2.1 ve üstü sürümler App Service tarafından barındırılan zaten bu kırılma değişikliği için yamalı ve Chrome 80 ve eski tarayıcıları uygun şekilde işledi. Buna ek olarak, ASP.NET Framework 4.7.2 için aynı yama Ocak 2020 boyunca App Service örneklerinde dağıtılmaktadır. Uygulamanızın yamayı edinip almadığını nasıl öğrenebildiğiniz hakkında daha fazla bilgi için [Azure App Service SameSite çerez güncelleştirmesine](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)bakın.
>

Yerel mobil uygulamalara özgü bilgiler için Azure [Uygulama Hizmeti ile mobil uygulamalar için Kullanıcı kimlik doğrulaması ve yetkilendirme](../app-service-mobile/app-service-mobile-auth.md)konusuna bakın.

## <a name="how-it-works"></a>Nasıl çalışır?

Kimlik doğrulama ve yetkilendirme modülü, uygulama kodunuzla aynı kum kutusunda çalışır. Etkinleştirildiğinde, gelen her HTTP isteği, uygulama kodunuz tarafından ele alınmadan önce bu istekten geçer.

![](media/app-service-authentication-overview/architecture.png)

Bu modül, uygulamanız için çeşitli şeyleri işler:

- Belirtilen sağlayıcıile kullanıcıları doğrular
- Belirteçleri doğrular, depolar ve yeniler
- Kimlik doğrulaması oturumu yönetir
- İstek üstbilgilerine kimlik bilgileri enjekte eder

Modül uygulama kodunuzdan ayrı olarak çalışır ve uygulama ayarları kullanılarak yapılandırılır. Hiçbir SDK, belirli diller veya uygulama kodunuzda değişiklik gereklidir. 

### <a name="userapplication-claims"></a>Kullanıcı/Uygulama talepleri

Tüm dil çerçeveleri için, App Service, istekleriniz üstbilgilerine enjekte ederek kodunuzda bulunan gelen belirteçteki talepleri (kimlik doğrulaması son kullanıcıdan veya istemci uygulamasından olsun) yapar. 4.6 ASP.NET uygulamalar için, Uygulama Hizmeti, kimlik doğrulaması yapılan kullanıcının talepleriyle [Birlikte ClaimsPrincipal.Current'ı](/dotnet/api/system.security.claims.claimsprincipal.current) doldurur, böylece öznitelik de dahil olmak üzere `[Authorize]` standart .NET kod deseni izleyebilirsiniz. Benzer şekilde, PHP uygulamaları için Uygulama `_SERVER['REMOTE_USER']` Hizmeti değişkeni doldurur. Java uygulamaları için, [iddialara Tomcat servletinden erişilebilir.](containers/configure-language-java.md#authenticate-users-easy-auth)

[Azure İşlevler](../azure-functions/functions-overview.md) `ClaimsPrincipal.Current` için ,.NET kodu için doldurulmaz, ancak yine de kullanıcı taleplerini `ClaimsPrincipal` istek üstbilgilerinde bulabilir veya nesneyi istek bağlamından veya hatta bağlayıcı bir parametre den alabilirsiniz. Daha fazla bilgi için [istemci kimlikleriyle çalışma](../azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities) hakkında bilgi edin.

Daha fazla bilgi için Bkz. [Access kullanıcı talepleri.](app-service-authentication-how-to.md#access-user-claims)

### <a name="token-store"></a>Belirteç deposu

App Service, web uygulamalarınızın, API'lerinizin veya yerel mobil uygulamalarınızın kullanıcılarıyla ilişkili belirteçlerin deposu olan yerleşik bir belirteç deposu sağlar. Herhangi bir sağlayıcıyla kimlik doğrulamasını etkinleştirdiğinizde, bu belirteç deposu hemen uygulamanız tarafından kullanılabilir. Uygulama kodunuz kullanıcı adına bu sağlayıcılardan gelen verilere erişmesi gerekiyorsa, aşağıdakiler gibi: 

- doğrulanmış kullanıcının Facebook zaman çizelgesine gönderme
- Microsoft Graph API'sini kullanarak kullanıcının kurumsal verilerini okuyun

Genellikle uygulamanızda bu belirteçleri toplamak, depolamak ve yenilemek için kod yazmanız gerekir. Belirteç deposunda, [jetonları](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) ihtiyacınız olduğunda alır ve uygulama hizmetine geçersiz olduklarında [yenilemesini söylersiniz.](app-service-authentication-how-to.md#refresh-identity-provider-tokens) 

Kimlik belirteçleri, erişim belirteçleri ve yenileme belirteçleri kimlik doğrulaması oturumu için önbelleğe alınır ve bunlara yalnızca ilişkili kullanıcı tarafından erişilebilir.  

Uygulamanızdaki belirteçlerle çalışmanız gerekmiyorsa, belirteç mağazasını devre dışı kullanabilirsiniz.

### <a name="logging-and-tracing"></a>Günlüğe kaydetme ve izleme

Uygulama [günlüğe kaydetmeyi](troubleshoot-diagnostic-logs.md)etkinleştiriseniz, kimlik doğrulama ve yetkilendirme izlemelerini doğrudan günlük dosyalarınızda görürsünüz. Beklemediğiniz bir kimlik doğrulama hatası görürseniz, varolan uygulama günlüklerinize bakarak tüm ayrıntıları rahatlıkla bulabilirsiniz. [Başarısız istek izlemeyi](troubleshoot-diagnostic-logs.md)etkinleştiriyorsanız, kimlik doğrulama ve yetkilendirme modülünün başarısız bir istekte tam olarak hangi rolü oynamış olabileceğini görebilirsiniz. İzleme günlüklerinde, '. adlı `EasyAuthModule_32/64`bir modüle başvurular arayın 

## <a name="identity-providers"></a>Kimlik sağlayıcıları

App Service, bir üçüncü taraf kimlik sağlayıcısının kullanıcı kimliklerini ve kimlik doğrulama akışını sizin için yönettiği [federe kimlik](https://en.wikipedia.org/wiki/Federated_identity)kullanır. Varsayılan olarak beş kimlik sağlayıcısı kullanılabilir: 

| Sağlayıcı | Oturum açma bitiş noktası |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft Hesabı](../active-directory/develop/v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/identity/choose-auth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |

Bu sağlayıcılardan biriyle kimlik doğrulama ve yetkilendirmeyi etkinleştirdiğinizde, oturum açma bitiş noktası kullanıcı kimlik doğrulaması ve sağlayıcıdan kimlik doğrulama belirteçlerinin doğrulanması için kullanılabilir. Kullanıcılarınıza bu oturum açma seçeneklerinden herhangi birini kolaylıkla sağlayabilirsiniz. Ayrıca başka bir kimlik sağlayıcısını veya [kendi özel kimlik çözümünüzü][custom-auth]entegre edebilirsiniz.

## <a name="authentication-flow"></a>Kimlik doğrulaması akışı

Kimlik doğrulama akışı tüm sağlayıcılar için aynıdır, ancak sağlayıcının SDK'sıyla oturum açıp açmadığınıza bağlı olarak farklılık gösterir:

- Sağlayıcı SDK olmadan: Uygulama, Uygulama Hizmeti'ne federe oturum açtıracak. Bu genellikle sağlayıcının giriş sayfasını kullanıcıya sunabilecek tarayıcı uygulamalarında geçerlidir. Sunucu kodu oturum açma işlemini yönetir, bu nedenle _sunucu yönelimli akış_ veya _sunucu akışı_olarak da adlandırılır. Bu durum tarayıcı uygulamaları için geçerlidir. SDK, Kullanıcıları App Service kimlik doğrulaması ile oturum alabilmek için bir web görünümü açtığından, Mobil Uygulamalar istemcisi SDK'yı kullanarak kullanıcıları imzalayan yerel uygulamalar için de geçerlidir. 
- Sağlayıcı SDK ile: Uygulama kullanıcıları sağlayıcıya el ile imzalar ve doğrulama için Uygulama Hizmeti'ne kimlik doğrulama belirteci gönderir. Bu genellikle, sağlayıcının oturum açma sayfasını kullanıcıya sunamayan tarayıcısız uygulamalarda durum böyledir. Uygulama kodu oturum açma işlemini yönetir, bu nedenle _istemci yönelimli akış_ veya _istemci akışı_olarak da adlandırılır. Bu durum, REST API'leri, [Azure İşlevleri](../azure-functions/functions-overview.md)ve JavaScript tarayıcı istemcilerinin yanı sıra oturum açma işleminde daha fazla esnekliğe ihtiyaç duyan tarayıcı uygulamaları için de geçerlidir. Ayrıca, sağlayıcının SDK'sını kullanarak kullanıcıları imzalayan yerel mobil uygulamalar için de geçerlidir.

> [!NOTE]
> Uygulama Hizmeti'ndeki güvenilir bir tarayıcı uygulamasından App Hizmeti'ndeki veya [Azure İşlerinden](../azure-functions/functions-overview.md) başka bir REST API'ye yapılan aramalar, sunucu nun yönettiği akış kullanılarak doğrulanabilir. Daha fazla bilgi için [Uygulama Hizmeti'nde kimlik doğrulamave yetkilendirmeyi özelleştir'e](app-service-authentication-how-to.md)bakın.
>

Aşağıdaki tabloda kimlik doğrulama akışının adımları gösterilmektedir.

| Adım | Sağlayıcı SDK olmadan | Sağlayıcı SDK ile |
| - | - | - |
| 1. Oturum aç | İstemciyi ' `/.auth/login/<provider>`e yönlendirir. | İstemci kodu kullanıcıyı doğrudan sağlayıcının SDK'sıyla işaretler ve bir kimlik doğrulama belirteci alır. Bilgi için sağlayıcının belgelerine bakın. |
| 2. Kimlik doğrulama sonrası | Sağlayıcı istemciyi ' `/.auth/login/<provider>/callback`ye yönlendirir. | İstemci kodu, sağlayıcıdan doğrulama `/.auth/login/<provider>` için [belirteç gönderir.](app-service-authentication-how-to.md#validate-tokens-from-providers) |
| 3. Kimlik doğrulaması oturumu oluşturma | Uygulama Hizmeti yanıta kimlik doğrulama çerezi ekler. | App Service, kendi kimlik doğrulama belirteciyle istemci koduna döndürür. |
| 4. Kimlik doğrulaması içerik sunma | İstemci sonraki isteklerde kimlik doğrulama çerezi içerir (otomatik olarak tarayıcı tarafından işlenir). | İstemci kodu üstbilgide `X-ZUMO-AUTH` kimlik doğrulama belirteci sunar (otomatik olarak Mobile Apps istemci SDK'leri tarafından işlenir). |

İstemci tarayıcıları için, App Service otomatik olarak `/.auth/login/<provider>`tüm kimliği doğrulanmamış kullanıcıları . Ayrıca, kullanıcılara seçtikleri sağlayıcıyı kullanarak uygulamanızda oturum açmaları için bir veya daha fazla `/.auth/login/<provider>` bağlantı da sunabilirsiniz.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Yetkilendirme davranışı

Azure [portalında,](https://portal.azure.com)gelen istek doğrulanmadığında Uygulama Hizmeti yetkilendirmesini bir dizi davranışla yapılandırabilirsiniz.

![](media/app-service-authentication-overview/authorization-flow.png)

Aşağıdaki başlıklar seçenekleri açıklar.

### <a name="allow-anonymous-requests-no-action"></a>Anonim isteklere izin ver (işlem yok)

Bu seçenek, uygulama kodunuza kimlik doğrulamamış trafiğin onayını erteler. Kimlik doğrulaması yapılan istekler için, App Service http üstbilgilerikimlik bilgileri boyunca da geçer. 

Bu seçenek, anonim istekleri işleme de daha fazla esneklik sağlar. Örneğin, kullanıcılarınıza [birden çok oturum açma sağlayıcısı sunmanıza](app-service-authentication-how-to.md#use-multiple-sign-in-providers) olanak tanır. Ancak, kod yazmanız gerekir. 

### <a name="allow-only-authenticated-requests"></a>Yalnızca kimlik doğrulaması isteklerine izin verme

Bu seçenek sağlayıcı **>\<oturum açmaktır. ** Uygulama Hizmeti, seçtiğiniz sağlayıcı `/.auth/login/<provider>` için tüm anonim istekleri yeniden yönlendirir. Anonim istek yerel bir mobil uygulamadan geliyorsa, döndürülen yanıt bir `HTTP 401 Unauthorized`.

Bu seçenekle, uygulamanıza herhangi bir kimlik doğrulama kodu yazmanız gerekmez. Role özel yetkilendirme gibi daha ince yetkilendirme, kullanıcının taleplerini inceleyerek işlenebilir (bkz. [Access kullanıcı talepleri).](app-service-authentication-how-to.md#access-user-claims)

> [!CAUTION]
> Bu şekilde erişimi kısıtlamak, uygulamanıza yapılan tüm aramalar için geçerlidir ve bu da birçok tek sayfalı uygulamada olduğu gibi herkese açık bir ana sayfa isteyen uygulamalar için isden edilmeyebilir.

## <a name="more-resources"></a>Diğer kaynaklar

[Öğretici: Azure Uygulama Hizmeti'nde (Windows) kullanıcıların sonuna kadar kimlik doğrulaması ve yetkilendirmesi](app-service-web-tutorial-auth-aad.md)  
[Öğretici: Linux için Azure Uygulama Hizmeti'nde kullanıcıların sonuna kadar kimlik doğrulaması ve yetkilendirmesi](containers/tutorial-auth-aad.md)  
[Uygulama Hizmetinde kimlik doğrulamayı ve yetkilendirmeyi özelleştirme](app-service-authentication-how-to.md)

Sağlayıcıya özgü nasıl yapılacağını kılavuzları:

* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma][AAD]
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma][Facebook]
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma][Google]
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma][MSA]
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma][Twitter]
* [Nasıl kullanılır: Uygulamanız için özel kimlik doğrulaması kullanın][custom-auth]

[AAD]: configure-authentication-provider-aad.md
[Facebook]: configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: configure-authentication-provider-microsoft.md
[Twitter]: configure-authentication-provider-twitter.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
