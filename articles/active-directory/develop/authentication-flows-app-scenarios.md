---
title: Uygulama senaryolarına & Microsoft Identity platform kimlik doğrulama akışları | Mavisi
description: Kimlik doğrulama, belirteçleri alma ve korumalı API 'Leri çağırma dahil olmak üzere Microsoft Identity platformu için uygulama senaryoları hakkında bilgi edinin.
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
ms.openlocfilehash: 89bafeb077fc83f4f3165d591006831bf8287875
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78365304"
---
# <a name="authentication-flows-and-application-scenarios"></a>Kimlik doğrulama akışları ve uygulama senaryoları

Microsoft Identity platform (v 2.0) uç noktası, farklı türlerde modern uygulama mimarilerinin kimlik doğrulamasını destekler. Mimarilerin hepsi, [OAuth 2,0 ve OpenID Connect](active-directory-v2-protocols.md)sektör standardı protokollerine dayanır.  [Microsoft Identity platform kimlik doğrulama kitaplıklarını](reference-v2-libraries.md)kullanarak, uygulamalar kimliklerin kimliğini doğrular ve korunan API 'lere erişmek için belirteçleri elde edin.

Bu makalede, kimlik doğrulama akışları ve kullandıkları uygulama senaryoları açıklanmaktadır:

