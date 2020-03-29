---
title: Web API'leri çağıran bir web uygulamasını yapılandırma - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir web uygulamasının kodunu nasıl yapılandırılacaköğren
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 374b215a737efbe3d421b6dc49af01303ec54473
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759169"
---
# <a name="a-web-app-that-calls-web-apis-code-configuration"></a>Web API'lerini çağıran bir web uygulaması: Kod yapılandırması

Web uygulamasında [kullanıcı senaryosunu işaretleyen](scenario-web-app-sign-user-overview.md) gösterildiği gibi, web uygulaması kullanıcıyı oturum alabilmek için [OAuth 2.0 yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md) kullanır. Bu akış iki adımdan oluşur:

1. Bir yetkilendirme kodu isteyin. Bu bölüm, kullanıcıyla özel bir diyaloğu Microsoft kimlik platformuna devreder. Bu diyalog sırasında, kullanıcı web API'lerinin kullanımını kabul eder ve imzalar. Özel diyalog başarıyla sona erdiğinde, web uygulaması uri yönlendirmesi hakkında bir yetkilendirme kodu alır.
1. Yetkilendirme kodunu katarak API için bir erişim belirteci isteyin.

[Kullanıcı senaryolarında işaretleyen Web uygulaması](scenario-web-app-sign-user-overview.md) yalnızca ilk adımı kapsamaktadır. Burada, yalnızca kullanıcıları işaretlemekle kalmıyor, aynı zamanda web API'lerini de çağıracak şekilde web uygulamanızı nasıl değiştirabileceğinizi öğreniyorsunuz.

## <a name="libraries-that-support-web-app-scenarios"></a>Web uygulaması senaryolarını destekleyen kitaplıklar

Microsoft Kimlik Doğrulama Kitaplığı'ndaki (MSAL) aşağıdaki kitaplıklar web uygulamaları için yetkilendirme kodu akışını destekler:

| MSAL kütüphanesi | Açıklama |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | .NET Framework ve .NET Core platformları için destek. Desteklenmeyen evrensel Windows Platformu (UWP), Xamarin.iOS ve Xamarin.Android, çünkü bu platformlar kamu istemci uygulamaları oluşturmak için kullanılır. |
| ![MSAL Piton](media/sample-v2-code/logo_python.png) <br/> Python için MSAL | Python web uygulamaları için destek. |
| ![MSAL Java](media/sample-v2-code/logo_java.png) <br/> Java için MSAL | Java web uygulamaları için destek. |

İlgilendiğiniz platformun sekmesini seçin:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Kullanıcı oturum açma açık kimlik bağlantısı (OIDC) ara yazılımına devredildiklerine göre, OIDC işlemiyle etkileşimde bulunmaktadır. Nasıl etkileşimde bulunduğun kullandığınız çerçeveye bağlıdır.

ASP.NET Core için, ara yazılım OIDC etkinliklerine abone olacaksınız:

- ASP.NET Core'un Open ID Connect ara yazılımı yoluyla bir yetkilendirme kodu istemesine izin vereceksiniz. ASP.NET veya ASP.NET Core kullanıcının oturum açmasına ve onay alamasına izin verecektir.
- Yetkilendirme kodunu almak için web uygulamasına abone olacaksınız. Bu abonelik bir C# temsilcisi kullanılarak yapılır.
- Yetkilendirme kodu alındığı zaman, bunu kullanmak için MSAL kitaplıklarını kullanırsınız. Ortaya çıkan erişim belirteçleri ve yenileme belirteçleri belirteç önbelleğinde depolanır. Önbellek, diğer belirteçleri sessizce elde etmek için uygulamanın denetleyiciler gibi diğer bölümlerinde kullanılabilir.

Bu makalede kod örnekleri ve aşağıdaki bir [ASP.NET Core web uygulaması artımlı öğretici, bölüm 2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph)ayıklanır. Tam uygulama ayrıntıları için bu öğreticiye başvurmak isteyebilirsiniz.

