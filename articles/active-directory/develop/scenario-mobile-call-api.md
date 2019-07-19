---
title: Web API 'Lerini çağıran ve Web API 'sini çağıran mobil uygulama | Microsoft Identity platformu
description: Web API 'Leri çağıran bir mobil uygulama oluşturmayı öğrenin (Web API 'SI çağırma)
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: e1408c06570babfd93c46fdfc7a3c6754000bcbc
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68320845"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Web API 'Lerini çağıran mobil uygulama-bir Web API 'SI çağırma

Uygulamanız bir kullanıcıya kaydolduktan ve belirteçleri aldıktan sonra, MSAL Kullanıcı, kullanıcının ortamı ve verilen belirteçler hakkında çeşitli bilgi parçalarını gösterir. Uygulamanız, bu değerleri bir Web API 'SI çağırmak veya kullanıcıya bir hoş geldiniz iletisi göstermek için kullanabilir.

İlk olarak MSAL sonucuna bakacağız. Daha sonra, `AuthenticationResult` veya korumalı bir Web API 'sini çağırmak için veya `result` ' den erişim belirtecinin nasıl kullanılacağını inceleyeceğiz.

## <a name="msal-result"></a>MSAL sonucu
MSAL aşağıdaki değerleri sağlar: 

- `AccessToken`: Korunan Web API 'Lerini bir HTTP taşıyıcı isteğinde çağırmak için kullanılır.
- `IdToken`: Kullanıcının adı, ev kiracısı ve depolama için benzersiz bir tanımlayıcı gibi oturum açmış kullanıcıyla ilgili yararlı bilgiler içerir.
- `ExpiresOn`: Belirtecin sona erme saati. MSAL, uygulamalar için otomatik yenilemeyi işler.
- `TenantId`: Kullanıcının oturum açmadığı kiracının tanımlayıcısı. Konuk kullanıcılar (Azure Active Directory B2B) için, bu değer kullanıcının giriş kiracısına değil, kullanıcının oturum açmasından kiracıyı belirler.  
- `Scopes`: Belirtecinizle verilen kapsamlar. Verilen kapsamlar, istediğiniz kapsamların bir alt kümesi olabilir.

MSAL, için bir `Account`soyutlama de sağlar. Geçerli kullanıcının oturum açmış olan hesabını temsileder.`Account`

- `HomeAccountIdentifier`: Kullanıcının ana kiracının tanımlayıcısı.
- `UserName`: Kullanıcının tercih ettiği Kullanıcı adı. Bu, Azure Active Directory B2C kullanıcılar için boş olabilir.
- `AccountIdentifier`: Oturum açmış kullanıcının tanımlayıcısı. Bu değer, Kullanıcı başka bir kiracıda `HomeAccountIdentifier` Konuk olmadığı sürece çoğu durumda değeri ile aynı olacaktır.

## <a name="call-an-api"></a>API çağırma

Erişim belirtecine sahip olduktan sonra, bir Web API 'SI çağırmak kolaydır. Uygulamanız bir HTTP isteği oluşturmak için belirteci kullanır ve sonra isteği çalıştırır.

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

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put access token in HTTP request.
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            // Successfully got data from Graph.
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put access token in HTTP request.
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Birkaç API isteği oluşturma

Aynı API 'yi birkaç kez çağırmanız gerekiyorsa veya birden çok API çağırmanız gerekiyorsa, uygulamanızı oluştururken aşağıdakileri dikkate alın:

- **Artımlı izin**: Microsoft Identity platform, uygulamanın başlangıç aşamasında değil, izin gerektiğinde kullanıcıların onayını almasına izin verir. Uygulamanız bir API 'yi çağırmaya her seferinde yalnızca kullanması gereken kapsamları istemesi gerekir.
- **Koşullu erişim**: Bazı senaryolarda, birkaç API isteği yaptığınızda ek koşullu erişim gereksinimleri alabilirsiniz. Bu durum, ilk isteğin uygulanmış koşullu erişim ilkesi yoksa ve uygulamanız koşullu erişim gerektiren yeni bir API 'ye sessizce erişmeye çalışırsa meydana gelebilir. Bu senaryoyu işlemek için sessiz isteklerden gelen hataları yakalamalı ve etkileşimli bir istek oluşturmak için hazırlandığınızdan emin olun.  Daha fazla bilgi için bkz. [koşullu erişim Için rehberlik](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretime taşı](scenario-mobile-production.md)
