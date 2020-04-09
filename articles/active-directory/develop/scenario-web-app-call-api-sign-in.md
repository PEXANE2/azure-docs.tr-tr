---
title: Oturum açmadaki belirteç önbelleğinden hesapları kaldırma - Microsoft kimlik platformu | Azure
description: Oturum açma daki belirteç önbelleğinden hesabı nasıl kaldıracağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 934b756329065c466f21fca1480247065bdea28b
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881621"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Web API'lerini çağıran bir web uygulaması: Hesapları genel oturum açmadaki belirteç önbelleğinden kaldırın

Web uygulamasında kullanıcılarda oturum açan web [uygulamanıza](scenario-web-app-sign-user-sign-in.md)oturum açma eklemeyi öğrendiniz: Oturum açma ve oturum açma.

Web apis çağıran bir web uygulaması için oturum açma farklıdır. Kullanıcı uygulamanızdan veya herhangi bir uygulamadan çıkış yaptığında, bu kullanıcıyla ilişkili belirteçleri belirteç önbelleğinden kaldırmanız gerekir.

## <a name="intercept-the-callback-after-single-sign-out"></a>Tek oturum sonla'dan sonra geri aramayı durdurma

Oturumunuzu imzalayan hesapla ilişkili belirteç önbellek girişini temizlemek için, `logout` uygulamanız olaydan sonra engelleyebilir. Web uygulamaları, belirteç önbelleğinde her kullanıcı için erişim belirteçleri saklar. Web uygulamanız, `logout` sonradan geri aramayı engelleyerek kullanıcıyı önbellekten kaldırabilir.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core için, durdurma mekanizması `AddMsal()` [WebAppServiceCollectionExtensions.cs#L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157)yönteminde gösterilmiştir.

Uygulamanız için daha önce kaydolduğunuz Oturum Açma URL'si, tek bir oturum açma uygulamanızı sağlar. Microsoft kimlik `logout` platformu bitiş noktası, Logout URL'nizi çağırır. Bu çağrı, oturum açma web uygulamanızdan veya başka bir web uygulamasından veya tarayıcıdan başlatılırsa gerçekleşir. Daha fazla bilgi için tek [oturum açma'ya](v2-protocols-oidc.md#single-sign-out)bakın.

```csharp
public static class WebAppServiceCollectionExtensions
{
 public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
 {
  // Code omitted here

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Code omitted here

   // Handling the sign-out: Remove the account from MSAL.NET cache.
   options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
   {
    // Remove the account from MSAL.NET token cache.
    var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
    await tokenAcquisition.RemoveAccountAsync(context).ConfigureAwait(false);
   };
  });
  return services;
 }
}
```

Kodu `RemoveAccountAsync` [Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288)adresinden edinilebilir.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET örneği, hesapları genel oturum açma önbelleğinden kaldırmaz.

# <a name="java"></a>[Java](#tab/java)

Java örneği, hesapları genel oturum açma önbelleğinden kaldırmaz.

# <a name="python"></a>[Python](#tab/python)

Python örneği, hesapları genel oturum açma önbelleğinden kaldırmaz.

---

## <a name="next-steps"></a>Sonraki adımlar

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Web uygulaması için bir belirteç edinme](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Web uygulaması için bir belirteç edinme](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Web uygulaması için bir belirteç edinme](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Web uygulaması için bir belirteç edinme](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
