---
title: Gizli bir istemci uygulaması örneğini oluşturma (MSAL.NET) | Mavisi
titleSuffix: Microsoft identity platform
description: .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak yapılandırma seçenekleriyle gizli bir istemci uygulamasını nasıl örnekleyeceğinizi öğrenin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 04/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: a76935c5b826f8aa686167f702f7170522744155
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85477473"
---
# <a name="instantiate-a-confidential-client-application-with-configuration-options-using-msalnet"></a>MSAL.NET kullanarak yapılandırma seçenekleriyle bir gizli istemci uygulaması örneği oluşturma

Bu makalede, .NET için Microsoft kimlik doğrulama kitaplığı 'nı (MSAL.NET) kullanarak bir [Gizli istemci uygulamasının](msal-client-applications.md) örneğini oluşturma açıklanmaktadır.  Uygulama, bir ayar dosyasında tanımlanan yapılandırma seçenekleriyle birlikte oluşturulur.

Uygulamayı başlatmadan önce, uygulamanızın Microsoft Identity platformu ile tümleştirilebilmesi için öncelikle [kaydetmeniz](quickstart-register-app.md) gerekir. Kayıttan sonra, aşağıdaki bilgiler (Azure portal bulunabilir) gerekebilir:

- İstemci KIMLIĞI (bir GUID 'YI temsil eden dize)
- Kimlik sağlayıcısı URL 'SI (örnek olarak adlandırılır) ve uygulamanız için oturum açma hedef kitlesi. Bu iki parametre, her topluca yetkili olarak bilinir.
- Yalnızca kuruluşunuz için bir iş kolu uygulaması yazıyorsanız (tek kiracılı uygulama olarak da adlandırılır) kiracı KIMLIĞI.
- Gizli bir istemci uygulaması ise, uygulama gizli anahtarı (istemci gizli dizisi) veya sertifika (X509Certificate2 türünde).
- Web uygulamaları için ve bazen genel istemci uygulamaları için (uygulamanızın bir aracı kullanması gerektiğinde), kimlik sağlayıcısının güvenlik belirteçleriyle uygulamanızı geri yükleyeceğim yeniden yönlendirilebilir.

## <a name="configure-the-application-from-the-config-file"></a>Uygulamayı yapılandırma dosyasından yapılandırma
MSAL.NET içindeki seçeneklerin özelliklerinin adı ASP.NET Core içindeki özelliklerinin adı ile eşleşir `AzureADOptions` , bu nedenle herhangi bir birleştirici kod yazmanız gerekmez.

Bir ASP.NET Core uygulama yapılandırması, bir *appsettings.js* dosyasında açıklanmıştır:

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

MSAL.NET v3. x sürümünden başlayarak, gizli istemci uygulamanızı yapılandırma dosyasından yapılandırabilirsiniz.

Uygulamanızı yapılandırmak ve örneklerinizi başlatmak istediğiniz sınıfta bir nesne bildirmeniz gerekir `ConfidentialClientApplicationOptions` .  Microsoft.Extensions.Configbir yöntemi kullanarak, kaynaktan okunan yapılandırmayı (dosyadaki appconfig.jsdosyası dahil) uygulama seçeneklerinin örneğine bağlayın `IConfigurationRoot.Bind()` [. Ciltçi NuGet paketi](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder):

```csharp
using Microsoft.Identity.Client;

private ConfidentialClientApplicationOptions _applicationOptions;
_applicationOptions = new ConfidentialClientApplicationOptions();
configuration.Bind("AzureAD", _applicationOptions);
```

Bu, dosyadaki *appsettings.js* "azuread" bölümünün içeriğinin nesnenin karşılık gelen özelliklerine bağlanmasını sağlar `ConfidentialClientApplicationOptions` .  Sonra, bir `ConfidentialClientApplication` nesne oluşturun:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
        .Build();
```

## <a name="add-runtime-configuration"></a>Çalışma Zamanı Yapılandırması Ekle
Gizli bir istemci uygulamasında, genellikle Kullanıcı başına bir önbelleğiniz olur. Bu nedenle, kullanıcıyla ilişkili önbelleği almanız ve uygulamayı kullanmak istediğiniz uygulama oluşturucusunu bildirmeniz gerekecektir. Aynı şekilde, dinamik olarak hesaplanmış bir yeniden yönlendirme URI 'SI olabilir. Bu durumda, kod aşağıdaki gibi olur:

```csharp
IConfidentialClientApplication app;
var request = httpContext.Request;
var currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, _azureAdOptions.CallbackPath ?? string.Empty);
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
       .WithRedirectUri(currentUri)
       .Build();
TokenCache userTokenCache = _tokenCacheProvider.SerializeCache(app.UserTokenCache,httpContext, claimsPrincipal);
```

