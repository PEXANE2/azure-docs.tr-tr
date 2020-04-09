---
title: Web API'lerini çağıran bir web uygulamasında belirteç alın - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir web uygulaması için nasıl belirteç elde edebilirsiniz öğrenin
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
ms.openlocfilehash: 196d941c5c3b18b737f7a11c25ebbb9eab91be1e
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885047"
---
# <a name="a-web-app-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API'lerini çağıran bir web uygulaması: Uygulama için bir belirteç edinme

İstemci uygulama nesnenizi oluşturabilirsiniz. Şimdi, bir web API aramak için bir belirteç elde etmek için kullanacağız. ASP.NET veya ASP.NET Core'da, web API'sini çağırmak denetleyicide yapılır:

- Belirteç önbelleğini kullanarak web API'si için bir belirteç alın. Bu belirteci almak `AcquireTokenSilent` için, yöntemi arayın.
- Erişim belirtecibir parametre olarak geçen, korumalı API çağırın.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Denetleyici yöntemleri, kullanıcıların `[Authorize]` kimlik doğrulaması nın web uygulamasını kullanmasını gerektiren bir öznitelik tarafından korunur. Microsoft Graph çağıran kod şunlardır:

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

Hizmet `ITokenAcquisition` bağımlılık enjeksiyonu kullanılarak ASP.NET tarafından enjekte edilir.

Microsoft Graph'ı aramak için bir `HomeController`belirteç alan eyleminin basitleştirilmiş kodu aşağıda verilmiştir:

```csharp
public async Task<IActionResult> Profile()
{
 // Acquire the access token.
 string[] scopes = new string[]{"user.read"};
 string accessToken = await tokenAcquisition.GetAccessTokenOnBehalfOfUserAsync(scopes);

 // Use the access token to call a protected web API.
 HttpClient client = new HttpClient();
 client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
 string json = await client.GetStringAsync(url);
}
```

Bu senaryo için gerekli kodu daha iyi anlamak için, [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) öğreticisinin faz 2[(2-1-Web App Calls Microsoft Graph)](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)adımına bakın.

Diğer karmaşık varyasyonlar vardır, örneğin:

- Birkaç API arıyorum.
- Aşamalı onay ve koşullu erişim işleme.

Bu gelişmiş [adımlar, 3-WebApp-multi-API'ler](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/3-WebApp-multi-APIs) öğreticinin 3.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET kodu ASP.NET Core için gösterilen koda benzer:

- [Authorize] özniteliği tarafından korunan bir denetleyici eylemi, denetleyicinin `ClaimsPrincipal` üyesinin kiracı kimliğini ve kullanıcı kimliğini ayıklar. (ASP.NET `HttpContext.User`kullanır .)
- Oradan, MSAL.NET `IConfidentialClientApplication` bir nesne oluşturur.
- Son olarak, `AcquireTokenSilent` gizli istemci uygulama yöntemi çağırır.

# <a name="java"></a>[Java](#tab/java)

Java örneğinde, API çağıran kod [AuthPageController.java#L62'deki](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthPageController.java#L62)getUsersFromGraph yönteminde dir.

Yöntem. `getAuthResultBySilentFlow` Kullanıcının daha fazla kapsama izin alması gerekiyorsa, kod `MsalInteractionRequiredException` nesneyi kullanıcıya meydan okumak için işler.

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

Python örneğinde, Microsoft Graph adını veren kod [app.py#L53-L62'dedir.](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/48637475ed7d7733795ebeac55c5d58663714c60/app.py#L53-L62)

Kod, belirteç önbelleğinden bir belirteç almaya çalışır. Daha sonra, yetkilendirme üstbilgisini ayarladıktan sonra web API'sını çağırır. Bir belirteç alamıyorsa, kullanıcıyı yeniden imzalar.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API çağrısı](scenario-web-app-call-api-call-api.md)
