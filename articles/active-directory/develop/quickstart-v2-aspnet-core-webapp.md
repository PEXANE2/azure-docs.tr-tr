---
title: "Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme | Mavisi"
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, bir uygulamanın OpenID Connect kullanarak bir ASP.NET Core Web uygulamasında Microsoft oturum açma işlemini nasıl uyguladığını öğrenirsiniz.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/11/2020
ms.author: jmprieur
ms.custom: devx-track-csharp, aaddev, identityplatformtop40, scenarios:getting-started, languages:aspnet-core
ms.openlocfilehash: d0d3154d123b5e073a4eadf976d5259d51972da8
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102436490"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı başlangıç: Microsoft 'a ASP.NET Core Web uygulamasına oturum açma ekleme

Bu hızlı başlangıçta, bir ASP.NET Core Web uygulamasının kullanıcılara herhangi bir Azure Active Directory (Azure AD) kuruluştan nasıl oturum açıp çalıştırabileceğinizi gösteren bir kod örneği indirip çalıştırırsınız.  

> [!div renderon="docs"]
> Aşağıdaki diyagramda örnek uygulamanın nasıl çalıştığı gösterilmektedir:
>
> ![Web tarayıcısı, Web uygulaması ve örnek uygulamadaki Microsoft Identity platformu arasındaki etkileşimin diyagramı.](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)
>
> ## <a name="prerequisites"></a>Önkoşullar
>
> * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) veya [Visual Studio Code](https://code.visualstudio.com/)
> * [.NET Core SDK 3.1 +](https://dotnet.microsoft.com/download)
>
> ## <a name="register-and-download-the-app"></a>Uygulamayı kaydedin ve indirin
> Uygulamanızı oluşturmaya başlamak için iki seçeneğiniz vardır: otomatik veya el ile yapılandırma.
>
> ### <a name="automatic-configuration"></a>Otomatik yapılandırma
> Uygulamanızı otomatik olarak yapılandırmak ve ardından kod örneğini indirmek istiyorsanız aşağıdaki adımları izleyin:
>
> 1. <a href="https://aka.ms/aspnetcore2-1-aad-quickstart-v2/" target="_blank">Uygulama kaydı için Azure Portal sayfasına</a>gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı tek tıklamayla indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="manual-configuration"></a>El ile yapılandırma
> Uygulamanızı ve kod örneğinizi el ile yapılandırmak istiyorsanız aşağıdaki yordamları kullanın.
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> 1. <a href="https://portal.azure.com/" target="_blank">Azure portalında</a> oturum açın.
> 1. Birden çok kiracıya erişiminiz varsa, uygulamayı kaydetmek istediğiniz kiracıyı seçmek için üst menüdeki **Dizin + abonelik** filtresini kullanın :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: .
> 1. **Azure Active Directory**'yi bulun ve seçin.
> 1. **Yönet** altında   >  **Yeni kayıt** uygulama kayıtları ' yi seçin.
> 1. **Ad** için, uygulamanız için bir ad girin. Örneğin, **Aspnetcore-QuickStart** yazın. Uygulamanızın kullanıcıları bu adı görür ve daha sonra değiştirebilirsiniz.
> 1. **Yeniden yönlendirme URI 'si** için girin **https://localhost:44321/signin-oidc** .
> 1. **Kaydet**’i seçin.
> 1. **Yönet** altında **kimlik doğrulaması**' nı seçin.
> 1. **Ön kanal oturum kapatma URL 'si** için girin **https://localhost:44321/signout-oidc** .
> 1. **Örtük verme ve karma akışlar** altında **Kimlik belirteçleri**' ni seçin.
> 1. **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için:
> - **Yeniden yönlendirme URI 'si** için **https://localhost:44321/** ve girin **https://localhost:44321/signin-oidc** .
> - **Ön kanal oturum kapatma URL 'si** için girin **https://localhost:44321/signout-oidc** . 
>
> Yetkilendirme uç noktası, istek KIMLIĞI belirteçleri oluşturacak.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-aspnet-core-project"></a>2. Adım: ASP.NET Core projesini Indirin

> [!div renderon="docs"]
> [ASP.NET Core çözümünü indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div renderon="portal" class="sxs-lookup"]
> Projeyi çalıştırın.

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore3-1.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
> Projenizi uygulamanızın özelliklerinin değerleriyle yapılandırdık ve çalıştırılmaya hazırlanıyor.
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`
> [!div renderon="docs"]
> #### <a name="step-3-configure-your-aspnet-core-project"></a>3. Adım: ASP.NET Core projenizi yapılandırma
> 1. . Zip arşivini sürücünüzün köküne yakın bir yerel klasöre ayıklayın. Örneğin, *C:\Azure-Samples*' a ayıklayın.
> 
>    Windows üzerinde yol uzunluğu sınırlamalarından kaynaklanan hataları önlemek için, arşivin sürücünüzün köküne yakın bir dizine ayıklanmasını öneririz.
> 1. Visual Studio 2019 ' de çözümü açın.
> 1. appsettings.jsdosya *üzerinde* açın ve aşağıdaki kodu değiştirin:
>
>    ```json
>    "ClientId": "Enter_the_Application_Id_here",
>    "TenantId": "common",
>    ```
>
>    - `Enter_the_Application_Id_here`Azure Portal kaydettiğiniz uygulamanın uygulama (istemci) kimliğiyle değiştirin. Uygulamanın **genel bakış** sayfasında **uygulama (istemci) kimliği** değerini bulabilirsiniz.
>    - `common`Aşağıdakilerden biriyle değiştirin:
>       - Uygulamanız **yalnızca bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri dizin (KIRACı) KIMLIĞI (GUID) veya kiracı adı (örneğin,) ile değiştirin `contoso.onmicrosoft.com` . **Dizin (kiracı) kimliği** değerini uygulamanın **genel bakış** sayfasında bulabilirsiniz.
>       - Uygulamanız **herhangi bir kuruluş dizinindeki hesapları** destekliyorsa, bu değeri ile değiştirin `organizations` .
>       - Uygulamanız **tüm Microsoft hesabı kullanıcıları** destekliyorsa, bu değeri olarak bırakın `common` .
>
> Bu hızlı başlangıçta, dosyadaki *appsettings.js* başka bir değeri değiştirmeyin.
>
> #### <a name="step-4-build-and-run-the-application"></a>4. Adım: Uygulamayı derleme ve çalıştırma
>
> Hata **ayıklamayı başlat**> veya F5 tuşuna basarak **hata ayıklama** menüsünü seçerek uygulamayı Visual Studio 'da derleyin ve çalıştırın.
>
> Kimlik bilgileriniz istenir ve ardından uygulamanızın gerektirdiği izinleri onaylıyoruz. Onay isteminde **kabul et** ' i seçin.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-01-consent.png" alt-text="Uygulamanın kullanıcıdan istediği izinleri gösteren izin iletişim kutusunun ekran görüntüsü.":::
>
> İstenen izinleri kabul ettikten sonra uygulama, Azure Active Directory kimlik bilgilerinizle başarıyla oturum açdığınızı gösterir.
>
> :::image type="content" source="media/quickstart-v2-aspnet-core-webapp/webapp-02-signed-in.png" alt-text="Çalışan Web uygulamasını ve oturum açmış kullanıcıyı gösteren bir Web tarayıcısının ekran görüntüsü.":::

## <a name="more-information"></a>Daha fazla bilgi

Bu bölüm, kullanıcıların oturum açması için gereken koda genel bir bakış sağlar. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenlerin ne olduğunu ve mevcut bir ASP.NET Core uygulamasına nasıl oturum ekleneceğini anlamak için yararlı olabilir.

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı
>
> ![Web tarayıcısı, Web uygulaması ve örnek uygulamadaki Microsoft Identity platformu arasındaki etkileşimin diyagramı.](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft. AspNetCore. Authentication* ara yazılımı, `Startup` barındırma işlemi başladığında çalıştırılan bir sınıfı kullanır:

```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
          .AddMicrosoftIdentityWebApp(Configuration.GetSection("AzureAd"));

      services.AddControllersWithViews(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
          options.Filters.Add(new AuthorizeFilter(policy));
      });
      services.AddRazorPages()
          .AddMicrosoftIdentityUI();
  }
