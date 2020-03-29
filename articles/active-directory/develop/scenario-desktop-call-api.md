---
title: Bir masaüstü uygulamasından web API'lerini arayın - Microsoft kimlik platformu | Azure
description: Web API'leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: 2b3d9fdc163d0661670f3d0cf6e6a276c8b691bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76702173"
---
# <a name="desktop-app-that-calls-web-apis-call-a-web-api"></a>Web API'lerini çağıran masaüstü uygulaması: Web API'sını arayın

Artık bir belirteç var, korumalı web API arayabilirsiniz.

## <a name="call-a-web-api"></a>Web API çağrısı

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->
# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

# <a name="macos"></a>[Macos](#tab/macOS)

## <a name="call-a-web-api-in-msal-for-ios-and-macos"></a>iOS ve macOS için MSAL'da web API'sını arayın

Belirteçleri elde etme `MSALResult` yöntemleri bir nesneyi döndürer. `MSALResult`web API'sını çağırmak için kullanılabilecek bir `accessToken` özelliği ortaya çıkarır. Korumalı web API'sine erişmek için arama yapmadan önce HTTP yetkilendirme üstbilgisine bir erişim belirteci ekleyin.

Amaç-C:

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };

NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift:

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]

let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

## <a name="call-several-apis-incremental-consent-and-conditional-access"></a>Birkaç API arayın: Artımlı onay ve koşullu erişim

Aynı kullanıcı için birkaç API çağırmak için, ilk API için `AcquireTokenSilent`bir belirteç aldıktan sonra, arayın. Çoğu zaman diğer API'ler için bir belirteç alırsınız.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Etkileşim şu zaman gereklidir:

- Kullanıcı ilk API için izin verdi, ancak şimdi daha fazla kapsam için onay alması gerekiyor. Bu tür bir rıza, artımlı rıza olarak bilinir.
- İlk API çok faktörlü kimlik doğrulaması gerektirmedi, ama bir sonraki yok.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```
---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşıma](scenario-desktop-production.md)
