---
title: Web API 'Lerini çağıran Web API 'SI | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Lerini çağıran bir Web API 'SI oluşturmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/26/2020
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bbb42a4955ff0b4fbbac58830ec5c8aecf04915d
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101686742"
---
# <a name="a-web-api-that-calls-web-apis-call-an-api"></a>Web API 'Leri çağıran bir Web API 'si: API çağırma

Belirteciniz olduktan sonra korumalı bir Web API 'SI çağırabilirsiniz. Genellikle, Web API 'nizin denetleyicisinden veya sayfalarından aşağı akış API 'Lerini çağırırın.

## <a name="controller-code"></a>Denetleyici kodu

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

*Microsoft. Identity. Web* kullandığınızda, üç kullanım senaryosundan yararlanabilirsiniz:

- [Web API 'Leri çağıran bir Web API 'si: API çağırma](#a-web-api-that-calls-web-apis-call-an-api)
  - [Denetleyici kodu](#controller-code)
- [ASP.NET Core](#aspnet-core)
      - [Seçenek 1: SDK ile Microsoft Graph çağırma](#option-1-call-microsoft-graph-with-the-sdk)
      - [2. seçenek: yardımcı sınıfla bir aşağı akış Web API 'SI çağırma](#option-2-call-a-downstream-web-api-with-the-helper-class)
      - [Seçenek 3: yardımcı sınıfı olmadan bir aşağı akış Web API 'SI çağırma](#option-3-call-a-downstream-web-api-without-the-helper-class)
- [Java](#java)
- [Python](#python)
  - [Sonraki adımlar](#next-steps)

#### <a name="option-1-call-microsoft-graph-with-the-sdk"></a>Seçenek 1: SDK ile Microsoft Graph çağırma

Bu senaryoda, `.AddMicrosoftGraph()` [kod yapılandırmasında](scenario-web-api-call-api-app-configuration.md#option-1-call-microsoft-graph)belirtildiği gibi *Startup.cs* ' ye eklemiş olmanız ve `GraphServiceClient` eylemlerdeki kullanım için denetleyici veya sayfa yapıcısına doğrudan ekleyebilmeniz gerekir. Aşağıdaki örnek Razor sayfası, oturum açmış kullanıcının fotoğrafını görüntüler.

```CSharp
 [Authorize]
 [AuthorizeForScopes(Scopes = new[] { "user.read" })]
 public class IndexModel : PageModel
 {
     private readonly GraphServiceClient _graphServiceClient;

     public IndexModel(GraphServiceClient graphServiceClient)
     {
         _graphServiceClient = graphServiceClient;
     }

     public async Task OnGet()
     {
         var user = await _graphServiceClient.Me.Request().GetAsync();
         try
         {
             using (var photoStream = await _graphServiceClient.Me.Photo.Content.Request().GetAsync())
             {
                 byte[] photoByte = ((MemoryStream)photoStream).ToArray();
                 ViewData["photo"] = Convert.ToBase64String(photoByte);
             }
             ViewData["name"] = user.DisplayName;
         }
         catch (Exception)
         {
             ViewData["photo"] = null;
         }
     }
 }
```

#### <a name="option-2-call-a-downstream-web-api-with-the-helper-class"></a>2. seçenek: yardımcı sınıfla bir aşağı akış Web API 'SI çağırma

Bu senaryoda, `.AddDownstreamWebApi()` [kod yapılandırmasında](scenario-web-api-call-api-app-configuration.md#option-2-call-a-downstream-web-api-other-than-microsoft-graph)belirtildiği gibi *Startup.cs* ' ye eklediniz ve `IDownstreamWebApi` denetleyicinize veya sayfa yapıcısına doğrudan bir hizmet ekleyebilir ve bunları eylemlerde kullanabilirsiniz:

```CSharp
 [Authorize]
 [AuthorizeForScopes(ScopeKeySection = "TodoList:Scopes")]
 public class TodoListController : Controller
 {
     private IDownstreamWebApi _downstreamWebApi;
     private const string ServiceName = "TodoList";

     public TodoListController(IDownstreamWebApi downstreamWebApi)
     {
         _downstreamWebApi = downstreamWebApi;
     }

     public async Task<ActionResult> Details(int id)
     {
         var value = await _downstreamWebApi.CallWebApiForUserAsync(
             ServiceName,
             options =>
             {
                 options.RelativePath = $"me";
             });
         return View(value);
     }
```

`CallWebApiForUserAsync`Yöntemi Ayrıca, doğrudan bir nesne almanızı sağlayan kesin olarak yazılmış genel geçersiz kılmalar içerir. Örneğin, aşağıdaki yöntem, `Todo` Web API 'si tarafından döndürülen JSON öğesinin kesin türü belirtilmiş bir temsili olan bir örnek almıştır.

```CSharp
 // GET: TodoList/Details/5
 public async Task<ActionResult> Details(int id)
 {
     var value = await _downstreamWebApi.CallWebApiForUserAsync<object, Todo>(
         ServiceName,
         null,
         options =>
         {
             options.HttpMethod = HttpMethod.Get;
             options.RelativePath = $"api/todolist/{id}";
         });
     return View(value);
 }
```

#### <a name="option-3-call-a-downstream-web-api-without-the-helper-class"></a>Seçenek 3: yardımcı sınıfı olmadan bir aşağı akış Web API 'SI çağırma

Hizmeti kullanarak bir belirteci el ile almaya karar verdiyseniz `ITokenAcquisition` , artık belirtecini kullanmanız gerekir. Bu durumda, aşağıdaki kod, [Web API 'lerini çağıran bir Web API 'sinde gösterilen örnek koda devam eder: uygulama için bir belirteç alın](scenario-web-api-call-api-acquire-token.md). Kod, API denetleyicilerinin eylemleri içinde çağrılır. *ToDoList* adlı bir aşağı akış API 'si çağırır.

 Belirteci aldıktan sonra, aşağı akış API 'sini çağırmak için bunu bir taşıyıcı belirteç olarak kullanın.

```csharp
 private async Task CallTodoListService(string accessToken)
 {
  // After the token has been returned by Microsoft.Identity.Web, add it to the HTTP authorization header before making the call to access the todolist service.
 _httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

 // Call the todolist service.
 HttpResponseMessage response = await _httpClient.GetAsync(TodoListBaseAddress + "/api/todolist");
 // ...
 }
 ```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki kod, [Web API 'lerini çağıran bir Web API](scenario-web-api-call-api-acquire-token.md)'sinde gösterilen örnek kodu devam ettirir: uygulama Için belirteç alma. Kod, API denetleyicilerinin eylemleri içinde çağrılır. Aşağı akış API MS grafiğini çağırır.

Belirteci aldıktan sonra, aşağı akış API 'sini çağırmak için bunu bir taşıyıcı belirteç olarak kullanın.

```Java
private String callMicrosoftGraphMeEndpoint(String accessToken){
    RestTemplate restTemplate = new RestTemplate();

    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);

    headers.set("Authorization", "Bearer " + accessToken);

    HttpEntity<String> entity = new HttpEntity<>(null, headers);

    String result = restTemplate.exchange("https://graph.microsoft.com/v1.0/me", HttpMethod.GET,
            entity, String.class).getBody();

    return result;
}
```

# <a name="python"></a>[Python](#tab/python)
Bu akışı MSAL Python ile gösteren bir örnek, [MS-Identity-Python-on-of](https://github.com/Azure-Samples/ms-identity-python-on-behalf-of)-The

---

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryodaki sonraki makaleye geçin, [üretime geçin](scenario-web-api-call-api-production.md).