```

`AddAuthentication()`Yöntemi, hizmeti tanımlama bilgisi tabanlı kimlik doğrulaması ekleyecek şekilde yapılandırır. Bu kimlik doğrulaması tarayıcı senaryolarında kullanılır ve sınamayı OpenID Connect olarak ayarlar.

İçeren çizgi, `.AddMicrosoftIdentityWebApp` uygulamanıza Microsoft Identity platform kimlik doğrulaması ekler. Uygulama daha sonra `AzureAD` yapılandırma dosyasında *appsettings.js* bölümünde aşağıdaki bilgilere dayanarak kullanıcıları oturum açmak üzere yapılandırılır:

| *appsettings.js* anahtarda | Açıklama                                                                                                                                                          |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `ClientId`             | Azure portal kayıtlı uygulamanın uygulama (istemci) KIMLIĞI.                                                                                       |
| `Instance`             | Kullanıcının kimlik doğrulaması için güvenlik belirteci hizmeti (STS) uç noktası. Bu değer genellikle `https://login.microsoftonline.com/` Azure genel bulutu 'nı gösterir. |
| `TenantId`             | Kiracınızın veya kiracı KIMLIĞININ adı (bir GUID) ya da `common` iş veya okul hesaplarıyla veya Microsoft kişisel hesaplarıyla kullanıcıları oturum açın.                             |

`Configure()`Yöntemi, `app.UseAuthentication()` adlandırılmış işlevlerini etkinleştiren iki önemli yöntem içerir `app.UseAuthorization()` . Ayrıca `Configure()` yönteminde, Microsoft Identity Web yollarını en az bir çağrısıyla `endpoints.MapControllerRoute()` veya bir çağrısına kaydetmeniz gerekir `endpoints.MapControllers()` :

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

### <a name="attribute-for-protecting-a-controller-or-methods"></a>Denetleyiciyi veya yöntemleri koruma özniteliği

Özniteliğini kullanarak bir denetleyiciyi veya denetleyici yöntemlerini koruyabilirsiniz `[Authorize]` . Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılara izin vererek denetleyiciye veya yöntemlere erişimi kısıtlar. Kullanıcının kimliği doğrulanmadıysa, denetleyiciye erişmek için bir kimlik doğrulama sınaması başlatılabilir.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu ASP.NET Core öğreticisini içeren GitHub deposu, nasıl yapılacağını gösteren yönergeler ve daha fazla kod örneği içerir:

- Yeni bir ASP.NET Core Web uygulamasına kimlik doğrulaması ekleyin.
- Microsoft Graph, diğer Microsoft API 'Lerini veya kendi Web API 'lerinizi çağırın.
- Yetkilendirme ekleyin.
- Kullanıcıları Ulusal bulutlarda veya sosyal kimliklerle oturum açın.

> [!div class="nextstepaction"]
> [GitHub 'da Web uygulaması öğreticileri ASP.NET Core](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
