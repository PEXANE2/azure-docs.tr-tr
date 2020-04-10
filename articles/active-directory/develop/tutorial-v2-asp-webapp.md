---
title: Microsoft kimlik platformuna web uygulaması ASP.NET oturum açma ekleme
titleSuffix: Microsoft identity platform
description: Geleneksel web tarayıcısı tabanlı bir uygulama ve OpenID Connect standardı kullanarak Microsoft oturum açma nın ASP.NET bir çözümde uygulanması
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 08/28/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 94d3993c6a0c62a68ea77a888d3351c8fea1d935
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80990999"
---
# <a name="add-sign-in-to-microsoft-to-an-aspnet-web-app"></a>ASP.NET bir web uygulamasına Microsoft'a oturum açma ekleme

Bu kılavuz, geleneksel web tarayıcısı tabanlı bir uygulama ve OpenID Connect kullanarak ASP.NET bir MVC çözümü aracılığıyla Microsoft'a oturum açmanın nasıl uygulanacağını göstermektedir.

Bu kılavuzu tamamladığınızda, başvurunuz outlook.com ve live.com gibi kişisel hesapların oturum açma larını kabul edebilecektir. Ayrıca, Microsoft kimlik platformuyla bütünleşmiş herhangi bir şirket veya kuruluşun iş ve okul hesapları uygulamanızda oturum açabilecektir.

