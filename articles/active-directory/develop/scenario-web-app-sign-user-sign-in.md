---
title: Kullanıcıları işaretleyen bir web uygulaması yazın - Microsoft kimlik platformu | Azure
description: Kullanıcıları işaretleyen/giden bir web uygulaması oluşturmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 317ca55adb9f680dc93343a185395abad08889da
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881324"
---
# <a name="web-app-that-signs-in-users-sign-in-and-sign-out"></a>Kullanıcılarda oturum açan web uygulaması: Oturum açma ve oturum açma

Kullanıcılarda oturum açan web uygulamanızın koduna nasıl oturum açılabildiğini öğrenin. Sonra, onları oturum alalım öğrenin.

## <a name="sign-in"></a>Oturum açma

Oturum açma iki bölümden oluşur:

- HTML sayfasındaoturum açma düğmesi
- Denetleyicideki kod arkasında oturum açma eylemi

### <a name="sign-in-button"></a>Oturum açma düğmesi

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core'da `Views\Shared\_LoginPartial.cshtml`oturum açma düğmesi . Yalnızca doğrulanmış hesap olmadığında görüntülenir. Diğer bir deyişle, kullanıcı henüz oturum açmış veya oturum açmış değil görüntülenir.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

MVC'ASP.NET oturum açma düğmesi `Views\Shared\_LoginPartial.cshtml`. Yalnızca doğrulanmış bir hesap olduğunda görüntülenir. Diğer bir deyişle, kullanıcı daha önce oturum açmıştında görüntülenir.

```html
@if (Request.IsAuthenticated)
{
 // Code omitted code for clarity
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="java"></a>[Java](#tab/java)

Java quickstart'ımızda oturum açma düğmesi [main/resources/templates/index.html](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/resources/templates/index.html) dosyasında yer alır.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>HomePage</title>
</head>
<body>
<h3>Home Page</h3>

<form action="/msal4jsample/secure/aad">
    <input type="submit" value="Login">
</form>

</body>
</html>
```

# <a name="python"></a>[Python](#tab/python)

