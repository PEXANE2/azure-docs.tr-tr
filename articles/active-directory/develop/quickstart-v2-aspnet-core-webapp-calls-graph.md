---
title: 'Hızlı başlangıç: kullanıcılar ve çağrılar Microsoft Graph oturum açan Web uygulaması ASP.NET Core | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir uygulamanın OpenID Connect ve çağrılarını kullanarak bir ASP.NET Core Web uygulamasında Microsoft oturum açma uygulaması için Microsoft. Identity. Web 'ten nasıl yararlandığından öğrenin Microsoft Graph
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 12/10/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: 0a5d44f0cc42b80fa344e06f31275e87d265800e
ms.sourcegitcommit: 126ee1e8e8f2cb5dc35465b23d23a4e3f747949c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100103847"
---
# <a name="quickstart-aspnet-core-web-app-that-signs-in-users-and-calls-microsoft-graph-on-their-behalf"></a>Hızlı başlangıç: kullanıcılara kaydolan ve adına Microsoft Graph çağıran Web uygulaması ASP.NET Core

Bu hızlı başlangıçta, bir ASP.NET Core Web uygulamasının kullanıcılara herhangi bir Azure Active Directory (Azure AD) kuruluştan nasıl oturum açıp Microsoft Graph çağrılayabileceğinizi gösteren bir kod örneği indirip çalıştırırsınız.  

Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

