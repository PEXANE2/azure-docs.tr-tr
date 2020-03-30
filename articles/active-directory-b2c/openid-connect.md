---
title: OpenID Connect ile Web oturum açma - Azure Active Directory B2C
description: Azure Active Directory B2C'deki OpenID Connect kimlik doğrulama iletişim kuralını kullanarak web uygulamaları oluşturun.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 6640ab1660e6499a97a8c990a0001d5fbae4e997
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264394"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de OpenID Connect ile Web oturum açma

OpenID Connect, Kullanıcıları web uygulamalarında güvenli bir şekilde oturum açmak için kullanılabilen, OAuth 2.0'ın üzerine inşa edilmiş bir kimlik doğrulama protokolüdür. OpenID Connect'in Azure Active Directory B2C (Azure AD B2C) uygulamasını kullanarak, web uygulamalarınızdaki kaydolma, kaydolma ve diğer kimlik yönetimi deneyimlerini Azure Active Directory'ye (Azure AD) dış kaynak olarak sunabilirsiniz. Bu kılavuz, bunu dilden bağımsız bir şekilde nasıl yapacağınızı gösterir. Açık kaynak kitaplıklarımızdan herhangi birini kullanmadan HTTP iletilerinin nasıl gönderilip alınılacağını açıklar.

[OpenID Connect,](https://openid.net/specs/openid-connect-core-1_0.html) *kimlik doğrulama* protokolü olarak kullanılmak üzere OAuth 2.0 *yetkilendirme* protokolünü genişletir. Bu kimlik doğrulama protokolü, tek oturum açma gerçekleştirmenize olanak tanır. Bu bir *kimlik belirteci*kavramını tanıttı , hangi istemci kullanıcının kimliğini doğrulamak ve kullanıcı hakkında temel profil bilgilerini elde sağlar.

OAuth 2.0'ı genişletdiği için, uygulamaların güvenli bir şekilde *erişim belirteçleri*edinmesini de sağlar. Bir [yetkilendirme sunucusu](protocols-overview.md)tarafından güvenli kaynaklara erişmek için erişim belirteçleri kullanabilirsiniz. OpenID Connect, sunucuda barındırılan ve tarayıcı üzerinden erişilen bir web uygulaması oluşturuyorsanız önerilir. Belirteçler hakkında daha fazla bilgi için [Azure Etkin Dizin B2C'deki belirteçlere Genel Bakış](tokens-overview.md) bölümüne bakın

Azure AD B2C, standart OpenID Connect protokolünü basit kimlik doğrulama ve yetkilendirmeden daha fazlasını yapmak için genişletir. Uygulamanıza kaydolma, kaydolma ve profil yönetimi gibi kullanıcı deneyimleri eklemek için OpenID Connect'i kullanmanızı sağlayan [kullanıcı akış parametresini](user-flow-overview.md)sunar.

## <a name="send-authentication-requests"></a>Kimlik doğrulama istekleri gönderme

Web uygulamanızın kullanıcının kimliğini doğrulaması ve kullanıcı akışını çalıştırması gerektiğinde, `/authorize` kullanıcıyı bitiş noktasına yönlendirebilir. Kullanıcı akışına bağlı olarak harekete geçer.

