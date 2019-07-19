---
title: Web API 'Lerini çağıran Web uygulaması (kod yapılandırması)-Microsoft Identity platform
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir Web uygulaması oluşturmayı öğrenin
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
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15c12aebccf34957db8442034ebbcd6ac7c107e1
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68276722"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web API 'Lerini çağıran Web uygulaması-kod yapılandırması

Web uygulaması oturum açma [kullanıcıları senaryosunda](scenario-web-app-sign-user-overview.md)görüldüğü gibi, kullanıcının oturum açması, açık kimlik Connect (OIDC) ara yazılımı için temsilci olarak verildiğinde, OIDC işlemine bağlanmak istiyorsunuz. Bunu yapmanın yolu, kullandığınız çerçeveye göre farklılık gösteren (burada ASP.NET ve ASP.NET Core), ancak sonunda, ara yazılım OıDC olaylarına abone olacaksınız. Prensibi şunlardır:

- ASP.NET veya ASP.NET Core 'un bir yetkilendirme kodu istemesine izin vereceksiniz. Bu ASP.NET/ASP.NET çekirdeğini yaparak kullanıcının oturum açmasını ve izin vermesini sağlar,
- Web uygulaması tarafından yetkilendirme kodunun alımına abone olacaksınız.
- Kimlik doğrulama kodu alındığında, kodu ve sonuç erişim belirteçlerini ve belirteç önbelleğinde belirteç deposunu yenilemek için MSAL kitaplıklarını kullanacaksınız. Buradan, önbellek diğer belirteçleri sessizce almak için uygulamanın diğer bölümlerinde kullanılabilir.

## <a name="libraries-supporting-web-app-scenarios"></a>Web uygulaması senaryolarını destekleyen kitaplıklar

Web Apps için yetkilendirme kodu akışını destekleyen kitaplıklar şunlardır:

| MSAL kitaplığı | Açıklama |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Desteklenen platformlar .NET Framework ve .NET Core platformları (UWP, Xamarin. iOS ve Xamarin. Android değil, bu platformlar ortak istemci uygulamaları oluşturmak için kullanılır) |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL. Python | Geliştirme devam ediyor-genel önizlemede |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL. Java | Geliştirme devam ediyor-genel önizlemede |

## <a name="aspnet-core-configuration"></a>ASP.NET Core yapılandırması

ASP.NET Core, `Startup.cs` dosyada şeyler meydana gelir. `OnAuthorizationCodeReceived` Açık kimlik Connect olayına abone olmak ve bu olaydan msal çağrısı yapmak isteyeceksiniz. NET 'in, `AcquireTokenFromAuthorizationCode` belirteç önbelleğinde depolamanın etkisi, istenen kapsamlar için erişim belirteci ve zaman aşımı süresi sona ermeden veya aynı kullanıcı adına bir belirteç almak için kullanılacak yenileme belirtecinin bulunduğu Yöntem , ancak farklı bir kaynak için.

