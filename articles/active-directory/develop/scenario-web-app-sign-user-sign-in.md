---
title: Kullanıcılara oturum açan Web uygulaması (oturum açma)-Microsoft Identity platform
description: Kullanıcıları oturum açan bir Web uygulaması oluşturmayı öğrenin (oturum açın)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ed61a09ffc76b4813dcb97330d3a1a436aa16eb
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086468"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Kullanıcılar oturum açtığında oturum açan Web uygulaması

Kullanıcıları oturum açan Web uygulamanız için koda oturum açma eklemeyi öğrenin.

## <a name="sign-in"></a>Oturum açma

Önceki makale [uygulamasının kod yapılandırmasında](scenario-web-app-sign-user-app-configuration.md) gözden geçirdiğimiz kodun hepsi, oturum açma uygulamanız için gereklidir.
Kullanıcı uygulamanıza oturum açtıktan sonra muhtemelen oturumu kapatmalarını sağlamak isteyeceksiniz. ASP.NET çekirdek, oturumunuzu sizin için gerçekleştirir.

## <a name="what-sign-out-involves"></a>Oturum kapatma neleri içerir?

Bir Web uygulamasından oturum açmak, Web uygulamasının durumundan oturum açmış hesap hakkındaki bilgileri kaldırmasından daha fazla.
Web uygulaması oturumu kapatmak için kullanıcıyı Microsoft Identity Platform `logout` uç noktasına yönlendirmelidir. Web uygulamanız kullanıcıyı `logout` uç noktaya yönlendirirse, bu uç nokta kullanıcının oturumunu tarayıcıdan temizler. Uygulamanız `logout` uç noktaya gitmediyse, Kullanıcı Microsoft Identity platform uç noktası ile geçerli bir çoklu oturum açma oturumuna sahip olduklarından, kimlik bilgilerini tekrar girmeden uygulamanızı yeniden doğrulayacak.

