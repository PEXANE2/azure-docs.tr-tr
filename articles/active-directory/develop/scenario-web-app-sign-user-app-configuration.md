---
title: Kullanıcılar tarafından oturum açan bir Web uygulamasını yapılandırma-Microsoft Identity platform | Mavisi
description: Kullanıcılara oturum açan bir Web uygulaması oluşturma hakkında bilgi edinin (kod yapılandırması)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d54103cad8a3550bdc300cba2308397dd1ce3d6c
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79262444"
---
# <a name="web-app-that-signs-in-users-code-configuration"></a>Kullanıcılara oturum açan Web uygulaması: kod yapılandırması

Kullanıcılara oturum açan Web uygulamanız için kodu yapılandırmayı öğrenin.

## <a name="libraries-for-protecting-web-apps"></a>Web uygulamalarını korumaya yönelik kitaplıklar

<!-- This section can be in an include for Web App and Web APIs -->
Bir Web uygulamasını (ve bir Web API 'SI) korumak için kullanılan kitaplıklar şunlardır:

| Platform | Kitaplık | Açıklama |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [.NET için kimlik modeli uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Doğrudan ASP.NET ve ASP.NET Core tarafından kullanılan .NET için Microsoft Identity model uzantıları, hem .NET Framework hem de .NET Core üzerinde çalışan bir dll kümesi önerir. Bir ASP.NET veya ASP.NET Core Web uygulamasından belirteç doğrulamayı, **Tokenvalidationparameters** sınıfını (özellikle de bazı iş ortakları senaryolarında) kullanarak kontrol edebilirsiniz. |
| ![Java](media/sample-v2-code/small_logo_java.png) | [MSAL Java](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | Java Web uygulamaları için destek |
| ![Python](media/sample-v2-code/small_logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | Python web uygulamaları için destek |

İlgilendiğiniz platforma karşılık gelen sekmeyi seçin:

# <a name="aspnet-core"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

Bu makaledeki kod parçacıkları ve aşağıdakiler [ASP.NET Core Web uygulaması artımlı öğreticisi, Bölüm 1 '](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg)den ayıklanır.

Tam uygulama ayrıntıları için Bu öğreticiye başvurmak isteyebilirsiniz.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Bu makaledeki kod parçacıkları ve aşağıdakiler [ASP.NET Web uygulaması örneğinden](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)ayıklanır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

# <a name="java"></a>[Java](#tab/java)

Bu makaledeki kod parçacıkları ve aşağıdakiler, MSAL Java 'daki [Microsoft Graph örneğini çağıran Java Web uygulamasından](https://github.com/Azure-Samples/ms-identity-java-webapp) ayıklanır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

# <a name="python"></a>[Python](#tab/python)

Bu makaledeki kod parçacıkları ve aşağıdakiler, MSAL Python 'da [Microsoft Graph örneğini çağıran Python web uygulamasından](https://github.com/Azure-Samples/ms-identity-python-webapp) ayıklanır.

Tam uygulama ayrıntıları için bu örneğe başvurmak isteyebilirsiniz.

---

## <a name="configuration-files"></a>Yapılandırma dosyaları

Microsoft Identity platformunu kullanarak kullanıcıların oturum açmasını sağlayan Web uygulamaları genellikle yapılandırma dosyaları aracılığıyla yapılandırılır. Doldurmanız gereken ayarlar şunlardır:

- Uygulamanızın Ulusal bulutlarda çalışmasını istiyorsanız (örneğin,) bulut örneği (`Instance`)
- Kiracı KIMLIĞINDEKI hedef kitle (`TenantId`)
- Uygulamanız için Azure portal kopyalandığı şekilde istemci KIMLIĞI (`ClientId`)

Bazen, uygulamalar, `Instance` ve `TenantId`birleşimi olan `Authority`tarafından parametrized olabilir.

# <a name="aspnet-core"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

ASP.NET Core, bu ayarlar "AzureAd" bölümünde [appSettings. JSON](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/appsettings.json#L2-L8) dosyasında bulunur.

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

ASP.NET Core, başka bir dosya ([properties\launchSettings.JSON](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/bc564d68179c36546770bf4d6264ce72009bc65a/1-WebApp-OIDC/1-1-MyOrg/Properties/launchSettings.json#L6-L7)), uygulamanız ve çeşitli profilleriniz için URL (`applicationUrl`) ve SSL bağlantı noktasını (`sslPort`) içerir.

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

Azure portal, uygulamanız için **kimlik doğrulama** sayfasında kaydolmanız gereken yanıt URI 'Lerinin bu URL 'lerle eşleşmesi gerekir. Önceki iki yapılandırma dosyası için `https://localhost:44321/signin-oidc`olacaktır. Bunun nedeni `applicationUrl` `http://localhost:3110`, ancak `sslPort` belirtilir (44321). `CallbackPath`, `appsettings.json`tanımlanan `/signin-oidc`.

Aynı şekilde, oturum açma URI 'SI `https://localhost:44321/signout-callback-oidc`olarak ayarlanır.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET ' de, uygulama [Web. config](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/Web.config#L12-L15) dosyası aracılığıyla yapılandırılır, satır 12 ile 15 arası.

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

Azure portal, uygulamanız için **kimlik doğrulama** sayfasında kaydolmanız gereken yanıt URI 'Lerinin bu URL 'lerle eşleşmesi gerekir. Diğer bir deyişle, `https://localhost:44326/`olmaları gerekir.

# <a name="java"></a>[Java](#tab/java)

Java 'da, yapılandırma `src/main/resources`altında bulunan [Application. Properties](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/d55ee4ac0ce2c43378f2c99fd6e6856d41bdf144/src/main/resources/application.properties) dosyasında bulunur.

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraph=http://localhost:8080/msal4jsample/graph/me
```

Azure portal, uygulamanız için **kimlik doğrulama** sayfasında kaydolmanız gereken yanıt URI 'lerinin, uygulamanın tanımladığı `redirectUri` örnekleriyle eşleşmesi gerekir. Diğer bir deyişle, `http://localhost:8080/msal4jsample/secure/aad` ve `http://localhost:8080/msal4jsample/graph/me`olmaları gerekir.

# <a name="python"></a>[Python](#tab/python)

İşte [app_config. Kopyala](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.1.0/app_config.py)içindeki Python yapılandırma dosyası:

```Python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
AUTHORITY = "https://login.microsoftonline.com/common""
CLIENT_ID = "Enter_the_Application_Id_here"
ENDPOINT = 'https://graph.microsoft.com/v1.0/users'
SCOPE = ["User.ReadBasic.All"]
SESSION_TYPE = "filesystem"  # So the token cache will be stored in a server-side session
```

> [!NOTE]
> Bu hızlı başlangıç, istemci gizliliğini basitlik için yapılandırma dosyasında depolamayı önerir. Üretim uygulamanızda, [Flask belgelerinde](https://flask.palletsprojects.com/en/1.1.x/config/#configuring-from-environment-variables)açıklandığı gibi bir Anahtar Kasası veya ortam değişkeni gibi gizli dizinizi depolamak için başka yollar kullanmak isteyeceksiniz.
>
> ```python
> CLIENT_SECRET = os.getenv("CLIENT_SECRET")
> if not CLIENT_SECRET:
>     raise ValueError("Need to define CLIENT_SECRET environment variable")
> ```

---

## <a name="initialization-code"></a>Başlatma kodu

Başlatma kodu platforma bağlı olarak farklılık açmış. ASP.NET Core ve ASP.NET için, kullanıcıların oturum açması, OpenID Connect ara yazılımı için temsilci olarak oluşturulur. ASP.NET veya ASP.NET Core şablonu, Azure Active Directory (Azure AD) v 1.0 uç noktası için Web uygulamaları oluşturur. Bunları Microsoft Identity platform (v 2.0) uç noktasına uyarlamak için bazı yapılandırmalar gerekir. Java söz konusu olduğunda, uygulamanın iş birliği ile Spring tarafından işlenir.

# <a name="aspnet-core"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

ASP.NET Core Web Apps (ve Web API 'Leri) içinde, denetleyicilerde veya denetleyici eylemlerinde bir `[Authorize]` özniteliğine sahip olduğunuz için uygulama korunur. Bu öznitelik, kullanıcının kimliğinin doğrulandığını denetler. Uygulamayı başlatmakta olan kod Startup.cs dosyasında bulunur.

Microsoft Identity platform (eski adıyla Azure AD v 2.0) ile kimlik doğrulaması eklemek için aşağıdaki kodu eklemeniz gerekir. Koddaki yorumların kendine açıklayıcı olması gerekir.

> [!NOTE]
> Projenizi Visual Studio içinde varsayılan ASP.NET Core Web projesi veya `dotnet new mvc`kullanarak başlatırsanız, yöntem `AddAzureAD` varsayılan olarak kullanılabilir. Bunun nedeni, ilgili paketlerin otomatik olarak yüklenmalardır.
>
> Sıfırdan bir proje oluşturup aşağıdaki kodu kullanmaya çalışıyorsanız, `AddAzureAD` yöntemini kullanılabilir hale getirmek için **Microsoft. AspNetCore. Authentication. AzureAD. UI** NuGet paketini projenize eklemenizi öneririz.

Aşağıdaki kod [Başlangıçta kullanılabilir. cs # L33-L34](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34).

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

`AddMicrosoftIdentityPlatformAuthentication` uzantısı metodu [Microsoft. Identity. Web/WebAppServiceCollectionExtensions. cs # L23](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23)içinde tanımlanmıştır. İçerdiği

- Kimlik doğrulama hizmetini ekler.
- Yapılandırma dosyasını okumak için seçenekleri yapılandırır.
- OpenID Connect seçeneklerini, kullanılan yetkilinin Microsoft Identity platform (eski adıyla Azure AD v 2.0) uç noktası olacak şekilde yapılandırır.
- Belirtecin vereni doğrular.
- Ada karşılık gelen taleplerin KIMLIK belirtecindeki `preferred_username` talebine eşlendiğinden emin olmanızı sağlar.

Yapılandırmaya ek olarak, `AddMicrosoftIdentityPlatformAuthentication`çağırırken yapılandırma bölümünün adını belirtebilirsiniz. Varsayılan olarak, `AzureAd`.

OpenID Connect ara yazılım olaylarını izleme, kimlik doğrulaması çalışmazsa Web uygulamanızın sorunlarını gidermenize yardımcı olabilir. `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` `true` olarak ayarlamak, HTTP yanıtından `HttpContext.User`kullanıcı kimliğine kadar ilerlerken ASP.NET Core ara yazılımı tarafından bilgilerin nasıl ayrıntılı olduğunu gösterir.

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

`AadIssuerValidator` sınıfı, belirtecin verenin birçok durumda doğrulanmasını sağlar. Bu sınıf, bir v 1.0 veya v 2.0 belirteci, tek kiracılı veya çok kiracılı bir uygulama ya da kullanıcılara Azure genel bulutunda veya ulusal bulutlarda kişisel Microsoft hesaplarıyla oturum açan bir uygulamayla birlikte kullanılır. [Microsoft. Identity. Web/Resource/Aadıssuervalidator. cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs)tarafından kullanılabilir.

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Bir ASP.NET Web uygulamasında ve Web API 'Lerinde kimlik doğrulamasıyla ilgili kod [App_Start/Startup.auth.cs](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect/blob/a2da310539aa613b77da1f9e1c17585311ab22b7/WebApp/App_Start/Startup.Auth.cs#L17-L61) dosyasında bulunur.

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

Java örneği yay çerçevesini kullanır. Her HTTP yanıtını karşılar bir filtre uyguladığınız için uygulama korunur. Java Web uygulamalarına yönelik hızlı başlangıçta, bu filtre `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java``AuthFilter`.

Filtre, OAuth 2,0 yetkilendirme kodu akışını işler ve kullanıcının kimliğinin doğrulandığını denetler (`isAuthenticated()` yöntemi). Kullanıcının kimliği doğrulanmadıysa, Azure AD yetkilendirme uç noktalarının URL 'sini hesaplar ve tarayıcıyı bu URI 'ye yönlendirir.

Yanıt geldiğinde, yetkilendirme kodunu içeren, MSAL Java kullanarak belirteci alır. Son olarak belirteç uç noktasından (yeniden yönlendirme URI 'sindeki) belirteci aldığında Kullanıcı oturum açmış olur.

Ayrıntılar için bkz. [AuthFilter. Java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java)içindeki `doFilter()` yöntemi.

> [!NOTE]
> `doFilter()` kodu biraz farklı bir düzende yazılır, ancak akış tanımlanan bir sıradır.

Bu yöntemin tetiklediği yetkilendirme kodu akışı hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0 yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md).

# <a name="python"></a>[Python](#tab/python)

Python örneği Flask kullanır. Flask ve MSAL Python başlatma işlemi [app. Sip # L1-L28](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e03be352914bfbd58be0d4170eba1fb7a4951d84/app.py#L1-L28).

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

Sonraki makalede, oturum açma ve oturum kapatma tetiklemeyi öğreneceksiniz.

# <a name="aspnet-core"></a>[ASP.NET Çekirdeği](#tab/aspnetcore)

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnetcore)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=aspnet)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=java)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-app-sign-user-sign-in?tabs=python)

---
