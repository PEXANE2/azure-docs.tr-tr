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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e54e26bba1618a3b5835480ed1b1fe698bc8db4
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043421"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web API 'Leri çağıran bir Web API 'SI: kod yapılandırması

Web API 'nizi kaydettikten sonra, uygulamanın kodunu yapılandırabilirsiniz.

Web API 'nizi, bir Web API 'sini korumak için kullanılan kodun üstüne, akış Web API 'Lerini çağıran şekilde yapılandırmak için kullandığınız kod. Daha fazla bilgi için bkz. [korumalı Web API 'si: uygulama yapılandırması](scenario-protected-web-api-app-configuration.md).

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

Ayrıca, [Node. js ve Azure işlevlerinde](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61)OBO akış uygulamasının bir örneğini görebilirsiniz.

## <a name="protocol"></a>Protokol

OBO protokolü hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0-adına akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma](scenario-web-api-call-api-acquire-token.md)
