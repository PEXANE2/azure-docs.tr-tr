---
title: Azure Active Directory v1.0 için kod örnekleri | Microsoft Dokümanlar
description: Senaryoya göre düzenlenen Azure Active Directory (v1.0 uç noktası) kod örneklerinin dizinini sağlar.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 07/15/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: ae283529abb3b71ee50fc710dd1ebe0d17a12be0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154840"
---
# <a name="azure-active-directory-code-samples-v10-endpoint"></a>Azure Active Directory kod örnekleri (v1.0 bitiş noktası)

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Web uygulamalarınız ve web API'lerinize kimlik doğrulama ve yetkilendirme eklemek için Microsoft Azure Etkin Dizini'ni (Azure AD) kullanabilirsiniz.

Bu bölümde, Azure AD v1.0 bitiş noktası hakkında daha fazla bilgi edinmek için kullanabileceğiniz örneklere bağlantılar sağlanmaktadır. Bu örnekler, uygulamalarınızda kullanabileceğiniz kod parçacıklarıyla birlikte nasıl yapıldığını gösterir. Kod örneği sayfasında, gereksinimler, yükleme ve kurulumla ilgili yardımcı olan ayrıntılı okuma me konuları bulacaksınız. Ve kod kritik bölümleri anlamanıza yardımcı olmak için yorumlanır.

> [!NOTE]
> Azure AD V2 kod örnekleriyle ilgileniyorsanız, [senaryoya göre v2.0 kod örneklerine](../develop/sample-v2-code.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json)bakın.

Her örnek türü için temel senaryoyu anlamak [için Azure AD için Kimlik Doğrulama senaryolarına](v1-authentication-scenarios.md)bakın.

Ayrıca GitHub'daki örneklerimize de katkıda bulunabilirsiniz. Nasıl yapılacağını öğrenmek için [Microsoft Azure Etkin Dizin örnekleri ne ve belgelerine](https://github.com/Azure-Samples?page=3&query=active-directory)bakın.

## <a name="single-page-applications"></a>Tek sayfauygulamaları

Bu örnek, Azure AD ile güvenli tek sayfalı bir uygulamanın nasıl yazılabildiğini gösterir.

 Platform | Kendi API'sini çağırır | Başka bir Web API'si çağırır
 -------- |  --------------------- | ------------------ 
