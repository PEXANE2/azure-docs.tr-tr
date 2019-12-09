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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 276ff1e5e9f709aa5b38d1efa4055dfe3baf3cc5
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74919792"
---
# <a name="web-api-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web API 'Lerini çağıran Web API 'SI-uygulama için bir belirteç alın

Bir istemci uygulama nesnesi oluşturduktan sonra, bir Web API 'sini çağırmak için kullanabileceğiniz bir belirteç almak için bu uygulamayı kullanın.

## <a name="code-in-the-controller"></a>Denetleyicideki kod

Aşağıda, bir aşağı akış API 'SI (ToDoList adlı) çağıran API denetleyicilerinin eylemlerinde çağrılacak kod örneği verilmiştir.

```CSharp
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

`BuildConfidentialClient()`, [Web API 'lerini çağıran Web API 'si-uygulama yapılandırması](scenario-web-api-call-api-app-configuration.md)' nda gördüğünüze benzer. `BuildConfidentialClient()`, yalnızca bir hesabın bilgilerini içeren bir önbellekle `IConfidentialClientApplication` başlatır. Hesap `GetAccountIdentifier` yöntemi tarafından sağlanır.

`GetAccountIdentifier` yöntemi, Web API 'sinin JWT aldığı kullanıcı kimliğiyle ilişkili talepleri kullanır:

```CSharp
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
> [Web API 'SI çağırma](scenario-web-api-call-api-call-api.md)
