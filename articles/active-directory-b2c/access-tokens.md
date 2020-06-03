---
title: Erişim belirteci isteme-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C bir erişim belirteci isteme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a659d8929f845b1c58bd64f628b2d3f47a96674e
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84297539"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Azure Active Directory B2C erişim belirteci isteme

*Erişim belirteci* , API 'lerinize verilen izinleri tanımlamak için Azure Active Directory B2C (Azure AD B2C) içinde kullanabileceğiniz talepler içerir. Bir kaynak sunucusu çağrılırken, HTTP isteğinde bir erişim belirtecinin bulunması gerekir. Erişim belirteci, Azure AD B2C yanıtlarındaki **access_token** olarak gösterilir.

Bu makalede bir Web uygulaması ve Web API 'SI için erişim belirteci isteme yöntemi gösterilmektedir. Azure AD B2C belirteçleri hakkında daha fazla bilgi için [Azure Active Directory B2C belirteçlerine genel bakış](tokens-overview.md)bölümüne bakın.

> [!NOTE]
> **Web API zincirleri (Şirket içi) Azure AD B2C tarafından desteklenmez.** -Birçok mimaride, Azure AD B2C tarafından güvenliği sağlanmış başka bir aşağı akış Web API 'sini çağırması gereken bir Web API 'SI vardır. Bu senaryo, bir Web API 'SI arka ucu olan istemcilerde yaygındır ve bu da başka bir hizmeti çağırır. Bu zincirleme Web API 'SI senaryosu, OAuth 2,0 JWT taşıyıcı kimlik bilgisi verme kullanılarak desteklenir, aksi takdirde şirket adına akış olarak bilinir. Ancak, şirket adına akış şu anda Azure AD B2C uygulanmamıştır.

## <a name="prerequisites"></a>Önkoşullar

- Kullanıcıların uygulamanızda kaydolup oturum açmasını sağlamak için [bir Kullanıcı akışı oluşturun](tutorial-create-user-flows.md) .
- Daha önce yapmadıysanız, [Azure Active Directory B2C kiracınıza bir Web API uygulaması ekleyin](add-web-api-application.md).

## <a name="scopes"></a>Kapsamlar

Kapsamlar, korunan kaynaklarla izinleri yönetmek için bir yol sağlar. Erişim belirteci istendiğinde, istemci uygulamasının isteğin **kapsam** parametresinde istenen izinleri belirtmesi gerekir. Örneğin, uygulama KIMLIĞI URI 'si olan API 'nin **kapsam değerini** belirtmek için `read` **App ID URI** `https://contoso.onmicrosoft.com/api` kapsam olacaktır `https://contoso.onmicrosoft.com/api/read` .

Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Aynı istekte birden çok izin almak için, isteğin tek **kapsam** parametresine, boşluklarla ayırarak birden çok giriş ekleyebilirsiniz.

Aşağıdaki örnekte bir URL 'de kodu çözülen kapsamlar gösterilmektedir:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

Aşağıdaki örnek bir URL 'de kodlanan kapsamları gösterir:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

İstemci uygulamanız için sağlandıklarından daha fazla kapsam istemeniz durumunda, en az bir izin verildiğinde çağrı başarılı olur. Elde edilen erişim belirtecindeki **SCP** talebi yalnızca başarıyla verilen izinlerle doldurulur. OpenID Connect standardı birkaç özel kapsam değeri belirtir. Aşağıdaki kapsamlar, kullanıcının profiline erişme iznini temsil eder:

- **OpenID** -bir kimlik belirteci ister.
- **offline_access** - [kimlik doğrulama kod akışlarını](authorization-code-flow.md)kullanarak yenileme belirteci ister.

Bir istekteki **response_type** parametresi `/authorize` içeriyorsa `token` , **kapsam** parametresi, `openid` izin verilen ve verilecek en az bir kaynak kapsamı içermelidir `offline_access` . Aksi takdirde, `/authorize` istek başarısız olur.

## <a name="request-a-token"></a>Belirteç isteme

Erişim belirteci istemek için bir yetkilendirme kodu gerekir. Bir `/authorize` yetkilendirme kodu için uç nokta isteğine bir örnek aşağıda verilmiştir. Özel etki alanları, erişim belirteçleri ile kullanım için desteklenmez. İstek URL 'sinde tenant-name.onmicrosoft.com etki alanınızı kullanın.

Aşağıdaki örnekte, şu değerleri değiştirirsiniz:

- `<tenant-name>`-Azure AD B2C kiracınızın adı.
- `<policy-name>`-Özel ilkenizin veya Kullanıcı akışınız adı.
- `<application-ID>`-Kullanıcı akışını desteklemek için kaydettiğiniz Web uygulamasının uygulama tanımlayıcısı.
- `<redirect-uri>`-İstemci uygulamasını kaydettiğinizde girdiğiniz **yeniden yönlendirme URI 'si** .

```HTTP
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&response_type=code
```

Yetkilendirme koduna sahip yanıt bu örneğe benzer olmalıdır:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Yetkilendirme kodunu başarıyla aldıktan sonra, bir erişim belirteci istemek için bunu kullanabilirsiniz:

```HTTP
POST <tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/token HTTP/1.1
Host: <tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id=<application-ID>
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
&redirect_uri=https://jwt.ms
&client_secret=2hMG2-_:y12n10vwH...
```

Aşağıdaki yanıta benzer bir şey görmeniz gerekir:

```JSON
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrN...",
    "token_type": "Bearer",
    "not_before": 1549647431,
    "expires_in": 3600,
    "expires_on": 1549651031,
    "resource": "f2a76e08-93f2-4350-833c-965c02483b11",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiJjNjRhNGY3ZC0zMDkxLTRjNzMtYTcyMi1hM2YwNjk0Z..."
}
```

https://jwt.msDöndürülen erişim belirtecini incelemek için kullanırken, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

```JSON
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dl..."
}.{
  "iss": "https://contoso0926tenant.b2clogin.com/c64a4f7d-3091-4c73-a7.../v2.0/",
  "exp": 1549651031,
  "nbf": 1549647431,
  "aud": "f2a76e08-93f2-4350-833c-965...",
  "oid": "1558f87f-452b-4757-bcd1-883...",
  "sub": "1558f87f-452b-4757-bcd1-883...",
  "name": "David",
  "tfp": "B2C_1_signupsignin1",
  "nonce": "anyRandomValue",
  "scp": "read",
  "azp": "38307aee-303c-4fff-8087-d8d2...",
  "ver": "1.0",
  "iat": 1549647431
}.[Signature]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C belirteçlerin nasıl yapılandırılacağı](configure-tokens.md) hakkında bilgi edinin
