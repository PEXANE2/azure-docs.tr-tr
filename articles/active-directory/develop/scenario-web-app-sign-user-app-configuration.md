---
title: Kullanıcılarda oturum açan Web uygulaması (kod yapılandırması)-Microsoft Identity platform
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
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a746b0f6d85e3f012cdd2e78fff8cd10a586950
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086746"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Kullanıcıları oturum açan Web uygulaması-kod yapılandırması

Kullanıcıları oturum açan Web uygulamanız için kodu yapılandırmayı öğrenin.

## <a name="libraries-used-to-protect-web-apps"></a>Web Apps korumak için kullanılan kitaplıklar

<!-- This section can be in an include for Web App and Web APIs -->
Bir Web uygulamasını (ve bir Web API 'sini) korumak için kullanılan kitaplıklar şunlardır:

| Platform | Kitaplık | Açıklama |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [.NET için kimlik modeli uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Doğrudan ASP.NET ve ASP.NET Core tarafından kullanılan .NET için Microsoft Identity Extensions, hem .NET Framework hem de .NET Core üzerinde çalışan bir dll kümesini önerir. Bir ASP.NET/ASP.NET Core Web uygulamasından **Tokenvalidationparameters** sınıfını kullanarak belirteç doğrulamayı denetleyebilirsiniz (bazı ISV senaryolarında özellikle) |
| ![Java](media/sample-v2-code/logo_java.png) | [msal4j](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | Java için MSAL-Şu anda genel önizlemede |
| ![Python](media/sample-v2-code/logo_python.png) | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | Python için MSAL-Şu anda genel önizlemede |

Bu makaledeki kod parçacıkları ve aşağıdakiler öğesinden ayıklanır:

- [ASP.NET Core Web uygulaması artımlı öğreticisi, Bölüm 1](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg).
- [ASP.NET Web uygulaması örneği](https://github.com/Azure-Samples/ms-identity-aspnet-webapp-openidconnect)
- Microsoft Graph msal4j Web uygulaması örneğini [çağıran Java Web uygulaması](https://github.com/Azure-Samples/ms-identity-java-webapp) örneği
- [Microsoft Graph msal çağıran Python web uygulaması](https://github.com/Azure-Samples/ms-identity-python-webapp) . Python web uygulaması örneği

Tam uygulama ayrıntıları için bu öğreticilere ve örneğe başvurmak isteyebilirsiniz.

## <a name="configuration-files"></a>Yapılandırma dosyaları

Microsoft Identity platformu ile kullanıcıların oturum açmasını sağlayan Web uygulamaları genellikle yapılandırma dosyaları aracılığıyla yapılandırılır. Doldurmanız gereken ayarlar şunlardır:

- uygulamanızın çalışmasını `Instance` istiyorsanız bulut (örneğin, Ulusal bulutlar)
- içindeki hedef kitle`tenantId`
- Uygulamanız `clientId` için, Azure Portal kopyalandığı şekilde.

Bazen uygulamalar, `authority` `instance` ve ' nin birleşimi olan ile parametrized olabilir.`tenantId`

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core, bu ayarlar `appsettings.json` dosyada "azuread" bölümünde bulunur.

```Json
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.microsoftonline.com/" for Azure Public cloud.
    // "https://login.microsoftonline.us/" for Azure US government.
    // "https://login.microsoftonline.de/" for Azure AD Germany
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD Audience among:
    // - the tenant Id as a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account or Microsoft personal account
    // - "consumers" to sign-in users with Microsoft personal account only
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

ASP.NET Core, uygulamanız ve çeşitli profiller için URL`properties\launchSettings.json`(`applicationUrl`) ve SSL bağlantı noktasını (`sslPort`) içeren başka bir dosya () vardır.

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

Azure portal, uygulamanız için **kimlik doğrulama** sayfasına kaydolmanız gereken yanıt URI 'Lerinin bu URL 'lerle eşleşmesi gerekir; diğer bir deyişle, yukarıdaki `https://localhost:44321/signin-oidc` iki yapılandırma dosyası için `http://localhost:3110` ApplicationUrl olur ancak `/signin-oidc` `sslPort` belirtilir (44321) ve `CallbackPath` içinde `appsettings.json`tanımlanmıştır.
  
Aynı şekilde, oturum açma URI 'SI olarak `https://localhost:44321/signout-callback-oidc`ayarlanır.

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.net ' de, uygulama `Web.Config` dosya aracılığıyla yapılandırılır

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
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

Azure portal, uygulamanız için **kimlik doğrulama** sayfasına kaydolmanız gereken yanıt URI 'Lerinin bu URL 'lerle eşleşmesi gerekir; diğer bir `https://localhost:44326/`deyişle.

# <a name="javatabjava"></a>[Java](#tab/java)

Java 'da, yapılandırma altında bulunan `application.properties` dosyada bulunur`src/main/resources`

```Java
aad.clientId=Enter_the_Application_Id_here
aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
aad.secretKey=Enter_the_Client_Secret_Here
aad.redirectUriSignin=http://localhost:8080/msal4jsample/secure/aad
aad.redirectUriGraphUsers=http://localhost:8080/msal4jsample/graph/users
```

Azure Portal, uygulamanız için **kimlik doğrulama** sayfasında kaydolmanız gereken yanıt URI 'lerinin, uygulama `http://localhost:8080/msal4jsample/secure/aad` tarafından otomatik olarak tanımlanan redirectur`http://localhost:8080/msal4jsample/graph/users`

# <a name="pythontabpython"></a>[Python](#tab/python)

İşte [app_config. Kopyala](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/web_app_sample/app_config.py) içindeki Python yapılandırma dosyası

```Python
AUTHORITY = "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here"
CLIENT_ID = "Enter_the_Application_Id_here"
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
SCOPE = ["https://graph.microsoft.com/User.Read"]
REDIRECT_URI = "http://localhost:5000/getAToken"
```

---

## <a name="initialization-code"></a>Başlatma kodu

Başlatma kodu platforma bağlı olarak farklılık açmış. ASP.NET Core ve ASP.NET için, kullanıcıların oturum açması Openıdconnect ara yazılımı için temsilci olarak oluşturulur. Bugün ASP.NET/ASP.NET Core şablonu, Azure AD v 1.0 uç noktası için Web uygulamaları oluşturur. Bu nedenle, bunları Microsoft Identity platform (v 2.0) uç noktasına uyarlamak için bir dizi yapılandırma gerekir. Java söz konusu olduğunda, uygulamanın iş birliği ile Spring tarafından işlenir.

# <a name="aspnet-coretabaspnetcore"></a>[ASP.NET Core](#tab/aspnetcore)

ASP.NET Core Web Apps (ve Web API 'leri) içinde, denetleyicilerde veya denetleyici eylemlerinde bir `[Authorize]` özniteliğe sahip olduğunuz için uygulama korunur. Bu öznitelik, kullanıcının kimliğinin doğrulandığını denetler. Uygulama başlatmayı yapan kod `Startup.cs` dosyasında bulunur ve Microsoft Identity platformu (eskiden Azure AD v 2.0) ile kimlik doğrulaması eklemek için aşağıdaki kodu eklemeniz gerekir. Koddaki yorumların kendine açıklayıcı olması gerekir.

  > [!NOTE]
  > Projenizi Visual Studio içinde varsayılan ASP.NET Core Web projesi ile başlatırsanız veya `dotnet new mvc` ilgili paketler otomatik olarak yüklendiğinden, yöntemi `AddAzureAD` varsayılan olarak kullanılabilir.
  > Ancak, sıfırdan bir proje oluşturup aşağıdaki kodu kullanmaya çalışıyorsanız, `AddAzureAD` yöntemi kullanılabilir hale getirmek için projenize **"Microsoft. aspnetcore. Authentication. azuread. UI"** NuGet paketini eklemenizi öneririz.
  
Aşağıdaki kod [Başlangıçta kullanılabilir. cs # L33-L34](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/1-WebApp-OIDC/1-1-MyOrg/Startup.cs#L33-L34)

```CSharp
public class Startup
{
 ...

  // This method gets called by the runtime. Use this method to add services to the container.
  public void ConfigureServices(IServiceCollection services)
  {
    ...
      // Sign-in users with the Microsoft identity platform
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

, `AddMicrosoftIdentityPlatformAuthentication` [Microsoft. Identity. Web/WebAppServiceCollectionExtensions. cs # L23](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/faa94fd49c2da46b22d6694c4f5c5895795af26d/Microsoft.Identity.Web/WebAppServiceCollectionExtensions.cs#L23)içinde tanımlanan bir genişletme yöntemidir. İçerdiği

- kimlik doğrulama hizmetini ekler
- yapılandırma dosyasını okumak için seçenekleri yapılandırın
- OpenID Connect seçeneklerini, kullanılan yetkilinin Microsoft Identity platform (eski adıyla Azure AD v 2.0) uç noktası olması için yapılandırır
- belirtecin vereni onaylanır
- ada karşılık gelen talepler, KIMLIK belirtecindeki "preferred_username" talebi ile eşleştirilir 

Yapılandırmaya ek olarak, şunu çağırırken `AddMicrosoftIdentityPlatformAuthentication`şunları belirtebilirsiniz:

- yapılandırma bölümünün adı (varsayılan olarak AzureAD)
- kimlik doğrulaması çalışmazsa Web uygulamanızın sorunlarını gidermenize yardımcı olabilecek openıdconnect ara yazılım olaylarını izlemek istiyorsanız: ayarı `subscribeToOpenIdConnectMiddlewareDiagnosticsEvents` `true` , bilgilerin ASP.NET Core kümesi tarafından nasıl ayrıntılı olduğunu gösterir HTTP yanıtından, `HttpContext.User`içindeki kullanıcının kimliğine kadar ilerledikçe ara yazılım.

```CSharp
/// <summary>
/// Add authentication with Microsoft identity platform.
/// This method expects the configuration file will have a section named "AzureAd" with the necessary settings to initialize authentication options.
/// </summary>
/// <param name="services">Service collection to which to add this authentication scheme</param>
/// <param name="configuration">The Configuration object</param>
/// <param name="subscribeToOpenIdConnectMiddlewareDiagnosticsEvents">
/// Set to true if you want to debug, or just understand the OpenIdConnect events.
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
      // Per the code below, this application signs in users in any Work and School
      // accounts and any Microsoft Personal Accounts.
      // If you want to direct Azure AD to restrict the users that can sign-in, change
      // the tenant value of the appsettings.json file in the following way:
      // - only Work and School accounts => 'organizations'
      // - only Microsoft Personal accounts => 'consumers'
      // - Work and School and Personal accounts => 'common'
      // If you want to restrict the users that can sign-in to only one tenant
      // set the tenant value in the appsettings.json file to the tenant ID
      // or domain of this organization
      options.Authority = options.Authority + "/v2.0/";

      // If you want to restrict the users that can sign-in to several organizations
      // Set the tenant value in the appsettings.json file to 'organizations', and add the
      // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection
      options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.GetIssuerValidator(options.Authority).Validate;

      // Set the nameClaimType to be preferred_username.
      // This change is needed because certain token claims from Azure AD V1 endpoint
      // (on which the original .NET core template is based) are different than Microsoft identity platform endpoint.
      // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
      // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
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

`AadIssuerValidator` Sınıfı, Azure genel bulutunda veya ulusal bir şekilde, kullanıcıların kendi kişisel Microsoft hesaplarıyla oturum açtığı birçok durumda (v 1.0 veya v 2.0 belirteci, tek kiracılı veya çok kiracılı uygulama ya da uygulama ya da bir uygulamanın) doğrulanması için izin vermez. bulutlar). [Microsoft. Identity. Web/Resource/Aadıssuervalidator. cs](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/master/Microsoft.Identity.Web/Resource/AadIssuerValidator.cs) tarafından kullanılabilir

# <a name="aspnettabaspnet"></a>[ASP.NET](#tab/aspnet)

ASP.NET Web App/Web API 'lerinde kimlik doğrulamasıyla ilgili kod `App_Start/Startup.Auth.cs` dosyasında bulunur.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the identity platform endpoint - https://login.microsoftonline.com/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

# <a name="javatabjava"></a>[Java](#tab/java)

Java örneği yay çerçevesini kullanır. Uygulama, her HTTP yanıtını alan bir `Filter`uyguladığınız için korunur. Java Web uygulaması hızlı başlangıç sürümünde bu `AuthFilter`. `src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java` Filtre, OAuth 2,0 yetkilendirme kodu akışını işler ve bu nedenle:

- kullanıcının kimlik doğrulamasının yapılıp yapılmadığını doğrular (`isAuthenticated()` Yöntem)
- kullanıcının kimliği doğrulanmadıysa, Azure AD yetkilendirme uç noktalarının URL 'sini hesaplar ve tarayıcıyı bu URI 'ye yönlendirir
- Yanıt geldiğinde, kimlik doğrulama kod akışını içeren msal4j Token alma olanağı sağlar.
- son olarak belirteç uç noktasından (yeniden yönlendirme URI 'sindeki) belirteci aldığında Kullanıcı oturum açmış olur.

Ayrıntılar için bkz `doFilter()` [. AuthFilter. Java](https://github.com/Azure-Samples/ms-identity-java-webapp/blob/master/src/main/java/com/microsoft/azure/msalwebsample/AuthFilter.java) içindeki yöntemi

> [!NOTE]
> Kodu `doFilter()` biraz farklı bir düzende yazılır, ancak akış tanımlanan bir sıradır.

Bu yöntem tarafından tetiklenen yetkilendirme kodu akışı hakkında daha fazla bilgi için bkz. [Microsoft Identity platform ve OAuth 2,0 yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md)

# <a name="pythontabpython"></a>[Python](#tab/python)

Python örneği Flask kullanır. Flask ve MSAL başlatması. Python, uygulama içinde yapılır [. Kopyala # L1-L17](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e1199b4c3cdcb637cf0d8306832efbd85492e123/app.py#L1-L17)

```Python
import uuid
import flask
import requests
from flask import Flask, render_template, session, request
from flask_session import Session
import msal
import app_config

sess = Session()
app = Flask(__name__)
app.config.from_object('config.Config')
sess.init_app(app)
cache = msal.SerializableTokenCache()
application = msal.ConfidentialClientApplication(
    app_config.CLIENT_ID, authority=app_config.AUTHORITY,
    client_credential=app_config.CLIENT_SECRET,
    token_cache=cache)
```

Bu, MSAL. Kullanıcı oturum açmaya izin verme işlemini ele alacak Python. Bkz [. app. Sip # L74-84](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/e1199b4c3cdcb637cf0d8306832efbd85492e123/app.py#L74-84)

```Python
@app.route('/authenticate')
def authenticate():
    # Call to the authorize endpoint
    auth_state = str(uuid.uuid4())
    session[(request.cookies.get("session")+'state')] = auth_state
    authorization_url = application.get_authorization_request_url(app_config.SCOPE, state=auth_state,
                                                                  redirect_uri=app_config.REDIRECT_URI)
    resp = flask.Response(status=307)
    resp.headers['location'] = authorization_url
    return resp
```

---

## <a name="next-steps"></a>Sonraki adımlar

Sonraki makalede, oturum açma ve oturum kapatma işlemlerinin nasıl tetikleneceğini öğreneceksiniz.

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](scenario-web-app-sign-user-sign-in.md)
