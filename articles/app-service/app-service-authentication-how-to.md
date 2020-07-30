---
title: AuthN/AuthZ gelişmiş kullanımı
description: Farklı senaryolar için App Service kimlik doğrulaması ve yetkilendirme özelliğini özelleştirmeyi ve Kullanıcı taleplerini ve farklı belirteçleri almayı öğrenin.
ms.topic: article
ms.date: 07/08/2020
ms.custom: seodec18
ms.openlocfilehash: 747729b7cbb3dcce72eb36704b5965e8427b59e1
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87424265"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Azure App Service 'da gelişmiş kimlik doğrulama ve yetkilendirme kullanımı

Bu makalede, [App Service ' de yerleşik kimlik doğrulama ve yetkilendirmeyi](overview-authentication-authorization.md)nasıl özelleştireceğinizi ve uygulamanızdan kimlik yönetimi açıklanmaktadır. 

Hızlıca kullanmaya başlamak için aşağıdaki öğreticilerden birine bakın:

* [Öğretici: kullanıcıların Azure App Service (Windows) ile uçtan uca kimlik doğrulama ve yetkilendirme](app-service-web-tutorial-auth-aad.md)
* [Öğretici: Linux için Azure App Service Kullanıcı tarafından uçtan uca kimlik doğrulama ve yetkilendirme](containers/tutorial-auth-aad.md)
* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-aad.md)
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-facebook.md)
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-google.md)
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-microsoft.md)
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma](configure-authentication-provider-twitter.md)
* [Uygulamanızı bir OpenID Connect sağlayıcısı kullanarak oturum açmak üzere yapılandırma (Önizleme)](configure-authentication-provider-openid-connect.md)

## <a name="use-multiple-sign-in-providers"></a>Çoklu oturum açma sağlayıcılarını kullanma

Portal Yapılandırması, kullanıcılarınıza birden çok oturum açma sağlayıcısı (Facebook ve Twitter gibi) sunmak için bir anahtar açma yöntemi sunmaz. Ancak, işlevselliği uygulamanıza eklemek zor değildir. Adımlar aşağıdaki şekilde özetlenmiştir:

İlk olarak, Azure portal **kimlik doğrulama/yetkilendirme** sayfasında, etkinleştirmek istediğiniz her bir kimlik sağlayıcısını yapılandırın.

**İsteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem Için** **anonim isteklere izin ver (eylem yok)** seçeneğini belirleyin.

Oturum açma sayfasında veya gezinti çubuğunda veya uygulamanızın herhangi bir yerinde, etkinleştirdiğiniz her bir sağlayıcının () bir oturum açma bağlantısını ekleyin `/.auth/login/<provider>` . Örneğin:

```html
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Kullanıcı bağlantılardan birine tıkladığında, kullanıcının oturum açması için ilgili oturum açma sayfası açılır.

Kullanıcı oturum açma sonrası eklentisini özel bir URL 'ye yönlendirmek için `post_login_redirect_url` sorgu dizesi parametresini kullanın (kimlik sağlayıcısı yapılandırmanızda yeniden yönlendirme URI 'si ile karıştırılmamalıdır). Örneğin, oturum açtıktan sonra kullanıcıya gitmek için `/Home/Index` AŞAĞıDAKI HTML kodunu kullanın:

```html
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Sağlayıcılardan belirteçleri doğrula

