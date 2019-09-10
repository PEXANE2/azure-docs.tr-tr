---
title: Web API 'Lerini çağıran Web uygulaması (uygulama için bir belirteç edinin)-Microsoft Identity platform
description: Web API 'Lerini çağıran bir Web uygulaması oluşturmayı öğrenin (uygulama için bir belirteç alma)
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
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3aa144c76fb0a8e479658efdb5d43361fbbc085c
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860632"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web API 'Lerini çağıran Web uygulaması-uygulama için bir belirteç alın

Artık size istemci uygulama nesnesi oluşturduğunuza göre, bir Web API 'SI çağırmak için bir belirteç almak üzere onu kullanacaksınız. ASP.NET veya ASP.NET Core içinde, bir Web API 'SI çağrısı daha sonra denetleyicide yapılır. Şu şekilde olur:

- Belirteç önbelleğini kullanarak Web API 'SI için belirteç alma. Bu belirteci almak için öğesini çağırın `AcquireTokenSilent`.
- Korumalı API 'yi erişim belirteciyle çağırma.

## <a name="aspnet-core"></a>ASP.NET Core

Denetleyici yöntemleri, kullanıcıların kimlik doğrulamasından geçen `[Authorize]` ve Web uygulamasını kullanmasına zorlayan bir öznitelik tarafından korunur. Microsoft Graph çağıran kod aşağıda verilmiştir.

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

Aşağıda, Microsoft Graph çağırmak için bir belirteç alan HomeController eyleminin basit bir kodu verilmiştir.

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

Bu senaryo için gereken kodu daha kapsamlı olarak anlamak için, [MS-Identity-aspnetcore-WebApp-öğretici](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) öğreticisinin 2. aşama ([2-1-Web uygulaması çağrı Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)) adımına bakın.

Şöyle birçok ek karmaşıklık vardır:

- Web uygulaması için bir belirteç önbelleği uygulama (öğretici çeşitli uygulamalar sunar)
- Kullanıcı oturumu kapattığında hesabı önbellekten kaldırma
- Artımlı onay dahil olmak üzere çeşitli API 'Ler çağırma

## <a name="aspnet"></a>ASP.NET

ASP.NET ' de benzer şeyler:

- [Yetkilendir] özniteliğiyle korunan bir denetleyici eylemi, denetleyicinin `ClaimsPrincipal` üyesinin Kiracı kimliğini ve Kullanıcı kimliğini ayıklar. (ASP.NET kullanımları `HttpContext.User`.)
- Buradan, bir MSAL.NET `IConfidentialClientApplication`oluşturur.
- Son olarak, gizli istemci `AcquireTokenSilent` uygulamasının yöntemini çağırır.

Kod, ASP.NET Core gösterilen koda benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API çağrısı](scenario-web-app-call-api-call-api.md)