> [!div renderon="docs"]
> ## <a name="prerequisites"></a>Önkoşullar
>
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) veya [Visual Studio Code](https://code.visualstudio.com/)
> * [.NET Core SDK 3.1 +](https://dotnet.microsoft.com/download)
>
> ## <a name="register-and-download-the-quickstart-app"></a>Hızlı başlangıç uygulamasını kaydedin ve indirin
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. <a href="https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetCoreWebAppQuickstartPage/sourceType/docs" target="_blank">Azure portal uygulama kayıtları</a> hızlı başlangıç deneyimine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
> 1. Birden fazla kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.
> 1. Uygulamanız için bir **ad** girin (örneğin,) `AspNetCoreWebAppCallsGraph-Quickstart` . Uygulamanızın kullanıcıları bu adı görebilir ve daha sonra değiştirebilirsiniz.
> 1. **Yeniden yönlendirme URI 'si** girin `https://localhost:44321/signin-oidc` .
> 1. **Kaydet**’i seçin.
> 1. **Yönet** altında **kimlik doğrulaması**' nı seçin.
> 1. **Ön kanal oturum kapatma URL 'sini** girin `https://localhost:44321/signout-oidc` .
> 1. **Kaydet**’i seçin.
> 1. **Yönet**' in altında **Sertifikalar & gizli**  >  **anahtar istemci parolası**' nı seçin.
> 1. Örneğin bir **Açıklama** girin `clientsecret1` .
> 1. Gizli dizi süresi için **1 yılda** bir seçin.
> 1. **Ekle** ' yi seçin ve daha sonraki bir adımda kullanılacak gizli dizi **değerini** hemen kaydedin. Gizli dizi değeri *hiç bir kez daha gösterilmez* ve başka yollarla geri alınabilir. Parolayı istediğiniz gibi güvenli bir konuma kaydedin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için, uygulama kaydında bir **yeniden YÖNLENDIRME URI** 'Si `https://localhost:44321/signin-oidc` ve **ön kanal oturum kapatma URL 'si** ekleyin `https://localhost:44321/signout-oidc` .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-aspnet-core-project"></a>2. Adım: ASP.NET Core projesini Indirin

> [!div renderon="docs"]
> [ASP.NET Core çözümünü indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1-callsgraph.zip)

> [!div renderon="portal" class="sxs-lookup"]
> Projeyi çalıştırın.

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1-callsgraph.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-aspnet-core-project"></a>3. Adım: ASP.NET Core projenizi yapılandırma
> 1. . Zip arşivini sürücünüzün köküne yakın bir yerel klasöre ayıklayın. Örneğin, *C:\Azure-Samples*.
> 1. Visual Studio 2019 ' de çözümü açın.
> 1. appsettings.jsdosya *üzerinde* açın ve aşağıdakileri değiştirin:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "common",
>    "clientSecret": "Enter_the_Client_Secret_Here"
>    ```
>
>    - `Enter_the_Application_Id_here`Azure Portal kaydettiğiniz uygulamanın **uygulama (ISTEMCI) kimliğiyle** değiştirin. Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliğini** bulabilirsiniz.
>    - `common`Aşağıdakilerden biriyle değiştirin:
>       - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Dizin (kiracı) kimliği** (GUID) veya **kiracı adı** (örneğin,) ile değiştirin `contoso.onmicrosoft.com` . Uygulamanın **genel bakış** sayfasında **Dizin (kiracı) kimliğini** bulabilirsiniz.
>       - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>       - Uygulamanız **tüm Microsoft hesabı kullanıcıları** destekliyorsa, bu değeri şu şekilde bırakın `common`
>    - `Enter_the_Client_Secret_Here`Oluşturduğunuz ve daha önceki bir adımda kaydettiğiniz **istemci gizli anahtarı** ile değiştirin.
> 
> Bu hızlı başlangıçta, dosyadaki *appsettings.js* başka bir değeri değiştirmeyin.
>
> #### <a name="step-4-build-and-run-the-application"></a>4. Adım: Uygulamayı derleme ve çalıştırma
>
> Hata **ayıklamayı başlatmak**> **hata** ayıklama menüsünü seçerek veya anahtara basarak uygulamayı Visual Studio 'da derleyin ve çalıştırın `F5` .
>
> Kimlik bilgileriniz istenir ve ardından uygulamanızın gerektirdiği izinleri onaylıyoruz. Onay isteminde **kabul et** ' i seçin.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp-calls-graph/webapp-01-consent.png" alt-text="> kullanıcıdan uygulamanın istediği izinleri gösteren onay iletişim kutusu":::
>
> İstenen izinlere konduktan sonra uygulama, Azure Active Directory kimlik bilgilerinizi kullanarak başarıyla oturum açtığınızı gösterir ve sayfanın "API Result" bölümünde e-posta adresinizi görürsünüz. Bu, Microsoft Graph kullanılarak ayıklandı.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp-calls-graph/webapp-02-signed-in.png" alt-text="Çalışan Web uygulamasını ve oturum açan kullanıcıyı görüntüleyen Web tarayıcısı":::

## <a name="about-the-code"></a>Kod hakkında

Bu bölüm, kullanıcıların oturum açması ve Microsoft Graph API 'sini adına çağırması için gereken koda genel bir bakış sunar. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenlerin ve ayrıca var olan bir ASP.NET Core uygulamasına oturum açma eklemek ve Microsoft Graph çağırmak için yararlı olabilir. [Msal.net](msal-overview.md)etrafında bir sarmalayıcı olan [Microsoft. Identity. Web](microsoft-identity-web.md)'i kullanır.

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp-calls-graph/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft. AspNetCore. Authentication* ara yazılımı, `Startup` barındırma işlemi başlatıldığında yürütülen bir sınıfı kullanır:

```csharp
  // Get the scopes from the configuration (appsettings.json)
  var initialScopes = Configuration.GetValue<string>("DownstreamApi:Scopes")?.Split(' ');

  public void ConfigureServices(IServiceCollection services)
  {
      // Add sign-in with Microsoft
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
        .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"))

            // Add the possibility of acquiring a token to call a protected web API
            .EnableTokenAcquisitionToCallDownstreamApi(initialScopes)

                // Enables controllers and pages to get GraphServiceClient by dependency injection
                // And use an in memory token cache
                .AddMicrosoftGraph(Configuration.GetSection("DownstreamApi"))
                .AddInMemoryTokenCaches();

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });

      // Enables a UI and controller for sign in and sign out.
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

`AddAuthentication()`Yöntemi, hizmetini tarayıcı senaryolarında kullanılan ve kimlik doğrulama tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır ve bunu OpenID Connect olarak ayarlar.

İçeren çizgi, `.AddMicrosoftIdentityWebApp` uygulamanıza Microsoft Identity platform kimlik doğrulamasını ekler. Bu, [Microsoft. Identity. Web](microsoft-identity-web.md)tarafından sağlanır. Daha sonra `AzureAD` yapılandırma dosyasında *appsettings.js* bölümünde yer alan bilgileri temel alarak Microsoft Identity platformunu kullanarak oturum açmak için yapılandırılır:

| *appsettings.js* anahtarda | Description                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | Azure portal kayıtlı uygulamanın **uygulama (istemci) kimliği** .                                                                                       |
| `Instance`             | Kullanıcının kimlik doğrulaması için güvenlik belirteci hizmeti (STS) uç noktası. Bu değer genellikle `https://login.microsoftonline.com/` Azure genel bulutu 'nı gösterir. |
| `TenantId`             | İş veya okul hesaplarıyla veya Microsoft kişisel hesaplarıyla kullanıcıların oturum açması için kiracınızın adı veya kiracı KIMLIĞI (GUID) veya *ortak* .                             |

`EnableTokenAcquisitionToCallDownstreamApi`Yöntemi, uygulamanızın korumalı Web API 'lerini çağırmak için bir belirteç almasını sağlar. `AddMicrosoftGraph` denetleyicilerinizi veya Razor sayfalarınızın doğrudan `GraphServiceClient` (bağımlılık eklenmesine göre) avantajlarından faydalanmalarını sağlar ve Yöntemler, `AddInMemoryTokenCaches` uygulamanızın belirteç önbelleğinden faydalanabilir.

`Configure()`Yöntemi, `app.UseAuthentication()` adlandırılmış işlevlerini etkinleştiren iki önemli yöntem içerir `app.UseAuthorization()` . Ayrıca `Configure()` yönteminde, Microsoft Identity Web 'in yollarını en az bir çağrısıyla `endpoints.MapControllerRoute()` veya çağrısına kaydetmeniz gerekir `endpoints.MapControllers()` .

```csharp
app.UseAuthentication();
app.UseAuthorization();

app.UseEndpoints(endpoints =>
{

    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});

// endpoints.MapControllers(); // REQUIRED if MapControllerRoute() isn't called.
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Bir denetleyiciyi veya yöntemlerini `[Authorize]` denetleyicinin sınıfına ya da bir ya da daha fazlasına uygulamayı uygulayarak koruyabilirsiniz. Bu `[Authorize]` öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek erişimi kısıtlar. Kullanıcının kimliği doğrulanmadıysa, denetleyiciye erişmek için bir kimlik doğrulama sınaması başlatılabilir. Bu hızlı başlangıçta kapsamlar yapılandırma dosyasından okunmalıdır:

```CSharp
[AuthorizeForScopes(ScopeKeySection = "DownstreamApi:Scopes")]
public async Task<IActionResult> Index()
{
    var user = await _graphServiceClient.Me.Request().GetAsync();
    ViewData["ApiResult"] = user.DisplayName;

    return View();
}
 ```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta başvurulan ASP.NET Core kodu örneğini içeren GitHub deposu, aşağıdakileri gösteren yönergeler ve daha fazla kod örneği içerir:

- Yeni bir ASP.NET Core Web uygulamasına kimlik doğrulaması ekleme
- Microsoft Graph, diğer Microsoft API 'Lerini veya kendi Web API 'lerinizi çağırın
- Yetkilendirme Ekle
- Ulusal bulutlarda veya sosyal kimliklerle Kullanıcı oturum açma

> [!div class="nextstepaction"]
> [GitHub 'da Web uygulaması öğreticileri ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
