---
title: Mobil uygulamadan web API'sını arayın | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini çağıran bir mobil uygulama nın nasıl oluşturulabildiğini öğrenin. (Web API'sını arayın.)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 28f57c5657ce2f8537a654a7f67ed4481fab2c91
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882701"
---
# <a name="call-a-web-api-from-a-mobile-app"></a>Mobil uygulamadan web API'sını arama

Uygulamanız bir kullanıcıda işaretledikten ve belirteçleri aldıktan sonra, Microsoft Kimlik Doğrulama Kitaplığı (MSAL), kullanıcı, kullanıcının ortamı ve verilen belirteçler hakkındaki bilgileri ortaya çıkarır. Uygulamanız bu değerleri bir web API'sını aramak veya kullanıcıya hoş geldiniz iletisi görüntülemek için kullanabilir.

Bu makalede, ilk MSAL sonucu bakacağız. Daha sonra, korumalı bir web API'sinden `AuthenticationResult` erişim jetonunun nasıl kullanılacağına veya `result` aramanın nasıl yapılacağını inceeceğiz.

## <a name="msal-result"></a>MSAL sonucu
MSAL aşağıdaki değerleri sağlar: 

- `AccessToken`bir HTTP taşıyıcı isteği nde korumalı web API'lerini çağırır.
- `IdToken`oturum açmış kullanıcı hakkında yararlı bilgiler içerir. Bu bilgiler, kullanıcının adını, ev kiracısını ve depolama için benzersiz bir tanımlayıcıyı içerir.
- `ExpiresOn`belirteci son kullanma süresidir. MSAL, bir uygulamanın otomatik yenileme işlemlerini işler.
- `TenantId`kullanıcının oturum imzaladığı kiracının tanımlayıcısıdır. Azure Etkin Dizin (Azure AD) B2B'deki konuk kullanıcılar için bu değer, kullanıcının oturum imzaladığı kiracıyı tanımlar. Değer, kullanıcının ev kiracısını tanımlamaz.  
- `Scopes`belirteçile verilen kapsamları gösterir. Verilen kapsamlar, istediğiniz kapsamların bir alt kümesi olabilir.

MSAL ayrıca bir `Account` değer için bir soyutlama sağlar. Değer, `Account` geçerli kullanıcının oturum açmış hesabını temsil eder:

- `HomeAccountIdentifier`kullanıcının ev kiracısını tanımlar.
- `UserName`kullanıcının tercih ettiği kullanıcı adıdır. Bu değer Azure AD B2C kullanıcıları için boş olabilir.
- `AccountIdentifier`oturum açmış kullanıcıyı tanımlar. Çoğu durumda, kullanıcı başka bir `HomeAccountIdentifier` kiracıda konuk olmadığı sürece bu değer değerle aynıdır.

## <a name="call-an-api"></a>API'yi çağırın

Erişim jetonu aldıktan sonra, bir web API'sini arayabilirsiniz. Uygulamanız bir HTTP isteği oluşturmak ve sonra isteği çalıştırmak için belirteci kullanır.

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

Belirteçleri elde etme `MSALResult` yöntemleri bir nesneyi döndürer. `MSALResult`bir `accessToken` özelliği ortaya çıkarır. Web API'sını aramak için kullanabilirsiniz. `accessToken` Korumalı web API'sine erişmek için aramadan önce bu özelliği HTTP yetkilendirme üstbilgisine ekleyin.

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

## <a name="make-several-api-requests"></a>Birkaç API isteği nde bulunun

Aynı API'yi birkaç kez aramanız gerekiyorsa veya birden çok API aramanız gerekiyorsa, uygulamanızı oluştururken aşağıdaki konuları göz önünde bulundurun:

- **Artımlı onay**: Microsoft kimlik platformu, uygulamaların başlangıçta tümü yerine izinler gerektiğinde kullanıcı onayı almalarına olanak tanır. Uygulamanız API'yi her aramaya hazır olduğunda, yalnızca ihtiyaç duyduğu kapsamları istemelidir.

- **Koşullu erişim**: Birkaç API isteği nde, belirli senaryolarda ek koşullu erişim gereksinimlerini karşılamanız gerekebilir. İlk isteğin koşullu erişim ilkeleri yoksa ve uygulamanız koşullu erişim gerektiren yeni bir API'ye sessizce erişmeye çalışırsa gereksinimler bu şekilde artabilir. Bu sorunu işlemek için, sessiz isteklerden kaynaklanan hataları yakaladığınızdan emin olun ve etkileşimli bir istekte bulunmak için hazırlıklı olun.  Daha fazla bilgi [için koşullu erişim için Yönerge'ye](../azuread-dev/conditional-access-dev-guide.md)bakın.

## <a name="call-several-apis-by-using-incremental-consent-and-conditional-access"></a>Artımlı onay ve koşullu erişim kullanarak birkaç API'yi arayın

Aynı kullanıcı için birkaç API aramanız gerekiyorsa, kullanıcı için bir belirteç edindikten sonra, kullanıcıdan `AcquireTokenSilent` daha sonra bir belirteç almak için arayarak kullanıcıdan sürekli olarak kimlik bilgilerini istemekten kaçınabilirsiniz:

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Etkileşim şu zaman gereklidir:

- Kullanıcı ilk API için izin verdi, ancak şimdi daha fazla kapsam için onay alması gerekiyor. Bu durumda, artımlı onay kullanırsınız.
- İlk API çok faktörlü kimlik doğrulaması gerektirmez, ancak sonraki API gerektirir.

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
> [Üretime taşıma](scenario-mobile-production.md)
