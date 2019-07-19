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
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c962e95b3d213c4089b51f58139cab17a3332cbd
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67853064"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Kullanıcıları oturum açan Web uygulaması-kod yapılandırması

Kullanıcıları oturum açan Web uygulamanız için kodu yapılandırmayı öğrenin.

## <a name="libraries-used-to-protect-web-apps"></a>Web Apps korumak için kullanılan kitaplıklar

<!-- This section can be in an include for Web App and Web APIs -->
Bir Web uygulamasını (ve bir Web API 'sini) korumak için kullanılan kitaplıklar şunlardır:

| Platform | Kitaplık | Açıklama |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [.NET için kimlik modeli uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Doğrudan ASP.NET ve ASP.NET Core tarafından kullanılan .NET için Microsoft Identity Extensions, hem .NET Framework hem de .NET Core üzerinde çalışan bir dll kümesini önerir. Bir ASP.NET/ASP.NET Core Web uygulamasından **Tokenvalidationparameters** sınıfını kullanarak belirteç doğrulamayı denetleyebilirsiniz (bazı ISV senaryolarında özellikle) |

## <a name="aspnet-core-configuration"></a>ASP.NET Core yapılandırması

Bu makaledeki kod parçacıkları ve aşağıdakiler [ASP.NET Core Web uygulaması artımlı öğreticisi, Bölüm 1 '](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg)den ayıklanır. Tam uygulama ayrıntıları için Bu öğreticiye başvurmak isteyebilirsiniz.

### <a name="application-configuration-files"></a>Uygulama yapılandırma dosyaları

ASP.NET Core, Microsoft Identity platformu olan bir Web uygulaması oturum açan kullanıcılar `appsettings.json` dosya aracılığıyla yapılandırılır. Doldurmanız gereken ayarlar şunlardır:

- Uygulamanızın Ulusal `Instance` bulutlarda çalışmasını istiyorsanız bulut
- içindeki hedef kitle`tenantId`
- Uygulamanız `clientId` için, Azure Portal kopyalandığı şekilde.

```JSon
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

ASP.NET Core, uygulamanızın URL 'sini (`applicationUrl`) ve SSL bağlantı noktasını (`sslPort`) içeren başka bir dosya vardır ve çeşitli profiller de vardır.

```JSon
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
  
Aynı şekilde, oturum kapatma URI 'SI olarak `https://localhost:44321/signout-callback-oidc`ayarlanır.

### <a name="initialization-code"></a>Başlatma kodu

ASP.NET Core Web Apps (ve Web API 'leri) içinde, uygulama başlatmayı yapan kod `Startup.cs` dosyasında bulunur ve Microsoft Identity platform (eskiden Azure AD) v 2.0 ile kimlik doğrulaması eklemek için aşağıdaki kodu eklemeniz gerekir. Koddaki yorumların kendine açıklayıcı olması gerekir.

  > [!NOTE]
  > Projenizi Visual Studio içinde varsayılan ASP.NET Core Web projesi ile başlatırsanız veya `dotnet new mvc` ilgili paketler otomatik olarak yüklendiğinden, yöntemi `AddAzureAD` varsayılan olarak kullanılabilir. Ancak, sıfırdan bir proje oluşturup aşağıdaki kodu kullanmaya çalışıyorsanız, `AddAzureAD` yöntemi kullanılabilir hale getirmek için projenize **"Microsoft. aspnetcore. Authentication. azuread. UI"** NuGet paketini eklemenizi öneririz.
  
```CSharp
 services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
         .AddAzureAD(options => configuration.Bind("AzureAd", options));

 services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
 {
  // The ASP.NET core templates are currently using Azure AD v1.0, and compute
  // the authority (as {Instance}/{TenantID}). We want to use the Microsoft Identity Platform v2.0 endpoint
  options.Authority = options.Authority + "/v2.0/";

  // If you want to restrict the users that can sign-in to specific organizations
  // Set the tenant value in the appsettings.json file to 'organizations', and add the
  // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection.
  // Otherwise validate the issuer
  options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ForAadInstance(options.Authority).ValidateAadIssuer;

  // Set the nameClaimType to be preferred_username.
  // This change is needed because certain token claims from Azure AD v1.0 endpoint
  // (on which the original .NET core template is based) are different in Azure AD v2.0 endpoint.
  // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
  // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
  options.TokenValidationParameters.NameClaimType = "preferred_username";
  ...
```

## <a name="aspnet-configuration"></a>ASP.NET yapılandırması

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

ASP.NET Web App/Web API 'lerinde kimlik doğrulamasıyla ilgili kod `App_Start/Startup.Auth.cs` dosyasında bulunur.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum aç ve oturumu Kapat](scenario-web-app-sign-user-sign-in.md)