> [!NOTE]
> Burada kod örneklerini tam olarak anlamak için, [ASP.NET Temel temelleri](https://docs.microsoft.com/aspnet/core/fundamentals)ve özellikle [bağımlılık enjeksiyonu](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) ve [seçenekleri](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options)hakkında bilginiz olması gerekir.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Kullanıcı oturum açma açık kimlik bağlantısı (OIDC) ara yazılımına devredildiklerine göre, OIDC işlemiyle etkileşimde bulunmaktadır. Nasıl etkileşimde bulunduğun kullandığınız çerçeveye bağlıdır.

ASP.NET için, ara yazılım OIDC etkinliklerine abone olacaksınız:

- ASP.NET Core'un Open ID Connect ara yazılımı yoluyla bir yetkilendirme kodu istemesine izin vereceksiniz. ASP.NET veya ASP.NET Core kullanıcının oturum açmasına ve onay alamasına izin verecektir.
- Yetkilendirme kodunu almak için web uygulamasına abone olacaksınız. Bu abonelik bir C# temsilcisi kullanılarak yapılır.
- Yetkilendirme kodu alındığı zaman, bunu kullanmak için MSAL kitaplıklarını kullanırsınız. Ortaya çıkan erişim belirteçleri ve yenileme belirteçleri belirteç önbelleğinde depolanır. Önbellek, diğer belirteçleri sessizce elde etmek için uygulamanın denetleyiciler gibi diğer bölümlerinde kullanılabilir.

Bu makaledeki kod örnekleri ve aşağıdakiler [ASP.NET Web uygulaması örneğinden](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)ayıklanır. Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

# <a name="java"></a>[Java](#tab/java)

Bu makaledeki kod örnekleri ve aşağıdakiler, Java için MSAL kullanan bir web uygulaması örneği olan [Microsoft Graph(Java) adını veren Java web uygulamasından](https://github.com/Azure-Samples/ms-identity-java-webapp)çıkarılır.
Örnek şu anda Java için MSAL'ın yetkilendirme kodu URL'sini üretmesine ve microsoft kimlik platformunun yetkilendirme bitiş noktasına gezinmeyi işlemesine olanak tanır. Kullanıcıyı oturum etmek için Sprint güvenliğini kullanmak da mümkündür. Tam uygulama ayrıntıları için örneğe başvurmak isteyebilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Bu makaledeki kod örnekleri ve aşağıdakiler, MSAL kullanan bir web uygulaması örneği olan [Microsoft Graph(Microsoft Graph) olarak adlandıran Python web uygulamasından](https://github.com/Azure-Samples/ms-identity-python-webapp)çıkarılır. Python.
Örnek şu anda MSAL sağlar. Python yetkilendirme kodu URL'sini üretir ve gezinmeyi Microsoft kimlik platformuiçin yetkilendirme bitiş noktasına doğru işler. Tam uygulama ayrıntıları için örneğe başvurmak isteyebilirsiniz.

---

## <a name="code-that-redeems-the-authorization-code"></a>Yetkilendirme kodunu kurtaran kod

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

### <a name="startupcs"></a>Startup.cs

ASP.NET `Startup.cs` Core'da, dosyada `OnAuthorizationCodeReceived` OpenID Connect etkinliğine abone olabilirsiniz. Bu olaydan, MSAL.NET `AcquireTokenFromAuthorizationCode` yöntemini arayın. Bu yöntem, belirteç önbelleğinde aşağıdaki belirteçleri depolar:

- İstenen `scopes`için *erişim belirteci* .
- *Yenileme belirteci.* Bu belirteç, son kullanma tarihi yaklaştığında erişim belirteciyenilemek veya aynı kullanıcı adına ancak farklı bir kaynak için başka bir belirteç almak için kullanılır.

[ASP.NET Core Web uygulaması öğreticiweb](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) uygulamaları için yeniden kullanılabilir kod sağlar.

Aşağıdaki [Başlangıç.cs#L40-L42](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Startup.cs#L40-L42)kodudur. Bu aramalar özellikleri:

- Web `AddMicrosoftIdentityPlatformAuthentication` uygulamasına kimlik doğrulaması ekleyen yöntem.
- Web `AddMsal` API'lerini arama özelliğini ekleyen yöntem.
- Bir `AddInMemoryTokenCaches` belirteç önbellek uygulaması seçmekle ilgili yöntem.

```csharp
public class Startup
{
  // Code not shown here

  public void ConfigureServices(IServiceCollection services)
  {
      // Token acquisition service based on MSAL.NET
      // and chosen token-cache implementation
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
          .AddMsal(Configuration, new string[] { Constants.ScopeUserRead })
          .AddInMemoryTokenCaches();
  }

  // Code not shown here
}
```

`Constants.ScopeUserRead`[Constants.cs#L5](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/2-WebApp-graph-user/2-1-Call-MSGraph/Infrastructure/Constants.cs#L5)olarak tanımlanır :

```csharp
public static class Constants
{
    public const string ScopeUserRead = "User.Read";
}
```

Web uygulamasında `AddMicrosoftIdentityPlatformAuthentication` [kullanıcılarda işaretleyen](scenario-web-app-sign-user-app-configuration.md?tabs=aspnetcore#initialization-code)içeriği incelediniz - kod yapılandırması.

### <a name="the-addmsal-method"></a>AddMsal yöntemi

Kodu `AddMsal` [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L108-L159)bulunur.

```csharp

/// <summary>
/// Extensions for IServiceCollection for startup initialization
/// </summary>
public static class WebAppServiceCollectionExtensions
{
  // Code omitted here

  /// <summary>
  /// Add MSAL support to the web app or web API.
  /// </summary>
  /// <param name="services">Service collection to which to add authentication</param>
  /// <param name="initialScopes">Initial scopes to request at sign-in</param>
  /// <returns></returns>
  public static IServiceCollection AddMsal(this IServiceCollection services, IConfiguration configuration, IEnumerable<string> initialScopes, string configSectionName = "AzureAd")
  {
      // Ensure that configuration options for MSAL.NET, HttpContext accessor and the Token acquisition service
      // (encapsulating MSAL.NET) are available through dependency injection.
      services.Configure<ConfidentialClientApplicationOptions>(options => configuration.Bind(configSectionName, options));
      services.AddHttpContextAccessor();
      services.AddTokenAcquisition();

      services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
      {
          // Response type
          options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

          // This scope is needed to get a refresh token when users sign in by using their personal Microsoft accounts.
          // (It's required by MSAL.NET and automatically provided when users sign in by using work or school accounts.)
          options.Scope.Add("offline_access");
          if (initialScopes != null)
          {
              foreach (string scope in initialScopes)
              {
                  if (!options.Scope.Contains(scope))
                  {
                      options.Scope.Add(scope);
                  }
              }
          }

          // Handle the auth redemption by MSAL.NET so that a token is available in the token cache,
          // where it will be usable from controllers later (by means of the TokenAcquisition service).
          var handler = options.Events.OnAuthorizationCodeReceived;
          options.Events.OnAuthorizationCodeReceived = async context =>
          {
              var tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
              await tokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync(context, options.Scope).ConfigureAwait(false);
              await handler(context).ConfigureAwait(false);
          };
      });
      return services;
  }
}
```

Yöntem `AddMsal` şunları sağlar:

- ASP.NET Core web uygulaması hem kullanıcı için bir kimlik belirteci hem de kimlik doğrulama kodu (`options.ResponseType = OpenIdConnectResponseType.CodeIdToken`) ister.
- `offline_access` Kapsam eklenir. Bu kapsam, yenibir belirteç almak için uygulama için kullanıcı onayı alır.
- Uygulama OIDC `OnAuthorizationCodeReceived` etkinliğine abone olur ve burada yeniden kullanılabilir bir bileşen ekiolarak kapsüllenen MSAL.NET `ITokenAcquisition`kullanarak aramayı kullanır.

### <a name="the-tokenacquisitionaddaccounttocachefromauthorizationcodeasync-method"></a>TokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync yöntemi

Yöntem `TokenAcquisition.AddAccountToCacheFromAuthorizationCodeAsync` [Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L101-L145)bulunur. Bu sağlar:

- ASP.NET, kimlik doğrulama kodunu MSAL.NET paralel olarak`context.HandleCodeRedemption();`kullanmagirişiminde bulunmaz ( ).
- Kimlik belirtecindeki talepler, MSAL'ın kullanıcının hesabı için bir belirteç önbellek anahtarı hesaplaması için kullanılabilir.
- Gerekirse MSAL.NET uygulamanın bir örneği oluşturulur.
- Kod, MSAL.NET uygulaması tarafından itfa edilir.
- Yeni kimlik belirteci, `context.HandleCodeRedemption(null, result.IdToken);`''ye yapılan arama sırasında ASP.NET Core ile paylaşılır. Erişim belirteci ASP.NET Core ile paylaşılmaz. Kullanıcıyla ilişkili MSAL.NET belirteç önbelleğinde kalır ve burada ASP.NET Core denetleyicilerinde kullanılmaya hazırdır.

İşte ilgili `TokenAcquisition`kod:

```csharp
public class TokenAcquisition : ITokenAcquisition
{
  string[] scopesRequestedByMsalNet = new string[]{ "openid", "profile", "offline_access" };

  // Code omitted here for clarity


  public async Task AddAccountToCacheFromAuthorizationCodeAsync(AuthorizationCodeReceivedContext context, IEnumerable<string> scopes)
  {
   // Code omitted here for clarity

    try
    {
      // Because AcquireTokenByAuthorizationCodeAsync is asynchronous, we tell ASP.NET core that we're handing the code
      // even if it's not done yet, so that it doesn't concurrently call the token endpoint. Otherwise, there will be a
      // race condition that causes an Azure AD error message ("code already redeemed").
      context.HandleCodeRedemption();

      // The cache needs the claims from the ID token.
      // If they're not yet in the HttpContext.User's claims, add them here.
      if (!context.HttpContext.User.Claims.Any())
      {
          (context.HttpContext.User.Identity as ClaimsIdentity).AddClaims(context.Principal.Claims);
      }

      var application = GetOrBuildConfidentialClientApplication();

      // Don't share the access token with ASP.NET Core. If we share it, ASP.NET will cache it and won't send the OAuth 2.0 request if
      // a further call to AcquireTokenByAuthorizationCodeAsync is required later for incremental consent (getting a code requesting more scopes).
      // Do share the ID token, however.
      var result = await application
          .AcquireTokenByAuthorizationCode(scopes.Except(_scopesRequestedByMsalNet), context.ProtocolMessage.Code)
          .ExecuteAsync()
          .ConfigureAwait(false);

      context.HandleCodeRedemption(null, result.IdToken);
  }
  catch (MsalException ex)
  {
      Debug.WriteLine(ex.Message);
      throw;
  }
 }
```

### <a name="the-tokenacquisitionbuildconfidentialclientapplication-method"></a>TokenAcquisition.BuildConfidentialClientApplication yöntemi

ASP.NET Core'da, gizli istemci uygulamasını oluşturmak, `HttpContext`'deki bilgileri kullanır. İstekle `HttpContext` ilişkili `CurrentHttpContext` özellik kullanılarak erişilir. `HttpContext`web uygulamasının URL'si ve oturum açmış kullanıcı (bir' `ClaimsPrincipal`de) hakkında bilgi vardır. 

Yöntem `BuildConfidentialClientApplication` ayrıca ASP.NET Core yapılandırmasını da kullanır. Yapılandırmanın bir "AzureAD" bölümü vardır ve ayrıca aşağıdaki öğelerin her ikisine de bağlıdır:

- `_applicationOptions` [ConfidentialClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationoptions?view=azure-dotnet)türünün veri yapısı.
- ASP.NET `azureAdOptions` Core'da `Authentication.AzureAD.UI`tanımlanan [AzureAdOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/AzureADOptions.cs)türü örneği.

Son olarak, uygulama belirteç önbellekleri korumak gerekir. Bu konuda daha fazla bilgi sonraki bölümde öğreneceksiniz.

Yöntemin `GetOrBuildConfidentialClientApplication()` kodu [Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333'te](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L290-L333)dir. Bağımlılık enjeksiyonu ile enjekte edilen üyeleri kullanır `TokenAcquisition` [(Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59'da](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/4b12ba02e73f62e3e3137f5f4b9ef43cec7c14fd/Microsoft.Identity.Web/TokenAcquisition.cs#L47-L59)bulunan yapıda geçer).

İşte `GetOrBuildConfidentialClientApplication`kodu:

```csharp
public class TokenAcquisition : ITokenAcquisition
{
  // Code omitted here for clarity

  // Members
  private IConfidentialClientApplication application;
  private HttpContext CurrentHttpContext => _httpContextAccessor.HttpContext;

  // The following members are set by dependency injection in the TokenAcquisition constructor.
  private readonly AzureADOptions _azureAdOptions;
  private readonly ConfidentialClientApplicationOptions _applicationOptions;
  private readonly IMsalAppTokenCacheProvider _appTokenCacheProvider;
  private readonly IMsalUserTokenCacheProvider _userTokenCacheProvider;
  private readonly IHttpContextAccessor _httpContextAccessor;

  /// <summary>
  /// Creates an MSAL confidential client application, if needed.
  /// </summary>
  private IConfidentialClientApplication GetOrBuildConfidentialClientApplication()
  {
    if (application == null)
    {
        application = BuildConfidentialClientApplication();
    }
    return application;
  }

  /// <summary>
  /// Creates an MSAL Confidential client application
  /// </summary>
  /// <param name="claimsPrincipal"></param>
  /// <returns></returns>
  private IConfidentialClientApplication BuildConfidentialClientApplication()
  {
    var request = CurrentHttpContext.Request;
    var azureAdOptions = _azureAdOptions;
    var applicationOptions = _applicationOptions;
    string currentUri = UriHelper.BuildAbsolute(
        request.Scheme,
        request.Host,
        request.PathBase,
        azureAdOptions.CallbackPath ?? string.Empty);

    string authority = $"{applicationOptions.Instance}{applicationOptions.TenantId}/";

    var app = ConfidentialClientApplicationBuilder
        .CreateWithApplicationOptions(applicationOptions)
        .WithRedirectUri(currentUri)
        .WithAuthority(authority)
        .Build();

    // Initialize token cache providers
    _appTokenCacheProvider?.InitializeAsync(app.AppTokenCache);
    _userTokenCacheProvider?.InitializeAsync(app.UserTokenCache);

    return app;
  }

```

### <a name="summary"></a>Özet

`AcquireTokenByAuthorizationCode`gerçekten ASP.NET isteklerini ASP.NET ve kullanıcı belirteç önbelleğine eklenen belirteçleri alır MSAL.NET yöntemdir. Önbellekten belirteçler ASP.NET Çekirdek denetleyicilerinde kullanılır.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET, OpenID Connect yapılandırması ve `OnAuthorizationCodeReceived` etkinliğin aboneliğinin [App_Start\Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs) dosyasında gerçekleşmesi dışında, ASP.NET Core'a benzer şekilde işleri işler. Kavramlar da ASP.NET Core benzer, ASP.NET `RedirectUri` [web.config # L15](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/master/WebApp/Web.config#L15)belirtmeniz gerekir dışında . Bu yapılandırma, ASP.NET Core'dakinden biraz daha az sağlamdır, çünkü uygulamanızı dağıtırken değiştirmeniz gerekir.

İşte Startup.Auth.cs için kod:

```csharp
public partial class Startup
{
  public void ConfigureAuth(IAppBuilder app)
  {
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    // Custom middleware initialization. This is activated when the code obtained from a code_grant is present in the query string (&code=<code>).
    app.UseOAuth2CodeRedeemer(
        new OAuth2CodeRedeemerOptions
        {
            ClientId = AuthenticationConfig.ClientId,
            ClientSecret = AuthenticationConfig.ClientSecret,
            RedirectUri = AuthenticationConfig.RedirectUri
        }
      );

  app.UseOpenIdConnectAuthentication(
      new OpenIdConnectAuthenticationOptions
      {
        // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0.
        Authority = AuthenticationConfig.Authority,
        ClientId = AuthenticationConfig.ClientId,
        RedirectUri = AuthenticationConfig.RedirectUri,
        PostLogoutRedirectUri = AuthenticationConfig.RedirectUri,
        Scope = AuthenticationConfig.BasicSignInScopes + " Mail.Read", // A basic set of permissions for user sign-in and profile access "openid profile offline_access"
        TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = false,
            // In a real application, you would use IssuerValidator for additional checks, such as making sure the user's organization has signed up for your app.
            //     IssuerValidator = (issuer, token, tvp) =>
            //     {
            //        //if(MyCustomTenantValidation(issuer))
            //        return issuer;
            //        //else
            //        //    throw new SecurityTokenInvalidIssuerException("Invalid issuer");
            //    },
            //NameClaimType = "name",
        },
        Notifications = new OpenIdConnectAuthenticationNotifications()
        {
            AuthorizationCodeReceived = OnAuthorizationCodeReceived,
            AuthenticationFailed = OnAuthenticationFailed,
        }
      });
  }

  private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
  {
      // Upon successful sign-in, get the access token and cache it by using MSAL.
      IConfidentialClientApplication clientApp = MsalAppBuilder.BuildConfidentialClientApplication(new ClaimsPrincipal(context.AuthenticationTicket.Identity));
      AuthenticationResult result = await clientApp.AcquireTokenByAuthorizationCode(new[] { "Mail.Read" }, context.Code).ExecuteAsync();
  }

  private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
  {
      notification.HandleResponse();
      notification.Response.Redirect("/Error?message=" + notification.Exception.Message);
      return Task.FromResult(0);
  }
}
```

# <a name="java"></a>[Java](#tab/java)

[Kullanıcılarda işaretleyen Web uygulamasına bakın:](scenario-web-app-sign-user-app-configuration.md?tabs=java#initialization-code) Java örneğinin yetkilendirme kodunu nasıl aldığını anlamak için kod yapılandırması. Uygulama kodu aldıktan sonra [AuthFilter.java#L51-L56](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java#L51-L56):

1. `AuthHelper.processAuthenticationCodeRedirect` [AuthHelper.java#L67-L97'deki](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L67-L97)yönteme temsilciler.
1. Aramalar `getAuthResultByAuthCode`.

```Java
class AuthHelper {
  // Code omitted
  void processAuthenticationCodeRedirect(HttpServletRequest httpRequest, String currentUri, String fullUrl)
            throws Throwable {

  // Code omitted
  AuthenticationResponse authResponse = AuthenticationResponseParser.parse(new URI(fullUrl), params);

  // Code omitted
  IAuthenticationResult result = getAuthResultByAuthCode(
                    httpRequest,
                    oidcResponse.getAuthorizationCode(),
                    currentUri);

// Code omitted
  }
}
```

Yöntem `getAuthResultByAuthCode` [AuthHelper.java#L176'da](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L176)tanımlanmıştır. Bir MSAL `ConfidentialClientApplication`oluşturur ve ardından `acquireToken()` `AuthorizationCodeParameters` yetkilendirme kodundan oluşturulan çağrıları.

```Java
   private IAuthenticationResult getAuthResultByAuthCode(
            HttpServletRequest httpServletRequest,
            AuthorizationCode authorizationCode,
            String currentUri) throws Throwable {

        IAuthenticationResult result;
        ConfidentialClientApplication app;
        try {
            app = createClientApplication();

            String authCode = authorizationCode.getValue();
            AuthorizationCodeParameters parameters = AuthorizationCodeParameters.builder(
                    authCode,
                    new URI(currentUri)).
                    build();

            Future<IAuthenticationResult> future = app.acquireToken(parameters);

            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        }

        if (result == null) {
            throw new ServiceUnavailableException("authentication result was null");
        }

        SessionManagementHelper.storeTokenCacheInSession(httpServletRequest, app.tokenCache().serialize());

        return result;
    }

    private ConfidentialClientApplication createClientApplication() throws MalformedURLException {
        return ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(clientSecret)).
                authority(authority).
                build();
    }
```

# <a name="python"></a>[Python](#tab/python)

Yetkilendirme kodu akışı, web uygulamasında gösterildiği gibi [istenir ve kullanıcılarda işaretler: Kod yapılandırması.](scenario-web-app-sign-user-app-configuration.md?tabs=python#initialization-code) Kod daha sonra, `authorized` Flask'ın URL'den `/getAToken` yolladığı işlevden alınır. Bu kodun tam bağlamı için [app.py#L30-L44'e](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L30-L44) bakın:

```python
 @app.route("/getAToken")  # Its absolute URL must match your app's redirect_uri set in AAD.
def authorized():
    if request.args['state'] != session.get("state"):
        return redirect(url_for("login"))
    cache = _load_cache()
    result = _build_msal_app(cache).acquire_token_by_authorization_code(
        request.args['code'],
        scopes=app_config.SCOPE,  # Misspelled scope would cause an HTTP 400 error here.
        redirect_uri=url_for("authorized", _external=True))
    if "error" in result:
        return "Login failure: %s, %s" % (
            result["error"], result.get("error_description"))
    session["user"] = result.get("id_token_claims")
    _save_cache(cache)
    return redirect(url_for("index"))
```

---

Gizli istemci uygulaması, istemci sırrı yerine, istemci sertifikası veya istemci iddiası kullanarak kimliğini de kanıtlayabilir.
İstemci iddialarının kullanımı, [İstemci iddialarında](msal-net-client-assertions.md)ayrıntılı olarak ayrıntılı olarak kullanılan gelişmiş bir senaryodur.

## <a name="token-cache"></a>Belirteç önbelleği

> [!IMPORTANT]
> Web uygulamaları veya web API'leri için belirteç önbellek uygulaması, genellikle [dosya tabanlı](scenario-desktop-acquire-token.md#file-based-token-cache)olan masaüstü uygulamaları nın uygulamasından farklıdır.
> Güvenlik ve performans nedenleriyle, web uygulamaları ve web API'leri için kullanıcı hesabı başına bir belirteç önbelleği olduğundan emin olmak önemlidir. Her hesap için belirteç önbelleğini seri hale vermeniz gerekir.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET temel öğretici, uygulamanız için Startup.cs dosyasındaki belirteç önbelleği uygulamasına karar vermenizi sağlamak için bağımlılık enjeksiyonunu kullanır. Microsoft.Identity.Web, [Token cache serialization'da](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/README.md#token-cache-serialization)açıklanan önceden oluşturulmuş belirteç-önbellek seriizatörleriyle birlikte gelir. İlginç bir olasılık Core [dağıtılmış bellek önbellekleri](https://docs.microsoft.com/aspnet/core/performance/caching/distributed#distributed-memory-cache)ASP.NET seçmektir:

```csharp
// Use a distributed token cache by adding:
    services.AddMicrosoftIdentityPlatformAuthentication(Configuration)
            .AddMsal(new string[] { scopesToRequest })
            .AddDistributedTokenCaches();

// Then, choose your implementation.

// For instance, the distributed in-memory cache (not cleared when you stop the app):
services.AddDistributedMemoryCache()

// Or a Redis cache:
services.AddStackExchangeRedisCache(options =>
{
 options.Configuration = "localhost";
 options.InstanceName = "SampleInstance";
});

// Or even a SQL Server token cache:
services.AddDistributedSqlServerCache(options =>
{
 options.ConnectionString = _config["DistCache_ConnectionString"];
 options.SchemaName = "dbo";
 options.TableName = "TestCache";
});
```

Belirteç önbellek sağlayıcıları hakkında ayrıntılı bilgi için, Core Web uygulaması ASP.NET | [ Öğreticinin belirteç önbellek](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache) leri aşamasıdır.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Web uygulamaları veya web API'leri için belirteç önbellek uygulaması, genellikle [dosya tabanlı](scenario-desktop-acquire-token.md#file-based-token-cache)olan masaüstü uygulamaları nın uygulamasından farklıdır.

Web uygulaması uygulaması ASP.NET oturumunu veya sunucu belleği kullanabilir. Örneğin, [MsalAppBuilder.cs#L39-L51'deki](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Utils/MsalAppBuilder.cs#L39-L51)MSAL.NET uygulaması oluşturulduktan sonra önbellek uygulamasının nasıl bağdaştırılan olduğunu görün:

```csharp
public static class MsalAppBuilder
{
 // Omitted code
    public static IConfidentialClientApplication BuildConfidentialClientApplication(ClaimsPrincipal currentUser)
    {
      IConfidentialClientApplication clientapp = ConfidentialClientApplicationBuilder.Create(AuthenticationConfig.ClientId)
            .WithClientSecret(AuthenticationConfig.ClientSecret)
            .WithRedirectUri(AuthenticationConfig.RedirectUri)
            .WithAuthority(new Uri(AuthenticationConfig.Authority))
            .Build();

      // After the ConfidentialClientApplication is created, we overwrite its default UserTokenCache with our implementation.
      MSALPerUserMemoryTokenCache userTokenCache = new MSALPerUserMemoryTokenCache(clientapp.UserTokenCache, currentUser ?? ClaimsPrincipal.Current);

      return clientapp;
  }
```

# <a name="java"></a>[Java](#tab/java)

MSAL Java belirteç önbelleğini seri hale getirmek ve deserialize etmek için yöntemler sağlar. Java `getAuthResultBySilentFlow` [örneği, AuthHelper.java#L99-L122'deki](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/AuthHelper.java#L99-L122)yöntemde gösterildiği gibi, oturumdaki serileştirmeyi işler:

```Java
IAuthenticationResult getAuthResultBySilentFlow(HttpServletRequest httpRequest, HttpServletResponse httpResponse)
      throws Throwable {

  IAuthenticationResult result =  SessionManagementHelper.getAuthSessionObject(httpRequest);

  IConfidentialClientApplication app = createClientApplication();

  Object tokenCache = httpRequest.getSession().getAttribute("token_cache");
  if (tokenCache != null) {
      app.tokenCache().deserialize(tokenCache.toString());
  }

  SilentParameters parameters = SilentParameters.builder(
          Collections.singleton("User.Read"),
          result.account()).build();

  CompletableFuture<IAuthenticationResult> future = app.acquireTokenSilently(parameters);
  IAuthenticationResult updatedResult = future.get();

  // Update session with latest token cache.
  SessionManagementHelper.storeTokenCacheInSession(httpRequest, app.tokenCache().serialize());

  return updatedResult;
}
```

Sınıfın ayrıntıları [Java için MSAL örnek](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/java/com/microsoft/azure/msalwebsample/SessionManagementHelper.java)sağlanmaktadır. `SessionManagementHelper`

# <a name="python"></a>[Python](#tab/python)

Python örneğinde, her istek için gizli bir istemci uygulaması yeniden oluşturularak ve ardından Flask oturum önbelleğinde serihale edilerek hesap başına bir önbellek sağlanır:

```python
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)

# Code omitted here for simplicity

def _load_cache():
    cache = msal.SerializableTokenCache()
    if session.get("token_cache"):
        cache.deserialize(session["token_cache"])
    return cache

def _save_cache(cache):
    if cache.has_state_changed:
        session["token_cache"] = cache.serialize()

def _build_msal_app(cache=None):
    return msal.ConfidentialClientApplication(
        app_config.CLIENT_ID, authority=app_config.AUTHORITY,
        client_credential=app_config.CLIENT_SECRET, token_cache=cache)
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, kullanıcı kaydettiğinde, belirteç önbelleğinde depolanır. Web uygulamasının diğer bölümlerinde nasıl kullanıldığını görelim.

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web uygulaması: Hesapları genel oturum açma önbelleğinden kaldırın](scenario-web-app-call-api-sign-in.md)
