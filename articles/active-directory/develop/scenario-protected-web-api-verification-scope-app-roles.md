---
title: Kapsamları ve uygulama rollerini korumalı Web API'yi doğrulama | Azure
titleSuffix: Microsoft identity platform
description: Korumalı bir web API'sini nasıl oluşturup uygulamanızın kodunu nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 816a9620a3486b534f9293084b7c4f5b4f748033
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76768129"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>Korumalı web API'sı: Kapsamları ve uygulama rollerini doğrulayın

Bu makalede, web API'nize nasıl yetkilendirme ekleyebileceğiniz açıklanmaktadır. Bu koruma, API'nin yalnızca aşağıdakiler tarafından çağrılmasını sağlar:

- Doğru kapsamlara sahip kullanıcılar adına başvurular.
- Doğru uygulama rollerine sahip Daemon uygulamaları.

> [!NOTE]
> Bu makaledeki kod parçacıkları, tamamen işlevsel olan aşağıdaki örneklerden ayıklanır:
>
> - ASP.NET [Core Web API artımlı öğretici](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37) GitHub üzerinde
> - [ASP.NET Web API örneği](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

bir ASP.NET veya ASP.NET Core web API'sını korumak için, özniteliği aşağıdaki öğelerden birine eklemeniz `[Authorize]` gerekir:

- Tüm denetleyici eylemlerinin korunmasını istiyorsanız denetleyicinin kendisi
- API'niz için tek tek denetleyici eylemi

```csharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Ama bu koruma yeterli değil. Yalnızca ASP.NET ve ASP.NET Core'un belirteci doğruladığını garanti eder. API'nizin, API'yi aramak için kullanılan belirteci beklenen taleplerle birlikte istendiğini doğrulaması gerekir. Özellikle bu iddiaların doğrulanması gerekir:

- API kullanıcı adına çağrılırsa *kapsamları.*
- API bir daemon uygulamasından çağrılabiliyorsa *uygulama rolleri.*

## <a name="verify-scopes-in-apis-called-on-behalf-of-users"></a>Kullanıcılar adına çağrılan API'lerde kapsamları doğrulama

Bir istemci uygulaması bir kullanıcı adına API'nizi ararsa, API'nin API için belirli kapsamları olan bir taşıyıcı belirteci istemesi gerekir. Daha fazla bilgi için [Kod yapılandırması | Taşıyıcı belirteci](scenario-protected-web-api-app-configuration.md#bearer-token).

```csharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

Yöntem `VerifyUserHasAnyAcceptedScope` aşağıdaki adımları gibi bir şey yapar:

- Adlandırılmış `http://schemas.microsoft.com/identity/claims/scope` bir talep olduğunu `scp`doğrulayın veya .
- Talebin API tarafından beklenen kapsamı içeren bir değere sahip olduğunu doğrulayın.

```csharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

Önceki [örnek kod](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47) ASP.NET Core içindir. ASP.NET için, `HttpContext.User` sadece `ClaimsPrincipal.Current`, ile değiştirin ve talep türünü `"http://schemas.microsoft.com/identity/claims/scope"` değiştirin. `"scp"` Ayrıca bu makalede daha sonra kod parçacığı bakın.

## <a name="verify-app-roles-in-apis-called-by-daemon-apps"></a>Daemon uygulamaları tarafından çağrılan API'lerde uygulama rollerini doğrulama

Web API'niz bir [daemon uygulaması](scenario-daemon-overview.md)tarafından çağrılırsa, bu uygulama web API'nize uygulama izni gerektirmelidir. [Uygulama izinlerini (uygulama rollerini) açığa çıkarırken](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles)gösterildiği gibi, API'niz bu tür izinleri ortaya çıkarır. Bir örnek `access_as_application` uygulama rolüdür.

Şimdi API'nizin aldığı belirteç `roles` talebi içerdiğini ve bu talebin beklenen değere sahip olduğunu doğrulamasını gerekir. Doğrulama kodu, denetleyicinizin kapsamlar yerine roller için eylem testleri dışında, devralınan izinleri doğrulayan koda benzer:

```csharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

Yöntem `ValidateAppRole` şu gibi olabilir:

```csharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage
        { StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found"
        });
    }
}
}
```

Bu kez, kod parçacığı ASP.NET içindir. ASP.NET Core için, `ClaimsPrincipal.Current` `HttpContext.User`sadece , `"roles"` ile değiştirin ve talep adı değiştirin `"http://schemas.microsoft.com/identity/claims/roles"`. Ayrıca bu makalede daha önce kod parçacığı bakın.

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Web API'sinin yalnızca daemon uygulamaları tarafından çağrılması gerekiyorsa yalnızca uygulama belirteçlerini kabul etme

Kullanıcılar, [nasıl kullanılır: Uygulamanızda uygulama rollerini ekleme ve belirteçte alma](howto-add-app-roles-in-azure-ad-apps.md)da gösterildiği gibi, kullanıcı atama modellerinde rol taleplerini kullanabilirler. Roller her ikisine de atayabiliyorsa, rolleri denetlemek uygulamaların kullanıcı olarak oturum açmasına ve kullanıcıların uygulama olarak oturum açmasına izin verir. Bu karışıklığı önlemek için kullanıcılar ve uygulamalar için farklı roller beyan etmenizi öneririz.

Yalnızca daemon uygulamalarının web API'nizi aramasını istiyorsanız, uygulama rolünü doğrularken belirteci yalnızca uygulama belirteci olması koşulunu ekleyin.

```csharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Ters koşulu denetlemek, yalnızca oturum açan uygulamaların API'nizi aramasına olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşıma](scenario-protected-web-api-production.md)
