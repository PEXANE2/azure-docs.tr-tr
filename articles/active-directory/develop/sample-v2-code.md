---
title: Microsoft Identity platform için kod örnekleri
description: Senaryoya göre düzenlenmiş kullanılabilir Microsoft Identity platform (v 2.0 uç noktası) kod örneklerinin bir dizinini sağlar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 713fa09319f1b95fe2bcc0d15c973a2096a250ec
ms.sourcegitcommit: c8a0fbfa74ef7d1fd4d5b2f88521c5b619eb25f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801170"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Microsoft Identity platform kodu örnekleri (v 2.0 uç noktası)

Microsoft Identity platformunu kullanarak şunları yapabilirsiniz:

- Web uygulamalarınıza ve Web API 'Lerine kimlik doğrulaması ve yetkilendirme ekleyin.
- Korumalı bir Web API 'sine erişmek için erişim belirteci gerektir.

Bu makalede kısaca Microsoft Identity platform uç noktası için örneklere bağlantılar sunulmaktadır. Bu örnekler, bunun nasıl yapıldığını gösterir ve uygulamalarınızda kullanabileceğiniz kod parçacıklarını da sağlar. Kod örneği sayfasında gereksinimler, yükleme ve Kuruluma yardımcı olacak ayrıntılı Benioku konuları bulacaksınız. Kod içindeki açıklamalar kritik bölümleri anlamanıza yardımcı olur.

> [!NOTE]
> V 1.0 örnekleri ile ilgileniyorsanız, bkz. [Azure AD kod örnekleri (v 1.0 uç noktası)](../azuread-dev/sample-v1-code.md).

Her örnek tür için temel senaryoyu anlamak üzere bkz. [Microsoft Identity platform uç noktası Için uygulama türleri](v2-app-types.md).

GitHub 'daki örneklere da katkıda bulunabilirsiniz. Nasıl yapılacağını öğrenmek için bkz. [Microsoft Azure Active Directory örnekleri ve belgeleri](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Tek sayfalı uygulamalar

Bu örnekler, Microsoft Identity platform ile güvenliği sağlanmış tek sayfalı bir uygulamanın nasıl yazılacağını gösterir. Bu örnekler MSAL. js ' nin türlerini kullanır.

