---
title: Yetkilendirme kodu akışı - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure AD B2C ve OpenID Connect kimlik doğrulama protokolünü kullanarak web uygulamaları oluşturmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 8248ca0abb1d633786b09b894bcd6b1089ab2d8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260897"
---
# <a name="oauth-20-authorization-code-flow-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de OAuth 2.0 yetkilendirme kodu akışı

Web API'leri gibi korumalı kaynaklara erişmek için aygıta yüklenen uygulamalarda OAuth 2.0 yetkilendirme kodu hibesini kullanabilirsiniz. OAuth 2.0'ın Azure Active Directory B2C (Azure AD B2C) uygulamasını kullanarak mobil ve masaüstü uygulamalarınıza kaydolma, kaydolma ve diğer kimlik yönetimi görevlerini ekleyebilirsiniz. Bu makale dilden bağımsızdır. Makalede, herhangi bir açık kaynak kitaplık kullanmadan HTTP iletilerinin nasıl gönderilip alınılacağını açıklıyoruz.

OAuth 2.0 yetkilendirme kodu akışı, [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749)açıklanmıştır. Web uygulamaları ve yerel olarak yüklenmiş uygulamalar da dahil olmak üzere çoğu [uygulama türünde](application-types.md)kimlik doğrulama ve yetkilendirme için kullanabilirsiniz. OAuth 2.0 yetkilendirme kodu akışını, bir [yetkilendirme sunucusu](protocols-overview.md)tarafından güvenli kaynaklara erişmek için kullanılabilen uygulamalarınız için erişim belirteçlerini güvenli bir şekilde elde etmek ve belirteçlerini yenilemek için kullanabilirsiniz.  Yenileme belirteci, erişim belirteci süresi dolduğunda genellikle bir saat sonra istemcinin yeni erişim (ve yenileme) belirteçleri edinmesine olanak tanır.

Bu **makalede, kamu istemcileri** OAuth 2.0 yetkilendirme kodu akışı üzerinde duruluyor. Ortak istemci, gizli bir parolanın bütünlüğünü güvenli bir şekilde korumak için güvenilemeyen herhangi bir istemci uygulamasıdır. Buna mobil uygulamalar, masaüstü uygulamaları ve temelde bir cihazda çalışan ve erişim belirteçleri alması gereken tüm uygulamalar dahildir.

> [!NOTE]
> Azure AD B2C'yi kullanarak bir web uygulamasına kimlik yönetimi eklemek için OAuth 2.0 yerine [OpenID Connect'i](openid-connect.md) kullanın.

Azure AD B2C, standart OAuth 2.0 akışlarını basit kimlik doğrulama ve yetkilendirmeden daha fazlasını yapmak için genişletir. Bu kullanıcı [akışını](user-flow-overview.md)tanıttı. Kullanıcı akışları ile, uygulamanıza kaydolma, kaydolma ve profil yönetimi gibi kullanıcı deneyimleri eklemek için OAuth 2.0'ı kullanabilirsiniz. OAuth 2.0 protokolünü kullanan kimlik sağlayıcıları arasında [Amazon,](identity-provider-amazon.md) [Azure Active Directory,](identity-provider-azure-ad-single-tenant.md) [Facebook,](identity-provider-facebook.md) [GitHub,](identity-provider-github.md) [Google](identity-provider-google.md)ve [LinkedIn](identity-provider-linkedin.md)bulunmaktadır.

Bu makalede HTTP isteklerini denemek için:

1. Azure `{tenant}` AD B2C kiracınızın adıyla değiştirin.
1. Azure `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` AD B2C kiracınızda daha önce kaydolduğunuz bir uygulamanın uygulama kimliğiyle değiştirin.
1. Örneğin, `{policy}` `b2c_1_sign_in`kiracınızda oluşturduğunuz bir ilkenin adı ile değiştirin.

## <a name="1-get-an-authorization-code"></a>1. Yetkilendirme kodu alma
Yetkilendirme kodu akışı, istemcinin kullanıcıyı `/authorize` bitiş noktasına yönlendirmesiyle başlar. Bu, kullanıcının harekete geçirdiği akış etkileşimli bölümüdür. Bu istekte, istemci `scope` parametrede kullanıcıdan alması gereken izinleri gösterir. Aşağıdaki üç örnek (okunabilirlik için satır sonları ile) her biri farklı bir kullanıcı akışı kullanır.


