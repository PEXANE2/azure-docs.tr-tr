---
title: Microsoft kimlik platformu için kod örnekleri | Microsoft Dokümanlar
description: Senaryoya göre düzenlenen kullanılabilir Microsoft kimlik platformu (v2.0 uç noktası) kod örneklerinin dizinini sağlar.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 2420c98a95eb9e5cdfee36bbd9bec20d22ad24ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460611"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft kimlik platformu kodu örnekleri (v2.0 bitiş noktası)

Microsoft kimlik platformundan şu şekilde yararlanabilirsiniz:

- Web uygulamalarınız ve web API'lerinize kimlik doğrulama ve yetkilendirme ekleyin.
- Korumalı bir web API'sine erişmek için bir erişim belirteci gereksinimi.

Bu makalede, Microsoft kimlik platformu bitiş noktası için örneklere bağlantılar kısaca açıklar ve sağlar. Bu örnekler, nasıl yapıldığını gösterir ve uygulamalarınızda kullanabileceğiniz kod parçacıkları da sağlar. Kod örneği sayfasında, gereksinimler, kurulum ve kurulumla ilgili yardımcı olan ayrıntılı okuma konuları bulacaksınız. Kod içindeki yorumlar, kritik bölümleri anlamanıza yardımcı olur.

> [!NOTE]
> v1.0 örnekleriyle ilgileniyorsanız, [Azure AD kodu örneklerine (v1.0 bitiş noktası)](../azuread-dev/sample-v1-code.md)bakın.

Her örnek türü için temel senaryoyu anlamak [için Microsoft kimlik platformu bitiş noktası için Uygulama türlerine](v2-app-types.md)bakın.

Ayrıca GitHub'daki örneklere de katkıda bulunabilirsiniz. Nasıl yapılacağını öğrenmek için [Microsoft Azure Etkin Dizin örnekleri ne ve belgelerine](https://github.com/Azure-Samples?page=3&query=active-directory)bakın.

## <a name="single-page-applications"></a>Tek sayfauygulamaları

Bu örnekler, Microsoft kimlik platformu yla güvenli tek sayfalı bir uygulamanın nasıl yazılabildiğini gösterir. Bu örnekler MSAL.js tatlarından birini kullanır.

