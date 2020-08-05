---
title: Kimlik doğrulama ve yetkilendirme
description: Azure App Service ve Azure Işlevlerinde yerleşik kimlik doğrulama ve yetkilendirme desteği hakkında bilgi edinin ve uygulamanızın yetkisiz erişime karşı nasıl güvenli hale getirilmesine yardımcı olabilir.
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.topic: article
ms.date: 07/08/2020
ms.reviewer: mahender
ms.custom: seodec18, fasttrack-edit, has-adal-ref
ms.openlocfilehash: c8e0b476c50378bde00e01a39985fbcc188f04ed
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87562387"
---
# <a name="authentication-and-authorization-in-azure-app-service-and-azure-functions"></a>Azure App Service ve Azure Işlevlerinde kimlik doğrulama ve yetkilendirme

Azure App Service, yerleşik kimlik doğrulama ve yetkilendirme desteği sunarak, Web uygulamanızda, yeniden derlenen API 'de ve mobil arka uçta ve ayrıca [Azure işlevlerinde](../azure-functions/functions-overview.md)en az veya hiç kod yazmadan kullanıcılara oturum açabilir ve verilere erişebilirsiniz. Bu makalede, App Service uygulamanızın kimlik doğrulama ve yetkilendirme işlemlerini basitleştirmeye nasıl yardımcı olduğu açıklanır.

