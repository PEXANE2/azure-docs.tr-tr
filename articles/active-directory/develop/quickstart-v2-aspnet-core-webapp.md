---
title: Microsoft Identity platform ASP.NET Core Web uygulaması hızlı başlangıç | Mavisi
description: OpenID Connect kullanarak bir ASP.NET Core Web uygulamasında Microsoft oturum açma uygulamasını nasıl uygulayacağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 832fb890a91c9e87c26d8facdcba49ef9d810117
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74920744"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, bir ASP.NET Core Web uygulamasının herhangi bir Azure Active Directory (Azure AD) örneğinden kişisel hesaplara (hotmail.com, outlook.com, diğerleri) ve iş ve okul hesaplarına nasıl oturum açabiltireceğinizi öğreneceksiniz.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AspNetCore-Quickstart`.
>    - **Yeniden yönlendirme URI 'si**içinde `https://localhost:44321/`ekleyin ve **Kaydet**' i seçin.
> 1. **Kimlik doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Yeniden yönlendirme URI 'lerinde**`https://localhost:44321/signin-oidc`ekleyin ve **Kaydet**' i seçin.
>    - **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** `https://localhost:44321/signout-oidc`olarak ayarlayın.
>    - **Örtük onay**'ın altında **Kimlik belirteçleri**'ni işaretleyin.
>    - **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1\. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için, yanıt URL 'Lerini `https://localhost:44321/` ve `https://localhost:44321/signin-oidc`olarak eklemeniz, oturum kapatma URL 'sini `https://localhost:44321/signout-oidc`olarak eklemeniz ve yetkilendirme uç noktası tarafından verilecek istek KIMLIĞI belirteçlerini yüklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>2\. Adım: ASP.NET Core projenizi Indirin

- [Visual Studio 2019 çözümünü indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3\. Adım: Visual Studio projenizi yapılandırma

1. ZIP dosyasını kök klasör içindeki bir yerel klasöre ayıklayın; Örneğin, **C:\Azure-Samples**
1. Visual Studio 2019 kullanıyorsanız, Visual Studio 'da çözümü açın (isteğe bağlı).
1. **AppSettings. JSON** dosyasını düzenleyin. `ClientId` bulun ve `ClientId` değerini, kaydettiğiniz uygulamanın **uygulama (istemci) kimliği** değeriyle güncelleştirin. 

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "Enter_the_Tenant_Info_Here"
    ```

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıç Enter_the_Supported_Account_Info_Here destekler.

> [!div renderon="docs"]
> Nerede:
> - `Enter_the_Application_Id_here`-Azure portal kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** . Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliğini** bulabilirsiniz.
> - `Enter_the_Tenant_Info_Here`-aşağıdaki seçeneklerden biridir:
>   - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları**destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.Microsoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölüm, kullanıcıların oturum açması için gereken koda genel bir bakış sağlar. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenlerin ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız yararlı olabilir.

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

Yöntemi `AddAuthentication`, hizmetini tarayıcı senaryolarında kullanılan ve kimlik doğrulama tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır ve bunu OpenID Connect olarak ayarlar. 

`.AddAzureAd` içeren çizgi, Microsoft kimlik platformu kimlik doğrulamasını uygulamanıza ekler. Daha sonra Microsoft Identity platform uç noktası kullanılarak oturum açmak üzere yapılandırılmıştır.

> |Konum  |  |
> |---------|---------|
> | ClientId  | Azure portal kayıtlı uygulamadan uygulama (istemci) KIMLIĞI. |
> | Yetkili | Kullanıcının kimlik doğrulaması için STS uç noktası. Genellikle bu, genel bulut için <https://login.microsoftonline.com/{tenant}/v2.0>; burada {Tenant}, kiracınızın adı veya kiracı KIMLIĞINIZ veya ortak uç noktaya yönelik bir başvuru için *ortaktır* (çok kiracılı uygulamalar için kullanılır) |
> | TokenValidationParameters | Belirteç doğrulaması için parametre listesi. Bu durumda, tüm kişisel veya okul hesaplarından oturum açma işlemlerini kabul edemeyeceğini göstermek için `ValidateIssuer` `false` olarak ayarlanır. |


> [!NOTE]
> Bu hızlı başlangıç için `ValidateIssuer = false` ayarı basitleştirmesi. Gerçek uygulamalarda, sertifikayı vereni doğrulamanız gerekir.
> Bunun nasıl yapılacağını anlamak için örneklere bakın.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

`[Authorize]` özniteliğini kullanarak bir denetleyiciyi veya denetleyici yöntemlerini koruyabilirsiniz. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar. Bu, kullanıcının kimlik doğrulaması yapılmadığı takdirde denetleyiciye erişmek için kimlik doğrulama sınaması başlatılabileceği anlamına gelir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir ASP.NET Core Web uygulamasına kimlik doğrulaması ekleme, Microsoft Graph ve diğer Microsoft API 'Lerinin nasıl çağrılacağını, kendi API 'lerinizi nasıl çağrılacağını, nasıl ekleneceğini ve nasıl ekleneceğini açıklayan yönergeler dahil olmak üzere bu ASP.NET Core için GitHub deposuna göz atın. Yetkilendirme, Ulusal bulutlarda Kullanıcı oturumu açma veya sosyal kimlikler ve daha fazlası:

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması öğreticisi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)