- [Uygulama senaryoları ve desteklenen kimlik doğrulama akışları](#scenarios-and-supported-authentication-flows).
- [Uygulama senaryoları ve desteklenen platformlar ve diller](#scenarios-and-supported-platforms-and-languages).

## <a name="application-categories"></a>Uygulama kategorileri

Belirteçleri aşağıdakiler dahil olmak üzere çeşitli türde uygulamalardan elde edilebilir:

- Web uygulamaları
- Mobil uygulamalar
- Masaüstü uygulamaları
- Web API'leri

Belirteçler, tarayıcıya sahip olmayan veya IoT üzerinde çalışan cihazlarda çalışan uygulamalardan de elde edilebilir.

Uygulamalar aşağıdaki listede olduğu gibi kategorilere ayrılır:

- [Korumalı kaynaklara karşı istemci uygulamalar](#protected-resources-vs-client-applications): bazı senaryolar, Web uygulamaları veya Web API 'leri gibi kaynakları korumaya yönelik bir uygulamalardır. Diğer senaryolar, korumalı bir Web API 'SI çağırmak için bir güvenlik belirteci alma ile ilgilidir.
- [Kullanıcılar veya kullanıcılar olmadan](#with-users-or-without-users): bazı senaryolar oturum açmış bir Kullanıcı içerir, ancak diğer yandan Daemon senaryoları gibi diğerleri de bir kullanıcı içermez.
- [Tek sayfalı, genel istemci ve gizli istemci uygulamaları](#single-page-public-client-and-confidential-client-applications): Bu türler üç büyük uygulama kategorileridir. Her biri farklı kitaplıklar ve nesneler ile kullanılır.
- [Oturum açma hedef kitlesi](v2-supported-account-types.md#certain-authentication-flows-dont-support-all-the-account-types): kullanılabilir kimlik doğrulama akışları, oturum açma izleyicisine bağlı olarak farklılık gösterir. Bazı akışlar yalnızca iş veya okul hesapları için kullanılabilir. Bazıları hem iş ya da okul hesapları hem de kişisel Microsoft hesapları için kullanılabilir. İzin verilen hedef kitle, kimlik doğrulama akışlarına bağlıdır.
- [Desteklenen OAuth 2,0 akışları](#scenarios-and-supported-authentication-flows): kimlik doğrulama akışları, belirteç isteyen uygulama senaryolarını uygulamak için kullanılır. Uygulama senaryoları ve kimlik doğrulama akışları arasında bire bir eşleme yoktur.
- [Desteklenen platformlar](#scenarios-and-supported-platforms-and-languages): tüm uygulama senaryoları her platformda kullanılamaz.

### <a name="protected-resources-vs-client-applications"></a>Korumalı kaynaklara ve istemci uygulamalarına karşı

Kimlik doğrulama senaryoları iki etkinlik içerir:

- **Korumalı bir Web API 'si için güvenlik belirteçleri alınıyor**: özel Microsoft kimlik doğrulama KITAPLıĞı (msal) ailesinde belirteçleri almak için [Microsoft tarafından desteklenen istemci kitaplıklarını](reference-v2-libraries.md#microsoft-supported-client-libraries) kullanmanızı öneririz.
- Web **API 'sini veya Web uygulamasını koruma**: BIR Web API 'sini veya Web uygulaması kaynağını korumanın bir testi güvenlik belirtecini doğruluyor. Bazı platformlarda Microsoft, [Ara yazılım kitaplıkları](reference-v2-libraries.md#microsoft-supported-server-middleware-libraries)sunmaktadır.

### <a name="with-users-or-without-users"></a>Kullanıcılar veya kullanıcılar olmadan

Çoğu kimlik doğrulama senaryosunda, oturum açmış kullanıcılar adına belirteç elde edin.

![Kullanıcıları olan senaryolar](media/scenarios/scenarios-with-users.svg)

Ancak, uygulamaların kullanıcı olmadan kendi adına belirteç edinmeleri için de Daemon-App senaryoları vardır.

![Daemon uygulamalarıyla senaryolar](media/scenarios/daemon-app.svg)

### <a name="single-page-public-client-and-confidential-client-applications"></a>Tek sayfalı, ortak istemci ve gizli istemci uygulamaları

Güvenlik belirteçleri, birden çok uygulama türünden alınabilir. Bu uygulamalar üç kategoride ayrılmalıdır:

- **Tek sayfalı uygulamalar**: Ayrıca, Spas olarak da bilinen bu uygulamalar, tarayıcıda çalışan bir JavaScript veya TypeScript uygulamasından alınan belirteçlerin alındığı Web uygulamalardır. Birçok modern uygulamanın, birincil olarak JavaScript 'te yazılmış tek sayfalı bir uygulama ön ucu vardır. Uygulama genellikle angular, yanıt verme veya Vue gibi bir çerçeve kullanır. Tek sayfalı uygulamaları destekleyen tek Microsoft kimlik doğrulama kitaplığı MSAL. js ' dir.

- **Ortak istemci uygulamaları**: Bu uygulamalar kullanıcıların her zaman oturum açmasını ister:
  - Oturum açmış kullanıcı adına Web API 'Lerini çağıran masaüstü uygulamaları
  - Mobil uygulamalar
  - IoT üzerinde çalışan cihazlar gibi tarayıcıya sahip olmayan cihazlarda çalışan uygulamalar

  Bu uygulamalar MSAL [Publicclientapplication](/dotnet/api/microsoft.identity.client.publicclientapplication) sınıfı tarafından temsil edilir. Daha fazla bilgi için bkz. [genel istemci ve gizli istemci uygulamaları](msal-client-applications.md).

- **Gizli istemci uygulamaları**:
  - Web API 'sini çağıran Web uygulamaları
  - Web API 'sini çağıran Web API 'Leri
  - Linux Daemon veya Windows hizmeti gibi bir konsol hizmeti olarak uygulandığında bile Daemon uygulamaları

  Bu tür uygulamalar [ConfidentialClientApplication](/dotnet/api/microsoft.identity.client.confidentialclientapplication) sınıfını kullanır. Daha fazla bilgi için bkz. [genel istemci ve gizli istemci uygulamaları](msal-client-applications.md).

## <a name="application-scenarios"></a>Uygulama senaryoları

Microsoft Identity platform uç noktası, farklı uygulama mimarilerinin kimlik doğrulamasını destekler:

- Tek sayfalı uygulamalar
- Web uygulamaları
- Web API'leri
- Mobil uygulamalar
- Yerel uygulamalar
- Daemon uygulamaları
- Sunucu tarafı uygulamalar

Uygulamalar, kullanıcıları oturum açmak ve korunan API 'Leri çağırmak için belirteçleri almak üzere farklı kimlik doğrulama akışlarını kullanır.

### <a name="a-single-page-application"></a>Tek sayfalı uygulama

Birçok modern web uygulaması, istemci tarafı tek sayfalı uygulamalar olarak oluşturulmuştur. JavaScript kullanan bu uygulamalar, angular, Vue. js ve yanıt verme. js gibi tek sayfalı bir uygulama çerçevesi. Bu uygulamalar bir Web tarayıcısında çalışır.

Kimlik doğrulama özellikleri geleneksel sunucu tarafı Web uygulamalarından farklıdır. Tek sayfalı uygulamalar, Microsoft Identity platformunu kullanarak kullanıcıların oturumunu açabilir ve arka uç hizmetlerine veya Web API 'Lerine erişim belirteçleri alabilir.

![Tek sayfalı uygulama](media/scenarios/spa-app.svg)

Daha fazla bilgi için bkz. [tek sayfalı uygulamalar](scenario-spa-overview.md).

### <a name="a-web-app-that-is-signing-in-a-user"></a>Kullanıcı oturumu açan bir Web uygulaması

![Kullanıcı oturumu açan bir Web uygulaması](media/scenarios/scenario-webapp-signs-in-users.svg)

Bir kullanıcının oturum açtığı bir Web uygulamasını korumak için:

- .NET ortamında geliştirirseniz, ASP.NET Open ID Connect ara yazılımı ile ASP.NET veya ASP.NET Core kullanırsınız. Bir kaynağın korunması, [.NET kitaplığı Için IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) tarafından GERÇEKLEŞTIRILEN ve msal kitaplıklarını değil güvenlik belirtecinin doğrulanmasını içerir.

- Node. js ' de geliştirme yaptıysanız, Passport. js ' yi kullanırsınız.

Daha fazla bilgi için, bkz. [Kullanıcılar oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md).

### <a name="a-web-app-that-signs-in-a-user-and-calling-a-web-api-on-behalf-of-the-user"></a>Kullanıcıya oturum açan ve Kullanıcı adına bir Web API 'SI çağıran bir Web uygulaması

![Web API 'Lerini çağıran bir Web uygulaması](media/scenarios/web-app.svg)

Bir Web uygulamasından Kullanıcı adına bir Web API 'SI çağırmak için MSAL **ConfidentialClientApplication** sınıfını kullanın. Yetkilendirme kodu akışını kullanır ve alınan belirteçleri belirteç önbelleğinde depolar. Gerektiğinde, MSAL belirteçleri yeniler ve denetleyici önbellekteki belirteçleri sessizce alır.

Daha fazla bilgi için bkz. [Web API 'leri çağıran bir Web uygulaması](scenario-web-app-call-api-overview.md).

### <a name="a-desktop-app-calling-a-web-api-on-behalf-of-a-signed-in-user"></a>Bir Web API 'sini, oturum açmış bir kullanıcı adına çağıran bir masaüstü uygulaması

Bir masaüstü uygulamasının kullanıcılar tarafından oturum açan bir Web API 'sini çağırması için MSAL **Publicclientapplication** sınıfının etkileşimli belirteç alma yöntemlerini kullanın. Bu etkileşimli yöntemlerle oturum açma kullanıcı arabirimi deneyimini kontrol edebilirsiniz. MSAL bu etkileşim için bir Web tarayıcısı kullanır.

![Web API 'sini çağıran masaüstü uygulaması](media/scenarios/desktop-app.svg)

Windows etki alanına veya Azure Active Directory (Azure AD) ile Birleşik bilgisayarlarda Windows 'da barındırılan uygulamalar için başka bir olasılık vardır. Bu uygulamalar [Tümleşik Windows kimlik doğrulaması](https://aka.ms/msal-net-iwa)kullanarak sessizce bir belirteç elde edebilir.

Tarayıcı olmadan bir cihazda çalışan uygulamalar, bir kullanıcı adına API 'yi hala çağırabilir. Kimlik doğrulamak için, kullanıcının Web tarayıcısına sahip başka bir cihazda oturum açması gerekir. Bu senaryo, [cihaz kodu akışını](https://aka.ms/msal-net-device-code-flow)kullanmanızı gerektirir.

![Cihaz kod akışı](media/scenarios/device-code-flow-app.svg)

Bunu kullanmanızı önermeyiz, ancak [Kullanıcı adı/parola akışı](https://aka.ms/msal-net-up) ortak istemci uygulamalarında kullanılabilir. Bu akış, DevOps gibi bazı senaryolarda hala gereklidir.

Ancak bu akışın kullanılması uygulamalarınızı kısıtlar. Örneğin, uygulamalar Multi-Factor Authentication veya koşullu erişim kullanması gereken bir kullanıcıya oturum açamaz. Uygulamalarınız aynı zamanda çoklu oturum açma özelliğinden de yararlanır.

Kullanıcı adı/parola akışı ile kimlik doğrulaması, modern kimlik doğrulamasının ilkelerine karşı gider ve yalnızca eski nedenlerle sağlanır.

Masaüstü uygulamalarında, belirteç önbelleğinin kalıcı olmasını istiyorsanız [belirteç önbelleği serileştirmesini](https://aka.ms/msal-net-token-cache-serialization)özelleştirin. [Çift belirteç önbelleği serileştirmesini](https://aka.ms/msal-net-dual-cache-serialization)uygulayarak, geriye dönük uyumlu ve ileri uyumlu belirteç önbellekleri kullanabilirsiniz. Bu belirteçler, önceki nesil kimlik doğrulama kitaplıklarını destekler. Belirli kitaplıklar .NET için Azure AD kimlik doğrulama kitaplığı 'nı (ADAL.NET) sürüm 3 ve sürüm 4 içerir.

Daha fazla bilgi için bkz. [Web API 'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md).

### <a name="a-mobile-app-calling-a-web-api-on-behalf-of-an-interactive-user"></a>Etkileşimli kullanıcı adına Web API 'SI çağıran bir mobil uygulama

Bir masaüstü uygulamasına benzer şekilde, bir mobil uygulama, Web API 'sini çağırmak için bir belirteç almak üzere MSAL **Publicclientapplication** sınıfının etkileşimli belirteç alma yöntemlerini çağırır.

![Web API 'sini çağıran bir mobil uygulama](media/scenarios/mobile-app.svg)

MSAL iOS ve MSAL Android varsayılan olarak sistem Web tarayıcısını kullanır. Ancak, bunun yerine katıştırılmış Web görünümünü kullanmak için bunları yönlendirebilirsiniz. Evrensel Windows Platformu (UWP), iOS veya Android mobil platformuna bağlı olan değerler vardır.

Bir cihaz KIMLIĞI veya cihaz kaydıyla ilgili koşullu erişim içeren bazı senaryolar, cihaza bir aracı yüklenmesini gerektirir. Aracıların örnekleri, Android ve iOS üzerinde Microsoft Şirket Portalı Microsoft Authenticator. MSAL artık aracılar ile etkileşime geçebilir. Daha fazla bilgi için bkz. [Android ve iOS 'ta aracıları](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/leveraging-brokers-on-Android-and-iOS)kullanma.

Daha fazla bilgi için bkz. [Web API 'lerini çağıran mobil uygulama](scenario-mobile-overview.md).

> [!NOTE]
> MSAL. iOS, MSAL kullanan mobil uygulamanız. Xamarin üzerinde Android veya MSAL.NET, uygulama koruma ilkelerinin uygulanmasını sağlayabilir. Örneğin, ilkeler bir kullanıcının korumalı metni kopyalamasını engelleyebilir. Mobil uygulama, Intune tarafından yönetilir ve Intune tarafından yönetilen bir uygulama olarak tanınır. Daha fazla bilgi için bkz. [Microsoft Intune uygulama SDK 'sına genel bakış](https://docs.microsoft.com/intune/app-sdk).
>
> [Intune uygulama SDK 'Sı](https://docs.microsoft.com/intune/app-sdk-get-started) msal kitaplıklarından ayrıdır ve kendi kendıne Azure AD ile etkileşime girer.

### <a name="a-protected-web-api"></a>Korumalı bir Web API 'SI

Uygulamanızın yeniden Web API 'SI gibi Web hizmetlerini güvenli hale getirmek için Microsoft Identity platform uç noktasını kullanabilirsiniz. Korumalı bir Web API 'SI, erişim belirteci kullanılarak çağrılır. Belirteç, API 'nin verilerini güvenlik altına alır ve gelen isteklerin kimliğini doğrular. Bir Web API 'SI çağıran, bir HTTP isteğinin yetkilendirme üstbilgisine bir erişim belirteci ekler.

ASP.NET veya ASP.NET Core Web API 'nizi korumak istiyorsanız, erişim belirtecini doğrulamanız gerekir. Bu doğrulama için ASP.NET JWT ara yazılımını kullanırsınız. Doğrulama, MSAL.NET tarafından değil [.NET kitaplığı Için IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) tarafından yapılır.

Daha fazla bilgi için bkz. [korumalı Web API 'si](scenario-protected-web-api-overview.md).

### <a name="a-web-api-calling-another-web-api-on-behalf-of-a-user"></a>Kullanıcı adına başka bir Web API 'SI çağıran Web API 'SI

ASP.NET veya ASP.NET Core korunan Web API 'niz Kullanıcı adına başka bir Web API 'SI çağırmak için uygulamanızın, aşağı akış Web API 'SI için bir belirteç edinmesi gerekir. Bir belirteç almak için, uygulamanız **ConfidentialClientApplication** sınıfının [Acquiretokenonbehalfof](https://aka.ms/msal-net-on-behalf-of) yöntemini çağırır. Bu tür çağrılar Ayrıca *hizmetten hizmete* çağrılar olarak adlandırılır. Diğer Web API 'Lerini çağıran Web API 'Lerinin özel önbellek serileştirme sağlaması gerekir.

  ![Başka bir Web API 'SI çağıran Web API 'SI](media/scenarios/web-api.svg)

Daha fazla bilgi için bkz. [Web API 'lerini çağıran Web API 'si](scenario-web-api-call-api-overview.md).

### <a name="a-daemon-app-calling-a-web-api-in-the-daemons-name"></a>Arka plan programının adında Web API 'sini çağıran bir Daemon uygulaması

Uzun süre çalışan işlemlere sahip olan veya Kullanıcı etkileşimi olmadan çalışan uygulamalarda güvenli Web API 'Lerine erişmek için bir yol gerekir. Bu tür bir uygulama uygulamanın kimliğini kullanarak kimlik doğrulaması yapabilir ve belirteçleri alabilir. Uygulama, bir istemci gizli dizisi veya sertifikası kullanarak kimliğini kanıtlar.

MSAL **ConfidentialClientApplication** sınıfının [istemci kimlik bilgileri](https://aka.ms/msal-net-client-credentials) alma yöntemlerini kullanarak, çağıran uygulama için bir belirteç alan Daemon uygulamalarını yazabilirsiniz. Bu yöntemler, çağıran uygulamanın Azure AD ile gizli bir parola kaydettirmesini gerektirir. Ardından uygulama, parolayı çağrılan daemon ile paylaşır. Bu gizli dizi örnekleri uygulama parolaları, sertifika onaylama veya istemci onaylama işlemlerini içerir.

![Diğer uygulamalar ve API 'Ler tarafından çağrılan bir Daemon uygulaması](media/scenarios/daemon-app.svg)

Daha fazla bilgi için bkz. [Web API 'lerini çağıran Daemon uygulaması](scenario-daemon-overview.md).

## <a name="scenarios-and-supported-authentication-flows"></a>Senaryolar ve desteklenen kimlik doğrulama akışları

Belirteçleri alma ile ilgili senaryolar Ayrıca OAuth 2,0 kimlik doğrulama akışlarıyla eşlenir. Daha fazla bilgi için bkz. [Microsoft Identity platformunda OAuth 2,0 ve OpenID Connect Protocols](active-directory-v2-protocols.md).

<table>
 <thead>
  <tr><th>Senaryo</th> <th>Ayrıntılı senaryo izlenecek yol</th> <th>OAuth 2,0 Flow ve verme</th> <th>Hedef kitle</th></tr>
 </thead>
 <tbody>
  <tr>
   <td><a href="scenario-spa-overview.md"><img alt="Single-Page App" src="media/scenarios/spa-app.svg"></a></td>
   <td><a href="scenario-spa-overview.md">Tek sayfalı uygulama</a></td>
   <td><a href="v2-oauth2-implicit-grant-flow.md">İndirgen</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Microsoft Azure Active Directory B2C (Azure AD B2C)</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-sign-user-overview.md"><img alt="Web App that signs in users" src="media/scenarios/scenario-webapp-signs-in-users.svg"></a></td>
   <td><a href="scenario-web-app-sign-user-overview.md">Kullanıcılara oturum açan bir Web uygulaması</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Yetkilendirme kodu</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-web-app-call-api-overview.md"><img alt="Web App that signs in users" src="media/scenarios/web-app.svg"></a></td>
   <td><a href="scenario-web-app-call-api-overview.md">Web API 'Lerini çağıran bir Web uygulaması</a></td>
   <td><a href="v2-oauth2-auth-code-flow.md">Yetkilendirme kodu</a></td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td rowspan="3"><a href="scenario-desktop-overview.md"><img alt=Desktop app that calls web APIs" src="media/scenarios/desktop-app.svg"></a></td>
   <td rowspan="4"><a href="scenario-desktop-overview.md">Web API 'Lerini çağıran bir masaüstü uygulaması</a></td>
   <td>PKI ile <a href="v2-oauth2-auth-code-flow.md">yetkilendirme kodu</a> kullanarak etkileşimli</td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td>Tümleşik Windows kimlik doğrulaması</td>
   <td>İş veya okul hesapları</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Kaynak sahibi parolası</a></td>
   <td>İş veya okul hesapları ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-desktop-acquire-token.md#command-line-tool-without-a-web-browser"><img alt="Browserless application" src="media/scenarios/device-code-flow-app.svg"></a></td>
   <td><a href="v2-oauth2-device-code.md">Cihaz kodu</a></td>
   <td>İş veya okul hesapları</td>
 </tr>

 <tr>
   <td rowspan="2"><a href="scenario-mobile-overview.md"><img alt="Mobile app that calls web APIs" src="media/scenarios/mobile-app.svg"></a></td>
   <td rowspan="2"><a href="scenario-mobile-overview.md">Web API 'Lerini çağıran bir mobil uygulama</a></td>
   <td>PKI ile <a href="v2-oauth2-auth-code-flow.md">yetkilendirme kodu</a> kullanarak etkileşimli</td>
   <td>İş veya okul hesapları, kişisel hesaplar ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="v2-oauth-ropc.md">Kaynak sahibi parolası</a></td>
   <td>İş veya okul hesapları ve Azure AD B2C</td>
 </tr>

  <tr>
   <td><a href="scenario-daemon-overview.md"><img alt="Daemon app that calls web APIs" src="media/scenarios/daemon-app.svg"></a></td>
   <td><a href="scenario-daemon-overview.md">Web API 'Lerini çağıran bir Daemon uygulaması</a></td>
   <td><a href="v2-oauth2-client-creds-grant-flow.md">İstemci kimlik bilgileri</a></td>
   <td>Kullanıcı olmadan ve yalnızca Azure AD kuruluşlarda kullanılan yalnızca uygulama izinleri</td>
 </tr>

  <tr>
   <td><a href="scenario-web-api-call-api-overview.md"><img alt="Web API that calls web APIs" src="media/scenarios/web-api.svg"></a></td>
   <td><a href="scenario-web-api-call-api-overview.md">Web API 'Lerini çağıran bir Web API 'SI</a></td>
   <td><a href="v2-oauth2-on-behalf-of-flow.md">Adına</a></td>
   <td>İş veya okul hesapları ve kişisel hesaplar</td>
 </tr>

 </tbody>
</table>

## <a name="scenarios-and-supported-platforms-and-languages"></a>Senaryolar ve desteklenen platformlar ve diller

Microsoft kimlik doğrulama kitaplıkları Çoklu platformları destekler:

- JavaScript
- .NET Framework
- .NET Core
- Windows 10/UWP
- Xamarin.iOS
- Xamarin.Android
- Yerel iOS
- macOS
- Yerel Android
- Java
- Python

Uygulamalarınızı oluşturmak için çeşitli diller de kullanabilirsiniz.

> [!NOTE]
> Bazı uygulama türleri her platformda kullanılamaz.

Aşağıdaki tablonun Windows sütununda, .NET Core her bahsedildiğinde .NET Framework de mümkündür. İkinci değer, tablonun karışıklık oluşmasını önlemek için atlanır.

|Senaryo  | Windows | Linux | Mac | iOS | Android
|--|--|--|--|--|--|--|
| [Tek sayfalı uygulama](scenario-spa-overview.md) <br/>[Tek sayfalı uygulama ![](media/scenarios/spa-app.svg)](scenario-spa-overview.md) | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png) MSAL.js | ![MSAL.js](media/sample-v2-code/small_logo_js.png)<br/>MSAL.js
| [Kullanıcılarda oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md) <br/>[kullanıcıları oturum açan ![Web uygulaması](media/scenarios/scenario-webapp-signs-in-users.svg)](scenario-web-app-sign-user-overview.md) | ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdeği | ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdeği | ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Çekirdeği
| [Web API 'Lerini çağıran Web uygulaması](scenario-web-app-call-api-overview.md) <br/> <br/>[Web API 'Lerini çağıran ![Web uygulaması](media/scenarios/web-app.svg)](scenario-web-app-call-api-overview.md) | ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png) <br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>Flask + MSAL Python| ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>Flask + MSAL Python| ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/> ![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>Flask + MSAL Python
| [Web API'lerini çağıran masaüstü uygulaması](scenario-desktop-overview.md) <br/> <br/>Cihaz kod akışı ![[Web API 'lerini çağıran![masaüstü uygulaması](media/scenarios/desktop-app.svg)](scenario-desktop-overview.md)](media/scenarios/device-code-flow-app.svg) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/> ![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python <br/> ![iOS/amaç C veya Swift](media/sample-v2-code/small_logo_iOS.png) MSAL. ObjC |
| [Web API 'Lerini çağıran mobil uygulama](scenario-mobile-overview.md) <br/> [Web API 'Lerini çağıran mobil uygulama ![](media/scenarios/mobile-app.svg)](scenario-mobile-overview.md) | ![UWP](media/sample-v2-code/small_logo_windows.png) MSAL.NET ![Xamarin](media/sample-v2-code/small_logo_xamarin.png) MSAL.NET | | | ![iOS/amaç C veya Swift](media/sample-v2-code/small_logo_iOS.png) MSAL. ObjC | ![Android](media/sample-v2-code/small_logo_Android.png) MSAL. Android
| [Daemon uygulaması](scenario-daemon-overview.md) <br/> [![Daemon uygulaması](media/scenarios/daemon-app.svg)](scenario-daemon-overview.md) | ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png) MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python
| [Web API'lerini çağıran web API'si](scenario-web-api-call-api-overview.md) <br/><br/> [Web API 'Lerini çağıran ![Web API 'SI](media/scenarios/web-api.svg)](scenario-web-api-call-api-overview.md) | ![ASP.NET Çekirdeği](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python| ![.NET Core](media/sample-v2-code/small_logo_NETcore.png)<br/>ASP.NET Core + MSAL.NET ![MSAL Java](media/sample-v2-code/small_logo_java.png)<br/>MSAL Java<br/>![MSAL Python](media/sample-v2-code/small_logo_python.png)<br/>MSAL Python

Daha fazla bilgi için bkz. [OS/Language tarafından desteklenen Microsoft kitaplıkları](reference-v2-libraries.md#microsoft-supported-libraries-by-os--language).

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik doğrulama temelleri](authentication-scenarios.md) ve [Microsoft Identity platform erişim belirteçleri](access-tokens.md)hakkında daha fazla bilgi edinin.