| Platform | Açıklama | Bağlantı |
| -------- | --------------------- | -------- |
| ![Bu resimde JavaScript logosu](media/sample-v2-code/logo_js.png) [JavaScript (msal. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterilmektedir | Çağrılar Microsoft Graph |[JavaScript-graphapı-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2) |
| ![Bu resimde JavaScript logosu](media/sample-v2-code/logo_js.png) [JavaScript (msal. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterilmektedir | Kimlik doğrulama kod akışı kullanan Microsoft Graph çağırır g/PKCE |[JavaScript-v2](https://github.com/Azure-Samples/ms-identity-javascript-v2) |
| ![Bu resimde JavaScript logosu](media/sample-v2-code/logo_js.png) [JavaScript (msal. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterilmektedir | B2C çağırır |[B2C-JavaScript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![Bu görüntüde angular logosu](media/sample-v2-code/logo_angular.png) [JavaScript (msal angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) gösterilmektedir| Kendi Web API 'sini çağırır | [MS-Identity-JavaScript-angular-Spa-aspnetcore-WebApi](https://github.com/Azure-Samples/ms-identity-javascript-angular-spa-aspnetcore-webapi) |
| ![Bu görüntüde angular logosu](media/sample-v2-code/logo_angular.png) [JavaScript (msal angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) gösterilmektedir| Çağrılar Microsoft Graph  | [Active-Directory-JavaScript-singlepageapp-angular](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-angular) |
| ![Bu görüntü, yanıt verme logosu](media/sample-v2-code/logo_react.png) [JavaScript 'i (msal. js)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) gösterir| ' De çağıran Web API 'sini çağırır Microsoft Graph  | [MS-Identity-JavaScript-tepki-Spa-dotnetcore-WebApi-OBO](https://github.com/Azure-Samples/ms-identity-javascript-react-spa-dotnetcore-webapi-obo) |
| ![Bu görüntüde angular logosu](media/sample-v2-code/logo_angular.png) [JavaScript (msal angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular) gösterilmektedir | B2C çağırır |[Active-Directory-B2C-JavaScript-angular-Spa](https://github.com/Azure-Samples/active-directory-b2c-javascript-angular-spa) |

## <a name="web-applications"></a>Web uygulamaları

Aşağıdaki örneklerde, kullanıcıların oturum açmasını sağlayan Web uygulamaları gösterilmektedir. Bazı örnekler ayrıca Microsoft Graph çağıran uygulamayı ya da kullanıcının kimliğiyle kendi Web API 'nizi gösterir.

| Platform | Yalnızca kullanıcılar oturum açar | Kullanıcılar ve çağrılar Microsoft Graph imzalar |
| -------- | ------------------- | --------------------------------- |
| ![Bu görüntüde ASP.NET Core logosu gösterilmektedir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdeği | [ASP.NET Core WebApp oturum açan kullanıcılar öğreticisi](https://aka.ms/aspnetcore-webapp-sign-in) | [ASP.NET Core Web uygulaması çağrılarındaki](https://aka.ms/aspnetcore-webapp-call-msgraph) aynı örnek Microsoft Graph aşaması |
| ![Bu görüntüde ASP.NET logosu gösterilmektedir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [ASP.NET hızlı başlangıç](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [DotNet-WebApp-openıdconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [DotNet-Yönetici-kısıtlı-kapsamlar-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[MSGraph-eğitim-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png)  |                   | [MS-Identity-Java-WebApp](https://github.com/Azure-Samples/ms-identity-java-webapp) |
| ![Bu görüntüde Python logosu gösterilmektedir](media/sample-v2-code/logo_python.png)  |                   | [MS-Identity-Python-WebApp](https://github.com/Azure-Samples/ms-identity-python-webapp) |
| ![Bu görüntüde Ruby logosu gösterilmektedir](media/sample-v2-code/logo_ruby.png) |                   | [MSGraph-eğitim-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Masaüstü ve mobil ortak istemci uygulamaları

Aşağıdaki örneklerde, Microsoft Graph API 'sine veya bir kullanıcı adına kendi Web API 'sine erişen ortak istemci uygulamaları (masaüstü veya mobil uygulamalar) gösterilmektedir. Tüm bu istemci uygulamaları Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanır.

| İstemci uygulaması | Platform | Flow/ver | Çağrılar Microsoft Graph | ASP.NET Core Web API 'sini çağırır |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Masaüstü (WPF)      | ![Bu görüntüde .NET/C# logosu gösterilmektedir](media/sample-v2-code/logo_NET.png) | [etkileşimli](msal-authentication-flows.md#interactive)| [DotNet-masaüstü-MSGraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [DotNet-Native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Masaüstü (konsol)   | ![Bu görüntüde .NET/C# (Masaüstü) logosu gösterilir](media/sample-v2-code/logo_NET.png) | [Tümleşik Windows Kimlik Doğrulaması](msal-authentication-flows.md#integrated-windows-authentication) | [DotNet-IWA-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Masaüstü (konsol)   | ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png) | [Tümleşik Windows Kimlik Doğrulaması](msal-authentication-flows.md#integrated-windows-authentication) |[MS-Identity-Java-masaüstü](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Masaüstü (konsol)   | ![Bu görüntüde .NET/C# (Masaüstü) logosu gösterilir](media/sample-v2-code/logo_NETcore.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| WAM ile masaüstü (konsol)  | ![Bu görüntüde .NET/C# (Masaüstü) logosu gösterilir](media/sample-v2-code/logo_NETcore.png) | [WAM ile etkileşimli](msal-authentication-flows.md#interactive) |[DotNet-yerel-UWP-WAM](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  |
| Masaüstü (konsol)   | ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[MS-Identity-Java-masaüstü](https://github.com/Azure-Samples/ms-identity-java-desktop/) |  |
| Masaüstü (konsol)   | ![Bu görüntüde Python logosu gösterilmektedir](media/sample-v2-code/logo_python.png) | [Kullanıcı Adı/Parola](msal-authentication-flows.md#usernamepassword) |[MS-Identity-Python-Masaüstü](https://github.com/Azure-Samples/ms-identity-python-desktop) |  |
| Mobil (Android, iOS, UWP)   | ![Bu görüntüde .NET/C# (Xamarin) logosu gösterilir](media/sample-v2-code/logo_xamarin.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[Xamarin-Native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobil (iOS)       | ![Bu görüntüde iOS/amaç-C veya Swift gösterilmektedir](media/sample-v2-code/logo_iOS.png) | [etkileşimli](msal-authentication-flows.md#interactive) |[iOS-Swift-ObjC-Native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [iOS-Native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Masaüstü (macOS)       | Mac OS | [etkileşimli](msal-authentication-flows.md#interactive) |[macOS-Swift-ObjC-Native-v2](https://github.com/Azure-Samples/ms-identity-macOS-swift-objc) |  |
| Mobil (Android-Java)   | ![Bu resimde Android logosu gösterilir](media/sample-v2-code/logo_Android.png) | [etkileşimli](msal-authentication-flows.md#interactive) |  [Android-Java](https://github.com/Azure-Samples/ms-identity-android-java) |  |
| Mobil (Android-Kotlin)   | ![Bu resimde Android logosu gösterilir](media/sample-v2-code/logo_Android.png) | [etkileşimli](msal-authentication-flows.md#interactive) |  [Android-Kotlin](https://github.com/Azure-Samples/ms-identity-android-kotlin) |  |

## <a name="daemon-applications"></a>Daemon uygulamaları

Aşağıdaki örneklerde, Microsoft Graph API 'sine kendi kimliğiyle (Kullanıcı olmadan) erişen bir uygulama gösterilmektedir.

| İstemci uygulaması | Platform | Flow/ver | Çağrılar Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Konsol | ![Bu görüntüde .NET Core logosu gösterilmektedir](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [dotnetcore-Daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Web uygulaması | ![Bu görüntüde ASP.NET logosu gösterilmektedir](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [DotNet-Daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |
| Konsol | ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png) | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [MS-Identity-Java-Daemon](https://github.com/Azure-Samples/ms-identity-java-daemon) |
| Konsol | ![Bu görüntüde Python logosu gösterilmektedir](media/sample-v2-code/logo_python.png) | [İstemci kimlik bilgileri](msal-authentication-flows.md#client-credentials) | [MS-Identity-Python-Daemon](https://github.com/Azure-Samples/ms-identity-python-daemon) |

## <a name="headless-applications"></a>Gözetimsiz uygulamalar

Aşağıdaki örnek, bir Web tarayıcısı olmadan cihazda çalışan ortak bir istemci uygulamasını gösterir. Uygulama, bir komut satırı aracı, Linux veya Mac üzerinde çalışan bir uygulama ya da bir IoT uygulaması olabilir. Örnek, başka bir cihazda (cep telefonu gibi) etkileşimli oturum açan bir kullanıcının adında Microsoft Graph API 'sine erişen bir uygulama sunar. Bu istemci uygulaması Microsoft kimlik doğrulama kitaplığı 'nı (MSAL) kullanır.

| İstemci uygulaması | Platform | Flow/ver | Çağrılar Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Masaüstü (konsol)   | ![Bu görüntüde .NET/C# (Masaüstü) logosu gösterilir](media/sample-v2-code/logo_NETcore.png) | [Cihaz kod akışı](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |
| Masaüstü (konsol)   | ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png) | [Cihaz kod akışı](msal-authentication-flows.md#device-code) |[MS-Identity-Java-devicecodeflow](https://github.com/Azure-Samples/ms-identity-java-devicecodeflow) |
| Masaüstü (konsol)   | ![Bu görüntüde Python logosu gösterilmektedir](media/sample-v2-code/logo_python.png) | [Cihaz kod akışı](msal-authentication-flows.md#device-code) |[MS-Identity-Python-devicecodeflow](https://github.com/Azure-Samples/ms-identity-python-devicecodeflow) |

## <a name="web-apis"></a>Web API'leri

Aşağıdaki örneklerde, bir Web API 'sinin Microsoft Identity platform uç noktası ile nasıl korunacağı ve Web API 'sinden bir aşağı akış API 'sinin nasıl çağrılacağını gösterilmektedir.

| Platform | Örnek |
| -------- | ------------------- |
| ![Bu görüntüde ASP.NET Core logosu gösterilmektedir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdeği | [DotNet-Native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph) ASP.NET Core Web API (hizmeti)  |
| ![Bu görüntüde ASP.NET logosu gösterilmektedir](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | [MS-Identity-ASPNET-WebApi-OnBehalfOf](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) Web API (hizmeti) |
| ![Bu görüntüde Java logosu gösterilir](media/sample-v2-code/logo_java.png) | [MS-Identity-Java-WebApi](https://github.com/Azure-Samples/ms-identity-java-webapi) Web API (hizmeti) |
| ![Bu görüntüde Node. js logosu gösterilmektedir](media/sample-v2-code/logo_nodejs.png) | [Active-Directory-JavaScript-NodeJS-WebApi-v2](https://github.com/Azure-Samples/active-directory-javascript-nodejs-webapi-v2) Web API (hizmeti) |
| ![Bu görüntüde Node. js logosu gösterilmektedir](media/sample-v2-code/logo_nodejs.png) | B2C Web API (hizmeti) [Active-Directory-B2C-JavaScript-NodeJS-WebApi](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) |

## <a name="azure-functions-as-web-apis"></a>Web API 'Leri olarak Azure Işlevleri

Aşağıdaki örneklerde HttpTrigger kullanılarak bir Azure Işlevinin nasıl korunacağı ve Microsoft Identity platform uç noktasıyla bir Web API 'sinin nasıl yapılacağı ve Web API 'sinden bir aşağı akış API 'sinin nasıl çağrılacağını gösterilmektedir.

| Platform | Örnek |
| -------- | ------------------- |
| ![Bu görüntüde ASP.NET Core logosu gösterilmektedir](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Çekirdeği | ASP.NET Core Web API (hizmet) Azure Işlevi [DotNet-Native-aspnetcore-v2](https://github.com/Azure-Samples/ms-identity-dotnet-webapi-azurefunctions)  |
| ![Bu görüntüde Node. js logosu gösterilmektedir](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | NodeJS ve Passport Web API (hizmeti) [-Azure-AD](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-azurefunctions) |
| ![Bu görüntüde Python logosu gösterilmektedir](media/sample-v2-code/logo_python.png)</p>Python | [Python](https://github.com/Azure-Samples/ms-identity-python-webapi-azurefunctions) 'un Web API 'si (hizmeti) |
| ![Bu görüntüde Node. js logosu gösterilmektedir](media/sample-v2-code/logo_nodejs.png)</p>NodeJS | NodeJS ve Passport için Web API (hizmeti) [-Azure-AD 'nin adına using](https://github.com/Azure-Samples/ms-identity-nodejs-webapi-onbehalfof-azurefunctions) |

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Graph örnekleri

Azure AD ile kimlik doğrulaması da dahil olmak üzere Microsoft Graph API 'SI için farklı kullanım desenleri gösteren [örnekler](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) ve öğreticiler hakkında bilgi edinmek için bkz. [Microsoft Graph topluluk örnekleri & öğreticiler](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory (v 1.0) Geliştirici Kılavuzu](../azuread-dev/v1-overview.md)
- [Microsoft Graph API kavramsal ve başvuru](https://docs.microsoft.com/graph/use-the-api?context=graph%2Fapi%2Fbeta&view=graph-rest-beta)
