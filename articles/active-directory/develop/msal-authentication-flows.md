---
title: MSAL kimlik doğrulama akışları | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL) tarafından kullanılan kimlik doğrulama akışları ve hibeler hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/30/2020
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 25c219bedbbbec9fbc0c5617c7bd9fc482faf49a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050503"
---
# <a name="authentication-flows"></a>Kimlik doğrulama akışları

Bu makalede, Microsoft Kimlik Doğrulama Kitaplığı (MSAL) tarafından sağlanan farklı kimlik doğrulama akışları açıklanmaktadır.  Bu akışlar çeşitli farklı uygulama senaryolarında kullanılabilir.

| Akış | Açıklama | Kullanılan|  
| ---- | ----------- | ------- | 
| [Etkileşimli](#interactive) | Belirteci, kullanıcıdan kimlik bilgilerini bir tarayıcı veya açılır pencere den istenilen etkileşimli bir işlemle alır. | [Masaüstü uygulamaları,](scenario-desktop-overview.md) [mobil uygulamalar](scenario-mobile-overview.md) |
| [Örtülü hibe](#implicit-grant) | Uygulamanın arka uç sunucu kimlik bilgisi alışverişi yapmadan belirteçler elde etmesini sağlar. Bu, uygulamanın kullanıcıda oturum açmasına, oturumu sürdürmesine ve tüm istemci JavaScript kodu içinde bulunan diğer web API'lerine belirteçler almalarına olanak tanır.| [Tek sayfauygulamaları (SPA)](scenario-spa-overview.md) |
| [Yetkilendirme kodu](#authorization-code) | Web API'leri gibi korumalı kaynaklara erişmek için aygıta yüklenen uygulamalarda kullanılır. Bu, mobil ve masaüstü uygulamalarınıza oturum açma ve API erişimi eklemenize olanak tanır. | [Masaüstü uygulamaları,](scenario-desktop-overview.md) [mobil uygulamalar,](scenario-mobile-overview.md) [web uygulamaları](scenario-web-app-call-api-overview.md) | 
| [Adına](#on-behalf-of) | Bir uygulama, sırayla başka bir hizmet veya web API çağırmak için gereken bir hizmet veya web API çağırır. Amaç, devredilen kullanıcı kimliğini ve izinleri istek zinciri aracılığıyla yaymaktır. | [Web API'leri](scenario-web-api-call-api-overview.md) |
| [İstemci kimlik bilgileri](#client-credentials) | Bir uygulamanın kimliğini kullanarak web tarafından barındırılan kaynaklara erişmenizi sağlar. Genellikle, kullanıcıyla hemen etkileşim edilmeden arka planda çalışması gereken sunucudan sunucuya etkileşimler için kullanılır. | [Daemon uygulamaları](scenario-daemon-overview.md) |
| [Cihaz kodu](#device-code) | Kullanıcıların akıllı TV, IoT aygıtı veya yazıcı gibi giriş kısıtlamalı aygıtlarda oturum açmalarına olanak tanır. | [Masaüstü/mobil uygulamalar](scenario-desktop-acquire-token.md#command-line-tool-without-a-web-browser) |
| [Tümleşik Windows Kimlik Doğrulaması](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Etki alanı veya Azure Etkin Dizin (Azure AD) üzerindeki uygulamaların bilgisayarların birleştiği uygulamalara sessizce bir belirteç (kullanıcıdan herhangi bir Kullanıcı Arabirimi etkileşimi olmadan) alma olanağı sağlar.| [Masaüstü/mobil uygulamalar](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Kullanıcı adı/şifre](scenario-desktop-acquire-token.md#username-and-password) | Bir uygulamanın parolasını doğrudan işleyerek kullanıcıda oturum açmasına izin verir. Bu akış önerilmez. | [Masaüstü/mobil uygulamalar](scenario-desktop-acquire-token.md#username-and-password) |

## <a name="how-each-flow-emits-tokens-and-codes"></a>Her akış nasıl belirteçleri ve kodları yayır
 
İstemcinizin nasıl oluşturulabildiğine bağlı olarak, Microsoft kimlik platformu tarafından desteklenen kimlik doğrulama akışlarından birini (veya birkaçını) kullanabilir.  Bu akışlar, yetkilendirme kodlarının yanı sıra çeşitli belirteçler (id_tokens, yenileme belirteçleri, erişim belirteçleri) üretebilir ve bunları çalışması için farklı belirteçler gerektirebilir. Bu grafik genel bir bakış sağlar:
 
|Akış | Gerektirir | id_token | erişim belirteci | belirteci yenileme | yetkilendirme kodu | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Yetkilendirme kodu akışı](v2-oauth2-auth-code-flow.md) | | x | x | x | x|  
|[Örtük akış](v2-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Hibrid OIDC akışı](v2-protocols-oidc.md#get-access-tokens)| | x  | |          |            x   |
|[Belirteç itfasını yenile](v2-oauth2-auth-code-flow.md#refresh-the-access-token) | belirteci yenileme | x | x | x| |
|[On-behalf-of akışı](v2-oauth2-on-behalf-of-flow.md) | erişim belirteci| x| x| x| |
|[Cihaz kodu akışı](v2-oauth2-device-code.md) | | x| x| x| |
|[İstemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) | | | x (yalnızca uygulama)| | |
 
Örtük mod üzerinden verilen belirteçler, URL üzerinden tarayıcıya geri aktarılması `response_mode` `query` nedeniyle `fragment`bir uzunluk sınırlamasına sahiptir (nerede veya).  Bazı tarayıcıların, tarayıcı çubuğuna konulabilecek ve çok uzun olduğunda başarısız olabilecek URL boyutuyla ilgili bir sınırı vardır.  Bu nedenle, bu belirteçleri yok `groups` veya `wids` iddiaları yok.

## <a name="interactive"></a>Etkileşimli

MSAL, kullanıcıdan kimlik bilgilerinin oturum açmasıiçin etkileşimli olarak sorulmasını ve bu kimlik bilgilerini kullanarak bir belirteç edinmesini destekler.

![Etkileşimli akış diyagramı](media/msal-authentication-flows/interactive.png)

Belirli platformlarda etkileşimli olarak belirteçler elde etmek için MSAL.NET kullanma hakkında daha fazla bilgi için bkz:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Evrensel Windows Platformu](msal-net-uwp-considerations.md)

MSAL.js'deki etkileşimli aramalar hakkında daha fazla bilgi için, [MSAL.js etkileşimli isteklerde İstem davranışına](msal-js-prompt-behavior.md)bakın.

## <a name="implicit-grant"></a>Örtülü hibe

MSAL, uygulamanın arka uç sunucu kimlik bilgisi alışverişi yapmadan Microsoft kimlik platformundan jeton almalarına olanak tanıyan [OAuth 2 örtülü hibe akışını](v2-oauth2-implicit-grant-flow.md)destekler. Bu, uygulamanın kullanıcıda oturum açmasına, oturumu sürdürmesine ve tüm istemci JavaScript kodu içinde bulunan diğer web API'lerine belirteçler almalarına olanak tanır.

![Örtülü hibe akışının diyagramı](media/msal-authentication-flows/implicit-grant.svg)

Birçok modern web uygulamaları istemci tarafı, tek sayfa uygulamaları, JavaScript veya Açısal, Vue.js ve React.js gibi bir SPA çerçevesi kullanılarak yazılmış olarak inşa edilmiştir. Bu uygulamalar bir web tarayıcısında çalışır ve geleneksel sunucu tarafındaki web uygulamalarından farklı kimlik doğrulama özelliklerine sahiptir. Microsoft kimlik platformu, tek sayfalı uygulamaların kullanıcıları oturum açmasına ve örtülü hibe akışını kullanarak arka uç hizmetlerine veya web API'lerine erişmesini sağlar. Örtük akış, uygulamanın kimlik bilgilerini kimlik doğrulaması kullanıcıyı temsil etmesini ve korumalı API'leri aramak için gereken belirteçlere erişmesine olanak tanır.

Bu kimlik doğrulama akışı, yerel platformlarla etkileşim için daha fazla yetenek gerektirdiğinden, Elektron ve React-Native gibi platformlar arası JavaScript çerçevelerini kullanan uygulama senaryolarını içermez.

## <a name="authorization-code"></a>Yetkilendirme kodu

MSAL [OAuth 2 yetki kodu hibe](v2-oauth2-auth-code-flow.md)destekler. Bu hibe, web API'leri gibi korumalı kaynaklara erişmek için aygıta yüklenen uygulamalarda kullanılabilir. Bu, mobil ve masaüstü uygulamalarınıza oturum açma ve API erişimi eklemenize olanak tanır. 

Kullanıcılar web uygulamalarında (web sitelerinde) oturum açınca, web uygulaması bir yetkilendirme kodu alır.  Yetkilendirme kodu, web API'lerini aramak için bir belirteç elde etmek için itfa edilir. core web uygulamalarında ASP.NET ve ASP.NET'de tek amaç belirteç önbelleğine bir belirteç `AcquireTokenByAuthorizationCode` eklemektir. Belirteç daha sonra uygulama tarafından kullanılabilir (genellikle denetleyicileri, hangi sadece kullanarak `AcquireTokenSilent`bir API için bir belirteç olsun).

![Yetkilendirme kodu akışının diyagramı](media/msal-authentication-flows/authorization-code.png)

Önceki diyagramda, uygulama:

1. Erişim jetonu için kullanılan bir yetkilendirme kodu ister.
2. Web API'sını aramak için erişim belirteci kullanır.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- Bir belirteci kullanmak için yetkilendirme kodunu yalnızca bir kez kullanabilirsiniz. Aynı yetkilendirme koduyla birden çok kez belirteç elde etmeye çalışmayın (protokol standart belirtimi tarafından açıkça yasaktır). Kodu kasıtlı olarak birkaç kez kullandıysanız veya bir çerçevenin bunu sizin için de yaptığını fark etmediğiniz için aşağıdaki hatayı alırsınız:`AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- bir ASP.NET veya ASP.NET Core uygulaması yazıyorsanız, çerçeveye yetkilendirme kodunu zaten kullandığınızı söylemezseniz bu durum olabilir. Bunun için olay işleyicisi `context.HandleCodeRedemption()` `AuthorizationCodeReceived` yöntemini aramanız gerekir.

- Erişim belirtecisini, artımlı onamIzin doğru olmasını önleyebilecek ASP.NET paylaşmaktan kaçının. Daha fazla bilgi için [sorun #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693)bakın.

## <a name="on-behalf-of"></a>Adına

MSAL, [oAuth 2'yi kimlik doğrulama akışının adına](v2-oauth2-on-behalf-of-flow.md)destekler.  Bu akış, bir uygulama başka bir hizmeti veya web API'sını çağırması gereken bir hizmeti veya web API'sini çağırdığında kullanılır. Amaç, devredilen kullanıcı kimliğini ve izinleri istek zinciri aracılığıyla yaymaktır. Orta katman hizmetinin alt akış hizmetine kimlik doğrulama isteği nde bulunabilmesi için, kullanıcı adına Microsoft kimlik platformundan bir erişim belirteci sağlaması gerekir.

![Akış adına diyagramı](media/msal-authentication-flows/on-behalf-of.png)

Yukarıdaki diyagramda:

1. Uygulama web API için bir erişim belirteci kazanır.
2. İstemci (web, masaüstü, mobil veya tek sayfalı uygulama) korumalı bir web API'sını çağırır ve erişim belirteci, HTTP isteğinin kimlik doğrulama üstbilgisine taşıyıcı belirteci olarak ekler. Web API kullanıcının kimliğini doğrular.
3. İstemci web API'sini aradığında, web API kullanıcı adına başka bir belirteç ister.  
4. Korumalı web API'si, kullanıcı adına bir akış aşağı web API'sini çağırmak için bu belirteci kullanır.  Web API'si daha sonra diğer akış aşağı API'ler için belirteçler de isteyebilir (ancak yine de aynı kullanıcı adına).

## <a name="client-credentials"></a>İstemci kimlik bilgileri

MSAL [OAuth 2 istemci kimlik bilgileri akışını](v2-oauth2-client-creds-grant-flow.md)destekler. Bu akış, bir uygulamanın kimliğini kullanarak web tarafından barındırılan kaynaklara erişmenizi sağlar. Bu hibe türü genellikle, kullanıcıyla hemen etkileşim edilmeden arka planda çalışması gereken sunucudan sunucuya etkileşimler için kullanılır. Bu tür uygulamalar genellikle daemons veya hizmet hesapları olarak adlandırılır. 

Müşteri kimlik bilgileri, bir web hizmetinin (gizli bir istemci) başka bir web hizmetini ararken kimlik doğrulaması yapmak yerine, kullanıcı kimliği kullanmak yerine kendi kimlik bilgilerini kullanmasına izin verir. Bu senaryoda, istemci genellikle bir orta katman web hizmeti, bir daemon hizmeti veya bir web sitesidir. Daha yüksek bir güvence düzeyi için, Microsoft kimlik platformu arama hizmetinin bir sertifikayı (paylaşılan bir sır yerine) kimlik bilgisi olarak kullanmasına da izin verir.

> [!NOTE]
> Gizli istemci akışı mobil platformlarda (UWP, Xamarin.iOS ve Xamarin.Android) kullanılamaz, çünkü bunlar yalnızca genel istemci uygulamalarını destekler. Kamu istemcisi uygulamaları, uygulamanın kimliğini kimlik sağlayıcıya nasıl kanıtlayacaklarını bilmez. Bir sertifika dağıtılarak web uygulamasında veya web API arka uçlarında güvenli bir bağlantı sağlanabilir.

MSAL.NET iki tür istemci kimlik bilgilerini destekler. Bu istemci kimlik bilgilerinin Azure AD'ye kaydedilmesi gerekir. Kimlik bilgileri, kodunuzdaki gizli istemci uygulamasının kurucularına aktarılır.

### <a name="application-secrets"></a>Uygulama sırları

![Şifre ile gizli istemci diyagramı](media/msal-authentication-flows/confidential-client-password.png)

Önceki diyagramda, uygulama:

1. Uygulama gizli veya parola kimlik bilgilerini kullanarak bir belirteç kazanır.
2. Kaynağın istekte bulunmak için belirteci kullanır.

### <a name="certificates"></a>Sertifikalar

![Cert ile gizli istemci diyagramı](media/msal-authentication-flows/confidential-client-certificate.png)

Önceki diyagramda, uygulama:

1. Sertifika kimlik bilgilerini kullanarak bir belirteç kazanır.
2. Kaynağın istekte bulunmak için belirteci kullanır.

Bu istemci kimlik bilgileri:
- Azure AD'ye kayıtlı.
- Kodunuzda gizli istemci uygulamasının yapımında geçti.

## <a name="device-code"></a>Cihaz kodu

MSAL, kullanıcıların akıllı TV, IoT aygıtı veya yazıcı gibi giriş kısıtlamalı aygıtlarda oturum açmalarına olanak tanıyan [OAuth 2 cihaz kodu akışını](v2-oauth2-device-code.md)destekler. Azure AD ile etkileşimli kimlik doğrulaması için bir web tarayıcısı gerektirir. Aygıt kodu akışı, kullanıcının etkileşimli olarak oturum açmak için başka bir aygıt (örneğin, başka bir bilgisayar veya cep telefonu) kullanmasına olanak tanır ve burada aygıt veya işletim sistemi web tarayıcısı sağlamaz.

Uygulama, aygıt kodu akışını kullanarak, özellikle bu aygıtlar veya işletim sistemleri için tasarlanmış iki aşamalı bir işlem le belirteçler elde eder. Bu tür uygulamalara örnek olarak, IoT aygıtlarında veya komut satırı araçlarında (CLI) çalışanlar verilebilir. 

![Cihaz kodu akışının diyagramı](media/msal-authentication-flows/device-code.png)

Yukarıdaki diyagramda:

1. Kullanıcı kimlik doğrulaması gerektiğinde, uygulama bir kod sağlar ve kullanıcıdan URL'ye (örneğin) `https://microsoft.com/devicelogin`gitmek için başka bir aygıt (internete bağlı akıllı telefon gibi) kullanmasını ister. Daha sonra kullanıcıdan kodu girmesi istenir ve gerekirse onay istemleri ve çok faktörlü kimlik doğrulama dahil olmak üzere normal bir kimlik doğrulama deneyimi yle devam eder.

2. Başarılı bir kimlik doğrulaması üzerine, komut satırı uygulaması gerekli belirteçleri bir arka kanal üzerinden alır ve bunları ihtiyaç duyduğu web API çağrılarını gerçekleştirmek için kullanır.

### <a name="constraints"></a>Kısıtlamalar

- Aygıt kodu akışı yalnızca ortak istemci uygulamalarında kullanılabilir.
- Kamu müşteri başvurusu yaparken geçirilen yetki aşağıdakilerden biri olmalıdır:
  - Kiracı (kiracı kimliğini `https://login.microsoftonline.com/{tenant}/` `{tenant}` temsil eden GUID veya kiracı ile ilişkili bir etki alanı olan formun).
  - Herhangi bir iş ve`https://login.microsoftonline.com/organizations/`okul hesapları için ( ).
- Microsoft kişisel hesapları henüz Azure AD v2.0 bitiş noktası tarafından desteklenmez `/common` `/consumers` (kiracıları veya bunları kullanamazsınız).

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

MSAL, birleştirilmiş bir etki alanında çalışan veya Azure AD'nin Windows bilgisayarına katıldığı masaüstü veya mobil uygulamalar için Tümleşik Windows Kimlik Doğrulaması'nı (IWA) destekler. IWA kullanarak, bu uygulamalar sessizce (kullanıcıdan herhangi bir Kullanıcı etkileşimi olmadan) bir belirteç edinebilirsiniz. 

![Entegre Windows Kimlik Doğrulama Diyagramı](media/msal-authentication-flows/integrated-windows-authentication.png)

Önceki diyagramda, uygulama:

1. Tümleşik Windows Kimlik Doğrulaması'nı kullanarak bir belirteç kazanır.
2. Kaynağın istekte bulunmak için belirteci kullanır.

### <a name="constraints"></a>Kısıtlamalar

IWA yalnızca federe kullanıcıları destekler, yani Active Directory'de oluşturulan ve Azure AD tarafından desteklenen kullanıcılar. Active Directory desteği (yönetilen kullanıcılar) olmadan doğrudan Azure AD'da oluşturulan kullanıcılar bu kimlik doğrulama akışını kullanamaz. Bu sınırlama [kullanıcı adı/parola akışını](#usernamepassword)etkilemez.

IWA , .NET Framework, .NET Core ve Evrensel Windows Platformu platformları için yazılmış uygulamalar içindir.

IWA çok faktörlü kimlik doğrulamayı atlamaz. Çok faktörlü kimlik doğrulama yapılandırılırsa, çok faktörlü bir kimlik doğrulama zorluğu gerekiyorsa IWA başarısız olabilir. Çok faktörlü kimlik doğrulama, kullanıcı etkileşimi gerektirir.

Kimlik sağlayıcısının iki faktörlü kimlik doğrulamasının gerçekleştirilmesini istemesini denetlemezsiniz. Kiracı yönetici yapar. Genellikle, farklı bir ülkeden oturum açtığınızda, VPN ile kurumsal bir ağa bağlanmadığınızda ve bazen VPN üzerinden bağlandığınızda bile iki faktörlü kimlik doğrulama gerekir. Azure AD, iki faktörlü kimlik doğrulamanın gerekli olup olmadığını sürekli olarak öğrenmek için AI kullanır. IWA başarısız olursa, [etkileşimli kullanıcı istemi] (#interactive) geri dönmelisiniz.

Kamu müşteri başvurusu yaparken geçirilen yetki aşağıdakilerden biri olmalıdır:
- Kiracı (kiracı kimliğini `https://login.microsoftonline.com/{tenant}/` `tenant` veya kiracıyla ilişkili bir etki alanını temsil eden guid'in bulunduğu formun).
- Herhangi bir iş ve`https://login.microsoftonline.com/organizations/`okul hesapları için ( ). Microsoft kişisel hesapları desteklenmez (kullanamazsınız `/common` `/consumers` veya kiracıları).

IWA sessiz bir akış olduğundan, aşağıdakilerden biri doğru olmalıdır:
- Uygulamanızın kullanıcısı daha önce uygulamayı kullanmayı kabul etmiş olmalıdır. 
- Kiracı yöneticinin daha önce kiracıdaki tüm kullanıcıların uygulamayı kullanmasına izin vermiş olması gerekir.

Bu, aşağıdakilerden birinin doğru olduğu anlamına gelir:
- Geliştirici olarak Azure portalında Azure portalında **Grant'i** kendiniz seçtiniz.
- Kiracı yönetici, uygulama için kaydın **API izinleri** sekmesinde **{kiracı etki alanı} için Yönetici Onayı Ver/iptal** etti (bkz. [web API'lerine erişmek için izin ekle).](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)
- Kullanıcıların uygulamayı kabul etmesi için bir yol sağladınız (bkz. [bireysel kullanıcı onayı isteme).](v2-permissions-and-consent.md#requesting-individual-user-consent)
- Kiracı yöneticinin başvuruyu onaylaması için bir yol sağladınız (bkz. [yönetici onayı).](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)

.NET masaüstü, .NET Core ve Windows Evrensel Platform uygulamaları için IWA akışı etkinleştirilir. .NET Core'da kullanıcı adını IWA'ya sağlamanız gerekir, çünkü .NET Core işletim sisteminden kullanıcı adı alamaz.
  
Rıza hakkında daha fazla bilgi için [v2.0 izinlerine ve onaya](v2-permissions-and-consent.md)bakın.

## <a name="usernamepassword"></a>Kullanıcı adı/parola

MSAL, bir uygulamanın parolalarını doğrudan işleyerek kullanıcıda oturum açmasına olanak tanıyan [OAuth 2 kaynak sahibi parola kimlik bilgilerini](v2-oauth-ropc.md)destekler. Masaüstü uygulamanızda, sessizce bir belirteç elde etmek için kullanıcı adı/parola akışını kullanabilirsiniz. Uygulamayı kullanırken kullanıcı cayması gerekmez.

![Kullanıcı adı/parola akışının diyagramı](media/msal-authentication-flows/username-password.png)

Önceki diyagramda, uygulama:

1. Kullanıcı adı ve parolayı kimlik sağlayıcısına göndererek bir belirteç elde eder.
2. Belirteci kullanarak bir web API çağırır.

> [!WARNING]
> Bu akış önerilmez. Bu güven ve kullanıcı maruz kalma yüksek derecede gerektirir.  Bu akışı yalnızca diğer, daha güvenli, akışlar kullanılamıyorduğunda kullanmalısınız. Daha fazla bilgi için, [büyüyen parola sorununun çözümü nedir?](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/) 

Windows etki alanı birleştirilmiş makinelerde sessizce bir belirteç elde etmek için tercih edilen akış [Tümleşik Windows Kimlik Doğrulama.](#integrated-windows-authentication) Aksi takdirde, [Aygıt kodu akışını](#device-code)da kullanabilirsiniz.

Bu bazı durumlarda yararlı olsa da (DevOps senaryoları), kendi kullanıcı arabirimi sağlamak etkileşimli senaryolarda kullanıcı adı/ parola kullanmak istiyorsanız, bunu önlemek için deneyin. Kullanıcı adı/şifre kullanarak:
- Çok faktörlü kimlik doğrulaması yapması gereken kullanıcılar oturum açamaz (etkileşim olmadığı için).
- Kullanıcılar tek bir oturum açamaz.

### <a name="constraints"></a>Kısıtlamalar

[Tümleşik Windows Kimlik Doğrulama kısıtlamaları](#integrated-windows-authentication)dışında, aşağıdaki kısıtlamalar da geçerlidir:

- Kullanıcı adı/parola akışı Koşullu Erişim ve çok faktörlü kimlik doğrulama ile uyumlu değildir. Sonuç olarak, uygulamanız, kiracı yöneticisinin çok faktörlü kimlik doğrulaması gerektirdiği bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bunu yapar.
- Yalnızca iş ve okul hesapları için çalışır (Microsoft hesapları için değil).
- Akış .NET masaüstü ve .NET Core'da kullanılabilir, ancak Evrensel Windows Platformu'nda kullanılamaz.

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C özellikleri

MSAL.NET ve Azure AD B2C kullanma hakkında daha fazla bilgi [MSAL.NET](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c)için bkz.