> Bu kılavuz, Microsoft Visual Studio 2019'u gerektirir.  Sizde yok mu?  [Visual Studio 2019'u ücretsiz olarak indirin.](https://www.visualstudio.com/downloads/)

>[!NOTE]
> Microsoft kimlik platformunda yeniyseniz, [ASP.NET bir web uygulamasına Microsoft kimlik platformu ekle platformuyla](quickstart-v2-aspnet-webapp.md)başlamanızı öneririz.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulama nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

Oluşturduğunuz örnek uygulama, kullanıcının oturum açma düğmesi yle kimlik doğrulamasını gerektiren ASP.NET bir web sitesine erişmek için tarayıcıyı kullandığınız bir senaryoya dayanır. Bu senaryoda web sayfasını oluşturma işlemlerinin çoğu sunucu tarafında gerçekleştirilmektedir.

## <a name="libraries"></a>Kitaplıklar

Bu kılavuzda aşağıdaki kitaplıklar kullanılmaktadır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Uygulamanın kimlik doğrulaması için OpenIdConnect kullanmasını sağlayan ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Çerezleri kullanarak bir uygulamanın kullanıcı oturumunu sürdürmesini sağlayan ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|ASP.NET istek ardışık hattını kullanarak OWIN tabanlı uygulamaların Internet Bilgi Hizmetleri 'nde (IIS) çalışmasını sağlayan ara yazılım|

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölümde, OpenID Connect kullanarak ASP.NET bir projede OWIN ara yazılımı aracılığıyla kimlik doğrulama ardışık bölümünün nasıl yüklenir ve yapılandırılacağı açıklanmaktadır.

> Bunun yerine bu örneğin Visual Studio projesini indirmeyi mi tercih edersiniz? [Bir proje indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) ve yürütmeden önce kod örneğini yapılandırmak için [uygulamanızı Kaydedin'e](#register-your-application) atlayın.

### <a name="create-your-aspnet-project"></a>ASP.NET projenizi oluşturun

1. Visual Studio:**Yeni** > **Proje** **Dosya** > git.
2. **Visual C#\Web** bölümünde **ASP.NET Web Uygulaması (.NET Framework)** girişini seçin.
3. Uygulamanıza bir ad verin ve **Tamam**'ı seçin.
4. **Boş'u**seçin ve ardından **MVC** başvuruları eklemek için onay kutusunu seçin.

## <a name="add-authentication-components"></a>Kimlik doğrulama bileşenleri ekleme

1. Visual Studio: **Araçlar** > **Nuget Paket Yöneticisi** > **Paket Yöneticisi Konsol**gidin.
2. Paket Yöneticisi Konsolu penceresinde aşağıdakileri yazarak *OWIN ara yazılım NuGet paketlerini* ekleyin:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Bu kütüphaneler hakkında
> Bu kitaplıklar, çerez tabanlı kimlik doğrulama yoluyla OpenID Connect'i kullanarak tek oturum açma (SSO) sağlar. Kimlik doğrulaması tamamlandıktan ve kullanıcıyı temsil eden belirteç uygulamanıza gönderildikten sonra OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Tarayıcı daha sonra bu çerezi sonraki isteklerde kullanır, böylece kullanıcı parolayı yeniden yazmak zorunda kalmaz ve ek doğrulama gerekmez.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulama ardışık hattını yapılandırma

OpenID Connect kimlik doğrulamasını yapılandırmak için bir OWIN ara yazılım başlatma sınıfı oluşturmak için aşağıdaki adımlar kullanılır. IIS işleminiz başladığında bu sınıf otomatik olarak yürütülür.

> [!TIP]
> Projenizin kök klasöründe `Startup.cs` adlı bir dosya yoksa:
> 1. Projenin kök klasörünü sağ tıklatın ve sonra**Yeni Öğe** > **OWIN Başlangıç sınıfı** **ekle'yi** > seçin.<br/>
> 2. Adını **Startup.cs.**
>
>> Seçilen sınıfın standart bir C# sınıfı değil, OWIN Başlangıç sınıfı olduğundan emin olun. [derleme: OwinStartup(typeof({NameSpace}) gördüğünüzü doğrulayarak bunu doğrulayın. Başlangıç))] ad alanının üstünde.

1. *Startup.cs OWIN* ve *Microsoft.IdentityModel* referansları ekleyin:

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
        // The Client ID is used by the application to uniquely identify itself to Microsoft identity platform.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Microsoft identity platform endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
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
> Ayar, `ValidateIssuer = false` bu hızlı başlatma için basitleştirmedir. Gerçek uygulamalarda, vereni doğrulamanız gerekir.
> Bunu nasıl yapacağınızı öğrenmek için örneklere bakın.

<!--start-collapse-->
> ### <a name="more-information"></a>Daha fazla bilgi
> *OpenIDConnectAuthenticationOptions'da* sağladığınız parametreler, uygulamanın Microsoft kimlik platformuyla iletişim kurması için koordinat görevi sunar. OpenID Connect aracı arka planda tanımlama bilgileri kullandığından, önceki kodun gösterdiği gibi çerez kimlik doğrulaması da ayarlamanız gerekir. *ValidateIssuer* değeri OpenIdConnect'e belirli bir kuruluşa erişimi kısıtlamamasını söyler.
<!--end-collapse-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum açma isteklerini işlemek için denetleyici ekleme

Oturum açma ve imzalama yöntemlerini ortaya çıkarmak için yeni bir denetleyici oluşturmak için aşağıdaki adımları izleyin:

1.  **Denetleyiciler** klasörüne sağ tıklayın ve**Denetleyici** **Ekle'yi** > seçin.
2.  **MVC (.NET sürümü) Denetleyici – Boş** girişini seçin.
3.  **Ekle'yi**seçin.
4.  **HomeController** adını ve sonra **Ekle'yi**seçin.
5.  Sınıfa OWIN başvuruları ekleyin:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```

6. Kimlik doğrulama sorunu başlatarak denetleyicinize oturum açma ve oturum açma işlemlerini işlemek için aşağıdaki iki yöntemi ekleyin:

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

## <a name="create-the-apps-home-page-for-user-sign-in"></a>Kullanıcı oturum açma için uygulamanın ana sayfasını oluşturma

Visual Studio'da oturum açma düğmesini eklemek ve kimlik doğrulamadan sonra kullanıcı bilgilerini görüntülemek için yeni bir görünüm oluşturun:

1.  **Görünümler\Giriş Sayfası** klasörüne sağ tıklayın ve **Görünüm Ekle**'yi seçin.
2.  Yeni görünüm **Dizini'ni**adlandırın.
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
> ### <a name="more-information"></a>Daha fazla bilgi
>  Bu sayfa siyah arka plan üzerinde SVG biçiminde bir oturum açma düğmesi ekler:<br/>![Microsoft ile oturum aç](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Daha fazla oturum açma düğmesi [için, Markalama yönergelerine](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Marka yönergeleri")gidin.
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Kullanıcının taleplerini görüntülemek için denetleyici ekleme
Bu denetleyici bir denetleyiciyi koruma amacıyla `[Authorize]` özniteliğini kullanma şeklini gösterir. Bu öznitelik, yalnızca kimlik doğrulaması yapılan kullanıcılara izin vererek denetleyiciye erişimi kısıtlar. Aşağıdaki kod, oturum açmanın bir parçası olarak alınan kullanıcı taleplerini görüntülemek için öznitelikkullanır:

1.  **Denetleyiciler** klasörüne sağ tıklayın ve ardından**Denetleyici** **Ekle'yi** > seçin.
2.  **MVC {sürüm} Denetleyici – Boş** girişini seçin.
3.  **Ekle'yi**seçin.
4.  **ClaimsController** olarak adlandırın.
5.  Denetleyici sınıfınızın kodunu aşağıdaki kodla değiştirin. Bu sınıfa `[Authorize]` öznitelik ekler:

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
> ### <a name="more-information"></a>Daha fazla bilgi
> Öznitelik kullanımı `[Authorize]` nedeniyle, bu denetleyicinin tüm yöntemleri yalnızca kullanıcı nın kimliği doğrulanırsa yürütülebilir. Kullanıcı nın kimliği doğrulanmazsa ve denetleyiciye erişmeye çalışırsa, OWIN bir kimlik doğrulama sorunu başlatır ve kullanıcıyı kimlik doğrulamaya zorlar. Önceki kod, kullanıcının Id belirtecinde yer alan belirli kullanıcı öznitelikleriiçin talep listesine bakar. Bu öznitelik kullanıcının tam adını, kullanıcı adını ve genel kullanıcı tanımlayıcısı nesnesini içerir. Ayrıca kullanıcının kuruluşunun kimliğini temsil eden *Kiracı Kimliği* değerini de içerir. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcının taleplerini görüntülemek için bir görünüm oluşturma

Visual Studio'da kullanıcının taleplerini bir web sayfasında görüntülemek için yeni bir görünüm oluşturun:

1.  **Görünümler\Talepler** klasörüne sağ tıklayın ve ardından **Görünüm Ekle'yi**seçin.
2.  Yeni görünüm **Dizini'ni**adlandırın.
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

## <a name="register-your-application"></a>Uygulamanızı kaydetme

Başvurunuzu kaydetmek ve başvuru kayıt bilgilerinizi çözüme eklemek için iki seçeneğiniz var:

### <a name="option-1-express-mode"></a>Seçenek 1: Ekspres modu

Uygulamanızı hızlı bir şekilde kaydetmek için aşağıdaki adımları izleyin:

1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesine gidin.
1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
1. Yeni uygulamanızı tek bir tıklamayla indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod

Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:

1. Visual Studio'u açın ve sonra:
   1. Solution Explorer'da projeyi seçin ve Özellikler penceresini görüntüleyin (Özellikler penceresini görmüyorsanız F4'e basın).
   1. SSL'yi `True`Değiştir.
   1. Visual Studio'da projeyi sağ tıklatın, **Özellikler'i**seçin ve ardından **Web** sekmesini seçin. **Sunucular** bölümünde, **Proje Url** ayarını **SSL URL'si**olarak değiştirin.
   1. SSL URL'sini kopyalayın. Bu URL'yi, kayıt portalının bir sonraki adımda Yönlendirme URL'leri listesindeki Yönlendirme URL'leri listesine eklersiniz.<br/><br/>![Proje özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak oturum açın.
1. Hesabınız size birden fazla kiracıya erişim sağlıyorsa, sağ üst köşedeki hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
1. Geliştiriciler için Microsoft kimlik [platformuna](https://go.microsoft.com/fwlink/?linkid=2083908) gidin Uygulama kayıtları sayfası.
1. **Yeni kayıt**seçin.
1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
   1. **Ad** bölümüne, **ASPNET-Tutorial**gibi uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
   1. Visual Studio'dan kopyaladığınız SSL URL'sini Yanıtla URL'ye (örneğin,) `https://localhost:44368/` **YanıtLA URL'ye**ekleyin ve **Kaydol'u**seçin.
1. Kimlik **Doğrulama** menüsünü seçin, Örtülü **Hibe**altında **kimlik belirteçlerini** seçin ve ardından **Kaydet'i**seçin.
1. `configuration\appSettings` Bölümündeki kök klasöründe bulunan web.config dosyasına aşağıdakileri ekleyin:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Yeni `ClientId` kaydolduğunuz Uygulama Kimliği ile değiştirin.
1. Projenizin SSL URL'si ile değiştirin. `redirectUri`

## <a name="test-your-code"></a>Kodunuza test etme

Visual Studio'da uygulamanızı test etmek için projenizi çalıştırmak için F5 tuşuna basın. Tarayıcı http://<span></span>localhost:{port} konumuna açılır ve Microsoft ile **Oturum Aç'ı** görürsünüz. Oturum açma işlemini başlatmak için düğmeyi seçin.

Testinizi çalıştırmaya hazır olduğunuzda, bir Azure REKLAM hesabı (iş veya okul hesabı) veya kişisel bir Microsoft hesabı<span>(canlı) kullanın.</span> com veya <span>outlook.</span> com) oturum açmanız.

![Microsoft ile oturum aç](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Microsoft hesabınızda oturum açma](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

<!--start-collapse-->
> ###  <a name="permissions-and-consent-in-the-microsoft-identity-platform-endpoint"></a>Microsoft kimlik platformu bitiş noktasında izinler ve onay
>  Microsoft kimlik platformuyla tümlebilen uygulamalar, kullanıcılara ve yöneticilere verilere nasıl erişilebileceği üzerinde denetim sağlayan bir yetkilendirme modeli izler. Bir kullanıcıbu uygulamaya erişmek için Microsoft kimlik platformunda kimlik doğrulaması yaptıktan sonra, uygulama tarafından istenen izinleri kabul etmeleri istenir ("Temel profilinizi görüntüleyin" ve "Eriştinizin verilerine erişimi koruyun"). Bu izinleri kabul ettikten sonra, kullanıcı uygulama sonuçlarına devam eder. Ancak, aşağıdakilerden biri oluşursa, kullanıcıya **Need admin onay** sayfası istenebilir:
>  > - Uygulama geliştiricisi, **Yönetici onayı**gerektiren ek izinler ekler.
>  > - Veya kiracı, kullanıcıların kendi adlarına şirket verilerine erişen uygulamalara izin veremeyeceği şekilde **(Kurumsal Uygulamalar -> Kullanıcı**Ayarları'nda) yapılandırılır.
>
> Daha fazla bilgi [için, Microsoft kimlik platformu bitiş noktasında İzinler ve onay](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)bakın.
<!--end-collapse-->

#### <a name="view-application-results"></a>Uygulama sonuçlarını görüntüleme

Oturum açmanızdan sonra, kullanıcı web sitenizin ana sayfasına yönlendirilir. Ana sayfa, Microsoft Uygulama Kayıt Portalı'ndaki uygulama kayıt bilgilerinizde belirtilen HTTPS URL'sidir. Ana sayfada *bir "Merhaba \<kullanıcı>"* karşılama iletisi, oturum dışı kalmak için bir bağlantı ve kullanıcının iddialarını görüntülemek için bir bağlantı bulunmaktadır. Kullanıcının taleplerinin bağlantısı, daha önce oluşturduğunuz Talepler denetleyicisine bağlanır.

### <a name="view-the-users-claims"></a>Kullanıcının taleplerini görüntüleme

Kullanıcının taleplerini görüntülemek için, yalnızca kimlik doğrulaması yapılan kullanıcılar tarafından kullanılabilen denetleyici görünümüne göz atmak için bağlantıyı seçin.

#### <a name="view-the-claims-results"></a>Talep sonuçlarını görüntüleme

Denetleyici görünümüne göz attıktan sonra, kullanıcı için temel özellikleri içeren bir tablo görmeniz gerekir:

|Özellik |Değer |Açıklama |
|---|---|---|
|**Adı** |Kullanıcının tam adı | Kullanıcının adı ve soyadı
|**Username** |Kullanıcı<span>@domain.com</span> | Kullanıcıyı tanımlamak için kullanılan kullanıcı adı|
|**Konu** |Özne |Web'de kullanıcıyı benzersiz olarak tanımlayan bir dize|
|**Kiracı Kimliği** |Guid | Kullanıcının Azure REKLAM kuruluşunu benzersiz olarak temsil eden bir **kılavuz**|

Ayrıca, kimlik doğrulama isteğinde bulunan tüm taleplerin bir tablosunu görmeniz gerekir. Daha fazla bilgi için, [kimlik belirtecinde bulunan taleplerin listesine](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)bakın.

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Yetkilendirme özniteliği olan bir yönteme erişimi sına (isteğe bağlı)

Öznitelik tarafından `Authorize` korunan bir denetleyiciye anonim bir kullanıcı olarak erişimi sınamak için aşağıdaki adımları izleyin:

1. Kullanıcıyı oturum dansı yapmak ve oturum açma işlemini tamamlamak için bağlantıyı seçin.
2. <span></span>Tarayıcınızda, http:// localhost yazın:{port}/öznitelik tarafından `Authorize` korunan denetleyicinize erişmek için talepte bulunur.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Korumalı bir denetleyiciye erişinden sonra beklenen sonuçlar

Korumalı denetleyici görünümünü kullanmak için kimlik doğrulamanız istenir.

## <a name="advanced-options"></a>Gelişmiş seçenekler

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Tüm web sitenizi koruyun
**Global.asax** dosyasındaki tüm web sitenizi korumak `AuthorizeAttribute` `Application_Start` için, `GlobalFilters` yöntemdeki filtreye özniteliği ekleyin:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Uygulamanızda kimlerin oturum açabileceğini kısıtlama

Varsayılan olarak, bu kılavuz tarafından oluşturulan uygulamayı oluşturduğunuzda, uygulamanız kişisel hesapların (outlook.com, live.com ve diğerleri dahil) oturum açmalarının yanı sıra Microsoft kimlik platformuyla entegre edilmiş herhangi bir şirket veya kuruluşun iş ve okul hesaplarını kabul eder. Bu, SaaS uygulamaları için önerilen bir seçenektir.

Uygulamanız için kullanıcı oturum açma erişimini kısıtlamak için birden çok seçenek kullanılabilir.

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Seçenek 1: Kullanıcıları yalnızca bir kuruluşun Etkin Dizin örneğinden uygulamanızda oturum açmanızı kısıtlama (tek kiracılı)

Bu seçenek *lob uygulamaları*için sık sık kullanılır: Uygulamanızın yalnızca belirli bir Azure REKLAM örneğine ait hesaplardan (örneğin *konuk hesapları* dahil) oturum açma kabul etmesini istiyorsanız, aşağıdaki adımları izleyin:

1. web.config dosyasında, `Tenant` parametrenin `Common` değerini kuruluşun kiracı adına değiştirin. `contoso.onmicrosoft.com`
2. [OWIN Başlangıç sınıfınızda,](#configure-the-authentication-pipeline) `ValidateIssuer` bağımsız `true`değişkeni .

#### <a name="option-2-restrict-access-to-users-in-a-specific-list-of-organizations"></a>Seçenek 2: Belirli bir kuruluş listesindeki kullanıcılara erişimi kısıtlama

Oturum açma erişimini yalnızca izin verilen kuruluşlar listesinde yer alan bir Azure AD kuruluşundaki kullanıcı hesaplarıyla kısıtlayabilirsiniz:
1. [OWIN Başlangıç sınıfınızda,](#configure-the-authentication-pipeline) `ValidateIssuer` bağımsız `true`değişkeni .
2. Parametrenin `ValidIssuers` değerini izin verilen kuruluşlar listesine ayarlayın.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Seçenek 3: İhraççıları doğrulamak için özel bir yöntem kullanın

**İhraççı Validator** parametresini kullanarak verenleri doğrulamak için özel bir yöntem uygulayabilirsiniz. Bu parametrenin nasıl kullanılacağı hakkında daha fazla bilgi için [Bkz.](/previous-versions/visualstudio/dn464192(v=vs.114))

## <a name="next-steps"></a>Sonraki adımlar

Web uygulamalarının web API'lerini nasıl arayabildiği hakkında bilgi edinin.

### <a name="learn-how-to-create-the-application-used-in-this-quickstart-guide"></a>Bu hızlı başlangıç kılavuzunda kullanılan uygulamayı nasıl oluşturabilirsiniz öğrenin

Microsoft kimlik platformuyla web API'lerini arayan Web uygulamaları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Web API'lerini çağıran Web uygulamaları](scenario-web-app-sign-user-overview.md)

Microsoft Graph'ı arayarak Web uygulamaları oluşturmayı öğrenin:

> [!div class="nextstepaction"]
> [Microsoft Graph ASP.NET öğretici](https://docs.microsoft.com/graph/tutorials/aspnet)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
