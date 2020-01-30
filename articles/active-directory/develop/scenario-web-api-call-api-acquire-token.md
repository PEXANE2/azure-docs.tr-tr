---
title: Web API 'Lerini çağıran bir Web API 'SI için belirteç alın | Mavisi
titleSuffix: Microsoft identity platform
description: Uygulama için belirteç alma gerektiren Web API 'Lerini çağıran bir Web API 'SI oluşturmayı öğrenin.
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 2721837459af24f39bb15ee17d394345cbb37eb1
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76834119"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma

Bir istemci uygulama nesnesi oluşturduktan sonra, bir Web API 'sini çağırmak için kullanabileceğiniz bir belirteç almak için bu uygulamayı kullanın.

## <a name="code-in-the-controller"></a>Denetleyicideki kod

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

API denetleyicilerinin eylemlerinde çağrılan kod örneği aşağıda verilmiştir. *ToDoList*adlı bir aşağı akış API 'si çağırır.

```csharp
private async Task GetTodoList(bool isAppStarting)
{
 ...
 //
 // Get an access token to call the To Do service.
 //
 AuthenticationResult result = null;
 try
 {
  app = BuildConfidentialClient(HttpContext, HttpContext.User);
  result = await app.AcquireTokenSilent(Scopes, account)
                     .ExecuteAsync()
                     .ConfigureAwait(false);
 }
...
}
```

`BuildConfidentialClient()`, Web API ['lerini çağıran bir Web API 'sindeki senaryoya benzer: uygulama yapılandırması](scenario-web-api-call-api-app-configuration.md). `BuildConfidentialClient()`, yalnızca bir hesap için bilgi içeren bir önbellekle `IConfidentialClientApplication` başlatır. Hesap `GetAccountIdentifier` yöntemi tarafından sağlanır.

`GetAccountIdentifier` yöntemi, Web API 'sinin JSON Web Token (JWT) aldığı kullanıcı kimliğiyle ilişkili talepleri kullanır:

```csharp
public static string GetMsalAccountId(this ClaimsPrincipal claimsPrincipal)
{
 string userObjectId = GetObjectId(claimsPrincipal);
 string tenantId = GetTenantId(claimsPrincipal);

 if (    !string.IsNullOrWhiteSpace(userObjectId)
      && !string.IsNullOrWhiteSpace(tenantId))
 {
  return $"{userObjectId}.{tenantId}";
 }

 return null;
}
```

# <a name="javatabjava"></a>[Java](#tab/java)
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

# <a name="pythontabpython"></a>[Python](#tab/python)

Bir Python Web API 'sinin istemciden alınan taşıyıcı belirtecini doğrulamak için bazı ara yazılım kullanması gerekir. Web API 'si daha sonra [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) YÖNTEMINI çağırarak msal Python kitaplığı kullanarak aşağı akış API 'sine yönelik erişim belirtecini alabilir. Bu akışı MSAL Python ile gösteren bir örnek henüz kullanılamamaktadır.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web API 'si: API çağırma](scenario-web-api-call-api-call-api.md)
