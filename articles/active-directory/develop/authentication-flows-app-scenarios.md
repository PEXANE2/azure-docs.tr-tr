---
title: Microsoft kimlik platformu kimlik doğrulama & uygulama senaryoları | Azure
description: Kimliklerin doğrulanması, belirteçlerin alınması ve korumalı API'lerin çağrılması da dahil olmak üzere Microsoft kimlik platformuiçin uygulama senaryoları hakkında bilgi edinin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/03/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started
ms.openlocfilehash: e73da5e7b440a216841fffd65ca2e1b95de7a609
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480938"
---
# <a name="authentication-flows-and-application-scenarios"></a>Kimlik doğrulama akışları ve uygulama senaryoları

Microsoft kimlik platformu (v2.0) bitiş noktası, farklı türde modern uygulama mimarileri için kimlik doğrulamayı destekler. Tüm mimariler endüstri standardı protokolleri [OAuth 2.0 ve OpenID Connect](active-directory-v2-protocols.md)dayanmaktadır.  Microsoft [kimlik platformu kimlik doğrulama kitaplıklarını](reference-v2-libraries.md)kullanarak, uygulamalar kimliklerin kimliğini doğrular ve korumalı API'lere erişmek için belirteçleri edinir.

Bu makalede kimlik doğrulama akışları ve kullanıldıkları uygulama senaryoları açıklanmaktadır:

