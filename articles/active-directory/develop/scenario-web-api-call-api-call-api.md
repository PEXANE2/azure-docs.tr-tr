---
title: Web API 'Lerini çağıran Web API 'SI-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Web API 'SI oluşturmayı öğrenin.
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
ms.openlocfilehash: d66a08d4e84a3771d6c3fa46b96c975869435452
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76833388"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Web API 'Leri çağıran bir Web API 'si: API çağırma

Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz. Bunu, Web API 'nizin denetleyicisinden yapabilirsiniz.

## <a name="controller-code"></a>Denetleyici kodu

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

Aşağıdaki kod, [Web API 'lerini çağıran bir Web API](scenario-web-api-call-api-acquire-token.md)'sinde gösterilen örnek kodu devam ettirir: uygulama Için belirteç alma. Kod, API denetleyicilerinin eylemleri içinde çağrılır. *ToDoList*adlı bir aşağı akış API 'si çağırır.

Belirteci aldıktan sonra, aşağı akış API 'sini çağırmak için bunu bir taşıyıcı belirteç olarak kullanın.

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

// After the token has been returned by Microsoft Authentication Library (MSAL), add it to the HTTP authorization header before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

Aşağıdaki kod, [Web API 'lerini çağıran bir Web API](scenario-web-api-call-api-acquire-token.md)'sinde gösterilen örnek kodu devam ettirir: uygulama Için belirteç alma. Kod, API denetleyicilerinin eylemleri içinde çağrılır. Aşağı akış API MS grafiğini çağırır.

Belirteci aldıktan sonra, aşağı akış API 'sini çağırmak için bunu bir taşıyıcı belirteç olarak kullanın.

```Java
private String callMicrosoftGraphMeEndpoint(String accessToken){
    RestTemplate restTemplate = new RestTemplate();

    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);

    headers.set("Authorization", "Bearer " + accessToken);

    HttpEntity<String> entity = new HttpEntity<>(null, headers);

    String result = restTemplate.exchange("https://graph.microsoft.com/v1.0/me", HttpMethod.GET,
            entity, String.class).getBody();

    return result;
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)
Bu akışı MSAL Python ile gösteren bir örnek henüz kullanılamamaktadır.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran bir Web API 'SI: üretime taşı](scenario-web-api-call-api-production.md)
