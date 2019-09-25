---
title: Web API 'Lerini çağıran masaüstü uygulaması (bir Web API 'SI çağırma)-Microsoft Identity platform
description: Web API 'Leri çağıran bir masaüstü uygulaması oluşturmayı öğrenin (Web API 'SI çağırma)
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56d3d01e39adfeb6bf2ef5e7e7d595f49c90f5a5
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268288"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Web API 'Lerini çağıran masaüstü uygulaması-bir Web API 'SI çağırma

Artık bir belirteciniz olduğuna göre, korumalı bir Web API 'SI çağırabilirsiniz.

## <a name="calling-a-web-api-from-net"></a>.NET 'ten bir Web API 'SI çağırma

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-a-web-api-in-msal-for-ios-and-macos"></a>İOS ve macOS için MSAL içinde bir Web API 'SI çağırma

Belirteçleri elde etmek için yöntemler bir `MSALResult` nesne döndürür. `MSALResult`bir Web `accessToken` API 'sini çağırmak için kullanılabilecek bir özellik sunar. Korunan Web API 'sine erişmek için çağrı yapılmadan önce, erişim belirtecinin HTTP yetkilendirme üstbilgisine eklenmesi gerekir.

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

SWIFT

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Çeşitli API 'Leri çağırma-artımlı onay ve koşullu erişim

Aynı kullanıcı için birden çok API çağırmanız gerekiyorsa, ilk API için bir belirteç aldıktan sonra yalnızca çağırabilirsiniz `AcquireTokenSilent`ve diğer API 'ler için çoğu zaman sessizce bir belirteç alırsınız.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Etkileşimin gerekli olduğu durumlar şunlardır:

- Kullanıcı ilk API 'yi kabul etmiş, ancak artık daha fazla kapsam için onay gerektirir (artımlı onay)
- İlk API çok faktörlü kimlik doğrulaması gerektirmez, ancak bir sonraki tane.

```CSharp
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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-desktop-production.md)
