---
title: Azure AD B2C tarafından desteklenen uygulama türleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C ile kullanabileceğiniz uygulama türleri hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e8328db12bde531c2e27936c09247611ff1a3583
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190152"
---
# <a name="application-types-that-can-be-used-in-active-directory-b2c"></a>Active Directory B2C'de kullanılabilecek uygulama türleri

Azure Active Directory B2C (Azure AD B2C), çeşitli modern uygulama mimarileri için kimlik doğrulamayı destekler. Bunların tümü [OAuth 2.0](protocols-overview.md) veya [OpenID Connect](protocols-overview.md) endüstri standardı protokollerine dayalıdır. Bu makalede, tercih ettiğiniz dil veya platformdan bağımsız olarak oluşturabileceğiniz uygulama türleri açıklanmaktadır. Uygulama oluşturmaya başlamadan önce üst düzey senaryoları anlamanıza da yardımcı olur.

Azure AD B2C kullanan her uygulama, [Azure portalını](https://portal.azure.com/)kullanarak [Azure AD B2C kiracınızda](tutorial-create-tenant.md) kayıtlı olmalıdır. Başvuru kayıt işlemi aşağıdakiler gibi değerleri toplar ve atar:

* Uygulamanızı benzersiz olarak tanımlayan bir **Uygulama Kimliği.**
* Yanıtları uygulamanıza yönlendirmek için kullanılabilecek bir Yanıt URL'si. **Reply URL**

Azure AD B2C'ye gönderilen her istek, bir **kullanıcı akışı** (yerleşik bir ilke) veya Azure AD B2C'nin davranışını kontrol eden özel bir **ilke** belirtir. Her iki ilke türü de son derece özelleştirilebilir bir kullanıcı deneyimi kümesi oluşturmanıza olanak tanır.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, kullanıcıyı bir [ilke](user-flow-overview.md)yürütmek için v2.0 bitiş noktasına yönlendirir.
2. Kullanıcı, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama v2.0 bitiş noktasından bir güvenlik belirteci alır.
4. Uygulama, korumalı bilgilere veya korumalı bir kaynağa erişmek için güvenlik belirteci kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama, güvenlik belirtecini düzenli aralıklarla yeniler.

Bu adımlar, oluşturmakta olduğunuz uygulama türüne göre biraz farklılık görebilir.

## <a name="web-applications"></a>Web uygulamaları

Bir sunucuda barındırılan ve tarayıcı üzerinden erişilen web uygulamaları (.NET, PHP, Java, Ruby, Python ve Node.js dahil) için Azure AD B2C, tüm kullanıcı deneyimleri için [OpenID Connect'i](protocols-overview.md) destekler. OpenID Connect'in Azure AD B2C uygulamasında, web uygulamanız Azure AD'ye kimlik doğrulama istekleri vererek kullanıcı deneyimleri başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci, kullanıcının kimliğini temsil eder. Ayrıca kullanıcı hakkındaki bilgileri talep biçiminde sağlar:

```json
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

[Azure AD B2C belirteç başvurusunda](tokens-overview.md)bir uygulamanın kullanabileceği belirteç ve talep türleri hakkında daha fazla bilgi edinin.

Bir web uygulamasında, bir [ilkenin](user-flow-overview.md) her yürütülmesi şu üst düzey adımları alır:

1. Kullanıcı web uygulamasına göz atar.
2. Web uygulaması, kullanıcıyı yürütülecek ilkeyi belirten Azure AD B2C'ye yönlendirir.
3. Kullanıcı ilkeyi tamamlar.
4. Azure AD B2C `id_token` tarayıcıya bir şey döndürür.
5. Yeniden `id_token` yönlendirme URI'ye nakledilir.
6. Doğrulanır `id_token` ve oturum çerezi ayarlanır.
7. Güvenli bir sayfa kullanıcıya döndürülür.

Azure AD'den alınan bir ortak imzalama anahtarı kullanarak `id_token` doğrulaması yapma, kullanıcının kimliğini doğrulamak için yeterlidir. Bu işlem, sonraki sayfa isteklerinde kullanıcıyı tanımlamak için kullanılabilecek bir oturum çerezi de ayarlar.

Bu senaryoyu iş başında görmek [için, Başlangıç bölümümüzdeweb](overview.md)uygulaması oturum açma kodu örneklerinden birini deneyin.

Basit oturum açmayı kolaylaştırmaya ek olarak, bir web sunucusu uygulamasının da arka uçtaki bir web hizmetine erişmesi gerekebilir. Bu durumda, web uygulaması biraz farklı bir [OpenID Connect akışı](openid-connect.md) gerçekleştirebilir ve yetkilendirme kodları kullanarak belirteçleri elde edebilir ve belirteçleri yenilemek. Bu senaryo, aşağıdaki [Web API'leri bölümünde](#web-apis) belirtilmiştir.

## <a name="web-apis"></a>Web API'leri

Uygulamanızın restful web API'si gibi web hizmetlerini güvence altına almak için Azure AD B2C'yi kullanabilirsiniz. Web API’leri belirteçleri kullanarak gelen HTTP isteklerinin kimliğini doğrulama yoluyla verilerinin güvenliğini sağlamak üzere OAuth 2.0 kullanabilir. Web API'si çağıranı, HTTP isteğinin yetkilendirme üst bilgisine bir belirteç ekler:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API'si daha sonra, API çağıranının kimliğini doğrulamak ve belirteçte kodlanmış taleplerden çağıran hakkında bilgi ayıklamak için belirteci kullanabilir. [Azure AD B2C belirteç başvurusunda](tokens-overview.md) bir uygulama için kullanılabilir talepler ve belirteç türleri hakkında bilgi edinin.

Web API'si, web uygulamaları, masaüstü ve mobil uygulamalar, tek sayfa uygulamaları, sunucu tarafındaki daemon'lar ve diğer web API'leri de dahil olmak üzere birçok istemci türünden belirteçler alabilir. Aşağıda, web API çağıran bir web uygulaması için tam akış örneği verilmiştir:

1. Web uygulaması bir ilke yürütür ve kullanıcı kullanıcı deneyimini tamamlar.
2. Azure AD B2C tarayıcıya bir `id_token` (OpenID Connect) ve yetkilendirme kodu döndürür.
3. Tarayıcı, yetkilendirme `id_token` kodunu yeniden yönlendirme URI'ye gönderir.
4. Web sunucusu doğrular `id_token` ve bir oturum çerez ayarlar.
5. Web sunucusu, azure AD B2C'den yetkilendirme kodu, uygulama istemci kimliği ve istemci kimlik bilgilerini sağlayarak bir `access_token` soru sorar.
6. ve `access_token` `refresh_token` web sunucusuna döndürülür.
7. Web API bir yetkilendirme `access_token` üstbilgisinde denir.
8. Web API belirteci doğrular.
9. Güvenli veriler web uygulamasına döndürülür.

Yetkilendirme kodları, yenileme belirteçleri ve belirteç alma adımları hakkında daha fazla bilgi edinmek için [OAuth 2.0 protokolünü](authorization-code-flow.md) okuyun.

Azure AD B2C'yi kullanarak bir web API'sini nasıl güvence altına alacağınızı öğrenmek için [Başlarken bölümümüzdeki](overview.md) web API'si eğitimlerine bakın.

## <a name="mobile-and-native-applications"></a>Mobil ve yerel uygulamalar

Mobil ve masaüstü uygulamaları gibi aygıtlara yüklenen uygulamaların genellikle kullanıcılar adına arka uç hizmetlerine veya web API'lerine erişmeleri gerekir. Azure AD B2C ve [OAuth 2.0 yetkilendirme kodu akışını](authorization-code-flow.md)kullanarak yerel uygulamalarınıza özelleştirilmiş kimlik yönetimi deneyimleri ekleyebilir ve arka uç hizmetlerini güvenli bir şekilde arayabilirsiniz.

Bu akışta, uygulama [ilkeleri](user-flow-overview.md) yürütür `authorization_code` ve kullanıcı ilkeyi tamamladıktan sonra Azure AD'den bir ad alır. Uygulamanın, `authorization_code` şu anda oturum açmış olan kullanıcı adına arka uç hizmetleri arama iznini temsil eder. Uygulama daha sonra `authorization_code` arka planda bir `access_token` ve `refresh_token`bir .  Uygulama, HTTP `access_token` isteklerinde arka uçtaki bir web API'sine kimlik doğrulamak için kullanabilir. Ayrıca eskisinin süresi dolduğunda yeni bir `access_token` almak için `refresh_token` öğesini kullanabilir.

## <a name="current-limitations"></a>Geçerli sınırlamalar

### <a name="unsupported-application-types"></a>Desteklenmeyen uygulama türleri

#### <a name="daemonsserver-side-applications"></a>Daemons/sunucu tarafı uygulamaları

Uzun süren işlemler içeren veya kullanıcı nın varlığı olmadan çalışan uygulamaların web API'leri gibi güvenli kaynaklara erişmek için de bir yol gerekir. Bu uygulamalar, uygulamanın kimliğini (kullanıcının temsilci kimliği yerine) kullanarak ve OAuth 2.0 istemci kimlik bilgilerini kullanarak kimlik doğrulayabilir ve belirteçleri alabilir. İstemci kimlik bilgisi akışı, on-adına akışla aynı değildir ve sunucudan sunucuya kimlik doğrulaması için kullanılmamalıdır.

İstemci kimlik bilgisi akışı şu anda Azure AD B2C tarafından desteklenmese de, Azure AD'yi kullanarak istemci kimlik bilgisi akışını ayarlayabilirsiniz. Azure AD B2C kiracısı, azure AD kurumsal kiracılarıyla bazı işlevleri paylaşır.  İstemci kimlik bilgisi akışı, Azure AD B2C kiracısının Azure AD işlevi kullanılarak desteklenir.

İstemci kimlik bilgisi akışını ayarlamak için [Azure Active Directory v2.0 ve OAuth 2.0 istemci kimlik bilgileri akışına](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds)bakın. Başarılı bir kimlik doğrulama, [Azure AD belirteci referansında](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)açıklandığı gibi Azure AD tarafından kullanılabilecek şekilde biçimlendirilmiş bir belirteç alınmasıyla sonuçlanır.

#### <a name="web-api-chains-on-behalf-of-flow"></a>Web API'si zincirleri (temsili akış)

Çoğu mimari başka bir aşağı akış web API'si çağırmayı gerektiren bir web API'si içerir; her iki API de Azure AD B2C tarafından güvence altına alınır. Bu senaryo, Web API arka ucu olan ve Microsoft Graph API gibi bir Microsoft çevrimiçi hizmeti çağıran yerel istemcilerde yaygındır.

Bu zincirli web API'si senaryosu, temsili akış olarak da bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgisi yetkisi kullanılarak desteklenebilir.  Ancak temsili akış şu anda Azure AD B2C’de uygulanmamıştır.

### <a name="faulted-apps"></a>Hatalı uygulamalar

Azure AD B2C uygulamalarını şu şekilde değişirmeyin:

-  [Başvuru Kayıt Portalı](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)gibi diğer uygulama yönetim portallarında.
- Grafik API veya PowerShell kullanma.

Azure AD B2C uygulamasını Azure portalının dışında düzenliyorsanız, hatalı bir uygulama haline gelir ve artık Azure AD B2C ile kullanılabilir değildir. Uygulamayı silin ve yeniden oluşturun.

Uygulamayı silmek [için, Uygulama Kayıt Portalı'na](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) gidin ve uygulamayı silin. Uygulamanın görünür olması için uygulamanın sahibi olmanız (ve yalnızca kiracının yöneticisi olmamanız) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory B2C'de Kullanıcı akışları](user-flow-overview.md)tarafından sağlanan yerleşik ilkeler hakkında daha fazla bilgi edinin.
