---
title: AuthN/AuthO'nun gelişmiş kullanımı
description: Uygulama Hizmeti'ndeki kimlik doğrulama ve yetkilendirme özelliğini farklı senaryolar için özelleştirmeyi ve kullanıcı talepleri ve farklı belirteçler almayı öğrenin.
ms.topic: article
ms.date: 10/24/2019
ms.custom: seodec18
ms.openlocfilehash: d57b196bf95ebdf31bc459ad4b9d718fd32ca495
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280839"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirmenin gelişmiş kullanımı

Bu makalede, [Uygulama Hizmeti'nde](overview-authentication-authorization.md)yerleşik kimlik doğrulama ve yetkilendirmeyi nasıl özelleştirip uygulamanızdan kimliğinizi nasıl yönetebildiğiniz gösterilmektedir. 

Hızlı bir şekilde başlamak için aşağıdaki öğreticilerden birine bakın:

* [Öğretici: Azure Uygulama Hizmeti'nde (Windows) kullanıcıların sonuna kadar kimlik doğrulaması ve yetkilendirmesi](app-service-web-tutorial-auth-aad.md)
* [Öğretici: Linux için Azure Uygulama Hizmeti'nde kullanıcıların sonuna kadar kimlik doğrulaması ve yetkilendirmesi](containers/tutorial-auth-aad.md)
* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-aad.md)
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-facebook.md)
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-google.md)
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-microsoft.md)
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-twitter.md)

## <a name="use-multiple-sign-in-providers"></a>Birden çok oturum açma sağlayıcısı kullanma

Portal yapılandırması, kullanıcılarınıza (hem Facebook hem de Twitter gibi) birden fazla oturum açma sağlayıcısı sunmak için anahtar teslimi bir yol sunmaz. Ancak, işlevselliği uygulamanıza eklemek zor değildir. Adımlar aşağıdaki gibi özetlenmiştir:

İlk olarak, Azure portalındaki **Kimlik Doğrulama / Yetkilendirme** sayfasında, etkinleştirmek istediğiniz kimlik sağlayıcısının her birini yapılandırın.

**İstek doğrulanmadığında yapılacak**Eylem'de, **Anonim isteklere İzin Ver'i (eylem yok) seçeneğini belirleyin.**

Oturum açma sayfasında, navigasyon çubuğunda veya uygulamanızın başka bir konumuna, etkinleştirdiğiniz sağlayıcıların her birine`/.auth/login/<provider>`oturum açma bağlantısı ekleyin ( ). Örnek:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Kullanıcı bağlantılardan birini tıklattığında, ilgili oturum açma sayfası kullanıcıda oturum açmanız için açılır.

Oturum açma sonrası kullanıcıyı özel bir URL'ye `post_login_redirect_url` yönlendirmek için sorgu dize parametresini kullanın (kimlik sağlayıcı yapılandırmanızda Yeniden Yönlendirme URI ile karıştırılmamalıdır). Örneğin, kullanıcıya oturum `/Home/Index` açma dan sonra gitmek için aşağıdaki HTML kodunu kullanın:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Sağlayıcılardan gelen belirteçleri doğrulama

İstemci tarafından yönlendirilen oturum açmada, uygulama kullanıcıdaki oturumunu el ile sağlayıcıya işaretler ve doğrulama için Uygulama Hizmeti'ne kimlik doğrulama belirteci gönderir (bkz. [Kimlik Doğrulama akışı).](overview-authentication-authorization.md#authentication-flow) Bu doğrulamanın kendisi aslında istediğiniz uygulama kaynaklarına erişmenizi vermez, ancak başarılı bir doğrulama size uygulama kaynaklarına erişmek için kullanabileceğiniz bir oturum belirteci sağlar. 

Sağlayıcı belirteci doğrulamak için, App Service uygulamasının öncelikle istenen sağlayıcıyla yapılandırılması gerekir. Çalışma zamanında, sağlayıcınızdan kimlik doğrulama belirteci aldıktan sonra, `/.auth/login/<provider>` belirteci doğrulama için gönderin. Örnek: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

Belirteç biçimi sağlayıcıya göre biraz değişir. Ayrıntılar için aşağıdaki tabloya bakın:

