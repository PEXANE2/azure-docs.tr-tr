---
title: Web API 'Lerini çağıran Daemon uygulaması (uygulama belirteçleri alınıyor)-Microsoft Identity platform
description: Web API 'Lerini çağıran bir Daemon uygulamasının nasıl oluşturulduğunu öğrenin (belirteçler alınıyor)
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
ms.openlocfilehash: 6a5f15aa5264c0abf87cb15f0468e8a3a924e0b5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562351"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Web API 'Lerini çağıran Daemon uygulaması-belirteç alma

Gizli istemci uygulaması oluşturulduktan sonra, uygulamayı çağırarak ``AcquireTokenForClient``, kapsamı geçirerek ve belirtecin yenilenmesini zorlayarak veya yenileyerek uygulama için bir belirteç elde edebilirsiniz.

## <a name="scopes-to-request"></a>İstek için kapsamlar

İstemci kimlik bilgisi akışı için istenen kapsam, kaynağın ve sonrasında gelen `/.default`addır. Bu gösterim, Azure AD 'nin uygulama kaydı sırasında statik olarak belirtilen **uygulama düzeyi izinlerini** kullanmasını söyler. Ayrıca, daha önce görüldüğü gibi, bu API izinlerinin bir kiracı yöneticisi tarafından verilmesi gerekir

### <a name="net"></a>.NET

```CSharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

### <a name="python"></a>Python

MSAL içinde. Python, yapılandırma dosyası aşağıdaki kod parçacığı gibi görünür:

```Python
{
    "authority": "https://login.microsoftonline.com/organizations",
    "client_id": "your_client_id",
    "secret": "This is a sample only. You better NOT persist your password."
    "scope": ["https://graph.microsoft.com/.default"]
}
```

### <a name="java"></a>Java

```Java
public final static String KEYVAULT_DEFAULT_SCOPE = "https://vault.azure.net/.default";
```

### <a name="all"></a>Tümü

İstemci kimlik bilgileri için kullanılan kapsamın her zaman RESOURCEID + "/.exe" olması gerekir

### <a name="case-of-azure-ad-v10-resources"></a>Azure AD (v 1.0) kaynaklarının durumu

> [!IMPORTANT]
> Bir v 1.0 erişim belirtecini kabul eden bir kaynak için erişim belirteci isteyen MSAL (Microsoft Identity platform uç noktası) için, Azure AD, son eğik çizgiden önce her şeyi alarak ve bunu kaynak tanımlayıcısı olarak kullanarak istenen kapsamdaki hedef kitleyi ayrıştırır.
> Bu nedenle, Azure SQL ( **https://database.windows.net** ) gibi kaynak bir hedef kitleye eğik çizgi (Azure `https://database.windows.net/` SQL için) bekliyor gibi bir kapsam `https://database.windows.net//.default` istemeniz gerekir (çift eğik çizgiyi aklınızda bulunan). Ayrıca bkz. MSAL.NET sorun [#747](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747): Kaynak URL 'sinin sondaki eğik çizgi gözardı edilir ve bu da SQL kimlik doğrulama hatasına neden olur.

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient API 'SI

### <a name="net"></a>.NET

```CSharp
using Microsoft.Identity.Client;

// With client credentials flows the scopes is ALWAYS of the shape "resource/.default", as the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application does not have sufficient permissions
    // - did you declare enough app permissions in during the app creation?
    // - did the tenant admin needs to grant permissions to the application.
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be of the form "https://resourceurl/.default"
    // Mitigation: change the scope to be as expected !
}
```

#### <a name="application-token-cache"></a>Uygulama belirteci önbelleği

MSAL.net ' de `AcquireTokenForClient` , **uygulama belirteci önbelleğini** kullanır (tüm diğer `AcquireTokenSilent` acquiretokenxx yöntemleri kullanıcı belirteci önbelleğini kullanır), çağrılmadan `AcquireTokenForClient` önce `AcquireTokenSilent` çağrı yapmadan **Kullanıcı** belirteci önbelleğini kullanır. `AcquireTokenForClient`**uygulama** belirteci önbelleğinin kendisini denetler ve güncelleştirir.

### <a name="python"></a>Python

```Python
# Firstly, looks up a token from cache
# Since we are looking for token for the current app, NOT for an end user,
# notice we give account parameter as None.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

### <a name="java"></a>Java

```Java
ClientCredentialParameters parameters = ClientCredentialParameters
        .builder(Collections.singleton(KEYVAULT_DEFAULT_SCOPE))
        .build();

CompletableFuture<AuthenticationResult> future = cca.acquireToken(parameters);

// You can complete the future in many different ways. Here we use .get() for simplicity
AuthenticationResult result = future.get();
```

### <a name="protocol"></a>Protocol

Seçtiğiniz diliniz için henüz bir kitaplığınız yoksa, protokolü doğrudan kullanmak isteyebilirsiniz:

#### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: Paylaşılan gizli dizi ile belirteç isteğine erişim

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Bir sertifikayla erişim belirteci isteği

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

### <a name="learn-more-about-the-protocol"></a>Protokol hakkında daha fazla bilgi edinin

Daha fazla bilgi için bkz. protokol belgeleri: [Microsoft Identity platformu ve OAuth 2,0 istemci kimlik bilgileri akışı](v2-oauth2-client-creds-grant-flow.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="did-you-use-the-resourcedefault-scope"></a>Resource/. Default kapsamını mı kullanıyorsunuz?

Geçersiz bir kapsam kullanacağınızı söyleyen bir hata mesajı alırsanız, büyük olasılıkla `resource/.default` kapsamı kullanmadınız.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Yönetici onayı sağlamayı unuttunuz mu? Daemon uygulamalarında şunlar gerekir!

API 'YI çağırırken **işlemi tamamlamaya yönelik ayrıcalıklar yetersizse**bir hata alırsanız, kiracı yöneticisinin uygulamaya izin vermesi gerekir. Yukarıdaki istemci uygulamasını kaydettirme adım 6 ' ya bakın.
Genellikle aşağıdaki hata açıklamasına benzer bir hata görürsünüz:

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

> [!div class="nextstepaction"]
> [Daemon uygulaması-bir Web API 'SI çağırma](scenario-daemon-call-api.md)