Güvenli kimlik doğrulama ve yetkilendirme, Federasyon, şifreleme, [JSON Web belirteçleri (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) yönetimi, [izin verme](https://oauth.net/2/grant-types/)vb. gibi güvenlik konusunda ayrıntılı bilgiler gerektirir. App Service, müşterinize iş değeri sağlamaya yönelik daha fazla zaman ve enerji harcayabilmeniz için bu yardımcı programları sağlar.

> [!IMPORTANT]
> Kimlik doğrulama ve yetkilendirme için bu özelliği kullanmanız gerekli değildir. Seçtiğiniz Web çerçevesindeki paketlenmiş güvenlik özelliklerini kullanabilir veya kendi yardımcı programlarını yazabilirsiniz. Bununla birlikte, Chrome 80 ' in, farklı tanımlama bilgileri için (2020 Mart 'ta Yayın tarihi) ve özel uzaktan kimlik doğrulama veya siteler arası tanımlama bilgisine bağlı diğer senaryolar, istemci Chrome tarayıcıları güncelleştirilirken kesintiye uğrayabilecek [şekilde değişiklik yapıyor](https://www.chromestatus.com/feature/5088147346030592) . Farklı tarayıcılar için farklı SameSite davranışlarını desteklemesi gerektiğinden geçici çözüm karmaşıktır. 
>
> App Service tarafından barındırılan ASP.NET Core 2,1 ve üzeri sürümleri, bu son değişiklik için zaten düzeltme eki uygulanmış ve Chrome 80 ve daha eski tarayıcıları uygun şekilde işleyecek. Ayrıca, ASP.NET Framework 4.7.2 için de aynı düzeltme eki, 2020 Ocak boyunca App Service örneklerine dağıtılır. Daha fazla bilgi için bkz. [Azure App Service SameSite tanımlama bilgisi güncelleştirmesi](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/).
>

> [!NOTE]
> Kimlik doğrulama/yetkilendirme özelliği bazen "kolay kimlik doğrulaması" olarak da adlandırılır.

> [!NOTE]
> Bu özelliği etkinleştirmek, [https 'yi zorlamak](configure-ssl-bindings.md#enforce-https)için App Service yapılandırma ayarından bağımsız olarak, uygulamanıza yönelik **Tüm** güvenli olmayan http isteklerinin otomatik olarak https 'ye yönlendirilmesini sağlar. Gerekirse, `requireHttps` [kimlik doğrulama ayarları yapılandırma dosyasındaki](app-service-authentication-how-to.md#configuration-file-reference)ayarı aracılığıyla bunu devre dışı bırakabilirsiniz, ancak bundan böyle hiçbir güvenlik BELIRTECININ güvenli olmayan http bağlantıları üzerinden iletilmemesini sağlamak için dikkatli olmanız gerekir.

Yerel mobil uygulamalara özgü bilgiler için, bkz. [Azure App Service ile mobil uygulamalar Için Kullanıcı kimlik doğrulaması ve yetkilendirme](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Nasıl çalışır?

### <a name="on-windows"></a>Windows üzerinde

Kimlik doğrulama ve yetkilendirme modülü, uygulama kodunuzla aynı korumalı alanda çalışır. Etkin olduğunda, her gelen HTTP isteği, uygulama kodunuz tarafından işlenemeden önce üzerinden geçirilir.

![Dağıtılan sitede trafiğe izin vermeden önce kimlik sağlayıcılarıyla etkileşen site korumalı bir işlem tarafından kesilen istekleri gösteren bir mimari diyagramı](media/app-service-authentication-overview/architecture.png)

Bu modül, uygulamanız için çeşitli şeyleri işler:

- Belirtilen sağlayıcıyla kullanıcıların kimliğini doğrular
- Belirteçleri doğrular, depolar ve yeniler
- Kimliği doğrulanmış oturumu yönetir
- Kimlik bilgilerini istek üst bilgilerine ekler

Modül, uygulama kodınızdan ayrı olarak çalışır ve uygulama ayarları kullanılarak yapılandırılır. SDK, belirli dil veya uygulama kodunuzda değişiklik yapılması gerekmez. 

### <a name="on-containers"></a>Kapsayıcılar üzerinde

Kimlik doğrulama ve yetkilendirme modülü, uygulama kodınızdan yalıtılmış ayrı bir kapsayıcıda çalışır. [Amelçi deseninin](https://docs.microsoft.com/azure/architecture/patterns/ambassador)bilindiğinin kullanılması, Windows 'da olduğu gibi benzer işlevleri gerçekleştirmek üzere gelen trafikle etkileşime girer. İşlem içinde çalıştırılmadığından, belirli dil çerçeveleri ile doğrudan tümleştirme mümkün değildir; Ancak, uygulamanızın ihtiyaç duyacağı ilgili bilgiler, aşağıda açıklandığı gibi istek üstbilgileri kullanılarak geçirilir.

### <a name="userapplication-claims"></a>Kullanıcı/uygulama talepleri

Tüm dil çerçeveleri için App Service, gelen belirteçteki talepleri (kimliği doğrulanmış bir son kullanıcının veya bir istemci uygulamasından mi olsun), istek üst bilgilerine ekleme göre kodunuzun kullanımına sunar. ASP.NET 4,6 uygulamaları için App Service [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) 'ı kimliği doğrulanmış kullanıcının taleplerine göre doldurur. böylece, özniteliği de dahil olmak üzere standart .NET kod modelini izleyebilirsiniz `[Authorize]` . Benzer şekilde, PHP uygulamaları için App Service değişkeni doldurur `_SERVER['REMOTE_USER']` . Java uygulamaları için, talepler [Tomcat servlet 'ten erişilebilir](containers/configure-language-java.md#authenticate-users-easy-auth).

[Azure işlevleri](../azure-functions/functions-overview.md)için, `ClaimsPrincipal.Current` .NET kodu için doldurulmamış, ancak istek üst bilgilerinde kullanıcı taleplerini bulabilir veya istek `ClaimsPrincipal` bağlamından ya da bir bağlama parametresi aracılığıyla nesne alabilirsiniz. Daha fazla bilgi için bkz. [istemci kimlikleriyle çalışma](../azure-functions/functions-bindings-http-webhook-trigger.md#working-with-client-identities) .

Daha fazla bilgi için bkz. [Kullanıcı taleplerine erişme](app-service-authentication-how-to.md#access-user-claims).

> [!NOTE]
> Şu anda ASP.NET Core, geçerli kullanıcıyı kimlik doğrulama/yetkilendirme özelliğiyle doldurmayı desteklememektedir. Ancak, bu boşluğun doldurulmasına yardımcı olmak için bazı [üçüncü taraf, açık kaynaklı ara yazılım bileşenleri](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth) bulunur.
>

### <a name="token-store"></a>Belirteç deposu

App Service, Web uygulamalarınızın, API 'lerin veya yerel mobil uygulamalarınızın kullanıcılarıyla ilişkili belirteçlerin bir deposu olan yerleşik bir belirteç deposu sağlar. Herhangi bir sağlayıcı ile kimlik doğrulamasını etkinleştirdiğinizde, bu belirteç deposu uygulamanız için hemen kullanılabilir. Uygulama kodunuzun Kullanıcı adına bu sağlayıcılardan veriye erişmesi gerekiyorsa, örneğin: 

- kimliği doğrulanmış kullanıcının Facebook zaman çizelgesinde gönderi
- Microsoft Graph API 'sini kullanarak kullanıcının şirket verilerini okuyun

Uygulamanızda bu belirteçleri toplamak, depolamak ve yenilemek için genellikle kod yazmanız gerekir. Belirteç deposu ile, ihtiyacınız olduğunda [belirteçleri alır](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) ve geçersiz hale geldiklerinde [bunları yenilemek App Service söyleyin](app-service-authentication-how-to.md#refresh-identity-provider-tokens) . 

KIMLIK belirteçleri, erişim belirteçleri ve yenileme belirteçleri kimliği doğrulanmış oturum için önbelleğe alınır ve yalnızca ilişkili kullanıcı tarafından erişilebilir.  

Uygulamanızdaki belirteçlerle çalışmanız gerekmiyorsa, belirteç deposunu devre dışı bırakabilirsiniz.

### <a name="logging-and-tracing"></a>Günlüğe kaydetme ve izleme

[Uygulama günlüğünü etkinleştirirseniz](troubleshoot-diagnostic-logs.md), kimlik doğrulama ve yetkilendirme izlemelerini doğrudan günlük dosyalarınızda görürsünüz. Beklemediğiniz bir kimlik doğrulama hatası görürseniz, mevcut uygulama günlüklerinizi inceleyerek tüm ayrıntıları rahat bir şekilde bulabilirsiniz. [Başarısız istek izlemeyi](troubleshoot-diagnostic-logs.md)etkinleştirirseniz, kimlik doğrulama ve yetkilendirme modülünün başarısız bir istekte hangi rol oynadığını tam olarak görebilirsiniz. İzleme günlüklerinde adlı bir modüle başvuruları arayın `EasyAuthModule_32/64` . 

## <a name="identity-providers"></a>Kimlik sağlayıcıları

App Service, üçüncü taraf bir kimlik sağlayıcısının sizin için Kullanıcı kimliklerini ve kimlik doğrulama akışını yönettiği [Federal Kimlik](https://en.wikipedia.org/wiki/Federated_identity)kullanır. Beş kimlik sağlayıcısı varsayılan olarak kullanılabilir: 

| Sağlayıcı | Oturum açma uç noktası |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft Hesabı](../active-directory/develop/v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/identity/choose-auth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |
| Herhangi bir [OpenID Connect](https://openid.net/connect/) sağlayıcısı (Önizleme) | `/.auth/login/<providerName>` |

Kimlik doğrulama ve yetkilendirmeyi bu sağlayıcılardan biriyle etkinleştirdiğinizde, oturum açma uç noktası Kullanıcı kimlik doğrulaması ve sağlayıcıdan kimlik doğrulama belirteçleri doğrulaması için kullanılabilir. Kullanıcılarınıza dilediğiniz sayıda bu oturum açma seçeneğini kolayca sağlayabilirsiniz.

Diğer kimlik sağlayıcıları veya özel bir kimlik doğrulama çözümüyle tümleştirmek için [eski bir genişletilebilirlik yolu][custom-auth] vardır, ancak bu önerilmez. Bunun yerine, OpenID Connect desteğini kullanmayı göz önünde bulundurun.

## <a name="authentication-flow"></a>Kimlik doğrulaması akışı

Kimlik doğrulama akışı tüm sağlayıcılar için aynıdır, ancak sağlayıcının SDK 'Sı ile oturum açmak isteyip istemediğinize bağlı olarak farklılık gösterir:

- Sağlayıcı SDK 'Sı olmadan: uygulama App Service için Federasyon oturum açma temsilcisi sağlar. Bu durum genellikle, sağlayıcının oturum açma sayfasını kullanıcıya sunmanızı sağlayan tarayıcı uygulamaları ile aynıdır. Sunucu kodu, oturum açma işlemini yönetir, bu nedenle _sunucu yönlendirmeli akış_ veya _sunucu akışı_olarak da adlandırılır. Bu durum tarayıcı uygulamaları için geçerlidir. SDK, kullanıcıların App Service kimlik doğrulamasıyla oturum açmasını sağlamak üzere bir Web görünümü açtığından Mobile Apps istemci SDK 'sını kullanarak kullanıcıları imzalayasağlayan yerel uygulamalar için de geçerlidir. 
- Sağlayıcı SDK ile: uygulama, kullanıcıları sağlayıcıya el ile imzalar ve doğrulama için App Service kimlik doğrulama belirtecini gönderir. Bu durum genellikle, sağlayıcının oturum açma sayfasını kullanıcıya sunamıyoruz ve tarayıcı tarafından daha az uygulamalar için kullanılır. Uygulama kodu, oturum açma işlemini yönetir, bu nedenle _istemci yönlendirmeli akış_ veya _istemci akışı_olarak da adlandırılır. Bu durum REST API 'Leri, [Azure işlevleri](../azure-functions/functions-overview.md)ve JavaScript tarayıcı istemcileri için, oturum açma sürecinde daha fazla esneklik gerektiren tarayıcı uygulamaları için de geçerlidir. Ayrıca, sağlayıcının SDK 'sını kullanarak kullanıcıları imzalayasağlayan yerel mobil uygulamalar için de geçerlidir.

> [!NOTE]
> App Service bir güvenilen tarayıcı uygulamasından App Service veya [Azure işlevlerinde](../azure-functions/functions-overview.md) başka bir REST API olan çağrılar, sunucu ile yönlendirilen Flow kullanılarak doğrulanabilir. Daha fazla bilgi için bkz. [App Service kimlik doğrulaması ve yetkilendirmeyi özelleştirme](app-service-authentication-how-to.md).
>

Aşağıdaki tabloda, kimlik doğrulama akışı adımları gösterilmektedir.

| Adım | Sağlayıcı SDK 'Sı olmadan | Sağlayıcı SDK ile |
| - | - | - |
| 1. Kullanıcı oturum açma | İstemcisini olarak yeniden yönlendirir `/.auth/login/<provider>` . | İstemci kodu, kullanıcıyı doğrudan sağlayıcının SDK 'Sı ile imzalar ve bir kimlik doğrulama belirteci alır. Bilgi için sağlayıcının belgelerine bakın. |
| 2. kimlik doğrulama sonrası | Sağlayıcı istemciyi öğesine yeniden yönlendirir `/.auth/login/<provider>/callback` . | İstemci kodu, sağlayıcıya doğrulama için [olan belirteci gönderir](app-service-authentication-how-to.md#validate-tokens-from-providers) `/.auth/login/<provider>` . |
| 3. kimliği doğrulanmış oturum oluşturma | App Service, yanıta kimliği doğrulanmış tanımlama bilgisi ekler. | App Service, kendi kimlik doğrulama belirtecini istemci koduna döndürür. |
| 4. kimliği doğrulanmış içerik hizmeti | İstemci sonraki isteklerde kimlik doğrulama tanımlama bilgisi içerir (otomatik olarak tarayıcı tarafından işlenir). | İstemci kodu, kimlik doğrulama belirtecini `X-ZUMO-AUTH` üst bilgide gösterir (Mobile Apps Istemci SDK 'ları tarafından otomatik olarak işlenir). |

İstemci tarayıcıları için, App Service tüm kimliği doğrulanmamış kullanıcıları otomatik olarak öğesine yönlendirebilir `/.auth/login/<provider>` . Ayrıca, bir veya daha fazla bağlantıyla kullanıcılara `/.auth/login/<provider>` tercih ettiğiniz sağlayıcıyı kullanarak oturum açmak için bir veya daha fazla bağlantı sunabilirsiniz.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Yetkilendirme davranışı

[Azure Portal](https://portal.azure.com), gelen isteğin kimliği doğrulanmamış bir dizi davranışla App Service yetkilendirmeyi yapılandırabilirsiniz.

!["İstek kimlik doğrulaması olmadığında gerçekleştirilecek eylemi" açılan ekran görüntüsü](media/app-service-authentication-overview/authorization-flow.png)

Aşağıdaki başlıklar seçenekleri anlatmaktadır.

### <a name="allow-anonymous-requests-no-action"></a>Anonim isteklere izin ver (eylem yok)

Bu seçenek, uygulama kodunuza kimliği doğrulanmamış trafik yetkilendirmesini erteler. Kimliği doğrulanmış istekler için, App Service HTTP üstbilgilerinde kimlik doğrulama bilgileri boyunca de geçirilir. 

Bu seçenek, anonim istekleri işlemek için daha fazla esneklik sağlar. Örneğin, kullanıcılarınıza [birden çok oturum açma sağlayıcısı sunmanıza](app-service-authentication-how-to.md#use-multiple-sign-in-providers) olanak tanır. Ancak, kod yazmanız gerekir. 

### <a name="allow-only-authenticated-requests"></a>Yalnızca kimliği doğrulanmış isteklere izin ver

Seçeneği ** \<provider> Ile oturum açın **. App Service, tüm anonim istekleri `/.auth/login/<provider>` seçtiğiniz sağlayıcıya yönlendirir. Anonim istek yerel bir mobil uygulamadan geliyorsa, döndürülen yanıt bir olur `HTTP 401 Unauthorized` .

Bu seçenekle, uygulamanızda herhangi bir kimlik doğrulama kodu yazmanız gerekmez. Role özgü yetkilendirme gibi daha ayrıntılı yetkilendirme, kullanıcının taleplerini inceleyerek (bkz. [erişim kullanıcı talepleri](app-service-authentication-how-to.md#access-user-claims)) işlenebilir.

> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir.

## <a name="more-resources"></a>Diğer kaynaklar

[Öğretici: kullanıcıların Azure App Service (Windows) ile uçtan uca kimlik doğrulama ve yetkilendirme](app-service-web-tutorial-auth-aad.md)  
[Öğretici: Linux için Azure App Service Kullanıcı tarafından uçtan uca kimlik doğrulama ve yetkilendirme](containers/tutorial-auth-aad.md)  
App Service kimlik doğrulaması [ve yetkilendirmeyi özelleştirme](app-service-authentication-how-to.md) 
 [Azure AppService EasyAuth (3. taraf)](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth) 
 .NET Core tümleştirmesi [.NET Core ile çalışan Azure App Service kimlik doğrulaması alma (3. taraf)](https://github.com/kirkone/KK.AspNetCore.EasyAuthAuthentication)

Sağlayıcıya özgü nasıl yapılır Kılavuzu:

* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma][AAD]
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma][Facebook]
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma][Google]
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma][MSA]
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma][Twitter]
* [Uygulamanızı, oturum açmak için bir OpenID Connect sağlayıcısı kullanacak şekilde yapılandırma (Önizleme)][OIDC]

[AAD]: configure-authentication-provider-aad.md
[Facebook]: configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: configure-authentication-provider-microsoft.md
[Twitter]: configure-authentication-provider-twitter.md
[OIDC]: configure-authentication-provider-openid-connect.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