| Platform | Açıklama | Bağlantı |
| -------- | --------------------- | -------- |
| ![Bu resim JavaScript](media/sample-v2-code/logo_js.png) logosu [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterir | Aramalar Microsoft Graph |[javascript-graphapi-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2) |
| ![Bu resim JavaScript](media/sample-v2-code/logo_js.png) logosu [JavaScript (msal.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterir | Aramalar B2C |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![Bu resim Açısal JS](media/sample-v2-code/logo_angular.png) logosu [JavaScript (MSAL AçısalJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs) gösterir| Aramalar Microsoft Graph  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/MsalAngularjsDemoApp)
| ![Bu resim, Açısal](media/sample-v2-code/logo_angular.png) logo [JavaScript (MSAL Açısal)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) gösterir| Aramalar Microsoft Graph  | [javascript-singlepageapp-açısal](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) |
| ![Bu resim, Açısal](media/sample-v2-code/logo_angular.png) logo [JavaScript (MSAL Açısal)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterir | Aramalar B2C |[aktif-dizin-b2c-javascript-açısal-spa](https://github.com/Azure-Samples/active-directory-b2c-javascript-angular-spa) |

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örnekler, kullanıcıları oturum açan web uygulamalarını göstermektedir. Bazı örnekler, Microsoft Graph'ı veya kullanıcı kimliğini içeren kendi web API'nizi çağıran uygulamayı da gösterir.

| Platform | Yalnızca kullanıcılarda işaretler | Kullanıcılarda işaretler ve Microsoft Graph çağrıları |
| -------- | ------------------- | --------------------------------- |
| ![Bu resim ASP.NET Core logosunu gösterir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdek 2.2 | [ASP.NET Core WebApp'ta bulunan kullanıcılar öğretici](https://aka.ms/aspnetcore-webapp-sign-in) | ASP.NET Core [Web App](https://aka.ms/aspnetcore-webapp-call-msgraph) aynı örnek Microsoft Graph faz çağırır |
| ![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [ASP.NET Quickstart](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-sınırlı kapsamlar-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-eğitim-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png)  |                   | [ms-kimlik-java-webapp](https://github.com/Azure-Samples/ms-identity-java-webapp) |
| ![Bu resim Python logosunu gösterir](media/sample-v2-code/logo_python.png)  |                   | [ms-kimlik-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp) |
| ![Bu resim Ruby logosunu gösterir](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-eğitim-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil genel istemci uygulamaları

Aşağıdaki örnekler, Microsoft Graph API'ye veya kullanıcı adına kendi web API'nıza erişen ortak istemci uygulamalarını (masaüstü veya mobil uygulamalar) gösterir. Tüm bu istemci uygulamaları Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kullanır.

| İstemci başvurusu | Platform | Akış / Hibe | Aramalar Microsoft Graph | ASP.NET Core 2.0 web API'yi çağırır |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Masaüstü (WPF)      | ![Bu resim .NET/C# logosunu gösterir](media/sample-v2-code/logo_NET.png) | [etkileşimli](msal-authentication-flows.md#interactive)| [dotnet-masaüstü-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-yerli-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Masaüstü (Konsol)   | ![Bu resim .NET/C# (Masaüstü) logosunu gösterir](media/sample-v2-code/logo_NET.png) | [Tümleşik Windows Kimlik Doğrulaması](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Masaüstü (Konsol)   | ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png) | [Tümleşik Windows Kimlik Doğrulaması](msal-authentication-flows.md#integrated-windows-authentication) |[ms-identity-java-masaüstü](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Masaüstü (Konsol)   | ![Bu resim .NET/C# (Masaüstü) logosunu gösterir](media/sample-v2-code/logo_NETcore.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| WAM ile Masaüstü (Konsol)  | ![Bu resim .NET/C# (Masaüstü) logosunu gösterir](media/sample-v2-code/logo_NETcore.png) | [WAM ile etkileşimli](msal-authentication-flows.md#interactive) |[dotnet-yerli-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  |
| Masaüstü (Konsol)   | ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[ms-identity-java-masaüstü](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Masaüstü (Konsol)   | ![Bu resim Python logosunu gösterir](media/sample-v2-code/logo_python.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[ms-identity-python-masaüstü](https://github.com/Azure-Samples/ms-identity-python-desktop) |  |
| Mobil (Android, iOS, UWP)   | ![Bu resim .NET/C# (Xamarin) logosunu gösterir](media/sample-v2-code/logo_xamarin.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[xamarin-yerli-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobil (iOS)       | ![Bu resim iOS/Objective-C veya Swift'i gösterir](media/sample-v2-code/logo_iOS.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[ios-swift-objc-yerli-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-yerli-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Masaüstü (macOS)       | macOS | [etkileşimli](msal-authentication-flows.md#interactive) |[macOS-swift-objc-yerli-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Mobil (Android-Java)   | ![Bu resim Android logosunu gösterir](media/sample-v2-code/logo_Android.png) | [etkileşimli](msal-authentication-flows.md#interactive) |  [android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Mobil (Android-Kotlin)   | ![Bu resim Android logosunu gösterir](media/sample-v2-code/logo_Android.png) | [etkileşimli](msal-authentication-flows.md#interactive) |  [android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Daemon uygulamaları

Aşağıdaki örnekler, Microsoft Graph API'sine kendi kimliğiyle (kullanıcı olmadan) erişen bir uygulama gösterir.

| İstemci başvurusu | Platform | Akış / Hibe | Aramalar Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Konsol | ![Bu resim .NET Core logosunu gösterir](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [İstemci Kimlik Bilgileri](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web uygulaması | ![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [İstemci Kimlik Bilgileri](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |
| Konsol | ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png) | [İstemci Kimlik Bilgileri](msal-authentication-flows.md#client-credentials) | [ms-kimlik-java-daemon](https://github.com/Azure-Samples/ms-identity-java-daemon) |
| Konsol | ![Bu resim Python logosunu gösterir](media/sample-v2-code/logo_python.png) | [İstemci Kimlik Bilgileri](msal-authentication-flows.md#client-credentials) | [ms-kimlik-python-daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) |

## <a name="headless-applications"></a>Başsız uygulamalar

Aşağıdaki örnek, web tarayıcısı olmayan bir aygıtta çalışan bir ortak istemci uygulamasını gösterir. Uygulama bir komut satırı aracı, Linux veya Mac üzerinde çalışan bir uygulama veya bir IoT uygulaması olabilir. Örnekte, başka bir cihazda (cep telefonu gibi) etkileşimli olarak giriş yapan bir kullanıcı adına Microsoft Graph API'ye erişen bir uygulama bulunur. Bu istemci uygulaması Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kullanır.

| İstemci başvurusu | Platform | Akış / Hibe | Aramalar Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Masaüstü (Konsol)   | ![Bu resim .NET/C# (Masaüstü) logosunu gösterir](media/sample-v2-code/logo_NETcore.png) | [Cihaz kodu akışı](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |
| Masaüstü (Konsol)   | ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png) | [Cihaz kodu akışı](msal-authentication-flows.md#device-code) |[ms-identity-java-devicecodeflow](https://github.com/Azure-Samples/ms-identity-java-devicecodeflow) |
| Masaüstü (Konsol)   | ![Bu resim Python logosunu gösterir](media/sample-v2-code/logo_python.png) | [Cihaz kodu akışı](msal-authentication-flows.md#device-code) |[ms-identity-python-devicecodeflow](https://github.com/Azure-Samples/ms-identity-python-devicecodeflow) |

## <a name="web-apis"></a>Web API'leri

Aşağıdaki örnekler, Microsoft kimlik platformu bitiş noktasıyla bir web API'sinin nasıl korunup korunulmasını ve web API'sinden bir akış aşağı API'yi nasıl çağırılabildiğini gösterir.

| Platform | Örnek |
| -------- | ------------------- |
| ![Bu resim ASP.NET Core logosunu gösterir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdek 2.2 | ASP.NET Çekirdek web API (hizmet) [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web API (hizmet) [ms-kimlik-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |
| ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo_java.png) | Web API (hizmet) [ms-identity-java-webapi](https://github.com/Azure-Samples/ms-identity-java-webapi) |
| ![Bu resim Düğüm.js logosunu gösterir](media/sample-v2-code/logo_nodejs.png) | [Etkin-dizin-javascript-nodejs-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2) Web API (hizmet) |
| ![Bu resim Düğüm.js logosunu gösterir](media/sample-v2-code/logo_nodejs.png) | B2C Web API (hizmet) [aktif-dizin-b2c-javascript-nodejs-webapi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) |

## <a name="azure-functions-as-web-apis"></a>Web API'leri olarak Azure İşlevleri

Aşağıdaki örnekler, HttpTrigger kullanarak bir Azure Işlevini nasıl koruyacağını ve Microsoft kimlik platformu bitiş noktasıyla bir web API'sini nasıl açığa çıkarılabildiğini ve web API'sinden bir alt akış API'sini nasıl çağırılamayı gösterir.

| Platform | Örnek |
| -------- | ------------------- |
| ![Bu resim ASP.NET Core logosunu gösterir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdek 2.2 | ASP.NET Core web API (hizmet) Azure İşlevi [dotnet-native-aspnetcore-v2](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)  |
| ![Bu resim Düğüm.js logosunu gösterir](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | NodeJS web API (hizmet) [ve pasaport-azure-reklam](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) |
| ![Bu resim Python logosunu gösterir](media/sample-v2-code/logo_python.png)</p>Python | [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions) Web API (hizmet) |
| ![Bu resim Düğüm.js logosunu gösterir](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | NodeJS Web API (hizmet) [ve pasaport-azure-reklam adına kullanarak](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions) |

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Grafik örnekleri

Azure AD ile kimlik doğrulama dahil olmak üzere Microsoft Graph API için farklı kullanım desenleri gösteren [örnekler](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve öğreticiler hakkında bilgi edinmek [için, Microsoft Graph Community örnekleri & öğreticilere](https://github.com/microsoftgraph/msgraph-community-samples)bakın.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory (v1.0) geliştirici kılavuzu](../azuread-dev/v1-overview.md)
- [Microsoft Graph API kavramsal ve başvuru](https://docs.microsoft.com/graph/use-the-api?context=graph%2Fapi%2Fbeta&view=graph-rest-beta)