| Sağlayıcı değeri | İstek gövdesinde gerekli | Yorumlar |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | Özellik `expires_in` isteğe bağlıdır. <br/>Live hizmetlerinden belirteci isteğinde bulunduktan sonra, her zaman `wl.basic` kapsamı isteyin. |
| `google` | `{"id_token":"<id_token>"}` | Özellik `authorization_code` isteğe bağlıdır. Belirtildiğizaman, isteğe bağlı olarak `redirect_uri` özellik eşlik edebilir. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Facebook'tan geçerli bir [kullanıcı erişim jetonu](https://developers.facebook.com/docs/facebook-login/access-tokens) kullanın. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Sağlayıcı belirteci başarıyla doğrulanırsa, API yanıt `authenticationToken` gövdesinde oturum belirteci olan bir ile döndürür. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Bu oturum belirteci ne zaman, http isteklerinize `X-ZUMO-AUTH` üstbilgi ekleyerek korumalı uygulama kaynaklarına erişebilirsiniz. Örnek: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Oturumu oturumu niçin oturumu niçin imzalama

Kullanıcılar, uygulamanın `GET` `/.auth/logout` bitiş noktasına bir istek göndererek oturum açma başlatabilir. İstek `GET` aşağıdakileri yapar:

- Geçerli oturumdaki kimlik doğrulama tanımlama bilgilerini temizler.
- Geçerli kullanıcının belirteçlerini belirteç deposundan siler.
- Azure Active Directory ve Google için, kimlik sağlayıcısında sunucu tarafında oturum açma gerçekleştirir.

Web sayfasında basit bir oturum kapatma bağlantısı şöyle olabilir:

```HTML
<a href="/.auth/logout">Sign out</a>
```

Varsayılan olarak, başarılı bir oturum son verme istemciyi URL'ye `/.auth/logout/done`yönlendirir. Sorgu parametresini `post_logout_redirect_uri` ekleyerek oturum dışı bırakma sayfasını değiştirebilirsiniz. Örnek:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Değerini [kodlamanız](https://wikipedia.org/wiki/Percent-encoding) `post_logout_redirect_uri`önerilir.

Tam nitelikli URL'ler kullanırken, URL'nin aynı etki alanında barındırılması veya uygulamanız için izin verilen harici yönlendirme URL'si olarak yapılandırılması gerekir. Aşağıdaki örnekte, aynı etki `https://myexternalurl.com` alanında barındırılan olmayan ada yeniden yönlendirmek için:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

