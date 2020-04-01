---
title: Microsoft kimlik platformu kimlik doğrulama kitaplıkları | Microsoft Dokümanlar
description: Microsoft kimlik platformu bitiş noktası için uyumlu istemci kitaplıkları ve sunucu ara yazılım kitaplıkları, ilgili kitaplık, kaynak ve örnek bağlantılarla birlikte.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2019
ms.author: negoe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: c8f0d5d948ff54522e951b7b5d18c7018ae6d34a
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80419813"
---
# <a name="microsoft-identity-platform-authentication-libraries"></a>Microsoft kimlik platformu kimlik doğrulama kitaplıkları

[Microsoft kimlik platformu bitiş noktası,](active-directory-v2-compare.md) endüstri standardı OAuth 2.0 ve OpenID Connect 1.0 protokollerini destekler. Microsoft Kimlik Doğrulama Kitaplığı (MSAL), Microsoft kimlik platformu bitiş noktasıyla çalışmak üzere tasarlanmıştır. OAuth 2.0 ve OpenID Connect 1.0'ı destekleyen açık kaynak kitaplıklarını da kullanabilirsiniz.

Güvenlik Geliştirme Yaşam Döngüsü (SDL) metodolojisi izleyen protokol etki alanı uzmanları tarafından yazılmış kitaplıkları kullanmanızı öneririz. Bu tür metodolojiler [Microsoft'un izlediği metodolojileri][Microsoft-SDL]içerir. Protokoller için kod teslim ederseniz, Microsoft SDL gibi bir metodolojiizlemeniz gerekir. Her protokol için standart belirtimlerinde güvenlik hususlarına dikkat edin.

> [!NOTE]
> Azure Active Directory Authentication Library'yi (ADAL) mi arıyorsunuz? [ADAL kitaplık kılavuzuna](../azuread-dev/active-directory-authentication-libraries.md)göz atın.

## <a name="types-of-libraries"></a>Kütüphane türleri

Microsoft kimlik platformu bitiş noktası iki tür kitaplıkla çalışır:

* **İstemci kitaplıkları**: Yerel istemciler ve sunucular, Microsoft Graph gibi bir kaynağı çağırmak için erişim belirteçleri elde etmek için istemci kitaplıklarını kullanır.
* **Sunucu ara yazılım kitaplıkları**: Web uygulamaları, kullanıcı oturum açma için sunucu ara yazılım kitaplıklarını kullanır. Web API'leri, yerel istemciler veya diğer sunucular tarafından gönderilen belirteçleri doğrulamak için sunucu ara yazılım kitaplıklarını kullanır.

## <a name="library-support"></a>Kütüphane desteği

Kitaplıklar iki destek kategorisinde gelir:

* **Microsoft destekli**: Microsoft bu kitaplıklar için düzeltmeler sağlar ve bu kitaplıklar üzerinde SDL durum tespiti yapmıştır.
* **Uyumlu**: Microsoft bu kitaplıkları temel senaryolarda sınamıştır ve Microsoft kimlik platformu bitiş noktasıyla çalıştıklarını doğrulamıştır. Microsoft bu kitaplıklar için düzeltmeler sağlamaz ve bu kitaplıklar üzerinde inceleme yapmamıştır. Sorunlar ve özellik istekleri kitaplığın açık kaynak projesine yönlendirilmelidir.

Microsoft kimlik platformu bitiş noktasıyla çalışan kitaplıkların listesi için aşağıdaki bölümlere bakın.

## <a name="microsoft-supported-client-libraries"></a>Microsoft destekli istemci kitaplıkları

Korumalı web API'sini çağırmak için bir belirteç elde etmek için istemci kimlik doğrulama kitaplıklarını kullanın.

| Platform | Kitaplık | İndirme | Kaynak kod | Örnek | Başvuru | Kavramsal doküman | Yol Haritası |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ![JavaScript](media/sample-v2-code/logo_js.png) | MSAL.js  | [NPM](https://www.npmjs.com/package/msal) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/README.md) |  [Tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) | [Başvuru](https://azuread.github.io/microsoft-authentication-library-for-js/ref/msal-core/) | [Kavramsal dokümanlar](msal-overview.md)| [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)
![Angular](media/sample-v2-code/logo_angular.png) | MSAL Açısal | [NPM](https://www.npmjs.com/package/@azure/msal-angular) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | [Açısal SPA](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) | [Başvuru](https://azuread.github.io/microsoft-authentication-library-for-js/ref/msal-angular/) | [Kavramsal dokümanlar](msal-overview.md) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)
| ![.NET Framework](media/sample-v2-code/logo_NET.png) ![UWP](media/sample-v2-code/logo_windows.png) ![Xamarin](media/sample-v2-code/logo_xamarin.png) | MSAL.NET  |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Masaüstü uygulaması](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) | [MSAL.NET](https://docs.microsoft.com/dotnet/api/microsoft.identity.client?view=azure-dotnet-preview) |[Kavramsal dokümanlar](msal-overview.md) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki#roadmap)
| ![Python](media/sample-v2-code/logo_python.png) | MSAL Piton | [PyPI](https://pypi.org/project/msal) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [Örnekler](https://github.com/AzureAD/microsoft-authentication-library-for-python/tree/dev/sample) | [ReadTheDocs](https://msal-python.rtfd.io/) | [Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Roadmap)
| ![Java](media/sample-v2-code/logo_java.png) | MSAL Java | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-java) | [Örnekler](https://github.com/AzureAD/microsoft-authentication-library-for-java/tree/dev/src/samples) | [Başvuru](https://javadoc.io/doc/com.microsoft.azure/msal4j/latest/index.html) | [Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki) | [Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-java/wiki)
| iOS & macOS | MSAL iOS ve macOS | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS uygulaması](https://github.com/Azure-Samples/ms-identity-mobile-apple-swift-objc), [macOS uygulaması](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) | [Başvuru](https://azuread.github.io/microsoft-authentication-library-for-objc/index.html)  | [Kavramsal dokümanlar](msal-overview.md) | |
|![Android / Java](media/sample-v2-code/logo_Android.png) | MSAL Android | [Merkezi depo](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android uygulaması](quickstart-v2-android.md) | [JavaDocs](https://javadoc.io/doc/com.microsoft.identity.client/msal) | [Kavramsal dokümanlar](msal-overview.md) |[Yol Haritası](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Roadmap)

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft destekli sunucu ara yazılım kitaplıkları

Web uygulamalarını ve web API'lerini korumaya yardımcı olmak için ara yazılım kitaplıklarını kullanın. ASP.NET veya ASP.NET Core ile yazılmış web uygulamaları veya web API'leri ara yazılım kitaplıklarını kullanır.

| Platform | Kitaplık | İndirme | Kaynak Kodu | Örnek | Başvuru
| --- | --- | --- | --- | --- | --- |
| ![.NET](media/sample-v2-code/logo_NET.png) ![.NET Core](media/sample-v2-code/logo_NETcore.png) | ASP.NET Güvenlik |[NuGet](https://www.nuget.org/packages/Microsoft.AspNet.Mvc/) |[GitHub](https://github.com/aspnet/AspNetCore) |[MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[ASP.NET API başvurusu](https://docs.microsoft.com/dotnet/api/?view=aspnetcore-2.0) |
| ![.NET](media/sample-v2-code/logo_NET.png)| .NET için IdentityModel Uzantıları| |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | [MVC uygulaması](quickstart-v2-aspnet-webapp.md) |[Başvuru](https://docs.microsoft.com/dotnet/api/overview/azure/activedirectory/client?view=azure-dotnet) |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | Azure AD Passport |[NPM](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Web uygulaması](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) | |

## <a name="microsoft-supported-libraries-by-os--language"></a>Os / dil tarafından Microsoft destekli kitaplıklar

Desteklenen işletim sistemleri vs diller açısından, eşleme aşağıdaki gibidir:

|             | Windows    | Linux      | macOS      | iOS | Android    |
|-------------|------------|------------|------------|------------|------------|
| ![JavaScript](media/sample-v2-code/logo_js.png)  |  MSAL.js | MSAL.js | MSAL.js | MSAL.js |  MSAL.js |
| <img alt="C#" src="../../cognitive-services/speech-service/media/index/logo_csharp.svg" width="64px" height="64px" /> | ASP.NET, ASP.NET Çekirdek, MSAL.Net (.NET FW, Çekirdek, UWP)| ASP.NET Çekirdek, MSAL.Net (.NET Çekirdek) | ASP.NET Çekirdek, MSAL.Net (macOS)       | MSAL.Net (Xamarin.iOS) | MSAL.Net (Xamarin.Android)|
| Swift <br> Objective-C |            |            | [iOS ve macOS için MSAL](msal-overview.md) | [iOS ve macOS için MSAL](msal-overview.md) |            |
| ![Java](media/sample-v2-code/logo_java.png) Java | msal4j | msal4j | msal4j | | MSAL Android |
| ![Python](media/sample-v2-code/logo_python.png) Python | MSAL Piton | MSAL Piton | MSAL Piton |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) Node.JS | Passport.node | Passport.node | Passport.node |

Ayrıca [desteklenen platformlara ve dillere](authentication-flows-app-scenarios.md#scenarios-and-supported-platforms-and-languages) göre senaryolara bakın

## <a name="compatible-client-libraries"></a>Uyumlu istemci kitaplıkları

| Platform | Kitaplık adı | Test edilmiş sürüm | Kaynak kod | Örnek |
|:---:|:---:|:---:|:---:|:---:|
|![JavaScript](media/sample-v2-code/logo_js.png)|[Merhaba.js](https://adodson.com/hello.js/) | Sürüm 1.13.5 |[Merhaba.js](https://github.com/MrSwitch/hello.js) |[Spa](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2) |
| ![Java](media/sample-v2-code/logo_java.png) | [Scribe Java](https://github.com/scribejava/scribejava) | [Sürüm 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| ![Java](media/sample-v2-code/logo_java.png) | [Gluu OpenID Connect kitaplığı](https://github.com/GluuFederation/oxAuth) | [Sürüm 3.0.2](https://github.com/GluuFederation/oxAuth/releases/tag/3.0.2) | [Gluu OpenID Connect kitaplığı](https://github.com/GluuFederation/oxAuth) | |
| ![Python](media/sample-v2-code/logo_python.png) | [İstekler-OAuthlib](https://github.com/requests/requests-oauthlib) | [Sürüm 1.2.0](https://github.com/requests/requests-oauthlib/releases/tag/v1.2.0) | [İstekler-OAuthlib](https://github.com/requests/requests-oauthlib) | |
| ![Node.js](media/sample-v2-code/logo_nodejs.png) | [openid istemci](https://github.com/panva/node-openid-client) | [Sürüm 2.4.5](https://github.com/panva/node-openid-client/releases/tag/v2.4.5) | [openid istemci](https://github.com/panva/node-openid-client) | |
| ![PHP](media/sample-v2-code/logo_php.png) | [PHP Ligi oauth2-istemci](https://github.com/thephpleague/oauth2-client) | [Sürüm 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-istemci](https://github.com/thephpleague/oauth2-client/) | |
| ![Ruby](media/sample-v2-code/logo_ruby.png) |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth: 1.3.1<br />omniauth-oauth2: 1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)<br />[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |
| iOS, macOS, & Android  | [Tepki Yerli App Auth](https://github.com/FormidableLabs/react-native-app-auth) | [Sürüm 4.2.0](https://github.com/FormidableLabs/react-native-app-auth/releases/tag/v4.2.0) | [Tepki Yerli App Auth](https://github.com/FormidableLabs/react-native-app-auth) | |

Standartlara uygun kitaplık için Microsoft kimlik platformu bitiş noktasını kullanabilirsiniz. Destek için nereye gideceğimi bilmek önemlidir:

* Kitaplık kodundaki sorunlar ve yeni özellik istekleri için kitaplık sahibine başvurun.
* Hizmet tarafı iletişim kuralı uygulamasındaki sorunlar ve yeni özellik istekleri için Microsoft'a başvurun.
* Protokolde görmek istediğiniz ek özellikler için [bir özellik isteği dosya.](https://feedback.azure.com/forums/169401-azure-active-directory)
* Microsoft kimlik platformu bitiş noktasının OAuth 2.0 veya OpenID Connect 1.0 ile uyumlu olmadığı bir sorun bulursanız [bir destek isteği oluşturun.](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

## <a name="related-content"></a>İlgili içerik

Microsoft kimlik platformu bitiş noktası hakkında daha fazla bilgi için [Microsoft kimlik platformuna genel bakış'a][AAD-App-Model-V2-Overview]bakın.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[ClientLib-NET-Lib]: https://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: https://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: https://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
