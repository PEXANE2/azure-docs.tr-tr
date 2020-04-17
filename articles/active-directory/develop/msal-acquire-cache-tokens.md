---
title: MSAL ile & önbellek belirteçleri edinin | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı'nı (MSAL) kullanarak belirteçler edinme ve önbelleğe alma hakkında bilgi edinin.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 647dff9e6401322371ef795a25ca5ced2b517e9c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81534593"
---
# <a name="acquire-and-cache-tokens-using-the-microsoft-authentication-library-msal"></a>Microsoft kimlik doğrulama kitaplığını (MSAL) kullanarak belirteçleri edinme ve önbellek

[Erişim belirteçleri, istemcilerin](access-tokens.md) Azure tarafından korunan web API'lerini güvenli bir şekilde aramasını sağlar. Microsoft Kimlik Doğrulama Kitaplığı 'nı (MSAL) kullanarak bir belirteç edinmenin birçok yolu vardır. Bazı yollar bir web tarayıcısı üzerinden kullanıcı etkileşimleri gerektirir. Bazıları kullanıcı etkileşimi gerektirmez. Genel olarak, bir belirteç edinme nin yolu, uygulamanın bir kamu istemcisi uygulaması (masaüstü veya mobil uygulama) veya gizli bir istemci uygulaması (web uygulaması, web API veya Windows hizmeti gibi daemon uygulaması) olup olmadığını bağlıdır.

MSAL satın alındıktan sonra bir belirteci önbelleğe alınr.  Uygulama kodu, başka yollarla bir belirteç edinmeden önce, önce sessizce (önbellekten) bir belirteç almaya çalışmalıdır.

Ayrıca, hesapları önbellekten kaldırarak elde edilen belirteç önbelleğini de temizleyebilirsiniz. Bu olsa da, tarayıcıda oturum çerez kaldırmaz.

## <a name="scopes-when-acquiring-tokens"></a>Belirteçleri edinirken kapsamlar

[Kapsamlar,](v2-permissions-and-consent.md) istemci uygulamalarının erişim istemesi için bir web API'sinin ortaya çıkardığı izinlerdir. İstemci uygulamaları, web API'lerine erişmek için belirteçleri almak için kimlik doğrulama istekleri yaparken bu kapsamlar için kullanıcının onayını ister. MSAL, geliştiriciler için Azure AD (v1.0) ve Microsoft kimlik platformu (v2.0) API'lerine erişmek için belirteçler elde etmenizi sağlar. v2.0 protokolü isteklerde kaynak yerine kapsamları kullanır. Daha fazla bilgi için [v1.0 ve v2.0 karşılaştırmasını](active-directory-v2-compare.md)okuyun. Web API'nin kabul edilen belirteç sürümünü yapılandırmasına bağlı olarak, v2.0 bitiş noktası erişim belirtecisini MSAL'a döndürür.

MSAL edinme belirteç yöntemlerinin bir dizi *kapsam* parametresi gerektirir. Bu parametre, istenen izinleri ve kaynakları bildiren dizelerin basit bir listesidir. İyi bilinen kapsamlar [Microsoft Graph izinleridir.](/graph/permissions-reference)

MSAL'da v1.0 kaynaklarına erişmek de mümkündür. Daha fazla bilgi [için, v1.0 uygulaması için Kapsamlar'ı](msal-v1-app-scopes.md)okuyun.

### <a name="request-specific-scopes-for-a-web-api"></a>Web API'sı için belirli kapsamlar isteme

Uygulamanızın kaynak API'si için belirli izinlere sahip belirteçler istemesi gerektiğinde, API'nin uygulama kimliği URI'sini içeren kapsamları aşağıdaki biçimde geçirmeniz gerekir: * &lt;uygulama Kimliği URI&gt;/&lt;kapsamı&gt;*

Örneğin, Microsoft Graph API için kapsamlar:`https://graph.microsoft.com/User.Read`

Veya, örneğin, özel bir web API için kapsamlar:`api://abscdefgh-1234-abcd-efgh-1234567890/api.read`

Microsoft Graph API için yalnızca, `user.read` biçimlendirilecek `https://graph.microsoft.com/User.Read` bir kapsam değeri eşlemleri ve birbirinin yerine kullanılabilir.

