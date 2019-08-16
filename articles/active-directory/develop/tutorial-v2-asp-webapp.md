---
title: Azure AD v 2.0 ASP.NET Web sunucusu Başlarken | Microsoft Docs
description: OpenID Connect standardını kullanarak geleneksel bir Web tarayıcısı tabanlı uygulama ile ASP.NET çözümünde Microsoft oturum açma uygulama
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fce6ab2c5068d1628860356a9df0dd33c73948b3
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69511959"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Microsoft 'a bir ASP.NET Web uygulamasına oturum açma ekleme

Bu kılavuzda, OpenID Connect kullanan geleneksel bir Web tarayıcı tabanlı uygulamayla ASP.NET MVC çözümü kullanılarak Microsoft ile oturum açma işlemlerinin nasıl yapılacağı gösterilmiştir.

Bu kılavuzun sonunda, uygulamanız kişisel hesapların oturum açma işlemlerini kabul edebilir, örneğin outlook.com, live.com ve diğerleri. Bu hesaplar Ayrıca, Azure Active Directory ile tümleştirilmiş herhangi bir şirketten veya kuruluştan iş ve okul hesapları da içerir.

> Bu kılavuz, Visual Studio 2019 gerektirir.  Sizde yok mu?  [Visual Studio 2019 'i ücretsiz indirin](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulamanın nasıl çalıştığı

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

Oluşturduğunuz örnek uygulama, bir kullanıcının oturum açma düğmesiyle kimlik doğrulaması yapmasını isteyen bir ASP.NET Web sitesine erişmek için tarayıcıyı kullandığınız senaryoya bağlıdır. Bu senaryoda web sayfasını oluşturma işlemlerinin çoğu sunucu tarafında gerçekleştirilmektedir.

## <a name="libraries"></a>Kitaplıklar

Bu kılavuz aşağıdaki kitaplıkları kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Uygulamanın kimlik doğrulaması için OpenIdConnect kullanmasını sağlayan ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Uygulamanın tanımlama bilgilerini kullanarak kullanıcı oturumunu korumasını sağlayan ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|OWIN tabanlı uygulamaların ASP.NET istek işlem hattını kullanarak IIS üzerinde çalışmasını sağlar|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, OpenID Connect kullanarak bir ASP.NET projesi üzerinde OWıN ara yazılımı aracılığıyla kimlik doğrulama işlem hattını yüklemek ve yapılandırmak için gereken adımlar gösterilmektedir.

> Bunun yerine bu örneğin Visual Studio projesini indirmeyi tercih etmek istiyor musunuz? [Bir proje indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) ve yürütmeden önce kod örneğini yapılandırmak için [yapılandırma adımına](#register-your-application) atlayın.

### <a name="create-your-aspnet-project"></a>ASP.NET projenizi oluşturma

1. Visual Studio 'da:`File` > `New` > `Project`
2. *C#Visual \Web*altında öğesini seçin `ASP.NET Web Application (.NET Framework)`.
3. Uygulamanızı adlandırın ve *Tamam 'a* tıklayın
4. Başvuru `Empty` eklemek`MVC` için onay kutusunu seçin ve seçin

## <a name="add-authentication-components"></a>Kimlik doğrulama bileşenleri ekleme

1. Visual Studio 'da:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Paket Yöneticisi Konsolu penceresinde aşağıdakileri yazarak *OWIN ara yazılım NuGet paketlerini* ekleyin:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Bu kitaplıklar hakkında
> Yukarıdaki kitaplıklar, tanımlama bilgisi tabanlı kimlik doğrulaması aracılığıyla OpenID Connect kullanarak çoklu oturum açma (SSO) sağlar. Kimlik doğrulaması tamamlandıktan ve kullanıcıyı temsil eden belirteç uygulamanıza gönderildikten sonra OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Daha sonra tarayıcı bu tanımlama bilgisini sonraki isteklerde kullanır, böylece Kullanıcı parolayı yeniden yazması gerekmez ve ek bir doğrulama gerekmez.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulama işlem hattını yapılandırma

Aşağıdaki adımlar, OpenID Connect kimlik doğrulamasını yapılandırmak için bir OWıN ara yazılım başlangıç sınıfı oluşturmak için kullanılır. Bu sınıf, IIS işleminiz başladığında otomatik olarak yürütülür.

> [!TIP]
> Projenizin kök klasöründe `Startup.cs` adlı bir dosya yoksa:
> 1. Projenin kök klasörüne sağ tıklayın: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Bunu, `Startup.cs` olarak adlandırın.
>
>> Seçilen sınıfın standart bir C# sınıfı değil OWIN Başlangıç Sınıfı olduğundan emin olun. Doğrulamak için ad alanının üzerinde `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` yazıp yazmadığını kontrol edin.

1. *Içine owın* ve *Microsoft. IdentityModel* başvuruları `Startup.cs`ekleyin:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.IdentityModel.Tokens;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    ```

2. Başlangıç sınıfını aşağıdaki kodla değiştirin:

    ```csharp
    public class Startup
    {
        // The Client ID is used by the application to uniquely identify itself to Azure AD.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Azure Active Directory v2.0 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect 
        /// </summary>
        /// <param name="app"></param>
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
                        ValidateIssuer = false // This is a simplification
                    },
                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```

> [!NOTE]
> Ayar `ValidateIssuer = false` , bu hızlı başlangıç için bir basitleştirmesi. Gerçek uygulamalarda, bunu nasıl yapacağınızı öğrenmek için sertifikayı vereni doğrulamanız gerekir.

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> *OpenIDConnectAuthenticationOptions* içinde sağladığınız parametreler uygulamanın Azure AD ile iletişim kurmak için kullanacağı koordinatlara benzer. OpenID Connect ara yazılımı arka planda tanımlama bilgileri kullandığından, yukarıda gösterilen kod için tanımlama bilgisi kimlik doğrulamasını da ayarlamanız gerekir. *ValidateIssuer* değeri OpenIdConnect öğesine erişimi tek bir kuruluşla sınırlamamasını söyler.
<!--end-collapse-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma isteklerini işlemek için bir denetleyici ekleme

Bu adım, oturum açma ve oturum kapatma yöntemlerini göstermek için yeni bir denetleyici oluşturmayı gösterir.

1.  `Controllers` Klasöre sağ tıklayın ve şunları seçin`Add` > `Controller`
2.  `MVC (.NET version) Controller – Empty` öğesini seçin.
3.  *Ekle* 'ye tıklayın
4.  Adlandırın ve Ekle 'ye tıklayın `HomeController`
5.  Sınıfa *Owın* başvuruları ekleyin:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```
    
6. Kodu kullanarak kimlik doğrulama sınaması başlatarak, oturum açmayı işlemek ve denetleyicinize kaydolmak için aşağıdaki iki yöntemi ekleyin:
    
    ```csharp
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties{ RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }
    
    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
    }
    ```

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Kullanıcıların oturum açma düğmesi aracılığıyla oturum açmasını sağlamak için uygulamanın giriş sayfasını oluşturma

Visual Studio'da oturum açma düğmesini eklemek ve kimlik doğrulaması sonrasında kullanıcı bilgilerini görüntülemek için yeni bir görünüm ekleyin:

1.  `Views\Home` Klasöre sağ tıklayın ve şunları seçin`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Oturum açma düğmesini de içeren aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Guide</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
            <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
            <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
            <rect x="150" y="129" width="122" height="122" fill="#F35325" />
            <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
            <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
            <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
            <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> Bu sayfa, SVG formatında siyah bir arka plana sahip bir oturum açma düğmesi ekler:<br/>![Microsoft hesabıyla oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Daha fazla oturum açma düğmesi için lütfen [Bu sayfa](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "marka yönergelerine")gidin.
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Kullanıcının taleplerini göstermek için bir denetleyici ekleme
Bu denetleyici bir denetleyiciyi koruma amacıyla `[Authorize]` özniteliğini kullanma şeklini gösterir. Bu öznitelik yalnızca kimliği doğrulanan kullanıcılara izin vererek denetleyici erişimini sınırlar. Aşağıdaki kod, oturum açma özelliğinin bir parçası olarak alınan kullanıcı taleplerini göstermek için özniteliğini kullanır.

1.  `Controllers` Klasöre sağ tıklayın:`Add` > `Controller`
2.  `MVC {version} Controller – Empty` öğesini seçin.
3.  *Ekle* 'ye tıklayın
4.  Bunu, `ClaimsController` olarak adlandırın.
5.  Controller sınıfınızın kodunu aşağıdaki kodla değiştirin; bu `[Authorize]` özniteliği sınıfına ekler:

    ```csharp
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;
    
            //You get the user’s first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;
    
            // The 'preferred_username' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;
    
            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
    
            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;
    
            return View();
        }
    }
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> `[Authorize]` özniteliğinin kullanılması nedeniyle bu denetleyicinin tüm metotları yalnızca kullanıcının kimliğinin doğrulanması durumunda yürütülebilir. Kullanıcının kimliği doğrulanmıyorsa ve denetleyiciye erişmeye çalışırsa, OWıN bir kimlik doğrulama sınaması başlatacak ve kullanıcıyı kimlik doğrulamaya zorlayacaktır. Yukarıdaki kod, kullanıcının kimlik belirtecine dahil edilen belirli kullanıcı özniteliklerinin talepler listesine bakar. Bu öznitelik kullanıcının tam adını, kullanıcı adını ve genel kullanıcı tanımlayıcısı nesnesini içerir. Ayrıca kullanıcının kuruluşunun kimliğini temsil eden *Kiracı Kimliği* değerini de içerir. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcının taleplerini görüntülemek için bir görünüm oluşturun

