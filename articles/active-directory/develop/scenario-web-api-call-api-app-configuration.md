---
title: Web API'lerini çağıran bir web API'si yapılandırın | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini (uygulamanın kod yapılandırması) çağıran bir web API'sini nasıl oluşturacağa öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 38e319efb100d326d55f6f821e7c903306a7c7d0
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991016"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web API'lerini çağıran bir web API'sı: Kod yapılandırması

Web API'nizi kaydettikten sonra, uygulamanın kodunu yapılandırabilirsiniz.

Web API'nizi alt akış web API'lerini çağıracak şekilde yapılandırmak için kullandığınız kod, web API'sini korumak için kullanılan kodun üzerine oluşturur. Daha fazla bilgi için [Bkz. Korumalı web API: Uygulama yapılandırması.](scenario-protected-web-api-app-configuration.md)

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="code-subscribed-to-ontokenvalidated"></a>OnTokenValidated'a abone olan kod

Korumalı web API'leri için kod yapılandırmasının üstüne, API'niz çağrıldığında aldığınız taşıyıcı belirteci doğrulamasına abone olmanız gerekir:

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

## <a name="on-behalf-of-flow"></a>On-Behalf-Akış

AddAccountToCacheFromJwt() yöntemi nin şunları yapması gerekir:

- Microsoft Kimlik Doğrulama Kitaplığı (MSAL) gizli istemci uygulamasını anında aygıttır.
- `AcquireTokenOnBehalf` Yöntemi ara. Bu çağrı, aynı kullanıcı için bir taşıyıcı belirteci karşı web API için istemci tarafından satın alınan taşıyıcı belirteci değiştirir, ancak API bir downstream API çağrı vardır.

### <a name="instantiate-a-confidential-client-application"></a>Gizli bir istemci uygulamasını anında

Bu akış yalnızca gizli istemci akışında kullanılabilir, böylece korunan web API'sı gizli müşteri kimlik bilgilerini (istemci `WithClientSecret` `WithCertificate` sırrı veya sertifika) [ConfidentialClientApplicationBuilder sınıfına](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) ya da yöntemle sağlar.

![IConfidentialClientApplication yöntemleri listesi](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

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

Son olarak, gizli istemci uygulamaları, bir istemci sırrı veya sertifika yoluyla kimliklerini kanıtlamak yerine, istemci iddialarını kullanarak kimliklerini kanıtlayabilir.
Bu gelişmiş senaryo hakkında daha fazla bilgi için [Bkz. Gizli istemci iddiaları.](msal-net-client-assertions.md)

### <a name="how-to-call-on-behalf-of"></a>On-Behalf-Of nasıl çağrılır

On-Behalf-Of (OBO) aramasını, `IConfidentialClientApplication` arayüz deki [AcquireTokenOnBehalf yöntemini](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) arayarak yaparsınız.

Sınıf, `UserAssertion` web API'si tarafından kendi istemcilerinden alınan taşıyıcı belirtecinden oluşturulur. İki [yapıcı](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet)vardır:
* Bir JSON Web Belirteci (JWT) taşıyıcı belirteci alır
* Her türlü kullanıcı iddiasını alan, türü daha sonra adı verilen ek bir parametrede belirtilen başka bir tür güvenlik belirteci`assertionType`

![UserAssertion özellikleri ve yöntemleri](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

Uygulamada, OBO akışı genellikle bir akış aşağı API için bir belirteç elde etmek ve kullanıcı belirteç önbelleğinde MSAL.NET depolamak için kullanılır. Bunu, web API'sinin diğer bölümlerinin daha sonra akış ``AcquireTokenOnSilent`` aşağı API'leri aramayı geçersiz [kılanları](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) çağırabilmesi için yaparsınız. Bu çağrı, gerekirse belirteçleri yenileme etkisi vardır.

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
# <a name="java"></a>[Java](#tab/java)

On-adına (OBO) akışı downstream web API aramak için bir belirteç elde etmek için kullanılır. Bu akışta, web API'niz istemci uygulamasından kullanıcı tarafından yetkialan izinleri içeren bir taşıyıcı belirteci alır ve ardından bu belirteci downstream web API'yi aramak için başka bir erişim belirteciyle değiştirir.

Aşağıdaki kod, doğrulanmış taşıyıcı `SecurityContextHolder` belirteci almak için web API'sında Bahar Güvenlik çerçevesini kullanır. Daha sonra ile `acquireToken` arama kullanarak downstream API için bir belirteç elde etmek için MSAL Java kitaplığı `OnBehalfOfParameters`kullanır. MSAL belirteci önbelleğe alır, böylece API sonraki aramalar önbelleğe alınmış belirteci almak için kullanabilirsiniz. `acquireTokenSilently`

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

# <a name="python"></a>[Python](#tab/python)

On-adına (OBO) akışı downstream web API aramak için bir belirteç elde etmek için kullanılır. Bu akışta, web API'niz istemci uygulamasından kullanıcı tarafından yetkialan izinleri içeren bir taşıyıcı belirteci alır ve ardından bu belirteci downstream web API'yi aramak için başka bir erişim belirteciyle değiştirir.

Python web API'sinin istemciden alınan taşıyıcı belirteci doğrulamak için bazı ara yazılımlar kullanması gerekir. Web API'si daha sonra yöntemi arayarak MSAL Python kitaplığını [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) kullanarak akış aşağı API'ye erişim belirteci elde edebilir. Bu API'yi kullanma örneği için, [GitHub'daki microsoft kimlik doğrulama-kitaplık-for-python test koduna](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.2.0/tests/test_e2e.py#L429-L472)bakın. Ayrıca, orta katman uygulaması gereksinimini atlayan bir yaklaşım için aynı depodaki [53.](https://github.com/AzureAD/microsoft-authentication-library-for-python/issues/53)

---

[Ayrıca Düğüm.js ve Azure İşlevleri'nde](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61)OBO akış uygulamasının bir örneğini de görebilirsiniz.

## <a name="protocol"></a>Protokol

OBO protokolü hakkında daha fazla bilgi için [Microsoft kimlik platformu ve OAuth 2.0 On-Behalf-Of akışına](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'lerini çağıran bir web API'sı: Uygulama için bir belirteç edinme](scenario-web-api-call-api-acquire-token.md)
