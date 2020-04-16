---
title: Core web uygulamalarını ASP.NET Için Microsoft ile oturum açma ekleme - Microsoft kimlik platformu | Azure
description: OpenID Connect'i kullanarak Microsoft Oturum Açma'yı ASP.NET Bir Web Uygulamasında nasıl uygulayacağınızı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: ac53a6cf6953255f51a6b586703147c5ce2338b6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407147"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı başlangıç: ASP.NET Core web uygulamasına Microsoft ile oturum açma ekleme
Bu hızlı başlangıçta, bir ASP.NET Core web uygulamasının herhangi bir Azure Etkin Dizin (Azure AD) örneğinden kişisel hesaplarda (hotmail.com, outlook.com, diğerleri) ve iş ve okul hesaplarında nasıl oturum açabileceğini öğrenmek için bir kod örneği kullanırsınız. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)
> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Azure portalına gidin [- Uygulama kayıtları.](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Başvurunuzu kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için aşağıdaki adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
>    - **Ad** bölümüne, örneğin `AspNetCore-Quickstart`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>    - **URI'yi Yeniden** `https://localhost:44321/`Yönlendirme'de, ekle ve **Kaydol'u**seçin.
> 1. Kimlik **Doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Url'leri Yeniden**Yönlendirme'de, ekle `https://localhost:44321/signin-oidc`ve **Kaydet'i**seçin.
>    - Gelişmiş **ayarlar** **bölümünde, Giriş URL'sini** ' ye `https://localhost:44321/signout-oidc`ayarla.
>    - **Örtük onay**'ın altında **Kimlik belirteçleri**'ni işaretleyin.
>    - **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Adım 1: Uygulamanızı Azure portalında yapılandırın
> Bu hızlı başlatmanın işe yaraması için kod örneğinin çalışabilmek için, giriş `https://localhost:44321/signout-oidc`URL'lerini "Giriş URL'si" olarak `https://localhost:44321/` `https://localhost:44321/signin-oidc`eklemeniz ve yetkilendirme bitiş noktası tarafından düzenlenecek kimlik belirteçlerini istemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>Adım 2: ASP.NET Core projenizi indirin

> [!div renderon="docs"]
> [Visual Studio 2019 çözümlerini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Visual Studio 2019'u kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Adım 3: Uygulamanız yapılandırıldı ve çalışmaya hazır
> Projenizi uygulamanızın özellikleriyle yapılandırdık ve çalışmaya hazır. 
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-run-your-visual-studio-project"></a>Adım 3: Visual Studio projenizi çalıştırın
> 1. Zip dosyasını kök klasörü içindeki yerel bir klasöre ayıklama - örneğin, **C:\Azure-Örnekler**
> 1. Visual Studio'da çözümü açın 
> 1. **appsettings.json** dosyasını edin. Kaydettiğiniz uygulamanın `ClientId` Uygulama **(istemci) kimlik** değeri ile değerini bulun `ClientId` ve güncelleştirin. 
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here"
>    "TenantId": "Enter_the_Tenant_Info_Here"
>    ```



> [!div renderon="docs"]
> Konumlar:
> - `Enter_the_Application_Id_here`- Azure portalına kaydettiğiniz uygulamanın **Uygulama (istemci) kimliğidir.** **Uygulama (istemci) kimliğini** uygulamanın Genel **Bakış** sayfasında bulabilirsiniz.
> - `Enter_the_Tenant_Info_Here`- aşağıdaki seçeneklerden biridir:
>   - Uygulamanız **hesapları yalnızca bu kuruluş dizininde**destekliyorsa, bu değeri Kiracı Kimliği veya **Kiracı** **adı** ile değiştirin (örneğin, contoso.microsoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, oturum açılmak için kullanıcıların güvenliği Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenleri ve ayrıca varolan bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız anlamak için yararlı olabilir.

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft.AspNetCore.Authentication* ara ware barındırma işlemi başlattığında yürütülen bir Başlangıç sınıfı kullanır:

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

Yöntem, `AddAuthentication` hizmeti tarayıcı senaryolarında kullanılan çerez tabanlı kimlik doğrulaması eklemek ve sorunu OpenID Connect'e ayarlamak için yapılandırır. 

İçeren `.AddAzureAd` satır, uygulamanıza Microsoft kimlik platformu kimlik doğrulamasını ekler. Daha sonra Microsoft kimlik platformu bitiş noktasını kullanarak oturum açacak şekilde yapılandırılır.

> |Konum  |  |
> |---------|---------|
> | ClientId  | Azure portalına kayıtlı uygulamanın uygulama (istemci) kimliği. |
> | Yetkili | Kullanıcının kimlik doğrulaması için STS bitiş noktası. Genellikle bu, <https://login.microsoftonline.com/{tenant}/v2.0> {kiracı} kiracınızın veya kiracı kimliğinizin adı olduğu veya ortak bitiş noktasına başvuru için *yaygın* olan (çok kiracılı uygulamalar için kullanılan) genel bulut içindir. |
> | TokenValidationParametreleri | Belirteç doğrulaması için parametre listesi. Bu durumda, `ValidateIssuer` herhangi `false` bir kişisel veya iş veya okul hesaplarından oturum açma kabul edebilir belirtmek için ayarlanır. |


> [!NOTE]
> Ayar, `ValidateIssuer = false` bu hızlı başlatma için basitleştirmedir. Gerçek uygulamalarda veren doğrulamak gerekir.
> Bunu nasıl yapacağımı anlamak için örneklere bakın.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Özniteliği kullanarak `[Authorize]` bir denetleyici veya denetleyici yöntemlerini koruyabilirsiniz. Bu öznitelik, yalnızca kimlik doğrulaması yapılan kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar, bu da kullanıcının kimliği doğrulanmıyorsa kimlik doğrulama zorluğunun denetleyiciye erişmeye başlanabileceği anlamına gelir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Yepyeni bir ASP.NET Core Web uygulamasına kimlik doğrulamanın nasıl ekleyeceğiniz, Microsoft Graph'ı ve diğer Microsoft API'lerini nasıl arayacağınız, kendi API'lerinizi nasıl arayacağınız, yetkilendirme nasıl ekleyeceğiniz, ulusal bulutlardaki kullanıcılarda nasıl oturum açacağınız veya sosyal kimliklerle ve daha fazlası dahil olmak üzere daha fazla bilgi için bu ASP.NET Core öğreticisi için GitHub repo'ya göz atın :

> [!div class="nextstepaction"]
> [ASP.NET Core Web App öğretici](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
