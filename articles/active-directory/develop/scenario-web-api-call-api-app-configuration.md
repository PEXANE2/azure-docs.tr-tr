---
title: Web API 'Lerini çağıran bir Web API 'SI yapılandırma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir Web API 'SI oluşturmayı öğrenin
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
ms.openlocfilehash: 82b5e1d9753fbb65fd81f24b06016d302457144e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76834102"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web API 'Leri çağıran bir Web API 'SI: kod yapılandırması

Web API 'nizi kaydettikten sonra, uygulamanın kodunu yapılandırabilirsiniz.

Web API 'nizi, bir Web API 'sini korumak için kullanılan kodun üstüne, akış Web API 'Lerini çağıran şekilde yapılandırmak için kullandığınız kod. Daha fazla bilgi için bkz. [korumalı Web API 'si: uygulama yapılandırması](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="code-subscribed-to-ontokenvalidated"></a>Ontokendoğrulamalı 'e abone olunan kod

Korunan Web API 'Leri için kod yapılandırmasının üstünde, API 'niz çağrıldığında aldığınız taşıyıcı belirtecinin doğrulanmasına abone olmanız gerekir:

```csharp
/// <summary>
/// Protects the web API with the Microsoft identity platform, or Azure Active Directory (Azure AD) developer platform
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">The service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it
        // to the MSAL.NET cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache and handles the incremental consent
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>Adına akış

AddAccountToCacheFromJwt () yöntemi şunları gerektirir:

- Bir Microsoft kimlik doğrulama kitaplığı (MSAL) gizli istemci uygulaması örneğini oluşturun.
- Çağrı `AcquireTokenOnBehalf` yöntemi. Bu çağrı, istemci tarafından, Web API 'si için aynı kullanıcı için bir taşıyıcı belirtece karşı alınan taşıyıcı belirtecini, ancak API 'nin bir aşağı akış API 'sini çağırmasını içerir.

### <a name="instantiate-a-confidential-client-application"></a>Gizli bir istemci uygulaması örneği oluşturma

Bu akış yalnızca gizli istemci akışında kullanılabilir, böylece korumalı Web API 'SI, `WithClientSecret` veya `WithCertificate` yöntemi aracılığıyla [ConfidentialClientApplicationBuilder sınıfına](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) istemci kimlik bilgileri (istemci parolası veya sertifika) sağlar.

![IConfidentialClientApplication yöntemlerinin listesi](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```csharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

Son olarak, bir istemci gizli dizisi veya bir sertifika aracılığıyla kimliklerini sağlamak yerine, gizli istemci uygulamaları, istemci onaylamalarını kullanarak kimliklerini kanıtlayabilirler.
Bu gelişmiş senaryo hakkında daha fazla bilgi için bkz. [Gizli istemci onayları](msal-net-client-assertions.md).

### <a name="how-to-call-on-behalf-of"></a>-Adına çağırma

`IConfidentialClientApplication` arabiriminde [Acquiretokenonby metodunu](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) çağırarak, ADıNA (OBO) çağrısını yaparsınız.

`UserAssertion` sınıfı, kendi istemcilerinden Web API 'SI tarafından alınan taşıyıcı belirtecinden oluşturulur. [İki Oluşturucu](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet)vardır:
* Bir JSON Web Token (JWT) taşıyıcı belirteci alan bir
* Her türlü Kullanıcı onayını alan bir tür güvenlik belirteci, daha sonra türü adlı ek bir parametrede belirtilen başka bir tür güvenlik belirteci alır `assertionType`

![Kullanıcı onaylama özellikleri ve yöntemleri](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

Pratikte OBO akışı, genellikle bir aşağı akış API 'SI için belirteç almak ve MSAL.NET Kullanıcı belirteci önbelleğinde depolamak için kullanılır. Bunu, Web API 'sinin diğer bölümlerinin daha sonra aşağı akış API 'Lerini çağırmak için ``AcquireTokenOnSilent`` [Geçersiz Kılmalara](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) çağırabilmesi için yapabilirsiniz. Bu çağrının, gerekirse belirteçleri yenileme etkisi vardır.

```csharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
    try
    {
        UserAssertion userAssertion;
        IEnumerable<string> requestedScopes;
        if (jwtToken != null)
        {
            userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
            requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
        }
        else
        {
            throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
        }

        // Create the application
        var application = BuildConfidentialClientApplication(httpContext, principal);

        // .Result to make sure that the cache is filled in before the controller tries to get access tokens
        var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                        userAssertion)
                                .ExecuteAsync()
                                .GetAwaiter().GetResult();
     }
     catch (MsalException ex)
     {
         Debug.WriteLine(ex.Message);
         throw;
     }
}
```
# <a name="javatabjava"></a>[Java](#tab/java)

Şirket adına (OBO) akışı, aşağı akış Web API 'sini çağırmak için bir belirteç almak üzere kullanılır. Bu akışta, Web API 'niz, istemci uygulamasından Kullanıcı temsilcisi izinleri olan bir taşıyıcı belirteç alır ve ardından bu belirteci başka bir erişim belirteci için aşağı akış Web API 'sini çağırmak üzere değiş tokuş eder.

Aşağıdaki kod, doğrulanan taşıyıcı belirtecini almak için Web API 'sindeki Spring Security Framework `SecurityContextHolder` kullanır. Daha sonra, `OnBehalfOfParameters``acquireToken` çağrısını kullanarak aşağı akış API 'SI için bir belirteç almak üzere MSAL Java kitaplığını kullanır. MSAL, API 'ye yapılan sonraki çağrıların önbelleğe alınmış belirteci almak için `acquireTokenSilently` kullanabilmesi için belirteci önbelleğe alır.

```Java
@Component
class MsalAuthHelper {

    @Value("${security.oauth2.client.authority}")
    private String authority;

    @Value("${security.oauth2.client.client-id}")
    private String clientId;

    @Value("${security.oauth2.client.client-secret}")
    private String secret;

    @Autowired
    CacheManager cacheManager;

    private String getAuthToken(){
        String res = null;
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();

        if(authentication != null){
            res = ((OAuth2AuthenticationDetails) authentication.getDetails()).getTokenValue();
        }
        return res;
    }

    String getOboToken(String scope) throws MalformedURLException {
        String authToken = getAuthToken();

        ConfidentialClientApplication application =
                ConfidentialClientApplication.builder(clientId, ClientCredentialFactory.create(secret))
                        .authority(authority).build();

        String cacheKey = Hashing.sha256()
                .hashString(authToken, StandardCharsets.UTF_8).toString();

        String cachedTokens = cacheManager.getCache("tokens").get(cacheKey, String.class);
        if(cachedTokens != null){
            application.tokenCache().deserialize(cachedTokens);
        }

        IAuthenticationResult auth;
        SilentParameters silentParameters =
                SilentParameters.builder(Collections.singleton(scope))
                        .build();
        auth = application.acquireTokenSilently(silentParameters).join();

        if (auth == null){
            OnBehalfOfParameters parameters =
                    OnBehalfOfParameters.builder(Collections.singleton(scope),
                            new UserAssertion(authToken))
                            .build();

            auth = application.acquireToken(parameters).join();
        }

        cacheManager.getCache("tokens").put(cacheKey, application.tokenCache().serialize());

        return auth.accessToken();
    }
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Şirket adına (OBO) akışı, aşağı akış Web API 'sini çağırmak için bir belirteç almak üzere kullanılır. Bu akışta, Web API 'niz, istemci uygulamasından Kullanıcı temsilcisi izinleri olan bir taşıyıcı belirteç alır ve ardından bu belirteci başka bir erişim belirteci için aşağı akış Web API 'sini çağırmak üzere değiş tokuş eder.

Bir Python Web API 'sinin istemciden alınan taşıyıcı belirtecini doğrulamak için bazı ara yazılım kullanması gerekir. Web API 'si daha sonra [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) YÖNTEMINI çağırarak msal Python kitaplığı kullanarak aşağı akış API 'sine yönelik erişim belirtecini alabilir. Bu akışı MSAL Python ile gösteren bir örnek henüz kullanılamamaktadır.

---

Ayrıca, [Node. js ve Azure işlevlerinde](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61)OBO akış uygulamasının bir örneğini görebilirsiniz.

## <a name="protocol"></a>Protokol

OBO protokolü hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0-adına akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma](scenario-web-api-call-api-acquire-token.md)
