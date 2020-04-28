---
title: OpenID Connect ile Web 'de oturum açma-Azure Active Directory B2C
description: Azure Active Directory B2C 'de OpenID Connect kimlik doğrulama protokolünü kullanarak Web uygulamaları oluşturun.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79264394"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'de OpenID Connect ile Web oturumu açma

OpenID Connect, OAuth 2,0 üzerinde oluşturulan ve kullanıcıları Web uygulamalarına güvenli bir şekilde imzalamak için kullanılabilen bir kimlik doğrulama protokolüdür. OpenID Connect 'in Azure Active Directory B2C (Azure AD B2C) uygulamasını kullanarak, Web uygulamalarınızda bulunan kaydolma, oturum açma ve diğer kimlik yönetimi deneyimlerini Azure Active Directory (Azure AD) için dış olarak aktarabilirsiniz. Bu kılavuzda, bunu dilden bağımsız bir şekilde nasıl yapabileceğiniz gösterilmektedir. Açık kaynaklı kitaplıklarımızın hiçbirini kullanmadan HTTP iletilerinin nasıl gönderileceğini ve alınacağını açıklar.

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) , OAuth 2,0 *Yetkilendirme* protokolünü bir *kimlik doğrulama* protokolü olarak kullanılmak üzere genişletir. Bu kimlik doğrulama protokolü, çoklu oturum açma gerçekleştirmenize olanak tanır. İstemcinin kullanıcı kimliğini doğrulamasını ve Kullanıcı hakkında temel profil bilgilerini almasını sağlayan bir *kimlik belirteci*kavramını tanıtır.

OAuth 2,0 ' i genişlettiğinden, uygulamaların *erişim belirteçlerini*güvenli bir şekilde almasına de olanak sağlar. Bir [yetkilendirme sunucusu](protocols-overview.md)tarafından güvenliği sağlanmış kaynaklara erişmek için erişim belirteçlerini kullanabilirsiniz. Sunucuda barındırılan ve bir tarayıcıdan erişilen bir Web uygulaması oluşturuyorsanız OpenID Connect önerilir. Belirteçler hakkında daha fazla bilgi için bkz. [Azure Active Directory B2C belirteçlere genel bakış](tokens-overview.md)

Azure AD B2C, standart OpenID Connect protokolünü basit kimlik doğrulaması ve yetkilendirmeden daha fazlasını yapmak için genişletir. Uygulamanıza kaydolma, oturum açma ve profil yönetimi gibi kullanıcı deneyimleri eklemek için OpenID Connect 'i kullanmanızı sağlayan [Kullanıcı akışı parametresini](user-flow-overview.md)tanıtır.

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteklerini gönder

Web uygulamanızın kullanıcı kimliğini doğrulaması ve bir Kullanıcı akışı çalıştırması gerektiğinde, kullanıcıyı `/authorize` uç noktaya yönlendirebilir. Kullanıcı, Kullanıcı akışına bağlı olarak eylem gerçekleştirir.