```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
```


| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
|{kiracı}| Gerekli | Azure AD B2C kiracınızın adı|
| {ilke} | Gerekli | Çalıştırılacak kullanıcı akışı. Azure AD B2C kiracınızda oluşturduğunuz kullanıcı akışının adını belirtin. Örneğin: `b2c_1_sign_in`, `b2c_1_sign_up`, `b2c_1_edit_profile`veya . |
| client_id |Gerekli |[Azure portalında](https://portal.azure.com)uygulamanız için atanan uygulama kimliği. |
| response_type |Gerekli |Yetkilendirme kodu akışı için `code` içermesi gereken yanıt türü. |
| redirect_uri |Gerekli |Kimlik doğrulama yanıtlarının uygulamanız tarafından gönderildiği ve alındığı uygulamanızın URI'yi yönlendirmesi. URL kodlanmış olması dışında, portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. |
| scope |Gerekli |Alan ayrılmış kapsamlistesi. Tek bir kapsam değeri, istenen izinlerin her ikisini de Azure Etkin Dizin'e (Azure AD) gösterir. Kapsam olarak istemci kimliğinikullanmak, uygulamanızın aynı istemci kimliğiyle temsil edilen kendi hizmetinize veya web API'nıza karşı kullanılabilecek bir erişim belirtecine ihtiyacı olduğunu gösterir.  Kapsam, `offline_access` uygulamanızın kaynaklara uzun süreli erişim için yeni bir belirteci gerektiğini gösterir. Kapsamı, `openid` Azure AD B2C'den bir kimlik belirteci istemek için de kullanabilirsiniz. |
| response_mode |Önerilen |Elde edilen yetkilendirme kodunu uygulamanıza geri göndermek için kullandığınız yöntem. Olabilir, ya da `fragment`. `query` `form_post` |
| durum |Önerilen |İsteğe dahil edilen ve kullanmak istediğiniz herhangi bir içerik dizisi olabilecek bir değer. Genellikle, rasgele oluşturulan benzersiz bir değer, çapraz site isteği sahtecilik saldırılarını önlemek için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için de kullanılır. Örneğin, kullanıcının üzerinde olduğu sayfa veya yürütülmekte olan kullanıcı akışı. |
| Istemi |İsteğe bağlı |Gerekli kullanıcı etkileşimi türü. Şu anda, tek `login`geçerli değer , kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlayan değerdir. Tek oturum açma etkili olmaz. |

Bu noktada, kullanıcıdan kullanıcı akışının iş akışını tamamlaması istenir. Bu, kullanıcı nın kullanıcı adını ve parolasını girmesini, sosyal bir kimlikle oturum açmasını, dizine kaydolmayı veya başka sayıda adımı içerebileceğini. Kullanıcı eylemleri, kullanıcı akışının nasıl tanımlandığına bağlıdır.

Kullanıcı kullanıcı akışını tamamladıktan sonra, Azure AD uygulamanız için `redirect_uri`kullandığınız değerden bir yanıt döndürür. `response_mode` Parametrede belirtilen yöntemi kullanır. Yanıt, yürütülen kullanıcı akışından bağımsız olarak, her kullanıcı eylem senaryosu için tam olarak aynıdır.

Kullanan `response_mode=query` başarılı bir yanıt şuna benzer:

```HTTP
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametre | Açıklama |
| --- | --- |
| kod |Uygulamanın istediği yetkilendirme kodu. Uygulama, hedef kaynak için erişim jetonu istemek için yetkilendirme kodunu kullanabilir. Yetkilendirme kodları çok kısa ömürlüdür. Genellikle, yaklaşık 10 dakika sonra sona erer. |
| durum |Yukarıdaki bölümdeki tablodaki tam açıklamaya bakın. İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

Hata yanıtları, uygulamanın bunları uygun şekilde işleyebilmeleri için uri yönlendirmesine de gönderilebilir:

```HTTP
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Hataları tepkilemek için dizeyi de kullanabilirsiniz. |
| error_description |Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum |Önceki tablodaki tam açıklamaya bakın. İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

## <a name="2-get-a-token"></a>2. Bir belirteç alın
Artık bir yetkilendirme kodu edindiğinize göre, `code` `/token` bitiş noktasına bir POST isteği göndererek amaçlanan kaynağa bir belirteç için kullanabilirsiniz. Azure AD B2C'de, istekte kapsamlarını(lar) belirterek diğer API'ler için her zamanki gibi [erişim belirteçleri isteyebilirsiniz.](access-tokens.md#request-a-token)

Ayrıca, uygulamanızın istemci kimliğini istenen kapsam olarak kullanma kuralıyla uygulamanızın kendi arka uç Web API'si için bir erişim jetonu da isteyebilirsiniz (bu da bu istemci kimliğiyle "hedef kitle" olarak bir erişim belirteciyle sonuçlanır):

```HTTP
POST https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1

Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
|{kiracı}| Gerekli | Azure AD B2C kiracınızın adı|
|{ilke}| Gerekli| Yetkilendirme kodunu elde etmek için kullanılan kullanıcı akışı. Bu istekte farklı bir kullanıcı akışı kullanamazsınız. |
| client_id |Gerekli |[Azure portalında](https://portal.azure.com)uygulamanız için atanan uygulama kimliği.|
| client_secret | Evet, Web Apps'ta | [Azure portalında](https://portal.azure.com/)oluşturulan uygulama sırrı. İstemci sırları, istemcinin bir istemci sırrını güvenli bir şekilde depolayabildiği Web Uygulaması senaryoları için bu akışta kullanılır. Yerel Uygulama (ortak istemci) senaryoları için istemci sırları güvenli bir şekilde depolanamaz ve bu nedenle bu çağrıda kullanılmaz. Bir istemci sırrı kullanıyorsanız, lütfen periyodik olarak değiştirin. |
| grant_type |Gerekli |Hibe türü. Yetkilendirme kodu akışı için hibe türü `authorization_code`. |
| scope |Önerilen |Alan ayrılmış kapsamlistesi. Tek bir kapsam değeri, istenen izinlerin her ikisini de Azure AD'ye gösterir. Kapsam olarak istemci kimliğinikullanmak, uygulamanızın aynı istemci kimliğiyle temsil edilen kendi hizmetinize veya web API'nıza karşı kullanılabilecek bir erişim belirtecine ihtiyacı olduğunu gösterir.  Kapsam, `offline_access` uygulamanızın kaynaklara uzun süreli erişim için yeni bir belirteci gerektiğini gösterir.  Kapsamı, `openid` Azure AD B2C'den bir kimlik belirteci istemek için de kullanabilirsiniz. |
| kod |Gerekli |Akışın ilk ayağında edindiğiniz yetkilendirme kodu. |
| redirect_uri |Gerekli |Yetkilendirme kodunu aldığınız uygulamanın yeniden yönlendirme URI'si. |

Başarılı bir belirteç yanıtı şuna benzer:

```JSON
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Belirteç geçerli olarak kabul edilir zaman, dönem içinde. |
| token_type |Belirteç türü değeri. Azure AD'nin desteklediği tek tür Taşıyıcı'dır. |
| access_token |İstediğindiğiniz imzalı JSON Web Jetonu (JWT). |
| scope |Belirteç için geçerli olan kapsamlar. Daha sonra kullanmak üzere belirteçleri önbelleğe almak için kapsamları da kullanabilirsiniz. |
| expires_in |Belirteç geçerli olduğu süre uzunluğu (saniye cinsinden). |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama, geçerli belirteç süresi dolduktan sonra ek belirteçler elde etmek için bu belirteci kullanabilir. Yenileme belirteçleri uzun ömürlüdür. Bunları, kaynaklara erişimi uzun süreler boyunca korumak için kullanabilirsiniz. Daha fazla bilgi için [Azure AD B2C belirteç başvurusuna](tokens-overview.md)bakın. |

Hata yanıtları şuna benzer:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Hataları tepkilemek için dizeyi de kullanabilirsiniz. |
| error_description |Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="3-use-the-token"></a>3. Belirteci kullanın
Artık bir erişim jetonu elde ettiğinize göre, arka uçtaki web API'larınıza gelen isteklerde belirteci üstbilgiye `Authorization` ekleyerek kullanabilirsiniz:

```HTTP
GET /tasks
Host: mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Belirteci yenileyin
Erişim belirteçleri ve kimlik belirteçleri kısa ömürlüdür. Süreleri dolduktan sonra, kaynaklara erişmeye devam etmek için bunları yenilemeniz gerekir. Bunu yapmak için, `/token` bitiş noktasına başka bir POST isteği gönderin. Bu kez, `refresh_token` yerine `code`sağlamak:

```HTTP
POST https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1

Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
|{kiracı}| Gerekli | Azure AD B2C kiracınızın adı|
|{ilke} |Gerekli |Özgün yenileme belirteci elde etmek için kullanılan kullanıcı akışı. Bu istekte farklı bir kullanıcı akışı kullanamazsınız. |
| client_id |Gerekli |[Azure portalında](https://portal.azure.com)uygulamanız için atanan uygulama kimliği. |
| client_secret | Evet, Web Apps'ta | [Azure portalında](https://portal.azure.com/)oluşturulan uygulama sırrı. İstemci sırları, istemcinin bir istemci sırrını güvenli bir şekilde depolayabildiği Web Uygulaması senaryoları için bu akışta kullanılır. Yerel Uygulama (ortak istemci) senaryoları için istemci sırları güvenli bir şekilde depolanamaz ve bu nedenle bu çağrıda kullanılmaz. Bir istemci sırrı kullanıyorsanız, lütfen periyodik olarak değiştirin. |
| grant_type |Gerekli |Hibe türü. Yetkilendirme kodu akışının bu ayağı için hibe `refresh_token`türü . |
| scope |Önerilen |Alan ayrılmış kapsamlistesi. Tek bir kapsam değeri, istenen izinlerin her ikisini de Azure AD'ye gösterir. Kapsam olarak istemci kimliğinikullanmak, uygulamanızın aynı istemci kimliğiyle temsil edilen kendi hizmetinize veya web API'nıza karşı kullanılabilecek bir erişim belirtecine ihtiyacı olduğunu gösterir.  Kapsam, `offline_access` uygulamanızın kaynaklara uzun süreli erişim için yeni bir belirteci gerektiğini gösterir.  Kapsamı, `openid` Azure AD B2C'den bir kimlik belirteci istemek için de kullanabilirsiniz. |
| redirect_uri |İsteğe bağlı |Yetkilendirme kodunu aldığınız uygulamanın yeniden yönlendirme URI'si. |
| refresh_token |Gerekli |Akışın ikinci ayağında edindiğiniz orijinal yenileme belirteci. |

Başarılı bir belirteç yanıtı şuna benzer:

```JSON
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametre | Açıklama |
| --- | --- |
| not_before |Belirteç geçerli olarak kabul edilir zaman, dönem içinde. |
| token_type |Belirteç türü değeri. Azure AD'nin desteklediği tek tür Taşıyıcı'dır. |
| access_token |İstediğin imzalı JWT. |
| scope |Belirteç için geçerli olan kapsamlar. Daha sonra kullanmak üzere belirteçleri önbelleğe almak için kapsamları da kullanabilirsiniz. |
| expires_in |Belirteç geçerli olduğu süre uzunluğu (saniye cinsinden). |
| refresh_token |Bir OAuth 2.0 yenileme belirteci. Uygulama, geçerli belirteç süresi dolduktan sonra ek belirteçler elde etmek için bu belirteci kullanabilir. Yenileme belirteçleri uzun ömürlüdür ve kaynaklara erişimi uzun süre tutmak için kullanılabilir. Daha fazla bilgi için [Azure AD B2C belirteç başvurusuna](tokens-overview.md)bakın. |

Hata yanıtları şuna benzer:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanabileceğiniz bir hata kodu dizesi. Hataları tepkilemek için dizeyi de kullanabilirsiniz. |
| error_description |Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Kendi Azure AD B2C dizininizi kullanın
Bu istekleri kendiniz denemek için aşağıdaki adımları tamamlayın. Bu makalede kullandığımız örnek değerleri kendi değerlerinizle değiştirin.

1. [Azure AD B2C dizini oluşturun.](tutorial-create-tenant.md) İsteklerde dizininizin adını kullanın.
2. Uygulama kimliği ve yeniden yönlendirme URI'si almak için bir [uygulama oluşturun.](tutorial-register-applications.md) Uygulamanıza yerel bir müşteri ekleyin.
3. Kullanıcı akış adlarınızı almak için [kullanıcı akışlarınızı oluşturun.](user-flow-overview.md)
