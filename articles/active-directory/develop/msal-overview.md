---
title: MSAL hakkında bilgi edinin | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL), uygulama geliştiricilerin güvenli web API'lerini aramak için belirteçler edinmelerini sağlar. Bu web API'leri Microsoft Graph, diğer Microsoft API'leri, üçüncü taraf web API'leri veya kendi web API'larınız olabilir. MSAL birden çok uygulama mimarisini ve platformlarını destekler.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 89c5117e59f7856c3bd572bbea297a836b5ad589
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536225"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Microsoft Kimlik Doğrulama Kitaplığına (MSAL) Genel Bakış
Microsoft Kimlik Doğrulama Kitaplığı (MSAL), geliştiricilerin güvenli web API'lerine erişmek için Microsoft kimlik platformu bitiş noktasından [belirteçler](developer-glossary.md#security-token) edinmelerine olanak tanır. Bu web API'leri Microsoft Graph, diğer Microsoft API'leri, üçüncü taraf web API'leri veya kendi web API'larınız olabilir. MSAL, birçok farklı uygulama mimarisini ve platformlarını destekleyen .NET, JavaScript, Android ve iOS için kullanılabilir.

MSAL, çeşitli platformlar için tutarlı bir API ile jeton almanın birçok yolunu sunar. MSAL'ı kullanmak aşağıdaki avantajları sağlar:

* OAuth kitaplıklarını veya kodu uygulamanızdaki protokole karşı doğrudan kullanmanıza gerek yoktur.
* Kullanıcı adına veya bir uygulama adına (platforma uygulanabilirse) belirteçler edinir.
* Bir belirteç önbelleği tutar ve süresi dolmak üzere yken sizin için belirteçleri yeniler. Belirteç son kullanma tarihini tek başına halletmene gerek yok.
* Uygulamanızın oturum etmesini istediğiniz kitleyi (org, çeşitli orglar, iş ve okul ve Microsoft kişisel hesaplarınız, Azure AD B2C ile sosyal kimlikler, bağımsız kullanıcılar ve ulusal bulutlar) belirtmenize yardımcı olur.
* Uygulamanızı yapılandırma dosyalarından ayarlamanıza yardımcı olur.
* Uygulanabilir özel durumları, günlüğe kaydetmeve telemetriyi ortaya çıkararak uygulamanızı sorun gidermenize yardımcı olur.

## <a name="application-types-and-scenarios"></a>Uygulama türleri ve senaryolar
MSAL kullanılarak, bir belirteç bir dizi uygulama türünden elde edilebilir: web uygulamaları, web API'leri, tek sayfalı uygulamalar (JavaScript), mobil ve yerel uygulamalar, daemons ve sunucu tarafı uygulamaları.

MSAL aşağıdakiler de dahil olmak üzere birçok uygulama senaryosunda kullanılabilir:

* [Tek sayfauygulamaları (JavaScript)](scenario-spa-overview.md)
* [Kullanıcılarda Web uygulaması imzalama](scenario-web-app-sign-user-overview.md)
* [Web uygulaması bir kullanıcı da imzalama ve kullanıcı adına bir web API arama](scenario-web-app-call-api-overview.md)
* [Yalnızca kimlik doğrulaması yapılan kullanıcıların erişebilmeleri için web API'nin korunması](scenario-protected-web-api-overview.md)
* [Web API oturum açmış kullanıcı adına başka bir downstream web API arama](scenario-web-api-call-api-overview.md)
* [Oturum açmış kullanıcı adına web API çağıran masaüstü uygulaması](scenario-desktop-overview.md)
* [Mobil uygulama interaktif olarak oturum açmış kullanıcı adına bir web API çağıran](scenario-mobile-overview.md).
* [Kendisi adına web API çağıran masaüstü / hizmet daemon uygulaması](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Diller ve çerçeveler

| Kitaplık | Desteklenen platformlar ve çerçeveler|
| --- | --- |
| [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET Framework, .NET Core, Xamarin Android, Xamarin iOS, Evrensel Windows Platformu|
| [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| AngularJS, Ember.js veya Durandal.js gibi JavaScript/TypeScript çerçeveleri|
| [Android için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| [iOS ve macOS için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS ve macOS|
| [MSAL Java (önizleme)](https://github.com/AzureAD/microsoft-authentication-library-for-java)|Java|
| [MSAL Python (önizleme)](https://github.com/AzureAD/microsoft-authentication-library-for-python)|Python|

## <a name="differences-between-adal-and-msal"></a>ADAL ve MSAL arasındaki farklar

Active Directory Authentication Library (ADAL), MSAL'ın Microsoft kimlik platformu (v2.0) bitiş noktasıyla tümleştiği geliştiriciler için Azure AD (v1.0) bitiş noktasıyla tümleşir. v1.0 bitiş noktası çalışma hesaplarını destekler, ancak kişisel hesapları desteklemez. v2.0 bitiş noktası, Microsoft kişisel hesaplarının ve çalışma hesaplarının tek bir kimlik doğrulama sisteminde birleştirilmesidir. Ayrıca, MSAL ile Azure AD B2C için kimlik doğrulamalarını da alabilirsiniz.

Daha ayrıntılı bilgi için, [ADAL.NET MSAL.NET'a göç](msal-net-migration.md) etme ve [ADAL.js'den MSAL.js'ye geçiş](msal-compare-msal-js-and-adal-js.md)hakkında bilgi edinin.
