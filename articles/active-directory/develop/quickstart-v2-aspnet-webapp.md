---
title: Microsoft kimlik platformu oturum açma ASP.NET bir web uygulamasına ekleme | Azure
description: OpenID Connect'i kullanarak ASP.NET bir web uygulamasında Microsoft oturum açma uygulamasını nasıl uygulayacağınızı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 2946551233500a80c6fa7f02ad4c427f2d654b40
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80883653"
---
# <a name="quickstart-add-microsoft-identity-platform-sign-in-to-an-aspnet-web-app"></a>Hızlı başlangıç: ASP.NET bir web uygulamasına Microsoft kimlik platformu oturum açma ekleme
Bu hızlı başlangıçta, ASP.NET bir web uygulamasının herhangi bir Azure Etkin Dizin (Azure AD) örneğinden kişisel hesaplarda (hotmail.com, outlook.com, diğerleri) ve iş ve okul hesaplarında nasıl oturum açabildiğini öğrenmek için bir kod örneği kullanırsınız.  (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)
> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
>      - **Ad** bölümüne, örneğin `ASPNET-Quickstart`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - `http://localhost:44368/` **URI'yi Yeniden Yönlendirme'ye**ekleyin ve **Kaydol'u**tıklatın.
>      - Yönet bölümünün altındaki sol gezinti bölmesinden **Kimlik Doğrulama'yı** seçin
>          - Örtülü **Hibe** alt bölümü **altında, kimlik belirteçleri**seçin.
>          - Ve sonra **Kaydet'i**seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için `https://localhost:44368/` gibi bir yanıt URL’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu öznitelikle yapılandırılmış

#### <a name="step-2-download-your-project"></a>2. Adım: Projenizi indirme

> [!div renderon="docs"]
> [Visual Studio 2019 çözümlerini indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip)

> [!div renderon="portal"]
> Visual Studio 2019'u kullanarak projeyi çalıştırın.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Adım 3: Uygulamanız yapılandırıldı ve çalışmaya hazır
> Projenizi uygulamanızın özellikleriyle yapılandırdık. 

> [!div renderon="docs"]
> #### <a name="step-3-run-your-visual-studio-project"></a>Adım 3: Visual Studio projenizi çalıştırın

1. Zip dosyasını kök klasöre yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) açın
1. Çözümü Visual Studio’da açın (AppModelv2-WebApp-OpenIDConnect-DotNet.sln)
1. Visual Studio sürümüne bağlı olarak, projeye `AppModelv2-WebApp-OpenIDConnect-DotNet` sağ tıklamanız ve **NuGet paketlerini geri yüklemeniz** gerekebilir
1. Paket Yöneticisi Konsolu'nu açın (Görünüm -> Diğer Windows -> Paket Yöneticisi Konsolu) ve çalıştırın`Update-Package Microsoft.CodeDom.Providers.DotNetCompilerPlatform -r`

> [!div renderon="docs"]
> 5. **Web.config** dosyasını düzenleyip `ClientId` ve `Tenant` parametrelerini şu şekilde değiştirin:
>    ```xml
>    <add key="ClientId" value="Enter_the_Application_Id_here" />
>    <add key="Tenant" value="Enter_the_Tenant_Info_Here" />
>    ```
>    Konumlar:
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Tenant_Info_Here` - Aşağıdaki seçeneklerden biridir:
>   - Başvurunuz **yalnızca Kuruluşumu**destekliyorsa, bu değeri **Kiracı Kimliği** veya **Kiracı adı** ile değiştirin (örneğin, contoso.onmicrosoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > - *Uygulama Kimliği*, *Dizin (kiracı) Kimliği* ve *Desteklenen hesap türleri* değerlerini bulmak için **Genel bakış** sayfasına gidin
> > - `redirectUri` **Web.config'deki** değerin Azure AD'deki Uygulama Kaydı için tanımlanan **Yönlendirme URI'ye** karşılık gelen değerolduğundan emin olun (değilse, Uygulama Kaydı için **Kimlik Doğrulama** menüsüne gidin ve **YÖNLENDIRME URI'sini** eşleşecek şekilde güncelleyin)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, kullanıcıların oturumunu açmak için gereken koda genel bir bakış sağlanır. Bu genel bakış, kodun nasıl çalıştığını, ana bağımsız değişkenleri ve ayrıca varolan bir ASP.NET uygulamasına oturum açma eklemek istiyorsanız anlamak için yararlı olabilir.

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-webapp/aspnetwebapp-intro.svg)

### <a name="owin-middleware-nuget-packages"></a>OWIN ara yazılımı NuGet paketleri

OWIN Ara Yazılımı paketleriyle ASP.NET’te OpenID Connect’i kullanarak, tanımlama bilgisi tabanlı kimlik doğrulamasıyla kimlik doğrulaması işlem hattını ayarlayabilirsiniz. Visual Studio’nun **Paket Yöneticisi Konsolu**’nda şu komutları çalıştırarak bu paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb  
```