> [!NOTE]
> Azure Kaynak Yöneticisi API'sihttps://management.core.windows.net/) gibi belirli web API'leri ( hedef kitlenin erişim jetonuna (aud) sahip olduğu bir '/' beklentisi içindedir. Bu durumda, belirteç API https://management.core.windows.net//user_impersonation geçerli olması için kapsam (çift eğik çizgi not) olarak geçmek önemlidir.

### <a name="request-dynamic-scopes-for-incremental-consent"></a>Artımlı onay için dinamik kapsamlar isteme

v1.0 kullanarak uygulama yaparken, kullanıcının oturum açma sırasında onay alması için uygulamanın gerektirdiği tüm izinleri (statik kapsamlar) kaydetmeniz gerekir. v2.0'da, kapsam parametresini kullanarak gerektiğinde ek izinler isteyebilirsiniz. Bunlara dinamik kapsamlar denir ve kullanıcının kapsamlara artımlı onay sağlamasına izin verir.

Örneğin, başlangıçta kullanıcıoturum açabilir ve herhangi bir erişim inkar edebilirsiniz. Daha sonra, edinme belirteç yöntemlerinde takvim kapsamını isteyerek kullanıcının takvimini okuma ve kullanıcının onayını alma olanağı verebilirsiniz.

Örneğin: `https://graph.microsoft.com/User.Read` ve`https://graph.microsoft.com/Calendar.Read`

## <a name="acquiring-tokens-silently-from-the-cache"></a>Belirteçleri sessizce edinme (önbellekten)

MSAL bir belirteç önbelleği (veya gizli istemci uygulamaları için iki önbellek) tutar ve satın alındıktan sonra bir belirteç önbelleğe gelir.  Çoğu durumda, sessizce bir belirteç almaya çalışırken önbellekte bir belirteç dayalı daha fazla kapsamları ile başka bir belirteç elde edecektir. Ayrıca, son kullanma tarihi yaklaştıkça bir belirteci yenileme yeteneğine sahiptir (belirteç önbelleği de yenileme belirteci içerdiğinden).

### <a name="recommended-call-pattern-for-public-client-applications"></a>Kamu istemcisi uygulamaları için önerilen çağrı deseni

Uygulama kodu, önce sessizce (önbellekten) bir belirteç almaya çalışmalıdır.  Yöntem çağrısı bir "UI gerekli" hata veya özel durum döndürür, başka yollarla bir belirteç edinmeyi deneyin.

Ancak, önce sessizce bir belirteç elde etmeye **çalışmamanız gereken** iki akış vardır:

- kullanıcı belirteç önbelleğini kullanmayan istemci [kimlik bilgileri akışı,](msal-authentication-flows.md#client-credentials)ancak bir uygulama belirteç önbelleği. Bu yöntem, STS'ye bir istek göndermeden önce bu uygulama belirteç önbelleğini doğrulamayı dikkate alır.
- uygulamanın kullanıcıya oturum açma ve daha fazla kapsam için onay ını alarak aldığı bir kodu kullandığı ndan, web uygulamalarında [yetkilendirme kodu akışı.](msal-authentication-flows.md#authorization-code) Bir kod hesap olarak değil, parametre olarak geçirildiği için, yöntem kodu kullanmadan önce önbelleğe bakamaz ve bu da yine de hizmete çağrı gerektirir.

### <a name="recommended-call-pattern-in-web-apps-using-the-authorization-code-flow"></a>Yetkilendirme Kodu akışını kullanarak web uygulamalarında önerilen çağrı deseni

[OpenID Connect yetkilendirme kodu akışını](v2-protocols-oidc.md)kullanan Web uygulamaları için denetleyicilerde önerilen desen aşağıdakileri yapmaktır:

- Gizli bir istemci uygulamasını, özelleştirilmiş serileştirme yle belirteç önbelleğiyle anında leştirin.
- Yetkilendirme kodu akışını kullanarak belirteci edinme

## <a name="acquiring-tokens"></a>Belirteçlerin edinilmesi

Genellikle, bir belirteç edinme yöntemi, bir kamu istemcisi veya gizli istemci uygulaması olup olmadığına bağlıdır.

### <a name="public-client-applications"></a>Kamu müşteri başvuruları

Genel istemci uygulamaları (masaüstü veya mobil uygulama) için şunları yapıyorsuniz:
- Kullanıcının kullanıcı veya açılır pencere üzerinden oturum açmasını sağlamak için genellikle etkileşimli olarak belirteçler edinin.
- Masaüstü uygulaması bir etki alanına veya Azure'a katılan bir Windows bilgisayarında çalışıyorsa, Tümleşik Windows Kimlik Doğrulama (IWA/Kerberos) kullanarak [oturum açmış kullanıcı için sessizce bir belirteç alabilir.](msal-authentication-flows.md#integrated-windows-authentication)
- .NET framework masaüstü istemci uygulamalarında [kullanıcı adı ve parola içeren bir belirteç alabilirsiniz,](msal-authentication-flows.md#usernamepassword) ancak bu önerilmez. Gizli istemci uygulamalarında kullanıcı adı/şifre kullanmayın.
- Web tarayıcısı olmayan cihazlarda çalışan uygulamalarda [cihaz kodu akışı](msal-authentication-flows.md#device-code) üzerinden bir belirteç elde edebilirsiniz. Kullanıcıya bir URL ve kod sağlanır, bu da daha sonra başka bir cihazdaki bir web tarayıcısına gider ve kodu girer ve kodu girer.  Azure AD daha sonra tarayıcısız aygıta bir belirteç gönderir.

### <a name="confidential-client-applications"></a>Gizli müşteri uygulamaları

Gizli istemci uygulamaları (web uygulaması, web API veya Windows hizmeti gibi daemon uygulaması) için:
- [İstemci kimlik bilgilerini kullanarak](msal-authentication-flows.md#client-credentials)kullanıcı için değil, **uygulamanın kendisi için** belirteçler edinin. Bu, belirli bir kullanıcıyı değil, kullanıcıları genel olarak işleyen araçları veya araçları eşitleme için kullanılabilir.
- Kullanıcı adına BIR API çağırmak için bir web API'si için [On-behalf akışını](msal-authentication-flows.md#on-behalf-of) kullanın. Uygulama, kullanıcı iddiasına (örneğin SAML veya JWT belirteci) dayalı bir belirteç elde etmek için istemci kimlik bilgileriyle tanımlanır. Bu akış, hizmet-servis aramalarında belirli bir kullanıcının kaynaklarına erişimi gereken uygulamalar tarafından kullanılır.
- Kullanıcı yetkilendirme isteği URL'si aracılığıyla giriş yaptıktan sonra web uygulamalarındaki [yetkilendirme kodu akışını](msal-authentication-flows.md#authorization-code) kullanarak belirteçler edinin. OpenID Connect uygulaması genellikle bu mekanizmayı kullanır ve bu mekanizma, kullanıcının Açık Kimlik'i kullanarak oturum açmasına ve ardından kullanıcı adına web API'lere erişmesini sağlar.

## <a name="authentication-results"></a>Kimlik doğrulama sonuçları

Müşteriniz bir erişim jetonu istediğinde, Azure AD erişim belirteciyle ilgili bazı meta verileri içeren bir kimlik doğrulama sonucu da verir. Bu bilgiler, erişim belirtecinin son kullanma süresini ve geçerli olduğu kapsamları içerir. Bu veriler, uygulamanızın erişim belirteci girişini ayrıştırmak zorunda kalmadan akıllı erişim belirteçleri önbelleğe alma masına olanak tanır.  Kimlik doğrulama sonucu şunları ortaya çıkarır:

- Kaynaklara erişmek için web API'sinin [erişim belirteci.](access-tokens.md) Bu bir dize, genellikle bir base64 kodlanmış JWT ama istemci erişim belirteci içinde bakmak asla. Biçimkararlı kalması garanti değildir ve kaynak için şifrelenebilir. İstemci deki erişim belirteç içeriğine bağlı olarak kod yazan kişiler, hataların ve istemci mantığının kırılmalarının en büyük kaynaklarından biridir.
- Kullanıcı için [kimlik belirteci](id-tokens.md) (bu bir JWT' dir).
- Belirteç süresinin dolduğu tarihi/saati söyleyen belirteç süresi.
- Kiracı kimliği, kullanıcının bulunduğu kiracıyı içerir. Konuk kullanıcılar için (Azure AD B2B senaryoları), kiracı kimliği tekil kiracı değil, konuk kiracıdır. Belirteç bir kullanıcı adına teslim edildiğinde, kimlik doğrulama sonucu da bu kullanıcı hakkında bilgi içerir. Jetonların kullanıcı olmadan talep edildiği gizli istemci akışları için (uygulama için), bu kullanıcı bilgileri geçersizdir.
- Belirteci verilen kapsamlar.
- Kullanıcı için benzersiz kimlik.

## <a name="next-steps"></a>Sonraki adımlar

Java için MSAL kullanıyorsanız, [Java için MSAL'da Özel belirteç önbelleği serileştirme](msal-java-token-cache-serialization.md)si hakkında bilgi edinin.

İşlem [hataları ve özel durumlar](msal-handling-exceptions.md)hakkında bilgi edinin.