![Bu resim JavaScript logosunu gösterir](media/sample-v2-code/logo-js.png) | [javascript-singlepageapp](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
![Bu resim Açısal JS logosunu gösterir](media/sample-v2-code/logo-angular.png) | [köşelijler-tek sayfalı uygulama](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | [köşelijler-singlepageapp-kors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="web-applications"></a>Web Uygulamaları

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Kullanıcılarda oturum açma, Microsoft Graph veya kullanıcı kimliğiyle web API araması

Aşağıdaki örnekler, kullanıcıları imzalayan Web uygulamalarını göstermektedir. Bu uygulamalardan bazıları, oturum açmış kullanıcı adına Microsoft Graph'ı veya kendi Web API'nizi de çağırır.

 Platform | Yalnızca kullanıcılarda işaretler | Aramalar Microsoft Graph | Başka bir ASP.NET veya ASP.NET Core 2.0 Web API çağırır
 -------- | ------------------- | --------------------- | -------------------------
![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo-netcore.png)</p>ASP.NET Çekirdek 2.0 | [dotnet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [webapp-webapi-multitenant-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) </p>(AAD Grafiği) | [dotnet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo-netframework.png)</p> ASP.NET 4.5 |  </p> [webapp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) </p> [dotnet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | [dotnet-webapp-multitenant-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect)</p> (AAD Grafiği) |
![Bu resim Python logosunu gösterir](media/sample-v2-code/logo-python.png) | | [python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
![Bu resim Java günlüğünü gösterir](media/sample-v2-code/logo-java.png)  | | [java-webapp-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
![Bu resim PHP logosunu gösterir](media/sample-v2-code/logo-php.png) | | [php-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Rol tabanlı erişim denetimi (yetkilendirme) gösteren web uygulamaları

Aşağıdaki örnekler, rol tabanlı erişim denetiminin (RBAC) nasıl uygulanacağını gösterir. RBAC, bir web uygulamasındaki belirli özelliklerin izinlerini belirli kullanıcılarla sınırlamak için kullanılır. Kullanıcılar, bir **Azure REKLAM grubuna** mı ait olup olmadıklarına veya belirli bir uygulama **rolüne**sahip olup olmadıklarına bağlı olarak yetkilendirilmiştir.

Platform | Örnek |
 -------- | ------------------- |
![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo-netframework.png)</p> ASP.NET 4.5 | [dotnet-webapp-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) </p>  [dotnet-webapp-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Yetkilendirme için Azure AD **rollerini** kullanan bir .NET 4.5 MVC web uygulaması

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Microsoft Graph veya Web API'yi çağıran masaüstü ve mobil kamu istemcisi uygulamaları

Aşağıdaki örnekler, Microsoft Graph'a veya bir kullanıcı adına Web API'sine erişen ortak istemci uygulamalarını (deskto/pmobile uygulamaları) göstermektedir. Cihazlara ve platformlara bağlı olarak, uygulamalar kullanıcılarda farklı şekillerde oturum açabilir (akışlar/hibeler):

- Etkileşimli
- Sessizce (Windows'da Tümleşik Windows Kimlik Doğrulaması veya kullanıcı adı/parola ile)
- Etkileşimli oturum açmayı başka bir aygıta devrederek (web denetimleri sağlamayan cihazlarda kullanılan aygıt kodu akışı)

İstemci başvurusu | Platform | Akış / Hibe | Aramalar Microsoft Graph | ASP.NET veya ASP.NET Core 2.x Web API'yi çağırır
------------------ | -------- | ---------- | -------------------- | -------------------------
Masaüstü (WPF)           | ![Bu resim .NET/C# logosunu gösterir](media/sample-v2-code/logo-net.png)  | Etkileşimli | [dotnet-yerli-çok hedefli](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) bir parçası | [Dotnet-native-desktop](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [dotnet-yerli-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore/)</p> [dotnet-webapi-manuel-jwt-doğrulama](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobil (UWP)            | .![Bu resim .NET/C#/UWP](media/sample-v2-code/logo-windows.png)   | Etkileşimli | [dotnet-yerli-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) </p> Bu örnek [WAM](/windows/uwp/security/web-account-manager)kullanır, [ADAL.NET](https://aka.ms/adalnet) değil|  [dotnet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (Tek bir kiracı Web API aramak için ADAL.NET kullanarak UWP uygulaması) </p> [dotnet-webapi-multitenant-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (Çok kiracılı Web API aramak için ADAL.NET kullanarak UWP uygulaması)|
Mobil (Android, iOS, UWP)   | ![Bu resim .NET/C# (Xamarin)](media/sample-v2-code/logo-xamarin.png) | Etkileşimli | [dotnet-yerli-çok hedef](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Mobil (Android)           | ![Bu resim Android logosunu gösterir](media/sample-v2-code/logo-android.png) | Etkileşimli |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Mobil (iOS)           | ![Bu resim iOS / Objective C veya Swift'i gösterir](media/sample-v2-code/logo-ios.png) | Etkileşimli |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Masaüstü (Konsol)          | ![Bu resim .NET/C# logosunu gösterir](media/sample-v2-code/logo-net.png) | Kullanıcı Adı / Şifre </p>  Tümleşik Windows Kimlik Doğrulaması | | [dotnet-yerli-başsız](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Masaüstü (Konsol)          | ![Bu resim Java logosunu gösterir](media/sample-v2-code/logo-java.png) | Kullanıcı Adı / Şifre | | [java-yerli-headless](https://github.com/Azure-Samples/active-directory-java-native-headless)
Masaüstü (Konsol)           | ![Bu resim .NET Core/C# logosunu gösterir](media/sample-v2-code/logo-netcore.png) | Cihaz kodu akışı | | [dotnet-deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Daemon uygulamaları (uygulamanın kimliği ile web API'lerine erişim)

Aşağıdaki örnekler, Microsoft Graph'a veya kullanıcı olmadan (uygulama kimliğiyle) bir web API'ye erişen masaüstü veya web uygulamalarını gösterir.

İstemci başvurusu | Platform | Akış / Hibe | ASP.NET veya ASP.NET Core 2.0 Web API'yi çağırır
------------------ | -------- | ---------- | -------------------- 
Daemon uygulaması (Konsol)          | ![Bu resim .NET logosunu gösterir](media/sample-v2-code/logo-netframework.png) | Uygulama sırrı veya sertifikaiçeren Istemci Kimlik Bilgileri | [dotnet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [dotnet-daemon-sertifika-kimlik bilgisi](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Daemon uygulaması (Konsol)         | ![Bu resim .NET logosunu gösterir](media/sample-v2-code/logo-netcore.png) | Sertifikalı İstemci Kimlik Bilgileri| [dotnetcore-daemon-sertifika-kimlik bilgisi](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
ASP.NET Web Uygulaması  | ![Bu resim .NET logosunu gösterir](media/sample-v2-code/logo-netframework.png) | İstemci kimlik bilgileri | [dotnet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>Web API'leri

### <a name="web-api-protected-by-azure-active-directory"></a>Azure Etkin Dizin tarafından korunan Web API'si

Aşağıdaki örnek, Azure AD ile bir düğüm.js web API'sını nasıl koruyacağını gösterir.

Bu makalenin önceki bölümlerinde, ASP.NET veya ASP.NET Core **Web API** **çağıran** bir istemci uygulamasını gösteren diğer örnekleri de bulabilirsiniz. Bu örnekler bu bölümde tekrar belirtilmemektedir, ancak bunları yukarıdaki veya aşağıdaki tabloların son sütununda bulabilirsiniz

| Platform | Örnek |
|--------|-------------------|
| ![Bu resim Düğüm.js logosunu gösterir](media/sample-v2-code/logo-nodejs.png)  | [düğüm-webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>Microsoft Graph veya başka bir Web API'si arama web API'si

Aşağıdaki örnekler, başka bir web API çağıran bir web API'sini gösterir. İkinci örnek, Koşullu Erişim'in nasıl işleyeceğini gösterir.

| Platform |  Aramalar Microsoft Graph | Başka bir ASP.NET veya ASP.NET Core 2.0 Web API çağırır |
| -------- |  --------------------- | ------------------------- |
| ![Bu resim ASP.NET logosunu gösterir](media/sample-v2-code/logo-netframework.png)</p> ASP.NET 4.5 | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) | [dotnet-webapi-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) </p> [dotnet-webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |

## <a name="other-microsoft-graph-samples"></a>Diğer Microsoft Grafik örnekleri

Azure AD ile kimlik doğrulama dahil olmak üzere Microsoft Graph API için farklı kullanım desenleri gösteren örnekler ve öğreticiler için [Microsoft Graph Topluluk Örnekleri & Öğreticiler'e](https://github.com/microsoftgraph/msgraph-community-samples)bakın.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory Geliştirici Kılavuzu](v1-overview.md)
- [Azure Etkin Dizin Kimlik Doğrulama kitaplıkları](active-directory-authentication-libraries.md)
- [Microsoft Graph API kavramsal ve başvuru](https://docs.microsoft.com/graph/use-the-api)
