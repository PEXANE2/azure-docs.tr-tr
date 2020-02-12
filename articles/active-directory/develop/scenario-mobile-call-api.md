---
title: Mobil uygulamadan bir Web API 'SI çağırma | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini çağıran bir mobil uygulama oluşturmayı öğrenin. (Bir Web API 'SI çağırın.)
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
ms.reviwer: brandwe
ms.custom: aaddev
ms.openlocfilehash: f41c9a0e4754c60fd248e540a81e2afa833d655b
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132397"
---
# <a name="call-a-web-api-from-a-mobile-app"></a>Mobil uygulamadan bir Web API 'SI çağırma

Uygulamanız bir kullanıcıya kaydolduktan ve belirteçleri aldıktan sonra, Microsoft kimlik doğrulama kitaplığı (MSAL) Kullanıcı, kullanıcının ortamı ve verilen belirteçler hakkında bilgi gösterir. Uygulamanız, bu değerleri bir Web API 'SI çağırmak veya kullanıcıya bir hoş geldiniz iletisi göstermek için kullanabilir.

Bu makalede, ilk olarak MSAL sonucuna bakacağız. Daha sonra, korumalı bir Web API 'sini çağırmak için `AuthenticationResult` veya `result` erişim belirtecinin nasıl kullanılacağını inceleyeceğiz.

## <a name="msal-result"></a>MSAL sonucu
MSAL aşağıdaki değerleri sağlar: 

- `AccessToken`, HTTP taşıyıcı isteğinde korunan Web API 'Lerini çağırır.
- `IdToken`, oturum açmış kullanıcıyla ilgili yararlı bilgiler içerir. Bu bilgiler kullanıcının adını, ana kiracısını ve depolama için benzersiz bir tanımlayıcıyı içerir.
- `ExpiresOn`, belirtecin sona erme zamanıdır. MSAL, bir uygulamanın otomatik yenilemesini işler.
- `TenantId`, kullanıcının oturum açmadığı kiracının tanımlayıcısıdır. Azure Active Directory (Azure AD) B2B 'daki Konuk kullanıcılar için bu değer, kullanıcının oturum açmakta olduğu kiracıyı tanımlar. Değer, kullanıcının ana kiracısını tanımlamaz.  
- `Scopes`, belirtecinizle verilen kapsamları gösterir. Verilen kapsamlar, istediğiniz kapsamların bir alt kümesi olabilir.

MSAL Ayrıca `Account` bir değer için bir soyutlama sağlar. `Account` bir değer, geçerli kullanıcının oturum açmış olan hesabını temsil eder:

- `HomeAccountIdentifier` kullanıcının ana kiracısını tanımlar.
- kullanıcının tercih ettiği Kullanıcı adı `UserName`. Azure AD B2C kullanıcılar için bu değer boş olabilir.
- `AccountIdentifier`, oturum açan kullanıcıyı tanımlar. Çoğu durumda, Kullanıcı başka bir kiracıda Konuk olmadığı sürece bu değer `HomeAccountIdentifier` değeriyle aynıdır.

## <a name="call-an-api"></a>API çağırma

Erişim belirtecine sahip olduktan sonra, bir Web API 'SI çağırabilirsiniz. Uygulamanız bir HTTP isteği oluşturmak için belirteci kullanır ve sonra isteği çalıştırır.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="msal-for-ios-and-macos"></a>iOS ve macOS için MSAL

Belirteçleri elde etmek için yöntemler bir `MSALResult` nesnesi döndürür. `MSALResult`, bir `accessToken` özelliğini kullanıma sunar. Bir Web API 'sini çağırmak için `accessToken` kullanabilirsiniz. Korumalı Web API 'sine erişmek için çağrı yapmadan önce bu özelliği HTTP yetkilendirme üstbilgisine ekleyin.

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

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]
     
let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

### <a name="xamarin"></a>Xamarin

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

## <a name="make-several-api-requests"></a>Birkaç API isteği yapın

Aynı API 'yi birkaç kez çağırmanız gerekiyorsa veya birden çok API çağırmanız gerekiyorsa, uygulamanızı oluştururken aşağıdaki konuları göz önünde bulundurun:

- **Artımlı izin**: Microsoft Identity platform, uygulamaların başlangıçta değil, izin gerektiğinde kullanıcı onayı almasına izin verir. Uygulamanız bir API 'yi çağırmaya her seferinde, yalnızca ihtiyaç duyacağı kapsamları istemesi gerekir.

- **Koşullu erişim**: birkaç API isteği yaptığınızda, bazı senaryolarda ek koşullu erişim gereksinimlerini karşılamanız gerekebilir. Bu şekilde, ilk isteğin koşullu erişim ilkeleri yoksa ve uygulamanız koşullu erişim gerektiren yeni bir API 'ye sessizce erişmeyi denediğinde gereksinimler bu şekilde artabilir. Bu sorunu gidermek için, sessiz isteklerden gelen hataları yakalamalı ve etkileşimli bir istek oluşturmak için hazırlandığınızdan emin olun.  Daha fazla bilgi için bkz. [koşullu erişim Için rehberlik](conditional-access-dev-guide.md).

## <a name="call-several-apis-by-using-incremental-consent-and-conditional-access"></a>Artımlı onay ve koşullu erişim kullanarak birkaç API çağırın

Aynı kullanıcı için birden çok API çağırmanız gerekiyorsa, Kullanıcı için bir belirteç aldıktan sonra, bir belirteci almak için `AcquireTokenSilent` daha sonra çağırarak kullanıcıdan kimlik bilgilerini tekrar tekrar sormaktan kaçınabilirsiniz:

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Şu durumlarda etkileşim gerekir:

- Kullanıcı ilk API 'ye onay verdi, ancak şimdi daha fazla kapsam için onay gerektirir. Bu durumda, artımlı onay kullanırsınız.
- İlk API çok faktörlü kimlik doğrulaması gerektirmez, ancak sonraki API.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-mobile-production.md)
