---
title: Kapsamları ve uygulama rollerini korunan Web API 'sini doğrulama | Mavisi
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI oluşturmayı ve uygulamanızın kodunu yapılandırmayı öğrenin.
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
ms.openlocfilehash: a4ee2679da5065ab9e9b02d4ddb313fab75e78f7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83845144"
---
# <a name="protected-web-api-verify-scopes-and-app-roles"></a>Korumalı Web API 'SI: kapsamları ve uygulama rollerini doğrulama

Bu makalede, Web API 'nize nasıl yetkilendirme ekleyebileceğiniz açıklanır. Bu koruma, API 'nin yalnızca şu şekilde çağrılmasına sağlar:

- Doğru kapsamları olan kullanıcılar adına uygulamalar.
- Doğru uygulama rollerine sahip uygulamaları Daemon.

> [!NOTE]
> Bu makaledeki kod parçacıkları, tam işlevli olan aşağıdaki örneklerden ayıklanır:
>
> - GitHub 'da [Web API 'si artımlı öğreticisini ASP.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37)
> - [ASP.NET Web API 'SI örneği](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

Bir ASP.NET veya ASP.NET Core Web API 'sini korumak için, `[Authorize]` aşağıdaki öğelerden birine özniteliğini eklemeniz gerekir:

- Tüm denetleyici eylemlerinin korunmasını istiyorsanız denetleyicinin kendisi
- API 'niz için bireysel denetleyici eylemi

```csharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Ancak bu koruma yeterli değildir. Yalnızca bu ASP.NET ve ASP.NET Core belirtecini doğrular. API 'nizin, API 'YI çağırmak için kullanılan belirtecin beklenen taleplerle istendiğini doğrulaması gerekir. Bu talepler belirli bir doğrulamaya gerek duyar:

- Bir kullanıcı adına API çağrılırsa *kapsamlar* .
- API bir Daemon uygulamasından çağrılabilecek ise *uygulama rolleri* .

## <a name="verify-scopes-in-apis-called-on-behalf-of-users"></a>API 'lerde Kullanıcı adına çağrılan kapsamları doğrulama

İstemci uygulaması, API 'nizi bir kullanıcı adına çağırırsa, API 'nin API için belirli kapsamları olan bir taşıyıcı belirteç istemesi gerekir. Daha fazla bilgi için bkz. [kod yapılandırma | Taşıyıcı belirteci](scenario-protected-web-api-app-configuration.md#bearer-token).

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

`VerifyUserHasAnyAcceptedScope`Yöntemi aşağıdaki adımlara benzer bir şekilde yapılır:

- Veya adlı bir talep olduğunu doğrulayın `http://schemas.microsoft.com/identity/claims/scope` `scp` .
- Talebin API tarafından beklenen kapsamı içeren bir değeri olduğunu doğrulayın.

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

Yukarıdaki [örnek kod](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47) ASP.NET Core içindir. ASP.NET için yalnızca ile değiştirin `HttpContext.User` `ClaimsPrincipal.Current` ve talep türünü `"http://schemas.microsoft.com/identity/claims/scope"` ile değiştirin `"scp"` . Ayrıca, bu makalenin ilerleyen kısımlarında kod parçacığına bakın.

## <a name="verify-app-roles-in-apis-called-by-daemon-apps"></a>Daemon uygulamaları tarafından çağrılan API 'lerde uygulama rollerini doğrulama

Web API 'niz bir [Daemon uygulaması](scenario-daemon-overview.md)tarafından çağrılırsa, bu UYGULAMANıN Web API 'niz için uygulama izni olması gerekir. [Uygulama izinlerini (uygulama rolleri) gösterme](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles)bölümünde gösterildiği gıbı, API 'niz bu izinleri gösterir. Uygulama rolü bir örnektir `access_as_application` .

Şimdi API 'nizin aldığı belirtecin `roles` talebi içerdiğini ve bu talebin beklenen değere sahip olduğunu doğrulaması gerekir. Doğrulama kodu, izin verilen izinleri doğrulayan koda benzerdir, ancak denetleyici eylemi kapsamlar yerine roller için test eder:

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

`ValidateAppRole`Yöntemi şuna benzer olabilir:

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

Bu kez, kod parçacığı ASP.NET içindir. ASP.NET Core için yalnızca ile değiştirin `ClaimsPrincipal.Current` `HttpContext.User` ve `"roles"` talep adını ile değiştirin `"http://schemas.microsoft.com/ws/2008/06/identity/claims/role"` . Ayrıca, bu makalenin önceki kısımlarında kod parçacığına bakın.

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Web API 'sinin yalnızca Daemon uygulamaları tarafından çağrılması gerekiyorsa yalnızca uygulama belirteçlerini kabul etme

Kullanıcılar ayrıca, [nasıl yapılır: uygulamanızda uygulama rolleri ekleme ve bunları belirteçte alma](howto-add-app-roles-in-azure-ad-apps.md)bölümünde gösterildiği gibi kullanıcı atama desenlerinde roller taleplerini da kullanabilir. Roller her ikisine de atanabilir ise, rolleri denetlemek uygulamaların kullanıcılar ve kullanıcılar olarak oturum açmalarına olanak tanır. Bu karışıklık oluşmasını önlemek için kullanıcılar ve uygulamalar için farklı roller bildirmeniz önerilir.

Yalnızca Daemon uygulamalarının Web API 'nizi çağırmasını istiyorsanız, uygulama rolünü doğruladığınızda, belirtecin yalnızca uygulama belirteci olduğu koşulu ekleyin.

```csharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Ters koşulun denetlenmesi, yalnızca bir kullanıcının API 'nizi çağırması için oturum açmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-protected-web-api-production.md)