Python hızlı başlatmada oturum açma düğmesi yoktur. Kod arkası, kullanıcıyı web uygulamasının köküne ulaştığında otomatik olarak oturum açmaya teşvik eder. Bkz. [app.py#L14-L18](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app.py#L14-L18).

```Python
@app.route("/")
def index():
    if not session.get("user"):
        return redirect(url_for("login"))
    return render_template('index.html', user=session["user"])
```

---

### <a name="signin-action-of-the-controller"></a>`SignIn`denetleyicinin eylemi

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET, web uygulamasında **Oturum Açma** düğmesini seçmek `SignIn` `AccountController` denetleyicideki eylemi tetikler. ASP.NET çekirdek şablonlarının önceki `Account` sürümlerinde, denetleyici web uygulamasına katıştı. Denetleyici artık ASP.NET Çekirdek çerçevesinin bir parçası olduğu için artık durum böyle değil.

Kodu `AccountController` [AccountController.cs'daki](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs)ASP.NET Core deposundan edinilebilir. Hesap denetimi, Microsoft kimlik platformu bitiş noktasına yönlendirerek kullanıcıya meydan okur. Ayrıntılar için, ASP.NET Core'un bir parçası olarak sağlanan [SignIn](https://github.com/aspnet/AspNetCore/blob/f3e6b74623d42d5164fd5f97a288792c8ad877b6/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs#L23-L31) yöntemine bakın.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET, çıkış bir denetleyici (örneğin, `SignOut()` [AccountController.cs#L16-L23)](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L16-L23)üzerinde yöntemden tetiklenir. Bu yöntem ASP.NET çerçevenin bir parçası değildir (Core'ASP.NET ne olduğunun aksine). Bir yeniden yönlendirme URI önerdikten sonra bir OpenID oturum açma sorunu gönderir.

```csharp
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

# <a name="java"></a>[Java](#tab/java)

Java'da oturum açma, Microsoft kimlik platformu `logout` bitiş noktasını doğrudan `post_logout_redirect_uri` arayarak ve değeri sağlayarak gerçekleştirilir. Ayrıntılar için [AuthPageController.java#L30-L48](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L30-L48)sayfasına bakın.

```Java
@Controller
public class AuthPageController {

    @Autowired
    AuthHelper authHelper;

    @RequestMapping("/msal4jsample")
    public String homepage(){
        return "index";
    }

    @RequestMapping("/msal4jsample/secure/aad")
    public ModelAndView securePage(HttpServletRequest httpRequest) throws ParseException {
        ModelAndView mav = new ModelAndView("auth_page");

        setAccountInfo(mav, httpRequest);

        return mav;
    }

    // More code omitted for simplicity
```

# <a name="python"></a>[Python](#tab/python)

Diğer platformların aksine, MSAL Python kullanıcının oturum açma sayfasından oturum açmasına izin vermekle ilgilenir. Bkz. [app.py#L20-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L20-L28).

```Python
@app.route("/login")
def login():
    session["state"] = str(uuid.uuid4())
    auth_url = _build_msal_app().get_authorization_request_url(
        app_config.SCOPE,  # Technically we can use an empty list [] to just sign in
                           # Here we choose to also collect user consent up front
        state=session["state"],
        redirect_uri=url_for("authorized", _external=True))
    return "<a href='%s'>Login with Microsoft Identity</a>" % auth_url
```

Yöntem `_build_msal_app()` [app.py#L81-L88'de](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L81-L88) aşağıdaki gibi tanımlanmıştır:

```Python
def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)

def _get_token_from_cache(scope=None):
    cache = _load_cache()  # This web app maintains one cache per session
    cca = _build_msal_app(cache)
    accounts = cca.get_accounts()
    if accounts:  # So all accounts belong to the current signed-in user
        result = cca.acquire_token_silent(scope, account=accounts[0])
        _save_cache(cache)
        return result

```

---

Kullanıcı uygulamanızda oturum aştıktan sonra, oturumunuzu oturum açmalarını sağlamak istersiniz.

## <a name="sign-out"></a>Oturumu kapatma

Bir web uygulamasından oturum açma, oturum açmış hesapla ilgili bilgileri web uygulamasının durumundan kaldırmaktan daha fazlasını içerir.
Web uygulaması ayrıca, oturum u imzalamak için `logout` kullanıcıyı Microsoft kimlik platformu bitiş noktasına yönlendirmelidir. 

Web uygulamanız kullanıcıyı `logout` bitiş noktasına yönlendirdiğinde, bu uç nokta kullanıcının oturumunu tarayıcıdan temizler. Uygulamanız `logout` bitiş noktasına gitmediyse, kullanıcı kimlik bilgilerini tekrar girmeden uygulamanızın kimliğini yeniden doğrular. Bunun nedeni, Microsoft kimlik platformu bitiş noktası ile geçerli bir tek oturum oturumu olacak olmasıdır.

Daha fazla bilgi edinmek için Microsoft kimlik platformunda oturum [açma isteği gönder](v2-protocols-oidc.md#send-a-sign-out-request) [bölümüne ve OpenID Connect iletişim kuralı belgelerine](v2-protocols-oidc.md) bakın.

### <a name="application-registration"></a>Uygulama kaydı

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Başvuru kaydı sırasında, bir post-logout URI kaydedin. Öğreticimizde, Kimlik `https://localhost:44321/signout-oidc` **Doğrulama** sayfasındagelişmiş **ayarlar** bölümünün **Giriş URL** alanına kaydoldunuz. Ayrıntılar için [webApp uygulamasını kaydedin.](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Başvuru kaydı sırasında, bir post-logout URI kaydedin. Öğreticimizde, Kimlik `https://localhost:44308/Account/EndSession` **Doğrulama** sayfasındagelişmiş **ayarlar** bölümünün **Giriş URL** alanına kaydoldunuz. Ayrıntılar için [webApp uygulamasını kaydedin.](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet)

# <a name="java"></a>[Java](#tab/java)

Başvuru kaydı sırasında, bir post-logout URI kaydedin. Öğreticimizde, Kimlik `http://localhost:8080/msal4jsample/sign_out` **Doğrulama** sayfasındagelişmiş **ayarlar** bölümünün **Giriş URL** alanına kaydoldunuz.

# <a name="python"></a>[Python](#tab/python)

Başvuru kaydı sırasında, ek bir giriş URL'si kaydetmeniz gerekmez. Uygulama ana URL'sine geri çağrılacaktır.

---

### <a name="sign-out-button"></a>Oturum açma düğmesi

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core'da, `Views\Shared\_LoginPartial.cshtml`oturum açma düğmesi . Yalnızca doğrulanmış bir hesap olduğunda görüntülenir. Diğer bir deyişle, kullanıcı daha önce oturum açmıştında görüntülenir.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

MVC'ASP.NET oturum açma düğmesi `Views\Shared\_LoginPartial.cshtml`. Yalnızca doğrulanmış bir hesap olduğunda görüntülenir. Diğer bir deyişle, kullanıcı daha önce oturum açmıştında görüntülenir.

```html
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

# <a name="java"></a>[Java](#tab/java)

Java quickstart'ımızda, oturum açma düğmesi main/resources/templates/auth_page.html dosyasında yer alır.

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="python"></a>[Python](#tab/python)

Python quickstart'ta, oturum açma düğmesi [şablonlar/index.html#L10](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/templates/index.html#L10) dosyasında bulunur.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Microsoft Identity Python Web App</h1>
    Welcome {{ user.get("name") }}!
    <li><a href='/graphcall'>Call Microsoft Graph API</a></li>
    <li><a href="/logout">Logout</a></li>
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>`SignOut`denetleyicinin eylemi

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET, web uygulamasında **Oturum Açma** düğmesini seçmek `SignOut` `AccountController` denetleyicideki eylemi tetikler. ASP.NET Core şablonlarının önceki `Account` sürümlerinde, denetleyici web uygulamasına katıştı. Denetleyici artık ASP.NET Çekirdek çerçevesinin bir parçası olduğu için artık durum böyle değil.

Kodu `AccountController` [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs)ASP.NET çekirdek deposundan kullanılabilir. Hesap denetimi:

- Bir OpenID yeniden yönlendirmesi URI'yi ayarlar, `/Account/SignedOut` böylece Azure AD oturum açma tamamlandıktan sonra denetleyici geri çağrılır.
- OpenID Connect ara yazılımının Microsoft kimlik `Signout()`platformu `logout` bitiş noktasına başvurmasına olanak tanıyan çağrılar. Bitiş noktası sonra:

  - Oturum çerezini tarayıcıdan temizler.
  - Oturum açma URL'sini geri çağırır. Varsayılan olarak, oturum açma URL'si oturum açmış görünüm sayfasını [Görüntüler İmzaÇıktı.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml). Bu sayfa da ASP.NET Core bir parçası olarak sağlanmaktadır.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET, çıkış bir denetleyici (örneğin, `SignOut()` [AccountController.cs#L25-L31)](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Controllers/AccountController.cs#L25-L31)üzerinde yöntemden tetiklenir. Bu yöntem, ASP.NET Core'da olanların aksine, ASP.NET çerçevesinin bir parçası değildir. Bu:

- OpenID oturum açma mücadelesi gönderir.
- Önbelleği temizler.
- İstediği sayfaya yönlendirir.

```csharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 Response.Redirect("/");
}
```

# <a name="java"></a>[Java](#tab/java)

Java'da oturum açma, Microsoft kimlik platformu `logout` bitiş noktasını doğrudan `post_logout_redirect_uri` arayarak ve değeri sağlayarak gerçekleştirilir. Ayrıntılar için [AuthPageController.java#L50-L60 sayfasına](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L50-L60)bakın.

```Java
@RequestMapping("/msal4jsample/sign_out")
    public void signOut(HttpServletRequest httpRequest, HttpServletResponse response) throws IOException {

        httpRequest.getSession().invalidate();

        String endSessionEndpoint = "https://login.microsoftonline.com/common/oauth2/v2.0/logout";

        String redirectUrl = "http://localhost:8080/msal4jsample/";
        response.sendRedirect(endSessionEndpoint + "?post_logout_redirect_uri=" +
                URLEncoder.encode(redirectUrl, "UTF-8"));
    }
```

# <a name="python"></a>[Python](#tab/python)

Kullanıcıyı işaretleyen kod [app.py#L46-L52'dedir.](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L47-L48)

```Python
@app.route("/logout")
def logout():
    session.clear()  # Wipe out the user and the token cache from the session
    return redirect(  # Also need to log out from the Microsoft Identity platform
        "https://login.microsoftonline.com/common/oauth2/v2.0/logout"
        "?post_logout_redirect_uri=" + url_for("index", _external=True))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Aramayı `logout` bitiş noktasına kadar durdurma

Oturum açma sonrası URI, uygulamaların genel oturum açmaya katılmasını sağlar.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core OpenID Connect ara yazılımı, uygulamanızın Microsoft kimlik `logout` platformu bitiş noktasına yapılan çağrıyı, `OnRedirectToIdentityProviderForSignOut`openid connect adlı bir olay sağlayarak engellemesini sağlar. Bu etkinliğe nasıl abone olunacağınıöğrenmek için (belirteç önbelleğini temizlemek için) [bkz.](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156)

```csharp
    // Handling the global sign-out
    options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
    {
        // Forget about the signed-in user
    };
```

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET oturum çerezini temizleyerek oturumu yürütmek için ara yazılımı devresiniz:

```csharp
public class AccountController : Controller
{
 ...
 public void EndSession()
 {
  Request.GetOwinContext().Authentication.SignOut();
  Request.GetOwinContext().Authentication.SignOut(Microsoft.AspNet.Identity.DefaultAuthenticationTypes.ApplicationCookie);
  this.HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
 }
}
```

# <a name="java"></a>[Java](#tab/java)

Java quickstart, post-logout yönlendirme URI sadece index.html sayfasını görüntüler.

# <a name="python"></a>[Python](#tab/python)

Python quickstart'ında, giriş sonrası yeniden yönlendirme URI sadece index.html sayfasını görüntüler.

---

## <a name="protocol"></a>Protokol

Oturum açma hakkında daha fazla bilgi edinmek istiyorsanız, [Open ID Connect'te](./v2-protocols-oidc.md)bulunan protokol belgelerini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşıma](scenario-web-app-sign-user-production.md)
