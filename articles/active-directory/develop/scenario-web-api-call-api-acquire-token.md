---
title: Web API'lerini çağıran bir web API'si için belirteç alın | Azure
titleSuffix: Microsoft identity platform
description: Uygulama için bir belirteç edinmeyi gerektiren web API'lerini çağıran bir web API'sini nasıl oluştureceğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76834119"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API'lerini çağıran bir web API'sı: Uygulama için bir belirteç edinme

İstemci uygulama nesnesi oluşturduktan sonra, web API'sını çağırmak için kullanabileceğiniz bir belirteç edinmek için kullanın.

## <a name="code-in-the-controller"></a>Denetleyicideki kod

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Aşağıda, API denetleyicilerinin eylemlerinde çağrılan bir kod örneği verilmiştir. Bu *todolist*adlı bir downstream API çağırır.

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

`BuildConfidentialClient()`web API'lerini çağıran bir web API'sindeki senaryoya [benzer: Uygulama yapılandırması.](scenario-web-api-call-api-app-configuration.md) `BuildConfidentialClient()`yalnızca bir `IConfidentialClientApplication` hesap için bilgi içeren bir önbellekle anında gerçekleşir. Hesap `GetAccountIdentifier` yöntem tarafından sağlanır.

Yöntem, `GetAccountIdentifier` web API'sinin JSON Web Belirteci 'ni (JWT) aldığı kullanıcının kimliğiyle ilişkili talepleri kullanır:

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

# <a name="java"></a>[Java](#tab/java)
Aşağıda, API denetleyicilerinin eylemlerinde çağrılan bir kod örneği verilmiştir. Bu downstream API çağırır - Microsoft Graph.

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

Python web API'sinin istemciden alınan taşıyıcı belirteci doğrulamak için bazı ara yazılımlar kullanması gerekir. Web API'si daha sonra yöntemi arayarak MSAL Python kitaplığını [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) kullanarak akış aşağı API'ye erişim belirteci elde edebilir. MSAL Python ile bu akışı gösteren bir örnek henüz mevcut değildir.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web API'si: API'yi arayın](scenario-web-api-call-api-call-api.md)