[Azure Bulut Kabuğu'nda](../cloud-shell/quickstart.md)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>URL parçalarını koruma

Kullanıcılar uygulamanızda oturum açındıktan sonra genellikle aynı sayfanın aynı bölümüne yönlendirilmek `/wiki/Main_Page#SectionZ`isterler. Ancak, [URL parçaları](https://wikipedia.org/wiki/Fragment_identifier) (örneğin, `#SectionZ`) sunucuya hiç gönderilmediği için, OAuth oturum açma tamamlandıktan ve uygulamanıza geri yönlendirildikten sonra varsayılan olarak korunmuyorlar. Kullanıcılar daha sonra istenilen çapaya tekrar gitmek istediklerinde en uygun olmayan deneyimi yaşarlar. Bu sınırlama, sunucu tarafındaki tüm kimlik doğrulama çözümleri için geçerlidir.

Uygulama Hizmeti kimlik doğrulamasında, OAuth oturum açma boyunca URL parçalarını koruyabilirsiniz. Bunu yapmak için, '' `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` `true`için çağrılan bir uygulama ayarı ayarlayın. Bunu [Azure portalında](https://portal.azure.com)yapabilir veya [Azure Bulut Kabuğu'nda](../cloud-shell/quickstart.md)aşağıdaki komutu çalıştırabilirsiniz:

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Kullanıcı taleplerine erişin

Uygulama Hizmeti, özel üstbilgileri kullanarak kullanıcı taleplerini uygulamanıza iletin. Dış isteklerin bu üstbilgi kümelemesine izin verilmez, bu nedenle yalnızca App Service tarafından ayarlanırsa mevcuttur. Bazı örnek üstbilgi şunları içerir:

* X-MS-İsteMCİ-ESAS-ADI
* X-MS-İSTEMCI-MÜDÜR-KİmLİk

Herhangi bir dilde veya çerçevede yazılmış kod, bu üstbilgilerden ihtiyaç duyduğu bilgileri alabilir. 4.6 uygulamasıASP.NET için **ClaimsPrincipal** otomatik olarak uygun değerlerle ayarlanır. Ancak ASP.NET Core, App Service kullanıcı talepleriyle tümlebilen bir kimlik doğrulama aracı sağlamaz. Geçici çözüm için [MaximeRouiller.Azure.AppService.EasyAuth](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)adresine bakın.

Uygulamanız ayrıca kimlik doğrulaması yapılan kullanıcı yla `/.auth/me`ilgili ek bilgileri arayarak edinebilirsiniz. Mobil Uygulamalar sunucusu SDK'ları bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için Azure [Mobil Apps Düğümü.js SDK'yı nasıl kullanacağız](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)ve [Azure Mobile Apps için .NET arka uç sunucusu SDK ile nasıl çalışırsınız.](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info)

## <a name="retrieve-tokens-in-app-code"></a>Uygulama kodundaki belirteçleri alma

Sunucu kodunuzdan, sağlayıcıya özgü belirteçler istek üstbilgisine enjekte edilir, böylece bunlara kolayca erişebilirsiniz. Aşağıdaki tablo da olası belirteç üstbilgi adlarını gösterir:

| Sağlayıcı | Üstbilgi adları |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook Belirteci | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft Hesabı | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

İstemci kodunuzdan (mobil uygulama veya tarayıcı içi JavaScript `/.auth/me`gibi), bir HTTP `GET` isteği gönderin. Döndürülen JSON sağlayıcıya özgü belirteçlere sahiptir.

> [!NOTE]
> Erişim belirteçleri sağlayıcı kaynaklarına erişmek içindir, bu nedenle yalnızca sağlayıcınızı bir istemci sırrıyla yapılandırdığınızda mevcut olurlar. Yenileme belirteçlerini nasıl alacağınızı görmek için erişim belirteçlerini yenile'ye bakın.

## <a name="refresh-identity-provider-tokens"></a>Kimlik sağlayıcı belirteçlerini yenileme

Sağlayıcınızın erişim belirteci [(oturum belirteci](#extend-session-token-expiration-grace-period)değil) sona erdiğinde, bu belirteci yeniden kullanmadan önce kullanıcının yeniden kimlik doğruluğunu yeniden doğrulamanız gerekir. Başvurunuzun `/.auth/refresh` bitiş noktasına bir `GET` arama yaparak belirteç sona ermesini önleyebilirsiniz. Çağrıldığında, App Service kimlik doğrulaması yapılan kullanıcıiçin belirteç deposundaki erişim belirteçlerini otomatik olarak yeniler. Uygulama kodunuztarafından sonraki belirteçler için istekler yenilenmiş belirteçleri alırsınız. Ancak, belirteç yenilemesinin çalışması için belirteç deposunun sağlayıcınız için [yenileme belirteçleri](https://auth0.com/learn/refresh-tokens/) içermesi gerekir. Yenileme belirteçleri almanın yolu her sağlayıcı tarafından belgelenir, ancak aşağıdaki liste kısa bir özetidir:

- **Google**: API çağrınıza `access_type=offline` `/.auth/login/google` bir sorgu dizesi parametresini ekleyin. Mobil Uygulamalar SDK kullanıyorsanız, `LogicAsync` parametreyi aşırı yüklerden birine ekleyebilirsiniz (Bkz. Google [Yenile Belirteçleri).](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)
- **Facebook**: Yenileme belirteçleri sağlamaz. Uzun ömürlü belirteçlerin süresi 60 gün içinde doluyor (bkz. [Facebook Erişim Belirteçleri'nin Sona Ermesi ve Uzatılması).](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)
- **Twitter**: Erişim belirteçleri sona ermez [(Bkz. Twitter OAuth SSS).](https://developer.twitter.com/en/docs/basics/authentication/FAQ)
- **Microsoft Hesabı**: Microsoft Hesap Kimlik Doğrulama `wl.offline_access` [Ayarlarını yapılandırırken](configure-authentication-provider-microsoft.md)kapsamı seçin.
- **Azure Etkin Dizin**: In [https://resources.azure.com](https://resources.azure.com), aşağıdaki adımları yapın:
    1. Sayfanın üst kısmında **Oku/Yaz'ı**seçin.
    2. Sol tarayıcıda, **abonelikabonelik** > **_\<\_adı_** > **sites** > **_\<\_ _**  >  **providers** > **_\<\_\_ _** > **kaynakGruplar** > kaynak grup adı>sağlayıcıları**Microsoft.Web**siteleri uygulama adı> >  **config** > **authsettings**gidin. 
    3. **Düzenle**’ye tıklayın.
    4. Aşağıdaki özelliği değiştirin. Uygulama kimliği>erişmek istediğiniz hizmetin Azure Etkin Dizin uygulama kimliğiyle değiştirin. _\_ \<_

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. **Put'a**tıklayın. 

Sağlayıcınız yapılandırıldıktan sonra, [belirteç deposunda erişim belirteci için yenileme belirteci ve son kullanma süresini bulabilirsiniz.](#retrieve-tokens-in-app-code) 

Erişim belirtecinizi herhangi bir zamanda `/.auth/refresh` yenilemek için herhangi bir dili aramanız nıza gerek. Aşağıdaki parçacık, bir JavaScript istemcisinden erişim belirteçlerinizi yenilemek için jQuery'yi kullanır.

```JavaScript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Bir kullanıcı uygulamanıza verilen izinleri iptal ederse, `/.auth/me` aramanız bir `403 Forbidden` yanıtla başarısız olabilir. Hataları tanılamak için, ayrıntılar için uygulama günlüklerinizi kontrol edin.

## <a name="extend-session-token-expiration-grace-period"></a>Oturum belirteç süresi nin uzatılması

Kimlik doğrulaması oturumu8 saat sonra sona erer. Kimlik doğrulaması yapılan oturumun süresi dolduktan sonra, varsayılan olarak 72 saatlik bir yetkisiz kullanım süresi vardır. Bu yetkisiz kullanım süresi içinde, kullanıcının kimliğini yeniden doğrulamadan Uygulama Hizmeti ile oturum belirtecinizi yenilemenize izin verilir. Oturum belirteçleriniz geçersiz olduğunda arama `/.auth/refresh` yapabilirsiniz ve belirteç sona ermesini kendiniz izlemeniz gerekmez. 72 saatlik yetkisiz kullanım süresi sona erdiğinde, geçerli bir oturum belirteci almak için kullanıcının yeniden oturum açması gerekir.

72 saat sizin için yeterli değilse, bu son kullanma süresini uzatabilirsiniz. Sona erme süresinin uzun bir süre uzatılması önemli güvenlik etkilerine sahip olabilir (örneğin, kimlik doğrulama belirteci sızdırıldığında veya çalındığında). Bu nedenle varsayılan 72 saat içinde bırakmalısınız veya uzatma süresini en küçük değere ayarlamalısınız.

Varsayılan son kullanma süresini uzatmak için [Bulut Kabuğu'nda](../cloud-shell/overview.md)aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Yetkisiz kullanım süresi yalnızca kimlik sağlayıcılarının belirteçleri için değil, Yalnızca App Service kimlik doğrulama oturumu için geçerlidir. Süresi dolan sağlayıcı belirteçleri için yetkisiz kullanım süresi yoktur. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Oturum açma hesaplarının etki alanını sınırlama

Hem Microsoft Hesabı hem de Azure Etkin Dizini, birden çok etki alanında oturum açmanızı sağlar. Örneğin, Microsoft Hesabı _outlook.com,_ _live.com_ve _hotmail.com_ hesaplara izin verir. Azure AD, oturum açma hesapları için herhangi bir sayıda özel etki alanına izin verir. Ancak, kullanıcılarınızı doğrudan kendi markalı Azure AD oturum açma sayfanıza (örneğin) `contoso.com`hızlandırmak isteyebilirsiniz. Oturum açma hesaplarının etki alanı adını önermek için aşağıdaki adımları izleyin.

In [https://resources.azure.com](https://resources.azure.com), **abonelikler** > **_\<\_abonelik adı_** > **kaynak** > **_\<Gruplar kaynak\_grup\_adı>_**  >  **sağlayıcıları** > **Microsoft.Web** > **siteleri** > **_\<uygulama\_adı>_**  >  **config** > **authsettings**gidin. 

**Edit'i**tıklatın, aşağıdaki özelliği değiştirin ve sonra **Koy'u**tıklatın. _ \<Etki alanı\_adı>_ istediğiniz etki alanıyla değiştirdiğinden emin olun.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```

Bu ayar, `domain_hint` giriş yeniden yönlendirme URL'sine sorgu dize parametresini ekler. 

> [!IMPORTANT]
> İstemcinin yönlendirme URL'sini `domain_hint` aldıktan sonra parametreyi kaldırması ve ardından farklı bir etki alanıyla oturum açması mümkündür. Bu nedenle, bu işlev kullanışlı olsa da, bir güvenlik özelliği değildir.
>

## <a name="authorize-or-deny-users"></a>Kullanıcıları yetkilendirme veya reddetme

App Service en basit yetkilendirme durumunu ele alırken (örneğin, kimlik doğrulamamış istekleri reddetmek), uygulamanız yalnızca belirli bir kullanıcı grubuna erişimi sınırlamak gibi daha ayrıntılı yetkilendirme davranışı gerektirebilir. Bazı durumlarda, oturum açmış kullanıcıya erişime izin vermek veya reddetmek için özel uygulama kodu yazmanız gerekir. Diğer durumlarda, Uygulama Hizmeti veya kimlik sağlayıcınız kod değişikliğine gerek kalmadan yardımcı olabilir.

- [Sunucu düzeyi](#server-level-windows-apps-only)
- [Kimlik sağlayıcı düzeyi](#identity-provider-level)
- [Uygulama düzeyi](#application-level)

### <a name="server-level-windows-apps-only"></a>Sunucu düzeyi (yalnızca Windows uygulamaları)

Herhangi bir Windows uygulaması için, *Web.config* dosyasını düzenleyerek IIS web sunucusunun yetkilendirme davranışını tanımlayabilirsiniz. Linux uygulamaları IIS kullanmaz ve *Web.config*üzerinden yapılandırılamaz.

1. Gezinme`https://<app-name>.scm.azurewebsites.net/DebugConsole`

1. Uygulama Hizmeti dosyalarınızın tarayıcı gezgininde *siteye/wwwroot*adresine gidin. Bir *Web.config* yoksa, **+**  >  **Yeni Dosya**seçerek oluşturun. 

1. *Web.config* için kalemi seçin ve bunu edin. Aşağıdaki yapılandırma kodunu ekleyin ve **Kaydet'i**tıklatın. *Web.config* zaten varsa, sadece `<authorization>` içinde her şeyi ile öğeyi ekleyin. `<allow>` Öğede izin vermek istediğiniz hesapları ekleyin.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
       <system.web>
          <authorization>
            <allow users="user1@contoso.com,user2@contoso.com"/>
            <deny users="*"/>
          </authorization>
       </system.web>
    </configuration>
    ```

### <a name="identity-provider-level"></a>Kimlik sağlayıcı düzeyi

Kimlik sağlayıcı belirli anahtar teslim yetkilendirme sağlayabilir. Örnek:

- [Azure Uygulama Hizmeti](configure-authentication-provider-aad.md)için kurumsal düzeyde erişimi doğrudan Azure AD'de [yönetebilirsiniz.](../active-directory/manage-apps/what-is-access-management.md) Yönergeler için, [kullanıcının bir uygulamaya erişimini nasıl kaldıracağınız'a](../active-directory/manage-apps/methods-for-removing-user-access.md)bakın.
- [Google](configure-authentication-provider-google.md)için, bir [kuruluşa](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) ait Google API projeleri yalnızca kuruluşunuzdaki kullanıcılara erişime izin verecek şekilde yapılandırılabilir (bkz. [Google'ın **OAuth 2.0 destek sayfasına ayarlanması).** ](https://support.google.com/cloud/answer/6158849?hl=en)

### <a name="application-level"></a>Uygulama düzeyi

Diğer düzeylerden biri ihtiyacınız olan yetkilendirmeyi sağlamıyorsa veya platform veya kimlik sağlayıcınız desteklenmiyorsa, [kullanıcı taleplerine](#access-user-claims)göre kullanıcıları yetkilendirmek için özel kod yazmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Kullanıcıların uçlardan uca (Windows)](app-service-web-tutorial-auth-aad.md)
> Öğreticisini doğrulamak[ve yetkilendirmek: Kullanıcıların uçlardan uca (Linux) kimlik doğrulaması ve yetkilendirmesi](containers/tutorial-auth-aad.md)