- [Uygulama senaryoları ve desteklenen kimlik doğrulama akışları.](#scenarios-and-supported-authentication-flows)
- [Uygulama senaryoları ve desteklenen platformlar ve diller.](#scenarios-and-supported-platforms-and-languages)

## <a name="application-categories"></a>Uygulama kategorileri

Belirteçler, çeşitli uygulama türlerinden:

- Web uygulamaları
- Mobil uygulamalar
- Masaüstü uygulamaları
- Web API'leri

Belirteçler, tarayıcısı olmayan veya IoT'de çalışan cihazlarda çalışan uygulamalardan da elde edilebilir.

Uygulamalar aşağıdaki listede olduğu gibi kategorize edilebilir:

- [Korumalı kaynaklar ve istemci uygulamaları](#protected-resources-vs-client-applications): Bazı senaryolar web uygulamaları veya web API'leri gibi kaynakları korumakla ilgilidir. Diğer senaryolar, korumalı web API'sını çağırmak için bir güvenlik belirteci edinme yle ilgilidir.
- [Kullanıcılarla veya kullanıcı olmadan](#with-users-or-without-users): Bazı senaryolar oturum açmış bir kullanıcıyı içerir, ancak daemon senaryoları gibi diğerleri kullanıcı içermez.
- [Tek sayfalı, genel istemci ve gizli istemci uygulamaları](#single-page-public-client-and-confidential-client-applications): Bu tür uygulamalar üç büyük kategoridir. Her biri farklı kitaplıklar ve nesnelerle kullanılır.
- [Oturum açma hedef kitlesi](v2-supported-account-types.md#certain-authentication-flows-dont-support-all-the-account-types): Kullanılabilir kimlik doğrulama akışları oturum açma hedef kitlesine bağlı olarak değişir. Bazı akışlar yalnızca iş veya okul hesapları için kullanılabilir. Bazıları hem iş veya okul hesapları hem de kişisel Microsoft hesapları için kullanılabilir. İzin verilen hedef kitle kimlik doğrulama akışlarına bağlıdır.
- [Desteklenen OAuth 2.0 akışları](#scenarios-and-supported-authentication-flows): Kimlik doğrulama akışları belirteçleri isteyen uygulama senaryolarını uygulamak için kullanılır. Uygulama senaryoları ve kimlik doğrulama akışları arasında bire bir eşleme yoktur.
- [Desteklenen platformlar](#scenarios-and-supported-platforms-and-languages): Tüm uygulama senaryoları her platform için kullanılamaz.

### <a name="protected-resources-vs-client-applications"></a>Korumalı kaynaklar ve istemci uygulamaları

Kimlik doğrulama senaryoları iki etkinlik içerir:

- **Korumalı bir web API'si için güvenlik belirteçleri edinme**: Jeton elde etmek için [Microsoft destekli istemci kitaplıklarını](reference-v2-libraries.md#microsoft-supported-client-libraries) kullanmanızı öneririz, özellikle Microsoft Kimlik Doğrulama Kitaplığı (MSAL) ailesi.
- **Bir web API'sini veya bir web uygulamasını koruma**: Bir web API'sini veya web uygulaması kaynağını korumanın zorluklarından biri, güvenlik belirtecinin doğrulanmasıdır. Bazı platformlarda, Microsoft [ara yazılım kitaplıkları](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries)sunar.

### <a name="with-users-or-without-users"></a>Kullanıcılarla veya kullanıcılar olmadan

Kimlik doğrulama senaryolarının çoğu oturum açmış kullanıcılar adına belirteçler edinir.

![Kullanıcılarla senaryolar](media/scenarios/scenarios-with-users.svg)

Ancak, uygulamaların kullanıcı olmadan kendileri adına belirteçler edindiği daemon uygulaması senaryoları da vardır.

![Daemon uygulamaları ile senaryolar](media/scenarios/daemon-app.svg)

### <a name="single-page-public-client-and-confidential-client-applications"></a>Tek sayfa, genel istemci ve gizli istemci uygulamaları

Güvenlik belirteçleri birden çok uygulama türünden elde edilebilir. Bu uygulamalar üç kategoriye ayrılır eğilimindedir:

- **Tek sayfalı uygulamalar**: SPA olarak da bilinen bu uygulamalar, tarayıcıda çalışan bir JavaScript veya TypeScript uygulamasından belirteçlerin elde edildiği web uygulamalarıdır. Birçok modern uygulama öncelikle JavaScript yazılmış bir tek sayfalı uygulama ön ucu var. Uygulama genellikle Açısal, React veya Vue gibi bir çerçeve kullanır. MSAL.js, tek sayfalı uygulamaları destekleyen tek Microsoft kimlik doğrulama kitaplığıdır.

- **Genel istemci uygulamaları**: Bu uygulamalar her zaman kullanıcılarda oturum açtır:
  - Oturum açmış kullanıcı adına web API'lerini çağıran masaüstü uygulamaları
  - Mobil uygulamalar
  - iOT'de çalışan uygulamalar gibi tarayıcısı olmayan cihazlarda çalışan uygulamalar

  Bu uygulamalar MSAL [PublicClientApplication](/dotnet/api/microsoft.identity.client.publicclientapplication) sınıfı tarafından temsil edilir. Daha fazla bilgi için [Bkz. Genel istemci ve gizli istemci uygulamaları.](msal-client-applications.md)

- **Gizli müşteri uygulamaları:**
  - Web API çağıran web uygulamaları
  - Web API'leri web API'si arama
  - Daemon uygulamaları, Linux daemon veya Windows hizmeti gibi bir konsol hizmeti olarak uygulandığında bile

  Bu tür uygulamalar [ConfidentialClientApplication](/dotnet/api/microsoft.identity.client.confidentialclientapplication) sınıfını kullanır. Daha fazla bilgi için [Bkz. Genel istemci ve gizli istemci uygulamaları.](msal-client-applications.md)

## <a name="application-scenarios"></a>Uygulama senaryoları

Microsoft kimlik platformu bitiş noktası, farklı uygulama mimarileri için kimlik doğrulamayı destekler:

- Tek sayfalı uygulamalar
- Web uygulamaları
- Web API'leri
- Mobil uygulamalar
- Yerel uygulamalar
- Daemon uygulamaları
- Sunucu tarafındaki uygulamalar

Uygulamalar, kullanıcıları oturum etmek ve korumalı API'leri aramak için belirteçler almak için farklı kimlik doğrulama akışlarını kullanır.

### <a name="a-single-page-application"></a>Tek sayfalık bir uygulama

Birçok modern web uygulaması istemci tarafı tek sayfalı uygulamalar olarak oluşturulur. JavaScript veya Açısal, Vue.js ve React.js gibi tek sayfalık bir uygulama çerçevesi kullanarak bu uygulamalar. Bu uygulamalar bir web tarayıcısında çalışır.

Kimlik doğrulama özellikleri geleneksel sunucu tarafındaki web uygulamalarından farklıdır. Microsoft kimlik platformlarını kullanarak, tek sayfalı uygulamalar kullanıcıları oturum açabilir ve arka uç hizmetlere veya web API'lerine erişmek için belirteçler alabilir.

![Tek sayfalık bir uygulama](media/scenarios/spa-app.svg)

Daha fazla bilgi için tek [sayfalı uygulamalara](scenario-spa-overview.md)bakın.

### <a name="a-web-app-that-is-signing-in-a-user"></a>Kullanıcıda oturum açan bir web uygulaması

![Kullanıcıda işaretleyen bir web uygulaması](media/scenarios/scenario-webapp-signs-in-users.svg)

Kullanıcıda oturum açan bir web uygulamasını korumak için:

- .NET'te gelişirseniz, Open ID Connect aracını ASP.NET ile ASP.NET veya ASP.NET Core kullanırsınız. Kaynağın korunması, MSAL kitaplıkları için [değil, .NET](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) kitaplığı için IdentityModel uzantıları tarafından yapılan güvenlik belirtecinin doğrulanmasıiçerir.

- Eğer Node.js gelişirseniz, Passport.js kullanın.

Daha fazla bilgi için, [kullanıcılarda işaretleyen Web uygulamasına](scenario-web-app-sign-user-overview.md)bakın.

### <a name="a-web-app-that-signs-in-a-user-and-calling-a-web-api-on-behalf-of-the-user"></a>Kullanıcıda imzalayan ve kullanıcı adına web API'sini çağıran bir web uygulaması

![Web API'lerini çağıran bir web uygulaması](media/scenarios/web-app.svg)

Bir web uygulamasından bir web api'sini kullanıcı adına aramak için MSAL **ConfidentialClientApplication** sınıfını kullanın. Yetkilendirme kodu akışını kullanır ve edinilen belirteçleri belirteç önbelleğinde saklarsınız. Gerektiğinde, MSAL belirteçleri yeniler ve denetleyici sessizce önbellekten belirteçleri elde eder.

Daha fazla bilgi [için, web API'lerini çağıran bir web uygulamasına](scenario-web-app-call-api-overview.md)bakın.

### <a name="a-desktop-app-calling-a-web-api-on-behalf-of-a-signed-in-user"></a>Oturum açmış bir kullanıcı adına web API'sini çağıran bir masaüstü uygulaması

Bir masaüstü uygulamasının kullanıcılarda işaretleyen bir web API'sini araması için MSAL **PublicClientApplication** sınıfının etkileşimli belirteç edinme yöntemlerini kullanın. Bu etkileşimli yöntemlerle oturum açma UI deneyimini denetleyebilirsiniz. MSAL bu etkileşim için bir web tarayıcısı kullanır.

![Web API çağıran bir masaüstü uygulaması](media/scenarios/desktop-app.svg)

Windows etki alanına veya Azure Etkin Dizini 'ne (Azure AD) katılan bilgisayarlardaki Windows tarafından barındırılan uygulamalar için başka bir olasılık daha vardır. Bu uygulamalar sessizce [Entegre Windows Kimlik Doğrulama](https://aka.ms/msal-net-iwa)kullanarak bir belirteç elde edebilirsiniz.

Tarayıcısı olmayan bir aygıtta çalışan uygulamalar, kullanıcı adına API'yi aramaya devam edebilir. Kimlik doğrulaması yapmak için, kullanıcının web tarayıcısı olan başka bir cihazda oturum açması gerekir. Bu senaryo, Aygıt [Kodu Akışını](https://aka.ms/msal-net-device-code-flow)kullanmanız gerektiğini.

![Cihaz Kodu akışı](media/scenarios/device-code-flow-app.svg)

Kullanmanızı önermesek de, Kullanıcı [Adı/Parola akışı](https://aka.ms/msal-net-up) ortak istemci uygulamalarında kullanılabilir. Bu akış hala DevOps gibi bazı senaryolarda gereklidir.

Ancak bu akışı kullanmak uygulamalarınızı kısıtlar. Örneğin, uygulamalar çok faktörlü kimlik doğrulaması veya Koşullu Erişim kullanması gereken bir kullanıcı oturum açamaz. Uygulamalarınız da tek oturum açmadan yararlanamaz.

Kullanıcı adı/Parola akışı ile kimlik doğrulama modern kimlik doğrulama ilkelerine aykırıdır ve yalnızca eski nedenlerle sağlanır.

Masaüstü uygulamalarında, belirteç önbelleğinin devam etmesini istiyorsanız, [Token Önbelleği serileştirmesini](https://aka.ms/msal-net-token-cache-serialization)özelleştirin. [Çift belirteç önbellek serileştirme](https://aka.ms/msal-net-dual-cache-serialization)uygulayarak, geriye dönük uyumlu ve ileriye uyumlu belirteç önbellekleri kullanabilirsiniz. Bu belirteçler, önceki nesil kimlik doğrulama kitaplıklarını destekler. Özel kitaplıklar, .NET (ADAL.NET) sürüm 3 ve sürüm 4 için Azure AD Kimlik Doğrulama Kitaplığı'nı içerir.

Daha fazla bilgi için [web API'lerini çağıran Masaüstü uygulamasına](scenario-desktop-overview.md)bakın.

### <a name="a-mobile-app-calling-a-web-api-on-behalf-of-an-interactive-user"></a>Etkileşimli bir kullanıcı adına web API'sini çağıran bir mobil uygulama

Bir masaüstü uygulamasına benzer şekilde, bir mobil uygulama da MSAL **PublicClientApplication** sınıfının etkileşimli belirteç edinme yöntemlerini arayarak web API'sını aramak için bir belirteç elde etmeyi sağlar.

![Web API'yi çağıran bir mobil uygulama](media/scenarios/mobile-app.svg)

MSAL iOS ve MSAL Android varsayılan olarak sistem web tarayıcısını kullanır. Ancak, bunun yerine katışılmış Web Görünümü'ni kullanmaya yönlendirebilirsiniz. Mobil platforma bağlı özellikler vardır: Evrensel Windows Platformu (UWP), iOS veya Android.

Aygıt kimliği veya aygıt kaydıyla ilgili Koşullu Erişim içeren senaryolar gibi bazı senaryoların aygıta bir aracının yüklenmesini gerektirir. Brokerlere örnek olarak Android'deki Microsoft Company Portal ve Android ve iOS'ta Microsoft Authenticator verilebilir. MSAL artık brokerlarla etkileşimkurabilir. Daha fazla bilgi için Android [ve iOS'taki brokerlardan yararlanın.](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/leveraging-brokers-on-Android-and-iOS)

Daha fazla bilgi için [web API'lerini çağıran Mobil uygulamaya](scenario-mobile-overview.md)bakın.

> [!NOTE]
> MSAL.iOS, MSAL kullanan mobil uygulamanız. Android veya Xamarin'deki MSAL.NET uygulama koruma politikaları uygulanabilir. Örneğin, ilkeler bir kullanıcının korumalı metni kopyalamasını engelleyebilir. Mobil uygulama Intune tarafından yönetilir ve Intune tarafından yönetilen bir uygulama olarak tanınır. Daha fazla bilgi için [Microsoft Intune App SDK genel bakış](https://docs.microsoft.com/intune/app-sdk)bakın.
>
> [Intune App SDK,](https://docs.microsoft.com/intune/app-sdk-get-started) MSAL kitaplıklarından ayrıdır ve Azure AD ile tek başına etkileşimde bulunmaktadır.

### <a name="a-protected-web-api"></a>Korumalı web API'si

Uygulamanızın RESTful web API'si gibi web hizmetlerini güvenli hale getirmek için Microsoft kimlik platformu bitiş noktasını kullanabilirsiniz. Korumalı web API'si, erişim belirteci kullanılarak çağrılır. Belirteç, API'nin verilerini güvence altına almak ve gelen istekleri doğrulamak için kullanılır. Bir web API'nin arayanı arayan, bir HTTP isteğinin yetkilendirme üstbilgisinde bir erişim belirteci ekler.

ASP.NET veya ASP.NET Core Web API'nizi korumak istiyorsanız, erişim jetonunu doğrulamanız gerekir. Bu doğrulama için, ASP.NET JWT ara yazılımını kullanırsınız. Doğrulama ,MSAL.NET tarafından değil, .NET kitaplığı [için IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) tarafından yapılır.

Daha fazla bilgi için [Korumalı web API'si'ne](scenario-protected-web-api-overview.md)bakın.

### <a name="a-web-api-calling-another-web-api-on-behalf-of-a-user"></a>Bir kullanıcı adına başka bir web API çağıran bir web API

ASP.NET veya ASP.NET Core korumalı Web API'nizin bir kullanıcı adına başka bir web API'sını araması için uygulamanızın akış aşağı web API'sı için bir belirteç edinmesi gerekir. Bir belirteç edinmek için uygulamanız **ConfidentialClientApplication** sınıfının [AcquireTokenOnBehalfyöntemini](https://aka.ms/msal-net-on-behalf-of) çağırır. Bu tür aramalar da *hizmet-servis* çağrıları olarak adlandırılır. Diğer web API'lerini arayan web API'lerin özel önbellek serileştirmesağlaması gerekir.

  ![Başka bir web API'si çağıran bir web API'si](media/scenarios/web-api.svg)

Daha fazla bilgi için, [web API'lerini çağıran Web API'si'ne](scenario-web-api-call-api-overview.md)bakın.

### <a name="a-daemon-app-calling-a-web-api-in-the-daemons-name"></a>Daemon adına web API çağıran bir daemon uygulaması

Uzun süreli süreçleri olan veya kullanıcı etkileşimi olmadan çalışan uygulamaların da güvenli web API'lerine erişme nin bir yolunu bulabilmeniz gerekir. Böyle bir uygulama, uygulamanın kimliğini kullanarak kimlik doğrulaması yapabilir ve belirteçleri alabilir. Uygulama, bir istemci sırrı veya sertifika kullanarak kimliğini kanıtlıyor.

MSAL **ConfidentialClientApplication** sınıfının [istemci kimlik bilgileri](https://aka.ms/msal-net-client-credentials) edinme yöntemlerini kullanarak arama uygulaması için bir belirteç edinen bu tür daemon uygulamalarını yazabilirsiniz. Bu yöntemler, arama uygulamasının Azure AD'ye bir sır kaydettiğini gerektirir. Uygulama daha sonra adı daemon ile sırrı paylaşır. Bu tür sırlara örnek olarak uygulama parolaları, sertifika iddiası veya istemci iddiası verilebilir.

![Diğer uygulamalar ve API'ler tarafından çağrılan bir daemon uygulaması](media/scenarios/daemon-app.svg)

Daha fazla bilgi [için, web API'leri çağıran Daemon uygulamasına](scenario-daemon-overview.md)bakın.

## <a name="scenarios-and-supported-authentication-flows"></a>Senaryolar ve desteklenen kimlik doğrulama akışları

Belirteçleri edinme içeren senaryolar da OAuth 2.0 kimlik doğrulama akışları eşler. Daha fazla bilgi için [Microsoft kimlik platformundaki OAuth 2.0 ve OpenID Connect protokollerine](active-directory-v2-protocols.md)bakın.

<table>
 <thead>
  <tr><th>Senaryo</th> <th>Ayrıntılı senaryo walk-through</th> <th>OAuth 2.0 akış ve hibe</th> <th>Hedef kitle</th></tr>
 </thead>
 <tbody>
  <tr>
   <td><a href="scenario-spa-overview.md"><img alt="Single-Page App" src="media/scenarios/spa-app.svg"></a></td>
   <td><a href="scenario-spa-overview.md">Tek sayfalı uygulama</a></td>
   <td><a href="v2-oauth2-implicit-grant-flow.md">Örtülü</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Microsoft Azure Etkin Dizin B2C (Azure AD B2C)</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-sign-user-overview.md"><img alt="Web App that signs in users" src="media/scenarios/scenario-webapp-signs-in-users.svg"></a></td>
   <td><a href="scenario-web-app-sign-user-overview.md">Kullanıcılarda işaretleyen bir web uygulaması</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Yetkilendirme Kodu</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-call-api-overview.md"><img alt="Web App that signs in users" src="media/scenarios/web-app.svg"></a></td>
   <td><a href="scenario-web-app-call-api-overview.md">Web API'lerini çağıran bir web uygulaması</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Yetkilendirme Kodu</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td rowspan="3"><a href="scenario-desktop-overview.md"><img alt=Desktop app that calls web APIs" src="media/scenarios/desktop-app.svg"></a></td>
   <td rowspan="4"><a href="scenario-desktop-overview.md">Web API'lerini çağıran bir masaüstü uygulaması</a></td>
   <td>PKCE ile <a href="v2-oauth2-auth-code-flow.md">Yetkilendirme Kodu</a> kullanılarak etkileşimli</td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td>Entegre Windows Auth</td>
   <td>İş veya okul hesapları</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Kaynak Sahibi Şifresi</a></td>
   <td>İş veya okul hesapları ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-desktop-acquire-token.md#command-line-tool-without-a-web-browser"><img alt="Browserless application" src="media/scenarios/device-code-flow-app.svg"></a></td>
   <td><a href="v2-oauth2-device-code.md">Cihaz kodu</a></td>
   <td>İş veya okul hesapları</td>
 </tr>

 <tr>
   <td rowspan="2"><a href="scenario-mobile-overview.md"><img alt="Mobile app that calls web APIs" src="media/scenarios/mobile-app.svg"></a></td>
   <td rowspan="2"><a href="scenario-mobile-overview.md">Web API'lerini çağıran bir mobil uygulama</a></td>
   <td>PKCE ile <a href="v2-oauth2-auth-code-flow.md">Yetkilendirme Kodu</a> kullanılarak etkileşimli</td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Kaynak Sahibi Şifresi</a></td>
   <td>İş veya okul hesapları ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-daemon-overview.md"><img alt="Daemon app that calls web APIs" src="media/scenarios/daemon-app.svg"></a></td>
   <td><a href="scenario-daemon-overview.md">Web API'lerini çağıran bir daemon uygulaması</a></td>
   <td><a href="v2-oauth2-client-creds-grant-flow.md">İstemci kimlik bilgileri</a></td>
   <td>Kullanıcı olmadan ve yalnızca Azure REKLAM kuruluşlarında kullanılan yalnızca uygulama izinleri</td>
 </tr>

  <tr>
   <td><a href="scenario-web-api-call-api-overview.md"><img alt="Web API that calls web APIs" src="media/scenarios/web-api.svg"></a></td>
   <td><a href="scenario-web-api-call-api-overview.md">Web API'lerini çağıran bir web API'si</a></td>
   <td><a href="v2-oauth2-on-behalf-of-flow.md">Adına</a></td>
   <td>İş veya okul hesapları ve kişisel hesaplar</td>
 </tr>

 </tbody>
</table>

## <a name="scenarios-and-supported-platforms-and-languages"></a>Senaryolar ve desteklenen platformlar ve diller

Microsoft Kimlik Doğrulama kitaplıkları birden çok platformu destekler:

- JavaScript
- .NET Framework
- .NET Core
- Windows 10/UWP
- Xamarin.iOS
- Xamarin.Android
- Yerel iOS
- macOS
- Yerli Android
- Java
- Python

Uygulamalarınızı oluşturmak için çeşitli diller de kullanabilirsiniz.

> [!NOTE]
> Bazı uygulama türleri her platformda kullanılamaz.

Aşağıdaki tablonun Windows sütununda ,NET Core'dan her bahsedildiğizaman ,.NET Framework de mümkündür. İkincisi tabloda darmadağın önlemek için atlanır.

|Senaryo  | Windows | Linux | Mac | iOS | Android
|--|--|--|--|--|--|--|
| [Tek sayfalı uygulama](scenario-spa-overview.md) <br/>[![Tek Sayfauygulaması](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js
| [Kullanıcılarda işaretleyen Web Uygulaması](scenario-web-app-sign-user-overview.md) <br/>[![Web App bu-in kullanıcıları](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core
| [Web API'lerini çağıran Web Uygulaması](scenario-web-app-call-api-overview.md) <br/> <br/>[![Web API'lerini çağıran Web Uygulaması](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png) <br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>Şişe + MSAL Python| ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>Şişe + MSAL Python| ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/> ![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>Şişe + MSAL Python
| [Web API'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md) <br/> <br/>Web ![ [API'leri çağıran masaüstü uygulaması Aygıt kodu ![](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md) akışı](media/scenarios/device-code-flow-app.svg) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/> ![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton <br/> ![iOS / Objective C veya swift](media/sample-v2-code/small_logo_iOS.png) MSAL.objc |
| [Web API'lerini çağıran mobil uygulama](scenario-mobile-overview.md) <br/> [![Web API'lerini çağıran mobil uygulama](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | ![UWP](media/sample-v2-code/small_logo_windows.png) MSAL.NET ![Xamarin](media/sample-v2-code/small_logo_xamarin.png) MSAL.NET | | | ![iOS / Objective C veya swift](media/sample-v2-code/small_logo_iOS.png) MSAL.objc | ![Android](media/sample-v2-code/small_logo_Android.png) MSAL. Android
| [Daemon uygulaması](scenario-daemon-overview.md) <br/> [![Daemon uygulaması](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton
| [Web API'lerini çağıran web API'si](scenario-web-api-call-api-overview.md) <br/><br/> [![Web API'larını çağıran Web API'si](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | ![ASP.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdek + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Piton](media/sample-v2-code/small_logo_python.png)<br/>MSAL Piton

Daha fazla bilgi için, [Işletim Sistemi / dil tarafından Microsoft destekli kitaplıklara](reference-v2-libraries.md#microsoft-supported-libraries-by-os--language)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik Doğrulama temelleri](authentication-scenarios.md) ve [Microsoft kimlik platformu erişim belirteçleri](access-tokens.md)hakkında daha fazla bilgi edinin.
* [IoT uygulamalarına erişimi güvence altına alma](/azure/architecture/example-scenario/iot-aad/iot-aad)hakkında daha fazla bilgi edinin.
