---
title: Kullanıcılarda işaretleyen bir web uygulamasını yapılandırma - Microsoft kimlik platformu | Azure
description: Kullanıcılarda işaretleyen bir web uygulaması oluşturmayı öğrenin (kod yapılandırması)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3de1edc8560cfc85f52293c095fa824b364d2058
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80881647"
---
# <a name="web-app-that-signs-in-users-code-configuration"></a>Kullanıcılarda işaretleyen web uygulaması: Kod yapılandırması

Kullanıcılarda işaretleyen web uygulamanızın kodunu nasıl yapılandırıştırmayı öğrenin.

## <a name="libraries-for-protecting-web-apps"></a>Web uygulamalarını korumak için kitaplıklar

<!-- This section can be in an include for Web App and Web APIs -->
Bir web uygulamasını (ve web API'sini) korumak için kullanılan kitaplıklar şunlardır:

| Platform | Kitaplık | Açıklama |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [.NET için Kimlik Modeli Uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | ASP.NET ve ASP.NET Core tarafından doğrudan kullanılan Microsoft Identity Model Extensions for .NET, hem .NET Framework hem de .NET Core üzerinde çalışan bir dizi DL'yi önerir. bir ASP.NET veya ASP.NET Core web uygulamasından, Belirteç Doğrulama yı **TokenValidationParametreleri** sınıfını (özellikle bazı ortak senaryolarda) kullanarak denetleyebilirsiniz. |
| ![Java](media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | Java web uygulamaları için destek |
| ![Python](media/sample-v2-code/small_logo_python.png) | [MSAL Piton](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | Python web uygulamaları için destek |

İlgilendiğiniz platforma karşılık gelen sekmeyi seçin:

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Bu makalede kod parçacıkları ve aşağıdaki ASP.NET Core [web uygulaması artımlı öğretici, bölüm 1](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg)ayıklanır.

Tam uygulama ayrıntıları için bu öğreticiye başvurmak isteyebilirsiniz.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Bu makalede kod parçacıkları ve aşağıdaki web uygulaması [örnek ASP.NET](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)ayıklanır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

# <a name="java"></a>[Java](#tab/java)

Bu makaledeki kod parçacıkları ve aşağıdakiler, MSAL [Java'daki Microsoft grafik örneğini arayan Java web uygulamasından](https://github.com/Azure-Samples/ms-identity-java-webapp) çıkarılır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Bu makaledeki kod parçacıkları ve aşağıdakiler Python [web uygulamasından](https://github.com/Azure-Samples/ms-identity-python-webapp) MSAL Python'da Microsoft grafik örneği olarak adlandırarak ayıklanır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

---

## <a name="configuration-files"></a>Yapılandırma dosyaları

Microsoft kimlik platformlarını kullanarak kullanıcıları oturum açan Web uygulamaları genellikle yapılandırma dosyaları aracılığıyla yapılandırılır. Doldurmanız gereken ayarlar şunlardır:

- Örneğin, uygulamanızın ulusal bulutlarda çalışmasını istiyorsanız, bulut örneği`Instance`
- Kiracı kimliğindeki hedef`TenantId`kitle ( )
- Azure portalından`ClientId`kopyalanan uygulamanız için istemci kimliği

Bazen, uygulamalar tarafından `Authority`parametrize edilebilir , hangi bir `Instance` `TenantId`concatenation ve .

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core'da bu ayarlar [appsettings.json](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) dosyasında, "AzureAd" bölümünde yer alır.

```Json
{
  "AzureAd": {
    // Azure cloud instance among:
    // - "https://login.microsoftonline.com/" for Azure public cloud
    // - "https://login.microsoftonline.us/" for Azure US government
    // - "https://login.microsoftonline.de/" for Azure AD Germany
    // - "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD audience among:
    // - "TenantId" as a GUID obtained from the Azure portal to sign in users in your organization
    // - "organizations" to sign in users in any work or school account
    // - "common" to sign in users with any work or school account or Microsoft personal account
    // - "consumers" to sign in users with a Microsoft personal account only
    "TenantId": "[Enter the tenantId here]",

    // Client ID (application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

ASP.NET Core'da, başka bir dosya[(properties\launchSettings.json)](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7)URL ()`applicationUrl`ve`sslPort`TLS/SSL bağlantı noktasını ( ) uygulamanız ve çeşitli profiller için içerir.

```Json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

Azure portalında, uygulamanız için **Kimlik Doğrulama** sayfasına kaydolmanız gereken yanıt URL'lerinin bu URL'lerle eşleşmesi gerekir. Önceki iki yapılandırma dosyası `https://localhost:44321/signin-oidc`için, bunlar . Nedeni, ancak `applicationUrl` `http://localhost:3110`(44321) belirtilir `sslPort` olmasıdır. `CallbackPath`olduğu `/signin-oidc`gibi `appsettings.json`.

Aynı şekilde, oturum açma URI'si `https://localhost:44321/signout-callback-oidc`de .

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET olarak, uygulama [Web.config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) dosyası, satır 12-15 üzerinden yapılandırılır.

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

Azure portalında, uygulamanız için **Kimlik Doğrulama** sayfasına kaydolmanız gereken yanıt URL'lerinin bu URL'lerle eşleşmesi gerekir. Yani, onlar `https://localhost:44326/`olmalıdır .

# <a name="java"></a>[Java](#tab/java)

Java'da yapılandırma [application.properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) dosyasının altında `src/main/resources`yer alır.

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

Azure portalında, uygulamanız için **Kimlik Doğrulama** sayfasına kaydolmanız gereken yanıt IU'larının uygulamanın tanımladığı `redirectUri` örneklerle eşleşmesi gerekir. Yani, onlar olmalı `http://localhost:8080/msal4jsample/secure/aad` `http://localhost:8080/msal4jsample/graph/me`ve .

# <a name="python"></a>[Python](#tab/python)

Burada [app_config.py](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py)Python yapılandırma dosyası:

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.microsoftonline.com/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://graph.microsoft.com/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So the token cache will be stored in a server-side session
```

> [!NOTE]
> Bu hızlı başlatma, istemci sırrını basitlik için yapılandırma dosyasında depolamayı önerir. Üretim uygulamanızda, [flask](https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables)belgelerinde açıklandığı gibi anahtar kasa sı veya ortam değişkeni gibi sırrınızı depolamak için başka yollar da kullanmak isteyebilirsiniz.
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>Başlatma kodu

Başlatma kodu platforma bağlı olarak farklıdır. ASP.NET Core ve ASP.NET için, kullanıcılarda oturum açma OpenID Connect ara yazılımına devredilir. ASP.NET veya ASP.NET Core şablonu, Azure Etkin Dizin (Azure AD) v1.0 bitiş noktası için web uygulamaları oluşturur. Bazı yapılandırmamicrosoft kimlik platformu (v2.0) bitiş noktasına uyarlamak için gereklidir. Java durumunda, uygulamanın işbirliği ile Bahar tarafından ele alınır.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core web uygulamalarında (ve web API'lerinde), `[Authorize]` denetleyiciler veya denetleyici eylemleri üzerinde bir özniteliğiniz olduğundan uygulama korunur. Bu öznitelik, kullanıcının kimliğinin doğrulandığını denetler. Uygulamayı açan kod Startup.cs dosyasında.

Microsoft kimlik platformuyla (eski adıyla Azure AD v2.0) kimlik doğrulama eklemek için aşağıdaki kodu eklemeniz gerekir. Koddaki açıklamalar kendi kendini açıklayıcı olmalıdır.

> [!NOTE]
> Projenizi Visual studio'daki varsayılan ASP.NET Core web projesiyle veya kullanarak `dotnet new mvc`başlatTığınızda, yöntem `AddAzureAD` varsayılan olarak kullanılabilir. Bunun nedeni, ilgili paketlerin otomatik olarak yüklenmesidir.
>
> Bir projeyi sıfırdan oluşturuyorsanız ve aşağıdaki kodu kullanmaya çalışıyorsanız, `AddAzureAD` yöntemi kullanılabilir hale getirmek için projenize NuGet paketi **Microsoft.AspNetCore.Authentication.AzureAD.UI** eklemenizi öneririz.

Aşağıdaki kodu [Başlangıç.cs#L33-L34'ten](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34)edinilebilir.

```csharp
public class Startup
{
 ...

  // This method is called by the runtime. Use this method to add services to the container.
  public void ConfigureServices(IServiceCollection services)
  {
    ...
      // Sign in users with the Microsoft identity platform
      services.AddMicrosoftIdentityPlatformAuthentication(Configuration);

      services.AddMvc(options =>
      {
          var policy = new AuthorizationPolicyBuilder()
              .RequireAuthenticatedUser()
              .Build();
            options.Filters.Add(new AuthorizeFilter(policy));
            })
        .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    }
```

Uzantı `AddMicrosoftIdentityPlatformAuthentication` yöntemi [Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23'te](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23)tanımlanır. Bu:

- Kimlik doğrulama hizmetini ekler.
- Yapılandırma dosyasını okumak için seçenekleri yapılandırır.
- OpenID Connect seçeneklerini, kullanılan yetkinin Microsoft kimlik platformu (eski adıyla Azure AD v2.0) bitiş noktası olacak şekilde yapılandırır.
- Belirteci vereni doğrular.
- Ada karşılık gelen taleplerin kimlik belirtecindeki `preferred_username` talepten eşlenmesini sağlar.

Yapılandırmaya ek olarak, ararken `AddMicrosoftIdentityPlatformAuthentication`yapılandırma bölümünün adını belirtebilirsiniz. Varsayılan olarak, `AzureAd`bu.

OpenId Connect ara yazılım etkinliklerinin izlenmesi, kimlik doğrulama işe yaramazsa web uygulamanızı sorun gidermenize yardımcı olabilir. Ayarı, `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` http yanıtından kullanıcının kimliğine ilerlerken bilgilerin ASP.NET Core ara yazılım kümesi tarafından nasıl `HttpContext.User`ayrıntılı hale geldiğini `true` gösterecektir.

```csharp
/// <summary>
/// Add authentication with the Microsoft identity platform.
/// This method expects the configuration file to have a section named "AzureAd" with the necessary settings to initialize authentication options.
/// </summary>
/// <param name="services">Service collection to which to add this authentication scheme</param>
/// <param name="configuration">The Configuration object</param>
/// <param name="subscribeToOpenIdConnectMiddlewareDiagnosticsEvents">
/// Set to true if you want to debug, or just understand the OpenID Connect events.
/// </param>
/// <returns></returns>
public static IServiceCollection AddMicrosoftIdentityPlatformAuthentication(
  this IServiceCollection services,
  IConfiguration configuration,
  string configSectionName = "AzureAd",
  bool subscribeToOpenIdConnectMiddlewareDiagnosticsEvents = false)
{
  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
      .AddAzureAD(options => configuration.Bind(configSectionName, options));
  services.Configure<AzureADOptions>(options => configuration.Bind(configSectionName, options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
      // Per the following code, this application signs in users in any work or school
      // accounts and any Microsoft personal accounts.
      // If you want to direct Azure AD to restrict the users who can sign in, change
      // the tenant value of the appsettings.json file in the following way:
      // - Only work or school accounts => 'organizations'
      // - Only Microsoft personal accounts => 'consumers'
      // - Work or school and personal accounts => 'common'
      // If you want to restrict the users who can sign in to only one tenant,
      // set the tenant value in the appsettings.json file to the tenant ID
      // or domain of this organization.
      options.Authority = options.Authority + "/v2.0/";

      // If you want to restrict the users who can sign in to several organizations,
      // set the tenant value in the appsettings.json file to 'organizations', and add the
      // issuers you want to accept to the options.TokenValidationParameters.ValidIssuers collection.
      options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;

      // Set nameClaimType to be preferred_username.
      // This change is needed because certain token claims from the Azure AD v1 endpoint
      // (on which the original .NET Core template is based) are different from the Microsoft identity platform endpoint.
      // For more details, see [ID tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
      // and [Access tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens).
      options.TokenValidationParameters.NameClaimType = "preferred_username";

      // ...

      if (subscribeToOpenIdConnectMiddlewareDiagnosticsEvents)
      {
          OpenIdConnectMiddlewareDiagnostics.Subscribe(options.Events);
      }
  });
  return services;
}
  ...
```

Sınıf, `AadIssuerValidator` belirteci verenin birçok durumda doğrulanmasını sağlar. Bu sınıf, v1.0 veya v2.0 belirteci, tek kiracılı veya çok kiracılı bir uygulama veya azure genel bulutlarında veya ulusal bulutlarda kişisel Microsoft hesaplarıyla kullanıcılarda imzalayan bir uygulamayla çalışır. [Microsoft.Identity.Web/Resource/AadIssuerValidator.cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs)adresinden edinilebilir.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Bir ASP.NET web uygulamasında ve web API'lerinde kimlik doğrulamayla ilgili kod [App_Start/Startup.Auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61) dosyasında bulunur.

```csharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0.
     // `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/.
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="java"></a>[Java](#tab/java)

Java örneği Bahar çerçevesini kullanır. Uygulama, her BIR HTTP yanıtını engelleyen bir filtre uyguladığınız için korunur. Java web uygulamaları için hızlı başlatılırken, bu filtre `AuthFilter` . `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java`

Filtre, OAuth 2.0 yetkilendirme kodu akışını işler ve kullanıcının`isAuthenticated()` kimlik doğrulaması (yöntem) olup olmadığını denetler. Kullanıcının kimliği doğrulanmıyorsa, Azure AD yetkilendirme bitiş noktalarının URL'sini hesaplar ve tarayıcıyı bu URI'ye yönlendirir.

Yetkilendirme kodunu içeren yanıt geldiğinde, MSAL Java'yı kullanarak belirteci elde eder. Belirteç uç noktasından (yeniden yönlendirme URI'de) sonunda belirteci aldığında, kullanıcı oturum açmış olur.

Ayrıntılar için `doFilter()` [AuthFilter.java'daki](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java)yönteme bakın.

> [!NOTE]
> `doFilter()` Kodu biraz farklı bir sırada yazılır, ancak akış açıklanan biridir.

Bu yöntemin tetiklediği yetkilendirme kodu akışı yla ilgili ayrıntılar için [Microsoft kimlik platformu ve OAuth 2.0 yetkilendirme kodu akışına](v2-oauth2-auth-code-flow.md)bakın.

# <a name="python"></a>[Python](#tab/python)

Python örneği Flask kullanır. Flask ve MSAL Python'un başlatılması [app.py#L1-L28'de](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28)yapılır.

```Python
import uuid
import requests
from flask import Flask, render_template, session, request, redirect, url_for
from flask_session import Session  # https://pythonhosted.org/Flask-Session
import msal
import app_config


app = Flask(__name__)
app.config.from_object(app_config)
Session(app)
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki makalede, oturum açma ve imzalamayı nasıl tetiklediğinizi öğreneceksiniz.

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---