Aşağıdaki koddaki açıklamalar, dalgalı MSAL.NET ve ASP.NET Core karmaşık yönlerini anlamanıza yardımcı olur. [ASP.NET Core Web uygulaması artımlı öğreticisi, Bölüm 2 '](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) de tam ayrıntılar sunulmaktadır

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add(OidcConstants.ScopeOfflineAccess);
   options.Scope.Add("user.read"); // for instance

   // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
   // where it will be usable from Controllers later (through the TokenAcquisition service)
   var handler = options.Events.OnAuthorizationCodeReceived;
   options.Events.OnAuthorizationCodeReceived = async context =>
   {
    // As AcquireTokenByAuthorizationCode is asynchronous we want to tell ASP.NET core
    // that we are handing the code even if it's not done yet, so that it does 
    // not concurrently call the Token endpoint.
    context.HandleCodeRedemption();

    // Call MSAL.NET AcquireTokenByAuthorizationCode
    var application = BuildConfidentialClientApplication(context.HttpContext,
                                                         context.Principal);
    var result = await application.AcquireTokenByAuthorizationCode(scopes.Except(scopesRequestedByMsalNet), context.ProtocolMessage.Code)
                                  .ExecuteAsync();

    // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it
    // and will not send the OAuth 2.0 request in case a further call to
    // AcquireTokenByAuthorizationCodeAsync in the future for incremental consent 
    // (getting a code requesting more scopes)
    // Share the ID Token so that the identity of the user is known in the application (in 
    // HttpContext.User)
    context.HandleCodeRedemption(null, result.IdToken);

    // Call the previous handler if any
    await handler(context);
   };
```

ASP.NET Core, gizli istemci uygulaması oluşturmak HttpContext 'teki bilgileri kullanır. Bu HttpContext, Web uygulamasının URL 'sini ve oturum açmış kullanıcıyı (bir `ClaimsPrincipal`) bilir. Ayrıca, "azuread" bölümü olan ve `_applicationOptions` veri yapısına bağlantılı ASP.NET Core yapılandırmayı kullanır. Son olarak, uygulamanın belirteç önbelleklerini koruması gerekir.

```CSharp
/// <summary>
/// Creates an MSAL Confidential client application
/// </summary>
/// <param name="httpContext">HttpContext associated with the OIDC response</param>
/// <param name="claimsPrincipal">Identity for the signed-in user</param>
/// <returns></returns>
private IConfidentialClientApplication BuildConfidentialClientApplication(HttpContext httpContext, ClaimsPrincipal claimsPrincipal)
{
 var request = httpContext.Request;

 // Find the URI of the application)
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, azureAdOptions.CallbackPath ?? string.Empty);

 // Updates the authority from the instance (including national clouds) and the tenant
 string authority = $"{azureAdOptions.Instance}{azureAdOptions.TenantId}/";

 // Instantiates the application based on the application options (including the client secret)
 var app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
               .WithRedirectUri(currentUri)
               .WithAuthority(authority)
               .Build();

 // Initialize token cache providers. In the case of Web applications, there must be one
 // token cache per user (here the key of the token cache is in the claimsPrincipal which
 // contains the identity of the signed-in user)
 if (this.UserTokenCacheProvider != null)
 {
  this.UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (this.AppTokenCacheProvider != null)
 {
  this.AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

`AcquireTokenByAuthorizationCode`ASP.NET tarafından istenen yetkilendirme kodunu gerçekten yapın ve MSAL.NET Kullanıcı belirteci önbelleğine eklenen belirteçleri alır. Buradan, ASP.NET Core denetleyicilerinde kullanılır.

## <a name="aspnet-configuration"></a>ASP.NET yapılandırması

ASP.NET yöntemi, openıdconnect yapılandırması ve `OnAuthorizationCodeReceived` olay `App_Start\Startup.Auth.cs` aboneliği dosyada olması dışında, şeyleri işler. Benzer kavramları bulacaksınız, burada, daha az dayanıklı olan yapılandırma dosyasında RedirectUri belirtmeniz gerekir:

```CSharp
private void ConfigureAuth(IAppBuilder app)
{
 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

 app.UseCookieAuthentication(new CookieAuthenticationOptions { });

 app.UseOpenIdConnectAuthentication(
 new OpenIdConnectAuthenticationOptions
 {
  Authority = Globals.Authority,
  ClientId = Globals.ClientId,
  RedirectUri = Globals.RedirectUri,
  PostLogoutRedirectUri = Globals.RedirectUri,
  Scope = Globals.BasicSignInScopes, // a basic set of permissions for user sign in & profile access
  TokenValidationParameters = new TokenValidationParameters
  {
   NameClaimType = "name",
  },
  Notifications = new OpenIdConnectAuthenticationNotifications()
  {
   AuthorizationCodeReceived = OnAuthorizationCodeReceived,
  }
 });
}
```

```CSharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
{
 // Upon successful sign in, get & cache a token using MSAL
 string userId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;
 IConfidentialClientApplication clientapp;
 clientApp = ConfidentialClientApplicationBuilder.Create(Globals.ClientId)
                  .WithClientSecret(Globals.ClientSecret)
                  .WithRedirectUri(Globals.RedirectUri)
                  .WithAuthority(new Uri(Globals.Authority))
                  .Build();

  EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);

 AuthenticationResult result = await clientapp.AcquireTokenByAuthorizationCode(scopes, context.Code)
                                              .ExecuteAsync();
}
```

Son olarak, gizli istemci uygulaması, bir istemci sertifikası veya bir istemci onayı kullanarak kimliğini de kanıtlayabilirler.
İstemci onaylamaları kullanmak, [istemci onaylamaları](msal-net-client-assertions.md) hakkında ayrıntılı olarak açıklanan gelişmiş bir senaryodur

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>ASP.NET (Core) Web uygulaması için MSAL.NET Token Cache

Web uygulamalarında (veya aslında Web API 'Lerinde), belirteç önbelleği uygulaması masaüstü uygulamaları belirteç önbelleği uygulamalarından farklıdır (genellikle [dosya tabanlı](scenario-desktop-acquire-token.md#file-based-token-cache)olur. ASP.NET/ASP.NET Core oturumu veya bir Redsıs önbelleği veya bir veritabanı ya da Azure Blob depolama alanı kullanabilir. Yukarıdaki kod parçacığında, bu, bir önbellek hizmetini bağlayan `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);` yöntem çağrısının nesnesidir. Burada ne olacağı hakkında ayrıntı bu senaryo kılavuzunun kapsamı dışındadır, ancak bağlantılar aşağıda verilmiştir.

> [!IMPORTANT]
> Web uygulamaları ve Web API 'Leri için çok önemli bir şey, Kullanıcı başına (hesap başına) bir belirteç önbelleği olmalıdır. Her hesap için belirteç önbelleğini seri hale getirmeniz gerekir.

Web uygulamaları ve Web API 'Leri için belirteç önbelleklerinin nasıl kullanılacağına ilişkin örnekler, aşama [2-2 belirteç önbelleğindeki](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache) [ASP.NET Core Web uygulaması öğreticisinde](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) bulunabilir. Uygulamalar için, [Microsoft-Authentication-Extensions-for-DotNet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) kitaplığındaki ( [Microsoft. Identity. Client. Extensions. Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) klasöründe) [tokencacheproviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) ' a aşağıdaki klasöre göz atalım.

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, Kullanıcı bir belirteçle oturum açtığında belirteç önbelleğinde depolanır. Daha sonra, Web uygulamasının diğer bölümlerinde nasıl kullanıldığını görelim.

> [!div class="nextstepaction"]
> [Web uygulamasında oturum açma](scenario-web-app-call-api-sign-in.md)
