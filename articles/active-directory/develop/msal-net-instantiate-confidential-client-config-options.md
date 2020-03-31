---
title: Gizli bir istemci uygulamasını anında anons edin (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: .NET (MSAL.NET için Microsoft Kimlik Doğrulama Kitaplığı'nı kullanarak yapılandırma seçenekleriyle gizli bir istemci uygulamasını anında nasıl anons edebilirsiniz öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 1a520c5a1002e401f880fba84f8fc02a0a678133
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77084741"
---
# <a name="instantiate-a-confidential-client-application-with-configuration-options-using-msalnet"></a>MSAL.NET kullanarak yapılandırma seçenekleriyle gizli bir istemci uygulamasını anında

Bu makalede, .NET (MSAL.NET) için Microsoft Kimlik Doğrulama Kitaplığı kullanarak gizli bir [istemci uygulamasının](msal-client-applications.md) anlık olarak nasıl anında gerçekleştirilebildiğini açıklanmaktadır.  Uygulama, ayarlar dosyasında tanımlanan yapılandırma seçenekleriyle anında doldurulabilir.

Bir uygulamayı başlatmadan önce, uygulamanızın Microsoft kimlik platformuyla tümleştirilebilmeleri için uygulamanızı [kaydetmeniz](quickstart-register-app.md) gerekir. Kayıt olduktan sonra aşağıdaki bilgilere ihtiyacınız olabilir (Azure portalında bulunabilir):

- İstemci kimliği (GUID'i temsil eden bir dize)
- Kimlik sağlayıcı URL (örneğin adı) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre topluca otorite olarak bilinir.
- Yalnızca kuruluşunuz için bir iş başvurusu satırı yazıyorsanız kiracı kimliği (tek kiracılı uygulama olarak da adlandırılır).
- Uygulama gizli (istemci gizli dize) veya sertifika (tip X509Certificate2) gizli bir istemci uygulaması ise.
- Web uygulamaları ve bazen kamu istemcisi uygulamaları için (özellikle uygulamanızın bir broker kullanması gerektiğinde), kimlik sağlayıcısının güvenlik belirteçleri ile uygulamanıza geri döneceği redirectUri'yi de ayarlamış olacaksınız.

## <a name="configure-the-application-from-the-config-file"></a>Uygulamayı config dosyasından yapılandırma
MSAL.NET'daki seçeneklerin özelliklerinin adı, ASP.NET `AzureADOptions` Core'daki özelliklerin adla eşleşir, böylece yapıştırıcı kodu yazmanız gerekmez.

bir ASP.NET Core uygulama *yapılandırması bir appsettings.json* dosyasında açıklanmıştır:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "[Enter the domain of your tenant, e.g. contoso.onmicrosoft.com]",
    "TenantId": "[Enter 'common', or 'organizations' or the Tenant Id (Obtained from the Azure portal. Select 'Endpoints' from the 'App registrations' blade and use the GUID in any of the URLs), e.g. da41245a5-11b3-996c-00a8-4d99re19f292]",
    "ClientId": "[Enter the Client Id (Application ID obtained from the Azure portal), e.g. ba74781c2-53c2-442a-97c2-3d60re42f403]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    "ClientSecret": "[Copy the client secret added to the app from the Azure portal]"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

v3.x MSAL.NET itibaren, config dosyasından gizli istemci uygulamanızı yapılandırabilirsiniz.

Uygulamanızı yapılandırmak ve anında yapmak istediğiniz sınıfta bir `ConfidentialClientApplicationOptions` nesne bildirmeniz gerekir.  `IConfigurationRoot.Bind()` [Microsoft.Extensions.Configuration.Binder nuget paketinden](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder)yöntemi kullanarak, kaynaktan okunan yapılandırmayı (appconfig.json dosyası dahil) uygulama seçenekleri örneğine bağla:

```csharp
using Microsoft.Identity.Client;

private ConfidentialClientApplicationOptions _applicationOptions;
_applicationOptions = new ConfidentialClientApplicationOptions();
configuration.Bind("AzureAD", _applicationOptions);
```

Bu, *appsettings.json* dosyasının "AzureAD" bölümünün içeriğinin `ConfidentialClientApplicationOptions` nesnenin ilgili özelliklerine bağlanmasını sağlar.  Ardından, bir `ConfidentialClientApplication` nesne oluşturun:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
        .Build();
```

## <a name="add-runtime-configuration"></a>Çalışma zamanı yapılandırması ekleme
Gizli bir istemci uygulamasında, genellikle kullanıcı başına bir önbelleğiniz var. Bu nedenle önbelleği kullanıcıyla ilişkilendirmeniz ve uygulama oluşturucuya kullanmak istediğinizi bildirmeniz gerekir. Aynı şekilde, dinamik olarak hesaplanmış bir yeniden yönlendirme URI olabilir. Bu durumda kod aşağıdaki gibidir:

```csharp
IConfidentialClientApplication app;
var request = httpContext.Request;
var currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, _azureAdOptions.CallbackPath ?? string.Empty);
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
       .WithRedirectUri(currentUri)
       .Build();
TokenCache userTokenCache = _tokenCacheProvider.SerializeCache(app.UserTokenCache,httpContext, claimsPrincipal);
```

