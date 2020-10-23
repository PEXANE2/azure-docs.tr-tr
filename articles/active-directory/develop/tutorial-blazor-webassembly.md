---
title: Öğretici-kullanıcıları oturum açma ve bir Blazor WebAssembly uygulamasından korunan API çağırma
titleSuffix: Microsoft identity platform
description: Bu öğreticide, kullanıcıları oturum açın ve bir Blazor WebAssembly uygulamasında Microsoft Identity platformunu kullanarak korumalı bir API 'YI çağırın.
author: knicholasa
ms.author: nichola
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: ba3607c522191644ec0cc63db118de285d297c48
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92223104"
---
# <a name="tutorial-sign-in-users-and-call-a-protected-api-from-a-blazor-webassembly-app"></a>Öğretici: kullanıcılarda oturum açın ve Blazor WebAssembly uygulamasından korunan API 'yi çağırın

[Blazor WebAssembly](/aspnet/core/blazor#blazor-webassembly) , .NET ile etkileşimli istemci tarafı Web uygulamaları oluşturmaya yönelik tek sayfalı bir uygulama çerçevesidir. Bu öğreticide, kullanıcıları oturum açan ve [Microsoft Identity platformu](https://docs.microsoft.com/azure/active-directory/develop/)Ile bir Blazor WebAssembly (BLAZOR TI) uygulamasından korunan bir API 'den veri alan bir uygulama oluşturacaksınız.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
>
> * Microsoft Identity platformunu kullanarak [kimlik doğrulaması ve yetkilendirme](authentication-vs-authorization.md) için Azure Active Directory (Azure AD) kullanmak üzere yapılandırılmış yeni bir Blazor WebAssembly uygulaması oluşturun
> * Bu durumda korumalı bir Web API 'sinden veri alma [Microsoft Graph](https://docs.microsoft.com/graph/overview)

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* Bir uygulamayı kaydedebileceğiniz bir Azure AD kiracısı. Bir Azure AD kiracısına erişiminiz yoksa, [Microsoft 365 Geliştirici programına](https://developer.microsoft.com/microsoft-365/dev-program) kaydolarak veya [ücretsiz Azure hesabı](https://azure.microsoft.com/free)oluşturarak bir tane edinebilirsiniz.

## <a name="register-the-app-in-the-azure-portal"></a>Uygulamayı Azure portal kaydetme

Kimlik doğrulaması için Azure Active Directory (Azure AD) kullanan her uygulamanın Azure AD 'ye kayıtlı olması gerekir. Şu belirtimlere sahip [bir uygulamayı kaydetme](quickstart-register-app.md) bölümündeki yönergeleri izleyin:

- **Desteklenen hesap türleri**için **yalnızca bu kuruluş dizinindeki hesaplar**' ı seçin.
- **Yeniden yönlendirme URI 'si** açılan öğesini **Web** olarak ayarlayın ve girin `https://localhost:5001/authentication/login-callback` . Kestrel üzerinde çalışan bir uygulamanın varsayılan bağlantı noktası 5001 ' dir. Uygulama farklı bir bağlantı noktasında kullanılabiliyorsa, yerine bu bağlantı noktası numarasını belirtin `5001` .

Kaydolduktan sonra, **kimlik doğrulama**  >  **örtük izni**' nda, **erişim belirteçleri** ve **Kimlik belirteçleri**onay kutularını işaretleyin ve ardından **Kaydet** düğmesini seçin.

## <a name="create-the-app-using-the-net-core-cli"></a>.NET Core CLI kullanarak uygulamayı oluşturun

Uygulamayı oluşturmak için en son Blazor şablonlarına ihtiyacınız vardır. Aşağıdaki komutla .NET Core CLI için onları yükleyebilirsiniz:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.1
```

Ardından, uygulamayı oluşturmak için aşağıdaki komutu çalıştırın. Komutta yer tutucuları uygulamanızın genel bakış sayfasından uygun bilgilerle değiştirin ve komutu bir komut kabuğu 'nda yürütün. Seçeneğiyle belirtilen çıktı konumu, `-o|--output` mevcut değilse bir proje klasörü oluşturur ve uygulamanın adının bir parçası haline gelir.

```dotnetcli
dotnet new blazorwasm2 --auth SingleOrg --calls-graph -o {APP NAME} --client-id "{CLIENT ID}" --tenant-id "{TENANT ID}"
```

| Yer tutucu   | Azure portal adı       | Örnek                                |
| ------------- | ----------------------- | -------------------------------------- |
| `{APP NAME}`  | &mdash;                 | `BlazorWASMSample`                         |
| `{CLIENT ID}` | Uygulama (istemci) kimliği | `41451fa7-0000-0000-0000-69eff5a761fd` |
| `{TENANT ID}` | Dizin (kiracı) kimliği   | `e86c78e2-0000-0000-0000-918e0565a45e` |

## <a name="test-the-app"></a>Uygulamayı test etme

Artık uygulamayı derleyebilir ve çalıştırabilirsiniz. Bu şablon uygulamasını çalıştırdığınızda,--Framework kullanarak çalıştırılacak Framework 'ü belirtmeniz gerekir. Bu öğretici 2,1 .NET Standard kullanır, ancak şablon diğer çerçeveleri de destekler.

```dotnetcli
dotnet run --framework netstandard2.1
```

Tarayıcınızda `https://localhost:5001` , ' a gidin ve Azure AD Kullanıcı hesabı kullanarak oturum açarak Microsoft Identity platformunda kullanıcıları çalıştıran uygulamayı ve oturum açmasını görüntüleyin.

Bu şablonun Microsoft Identity platformunu kullanarak Azure AD ile oturum açmaları etkinleştiren bileşenleri, [Bu konudaki ASP.net doc](/aspnet/core/blazor/security/webassembly/standalone-with-azure-active-directory#authentication-package)bölümünde açıklanmaktadır.

## <a name="retrieving-data-from-microsoft-graph"></a>Microsoft Graph verileri alma

[Microsoft Graph](/graph/overview) , kiracınızdaki kullanıcıların Microsoft 365 verilerine erişim sağlayan bir dizi API sunar. Microsoft Identity platformunu uygulamanızın kimlik sağlayıcısı olarak kullanarak Microsoft Identity platform tarafından verilen belirteçleri Microsoft Graph doğrudan desteklediğinden bu bilgilere daha kolay erişebilirsiniz. Bu bölümde kod eklemek, oturum açan kullanıcının e-postalarını uygulamanın "verileri getir" sayfasında görüntüleyebilir.

Başlamadan önce, gerekli izinlerde değişiklikler yaptığınız için uygulamanızı oturumunuzu kapatıp geçerli belirteciniz çalışmayacak. Henüz yapmadıysanız, aşağıdaki kodu güncelleştirmeden önce uygulamanızı yeniden çalıştırın ve **Oturumu Kapat** ' ı seçin.

Şimdi, bir kullanıcının e-postalarını çekmek ve iletileri uygulama içinde göstermek için uygulamanızın kaydını ve kodunu güncelleştiğinizde.

İlk olarak, `Mail.Read` Azure AD 'nin kullanıcıların e-postasına erişim isteyeceğini bilmesi için uygulamanın kaydına API iznini ekleyin.

1. Azure portal, **uygulama kayıtları**' de Uygulamanızı seçin.
1. **Yönet**altında **API izinleri**' ni seçin.
1. Microsoft Graph **izin Ekle**' yi seçin  >  **Microsoft Graph**.
1. **Temsilci izinleri**' ni seçin, sonra **posta. Read** iznini arayıp seçin.
1. **Izin Ekle**' yi seçin.

Ardından, aşağıdaki,, Netstandard 2.1 **ItemGroup**'daki projenizin *. csproj* dosyasına aşağıdakini ekleyin. Bu, bir sonraki adımda özel HttpClient oluşturmanıza olanak sağlayacak.

```xml
<PackageReference Include="Microsoft.Extensions.Http" Version="3.1.7" />
```

Ardından, sonraki birkaç adımda belirtilen kodu değiştirin. Bu değişiklikler, Microsoft Graph API 'sine gönderilen giden isteklere [erişim belirteçleri](access-tokens.md) ekler. Bu model, [ASP.NET Core Blazor WebAssembly ek güvenlik senaryolarında](/aspnet/core/blazor/security/webassembly/additional-scenarios)daha ayrıntılı bir şekilde ele alınmıştır.

İlk olarak, aşağıdaki kodla *GraphAuthorizationMessageHandler.cs* adlı yeni bir dosya oluşturun. Bu işleyici, `User.Read` ve `Mail.Read` kapsamları IÇIN Microsoft Graph API 'sine giden isteklere yönelik bir erişim belirteci ekleyecek Kullanıcı olacaktır.

```csharp
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;

public class GraphAPIAuthorizationMessageHandler : AuthorizationMessageHandler
{
    public GraphAPIAuthorizationMessageHandler(IAccessTokenProvider provider,
        NavigationManager navigationManager)
        : base(provider, navigationManager)
    {
        ConfigureHandler(
            authorizedUrls: new[] { "https://graph.microsoft.com" },
            scopes: new[] { "https://graph.microsoft.com/User.Read", "https://graph.microsoft.com/Mail.Read" });
    }
}
```

Ardından, `Main` *program.cs* içindeki yönteminin içeriğini aşağıdaki kodla değiştirin. Bu kod, yeni ' nin kullanımını ve `GraphAPIAuthorizationMessageHandler` `User.Read` `Mail.Read` Kullanıcı ilk kez oturum açtığında uygulamanın isteyeceğini varsayılan kapsamları ekler.

```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("app");

builder.Services.AddScoped<GraphAPIAuthorizationMessageHandler>();

builder.Services.AddHttpClient("GraphAPI",
        client => client.BaseAddress = new Uri("https://graph.microsoft.com"))
    .AddHttpMessageHandler<GraphAPIAuthorizationMessageHandler>();

builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("User.Read");
    options.ProviderOptions.DefaultAccessTokenScopes.Add("Mail.Read");
});

await builder.Build().RunAsync();
```

Son olarak, *Fetchdata. Razor* sayfasının içeriğini aşağıdaki kodla değiştirin. Bu kod, kullanıcı e-posta verilerini Microsoft Graph API 'den getirir ve bunları bir liste olarak görüntüler. `OnInitializedAsync`' De, `HttpClient` uygun erişim belirtecini kullanan yeni, ISTEğI Microsoft Graph API 'sine yapmak için oluşturulur ve kullanılır.

```c#
@page "/fetchdata"
@using System.ComponentModel.DataAnnotations
@using System.Text.Json.Serialization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@using Microsoft.Extensions.Logging
@inject IAccessTokenProvider TokenProvider
@inject IHttpClientFactory ClientFactory
@inject IHttpClientFactory HttpClientFactory

<p>This component demonstrates fetching data from a service.</p>

@if (messages == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <h1>Hello @userDisplayName !!!!</h1>
    <table class="table">
        <thead>
            <tr>
                <th>Subject</th>
                <th>Sender</th>
                <th>Received Time</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var mail in messages)
            {
                <tr>
                    <td>@mail.Subject</td>
                    <td>@mail.Sender</td>
                    <td>@mail.ReceivedTime</td>
                </tr>
            }
        </tbody>
    </table>
}

@code {

    private string userDisplayName;
    private List<MailMessage> messages = new List<MailMessage>();

    private HttpClient _httpClient;

    protected override async Task OnInitializedAsync()
    {
        _httpClient = HttpClientFactory.CreateClient("GraphAPI");
        try {
            var dataRequest = await _httpClient.GetAsync("https://graph.microsoft.com/beta/me");

            if (dataRequest.IsSuccessStatusCode)
            {
                var userData = System.Text.Json.JsonDocument.Parse(await dataRequest.Content.ReadAsStreamAsync());
                userDisplayName = userData.RootElement.GetProperty("displayName").GetString();
            }

            var mailRequest = await _httpClient.GetAsync("https://graph.microsoft.com/beta/me/messages?$select=subject,receivedDateTime,sender&$top=10");

            if (mailRequest.IsSuccessStatusCode)
            {
                var mailData = System.Text.Json.JsonDocument.Parse(await mailRequest.Content.ReadAsStreamAsync());
                var messagesArray = mailData.RootElement.GetProperty("value").EnumerateArray();

                foreach (var m in messagesArray)
                {
                    var message = new MailMessage();
                    message.Subject = m.GetProperty("subject").GetString();
                    message.Sender = m.GetProperty("sender").GetProperty("emailAddress").GetProperty("address").GetString();
                    message.ReceivedTime = m.GetProperty("receivedDateTime").GetDateTime();
                    messages.Add(message);
                }
            }
        }
        catch (AccessTokenNotAvailableException ex)
        {
            // Tokens are not valid - redirect the user to log in again
            ex.Redirect();
        }
    }

    public class MailMessage
    {
        public string Subject;
        public string Sender;
        public DateTime ReceivedTime;
    }
}
```

Şimdi uygulamayı yeniden başlatın. E-postanızı okumak için uygulamaya erişim vermeniz istenir. Bu, bir uygulama kapsamı istediğinde beklenmektedir `Mail.Read` .

Onay verdikten sonra, bazı e-postaları okumak için "verileri getir" sayfasına gidin.

:::image type="content" source="./media/tutorial-blazor-webassembly/final-app.png" alt-text="Son uygulamanın ekran görüntüsü. Hello Nicholas yazan bir başlığa sahiptir ve Nicholas 'e ait e-postaların bir listesini gösterir.":::

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Identity platform en iyi uygulamaları ve önerileri](./identity-platform-integration-checklist.md)
- [ASP.NET Core Blazor’a giriş](/aspnet/core/blazor)
