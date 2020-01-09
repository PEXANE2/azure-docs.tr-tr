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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5829ca41aaa4bd61f8878657e5eedbf6351b5df4
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423572"
---
# <a name="web-api-that-calls-web-apis---call-an-api"></a>Web API 'Lerini çağıran Web API 'si-API çağrısı

Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz. Bu, ASP.NET/ASP.NET Core Web API 'nizin denetleyicisinden yapılır.

## <a name="controller-code"></a>Denetleyici kodu

Aşağıda, [korumalı Web API 'si çağrıları Web API 'lerinde](scenario-web-api-call-api-acquire-token.md)gösterilen örnek kodun devamı vardır. API denetleyicilerinin eylemlerde çağrılan, bir aşağı akış API 'si (ToDoList adlı) çağıran bir belirteç alınıyor.

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

// Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
_httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the To Do list service.
HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
...
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-web-api-call-api-production.md)
