---
title: Microsoft Identity platform için kimlik doğrulama senaryoları | Mavisi
description: Microsoft Identity platform için kimlik doğrulama akışları ve uygulama senaryoları hakkında bilgi edinin. Kimliklerin kimliğini doğrulayabilecek, belirteçleri elde eden ve korunan API 'Leri çağıran farklı uygulama türleri hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/25/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fb2e3e45da0a072eadb0eac9f8a0266f9e14cda2
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69031965"
---
# <a name="authentication-flows-and-application-scenarios"></a>Kimlik doğrulama akışları ve uygulama senaryoları

Microsoft Identity platform (v 2.0) uç noktası, çeşitli modern uygulama mimarilerinin kimlik doğrulamasını destekler. Bu, endüstri standardı protokoller [OAuth 2,0 veya OpenID Connect](active-directory-v2-protocols.md)' i temel alır.  [Kimlik doğrulama kitaplıklarını](reference-v2-libraries.md)kullanarak, uygulamalar kimliklerin kimliğini doğrular ve korunan API 'lere erişmek için belirteçleri elde edin. Bu makalede farklı kimlik doğrulama akışları ve içinde kullanıldıkları uygulama senaryoları açıklanmaktadır.  Bu makalede ayrıca [uygulama senaryoları ve desteklenen kimlik doğrulama akışları](#scenarios-and-supported-authentication-flows) ile [uygulama senaryoları ve desteklenen platformlar ve diller](#scenarios-and-supported-platforms-and-languages)listelenmiştir.

## <a name="application-categories"></a>Uygulama kategorileri

Belirteçleri bir dizi uygulama türünden elde edilebilir: Web uygulamaları, mobil veya masaüstü uygulamaları, Web API 'Leri ve tarayıcıya (veya IoT) sahip olmayan cihazlarda çalışan uygulama. Uygulamalar aşağıdaki şekilde kategorilere ayrılmıştır:

- [Korumalı kaynaklar vs istemci uygulamaları](#protected-resources-vs-client-applications). Bazı senaryolar, kaynakları koruma (Web Apps veya Web API 'Leri) ve diğerleri, korunan bir Web API 'sini çağırmak için bir güvenlik belirteci alma hakkında bilgi edinmektir.
- [Kullanıcılar veya kullanıcılar olmadan](#with-users-or-without-users). Bazı senaryolar, oturum açmış bir Kullanıcı (diğer bir deyişle, bir kullanıcı dahil değil) içerir.
- [Tek sayfalı uygulamalar, genel istemci uygulamaları ve gizli istemci uygulamaları](#single-page-applications-public-client-applications-and-confidential-client-applications). Bunlar, uygulama türlerinin üç büyük kategorileridir. Bunları işlemek için kullanılan kitaplıklar ve nesneler farklı olacaktır.
- [Oturum açma kiti](v2-supported-account-types.md#certain-authentication-flows-dont-support-all-the-account-types). Bazı kimlik doğrulama akışları bazı oturum açma izleyicileri için kullanılamaz. Bazı akışlar yalnızca Iş veya okul hesapları için kullanılabilir ve bazıları Iş veya okul hesapları ve kişisel Microsoft hesapları için kullanılabilir. İzin verilen hedef kitle, kimlik doğrulama akışlarına bağlıdır.
- [Desteklenen OAuth 2,0 akışları](#scenarios-and-supported-authentication-flows).  Kimlik doğrulama akışları, belirteç isteyen uygulama senaryolarını uygulamak için kullanılır.  Uygulama senaryoları ve kimlik doğrulama akışları arasında bire bir eşleme yoktur.
- [Desteklenen platformlar](#scenarios-and-supported-platforms-and-languages). Tüm uygulama senaryoları her platform için kullanılamaz.

### <a name="protected-resources-vs-client-applications"></a>Korumalı kaynaklar vs istemci uygulamaları

Kimlik doğrulama senaryoları iki etkinlik içerir:

- Korumalı bir Web API 'SI için **güvenlik belirteçleri alınıyor** . Microsoft, özel Microsoft kimlik doğrulama kitaplıkları ailesi (MSAL) içinde belirteçleri almak için [kimlik doğrulama kitaplıklarını](reference-v2-libraries.md#microsoft-supported-client-libraries) kullanmanızı önerir
- **Web API 'Sini koruma** (veya bir Web uygulaması). Bir kaynağı korumanın güçlüklerinden biri (Web uygulaması veya Web API 'SI), güvenlik belirtecini doğrulamaktır. Microsoft, bazı platformlarda, [Ara yazılım kitaplıklarında](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries)sunulmaktadır.

### <a name="with-users-or-without-users"></a>Kullanıcılar veya kullanıcılar olmadan

Çoğu kimlik doğrulama senaryosunda bir (oturum açmış) **Kullanıcı**adına belirteç elde edin.

![kullanıcıları olan senaryolar](media/scenarios/scenarios-with-users.svg)

Ancak, uygulamaların kendi adına (Kullanıcı olmadan) belirteç alabilecekleri senaryolar (daemon uygulamaları) de vardır.

![Daemon uygulamaları](media/scenarios/daemon-app.svg)

### <a name="single-page-applications-public-client-applications-and-confidential-client-applications"></a>Tek sayfalı uygulamalar, genel istemci uygulamaları ve gizli istemci uygulamaları

Güvenlik belirteçleri, bir dizi uygulama türünden elde edilebilir. Uygulamalar üç kategoride ayrılmalıdır:

- **Tek sayfalı uygulamalar** (SPA), simgelerin tarayıcıda çalışan uygulamadan alındığı Web uygulamalarının bir biçimidir (JavaScript veya TypeScript 'te yazılmıştır). Birçok modern uygulama, birincil olarak JavaScript 'te yazılmış tek sayfalı bir uygulama ön ucuna sahiptir. Genellikle, uygulama angular, tepki verme veya Vue gibi bir çerçeve kullanılarak yazılır. Tek sayfalı uygulamaları destekleyen tek Microsoft kimlik doğrulama kitaplığı, MSAL. js ' dir.

- **Ortak istemci uygulamaları** her zaman Kullanıcı oturumu açma. Bu uygulamalar şunlardır:
  - Oturum açmış kullanıcı adına Web API 'Lerini çağıran masaüstü uygulamaları.
  - Mobil uygulamalar.
  - Tarayıcıya sahip olmayan cihazlarda çalışan, üçüncü bir uygulama kategorisi (örneğin, uygulama için IoT üzerinde çalışan).

  Bunlar, [Publicclientapplication](msal-client-applications.md)adlı msal sınıfı tarafından temsil edilir.

- **Gizli istemci uygulamaları**
  - Web API 'sini çağıran Web uygulamaları
  - Web API 'sini çağıran Web API 'Leri
  - Daemon uygulamaları (Linux üzerinde bir arka plan programı gibi bir konsol hizmeti olarak uygulandığında bile veya bir Windows hizmeti)
 
  Bu tür uygulamalar [ConfidentialClientApplication](msal-client-applications.md) kullanır

## <a name="application-scenarios"></a>Uygulama senaryoları

Microsoft Identity platform uç noktası çeşitli uygulama mimarilerinde kimlik doğrulamasını destekler: tek sayfalı uygulamalar, Web uygulamaları, Web API 'Leri, mobil ve yerel uygulamalar, Daemon 'ları ve sunucu tarafı uygulamalar.  Uygulamalar, kullanıcıların oturumunu açmak ve korunan API 'Leri çağırmak için belirteçleri almak üzere çeşitli kimlik doğrulama akışlarını kullanır.

### <a name="single-page-application"></a>Tek sayfalı uygulama

Birçok modern web uygulaması, JavaScript veya angular, Vue. js ve tepki verme. js gibi bir SPA çerçevesi kullanılarak yazılmış istemci tarafı tek sayfalı uygulamalar olarak oluşturulmuştur. Bu uygulamalar bir Web tarayıcısında çalışır ve geleneksel sunucu tarafı Web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir. Microsoft Identity platformu, tek sayfalı uygulamaların kullanıcılara oturum açmasını ve arka uç hizmetlerine veya Web API 'Lerine erişim belirteçleri almasını sağlar.

![Tek sayfalı uygulama](media/scenarios/spa-app.svg)

Daha fazla bilgi için [tek sayfalı uygulamaları](scenario-spa-overview.md)okuyun.

### <a name="web-application-signing-in-a-user"></a>Web uygulaması oturum açma-bir Kullanıcı

![Kullanıcılarda Web uygulaması işaretleri](media/scenarios/scenario-webapp-signs-in-users.svg)

**Bir Web uygulamasını** (Kullanıcı oturumu açmak) korumak için şunu kullanacaksınız:

- .NET dünyasında, ASP.NET veya ASP.NET Core Open ID Connect ara yazılım ile birlikte. Aynı şekilde, bir kaynağın korunması, MSAL kitaplıklarını değil [.NET kitaplığı Için IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) tarafından gerçekleştirilen güvenlik belirtecini doğrulamaya yöneliktir

- Node. js ' de geliştirirseniz, Passport. js ' yi kullanacaksınız.

Daha fazla bilgi için [kullanıcıları oturum açan Web uygulamasını](scenario-web-app-sign-user-overview.md)okuyun.

### <a name="web-application-signing-in-a-user-and-calling-a-web-api-on-behalf-of-the-user"></a>Web uygulaması oturumu açma ve Kullanıcı adına bir Web API 'SI çağırma

![Web uygulaması, Web API 'Lerini çağırır](media/scenarios/web-app.svg)

Web uygulamasından Kullanıcı adına **Web API 'sini çağırmak** için msal `ConfidentialClientApplication`kullanın. Alınan belirteci belirteç önbelleğinde depolayarak yetkilendirme kodu akışını kullanacaksınız. Daha sonra denetleyici gerektiğinde belirteçleri sessizce önbellekten elde eder. MSAL, gerekirse belirteci yeniler.

Daha fazla bilgi için Web [uygulaması çağrıları Web API 'lerini](scenario-web-app-call-api-overview.md)okuyun.

### <a name="desktop-application-calling-a-web-api-on-behalf-of-the-signed-in-user"></a>Oturum açmış kullanıcı adına bir Web API 'SI çağıran masaüstü uygulaması

Kullanıcılar tarafından oturum açan bir masaüstü uygulamasından bir Web API 'SI çağırmak için, MSAL 'in Publicclientapplication'un etkileşimli belirteç alma yöntemlerini kullanın. Bu etkileşimli Yöntemler oturum açma kullanıcı arabirimi deneyimini denetlemenizi sağlar. Bu etkileşimi etkinleştirmek için MSAL bir Web tarayıcısından yararlanır.

![Masaüstü](media/scenarios/desktop-app.svg)

Bir Windows etki alanına veya AAD 'ye katılmış bilgisayarlarda çalışan Windows barındırılan uygulamalar için başka bir olasılık vardır. Bu uygulamalar, [Tümleşik Windows kimlik doğrulaması](https://aka.ms/msal-net-iwa)kullanarak sessizce bir belirteç alabilir.

Tarayıcı olmadan bir cihazda çalışan uygulamalar, bir kullanıcı adına bir API 'yi çağırabilecektir. Kimlik doğrulamak için, kullanıcının bir Web tarayıcısına sahip olan başka bir cihazda oturum açması gerekecektir. Bu senaryoyu etkinleştirmek için [cihaz kod akışını](https://aka.ms/msal-net-device-code-flow) kullanmanız gerekir

![Cihaz kod akışı](media/scenarios/device-code-flow-app.svg)

Son olarak, bu önerilmese de, genel istemci uygulamalarında [Kullanıcı adı/parola](https://aka.ms/msal-net-up) kullanabilirsiniz. Bu akış, bazı senaryolarda (DevOps gibi) hala gereklidir, ancak bunu kullanmanın uygulamanıza kısıtlamalar getirdiğinden emin olun. Örneğin, bu akışı kullanan uygulamalar Multi-Factor Authentication (koşullu erişim) gerçekleştirmesi gereken bir kullanıcıya oturum açamaz. Uygulamanızın çoklu oturum açma özelliğinden faydalanabilir. Kullanıcı adı/parola ile kimlik doğrulaması, modern kimlik doğrulamasının ilkelerine karşı gider ve yalnızca eski nedenlerle sağlanır.

Masaüstü uygulamalarında, belirteç önbelleğinin kalıcı olmasını istiyorsanız [belirteç önbelleği serileştirmesini özelleştirmeniz](https://aka.ms/msal-net-token-cache-serialization)gerekir. [Çift belirteç önbelleği serileştirmesini](https://aka.ms/msal-net-dual-cache-serialization)uygulayarak önceki nesil kimlik doğrulama kitaplıkları (ADAL.NET 3. x ve 4. x) ile uyumlu belirteç önbelleklerini geri ve ileri sarabilirsiniz.

Daha fazla bilgi için, [Web API 'lerini çağıran masaüstü uygulamasını](scenario-desktop-overview.md)okuyun.

### <a name="mobile-application-calling-a-web-api-on-behalf-of-the-user-whos-signed-in-interactively"></a>Etkileşimli olarak oturum açan kullanıcı adına bir Web API 'SI çağıran mobil uygulama

Masaüstü uygulamalarına benzer şekilde, bir mobil uygulama, Web API 'sini çağırmak için bir belirteç almak üzere MSAL 'in Publicclientapplication'un etkileşimli belirteç alma yöntemlerini kullanır.

![Mobil](media/scenarios/mobile-app.svg)

MSAL iOS ve MSAL Android, varsayılan olarak sistem Web tarayıcısını kullanır. Ancak, bunu katıştırılmış Web görünümünü kullanmak için de yönlendirebilirsiniz. Mobil platforma bağlı olarak şu şekilde bir farklılık vardır: (UWP, iOS, Android).

Cihaz KIMLIĞIYLE ilgili koşullu erişimi veya Kaydolmakta olan bir cihazı içeren bazı senaryolar bir cihaza bir [Aracı](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/leveraging-brokers-on-Android-and-iOS) yüklenmesini gerektirir. Aracılar örnekleri, Microsoft şirket portalı (Android 'de), Microsoft Authenticator (Android ve iOS). MSAL artık aracılar ile etkileşim kurabiliyor.

> [!NOTE]
> Mobil uygulamanız (MSAL. iOS, MSAL kullanarak. Android veya MSAL.NET/Xamarin), uygulama koruma ilkelerinin uygulanmasını sağlayabilir (örneğin, kullanıcının bazı korumalı metinleri kopyalamasını önler). Bu, [Intune tarafından yönetilir](https://docs.microsoft.com/intune/app-sdk) ve Intune tarafından yönetilen bir uygulama olarak tanınır. [Intune SDK 'Sı](https://docs.microsoft.com/intune/app-sdk-get-started) msal kitaplıklarından ayrıdır ve kendı kendine AAD ile iletişim kuran.

Daha fazla bilgi için, [Web API 'lerini çağıran mobil uygulamayı](scenario-mobile-overview.md)okuyun.

### <a name="protected-web-api"></a>Korumalı Web API 'SI

Uygulamanızın daha fazla Web API 'SI gibi Web hizmetlerini güvenli hale getirmek için Microsoft Identity platform uç noktasını kullanabilirsiniz. Korumalı bir Web API 'SI, verilerini güvenli hale getirmek ve gelen isteklerin kimliğini doğrulamak için bir erişim belirteciyle çağırılır. Bir Web API 'SI çağıran, bir HTTP isteğinin yetkilendirme üstbilgisine bir erişim belirteci ekler. ASP.NET veya ASP.NET Core Web API 'nizi korumak istiyorsanız, erişim belirtecini doğrulamanız gerekir. Bunun için ASP.NET JWT ara yazılımını kullanacaksınız. Bu işlem, MSAL.NET değil, [.NET kitaplığı Için IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) tarafından yapılır.

Daha fazla bilgi için, [korumalı Web API 'sini](scenario-protected-web-api-overview.md)okuyun.

### <a name="web-api-calling-another-downstream-web-api-on-behalf-of-the-user-for-whom-it-was-called"></a>Web API 'SI, çağrıldığı Kullanıcı adına başka bir aşağı akış Web API 'SI çağırma

Ayrıca, ASP.NET veya ASP.NET Core Protected Web API 'nizin Kullanıcı adına başka bir Web API 'sini çağırmasını istiyorsanız, uygulamanın, ConfidentialClientApplication 's metodunu kullanarak aşağı akış Web API 'SI için bir belirteç edinmesi gerekir. [ Kullanıcı](https://aka.ms/msal-net-on-behalf-of). Bu, hizmet çağrıları için de hizmet olarak adlandırılır.
Diğer Web API 'sini çağıran Web API 'Lerinin Ayrıca özel bir önbellek serileştirmesi sağlaması gerekir

  ![Web API'si](media/scenarios/web-api.svg)

Daha fazla bilgi için [Web API 'lerini çağıran Web API 'sini](scenario-web-api-call-api-overview.md)okuyun.

### <a name="desktopservice-or-web-daemon-application-calling-web-api-without-a-user-in-its-own-name"></a>Web API 'sini Kullanıcı olmadan çağıran masaüstü/hizmet veya Web Daemon uygulaması (kendi adında)

Uzun süre çalışan işlemlere sahip olan veya Kullanıcı etkileşimi olmadan çalışan uygulamalarda güvenli Web API 'Lerine erişmek için bir yol gerekir. Bu uygulamalar, kullanıcının temsilci kimliği yerine uygulamanın kimliğini kullanarak kimlik doğrulaması yapabilir ve belirteçleri alabilir. Bir istemci gizli dizisi veya sertifikası kullanarak kimliklerini kanıtlarlar.
Bu tür uygulamalar (daemon uygulaması), MSAL 'in ConfidentialClientApplication's [istemci kimlik bilgileri](https://aka.ms/msal-net-client-credentials) alma yöntemlerini kullanarak en üstteki uygulama için bir belirteç alıyor. Bu, uygulamanın daha önce bu çağrıyla paylaştığı bir gizli dizi (uygulama parolası veya sertifika veya istemci onaylama) ile Azure AD 'ye kaydolduğunu varsayalım.

![Daemon uygulaması](media/scenarios/daemon-app.svg)

Daha fazla bilgi için, [Web API 'lerini çağıran Daemon uygulamasını](scenario-daemon-overview.md)okuyun.

## <a name="scenarios-and-supported-authentication-flows"></a>Senaryolar ve desteklenen kimlik doğrulama akışları

Belirteçleri alma ile ilgili senaryolar Ayrıca [Microsoft Identity platform Protocols](active-directory-v2-protocols.md) ayrıntılarında açıklanan OAuth 2,0 kimlik doğrulama akışlarıyla eşlenir

|Senaryo | Ayrıntılı senaryo izlenecek yol | OAuth 2,0 Flow/ver | Hedef kitle |
|--|--|--|--|
| [![Tek sayfalı uygulama](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | [Tek sayfalı uygulama](scenario-spa-overview.md) | [İndirgen](v2-oauth2-implicit-grant-flow.md) | İş veya okul hesapları ve kişisel hesaplar, B2C
| [![Kullanıcıları oturum açan Web uygulaması](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | [Kullanıcılarda oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md) | [Yetkilendirme kodu](v2-oauth2-auth-code-flow.md) | İş veya okul hesapları ve kişisel hesaplar, B2C |
| [![Web API 'Lerini çağıran Web uygulaması](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | [Web API 'Lerini çağıran Web uygulaması](scenario-web-app-call-api-overview.md) | [Yetkilendirme kodu](v2-oauth2-auth-code-flow.md) | İş veya okul hesapları ve kişisel hesaplar, B2C |
| [![Web API 'Lerini çağıran masaüstü uygulaması](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md) | [Web API'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md)| Etkileşimli (PKI CE ile[yetkilendirme kodu](v2-oauth2-auth-code-flow.md) ) | İş veya okul hesapları ve kişisel hesaplar, B2C |
| | | Tümleşik Windows | İş veya okul hesapları |
| | | [Kaynak sahibi parolası](v2-oauth-ropc.md)  | İş veya okul hesapları, B2C |
| ![Cihaz kod akışı](media/scenarios/device-code-flow-app.svg)| [Web API'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md) | [Cihaz kodu](v2-oauth2-device-code.md)  | İş veya okul hesapları * |
| [![Web API 'Lerini çağıran mobil uygulama](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | [Web API 'Lerini çağıran mobil uygulama](scenario-mobile-overview.md) | Etkileşimli (PKI CE ile[yetkilendirme kodu](v2-oauth2-auth-code-flow.md) )  |   İş veya okul hesapları ve kişisel hesaplar, B2C
| | | Kaynak sahibi parolası  | İş veya okul hesapları, B2C |
| [![Daemon uygulaması](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | [Daemon uygulaması](scenario-daemon-overview.md) | [İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md)  |   Yalnızca uygulama izinleri (Kullanıcı yok) yalnızca AAD kuruluşlarında
| [![Web API 'Lerini çağıran Web API 'SI](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | [Web API 'Lerini çağıran Web API 'SI](scenario-web-api-call-api-overview.md)| [Adına](v2-oauth2-on-behalf-of-flow.md) | İş veya okul hesapları ve kişisel hesaplar |

## <a name="scenarios-and-supported-platforms-and-languages"></a>Senaryolar ve desteklenen platformlar ve diller

Her platformda her uygulama türü kullanılamaz. Uygulamalarınızı oluşturmak için çeşitli diller de kullanabilirsiniz. Microsoft kimlik doğrulama kitaplıkları birçok **platformu** destekler (JavaScript, .NET Framework, .NET Core, Windows 10/UWP, Xamarin. IOS, Xamarin. Android, Yerel iOS, yerel Android, Java, Python)

|Senaryo  | Windows | Linux | Mac | iOS | Android
|--|--|--|--|--|--|--|
| [Tek sayfalı uygulama](scenario-spa-overview.md) <br/>[![Tek sayfalı uygulama](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/logo_js.png) MSAL.js
| [Kullanıcılarda oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md) <br/>[![Kullanıcıları oturum açan Web uygulaması](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | ![ASP.NET](media/sample-v2-code/logo_NET.png)</br> ASP.NET ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core
| [Web API 'Lerini çağıran Web uygulaması](scenario-web-app-call-api-overview.md) <br/> [![Web API 'Lerini çağıran Web uygulaması](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | ![ASP.NET](media/sample-v2-code/logo_NET.png) </br> ASP.NET + MSAL.NET </br> ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python| ![ASP.NET Core](media/sample-v2-code/logo_NETcore.png)ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) Flask + MSAL Python
| [Web API'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md) <br/> Web API 'leri![cihaz kod akışını [çağıran masaüstü uygulaması ![](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md)](media/scenarios/device-code-flow-app.svg) | ![MSAL.NET](media/sample-v2-code/logo_NET.png)  MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python
| [Web API 'Lerini çağıran mobil uygulama](scenario-mobile-overview.md) <br/> [![Web API 'Lerini çağıran mobil uygulama](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | ![UWP](media/sample-v2-code/logo_windows.png) MSAL.NET ![Xamarin](media/sample-v2-code/logo_xamarin.png) MSAL.NET | | | ![iOS/amaç C veya Swift](media/sample-v2-code/logo_iOS.png) MSAL. iOS | ![Android](media/sample-v2-code/logo_Android.png) MSAL. Android
| [Daemon uygulaması](scenario-daemon-overview.md) <br/> [![Daemon uygulaması](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | ![.NET](media/sample-v2-code/logo_NET.png) MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python
| [Web API 'Lerini çağıran Web API 'SI](scenario-web-api-call-api-overview.md) <br/> [![Web API 'Lerini çağıran Web API 'SI](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | ![.NET](media/sample-v2-code/logo_NET.png) MSAL.NET ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python| ![.NET Core](media/sample-v2-code/logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/logo_java.png) msal4j ![MSAL Python](media/sample-v2-code/logo_python.png) MSAL Python

Ayrıca bkz. [OS/Language Ile Microsoft tarafından desteklenen kitaplıklar](reference-v2-libraries.md#microsoft-supported-libraries-by-os--language)

## <a name="next-steps"></a>Sonraki adımlar
[Kimlik doğrulama temelleri](authentication-scenarios.md) ve [erişim belirteçleri](access-tokens.md)hakkında daha fazla bilgi edinin.