Daha fazla bilgi edinmek için [Microsoft Identity platform ve OpenID Connect Protocol](v2-protocols-oidc.md) kavramsal belgelerindeki [oturum açma isteği gönderme](v2-protocols-oidc.md#send-a-sign-out-request) bölümüne bakın.

### <a name="application-registration"></a>Uygulama kaydı

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Uygulama kaydı sırasında, bir **Post Logout URI 'si**kaydettiniz. Öğreticimizde, **kimlik doğrulama** sayfasındaki `https://localhost:44321/signout-oidc` **Gelişmiş ayarlar** bölümünün **oturum kapatma URL 'si** alanına kaydolduysanız. Ayrıntılar için bkz [. WebApp uygulamasını kaydetme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

Uygulama kaydı sırasında, bir **Post Logout URI 'si**kaydettiniz. Öğreticimizde, **kimlik doğrulama** sayfasındaki `https://localhost:44308/Account/EndSession` **Gelişmiş ayarlar** bölümünün **oturum kapatma URL 'si** alanına kaydolduysanız. Ayrıntılar için bkz [. WebApp uygulamasını kaydetme](https://github.com/Azure-Samples/active-directory-dotnet-web-single-sign-out#register-the-service-app-webapp-distributedsignout-dotnet)

# <a name="javatabjava"></a>[Java](#tab/java)

Uygulama kaydı sırasında, bir **Post Logout URI 'si**kaydetmelisiniz. Öğreticimizde, **kimlik doğrulama** sayfasındaki `http://localhost:8080/msal4jsample/` **Gelişmiş ayarlar** bölümünün **oturum kapatma URL 'si** alanına kaydolduysanız.

# <a name="pythontabpython"></a>[Python](#tab/python)

Uygulama kaydı sırasında bir oturum kapatma URL 'SI kaydetmeniz gerekmez. Örnek, genel oturum kapatma uygulamaz

---

### <a name="sign-out-button"></a>Oturum kapatma düğmesi

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core ' de, oturum kapatma düğmesi içinde `Views\Shared\_LoginPartial.cshtml` gösterilir ve yalnızca kimliği doğrulanmış bir hesap (Kullanıcı daha önce oturum açtığında) görüntülenir.

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

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET MVC 'de, oturum kapatma düğmesi içinde `Views\Shared\_LoginPartial.cshtml` gösterilir ve yalnızca kimliği doğrulanmış bir hesap olduğunda (Kullanıcı daha önce oturum açtığında) görüntülenir.

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

# <a name="javatabjava"></a>[Java](#tab/java)

Java hızlı başlangıç bölümünde, oturum kapatma düğmesi main/resources/templates/auth_page.html dosyasında bulunur

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>

<form action="/msal4jsample/sign_out">
    <input type="submit" value="Sign out">
</form>
...
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Python hızlı başlangıçta, oturum kapatma düğmesi [Templates/Display.html](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e1199b4c3cdcb637cf0d8306832efbd85492e123/templates/display.html#L18-L20) dosyasında bulunur

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Acquire Token Result </title>
</head>
<body>
    {% if cond  %}
        <p1><b>Your information</b> </p1>
        <table>
        {% for key, value in auth_result.items() %}
           <tr>
                <th> {{ key }} </th>
                <td> {{ value }} </td>
           </tr>
        {% endfor %}
        </table>
        <form action="/logout" >
            <input type="submit" value=" Logout"/>
        </form>
    {% else %}
        <p1><b> {{auth_result}} </b> </p1>
        <form action="/authenticate" >
            <input type="submit" value=" Sign-in"/>
        </form>
    {% endif %}
</body>
</html>
```

---

### <a name="signout-action-of-the-controller"></a>`Signout()`denetleyicinin eylemi

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.net ' de, Web uygulamasındaki **oturumu** Kapat düğmesine basıldığında `SignOut` `AccountController` denetleyicinin eylemi tetiklenir. ASP.NET Core şablonlarının önceki sürümlerinde, `Account` denetleyici Web uygulamasıyla katıştırılmıştır, ancak artık ASP.NET Core çerçevesinin bir parçası olduğu için bu durum artık böyle değildir.

İçin `AccountController` kodu, [accountcontroller.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs)adresinden ASP.NET Core deposundan kullanılabilir. Hesap denetimi:

- Azure AD oturum kapatma işlemini gerçekleştirdiyse `/Account/SignedOut` denetleyicinin geri çağrılması için bir OpenID yeniden yönlendirme URI 'si olarak ayarlar
- Openıdconnect ara yazılımların Microsoft Identity Platform `Signout()` `logout` uç noktasıyla iletişim kurmasına izin veren çağrılar:

  - Oturum tanımlama bilgisini tarayıcıdan temizler ve
  - son olarak, oturum **kapatma URL**'sini geri çağırır. Bu, varsayılan olarak, ASP.NET Core bir parçası olarak da sağlanmış olan imzalanmış görünüm sayfasını [signeout. html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) ' i görüntüler.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.net ' de, oturum kapatma bir denetleyicideki `SignOut()` yönteminden tetiklenir (örneğin accountcontroller). Bu yöntem, ASP.NET çerçevesinin bir parçası değildir (ASP.NET Core olduğu gibi). Kullanmaz `async`ve bu nedenle şu şekilde olur:

- bir OpenID oturum kapatma sınaması gönderir
- önbelleği temizler
- istediği sayfaya yeniden yönlendirir

```CSharp
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

# <a name="javatabjava"></a>[Java](#tab/java)

Java 'da, oturum kapatma doğrudan Microsoft Identity platform Logout uç noktası çağırarak ve post_logout_redirect_uri sağlayarak işlenir.

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

# <a name="pythontabpython"></a>[Python](#tab/python)

```Python
@app.route("/logout")
def logout():
    return flask.redirect(flask.url_for('index'))
```

---

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>`logout` Uç nokta çağrısını kesintiye uğratan

Logout Post sonrası URI, uygulamaların genel oturum açma 'ya katılmasına olanak sağlar.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core openıdconnect ara yazılımı, uygulamanızın adlı `logout` `OnRedirectToIdentityProviderForSignOut`bir openıdconnect olayı sağlayarak Microsoft Identity platform uç noktası çağrısını ele almasını sağlar. Bu olaya abone olunacak bir örnek için bkz. [Microsoft. Identity. Web/WebAppServiceCollectionExtensions. cs # L151-L156](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L156) (belirteç önbelleğini temizlemek için)

```CSharp
               // Handling the global sign-out
                options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
                {
                    // Forget about the signed-in user
                };
```

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET ' de, oturum açma bilgilerini temizleyerek oturumu kapatma işlemini yürütmek için ara yazılım temsilcisine temsilci seçin:

```CSharp
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

# <a name="javatabjava"></a>[Java](#tab/java)

Java hızlı başlangıç bölümünde, oturum açma sonrası yeniden yönlendirme URI 'SI yalnızca index. html sayfasını görüntüler 

# <a name="pythontabpython"></a>[Python](#tab/python)

Python hızlı başlangıçta, oturum açma sonrası yeniden yönlendirme URI 'SI yalnızca index. html sayfasını görüntüler.

---

## <a name="protocol"></a>Protocol

Oturum kapatma hakkında daha fazla bilgi edinmek istiyorsanız, [Açık kimlik Connect](./v2-protocols-oidc.md)'ten erişilebilen protokol belgelerini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-web-app-sign-user-production.md)
