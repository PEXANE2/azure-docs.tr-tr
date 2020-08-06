---
title: Web API 'Lerini çağıran bir Web API 'SI yapılandırma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini (uygulamanın kod yapılandırması) çağıran bir Web API 'SI oluşturmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/05/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 95cb1181f841ce5f958b8a85697d7261f442b410
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87799608"
---
# <a name="a-web-api-that-calls-web-apis-code-configuration"></a>Web API 'Leri çağıran bir Web API 'SI: kod yapılandırması

Web API 'nizi kaydettikten sonra, uygulamanın kodunu yapılandırabilirsiniz.

Web API 'nizi, bir Web API 'sini korumak için kullanılan kodun üstüne, akış Web API 'Lerini çağıran şekilde yapılandırmak için kullandığınız kod. Daha fazla bilgi için bkz. [korumalı Web API 'si: uygulama yapılandırması](scenario-protected-web-api-app-configuration.md).

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

## <a name="client-secrets-or-client-certificates"></a>İstemci parolaları veya istemci sertifikaları

Web API 'niz artık bir aşağı akış Web API 'sini çağırırsa, dosyadaki *appsettings.js* bir istemci parolası veya istemci sertifikası sağlamanız gerekir.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"
  
   // To call an API
   "ClientSecret": "[Copy the client secret added to the app from the Azure portal]",
   "ClientCertificates": [
  ]
 }
}
```

İstemci parolası yerine bir istemci sertifikası sağlayabilirsiniz. Aşağıdaki kod parçacığı, Azure Key Vault depolanan bir sertifikanın kullanımını gösterir.

```JSON
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    "TenantId": "common"
  
   // To call an API
   "ClientCertificates": [
      {
        "SourceType": "KeyVault",
        "KeyVaultUrl": "https://msidentitywebsamples.vault.azure.net",
        "KeyVaultCertificateName": "MicrosoftIdentitySamplesCert"
      }
  ]
 }
}
```

Microsoft. Identity. Web, sertifikaları yapılandırma veya koda göre tanımlamaya yönelik çeşitli yollar sağlar. Ayrıntılar için bkz. [Microsoft. Identity. Web wiki-GitHub 'da sertifikaları kullanma](https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates) .

## <a name="startupcs"></a>Startup.cs

Web API 'nizin aşağı akış Web API 'Lerini aramasını istiyorsanız Microsoft. Identity. Web 'i kullanarak `.AddMicrosoftWebApiCallsWebApi()` satırı sonra ekleyin `.AddMicrosoftWebApiAuthentication(Configuration)` ve ardından bir belirteç önbelleği uygulamasını seçin, örneğin `.AddInMemoryTokenCaches()` , *Startup.cs*:

```csharp
using Microsoft.Identity.Web;

public class Startup
{
  ...
  public void ConfigureServices(IServiceCollection services)
  {
   // ...
   services.AddMicrosoftWebApiAuthentication(Configuration)
           .AddMicrosoftWebApiCallsWebApi(Configuration)
           .AddInMemoryTokenCaches();
  // ...
  }
  // ...
}
```

Web uygulamalarında olduğu gibi, çeşitli belirteç önbelleği uygulamalarını seçebilirsiniz. Ayrıntılar için bkz. GitHub 'da [Microsoft Identity Web wiki-belirteç önbelleği serileştirme](https://aka.ms/ms-id-web/token-cache-serialization) .

Web API 'nizin belirli kapsamlara ihtiyaç duymasını istiyorsanız, isteğe bağlı olarak bunları ' ye bağımsız değişken olarak geçirebilirsiniz `AddMicrosoftWebApiCallsWebApi` .

# <a name="java"></a>[Java](#tab/java)

Şirket adına (OBO) akışı, aşağı akış Web API 'sini çağırmak için bir belirteç almak üzere kullanılır. Bu akışta, Web API 'niz, istemci uygulamasından Kullanıcı temsilcisi izinleri olan bir taşıyıcı belirteç alır ve ardından bu belirteci başka bir erişim belirteci için aşağı akış Web API 'sini çağırmak üzere değiş tokuş eder.

Aşağıdaki kod, `SecurityContextHolder` doğrulanan taşıyıcı belirtecini almak için Web API 'Sindeki Spring Security Framework 'ü kullanır. Daha sonra, ile çağrısını kullanarak bir aşağı akış API belirteci almak için MSAL Java kitaplığını kullanır `acquireToken` `OnBehalfOfParameters` . MSAL, API 'ye yapılan sonraki çağrıların `acquireTokenSilently` önbelleğe alınmış belirteci almak için kullanabilmesi için belirteci önbelleğe alır.

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

Şirket adına (OBO) akışı, aşağı akış Web API 'sini çağırmak için bir belirteç almak üzere kullanılır. Bu akışta, Web API 'niz, istemci uygulamasından Kullanıcı temsilcisi izinleri olan bir taşıyıcı belirteç alır ve ardından bu belirteci başka bir erişim belirteci için aşağı akış Web API 'sini çağırmak üzere değiş tokuş eder.

Bir Python Web API 'sinin istemciden alınan taşıyıcı belirtecini doğrulamak için bazı ara yazılım kullanması gerekir. Web API 'si daha sonra yöntemi çağırarak MSAL Python kitaplığı kullanarak aşağı akış API 'sine yönelik erişim belirtecini alabilir [`acquire_token_on_behalf_of`](https://msal-python.readthedocs.io/en/latest/?badge=latest#msal.ConfidentialClientApplication.acquire_token_on_behalf_of) . Bu API 'nin kullanımıyla ilgili bir örnek için bkz. [GitHub 'da Microsoft-Authentication-Library-for-Python için test kodu](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.2.0/tests/test_e2e.py#L429-L472). Ayrıca, orta katman uygulama ihtiyacını atlayan bir yaklaşım için aynı depodaki [53 sorun](https://github.com/AzureAD/microsoft-authentication-library-for-python/issues/53) hakkındaki tartışmaya bakın.

---

Ayrıca, [Node.js ve Azure işlevlerinde](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions/blob/master/MiddleTierAPI/MyHttpTrigger/index.js#L61)OBO akış uygulamasına bir örnek de görebilirsiniz.

## <a name="protocol"></a>Protokol

OBO protokolü hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0-adına akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran bir Web API 'SI: uygulama için belirteç alma](scenario-web-api-call-api-acquire-token.md)
