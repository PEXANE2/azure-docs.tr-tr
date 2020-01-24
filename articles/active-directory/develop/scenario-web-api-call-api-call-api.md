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
ms.openlocfilehash: c7c54c7c4718cea479a812574e961ef2338a6be6
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701765"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Web API 'Leri çağıran bir Web API 'si: API çağırma

Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz. Bunu ASP.NET veya ASP.NET Core Web API 'nizin denetleyicisinden yapabilirsiniz.

## <a name="controller-code"></a>Denetleyici kodu

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran bir Web API 'SI: üretime taşı](scenario-web-api-call-api-production.md)
