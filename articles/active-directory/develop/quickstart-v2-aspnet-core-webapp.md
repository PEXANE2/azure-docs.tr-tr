---
title: Microsoft 'a ASP.NET Core Web Apps ile oturum açma ekleme-Microsoft Identity platform | Mavisi
description: OpenID Connect kullanarak bir ASP.NET Core Web uygulamasında Microsoft oturum açma uygulamasını nasıl uygulayacağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: fdc1f0db956d0f64938b6a0433fda21dc4462ced
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691335"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme
Bu hızlı başlangıçta, bir ASP.NET Core Web uygulamasının herhangi bir Azure Active Directory (Azure AD) örneğinden kişisel hesaplara (hotmail.com, outlook.com, diğerleri) ve iş ve okul hesaplarına nasıl oturum açabileceğinizi öğrenmek için bir kod örneği kullanırsınız. (Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.)
> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
>    - **Ad** bölümünde, örneğin, uygulamanın kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin `AspNetCore-Quickstart` .
>    - **Yeniden yönlendirme URI 'si**Içinde, `https://localhost:44321/` **Kaydet**' i ekleyin ve seçin.
> 1. **Kimlik doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Yeniden yönlendirme URI 'lerinde**, ekleyin `https://localhost:44321/signin-oidc` ve **Kaydet**' i seçin.
>    - **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** olarak ayarlayın `https://localhost:44321/signout-oidc` .
>    - **Örtük onay**'ın altında **Kimlik belirteçleri**'ni işaretleyin.
>    - **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, yanıt URL 'Lerini olarak eklemeniz `https://localhost:44321/` `https://localhost:44321/signin-oidc` , oturum kapatma URL 'sini olarak eklemeniz `https://localhost:44321/signout-oidc` ve yetkilendirme uç noktası tarafından verilecek istek kimliği belirteçlerini yüklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>2. Adım: ASP.NET Core projenizi Indirin

> [!div renderon="docs"]
> [ASP.NET Core çözümünü indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-run-your-aspnet-core-project"></a>3. Adım: ASP.NET Core projenizi çalıştırma
> 1. ZIP dosyasını kök klasör içindeki bir yerel klasöre ayıklayın; Örneğin, **C:\Azure-Samples**
> 1. Çözümü IDE 'niz içinde açın
> 1. Dosyadaki **appsettings.js** düzenleyin. `ClientId`' İ kaydettiğiniz `ClientId` uygulamanın **uygulama (istemci) kimliği** değeri ile değerini bulun ve güncelleştirin.
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here"
>    "TenantId": "Enter_the_Tenant_Info_Here"
>    ```



> [!div renderon="docs"]
> Burada:
> - `Enter_the_Application_Id_here` -Azure portal kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** . Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliğini** bulabilirsiniz.
> - `Enter_the_Tenant_Info_Here` -Aşağıdaki seçeneklerden biridir:
>   - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları**destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölüm, kullanıcıların oturum açması için gereken koda genel bir bakış sağlar. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenlerin ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız yararlı olabilir.

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft. AspNetCore. Authentication* ara yazılımı, barındırma işlemi başlatıldığında yürütülen bir başlangıç sınıfı kullanır:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<CookiePolicyOptions>(options =>
  {
    // This lambda determines whether user consent for non-essential cookies is needed for a given request.
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.None;
  });

  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
          .AddAzureAD(options => Configuration.Bind("AzureAd", options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
    options.Authority = options.Authority + "/v2.0/";         // Microsoft identity platform

    options.TokenValidationParameters.ValidateIssuer = false; // accept several tenants (here simplified)
  });

  services.AddMvc(options =>
  {
     var policy = new AuthorizationPolicyBuilder()
                     .RequireAuthenticatedUser()
                     .Build();
     options.Filters.Add(new AuthorizeFilter(policy));
  })
  .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

Yöntemi, `AddAuthentication` hizmetini tarayıcı senaryolarında kullanılan ve kimlik doğrulama tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır ve bunu OpenID Connect olarak ayarlar.

İçeren çizgi, `.AddAzureAd` uygulamanıza Microsoft Identity platform kimlik doğrulamasını ekler. Daha sonra Microsoft Identity platform uç noktası kullanılarak oturum açmak üzere yapılandırılmıştır.

> |Konum | Açıklama |
> |---------|---------|
> | ClientId  | Azure portal kayıtlı uygulamadan uygulama (istemci) KIMLIĞI. |
> | Yetkili | Kullanıcının kimlik doğrulaması için STS uç noktası. Genellikle, bu `https://login.microsoftonline.com/{tenant}/v2.0` genel buluta yöneliktir; burada {Tenant} kiracınızın adı, KIRACı kimliğiniz veya ortak uç noktaya yönelik bir başvuru için *ortaktır* (çok kiracılı uygulamalar için kullanılır) |
> | TokenValidationParameters | Belirteç doğrulaması için parametre listesi. Bu durumda, `ValidateIssuer` `false` kişisel veya iş veya okul hesaplarından oturum açma işlemlerini kabul edemeyeceğini belirtmek için olarak ayarlanır. |


> [!NOTE]
> Ayar `ValidateIssuer = false` , bu hızlı başlangıç için bir basitleştirmesi. Gerçek uygulamalarda, sertifikayı vereni doğrulamanız gerekir.
> Bunun nasıl yapılacağını anlamak için örneklere bakın.
>
> Ayrıca `Configure` iki önemli yöntem içeren yöntemine dikkat edin: `app.UseCookiePolicy()` ve `app.UseAuthentication()`

```csharp
// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // more core
    app.UseCookiePolicy();
    app.UseAuthentication();
    // more core
}
```

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Özniteliğini kullanarak bir denetleyiciyi veya denetleyici yöntemlerini koruyabilirsiniz `[Authorize]` . Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar. Bu, kullanıcının kimlik doğrulaması yapılmadığı takdirde denetleyiciye erişmek için kimlik doğrulama sınaması başlatılabileceği anlamına gelir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir ASP.NET Core Web uygulamasına kimlik doğrulaması ekleme, Microsoft Graph ve diğer Microsoft API 'Lerinin nasıl çağrılacağını, kendi API 'lerinizi nasıl çağrılacağını, kimlik doğrulamasının nasıl yapılacağını, Ulusal bulutlarda Kullanıcı oturumunu nasıl kullanacağınızı veya sosyal kimlik ve daha fazlasını içeren bu ASP.NET Core öğreticisine ilişkin daha fazla bilgi edinmek için GitHub deposuna göz atın :

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması öğreticisi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