İstemci ile yönlendirilen bir oturum açma bölümünde, uygulama kullanıcıdan sağlayıcıya el ile oturum açar ve ardından kimlik doğrulama belirtecini doğrulama için App Service (bkz. [kimlik doğrulama akışı](overview-authentication-authorization.md#authentication-flow)) gönderir. Bu doğrulamanın kendisi, istenen uygulama kaynaklarına erişim hakkı vermez, ancak başarılı bir doğrulama size uygulama kaynaklarına erişmek için kullanabileceğiniz bir oturum belirteci verecektir. 

Sağlayıcı belirtecini doğrulamak için App Service uygulamasının öncelikle istenen sağlayıcıyla yapılandırılması gerekir. Çalışma zamanında, sağlayıcınızdan kimlik doğrulama belirtecini aldıktan sonra, `/.auth/login/<provider>` doğrulama için belirteci gönderin. Örneğin: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

Belirteç biçimi sağlayıcıya göre biraz farklılık gösterir. Ayrıntılar için aşağıdaki tabloya bakın:

| Sağlayıcı değeri | İstek gövdesinde gerekli | Yorumlar |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | `expires_in`Özelliği isteğe bağlıdır. <br/>Canlı hizmetlerden belirteç istenirken, her zaman `wl.basic` kapsam isteyin. |
| `google` | `{"id_token":"<id_token>"}` | `authorization_code`Özelliği isteğe bağlıdır. Belirtildiğinde, isteğe bağlı olarak özelliği de kullanılabilir `redirect_uri` . |
| `facebook`| `{"access_token":"<user_access_token>"}` | Facebook 'tan geçerli bir [Kullanıcı erişim belirteci](https://developers.facebook.com/docs/facebook-login/access-tokens) kullanın. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Sağlayıcı belirteci başarıyla doğrulandıktan sonra, API, `authenticationToken` yanıt gövdesinde, oturum belirteciniz olan bir ile birlikte döndürür. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Bu oturum belirtecine sahip olduğunuzda, `X-ZUMO-AUTH` http isteklerinize üst bilgi ekleyerek korumalı uygulama kaynaklarına erişebilirsiniz. Örneğin: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Oturum kapatma

Kullanıcılar `GET` , uygulamanın uç noktasına bir istek göndererek oturumu kapatma işlemini başlatabilir `/.auth/logout` . `GET`İstek şunları yapar:

- Geçerli oturumdan kimlik doğrulama tanımlama bilgilerini temizler.
- Geçerli kullanıcının belirteçlerini belirteç deposundan siler.
- Azure Active Directory ve Google için, kimlik sağlayıcısında sunucu tarafında oturum kapatma gerçekleştirir.

Web sayfasında basit bir oturum kapatma bağlantısı şöyle olabilir:

```html
<a href="/.auth/logout">Sign out</a>
```

Varsayılan olarak, başarılı bir oturum kapatma istemciyi URL 'ye yeniden yönlendirir `/.auth/logout/done` . Sorgu parametresini ekleyerek, oturum kapatma sonrası yeniden yönlendirme sayfasını değiştirebilirsiniz `post_logout_redirect_uri` . Örneğin:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Değerini [kodlamanız](https://wikipedia.org/wiki/Percent-encoding) önerilir `post_logout_redirect_uri` .

Tam nitelikli URL 'Ler kullanılırken, URL aynı etki alanında barındırılıyor ya da uygulamanız için izin verilen bir dış yeniden yönlendirme URL 'SI olarak yapılandırılmış olmalıdır. Aşağıdaki örnekte, `https://myexternalurl.com` aynı etki alanında barındırılmayan öğesine yeniden yönlendirmek için:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

[Azure Cloud Shell](../cloud-shell/quickstart.md)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>URL parçalarını koruma

Kullanıcılar uygulamanızda oturum açtıktan sonra, genellikle gibi aynı sayfanın aynı bölümüne yönlendirilmek isterler `/wiki/Main_Page#SectionZ` . Bununla birlikte, [URL parçaları](https://wikipedia.org/wiki/Fragment_identifier) (örneğin, `#SectionZ` ) sunucuya hiçbir şekilde gönderilmediğinden, OAuth oturum açma tamamlandıktan sonra varsayılan olarak korunmaz ve uygulamanıza yeniden yönlendirilir. Kullanıcılar daha sonra istediğiniz yere gitmek gerektiğinde bir daha iyi deneyim yaşar. Bu sınırlama, tüm sunucu tarafı kimlik doğrulama çözümleri için geçerlidir.

App Service kimlik doğrulaması ' nda, OAuth oturum açma genelinde URL parçalarını koruyabilirsiniz. Bunu yapmak için olarak adlandırılan bir uygulama ayarı belirleyin `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` `true` . Bunu [Azure Portal](https://portal.azure.com)yapabilir veya [Azure Cloud Shell](../cloud-shell/quickstart.md)yalnızca aşağıdaki komutu çalıştırabilirsiniz:

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Kullanıcı taleplerine erişin

App Service, özel üstbilgiler kullanarak Kullanıcı taleplerini uygulamanıza geçirir. Dış isteklerin bu üst bilgileri ayarlama izni yoktur, bu nedenle yalnızca App Service tarafından ayarlandıysa mevcut olmaları gerekir. Bazı örnek üstbilgileri şunlardır:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ıD

Herhangi bir dilde veya çerçevede yazılan kod, bu üst bilgilerden ihtiyaç duymakta olan bilgileri alabilir. ASP.NET 4,6 uygulamaları için, **ClaimsPrincipal** otomatik olarak uygun değerlerle ayarlanır. Ancak ASP.NET Core, App Service Kullanıcı taleplerini tümleştiren bir kimlik doğrulama ara yazılımı sağlamaz. Geçici bir çözüm için bkz. [Maximerouiller. Azure. AppService. EasyAuth](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth).

Uygulamanız, çağırarak kimliği doğrulanmış kullanıcı hakkında ek ayrıntılar de alabilir `/.auth/me` . Mobile Apps Server SDK 'Ları, bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için bkz. [azure Mobile Apps Node.js SDK 'sını kullanma](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)ve [Azure Mobile Apps için .net arka uç sunucu SDK 'sı ile çalışma](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Uygulama kodundaki belirteçleri al

Sunucu kodunuzda sağlayıcıya özgü belirteçler istek üstbilgisine eklenir, bu sayede kolayca erişebilirsiniz. Aşağıdaki tabloda olası belirteç üstbilgisi adları gösterilmektedir:

| Sağlayıcı | Üst bilgi adları |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook belirteci | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft Hesabı | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

İstemci kodınızdan (bir mobil uygulama veya tarayıcı içi JavaScript gibi), için bir HTTP `GET` isteği gönderin `/.auth/me` . Döndürülen JSON, sağlayıcıya özgü belirteçlere sahiptir.

> [!NOTE]
> Erişim belirteçleri sağlayıcı kaynaklarına erişmek için kullanılır, bu nedenle yalnızca sağlayıcınızı bir istemci gizli anahtarı ile yapılandırırsanız vardır. Yenileme belirteçlerinin nasıl alınacağını görmek için bkz. erişim belirteçlerini yenileme.

## <a name="refresh-identity-provider-tokens"></a>Kimlik sağlayıcısı belirteçlerini Yenile

Sağlayıcınızın erişim belirtecinin ( [oturum belirteci](#extend-session-token-expiration-grace-period)değil) süresi dolmuşsa, bu belirteci yeniden kullanmadan önce kullanıcıyı yeniden kimlik doğrulaması yapmanız gerekir. `GET`Uygulamanızın uç noktasına bir çağrı yaparak belirteç süre sonundan kaçınabilirsiniz `/.auth/refresh` . Çağrıldığında, App Service kimliği doğrulanmış kullanıcı için belirteç deposundaki erişim belirteçlerini otomatik olarak yeniler. Uygulama kodunuzun belirteçleri için sonraki istekleri yenilenen belirteçleri alır. Ancak, belirteç yenilemenin çalışması için, belirteç deposu sağlayıcınız için [yenileme belirteçleri](https://auth0.com/learn/refresh-tokens/) içermelidir. Yenileme belirteçlerini almanın yöntemi her sağlayıcı tarafından belgelenmiştir, ancak aşağıdaki liste kısa bir özettir:

- **Google**: `access_type=offline` API çağrın sorgu dizesi parametresini ekleyin `/.auth/login/google` . Mobile Apps SDK kullanıyorsanız, `LogicAsync` aşırı yüklerden birine parametreyi ekleyebilirsiniz (bkz. [Google Refresh belirteçleri](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: yenileme belirteçleri sağlamıyor. Uzun süreli belirteçlerin süresi 60 gün içinde doluyor (bkz. [Facebook süre sonu ve erişim belirteçleri uzantısı](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: erişim belirteçlerinin süre sonu yok (bkz. [Twitter OAuth SSS](https://developer.twitter.com/en/docs/basics/authentication/FAQ)).
- **Microsoft hesabı**: [Microsoft hesabı kimlik doğrulaması ayarlarını yapılandırırken](configure-authentication-provider-microsoft.md) `wl.offline_access` kapsamı seçin.
- **Azure Active Directory**: içinde [https://resources.azure.com](https://resources.azure.com) , aşağıdaki adımları uygulayın:
    1. Sayfanın üst kısmında **oku/yaz**' ı seçin.
    2. Sol tarayıcıda, **abonelikler** > * *_ \<subscription\_name_** > **ResourceGroups** > * *_ \<resource\_group\_name> _* * > **sağlayıcıları**  >  **Microsoft. Web**  >  **Sites** > * *_ \<app\_name> _ * * > **config**  >  **authsettings öğesine tıklayın**' e gidin. 
    3. **Düzenle**’ye tıklayın.
    4. Aşağıdaki özelliği değiştirin. _\<app\_id>_ Erişmek istediğiniz hizmetin Azure Active Directory uygulama kimliğiyle değiştirin.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. **Yerleştir**' e tıklayın. 

Sağlayıcınız yapılandırıldıktan sonra, belirteç deposundaki [erişim belirtecinin yenileme belirtecini ve sona erme zamanını bulabilirsiniz](#retrieve-tokens-in-app-code) . 

Erişim belirtecinizi dilediğiniz zaman yenilemek için `/.auth/refresh` herhangi bir dilde çağrı yapmanız yeterlidir. Aşağıdaki kod parçacığı bir JavaScript istemcisinden erişim belirteçlerinizi yenilemek için jQuery kullanır.

```javascript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Bir Kullanıcı uygulamanıza verilen izinleri iptal ederseniz, çağrısı `/.auth/me` bir yanıt vererek başarısız olabilir `403 Forbidden` . Hataları tanılamak için, Ayrıntılar için uygulama günlüklerinizi denetleyin.

## <a name="extend-session-token-expiration-grace-period"></a>Oturum belirteci süre sonu yetkisiz kullanım süresini uzat

Kimliği doğrulanmış oturumun süresi 8 saat sonra dolar. Kimliği doğrulanmış bir oturumun süresi dolduktan sonra, varsayılan olarak 72 saatlik bir yetkisiz kullanım süresi vardır. Bu yetkisiz kullanım süresi içinde, Kullanıcı yeniden kimlik doğrulaması yapmadan oturum belirtecini App Service yenilemeye izin verilir. Yalnızca `/.auth/refresh` oturum belirtecinizin geçersiz hale geldiği zaman çağrı yapabilir ve belirteç kullanım süresini kendiniz izlemeniz gerekmez. 72 saatlik yetkisiz kullanım süresi eşitlendikten sonra, Kullanıcı geçerli bir oturum belirteci almak için yeniden oturum açması gerekir.

Bu süre boyunca 72 saat yeterli değilse, bu süre sonu penceresini genişletebilirsiniz. Süre sonunu uzun bir süre boyunca uzatmak önemli güvenlik etkilerine (örneğin, bir kimlik doğrulama belirtecinin sızma veya çalındığı zaman) sahip olabilir. Bu nedenle, bunu varsayılan 72 saat olarak bırakmanız veya uzantı süresini en küçük değere ayarlamanız gerekir.

Varsayılan süre sonu penceresini genişletmek için [Cloud Shell](../cloud-shell/overview.md)aşağıdaki komutu çalıştırın.

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> Yetkisiz kullanım süresi, kimlik sağlayıcılarındaki belirteçleri değil, yalnızca App Service kimliği doğrulanmış oturum için geçerlidir. Süresi geçen sağlayıcı belirteçleri için yetkisiz kullanım süresi yoktur. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Oturum açma hesaplarının etki alanını sınırlayın

Hem Microsoft hesabı hem de Azure Active Directory birden çok etki alanından oturum açmanızı sağlar. Örneğin, Microsoft hesabı _Outlook.com_, _Live.com_ve _hotmail.com_ hesaplarına izin verir. Azure AD, oturum açma hesapları için herhangi bir sayıda özel etki alanı sağlar. Ancak, kullanıcılarınızı kendi markalı Azure AD oturum açma sayfanıza (örneğin,) doğrudan hızlandırmak isteyebilirsiniz `contoso.com` . Oturum açma hesaplarının etki alanı adını önermek için aşağıdaki adımları izleyin.

İçinde [https://resources.azure.com](https://resources.azure.com) , **abonelikler** > * *_ \<subscription\_name_** > **ResourceGroups** > * *_ \<resource\_group\_name> _* * > **sağlayıcıları**  >  **Microsoft. Web**  >  **Sites** > * *_ \<app\_name> _ * * > **config**  >  **authsettings öğesine tıklayın**' e gidin. 

**Düzenle**' ye tıklayın, aşağıdaki özelliği değiştirin ve ardından **Yerleştir**' e tıklayın. İstediğiniz _\<domain\_name>_ etki alanı ile değiştirdiğinizden emin olun.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```

Bu ayar, `domain_hint` sorgu dizesi parametresini, oturum açma yeniden YÖNLENDIRME URL 'sine ekler. 

> [!IMPORTANT]
> `domain_hint`Yeniden YÖNLENDIRME URL 'sini aldıktan sonra istemcinin parametresini kaldırması ve sonra farklı bir etki alanı ile oturum açması mümkündür. Bu işlev kullanışlı olsa da, bir güvenlik özelliği değildir.
>

## <a name="authorize-or-deny-users"></a>Kullanıcılara yetki verme veya reddetme

App Service en basit yetkilendirme durumu (yani, kimliği doğrulanmamış istekleri reddetme) ile ilgilenirken, uygulamanız yalnızca belirli bir kullanıcı grubuyla erişimi sınırlandırma gibi daha ayrıntılı yetkilendirme davranışı gerektirebilir. Belirli durumlarda, oturum açmış kullanıcıya izin vermek veya erişimi reddetmek için özel uygulama kodu yazmanız gerekir. Diğer durumlarda, App Service veya kimlik sağlayıcınız kod değişikliği gerektirmeden yardım edebilir.

- [Sunucu düzeyi](#server-level-windows-apps-only)
- [Kimlik sağlayıcısı düzeyi](#identity-provider-level)
- [Uygulama düzeyi](#application-level)

### <a name="server-level-windows-apps-only"></a>Sunucu düzeyi (yalnızca Windows uygulamaları)

Herhangi bir Windows uygulaması için, *Web.config* dosyasını düzenleyerek IIS Web sunucusunun yetkilendirme davranışını tanımlayabilirsiniz. Linux uygulamaları IIS kullanmaz ve *Web.config*aracılığıyla yapılandırılamaz.

1. `https://<app-name>.scm.azurewebsites.net/DebugConsole` sayfasına gidin

1. App Service dosyalarınızın tarayıcı Gezgini ' nde, *site/Wwwroot ' ya*gidin. *Web.config* yoksa, **+**  >  **yeni dosya**' yı seçerek oluşturun. 

1. *Web.config* düzenlemek için kurşun kalem ' i seçin. Aşağıdaki yapılandırma kodunu ekleyin ve **Kaydet**' e tıklayın. *Web.config* zaten varsa, `<authorization>` öğeyi içindeki her şeyi eklemeniz yeterlidir. Öğesinde izin vermek istediğiniz hesapları ekleyin `<allow>` .

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

### <a name="identity-provider-level"></a>Kimlik sağlayıcısı düzeyi

Kimlik sağlayıcısı, belirli bir anahtar yetkilendirme sağlayabilir. Örneğin:

- [Azure App Service](configure-authentication-provider-aad.md)için, [Kurumsal düzeyde ERIŞIMI](../active-directory/manage-apps/what-is-access-management.md) doğrudan Azure AD 'de yönetebilirsiniz. Yönergeler için bkz. [kullanıcının bir uygulamaya erişimini kaldırma](../active-directory/manage-apps/methods-for-removing-user-access.md).
- [Google](configure-authentication-provider-google.md)için, bir [kuruluşa](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) ait Google API projeleri yalnızca kuruluşunuzdaki kullanıcılara erişime izin verecek şekilde yapılandırılabilir (bkz. [Google 'ın **OAuth 2,0** destek sayfasını ayarlama](https://support.google.com/cloud/answer/6158849?hl=en)).

### <a name="application-level"></a>Uygulama düzeyi

Diğer düzeylerin herhangi biri ihtiyacınız olan yetkilendirmeyi sağlamıyorsa veya platformunuz veya kimlik sağlayıcınız desteklenmiyorsa, [Kullanıcı taleplerine](#access-user-claims)göre kullanıcılara yetki vermek için özel kod yazmanız gerekir.

## <a name="configure-using-a-file-preview"></a><a name="config-file"> </a>Dosya kullanarak yapılandırma (Önizleme)

Kimlik doğrulama ayarlarınız isteğe bağlı olarak, dağıtımınız tarafından sağlanmış bir dosya aracılığıyla yapılandırılabilir. Bu, App Service kimlik doğrulaması/yetkilendirme için bazı Önizleme özellikleri tarafından gerekli olabilir.

> [!IMPORTANT]
> Uygulama yükünüzü ve bu dosyanın, [yuvalarda](./deploy-staging-slots.md)olduğu gibi ortamlar arasında hareket edebilir olduğunu unutmayın. Büyük olasılıkla her bir yuvaya sabitlenmiş farklı bir uygulama kaydı yapmak isteyebilirsiniz ve bu durumlarda yapılandırma dosyasını kullanmak yerine standart yapılandırma yöntemini kullanmaya devam etmelisiniz.

### <a name="enabling-file-based-configuration"></a>Dosya tabanlı yapılandırmayı etkinleştirme

> [!CAUTION]
> Önizleme süresince dosya tabanlı yapılandırmayı etkinleştirmek, Azure portal, Azure CLı ve Azure PowerShell gibi bazı istemciler aracılığıyla uygulamanız için App Service kimlik doğrulaması/yetkilendirme özelliğinin yönetimini devre dışı bırakır.

1. Projenizin kökündeki yapılandırmanız için yeni bir JSON dosyası oluşturun (Web/işlev uygulamanızda D:\home\site\wwwroot dosyasına dağıtılır). [Dosya tabanlı yapılandırma başvurusuna](#configuration-file-reference)göre istediğiniz yapılandırmayı girin. Mevcut bir Azure Resource Manager yapılandırmasını değiştiriyorsanız, koleksiyonda yakalanan özellikleri yapılandırma dosyanıza çevirdiğinizden emin olun `authsettings` .

2. Altındaki [Azure Resource Manager](../azure-resource-manager/management/overview.md) API 'lerinde yakalanan varolan yapılandırmayı değiştirin `Microsoft.Web/sites/<siteName>/config/authsettings` . Bunu değiştirmek için [Azure Resource Manager şablonu](../azure-resource-manager/templates/overview.md) veya [Azure Kaynak Gezgini](https://resources.azure.com/)gibi bir aracı kullanabilirsiniz. Authsettings öğesine tıklayın koleksiyonu içinde üç özellik ayarlamanız gerekir (ve diğerlerini kaldırabilir):

    1.  `enabled`"True" olarak ayarlayın
    2.  `isAuthFromFile`"True" olarak ayarlayın
    3.  `authFilePath`Dosyanın adına ayarlanır (örneğin, "auth.json")

Bu yapılandırma güncelleştirmesini yaptıktan sonra, bu site için App Service kimlik doğrulaması/yetkilendirme davranışını tanımlamak üzere dosyanın içeriği kullanılacaktır. Azure Resource Manager yapılandırmaya geri dönmek isterseniz, bunu `isAuthFromFile` "false" olarak ayarlayarak yapabilirsiniz.

### <a name="configuration-file-reference"></a>Yapılandırma dosyası başvurusu

Yapılandırma dosyanızda başvurulacak tüm gizli dizi, [uygulama ayarları](./configure-common.md#configure-app-settings)olarak depolanmalıdır. Ayarları istediğiniz şekilde adlandırın. Yalnızca yapılandırma dosyasındaki başvuruların aynı anahtarları kullandığından emin olun.

Aşağıdakiler dosya içinde olası yapılandırma seçeneklerini tüketmektedir:

```json
{
    "platform": {
        "enabled": <true|false>
    },
    "globalValidation": {
        "requireAuthentication": <true|false>,
        "unauthenticatedClientAction": "RedirectToLoginPage|AllowAnonymous|Return401|Return403",
        "redirectToProvider": "<default provider alias>",
        "excludedPaths": [
            "/path1",
            "/path2"
        ]
    },
    "identityProviders": {
        "azureActiveDirectory": {
            "enabled": <true|false>,
            "registration": {
                "openIdIssuer": "<issuer url>",
                "clientId": "<app id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_AAD_SECRET",
            },
            "login": {
                "loginParameters": [
                    "paramName1=value1",
                    "paramName2=value2"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "facebook": {
            "enabled": <true|false>,
            "registration": {
                "appId": "<app id>",
                "appSecretSettingName": "APP_SETTING_CONTAINING_FACEBOOK_SECRET"
            },
            "graphApiVersion": "v3.3",
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            },
        },
        "gitHub": {
            "enabled": <true|false>,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GITHUB_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            }
        },
        "google": {
            "enabled": true,
            "registration": {
                "clientId": "<client id>",
                "clientSecretSettingName": "APP_SETTING_CONTAINING_GOOGLE_SECRET"
            },
            "login": {
                "scopes": [
                    "profile",
                    "email"
                ]
            },
            "validation": {
                "allowedAudiences": [
                    "audience1",
                    "audience2"
                ]
            }
        },
        "twitter": {
            "enabled": <true|false>,
            "registration": {
                "consumerKey": "<consumer key>",
                "consumerSecretSettingName": "APP_SETTING_CONTAINING TWITTER_CONSUMER_SECRET"
            }
        },
        "openIdConnectProviders": {
            "provider name": {
                "enabled": <true|false>,
                "registration": {
                    "clientId": "<client id>",
                    "clientCredential": {
                        "secretSettingName": "<name of app setting containing client secret>"
                    },
                    "openIdConnectConfiguration": {
                        "authorizationEndpoint": "<url specifying authorization endpoint>",
                        "tokenEndpoint": "<url specifying token endpoint>",
                        "issuer": "<url specifying issuer>",
                        "certificationUri": "<url specifying jwks endpoint>",
                        "wellKnownOpenIdConfiguration": "<url specifying .well-known/open-id-configuration endpoint - if this property is set, the other properties of this object are ignored, and authorizationEndpoint, tokenEndpoint, issuer, and certificationUri are set to the corresponding values listed at this endpoint>"
                    }
                },
                "login": {
                    "nameClaimType": "<name of claim containing name>",
                    "scope": [
                        "openid",
                        "profile",
                        "email"
                    ],
                    "loginParameterNames": [
                        "paramName1=value1",
                        "paramName2=value2"
                    ],
                }
            },
            //...
        },
        "login": {
            "routes": {
                "logoutEndpoint": "<logout endpoint>"
            },
            "tokenStore": {
                "enabled": <true|false>,
                "tokenRefreshExtensionHours": "<double>",
                "fileSystem": {
                    "directory": "<directory to store the tokens in if using a file system token store (default)>"
                },
                "azureBlobStorage": {
                    "sasUrlSettingName": "<app setting name containing the sas url for the Azure Blob Storage if opting to use that for a token store>"
                }
            },
            "preserveUrlFragmentsForLogins": <true|false>,
            "allowedExternalRedirectUrls": [
                "https://uri1.azurewebsites.net/",
                "https://uri2.azurewebsites.net/"
            ],
            "cookieExpiration": {
                "convention": "FixedTime|IdentityProviderDerived",
                "timeToExpiration": "<timespan>"
            },
            "nonce": {
                "validateNonce": <true|false>,
                "nonceExpirationInterval": "<timespan>"
            }
        },
        "httpSettings": {
            "requireHttps": <true|false>,
            "routes": {
                "apiPrefix": "<api prefix>"
            },
            "forwardProxy": {
                "convention": "NoProxy|Standard|Custom",
                "customHostHeaderName": "<host header value>",
                "customProtoHeaderName": "<proto header value>"
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: kullanıcıların kimlik doğrulaması ve yetkilendirme (Windows)](app-service-web-tutorial-auth-aad.md) 
>  [Öğretici: kullanıcıların kimlik doğrulaması ve yetkilendirme (Linux)](containers/tutorial-auth-aad.md)
