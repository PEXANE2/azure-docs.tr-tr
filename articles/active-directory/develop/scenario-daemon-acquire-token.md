---
title: Bir Web API 'SI çağırmak için belirteçler alma (daemon uygulaması)-Microsoft Identity platform | Mavisi
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
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: b2d388160c6ca744b10c17bda17c59e22940f98b
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775251"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Web API 'Lerini çağıran Daemon uygulaması-belirteç alma

Gizli bir istemci uygulaması oluşturduktan sonra, `AcquireTokenForClient`çağırarak, kapsamı geçirerek ve isteğe bağlı olarak belirtecin yenilenmesini zorlayarak uygulama için bir belirteç elde edebilirsiniz.

## <a name="scopes-to-request"></a>İstek için kapsamlar

İstemci kimlik bilgisi akışı için istenen kapsam, kaynağın adı ve ardından `/.default`. Bu gösterim Azure Active Directory (Azure AD) uygulama kaydı sırasında statik olarak belirtilen *uygulama düzeyi izinleri* kullanmasını söyler. Ayrıca, bu API izinlerinin bir kiracı yöneticisi tarafından verilmesi gerekir.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

MSAL Python 'da, yapılandırma dosyası şu kod parçacığına benzer şekilde görünür:

```Json
{
    "scope": ["https://graph.microsoft.com/.default"],
}
```

# <a name="javatabjava"></a>[Java](#tab/java)

```Java
final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
```

---

### <a name="azure-ad-v10-resources"></a>Azure AD (v 1.0) kaynakları

İstemci kimlik bilgileri için kullanılan kapsamın her zaman kaynak KIMLIĞI ve ardından `/.default`olması gerekir.

> [!IMPORTANT]
> MSAL, sürüm 1,0 erişim belirtecini kabul eden bir kaynak için bir erişim belirteci istediğinde, Azure AD, son eğik çizgiden önce her şeyi alarak ve bunu kaynak tanımlayıcısı olarak kullanarak istenen kapsamdaki hedef kitleyi ayrıştırır.
> Bu nedenle, Azure SQL veritabanı (**https:\//Database.Windows.net**) gibi, kaynak bir eğik çizgi ile biten bir izleyiciyi bekler (Azure SQL veritabanı, `https://database.windows.net/`için), `https://database.windows.net//.default`kapsamını istemeniz gerekir. (Çift eğik çizgiye göz önünde edin.) Ayrıca bkz. MSAL.NET sorun [#747: kaynak URL 'sinin sondaki eğik çizgi atlandığından SQL kimlik doğrulama hatasına neden olur](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient API 'SI

Uygulamanın bir belirtecini almak için platforma bağlı olarak `AcquireTokenForClient` veya eşdeğerini kullanacaksınız.

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

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

# <a name="pythontabpython"></a>[Python](#tab/python)

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

# <a name="javatabjava"></a>[Java](#tab/java)

Bu kod, [msal Java dev örneklerinden](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/)ayıklanır.

```Java
ClientCredentialParameters clientCredentialParam = ClientCredentialParameters.builder(
        Collections.singleton(GRAPH_DEFAULT_SCOPE))
        .build();

CompletableFuture<IAuthenticationResult> future = app.acquireToken(clientCredentialParam);

BiConsumer<IAuthenticationResult, Throwable> processAuthResult = (res, ex) -> {
    if (ex != null) {
        System.out.println("Oops! We have an exception - " + ex.getMessage());
    }
    System.out.println("Returned ok - " + res);
    System.out.println("ID Token - " + res.idToken());

    /* Call a protected API with res.accessToken() */
};

future.whenCompleteAsync(processAuthResult);
future.join();
```

---

### <a name="protocol"></a>Protokol

Seçtiğiniz dil için henüz bir kitaplığınız yoksa, protokolü doğrudan kullanmak isteyebilirsiniz:

#### <a name="first-case-access-the-token-request-by-using-a-shared-secret"></a>İlk durum: paylaşılan bir gizli dizi kullanarak belirteç isteğine erişin

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity.
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-the-token-request-by-using-a-certificate"></a>İkinci durum: bir sertifika kullanarak belirteç isteğine erişin

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

Daha fazla bilgi için bkz. protokol belgeleri: [Microsoft Identity platform ve OAuth 2,0 istemci kimlik bilgileri akışı](v2-oauth2-client-creds-grant-flow.md).

## <a name="application-token-cache"></a>Uygulama belirteci önbelleği

MSAL.NET ' de, `AcquireTokenForClient` uygulama belirteci önbelleğini kullanır. (Tüm diğer AcquireToken*xx* yöntemleri kullanıcı belirteci önbelleğini kullanır.) `AcquireTokenSilent` *Kullanıcı* belirteci önbelleğini kullandığından, `AcquireTokenForClient`çağırmadan önce `AcquireTokenSilent` çağırmayın. `AcquireTokenForClient`, *uygulama* belirteci önbelleğinin kendisini denetler ve güncelleştirir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="did-you-use-the-resourcedefault-scope"></a>Resource/. Default kapsamını mı kullanıyorsunuz?

Geçersiz bir kapsam kullandığını söyleyen bir hata mesajı alırsanız, büyük olasılıkla `resource/.default` kapsamını kullanmadınız.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Yönetici onayı sağlamayı unuttunuz mu? Daemon uygulamalarında şunlar gerekir!

API 'yi çağırdığınızda **işlem hatasını tamamlamaya yetecek ayrıcalıklara** sahipseniz, kiracı yöneticisinin uygulamaya izin vermesi gerekir. Yukarıdaki istemci uygulamasını kaydettirme adım 6 ' ya bakın.
Genellikle şu hata gibi görünen bir hata görürsünüz:

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

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon uygulaması-bir Web API 'SI çağırma](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=dotnet)

# <a name="pythontabpython"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon uygulaması-bir Web API 'SI çağırma](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=python)

# <a name="javatabjava"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon uygulaması-bir Web API 'SI çağırma](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=java)

---
