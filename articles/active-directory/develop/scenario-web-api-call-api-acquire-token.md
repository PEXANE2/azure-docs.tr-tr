---
title: Web API 'Lerini çağıran bir Web API 'SI için belirteç alın | Mavisi
titleSuffix: Microsoft identity platform
description: Uygulama için belirteç alma gerektiren Web API 'Lerini çağıran bir Web API 'SI oluşturmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7e0701cc5a9bb14800a48e2281dba1eb6ea0cf72
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026467"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma

Bir istemci uygulama nesnesi oluşturduktan sonra, bir Web API 'sini çağırmak için kullanabileceğiniz bir belirteç almak için bu uygulamayı kullanın.

## <a name="code-in-the-controller"></a>Denetleyicideki kod

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

API denetleyicilerinin eylemlerinde çağrılan Microsoft. Identity. Web kullanan kod örneği aşağıda verilmiştir. *ToDoList*adlı bir aşağı akış API 'si çağırır. Aşağı akış API 'sini çağırmak için bir belirteç almak üzere, `ITokenAcquisition` hizmet programını denetleyicinin yapıcısına (veya Blazor kullanıyorsanız sayfa yapıcısına) ekleyerek, `GetAccessTokenForUserAsync` `GetAccessTokenForAppAsync` bir Daemon senaryosu olması durumunda bu hizmeti, Kullanıcı () veya uygulamanın kendisi () için bir belirteç alarak, denetleyici eylemlerinde kullanabilirsiniz.

```csharp
[Authorize]
public class MyApiController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    static readonly string[] scopeRequiredByApi = new string[] { "access_as_user" };

     static readonly string[] scopesToAccessDownstreamApi = new string[] { "api://MyTodolistService/access_as_user" };

    private readonly ITokenAcquisition _tokenAcquisition;

    public MyApiController(ITokenAcquisition tokenAcquisition)
    {
        _tokenAcquisition = tokenAcquisition;
    }

    public IActionResult Index()
    {
        HttpContext.VerifyUserHasAnyAcceptedScope(scopeRequiredByApi);

        string accessToken = _tokenAcquisition.GetAccessTokenForUserAsync(scopesToAccessDownstreamApi);
        return await callTodoListService(accessToken);
    }
}
```

Yöntemi hakkında daha fazla bilgi için `callTodoListService` bkz. [Web API 'Leri çağıran BIR Web API 'SI: API çağırma](scenario-web-api-call-api-call-api.md).

# <a name="java"></a>[Java](#tab/java)
API denetleyicilerinin eylemlerinde çağrılan kod örneği aşağıda verilmiştir. Aşağı akış API 'sini Microsoft Graph çağırır.

```java
@RestController
public class ApiController {

    @Autowired
    MsalAuthHelper msalAuthHelper;

    @RequestMapping("/graphMeApi")
    public String graphMeApi() throws MalformedURLException {

        String oboAccessToken = msalAuthHelper.getOboToken("https://graph.microsoft.com/.default");

        return callMicrosoftGraphMeEndpoint(oboAccessToken);
    }

}
```

# <a name="python"></a>[Python](#tab/python)

Bir Python Web API 'sinin istemciden alınan taşıyıcı belirtecini doğrulamak için bazı ara yazılım kullanması gerekir. Web API 'si daha sonra yöntemi çağırarak MSAL Python kitaplığı kullanarak aşağı akış API 'sine yönelik erişim belirtecini alabilir [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) . Bu akışı MSAL Python ile gösteren bir örnek henüz kullanılamamaktadır.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web API 'si: API çağırma](scenario-web-api-call-api-call-api.md)
