---
title: Oturum kapatma sırasında belirteç önbelleğinden hesapları kaldırma-Microsoft Identity platform | Mavisi
description: Oturum kapatma sırasında bir hesabı belirteç önbelleğinden kaldırma hakkında bilgi edinin
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
ms.date: 09/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: ea18538662dc63876a50f52e9e6a8b3fffb3b35a
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76758879"
---
# <a name="a-web-app-that-calls-web-apis-remove-accounts-from-the-token-cache-on-global-sign-out"></a>Web API 'Lerini çağıran bir Web uygulaması: genel oturum açma sırasında belirteç önbelleğinden hesapları kaldırma

Web uygulamasında oturum açma ve oturum kapatma için [Kullanıcı oturumunu açan](scenario-web-app-sign-user-sign-in.md)Web uygulamasına oturum açma eklemeyi öğrendiniz.

Oturum kapatma, Web API 'lerini çağıran bir Web uygulaması için farklıdır. Kullanıcı uygulamanızdan veya herhangi bir uygulamadan oturumu kapattığında, bu kullanıcıyla ilişkili belirteçleri belirteç önbelleğinden kaldırmanız gerekir.

## <a name="intercept-the-callback-after-single-sign-out"></a>Çoklu oturum kapatıldıktan sonra geri çağırma işlemini kesme

Oturum açan hesapla ilişkili belirteç önbelleği girişini temizlemek için, uygulamanız `logout` sonrasında olayını ele geçirebilir. Web Apps, bir belirteç önbelleğindeki her bir kullanıcı için erişim belirteçlerini depolar. `logout` geri aramadan sonra, Web uygulamanız kullanıcıyı önbellekten kaldırabilir.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core için, ele geçirme mekanizması [WebAppServiceCollectionExtensions. cs # L151-L157](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L151-L157)`AddMsal()` yönteminde gösterilmiştir.

Uygulamanıza önceden kaydolduğunuz oturum kapatma URL 'SI, çoklu oturum kapatma uygulamanıza olanak sağlar. Microsoft Identity platform `logout` uç noktası, oturum kapatma URL 'nizi çağırır. Bu çağrı, Web uygulamanızdan veya başka bir Web uygulamasından ya da tarayıcıdan oturum kapatma işlemi başlatıldığında gerçekleşir. Daha fazla bilgi için bkz. [Çoklu oturum kapatma](v2-protocols-oidc.md#single-sign-out).

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

`RemoveAccountAsync` için kod [Microsoft. Identity. Web/Tokenalımı. cs # L264-L288](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/db7f74fd7e65bab9d21092ac1b98a00803e5ceb2/Microsoft.Identity.Web/TokenAcquisition.cs#L264-L288)' den kullanılabilir.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET örneği genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

# <a name="javatabjava"></a>[Java](#tab/java)

Java örneği, genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

# <a name="pythontabpython"></a>[Python](#tab/python)

Python örneği genel oturum kapatma üzerindeki önbellekten hesapları kaldırmaz.

---

## <a name="next-steps"></a>Sonraki adımlar

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnetcore)

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=aspnet)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=java)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Web uygulaması için belirteç alma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-call-api-acquire-token?tabs=python)

---
