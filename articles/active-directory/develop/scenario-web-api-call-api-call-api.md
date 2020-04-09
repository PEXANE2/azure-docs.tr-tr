---
title: Web API'ları çağıran Web API'sı - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir web API'sini nasıl oluşturan öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6bbd24978891efd147b0c317c1746d13961ce5e9
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885098"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Web API'lerini çağıran bir web API'si: API'yi arayın

Bir belirteç aldıktan sonra, korumalı web API'sini arayabilirsiniz. Bunu web API'nizin denetleyicisinden yaparsınız.

## <a name="controller-code"></a>Denetleyici kodu

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Aşağıdaki kod, web API'lerini çağıran bir web API'sinde gösterilen örnek kodu devam [eder: Uygulama için bir belirteç edinin.](scenario-web-api-call-api-acquire-token.md) Kod, API denetleyicilerinin eylemlerinde çağrılır. Bu *todolist*adlı bir downstream API çağırır.

Belirteci aldıktan sonra, akış aşağı API'yi aramak için taşıyıcı belirteci olarak kullanın.

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

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki kod, web API'lerini çağıran bir web API'sinde gösterilen örnek kodu devam [eder: Uygulama için bir belirteç edinin.](scenario-web-api-call-api-acquire-token.md) Kod, API denetleyicilerinin eylemlerinde çağrılır. Bu downstream API MS Graph çağırır.

Belirteci aldıktan sonra, akış aşağı API'yi aramak için taşıyıcı belirteci olarak kullanın.

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

# <a name="python"></a>[Python](#tab/python)
MSAL Python ile bu akışı gösteren bir örnek henüz mevcut değildir.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web API'sı: Üretime geç](scenario-web-api-call-api-production.md)