Bu istekte istemci, `scope` parametresindeki kullanıcıdan almaları gereken izinleri gösterir ve çalıştırılacak Kullanıcı akışını belirtir. İsteğin nasıl çalıştığına ilişkin bir fikir almak için, isteği bir tarayıcıya yapıştırmayı ve çalıştırmayı deneyin. Kiracınızın adıyla değiştirin `{tenant}` . Daha `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` önce kiracınıza kaydettiğiniz UYGULAMANıN uygulama kimliğiyle değiştirin. Ayrıca, ilke adını ()`{policy}`kiracınızda bulunan ilke adıyla (örneğin `b2c_1_sign_in`,) değiştirin.

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
| Kiracı | Yes | Azure AD B2C kiracınızın adı |
| ilkesinin | Yes | Çalıştırılacak Kullanıcı akışı. Azure AD B2C kiracınızda oluşturduğunuz Kullanıcı akışının adını belirtin. Örneğin: `b2c_1_sign_in`, `b2c_1_sign_up`, veya. `b2c_1_edit_profile` |
| client_id | Yes | [Azure Portal](https://portal.azure.com/) uygulamanıza atanan uygulama kimliği. |
| nonce | Yes | İstek olarak ortaya çıkan KIMLIK belirtecine dahil edilen isteğe (uygulama tarafından oluşturulan) dahil bir değer. Daha sonra uygulama, belirteç yeniden yürütme saldırılarını azaltmak için bu değeri doğrulayabilirler. Değer genellikle, isteğin kaynağını belirlemek için kullanılan rastgele benzersiz bir dizedir. |
| response_type | Yes | OpenID Connect için bir KIMLIK belirteci içermelidir. Web uygulamanız, bir Web API 'SI çağırmak için belirteçler de gerekiyorsa, kullanabilirsiniz `code+id_token`. |
| scope | Yes | Kapsamların boşlukla ayrılmış listesi. `openid` Kapsam, kullanıcıya oturum açma ve kimlik belirteçleri biçimindeki Kullanıcı hakkında veri edinme iznini gösterir. Kapsam `offline_access` , Web uygulamaları için isteğe bağlıdır. Uygulamanızın, kaynaklara genişletilmiş erişim için *yenileme belirtecine* sahip olacağını belirtir. |
| isteme | Hayır | Gerekli Kullanıcı etkileşimi türü. Şu anda geçerli olan tek değer, kullanıcıyı `login`bu istek üzerine kimlik bilgilerini girmeye zorlayan olur. |
| redirect_uri | Hayır | Uygulamanızın `redirect_uri` kimlik doğrulama yanıtlarının gönderilebileceği ve alınabileceği, uygulamanızın parametresi. URL kodlamalı olması dışında, Azure portal kaydettiğiniz `redirect_uri` parametrelerden biriyle tam olarak eşleşmesi gerekir. |
| response_mode | Hayır | Elde edilen yetkilendirme kodunu uygulamanıza geri göndermek için kullanılan yöntem. Ya da olabilir `query` `form_post` `fragment`.  En `form_post` iyi güvenlik için yanıt modu önerilir. |
| durum | Hayır | İsteğin belirteç yanıtında de döndürülen bir değeri. İstediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer genellikle siteler arası istek sahteciliği saldırılarını önlemek için kullanılır. Durum Ayrıca, kullanıcının uygulamadaki durumuyla ilgili bilgileri, üzerinde bulundukları sayfa gibi kimlik doğrulama isteği yapılmadan önce kodlamak için de kullanılır. |

Bu noktada, kullanıcıdan iş akışını tamamlaması istenir. Kullanıcının Kullanıcı adı ve parolasını girmesi, sosyal kimlik ile oturum açması veya dizine kaydolmanız gerekebilir. Kullanıcı akışının nasıl tanımlandığına bağlı olarak başka herhangi bir sayıda adım olabilir.

Kullanıcı Kullanıcı akışını tamamladıktan sonra, `redirect_uri` `response_mode` parametresinde belirtilen yöntemi kullanarak, belirtilen parametrede uygulamanıza bir yanıt döndürülür. Yanıt, Kullanıcı akışından bağımsız olarak, önceki durumların her biri için aynıdır.

Kullanarak `response_mode=fragment` başarılı bir yanıt şöyle görünür:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| id_token | Uygulamanın istediği KIMLIK belirteci. KIMLIK belirtecini, kullanıcının kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için kullanabilirsiniz. |
| kod | Kullandıysanız `response_type=code+id_token`, uygulamanın istediği yetkilendirme kodu. Uygulama, bir hedef kaynak için erişim belirteci istemek üzere yetkilendirme kodunu kullanabilir. Yetkilendirme kodları genellikle yaklaşık 10 dakika sonra sona erer. |
| durum | İsteğe bir `state` parametre dahil ise, yanıtta aynı değer görünmelidir. Uygulamanın, istek ve yanıt `state` değerlerinin özdeş olduğunu doğrulaması gerekir. |

Ayrıca, uygulamanın bunları uygun şekilde işleyebilmesi `redirect_uri` için hataya yanıt, parametreye gönderilebilir:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılabilen bir kod. |
| error_description | Bir kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum | İsteğe bir `state` parametre dahil ise, yanıtta aynı değer görünmelidir. Uygulamanın, istek ve yanıt `state` değerlerinin özdeş olduğunu doğrulaması gerekir. |

## <a name="validate-the-id-token"></a>KIMLIK belirtecini doğrulama

Yalnızca bir KIMLIK belirtecinin alınması, kullanıcının kimliğini doğrulamak için yeterli değil. KIMLIK belirtecinin imzasını doğrulayın ve uygulamanızın gereksinimlerine göre belirteçteki talepleri doğrulayın. Azure AD B2C, belirteçleri imzalamak ve bunların geçerli olduğunu doğrulamak için [JSON Web belirteçleri (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve ortak anahtar şifrelemesi kullanır. Tercihinizin diline bağlı olarak JWTs 'yi doğrulamak için kullanılabilen çok sayıda açık kaynak kitaplığı vardır. Kendi doğrulama mantığınızı uygulamak yerine bu seçenekleri araştırmayı öneririz.

Azure AD B2C, bir uygulamanın çalışma zamanında Azure AD B2C hakkında bilgi almasına izin veren bir OpenID Connect meta veri uç noktası vardır. Bu bilgiler uç noktaları, belirteç içerikleri ve belirteç imzalama anahtarlarını içerir. B2C kiracınızdaki her Kullanıcı akışı için bir JSON meta veri belgesi vardır. Örneğin, içinde `b2c_1_sign_in` `fabrikamb2c.onmicrosoft.com` Kullanıcı akışı için meta veri belgesi şu konumda bulunur:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/v2.0/.well-known/openid-configuration
```

Bu yapılandırma belgesinin özelliklerinden biri `jwks_uri`, aynı kullanıcı akışı değeri şöyle olacaktır:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/discovery/v2.0/keys
```

Bir KIMLIK belirtecini imzalarken (ve meta verileri nereden alacağınız) hangi Kullanıcı akışının kullanıldığını anlamak için iki seçeneğiniz vardır. İlk olarak, Kullanıcı akış adı, KIMLIK belirtecindeki `acr` talebe dahil edilir. Diğer bir seçenek de, isteği keserken Kullanıcı akışını `state` parametre değerinde kodlamak ve sonra hangi Kullanıcı akışının kullanıldığını anlamak için onu çözmez. Her iki yöntem de geçerlidir.

Meta veri belgesini OpenID Connect meta veri uç noktasından aldıktan sonra, KIMLIK belirtecinin imzasını doğrulamak için RSA 256 ortak anahtarlarını kullanabilirsiniz. Bu uç noktada, her biri bir `kid` talep tarafından tanımlanan birden fazla anahtar olabilir. KIMLIK belirtecinin üstbilgisi de bir `kid` talep içerir, bu da kimlik belirtecini imzalamak için bu anahtarların hangisinin kullanıldığını gösterir.

Azure AD B2C belirteçleri doğrulamak için üs (e) ve mod (n) kullanarak ortak anahtarı oluşturmanız gerekir. Bunun nasıl yapılacağını ilgili programlama dilinizde uygun şekilde belirlemeniz gerekir. RSA protokolüyle ortak anahtar oluşturmayla ilgili resmi belgeler şurada bulunabilir:https://tools.ietf.org/html/rfc3447#section-3.1

KIMLIK belirtecinin imzasını doğruladıktan sonra doğrulamanız gereken birkaç talep vardır. Örneğin:

- Belirteç yeniden `nonce` yürütme saldırılarını önleme talebini doğrulayın. Değeri, oturum açma isteğinde belirtidikleriniz olmalıdır.
- KIMLIK belirtecinin `aud` uygulamanız için verildiğinden emin olmak için talebi doğrulayın. Değeri uygulamanızın uygulama KIMLIĞI olmalıdır.
- KIMLIK belirtecinin `iat` zaman `exp` aşımına ermediğinden emin olmak için ve taleplerini doğrulayın.

Ayrıca gerçekleştirmeniz gereken birkaç doğrulama daha vardır. Doğrulamalar, [OpenID Connect Core belirtiminde](https://openid.net/specs/openid-connect-core-1_0.html)ayrıntılı olarak açıklanmıştır. Senaryonuza bağlı olarak ek talepler de doğrulamak isteyebilirsiniz. Bazı ortak doğrulamalar şunları içerir:

- Kullanıcı/kuruluşun uygulama için kaydolmasını sağlama.
- Kullanıcının uygun yetkilendirme/ayrıcalıklara sahip olduğundan emin olma.
- Azure Multi-Factor Authentication gibi belirli bir kimlik doğrulama gücünün meydana geldiğinden emin olma.

KIMLIK belirtecini doğruladıktan sonra kullanıcıyla bir oturum başlatabilirsiniz. Uygulamanızdaki Kullanıcı hakkında bilgi edinmek için KIMLIK belirtecindeki talepler ' i kullanabilirsiniz. Bu bilgiler için kullanımları görüntüleme, kayıtlar ve yetkilendirme içerir.

## <a name="get-a-token"></a>Belirteç al

Web uygulamanızın yalnızca Kullanıcı akışlarını çalıştırması gerekiyorsa, sonraki birkaç bölümü atlayabilirsiniz. Bu bölümler yalnızca bir Web API 'sine kimliği doğrulanmış çağrılar yapması gereken Web uygulamaları için geçerlidir ve ayrıca Azure AD B2C tarafından korunur.

Uç noktaya `response_type=code+id_token` `POST` istek göndererek istenen kaynağa bir belirteç için aldığınız yetkilendirme kodunu (kullanarak) kullanabilirsiniz. `/token` Azure AD B2C, istek içinde kapsamlarını belirterek [diğer API 'ler için her zamanki gibi erişim belirteçleri isteyebilirsiniz](access-tokens.md#request-a-token) .

Ayrıca, uygulamanın istemci KIMLIĞINI istenen kapsam olarak kullanma kuralına göre uygulamanızın kendi arka uç Web API 'SI için bir erişim belirteci isteyebilirsiniz (Bu, "hedef kitle" olarak bu istemci KIMLIĞINE sahip bir erişim belirtecine neden olur):

```HTTP
POST {tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1
Host: {tenant}.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kiracı | Yes | Azure AD B2C kiracınızın adı |
| ilkesinin | Yes | Yetkilendirme kodunu almak için kullanılan Kullanıcı akışı. Bu istekte farklı bir Kullanıcı akışı kullanamazsınız. Bu parametreyi, POST gövdesine değil sorgu dizesine ekleyin. |
| client_id | Yes | [Azure Portal](https://portal.azure.com/) uygulamanıza atanan uygulama kimliği. |
| client_secret | Evet, Web Apps | [Azure Portal](https://portal.azure.com/)oluşturulan uygulama gizli dizisi. İstemci gizli dizileri, istemcinin güvenli bir şekilde istemci gizli depolayabileceği Web uygulaması senaryolarında Bu akışta kullanılır. Yerel uygulama (genel istemci) senaryolarında, istemci gizli dizileri güvenli bir şekilde depolanamaz, Bu akışta threön kullanılmaz. İstemci parolası kullanılıyorsa, lütfen düzenli aralıklarla değiştirin. |
| kod | Yes | Kullanıcı akışının başlangıcında elde ettiğiniz yetkilendirme kodu. |
| grant_type | Yes | Yetkilendirme kodu akışı için olması `authorization_code` gereken izin türü. |
| redirect_uri | Yes | Yetkilendirme `redirect_uri` kodunu aldığınız uygulamanın parametresi. |
| scope | Hayır | Kapsamların boşlukla ayrılmış listesi. `openid` Kapsam, kullanıcının oturum açma iznini gösterir ve id_token parametreleri biçiminde kullanıcı hakkında veri alabilir. Uygulamanın kendi arka uç Web API 'sine, istemcisiyle aynı uygulama KIMLIĞIYLE temsil edilen belirteçleri almak için kullanılabilir. Kapsam `offline_access` , uygulamanızın kaynaklara genişletilmiş erişim için bir yenileme belirteci gerektiğini gösterir. |

Başarılı bir belirteç yanıtı şöyle görünür:

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
| not_before | Belirtecin geçerli kabul edildiği zaman (dönem süresi). |
| token_type | Belirteç türü değeri. `Bearer`desteklenen tek türdür. |
| access_token | İstediğiniz imzalı JWT belirteci. |
| scope | Belirtecin geçerli olduğu kapsamlar. |
| expires_in | Erişim belirtecinin geçerli olduğu sürenin uzunluğu (saniye cinsinden). |
| refresh_token | Bir OAuth 2,0 yenileme belirteci. Uygulama, geçerli belirtecin süresi dolduktan sonra ek belirteçler almak için bu belirteci kullanabilir. Yenileme belirteçleri, kaynakların uzun süreleriyle erişiminin korunması için kullanılabilir. Kapsam `offline_access` , yenileme belirteci almak için hem yetkilendirme hem de belirteç isteklerinde kullanılmış olmalıdır. |

Hata yanıtları şöyle görünür:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılan kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek bir ileti. |

## <a name="use-the-token"></a>Belirteci kullanma

Bir erişim belirtecini başarıyla edindiniz, artık, arka uç Web API 'lerinize yönelik isteklerindeki belirteçleri `Authorization` üstbilgiye ekleyerek kullanabilirsiniz:

```HTTP
GET /tasks
Host: mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Belirteci Yenile

KIMLIK belirteçlerinin kısa bir süre içinde süresi doluyor. Kaynaklara erişmek için süre dolduktan sonra belirteçleri yenileyin. Bir belirteci, `POST` `/token` uç noktaya başka bir istek göndererek yenileyebilirsiniz. Bu kez parametre yerine `refresh_token` `code` parametresini sağlayın:

```HTTP
POST {tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/token HTTP/1.1
Host: {tenant}.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kiracı | Yes | Azure AD B2C kiracınızın adı |
| ilkesinin | Yes | Özgün yenileme belirtecini almak için kullanılan Kullanıcı akışı. Bu istekte farklı bir Kullanıcı akışı kullanamazsınız. Bu parametreyi, POST gövdesine değil sorgu dizesine ekleyin. |
| client_id | Yes | [Azure Portal](https://portal.azure.com/) uygulamanıza atanan uygulama kimliği. |
| client_secret | Evet, Web Apps | [Azure Portal](https://portal.azure.com/)oluşturulan uygulama gizli dizisi. İstemci gizli dizileri, istemcinin güvenli bir şekilde istemci gizli depolayabileceği Web uygulaması senaryolarında Bu akışta kullanılır. Yerel uygulama (genel istemci) senaryolarında, istemci gizli dizileri güvenli bir şekilde depolanamaz, bu çağrıda threön kullanılmaz. İstemci parolası kullanılıyorsa, lütfen düzenli aralıklarla değiştirin. |
| grant_type | Yes | Yetkilendirme kodu akışının bu bölümü için yenileme belirteci olması gereken izin türü. |
| refresh_token | Yes | Akışın ikinci bölümünde alınan orijinal yenileme belirteci. Bir `offline_access` yenileme belirteci almak için kapsamın hem yetkilendirme hem de belirteç isteklerinde kullanılması gerekir. |
| redirect_uri | Hayır | Yetkilendirme `redirect_uri` kodunu aldığınız uygulamanın parametresi. |
| scope | Hayır | Kapsamların boşlukla ayrılmış listesi. `openid` Kapsam, kullanıcıya oturum açma ve kimlik belirteçleri biçimindeki Kullanıcı hakkında veri edinme iznini gösterir. Uygulamanın kendi arka uç Web API 'sine belirteç göndermek için, istemcisiyle aynı uygulama KIMLIĞIYLE temsil edilen belirteçleri kullanabilirsiniz. Kapsam `offline_access` , uygulamanızın kaynaklara genişletilmiş erişim için bir yenileme belirteci gerektiğini gösterir. |

Başarılı bir belirteç yanıtı şöyle görünür:

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
| not_before | Belirtecin geçerli kabul edildiği zaman (dönem süresi). |
| token_type | Belirteç türü değeri. `Bearer`desteklenen tek türdür. |
| access_token | İstenen işaretli JWT belirteci. |
| scope | Belirtecin geçerli olduğu kapsam. |
| expires_in | Erişim belirtecinin geçerli olduğu sürenin uzunluğu (saniye cinsinden). |
| refresh_token | Bir OAuth 2,0 yenileme belirteci. Uygulama, geçerli belirtecin süresi dolduktan sonra ek belirteçler almak için bu belirteci kullanabilir. Yenileme belirteçleri, kaynakların uzun süreleriyle erişiminin korunması için kullanılabilir. |

Hata yanıtları şöyle görünür:

```JSON
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılan kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek bir ileti. |

## <a name="send-a-sign-out-request"></a>Oturum kapatma isteği gönder

Kullanıcının uygulamadan oturum açmasını istediğinizde, uygulamanın tanımlama bilgilerini temizlemek veya oturumu Kullanıcı ile sonlandırmak yeterli değildir. Oturumu kapatmak için kullanıcıyı Azure AD B2C yönlendirin. Bunu yapmazsanız, Kullanıcı kimlik bilgilerini tekrar girmeden uygulamanız için yeniden kimlik doğrulaması yapabilir.

Kullanıcının oturumunu kapatmak için kullanıcıyı daha önce açıklanan OpenID Connect `end_session` meta veri belgesinde listelenen uç noktaya yönlendirin:

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/logout?post_logout_redirect_uri=https%3A%2F%2Fjwt.ms%2F
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kiracı | Yes | Azure AD B2C kiracınızın adı |
| ilkesinin | Yes | Kullanıcıyı uygulamanızda imzalamak için kullanmak istediğiniz kullanıcı akışı. |
| id_token_hint| Hayır | Son kullanıcının istemci ile geçerli kimlik doğrulamalı oturum hakkında bir ipucu olarak oturum kapatma uç noktasına geçirilecek daha önceden verilen bir KIMLIK belirteci. , `id_token_hint` Uygulamasının Azure AD B2C uygulama `post_logout_redirect_uri` ayarlarınızda kayıtlı bir yanıt URL 'si olmasını sağlar. |
| client_id | Eşleşen | [Azure Portal](https://portal.azure.com/) uygulamanıza atanan uygulama kimliği.<br><br>\**Bu, yalıtım SSO yapılandırması `Application` kullanılırken gereklidir ve oturum kapatma Isteğinde _kimlik belirteci gerektir_ olarak `No`ayarlanır.* |
| post_logout_redirect_uri | Hayır | Başarılı oturum kapatıldıktan sonra kullanıcının yeniden yönlendirilmesi gereken URL. Dahil değilse, kullanıcıya genel bir ileti gösterir Azure AD B2C. Bir `id_token_hint`sağlamazsanız, bu url 'yi Azure AD B2C uygulama ayarlarınıza bir yanıt URL 'si olarak kaydetmemelisiniz. |
| durum | Hayır | İsteğe bir `state` parametre dahil ise, yanıtta aynı değer görünmelidir. Uygulamanın, istek ve yanıt `state` değerlerinin özdeş olduğunu doğrulaması gerekir. |

### <a name="secure-your-logout-redirect"></a>Logout yeniden yönlendirmenizi güvenli hale getirin

Oturum kapatıldıktan sonra, uygulama için belirtilen yanıt URL 'Lerinden bağımsız olarak, `post_logout_redirect_uri` Kullanıcı PARAMETRESINDE belirtilen URI 'ye yönlendirilir. Ancak, geçerli `id_token_hint` bir değer geçirilirse Azure AD B2C, değerinin yeniden yönlendirmeyi gerçekleştirmeden önce uygulamanın `post_logout_redirect_uri` yapılandırılmış yeniden yönlendirme URI 'lerinden biriyle eşleştiğini doğrular. Uygulama için eşleşen bir yanıt URL 'SI yapılandırılmamışsa, bir hata iletisi görüntülenir ve Kullanıcı yeniden yönlendirilmez.

### <a name="external-identity-provider-sign-out"></a>Dış kimlik sağlayıcısı oturumu kapatma

Kullanıcının `end_session` uç noktaya yönlendirilerek, Azure AD B2C ile kullanıcının çoklu oturum açma durumu temizlenir, ancak Kullanıcı sosyal kimlik sağlayıcısı (IDP) oturumunun dışında imzalanmaz. Kullanıcı sonraki oturum açma sırasında aynı ıDP 'yi seçerse, kimlik bilgileri girilmeden bu kimlik doğrulaması yapılır. Kullanıcı uygulamanın oturumunu kapatmak isterse, Facebook hesabında oturumu kapatmak istedikleri anlamına gelmez. Ancak, yerel hesaplar kullanılıyorsa, kullanıcının oturumu doğru şekilde sona erer.
