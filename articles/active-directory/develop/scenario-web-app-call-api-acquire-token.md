---
title: Web API 'Lerini çağıran bir Web uygulamasında belirteç alın | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini çağıran bir Web uygulaması için belirteç alma hakkında bilgi edinin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/25/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 4fe3744f3f8cb39a7493ce788ee9badc1b31b75e
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2020
ms.locfileid: "91396187"
---
# <a name="a-web-app-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API 'Leri çağıran bir Web uygulaması: uygulama için belirteç alma

İstemci uygulama nesneniz oluşturdunuz. Şimdi, bir Web API 'sini çağırmak için bir belirteç almak üzere kullanacaksınız. ASP.NET veya ASP.NET Core ' de, bir Web API 'SI çağırmak denetleyicide yapılır:

- Belirteç önbelleğini kullanarak Web API 'SI için bir belirteç alın. Bu belirteci almak için, MSAL `AcquireTokenSilent` yöntemini (veya Microsoft. Identity. Web 'de eşdeğerini) çağırın.
- Erişim belirtecini bir parametre olarak geçirerek korunan API 'yi çağırın.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft. Identity. Web* , Microsoft Graph veya bir aşağı akış Web API 'sine çağrı yapmak için kullanışlı hizmetler sağlayan uzantı yöntemleri ekler. Bu yöntemler, [Web API 'lerini çağıran bir Web](scenario-web-app-call-api-call-api.md)uygulamasında ayrıntılı olarak AÇıKLANMıŞTıR: API çağrısı. Bu yardımcı yöntemlerle el ile belirteç almanız gerekmez.

Ancak, bir belirteci el ile almak istiyorsanız aşağıdaki kod, bir giriş denetleyicisinde bunu yapmak için *Microsoft. Identity. Web* kullanımına ilişkin bir örnek gösterir. REST API kullanarak Microsoft Graph çağırır (Microsoft Graph SDK yerine). Aşağı akış API 'sini çağırmak için bir belirteç almak üzere, `ITokenAcquisition` hizmet programını denetleyicinizin yapıcısına (veya Blazor kullanıyorsanız sayfa yapıcısına) ekleyerek, `GetAccessTokenForUserAsync` bir Daemon senaryosunda Kullanıcı () ya da uygulamanın kendisi () için bir belirteç almanızı sağlar `GetAccessTokenForAppAsync` .

Denetleyici yöntemleri, `[Authorize]` yalnızca kimliği doğrulanmış kullanıcıların Web uygulamasını kullanmasını sağlayan bir öznitelik tarafından korunur.

```csharp
[Authorize]
public class HomeController : Controller
{
 readonly ITokenAcquisition tokenAcquisition;

 public HomeController(ITokenAcquisition tokenAcquisition)
 {
  this.tokenAcquisition = tokenAcquisition;
 }

 // Code for the controller actions (see code below)

}
```

`ITokenAcquisition`Hizmet, bağımlılık ekleme kullanılarak ASP.NET tarafından eklenir.

Aşağıda, `HomeController` Microsoft Graph çağrısı yapılacak bir belirteç alan eylemi için basitleştirilmiş kod verilmiştir:

```csharp
[AuthorizeForScopes(Scopes = new[] { "user.read" })]
public async Task<IActionResult> Profile()
{
 // Acquire the access token.
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenForUserAsync(scopes);

 // Use the access token to call a protected web API.
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

Bu senaryo için gereken kodu daha iyi anlamak için, [MS-Identity-aspnetcore-WebApp-öğreticisi](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) öğreticisinin 2. aşama ([2-1-Web uygulaması çağrı Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) adımına bakın.

`AuthorizeForScopes`Denetleyici eyleminin üstündeki öznitelik (veya Razor şablonu kullanıyorsanız Razor sayfası), Microsoft. Identity. Web tarafından sağlanır. Gerektiğinde kullanıcıdan onay istenmesini ve artımlı olarak alınmasını sağlar.

Farklı karmaşık çeşitlemeler vardır, örneğin:

- Çeşitli API 'Ler çağırma.
- Artımlı onay ve koşullu erişim işleniyor.

Bu gelişmiş adımlar [3-WebApp-Multi-API](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs) öğreticisinin Bölüm 3 ' te ele alınmıştır.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET için kod ASP.NET Core gösterilen koda benzerdir:

- Bir [Yetkilendir] özniteliğiyle korunan bir denetleyici eylemi, denetleyicinin üyesinin kiracı KIMLIĞINI ve kullanıcı KIMLIĞINI ayıklar `ClaimsPrincipal` . (ASP.NET kullanımları `HttpContext.User` .)
- Buradan, bir MSAL.NET `IConfidentialClientApplication` nesnesi oluşturur.
- Son olarak, `AcquireTokenSilent` Gizli istemci uygulamasının yöntemini çağırır.
- Etkileşim gerekliyse, Web uygulamasının kullanıcıyı sınaması (yeniden oturum açması) ve daha fazla talep istemesi gerekir.

Aşağıdaki kod parçacığı [MS-Identity-ASPNET-WebApp-openıdconnect](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect) ASP.NET MVC kod örneğindeki [HomeController. cs # L157-L192](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/257c8f96ec3ff875c351d1377b36403eed942a18/WebApp/Controllers/HomeController.cs#L157-L192) öğesinden ayıklanır:

```C#
public async Task<ActionResult> ReadMail()
{
    IConfidentialClientApplication app = MsalAppBuilder.BuildConfidentialClientApplication();
    AuthenticationResult result = null;
    var account = await app.GetAccountAsync(ClaimsPrincipal.Current.GetMsalAccountId());
    string[] scopes = { "Mail.Read" };

    try
    {
        // try to get token silently
        result = await app.AcquireTokenSilent(scopes, account).ExecuteAsync().ConfigureAwait(false);
    }
    catch (MsalUiRequiredException)
    {
        ViewBag.Relogin = "true";
        return View();
    }

    // More code here
    return View();
}
```

Ayrıntılar için bkz. [BuildConfidentialClientApplication ()](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/master/WebApp/Utils/MsalAppBuilder.cs) ve [Getmsalaccountıd](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/257c8f96ec3ff875c351d1377b36403eed942a18/WebApp/Utils/ClaimPrincipalExtension.cs#L38) için kod örneği


# <a name="java"></a>[Java](#tab/java)

Java örneğinde, bir API çağıran kod [Authpagecontroller. Java # L62](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62)Içindeki getUsersFromGraph yönteminde bulunur.

Yöntemi çağrılmaya çalışır `getAuthResultBySilentFlow` . Kullanıcının daha fazla kapsam için onay sağlaması gerekiyorsa, kod `MsalInteractionRequiredException` kullanıcıyı sınama için nesneyi işler.

```java
@RequestMapping("/msal4jsample/graph/me")
public ModelAndView getUserFromGraph(HttpServletRequest httpRequest, HttpServletResponse response)
        throws Throwable {

    IAuthenticationResult result;
    ModelAndView mav;
    try {
        result = authHelper.getAuthResultBySilentFlow(httpRequest, response);
    } catch (ExecutionException e) {
        if (e.getCause() instanceof MsalInteractionRequiredException) {

            // If the silent call returns MsalInteractionRequired, redirect to authorization endpoint
            // so user can consent to new scopes.
            String state = UUID.randomUUID().toString();
            String nonce = UUID.randomUUID().toString();

            SessionManagementHelper.storeStateAndNonceInSession(httpRequest.getSession(), state, nonce);

            String authorizationCodeUrl = authHelper.getAuthorizationCodeUrl(
                    httpRequest.getParameter("claims"),
                    "User.Read",
                    authHelper.getRedirectUriGraph(),
                    state,
                    nonce);

            return new ModelAndView("redirect:" + authorizationCodeUrl);
        } else {

            mav = new ModelAndView("error");
            mav.addObject("error", e);
            return mav;
        }
    }

    if (result == null) {
        mav = new ModelAndView("error");
        mav.addObject("error", new Exception("AuthenticationResult not found in session."));
    } else {
        mav = new ModelAndView("auth_page");
        setAccountInfo(mav, httpRequest);

        try {
            mav.addObject("userInfo", getUserInfoFromGraph(result.accessToken()));

            return mav;
        } catch (Exception e) {
            mav = new ModelAndView("error");
            mav.addObject("error", e);
        }
    }
    return mav;
}
// Code omitted here
```

# <a name="python"></a>[Python](#tab/python)

Python örneğinde Microsoft Graph çağıran kod [app. Sip # L53-L62](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62).

Kod, belirteç önbelleğinden bir belirteç almaya çalışır. Ardından, yetkilendirme üst bilgisini ayarladıktan sonra Web API 'sini çağırır. Belirteç alamazsanız, kullanıcıyı yeniden oturum açar.

```python
@app.route("/graphcall")
def graphcall():
    token = _get_token_from_cache(app_config.SCOPE)
    if not token:
        return redirect(url_for("login"))
    graph_data = requests.get(  # Use token to call downstream service.
        app_config.ENDPOINT,
        headers={'Authorization': 'Bearer ' + token['access_token']},
        ).json()
    return render_template('display.html', result=graph_data)
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API çağrısı](scenario-web-app-call-api-call-api.md)
