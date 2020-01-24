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
ms.openlocfilehash: 1c3d6f8d47152d70bdeaabbbc6d7b81187291857
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76701816"
---
# <a name="a-web-api-that-calls-web-apis-acquire-a-token-for-the-app"></a>Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma

Bir istemci uygulama nesnesi oluşturduktan sonra, bir Web API 'sini çağırmak için kullanabileceğiniz bir belirteç almak için bu uygulamayı kullanın.

## <a name="code-in-the-controller"></a>Denetleyicideki kod

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Leri çağıran bir Web API 'si: API çağırma](scenario-web-api-call-api-call-api.md)
