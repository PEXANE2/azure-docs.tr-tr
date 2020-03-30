---
title: Microsoft kimlik platformu için uygulama türleri | Azure
description: Microsoft kimlik platformu (v2.0) bitiş noktası tarafından desteklenen uygulama ve senaryo türleri.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/06/2019
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 94cddf097f2a9e51f061909f6bdd3dcd82f18bfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262535"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Microsoft kimlik platformu için uygulama türleri

Microsoft kimlik platformu (v2.0) bitiş noktası, hepsi endüstri standardı protokolleri [OAuth 2.0 veya OpenID Connect'e](active-directory-v2-protocols.md)dayalı olarak çeşitli modern uygulama mimarileri için kimlik doğrulamasını destekler. Bu makalede, tercih ettiğiniz dil veya platformdan bağımsız olarak Microsoft kimlik platformunu kullanarak oluşturabileceğiniz uygulama türleri açıklanmaktadır. Bilgiler, [kodla çalışmaya başlamadan](v2-overview.md#getting-started)önce üst düzey senaryoları anlamanıza yardımcı olmak için tasarlanmıştır.

> [!NOTE]
> Microsoft kimlik platformu bitiş noktası, tüm Azure Etkin Dizin (Azure AD) senaryolarını ve özelliklerini desteklemez. Microsoft kimlik platformu bitiş noktasını kullanıp kullanmadığınızı belirlemek için [Microsoft kimlik platformu sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

## <a name="the-basics"></a>Temel bilgiler

Microsoft kimlik platformu bitiş noktasını kullanan her uygulamayı yeni [Uygulama kayıtları portalına](https://go.microsoft.com/fwlink/?linkid=2083908)kaydetmeniz gerekir. Uygulama kayıt işlemi, uygulamanız için bu değerleri toplar ve atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama (istemci) kimliği**
* Yanıtları uygulamanıza yönlendirmek için kullanabileceğiniz bir **Yönlendirme URI**
* Desteklenen hesap türleri gibi birkaç senaryoya özgü değer

Ayrıntılar [için, bir uygulamayı](quickstart-register-app.md)nasıl kaydedin öğrenin.

Uygulama kaydedildikten sonra, uygulama son noktaya istekler göndererek Microsoft kimlik platformu ile iletişim kurar. Bu isteklerin ayrıntılarını işleyen açık kaynak çerçeveleri ve kitaplıklar salıyoruz. Ayrıca, bu uç noktalara istekler oluşturarak kimlik doğrulama mantığını kendiniz uygulama seçeneğiniz de vardır:

```HTTP
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>Tek sayfalı uygulamalar (JavaScript)

Birçok modern uygulama öncelikle JavaScript yazılmış bir tek sayfalı uygulama ön uç var. Genellikle, Açısal, React veya Vue gibi bir çerçeve kullanılarak yazılır. Microsoft kimlik platformu bitiş [noktası, OAuth 2.0 örtük akışını](v2-oauth2-implicit-grant-flow.md)kullanarak bu uygulamaları destekler.

Bu akışta, uygulama herhangi bir sunucu-sunucu alışverişi olmadan, bitiş noktası yetkilendirme microsoft kimlik platformudoğrudan belirteçleri alır. Tüm kimlik doğrulama mantığı ve oturum işleme tamamen JavaScript istemcisinde, ekstra sayfa yönlendirmeleri olmadan gerçekleşir.

![Örtük kimlik doğrulama akışını gösterir](./media/v2-app-types/convergence-scenarios-implicit.svg)

Bu senaryoyu iş başında görmek için, [Microsoft kimlik platformundaki](v2-overview.md#getting-started) tek sayfalık uygulama kodu örneklerinden birini deneyin.

## <a name="web-apps"></a>Web uygulamaları

Kullanıcının tarayıcı üzerinden erişebildiği web uygulamaları (.NET, PHP, Java, Ruby, Python, Node) için [openid connect](active-directory-v2-protocols.md) kullanıcı oturum açmak için kullanabilirsiniz. OpenID Connect'te web uygulaması bir kimlik belirteci alır. Kimlik belirteci, kullanıcının kimliğini doğrulayan ve kullanıcı hakkında talep biçiminde bilgi sağlayan bir güvenlik belirtecidir:

```JSON
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Microsoft kimlik platformu bitiş noktasında kullanılan farklı belirteç türlerinin daha ayrıntılı ayrıntıları [erişim belirteç](access-tokens.md) başvurusu ve [id_token referans](id-tokens.md) mevcuttur

Web sunucusu uygulamalarında oturum açma kimlik doğrulama akışı şu üst düzey adımları atar:

![Web uygulaması kimlik doğrulama akışını gösterir](./media/v2-app-types/convergence-scenarios-webapp.svg)

Kimlik belirteciyle Microsoft kimlik platformu bitiş noktasından alınan ortak imza anahtarıyla doğrulayarak kullanıcının kimliğini sağlayabilirsiniz. Sonraki sayfa isteklerinde kullanıcıyı tanımlamak için kullanılabilecek bir oturum çerezi ayarlanır.

Bu senaryoyu iş başında görmek için, [Microsoft kimlik platformundaki](v2-overview.md#getting-started) web uygulaması oturum açma kodu örneklerinden birini deneyin.

Basit oturum açmanın yanı sıra, bir web sunucusu uygulamasının REST API gibi başka bir web hizmetine erişmeleri gerekebilir. Bu durumda, web sunucusu uygulaması [OAuth 2.0 yetkilendirme kodu akışını](active-directory-v2-protocols.md)kullanarak, birleşik bir OpenID Connect ve OAuth 2.0 akışı yürüter. Bu senaryo hakkında daha fazla bilgi [için, web uygulamaları ve Web API'ları ile başlamak](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)hakkında okuyun.

## <a name="web-apis"></a>Web API'leri

Uygulamanızın yenilenebilir Web API'si gibi web hizmetlerini güvenli hale getirmek için Microsoft kimlik platformu bitiş noktasını kullanabilirsiniz. Web API'leri çok sayıda platform ve dilde uygulanabilir. Azure İşletmeleri'nde HTTP Tetikleyicileri kullanılarak da uygulanabilirler. Kimlik belirteçleri ve oturum çerezleri yerine, Web API'si verilerini güvence altına almak ve gelen istekleri doğrulamak için Bir OAuth 2.0 erişim belirteci kullanır. Bir Web API'sini arayan kişi, bir HTTP isteğinin yetkilendirme üstbilgisinde aşağıdaki gibi bir erişim belirteci ekler:

```HTTP
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API, API arayanın kimliğini doğrulamak ve erişim belirtecinde kodlanmış olan taleplerden arayan hakkında bilgi ayıklamak için erişim belirteci kullanır. Microsoft kimlik platformu bitiş noktasında kullanılan farklı belirteç türlerinin daha ayrıntılı [ayrıntıları, access token](access-tokens.md) başvurusu ve [id_token](id-tokens.md) başvuruda kullanılabilir.

Web API, kullanıcılara [kapsam](v2-permissions-and-consent.md)olarak da bilinen izinleri açığa çıkararak belirli işlevleri veya verileri devre dışı bırakma veya devre dışı bırakma gücü verebilir. Bir arama uygulamasının kapsam için izin alabilmesi için, kullanıcının akış sırasında kapsamı kabul etmesi gerekir. Microsoft kimlik platformu bitiş noktası kullanıcıdan izin ister ve ardından Web API'nın aldığı tüm erişim belirteçlerinde izinleri kaydeder. Web API, her çağrıda aldığı erişim belirteçlerini doğrular ve yetkilendirme denetimleri gerçekleştirir.

Web API'si, web sunucusu uygulamaları, masaüstü ve mobil uygulamalar, tek sayfalı uygulamalar, sunucu tarafındaki daemon'lar ve hatta diğer Web API'leri dahil olmak üzere her tür uygulamadan erişim belirteçleri alabilir. Bir Web API'si için üst düzey akış aşağıdaki gibi görünür:

![Web API kimlik doğrulama akışını gösterir](./media/v2-app-types/convergence-scenarios-webapi.svg)

OAuth2 erişim belirteçlerini kullanarak Bir Web API'sini nasıl güvenli hale getirebilirsiniz öğrenmek için, [Microsoft kimlik platformundaki](v2-overview.md#getting-started) Web API kod örneklerine göz atın.

Çoğu durumda, web API'leri de Microsoft kimlik platformu tarafından güvenli diğer downstream web API'lerine giden isteklerde bulunmak gerekir. Bunu yapmak için, web API'ler, web API'nin gelen erişim jetonunu giden isteklerde kullanılmak üzere başka bir erişim belirteciyle değiştirmesine olanak tanıyan **On-Behalf-Of** akışından yararlanabilir. Daha fazla bilgi için [Microsoft kimlik platformu ve OAuth 2.0 On-Behalf-Of akışına](v2-oauth2-on-behalf-of-flow.md)bakın.

## <a name="mobile-and-native-apps"></a>Mobil ve yerel uygulamalar

Mobil ve masaüstü uygulamaları gibi aygıtyüklü uygulamaların genellikle verileri depolayan ve kullanıcı adına işlevleri gerçekleştiren arka uç hizmetlere veya Web API'lerine erişmeleri gerekir. Bu [uygulamalar, OAuth 2.0 yetkilendirme kodu akışını](v2-oauth2-auth-code-flow.md)kullanarak arka uç hizmetlerine oturum açma ve yetkilendirme ekleyebilir.

Bu akışta, kullanıcı giriş yaptığında uygulama Microsoft kimlik platformu bitiş noktasından bir yetkilendirme kodu alır. Yetkilendirme kodu, uygulamanın oturum açmış olan kullanıcı adına arka uç hizmetleri arama iznini temsil eder. Uygulama, arka plandaki yetkilendirme kodunu OAuth 2.0 erişim jetonu ve yenileme belirteciyle değiştirebilir. Uygulama, http isteklerinde Web API'lerine kimlik doğrulamak için erişim belirteci'ni kullanabilir ve eski erişim belirteçlerinin süresi dolduğunda yeni erişim belirteçleri almak için yeni belirteci kullanabilir.

![Yerel uygulama kimlik doğrulama akışını gösterir](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>Daemons ve sunucu tarafı uygulamaları

Uzun süren süreçleri olan veya kullanıcıyla etkileşim olmadan çalışan uygulamaların, Web API'leri gibi güvenli kaynaklara erişmek için de bir yol göstermesi gerekir. Bu uygulamalar, OAuth 2.0 istemci kimlik bilgileri akışıyla, kullanıcının temsilci kimliği yerine uygulamanın kimliğini kullanarak kimlik doğrulaması yapabilir ve belirteçleri alabilir. Bir istemci sırrı veya sertifika kullanarak uygulamanın kimliğini kanıtlayabilirsiniz. Daha fazla bilgi için [Microsoft kimlik platformlarını kullanarak .NET Core daemon konsol uygulamasına](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2)bakın.

Bu akışta, uygulama erişim elde `/token` etmek için bitiş noktası ile doğrudan etkileşime:

![Daemon uygulaması kimlik doğrulama akışını gösterir](./media/v2-app-types/convergence-scenarios-daemon.svg)

Daemon uygulaması oluşturmak için [istemci kimlik bilgileri belgelerine](v2-oauth2-client-creds-grant-flow.md)bakın veya [bir .NET örnek uygulamasını](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)deneyin.
