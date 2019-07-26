---
title: Korumalı Web API 'SI-uygulama kodu yapılandırması | Azure Active Directory
description: Korumalı bir Web API 'SI oluşturmayı ve uygulamanızın kodunu yapılandırmayı öğrenin.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: b249b99faa62e73b9aa3247f71f88767fca96f01
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488843"
---
# <a name="protected-web-api-adding-authorization-to-your-api"></a>Korumalı Web API 'SI: API 'nize yetkilendirme ekleme

Bu makalede, Web API 'nize nasıl yetkilendirme ekleyebileceğiniz açıklanır. Bu koruma, API 'nin yalnızca şu şekilde çağrılmasına sağlar:

- Doğru kapsamları olan kullanıcılar adına uygulamalar.
- Doğru uygulama rollerine sahip uygulamaları Daemon.

> [!NOTE]
> Bu makaledeki kod parçacıkları, tam işlevli olan aşağıdaki örneklerden ayıklanır
>
> - GitHub 'da [Web API 'si artımlı öğreticisini ASP.NET Core](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/1.%20Desktop%20app%20calls%20Web%20API/TodoListService/Controllers/TodoListController.cs#L37)
> - [ASP.NET Web API 'SI örneği](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof/blob/dfd0115533d5a230baff6a3259c76cf117568bd9/TodoListService/Controllers/TodoListController.cs#L48)

Bir ASP.NET/ASP.NET Core Web API 'sini korumak için aşağıdakilerden birine `[Authorize]` özniteliği eklemeniz gerekir:

- Denetleyicinin kendisi, denetleyicinin tüm eylemlerinin korunmasını istiyorsanız
- API 'niz için bireysel denetleyici eylemi

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Ancak bu koruma yeterli değildir. Yalnızca ASP.NET/ASP.NET Core 'un belirteci doğrulayacağını garanti eder. API 'nizin, Web API 'nizi çağırmak için kullanılan belirtecin, beklediği taleplerle istendiğini doğrulaması gerekir:

- Bir kullanıcı adına API çağrılırsa *kapsamlar*.
- *Uygulama rolleri*, API bir Daemon uygulamasından çağrılabilir.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>API 'lerde Kullanıcı adına çağrılan kapsamları doğrulama

API 'niz bir kullanıcı adına bir istemci uygulaması tarafından çağrılırsa, API için belirli kapsamları olan bir taşıyıcı belirteci istemesi gerekir. (Bkz. [kod yapılandırması | Taşıyıcı belirteci](scenario-protected-web-api-app-configuration.md#bearer-token).)

```CSharp
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

`VerifyUserHasAnyAcceptedScope` Yöntemi aşağıdakine benzer şekilde yapılır:

- `http://schemas.microsoft.com/identity/claims/scope` Veya`scp`adlı bir talep olduğunu doğrulayın.
- Talebin API tarafından beklenen kapsamı içeren bir değere sahip olduğunu doğrulayın.

```CSharp
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

Bu [örnek kod](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/blob/02352945c1c4abb895f0b700053506dcde7ed04a/Microsoft.Identity.Web/Resource/ScopesRequiredByWebAPIExtension.cs#L47) ASP.NET Core içindir. `HttpContext.User` ASP.NET için yalnızca ile `ClaimsPrincipal.Current`değiştirin ve talep türünü `"http://schemas.microsoft.com/identity/claims/scope"` ile `"scp"`değiştirin. (Ayrıca bu makalenin ilerleyen kısımlarında kod parçacığına bakın.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Daemon uygulamaları tarafından çağrılan API 'lerde uygulama rolleri doğrulanıyor

Web API 'niz bir [Daemon uygulaması](scenario-daemon-overview.md)tarafından çağrılırsa, bu UYGULAMANıN Web API 'niz için uygulama izni olması gerekir. API 'nizin bu izinleri (örneğin, `access_as_application` uygulama rolü) sunduğu [uygulama izinlerini (uygulama rolleri) açığa çıkardık](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) .
Artık API 'lerinizin, aldığı belirtecin `roles` talebi içerdiğini ve bu talebin beklediği değere sahip olduğunu doğrulaması gerekir. Bu doğrulamayı yapan kod,, için `scopes`test etme yerine, test edilecek `roles`izinleri doğrulayan koda benzer.

```CSharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

`ValidateAppRole()` Yöntemi şuna benzer olabilir:

```CSharp
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

Bu kez, kod parçacığı ASP.NET içindir. `ClaimsPrincipal.Current` ASP.NET Core için yalnızca ile `HttpContext.User`değiştirin ve `"roles"` talep adını ile `"http://schemas.microsoft.com/identity/claims/roles"`değiştirin. (Bu makalenin önceki kısımlarında bulunan kod parçacığına da bakın.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Web API 'sinin yalnızca Daemon uygulamaları tarafından çağrılması gerekiyorsa yalnızca uygulama belirteçlerini kabul etme

Talep `roles` , kullanıcı atama desenlerindeki kullanıcılar için de kullanılır. (Bkz [. nasıl yapılır: Uygulamanıza uygulama rolleri ekleyin ve bunları belirteçte](howto-add-app-roles-in-azure-ad-apps.md)alın.) Bu nedenle, rollerin her ikisine de atanabilir olması halinde, uygulamaların kullanıcı olarak ve diğer şekilde oturum açmalarına olanak tanır. Bu karışıklık oluşmasını önlemek için kullanıcılar ve uygulamalar için farklı roller bildirmeniz önerilir.

Yalnızca Daemon uygulamalarının Web API 'nizi çağırmasını sağlamak istiyorsanız, bir koşul ekleyin, uygulama rolünü doğruladığınızda, belirteç yalnızca uygulama belirteci olur:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid")?.Value;
string sub = ClaimsPrincipal.Current.FindFirst("sub")?.Value;
bool isAppOnlyToken = oid == sub;
```

Ters koşulun denetlenmesi yalnızca, API 'nizi çağırmak için bir kullanıcının oturum açmasını sağlayacak olan uygulamalara izin verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-protected-web-api-production.md)
