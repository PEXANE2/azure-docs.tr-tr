---
title: Erişim jetonu isteyin - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'den nasıl erişim jetonu isteyeceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 8358d3378ea892ebeef653bcb51243c9f1aa0b8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259779"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de bir erişim jetonu isteme

*Erişim belirteci,* API'lerinize verilen izinleri tanımlamak için Azure Active Directory B2C'de (Azure AD B2C) kullanabileceğiniz talepleri içerir. Kaynak sunucusunu ararken, HTTP isteğinde bir erişim belirteci bulunmalıdır. Erişim belirteci, Azure AD B2C'nin yanıtlarında **access_token** olarak gösterilir.

Bu makalede, bir web uygulaması ve web API için bir erişim belirteci nasıl istek gösterir. Azure AD B2C'deki belirteçler hakkında daha fazla bilgi için [Azure Etkin Dizin B2C'deki belirteçlere genel bakış](tokens-overview.md)bölümüne bakın.

> [!NOTE]
> **Web API zincirleri (On-Behalf-Of) Azure AD B2C tarafından desteklenmez.** - Birçok mimari, her ikisi de Azure AD B2C tarafından güvenli başka bir alt akış web API'si çağırması gereken bir web API'si içerir. Bu senaryo, web API arka ucu na sahip istemcilerde yaygındır ve bu da başka bir hizmeti çağırır. Bu zincirli web API senaryosu OAuth 2.0 JWT Bearer Kimlik Bilgileri kullanılarak desteklenebilir, aksi takdirde On-Behalf-Of akışı olarak bilinir. Ancak, Adına akışı şu anda Azure AD B2C'de uygulanmaz.

## <a name="prerequisites"></a>Ön koşullar

- Kullanıcıların uygulamanıza kaydolmasını ve oturum açmalarını sağlamak için [bir kullanıcı akışı oluşturun.](tutorial-create-user-flows.md)
- Bunu daha önce yapmadıysanız, [Azure Etkin Dizin B2C kiracınıza bir web API uygulaması ekleyin.](add-web-application.md)

## <a name="scopes"></a>Kapsamlar

Kapsamlar, korumalı kaynaklara giden izinleri yönetmenin bir yolunu sağlar. Bir erişim belirteci istendiğinde, istemci uygulaması nın isteğin **kapsam** parametresinde istenen izinleri belirtmesi gerekir. Örneğin, **App ID URI** olan API `https://contoso.onmicrosoft.com/api` `https://contoso.onmicrosoft.com/api/read` `read` için Kapsam **Değerini** belirtmek için, kapsam .

Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Aynı istekte birden çok izin almak için, isteğin boşluklarla ayrılmış tek **kapsam** parametresine birden çok giriş ekleyebilirsiniz.

Aşağıdaki örnek, bir URL'de çözülmüş kapsamları gösterir:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

Aşağıdaki örnek, bir URL'de kodlanmış kapsamları gösterir:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

İstemci başvurunuz için verilenden daha fazla kapsam talep ederseniz, en az bir izin verilirse arama başarılı olur. Ortaya çıkan erişim belirtecisindeki **scp** talebi yalnızca başarıyla verilen izinlerle doldurulur. OpenID Connect standardı birkaç özel kapsam değeri belirtir. Aşağıdaki kapsamlar kullanıcının profiline erişim iznini temsil eder:

- **openid** - Kimlik belirteci ister.
- **offline_access** - [Auth Code akışlarını](authorization-code-flow.md)kullanarak yenileme belirteci ister.

`/authorize` Bir istekteki `token` **response_type** parametresi içeriyorsa, **kapsam** parametresi dışında `openid` `offline_access` en az bir kaynak kapsamı içermelidir ve bu parametre verilecektir. Aksi takdirde, `/authorize` istek başarısız olur.

## <a name="request-a-token"></a>Belirteç isteği

Erişim jetonu istemek için bir yetkilendirme koduna ihtiyacınız vardır. Aşağıda, yetkilendirme kodu için `/authorize` bitiş noktasına bir istek örneği verilmiştir. Özel etki alanları erişim belirteçleri ile kullanım için desteklenmez. Tenant-name.onmicrosoft.com etki alanınızı istek URL'sinde kullanın.

Aşağıdaki örnekte, aşağıdaki değerleri değiştirirsiniz:

- `<tenant-name>`- Azure AD B2C kiracınızın adı.
- `<policy-name>`- Özel politikanızın veya kullanıcı akışının adı.
- `<application-ID>`- Kullanıcı akışını desteklemek için kaydettiğiniz web uygulamasının uygulama tanımlayıcısı.
- `<redirect-uri>`- Müşteri başvurusunu kaydettiğinizde girdiğiniz Yönlendirme URI'si. **Redirect URI**

```HTTP
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&response_type=code
```

Yetkilendirme koduyla yanıt bu örneğe benzer olmalıdır:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Yetkilendirme kodunu başarıyla aldıktan sonra, bir erişim belirteci istemek için kullanabilirsiniz:

```HTTP
POST <tenant-name>.onmicrosoft.com/oauth2/v2.0/token?p=<policy-name> HTTP/1.1
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

Döndürülen https://jwt.ms erişim belirteci incelemek için kullanırken, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

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

- Azure AD [B2C'de belirteçleri nasıl yapılandırılabildiğini](configure-tokens.md) öğrenin