Visual Studio'da kullanıcının taleplerini bir web sayfasında görüntülemek için yeni bir görünüm oluşturun:

1.  `Views\Claims` Klasöre sağ tıklayın ve:`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
        <h3>Main Claims:</h3>
        <table class="table table-striped table-bordered table-hover">
            <tr><td>Name</td><td>@ViewBag.Name</td></tr>
            <tr><td>Username</td><td>@ViewBag.Username</td></tr>
            <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
            <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
        </table>
        <br />
        <h3>All Claims:</h3>
        <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
        </table>
        <br />
        <br />
        @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```

## <a name="register-your-application"></a>Uygulamanızı kaydedin

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze eklemek için iki seçeneğiniz vardır:

### <a name="option-1-express-mode"></a>Seçenek 1: Hızlı mod

Aşağıdakileri yaparak uygulamanızı hızlı bir şekilde kaydedebilirsiniz:

1. Yeni [Azure Portal-uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesine gidin.
1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:

1. Visual Studio 'ya gidin ve:
   1. Çözüm Gezgini, projeyi seçin ve Özellikler penceresi bakın (Özellikler penceresi görmüyorsanız F4 tuşuna basın).
   1. SSL etkin olarak `True`değiştirin.
   1. Visual Studio 'da projeye sağ tıklayın, ardından **Özellikler**' i ve **Web** sekmesi ' ni seçin. *Sunucular* bölümünde *proje URL* 'sini SSL URL 'si olacak şekilde değiştirin.
   1. SSL URL 'sini kopyalayın. Sonraki adımda, bu URL 'YI kayıt portalının yeniden yönlendirme URL 'leri listesine ekleyeceksiniz:<br/><br/>![Proje Özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
1. **Yeni kayıt**seçeneğini belirleyin.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `ASPNET-Tutorial`.
   1. Visual Studio 'dan, `https://localhost:44368/` **yanıt URL**'sindeki adım 1 ' de (örneğin), kopyaladığınız SSL URL 'sini ekleyin ve **Kaydet**' e tıklayın.
1. **Kimlik doğrulaması** menüsünü seçin, **Örtük izin verme** bölümünde **kimlik belirteçlerini** ayarlayın ve **Kaydet**'i seçin.
1. `web.config` Bölümünün`configuration\appSettings`altındaki kök klasörde bulunan içine aşağıdakini ekleyin:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Yeni `ClientId` kaydettiğiniz uygulama kimliğiyle değiştirin.
1. Projenizin `redirectUri` SSL URL 'siyle değiştirin.

## <a name="test-your-code"></a>Kodunuzu test etme

Visual Studio 'da uygulamanızı test etmek için **F5** 'e basarak projenizi çalıştırın. Tarayıcı, http://<span></span>localhost: {Port} konumu olarak açılır ve **Microsoft hesabıyla oturum açın** düğmesini görürsünüz. Oturum açma işlemini başlatmak için düğmeyi seçin.

Testinizi çalıştırmaya hazırsanız, bir Microsoft Azure Active Directory (Azure AD) hesabı (iş veya okul hesabı) veya kişisel Microsoft hesabı (canlı ' i kullanın<span>.</span> com veya <span>Outlook.</span> com) oturum açın.

![Microsoft hesabıyla oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabı oturum açın](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüle

Oturum açtıktan sonra, Kullanıcı Web sitenizin giriş sayfasına yönlendirilir. Giriş sayfası, Microsoft uygulama kayıt portalındaki uygulama kayıt bilgilerinde belirtilen HTTPS URL 'sidir. Giriş sayfası *"Merhaba \<Kullanıcı >,"* oturumu kapatma bağlantısı ve kullanıcının taleplerini görüntülemek için bir bağlantı içeren bir hoş geldiniz iletisi içerir. Kullanıcı taleplerinin bağlantısı, daha önce oluşturduğunuz *talep* denetleyicisine göz atar.

### <a name="browse-to-see-the-users-claims"></a>Kullanıcının taleplerini görmek için tarayın

Kullanıcının taleplerini görmek için, yalnızca kimliği doğrulanmış kullanıcılar tarafından kullanılabilen denetleyici görünümüne gitmek için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçlarını görüntüleme

Denetleyici görünümüne gözatdıktan sonra, kullanıcının temel özelliklerini içeren bir tablo görmeniz gerekir:

|Özellik |Value |Açıklama |
|---|---|---|
|**Name** |Kullanıcının tam adı | Kullanıcının adı ve soyadı.
|**Kullanıcı Adı** |kullanıcısını<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan Kullanıcı adı.
|**Subject** |Subject |Kullanıcıyı web genelinde benzersiz bir şekilde tanımlayan bir dize.|
|**Kiracı KIMLIĞI** |Guid | Kullanıcının Azure AD organizasyonunu benzersiz bir şekilde temsil eden bir **GUID** .|

Ayrıca, kimlik doğrulama isteğinde olan tüm taleplerin bir tablosunu görmeniz gerekir. Daha fazla bilgi için bkz. [Azure AD kimlik belirtecinde olan taleplerin listesi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Yetkilendir özniteliğine sahip bir yönteme erişimi test etme (isteğe bağlı)

`Authorize` Özniteliği ile korunan bir denetleyicide anonim kullanıcı olarak erişimi test etmek için şu adımları izleyin:

1. Kullanıcının oturumunu kapatmak ve oturum kapatma işlemini gerçekleştirmek için bağlantıyı seçin.
2. Tarayıcınızda, `Authorize` özniteliğiyle korunan denetleyicinize<span></span>erişmek için http://localhost: {Port} veya talepler yazın.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı denetleyiciye erişimi sonrasında beklenen sonuçlar

Korumalı denetleyici görünümünü kullanmak için kimlik doğrulamanız istenir.

## <a name="advanced-options"></a>Gelişmiş seçenekler

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Web sitenizin tamamını koruyun
Web sitenizin tamamını korumak için, **Global. asax** dosyasında, `AuthorizeAttribute` `Application_Start` yöntemi içindeki `GlobalFilters` filtreye özniteliğini ekleyin:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Uygulamanızda oturum açabilen kişileri kısıtla

Varsayılan olarak, bu kılavuz tarafından oluşturulan uygulamayı yapılandırdığınızda, uygulamanız kişisel hesapların (outlook.com, live.com ve diğerleri dahil) oturum açma işlemlerini kabul eder ve bunlarla tümleştirilmiş olan herhangi bir şirketten veya kuruluştan iş ve okul hesapları kabul eder. Azure Active Directory. Bu, SaaS uygulamaları için önerilen bir seçenektir.

Uygulamanıza yönelik kullanıcı oturum açma erişimini kısıtlamak için, birden çok seçenek mevcuttur:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Seçenek 1: Uygulamanızı yalnızca bir kuruluşun Active Directory örneğindeki kullanıcıların oturum açabileceği şekilde kısıtlama (tek kiracılı)

Bu seçenek, *LOB uygulamaları*için yaygın bir senaryodur: Uygulamanızın oturum açma işlemlerini yalnızca belirli bir Azure Active Directory örneğine ait olan hesaplardan (söz konusu örneğin *Konuk hesapları* dahil) kabul etmesini istiyorsanız aşağıdakileri yapın:

1. **Web. config** dosyasında, `Tenant` parametresi `Common` için değerini, kuruluşun kiracı `contoso.onmicrosoft.com`adıyla değiştirin (örneğin,).
2. [Owın başlangıç sınıfınız](#configure-the-authentication-pipeline)içinde, `ValidateIssuer` bağımsız değişkenini olarak `true`ayarlayın.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Seçenek 2: Uygulamanıza erişimi belirli bir kuruluş listesindeki kullanıcılarla kısıtlayın

Oturum açma erişimini yalnızca, izin verilen kuruluşlar listesindeki bir Azure AD kuruluşunda bulunan Kullanıcı hesaplarına kısıtlayabilirsiniz:
1. [Owın başlangıç sınıfınız](#configure-the-authentication-pipeline)içinde, `ValidateIssuer` bağımsız değişkenini olarak `true`ayarlayın.
2. `ValidIssuers` Parametresinin değerini izin verilen kuruluşlar listesine ayarlayın.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Seçenek 3: Verenler doğrulamak için özel bir yöntem kullanma

**Issuervalidator** parametresini kullanarak verenler doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için, [Tokenvalidationparameters sınıfı](/previous-versions/visualstudio/dn464192(v=vs.114))hakkında makalesini okuyun.

## <a name="next-steps"></a>Sonraki adımlar

Web API 'Lerini çağıran Web Apps hakkında bilgi edinin:.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Web API 'Lerini çağıran Web uygulamaları]( https://aka.ms/msal-net-authorization-code)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)