### <a name="owin-startup-class"></a>OWIN Startup Sınıfı

OWIN ara yazılım, barındırma işlemi başlattığında çalışan bir *başlangıç sınıfı* kullanır. Bu hızlı başlatmada, startup.cs *dosyası* kök klasöründe bulunur. Aşağıdaki kod bu hızlı başlangıcın kullandığı parametreyi gösterir:

```csharp
public void Configuration(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());
    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            // Sets the ClientId, authority, RedirectUri as obtained from web.config
            ClientId = clientId,
            Authority = authority,
            RedirectUri = redirectUri,
            // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
            PostLogoutRedirectUri = redirectUri,
            Scope = OpenIdConnectScope.OpenIdProfile,
            // ResponseType is set to request the id_token - which contains basic information about the signed-in user
            ResponseType = OpenIdConnectResponseType.IdToken,
            // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
            // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
            // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
            TokenValidationParameters = new TokenValidationParameters()
            {
                ValidateIssuer = false // Simplification (see note below)
            },
            // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = OnAuthenticationFailed
            }
        }
    );
}
```

> |Konum  |  |
> |---------|---------|
> | `ClientId`     | Azure portalına kaydedilen uygulamaya ait Uygulama Kimliği |
> | `Authority`    | Kimlik doğrulaması yapılacak kullanıcı için STS uç noktası. Çoğunlukla, genel bulut için <https://login.microsoftonline.com/{tenant}/v2.0>; burada {tenant}, kiracınızın adı, kiracınızın kimliği veya ortak uç noktaya başvuru olarak *common* değeridir (çok kiracılı uygulamalarda kullanılır) |
> | `RedirectUri`  | Microsoft kimlik platformu bitiş noktasına karşı kimlik doğrulamadan sonra kullanıcıların gönderildiği URL |
> | `PostLogoutRedirectUri`     | Oturum kapatıldıktan sonra kullanıcıların gönderildiği URL |
> | `Scope`     | İstenen kapsamların listesi, boşlukla ayrılmış |
> | `ResponseType`     | Kimlik doğrulamadan gelen yanıtın kimlik belirteci içerdiği istek |
> | `TokenValidationParameters`     | Belirteç doğrulaması için parametre listesi. Bu durumda, tüm kişisel, iş veya okul hesabı türleriyle yapılan okurum açma işlemlerini kabul edebileceğini belirtmek için `ValidateIssuer``false` olarak ayarlanır |
> | `Notifications`     | Farklı *OpenIdConnect* iletilerinde yürütülebilen temsilcilerin listesi |


> [!NOTE]
> Ayar, `ValidateIssuer = false` bu hızlı başlatma için basitleştirmedir. Gerçek uygulamalarda veren doğrulamak gerekir.
> Bunu nasıl yapacağımı anlamak için örneklere bakın.

### <a name="initiate-an-authentication-challenge"></a>Kimlik doğrulaması sınamasını başlatma

Denetleyicinizde kimlik doğrulaması sınaması isteyerek kullanıcıyı oturum açmaya zorlayabilirsiniz:

```csharp
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

> [!TIP]
> Yukarıdaki yöntem kullanılarak kimlik doğrulaması sınamasının istenmesi isteğe bağlıdır ve normalde hem kimlik doğrulamalı hem de kimlik doğrulamasız kullanıcıların erişebildiği bir görünüm istediğinizde kullanılır. Alternatif olarak, bir sonraki bölümde açıklanan yöntemle denetleyicileri koruyabilirsiniz.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Denetleyiciyi veya denetleyici eylemlerini `[Authorize]` özniteliğini kullanarak koruyabilirsiniz. Bu öznitelik, yalnızca kimliği doğrulanmış kullanıcıların denetleyicideki eylemlere erişmesine izin vererek denetleyiciye veya eylemlere erişimi kısıtlar. Başka bir deyişle kimlik doğrulama sınaması, *kimliği doğrulanmamış* bir kullanıcı `[Authorize]` özniteliğini içeren eylemlerden birine veya denetleyiciye erişmeyi denediğinde otomatik olarak gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için ASP.NET öğreticisini deneyin.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Oturum açma öğreticisi](./tutorial-v2-asp-webapp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
