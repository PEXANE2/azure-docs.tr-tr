---
title: Web API (daemon uygulaması) aramak için belirteçler edinin - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir daemon uygulaması oluşturmayı öğrenin (jeton edinme)
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7f1010949a72f95ef2836c43666e6cea9281e04d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262652"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Web API'leri çağıran Daemon uygulaması - bir belirteç edinin

Gizli bir istemci uygulaması yaptıktan sonra, arama `AcquireTokenForClient`yaparak, kapsamı geçerek ve isteğe bağlı olarak belirteçyenilemeyi zorlayarak uygulama için bir belirteç elde edebilirsiniz.

## <a name="scopes-to-request"></a>İstenecek kapsamlar

İstemci kimlik bilgisi akışı için talep edilen kapsam, izleyen `/.default`kaynağın adıdır. Bu gösterim, Azure Etkin Dizini'ne (Azure AD) uygulama kaydı sırasında statik olarak bildirilen *uygulama düzeyi izinlerini* kullanmasını söyler. Ayrıca, bu API izinleri bir kiracı yönetici tarafından verilmelidir.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

# <a name="python"></a>[Python](#tab/python)

MSAL Python'da yapılandırma dosyası şu kod parçacığına benzer:

```Json
{
    "scope": ["https://graph.microsoft.com/.default"],
}
```

# <a name="java"></a>[Java](#tab/java)

```Java
final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
```

---

### <a name="azure-ad-v10-resources"></a>Azure AD (v1.0) kaynakları

İstemci kimlik bilgileri için kullanılan kapsam `/.default`her zaman kaynak kimliği olmalıdır.

> [!IMPORTANT]
> MSAL sürüm 1.0 erişim belirteci kabul eden bir kaynak için bir erişim belirteci istediğinde, Azure AD, son eğik çizgiden önce her şeyi alarak ve kaynak tanımlayıcısı olarak kullanarak istenen hedef kitleyi istenen kapsamdan ayrıştırır.
> Bu nedenle, Azure SQL Veritabanı gibi **(https:\//database.windows.net),** kaynak bir eğik çizgi `https://database.windows.net/`yle biten bir hedef kitle `https://database.windows.net//.default`bekliyorsa (Azure SQL Veritabanı için), bir kapsam istemeniz gerekir. (Çift eğik çizgiye dikkat edin.) Ayrıca MSAL.NET sorunu [#747 bakınız: Kaynak url's izleme eğik çizgi atlanır, hangi sql auth hatasına neden oldu.](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747)

## <a name="acquiretokenforclient-api"></a>Satın AlmaTokenForClient API

Uygulama için bir belirteç elde etmek `AcquireTokenForClient` için, platforma bağlı olarak uygulamanın eşdeğerini veya eşdeğerini kullanırsınız.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
using Microsoft.Identity.Client;

// With client credentials flows, the scope is always of the shape "resource/.default" because the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator.
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application doesn't have sufficient permissions.
    // - Did you declare enough app permissions during app creation?
    // - Did the tenant admin grant permissions to the application?
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be in the form "https://resourceurl/.default"
    // Mitigation: Change the scope to be as expected.
}
```

# <a name="python"></a>[Python](#tab/python)

```Python
# The pattern to acquire a token looks like this.
result = None

# First, the code looks up a token from the cache.
# Because we're looking for a token for the current app, not for a user,
# use None for the account parameter.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])

if "access_token" in result:
    # Call a protected API with the access token.
    print(result["token_type"])
else:
    print(result.get("error"))
    print(result.get("error_description"))
    print(result.get("correlation_id"))  # You might need this when reporting a bug.
```

# <a name="java"></a>[Java](#tab/java)

Bu kod [MSAL Java geliştirme örneklerinden](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/)ayıklanır.

```Java
private static IAuthenticationResult acquireToken() throws Exception {

     // Load token cache from file and initialize token cache aspect. The token cache will have
     // dummy data, so the acquireTokenSilently call will fail.
     TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

     IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);
     ConfidentialClientApplication cca =
             ConfidentialClientApplication
                     .builder(CLIENT_ID, credential)
                     .authority(AUTHORITY)
                     .setTokenCacheAccessAspect(tokenCacheAspect)
                     .build();

     IAuthenticationResult result;
     try {
         SilentParameters silentParameters =
                 SilentParameters
                         .builder(SCOPE)
                         .build();

         // try to acquire token silently. This call will fail since the token cache does not
         // have a token for the application you are requesting an access token for
         result = cca.acquireTokenSilently(silentParameters).join();
     } catch (Exception ex) {
         if (ex.getCause() instanceof MsalException) {

             ClientCredentialParameters parameters =
                     ClientCredentialParameters
                             .builder(SCOPE)
                             .build();

             // Try to acquire a token. If successful, you should see
             // the token information printed out to console
             result = cca.acquireToken(parameters).join();
         } else {
             // Handle other exceptions accordingly
             throw ex;
         }
     }
     return result;
 }
```

---

### <a name="protocol"></a>Protokol

Seçtiğiniz dil için henüz bir kitaplığınız yoksa, protokolü doğrudan kullanmak isteyebilirsiniz:

#### <a name="first-case-access-the-token-request-by-using-a-shared-secret"></a>İlk durum: Paylaşılan bir sırrı kullanarak belirteç isteğine erişin

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity.
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-the-token-request-by-using-a-certificate"></a>İkinci durum: Bir sertifika kullanarak belirteç isteğine erişin

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity.
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

Daha fazla bilgi için protokol belgelerine bakın: [Microsoft kimlik platformu ve OAuth 2.0 istemci kimlik bilgileri akışı.](v2-oauth2-client-creds-grant-flow.md)

## <a name="application-token-cache"></a>Uygulama belirteç önbelleği

MSAL.NET, `AcquireTokenForClient` uygulama belirteç önbelleğini kullanır. (Diğer tüm AcquireToken*XX* yöntemleri kullanıcı belirteç önbelleğini kullanır.) Kullanıcı *belirteç* önbelleğini `AcquireTokenSilent` kullandığından, aramadan `AcquireTokenSilent` `AcquireTokenForClient`önce aramayın. `AcquireTokenForClient`*uygulama* belirteç önbelleğinin kendisini denetler ve güncelleştirir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="did-you-use-the-resourcedefault-scope"></a>Kaynak/.varsayılan kapsamı kullandınız mı?

Geçersiz bir kapsam kullandığınızı belirten bir hata iletisi `resource/.default` alırsanız, büyük olasılıkla kapsamı kullanmamışsınızdır.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Yönetici onayı vermeyi unuttun mu? Daemon uygulamaları gerekir!

API'yi aradiğinizde **işlem hatasını tamamlamak için yetersiz ayrıcalıklar** alırsanız, kiracı yöneticinin uygulamaya izin vermesi gerekir. Yukarıdaki istemci uygulamasını kaydedin adım 6'ya bakın.
Genellikle bu hataya benzeyen bir hata görürsünüz:

```JSon
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon uygulaması - bir web API arama](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon uygulaması - bir web API arama](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon uygulaması - bir web API arama](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=java)

---