Bu istekte, istemci `scope` parametrede kullanıcıdan alması gereken izinleri gösterir ve çalışacak kullanıcı akışını belirtir. İsteğin nasıl çalıştığı hakkında bilgi almak için, isteği bir tarayıcıya yapıştırmayı ve çalıştırmayı deneyin. Kiracınızın adıyla değiştirin. `{tenant}` Kiracınızda daha önce kaydolduğunuz uygulamanın uygulama kimliğiyle değiştirin. `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` Ayrıca, ilke`{policy}`adını ( ) kiracınızda bulunan ilke `b2c_1_sign_in`adıyla değiştirin, örneğin.

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| {kiracı} | Evet | Azure AD B2C kiracınızın adı |
| {ilke} | Evet | Çalıştırılacak kullanıcı akışı. Azure AD B2C kiracınızda oluşturduğunuz kullanıcı akışının adını belirtin. Örneğin: `b2c_1_sign_in`, `b2c_1_sign_up`, `b2c_1_edit_profile`veya . |
| client_id | Evet | [Azure portalının](https://portal.azure.com/) uygulamanıza atadığı uygulama kimliği. |
| Nonce | Evet | Talepte (uygulama tarafından oluşturulan) yer alan ve talep olarak ortaya çıkan kimlik belirtecine dahil edilen bir değer. Uygulama daha sonra belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Değer genellikle isteğin kaynağını tanımlamak için kullanılabilecek randomize benzersiz bir dizedir. |
| response_type | Evet | OpenID Connect için bir kimlik belirteci içermelidir. Web uygulamanızın web API'sini aramak için belirteçleri de gerekiyorsa, bunu kullanabilirsiniz. `code+id_token` |
| scope | Evet | Alan ayrılmış kapsamlistesi. Kapsam, `openid` kullanıcıda oturum açma ve kullanıcı hakkında kimlik belirteçleri şeklinde veri alma iznini gösterir. `offline_access` Kapsam web uygulamaları için isteğe bağlıdır. Bu, uygulamanızın kaynaklara genişletilmiş erişim için *yenibir belirteci* gerekeceğini gösterir. |
| Istemi | Hayır | Gerekli kullanıcı etkileşimi türü. Şu anda tek geçerli `login`değer, kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlayan değerdir. |
| redirect_uri | Hayır | Uygulamanızın `redirect_uri` parametresi, kimlik doğrulama yanıtlarının başvurunuz tarafından gönderilebileceği ve alınabileceği. URL kodlanmış olması `redirect_uri` dışında Azure portalında kaydettiğiniz parametrelerden biriyle tam olarak eşleşmelidir. |
| response_mode | Hayır | Elde edilen yetkilendirme kodunu uygulamanıza geri göndermek için kullanılan yöntem. Ya `query`da `form_post` `fragment`olabilir.  En `form_post` iyi güvenlik için yanıt modu önerilir. |
| durum | Hayır | Belirteç yanıtında döndürülen isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Rasgele oluşturulan benzersiz değer genellikle siteleri arası istek sahteciliği saldırılarını önlemek için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının uygulamadaki durumu yla ilgili bilgileri (örneğin, üzerinde oldukları sayfa) kodlamak için de kullanılır. |

Bu noktada, kullanıcıdan iş akışını tamamlaması istenir. Kullanıcının kullanıcı adını ve parolasını girmesi, sosyal kimlikle oturum açması veya dizine kaydolması gerekebilir. Kullanıcı akışının nasıl tanımlandığına bağlı olarak başka sayıda adım olabilir.

Kullanıcı kullanıcı akışını tamamladıktan sonra, `redirect_uri` `response_mode` parametrede belirtilen yöntem kullanılarak belirtilen parametredeki uygulamanıza bir yanıt döndürülür. Yanıt, kullanıcı akışından bağımsız olarak, önceki servis taleplerinin her biri için aynıdır.

Başarılı bir `response_mode=fragment` yanıt kullanarak gibi görünür:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| id_token | Başvurunun istediği kimlik belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için kimlik belirtecinizi kullanabilirsiniz. |
| kod | Uygulamanın istediği yetkilendirme kodu, kullandıysanız `response_type=code+id_token`. Uygulama, hedef kaynak için erişim jetonu istemek için yetkilendirme kodunu kullanabilir. Yetkilendirme kodları genellikle yaklaşık 10 dakika sonra sona erer. |
| durum | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebileceği şekilde parametreye de gönderilebilir:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılabilecek bir kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemeye yardımcı olabilecek belirli bir hata iletisi. |
| durum | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama

Yalnızca bir kimlik belirteci almak kullanıcının kimliğini doğrulamak için yeterli değildir. Kimlik belirteci imzasını doğrulayın ve başvurunuzun gereksinimlerine göre belirteçteki talepleri doğrulayın. Azure AD B2C, belirteçleri imzalamak ve geçerli olduklarını doğrulamak için [JSON Web Belirteçleri (JWT'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve ortak anahtar şifrelemesini kullanır. Tercih dilinize bağlı olarak, JWT'leri doğrulamak için kullanılabilen birçok açık kaynak kitaplığı vardır. Kendi doğrulama mantığınızı uygulamak yerine bu seçenekleri keşfetmenizi öneririz.

Azure AD B2C, bir uygulamanın çalışma zamanında Azure AD B2C hakkında bilgi edinmesini sağlayan bir OpenID Connect meta veri bitiş noktasına sahiptir. Bu bilgiler uç noktaları, belirteç içeriğini ve belirteç imzalama anahtarlarını içerir. B2C kiracınızdaki her kullanıcı akışı için bir JSON meta veri belgesi vardır. Örneğin, `b2c_1_sign_in` kullanıcı akışı `fabrikamb2c.onmicrosoft.com` için meta veri belgesi şu adreste bulunur:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/v2.0/.well-known/openid-configuration
```

Bu yapılandırma belgesinin özelliklerinden `jwks_uri`biri, aynı kullanıcı akışı için değeri olacaktır:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/discovery/v2.0/keys
```

Kimlik belirteci imzalamada hangi kullanıcı akışının kullanıldığını (ve meta verileri nereden alacağınız) belirlemek için iki seçeneğiniz var. İlk olarak, kullanıcı akışı adı `acr` kimlik belirtecinde talep te dahil edilir. Diğer seçeneğiniz, isteği verirken kullanıcı akışını `state` parametrenin değerinde kodlamak ve ardından hangi kullanıcı akışının kullanıldığını belirlemek için kodu çözmektir. Her iki yöntem de geçerlidir.

OpenID Connect meta veri bitiş noktasından meta veri belgesini aldıktan sonra, kimlik belirteci imzasını doğrulamak için RSA 256 ortak anahtarlarını kullanabilirsiniz. Bu bitiş noktasında, her biri bir `kid` taleple tanımlanan birden çok anahtar listelenmiş olabilir. Kimlik belirteci üstbilgisi, `kid` bu anahtarlardan hangisinin kimlik belirteci imzalamak için kullanıldığını gösteren bir talep de içerir.

Azure AD B2C'deki belirteçleri doğrulamak için, üst(e) ve modül(n) kullanarak ortak anahtarı oluşturmanız gerekir. Bunu ilgili programlama dilinizde buna göre nasıl yapacağınızı belirlemeniz gerekir. RSA protokolü ile Public Key üretimi ile ilgili resmi belgelere buradan bulabilirsiniz:https://tools.ietf.org/html/rfc3447#section-3.1

Kimlik belirteci imzasını doğruladıktan sonra doğrulamanız gereken birkaç talep vardır. Örneğin:

- Belirteç yeniden oynatma saldırılarını önlemek için `nonce` talebi doğrulayın. Değeri oturum açma isteğinde belirttiğiniz değer olmalıdır.
- Başvurunuz `aud` için kimlik belirteci nin verildiğinden emin olmak için talebi doğrulayın. Değeri, uygulamanızın uygulama kimliği olmalıdır.
- Kimlik `iat` belirteci süresinin dolmadığından emin olmak için ve `exp` talepleri doğrulayın.

Gerçekleştirmeniz gereken birkaç doğrulama daha vardır. Doğrulamalar [OpenID Connect Core Spec](https://openid.net/specs/openid-connect-core-1_0.html)ayrıntılı olarak açıklanmıştır. Senaryonuza bağlı olarak ek talepleri doğrulamak da isteyebilirsiniz. Bazı yaygın doğrulamalar şunlardır:

- Kullanıcının/kuruluşun uygulamaya kaydolmasını sağlamak.
- Kullanıcının uygun yetkilendirmeye/ayrıcalıklara sahip olmasını sağlamak.
- Azure Çok Faktörlü Kimlik Doğrulama gibi belirli bir kimlik doğrulama gücünün oluştuğunu sağlamak.

Kimlik belirteci doğruladıktan sonra, kullanıcı ile bir oturum başlayabilirsiniz. Uygulamanızdaki kullanıcı hakkında bilgi almak için kimlik belirtecindeki talepleri kullanabilirsiniz. Bu bilgilerin kullanımalanları görüntü, kayıt ve yetkilendirmeyi içerir.

## <a name="get-a-token"></a>Bir belirteç alın

Yalnızca kullanıcı akışlarını çalıştırmak için web uygulamanıza ihtiyacınız varsa, sonraki birkaç bölümü atlayabilirsiniz. Bu bölümler yalnızca bir web API'sine doğrulanmış aramalar yapması gereken ve Azure AD B2C tarafından korunan web uygulamaları için geçerlidir.

Satın aldığınız yetkilendirme kodunu (kullanarak) `response_type=code+id_token`bir belirteç için, `POST` `/token` bitiş noktasına bir istek göndererek istediğiniz kaynağa gönderebilirsiniz. Azure AD B2C'de, istekte kapsamlarını(lar) belirterek diğer API'ler için her zamanki gibi [erişim belirteçleri isteyebilirsiniz.](access-tokens.md#request-a-token)

Ayrıca, uygulamanızın istemci kimliğini istenen kapsam olarak kullanma kuralıyla uygulamanızın kendi arka uç Web API'si için bir erişim jetonu da isteyebilirsiniz (bu da bu istemci kimliğiyle "hedef kitle" olarak bir erişim belirteciyle sonuçlanır):

```HTTP
POST {tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1
Host: {tenant}.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| {kiracı} | Evet | Azure AD B2C kiracınızın adı |
| {ilke} | Evet | Yetkilendirme kodunu elde etmek için kullanılan kullanıcı akışı. Bu istekte farklı bir kullanıcı akışı kullanamazsınız. Bu parametreyi POST gövdesine değil, sorgu dizesi sine ekleyin. |
| client_id | Evet | [Azure portalının](https://portal.azure.com/) uygulamanıza atadığı uygulama kimliği. |
| client_secret | Evet, Web Apps'ta | [Azure portalında](https://portal.azure.com/)oluşturulan uygulama sırrı. İstemci sırları, istemcinin bir istemci sırrını güvenli bir şekilde depolayabildiği Web Uygulaması senaryoları için bu akışta kullanılır. Yerel Uygulama (ortak istemci) senaryoları için istemci sırları güvenli bir şekilde depolanamaz, bu akışta kullanılmaz. Bir istemci sırrı kullanıyorsanız, lütfen periyodik olarak değiştirin. |
| kod | Evet | Kullanıcı akışının başında edindiğiniz yetkilendirme kodu. |
| grant_type | Evet | Yetkilendirme kodu akışı için `authorization_code` olması gereken hibe türü. |
| redirect_uri | Evet | Yetkilendirme `redirect_uri` kodunu aldığınız uygulamanın parametresi. |
| scope | Hayır | Alan ayrılmış kapsamlistesi. Kapsam, `openid` kullanıcıda oturum açma ve kullanıcı hakkında veri alma iznini id_token parametreler ilerler. Bu, uygulamanızın istemciyle aynı uygulama kimliğiyle temsil edilen kendi arka uç web API'sine belirteçleri almak için kullanılabilir. Kapsam, `offline_access` uygulamanızın kaynaklara genişletilmiş erişim için yenibir belirteci gerektiğini gösterir. |

Başarılı bir belirteç yanıtı gibi görünür:

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
| --------- | ----------- |
| not_before | Belirteç geçerli olarak kabul edilir zaman, dönem içinde. |
| token_type | Belirteç türü değeri. `Bearer`desteklenen tek türdür. |
| access_token | İstediğinimzalı JWT jetonu. |
| scope | Belirteç için geçerli olan kapsamlar. |
| expires_in | Erişim belirtecinin geçerli olduğu süre uzunluğu (saniye cinsinden). |
| refresh_token | Bir OAuth 2.0 yenileme belirteci. Geçerli belirteç süresi dolduktan sonra ek belirteçler elde etmek için bu belirteci kullanabilirsiniz. Yenileme belirteçleri, kaynaklara erişimi uzun süre tutmak için kullanılabilir. Kapsam, `offline_access` yenibir belirteç almak için hem yetkilendirme hem de belirteç isteklerinde kullanılmış olmalıdır. |

Hata yanıtları gibi görünür:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılabilecek bir kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemeye yardımcı olabilecek bir ileti. |

## <a name="use-the-token"></a>Belirteci kullanma

Artık bir erişim jetonu elde ettiğinize göre, arka uçtaki web API'larınıza gelen isteklerde belirteci üstbilgiye `Authorization` ekleyerek kullanabilirsiniz:

```HTTP
GET /tasks
Host: mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Belirteci yenileyin

Kimlik belirteçleri kısa bir süre içinde sona erer. Kaynaklara erişebilmeye devam etmek için belirteçleri süresi dolduktan sonra yenileyin. Son noktaya başka `POST` bir istek `/token` göndererek belirteci yenileyebilirsiniz. Bu kez, `refresh_token` `code` parametre yerine parametre yi sağlayın:

```HTTP
POST {tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1
Host: {tenant}.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| {kiracı} | Evet | Azure AD B2C kiracınızın adı |
| {ilke} | Evet | Özgün yenileme belirteci elde etmek için kullanılan kullanıcı akışı. Bu istekte farklı bir kullanıcı akışı kullanamazsınız. Bu parametreyi POST gövdesine değil, sorgu dizesi sine ekleyin. |
| client_id | Evet | [Azure portalının](https://portal.azure.com/) uygulamanıza atadığı uygulama kimliği. |
| client_secret | Evet, Web Apps'ta | [Azure portalında](https://portal.azure.com/)oluşturulan uygulama sırrı. İstemci sırları, istemcinin bir istemci sırrını güvenli bir şekilde depolayabildiği Web Uygulaması senaryoları için bu akışta kullanılır. Yerel Uygulama (ortak istemci) senaryoları için istemci sırları güvenli bir şekilde depolanamaz, bu çağrıda kullanılmaz. Bir istemci sırrı kullanıyorsanız, lütfen periyodik olarak değiştirin. |
| grant_type | Evet | Yetkilendirme kodu akışının bu bölümü için yenilenme belirteci olması gereken hibe türü. |
| refresh_token | Evet | Akışın ikinci bölümünde edinilen orijinal yenileme belirteci. Yenileme `offline_access` belirteci almak için kapsam hem yetkilendirme hem de belirteç isteklerinde kullanılmalıdır. |
| redirect_uri | Hayır | Yetkilendirme `redirect_uri` kodunu aldığınız uygulamanın parametresi. |
| scope | Hayır | Alan ayrılmış kapsamlistesi. Kapsam, `openid` kullanıcıda oturum açma ve kullanıcı hakkında kimlik belirteçleri şeklinde veri alma iznini gösterir. Uygulamanızın istemciyle aynı uygulama kimliğiyle temsil edilen kendi arka uç web API'sine belirteçler göndermek için kullanılabilir. Kapsam, `offline_access` uygulamanızın kaynaklara genişletilmiş erişim için yenibir belirteci gerektiğini gösterir. |

Başarılı bir belirteç yanıtı gibi görünür:

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
| --------- | ----------- |
| not_before | Belirteç geçerli olarak kabul edilir zaman, dönem içinde. |
| token_type | Belirteç türü değeri. `Bearer`desteklenen tek türdür. |
| access_token | İstenilen imzalı JWT jetonu. |
| scope | Belirteç için geçerli olduğu kapsam. |
| expires_in | Erişim belirtecinin geçerli olduğu süre uzunluğu (saniye cinsinden). |
| refresh_token | Bir OAuth 2.0 yenileme belirteci. Geçerli belirteç süresi dolduktan sonra ek belirteçler elde etmek için bu belirteci kullanabilirsiniz. Yenileme belirteçleri, kaynaklara erişimi uzun süre tutmak için kullanılabilir. |

Hata yanıtları gibi görünür:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılabilecek bir kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemeye yardımcı olabilecek bir ileti. |

## <a name="send-a-sign-out-request"></a>Oturum açma isteği gönderme

Kullanıcıyı uygulamadan çıkarmak istediğinizde, uygulamanın tanımlama bilgilerini temizlemek veya kullanıcıyla oturumu sona erdirmek yeterli değildir. Oturumdan çıkmak için kullanıcıyı Azure AD B2C'ye yönlendirin. Bunu yapmazsanız, kullanıcı kimlik bilgilerini yeniden girmeden uygulamanızın yeniden kimlik doğrulaması yapabilir.

Kullanıcıyı oturum dışı kakmak için, kullanıcıyı daha önce açıklanan OpenID Connect meta veri belgesinde listelenen `end_session` bitiş noktasına yönlendirin:

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/logout?post_logout_redirect_uri=https%3A%2F%2Fjwt.ms%2F
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| {kiracı} | Evet | Azure AD B2C kiracınızın adı |
| {ilke} | Evet | Kullanıcıyı uygulamanızdan çıkarmak için kullanmak istediğiniz kullanıcı akışı. |
| id_token_hint| Hayır | Son kullanıcının istemciyle geçerli kimlik doğrulaması oturumu hakkında ipucu olarak oturum sonu noktasına geçmek için daha önce verilmiş bir kimlik belirteci. Azure `id_token_hint` AD B2C uygulama ayarlarınızda kayıtlı bir yanıt URL'si `post_logout_redirect_uri` olmasını sağlar. |
| client_id | Hayır* | [Azure portalının](https://portal.azure.com/) uygulamanıza atadığı uygulama kimliği.<br><br>\**Bu, yalıtım `Application` SSO yapılandırması kullanılırken gereklidir ve oturum açma `No`isteğinde _Kimlik Belirteci Gerektirir'ı_ .* |
| post_logout_redirect_uri | Hayır | Başarılı bir şekilde oturum dışı attıktan sonra kullanıcının yönlendirilmesi gereken URL. Azure AD B2C, kullanıcıya genel bir ileti gösterir. Bir `id_token_hint`, bu URL'yi Azure AD B2C uygulama ayarlarınızda yanıt URL'si olarak kaydetmemelisiniz. |
| durum | Hayır | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

### <a name="secure-your-logout-redirect"></a>Oturum açma yeniden yönlendirmenizi güvenli hale

Oturum açtıktan sonra, kullanıcı, uygulama için belirtilen `post_logout_redirect_uri` yanıt URL'lerinden bağımsız olarak parametrede belirtilen URI'ye yönlendirilir. Ancak, geçerli `id_token_hint` bir geçmişse, Azure AD B2C, yeniden yönlendirmegerçekleştirmeden önce uygulamanın `post_logout_redirect_uri` yapılandırılmış yeniden yönlendirme URL'lerinden biriyle eşleşen bir değer olduğunu doğrular. Uygulama için eşleşen yanıt URL'si yapılandırılmamışsa, bir hata iletisi görüntülenir ve kullanıcı yönlendirilmez.

### <a name="external-identity-provider-sign-out"></a>Dış kimlik sağlayıcı oturum açma

Kullanıcıyı `end_session` bitiş noktasına yönlendirmek, kullanıcının Azure AD B2C ile tek oturum açma durumunu temizler, ancak kullanıcıyı sosyal kimlik sağlayıcısı (IDP) oturumunun dışına çıkarmaz. Kullanıcı sonraki oturum açma sırasında aynı IDP'yi seçerse, kimlik bilgilerini girmeden yeniden kimlik doğrulaması yapılır. Bir kullanıcı uygulamadan çıkmak isterse, bu mutlaka Facebook hesabından oturum imzalamak istediği anlamına gelmez. Ancak, yerel hesaplar kullanılırsa, kullanıcının oturumu düzgün bir şekilde sona erer.
