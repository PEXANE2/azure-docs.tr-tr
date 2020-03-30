---
title: Belirteçlere Genel Bakış - Azure Active Directory B2C
description: Azure Active Directory B2C'de kullanılan belirteçler hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/27/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cbbd083a6b62733d71c316af95dffaa188b28955
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186497"
---
# <a name="overview-of-tokens-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki belirteçlere genel bakış

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), her [kimlik doğrulama akışını](application-types.md)işlerken çeşitli güvenlik belirteçleri yayır. Bu belge, her belirteç türünün biçimini, güvenlik özelliklerini ve içeriğini açıklar.

## <a name="token-types"></a>Belirteç türleri

Azure AD B2C, kimlik doğrulama ve kaynaklara güvenli erişim için belirteçlerden yararlanan [OAuth 2.0 ve OpenID Connect protokollerini](protocols-overview.md)destekler. Azure AD B2C'de kullanılan tüm belirteçler, taşıyıcı ve belirteç konusu hakkında bilgi iddiaları içeren [JSON web belirteçleridir (JWTs).](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)

Aşağıdaki belirteçler Azure AD B2C ile iletişimde kullanılır:

- *Kimlik belirteci* - Uygulamanızdaki kullanıcıları tanımlamak için kullanabileceğiniz talepleri içeren bir JWT. Bu belirteç, aynı uygulama veya hizmetin iki bileşeni arasında iletişim için HTTP isteklerinde güvenli bir şekilde gönderilir. Talepleri uygun gördüğünüz şekilde bir kimlik belirtecinde kullanabilirsiniz. Bunlar genellikle hesap bilgilerini görüntülemek veya bir uygulamada erişim denetimi kararları vermek için kullanılır. Kimlik belirteçleri imzalanır, ancak şifrelenmez. Uygulamanız veya API bir kimlik belirteci aldığında, belirteç orijinal olduğunu kanıtlamak için imzayı doğrulaması gerekir. Uygulamanız veya API'nizin geçerli olduğunu kanıtlamak için belirtecinizdeki birkaç talebi de doğrulaması gerekir. Senaryo gereksinimlerine bağlı olarak, bir uygulama tarafından doğrulanmış talepler değişebilir, ancak uygulamanızın her senaryoda bazı yaygın talep doğrulamaları gerçekleştirmesi gerekir.
- *Erişim belirteci* - API'lerinize verilen izinleri tanımlamak için kullanabileceğiniz talepleri içeren bir JWT. Erişim belirteçleri imzalanmış, ancak şifrelenmemiştir. Erişim belirteçleri, API'lere ve kaynak sunucularına erişim sağlamak için kullanılır.  API'niz bir erişim belirteci aldığında, belirteçorijinal olduğunu kanıtlamak için imzayı doğrulaması gerekir. API'nizin geçerli olduğunu kanıtlamak için belirtecinizdeki birkaç talebi de doğrulaması gerekir. Senaryo gereksinimlerine bağlı olarak, bir uygulama tarafından doğrulanmış talepler değişebilir, ancak uygulamanızın her senaryoda bazı yaygın talep doğrulamaları gerçekleştirmesi gerekir.
- *Belirteçleri yenileyin* - Yenileme belirteçleri, OAuth 2.0 akışında yeni kimlik belirteçleri ve erişim belirteçleri elde etmek için kullanılır. Uygulamanıza, bu kullanıcılarla etkileşim gerektirmeden kullanıcılar adına kaynaklara uzun süreli erişim sağlarlar. Yenileme belirteçleri uygulamanız için opak. Azure AD B2C tarafından verilir ve yalnızca Azure AD B2C tarafından denetlenebilir ve yorumlanabilir. Bunlar uzun ömürlüdür, ancak başvurunuz, yenilenme belirteci belirli bir süre sürecek beklentisi ile yazılmış olmamalıdır. Yenileme belirteçleri çeşitli nedenlerle her an geçersiz kılınabilir. Uygulamanızın yenibir belirteci geçerli olup olmadığını bilmesinin tek yolu, Azure AD B2C'ye belirteç isteğinde bulunarak bu belirteci kullanmaktır. Yeni bir belirteç için yeni bir belirteç kullandığınızda, belirteç yanıtında yeni bir yenileme belirteci alırsınız. Yeni yenileme belirteci kaydedin. İstekte daha önce kullandığınız yenileme belirtecinin yerini alır. Bu eylem, yenileme belirteçlerinizin mümkün olduğunca uzun süre geçerli kalmasını garanti eder.

## <a name="endpoints"></a>Uç Noktalar

Kayıtlı bir [uygulama](tutorial-register-applications.md) belirteçleri alır ve aşağıdaki uç noktalara istekler göndererek Azure AD B2C ile iletişim kurar:

- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/authorize`
- `https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/oauth2/v2.0/token`

Uygulamanızın Azure AD B2C'den aldığı güvenlik belirteçleri, son noktalardan `/authorize` veya `/token` uç noktalardan gelebilir. Kimlik belirteçleri `/authorize` bitiş noktasından edinildiğinde, genellikle JavaScript tabanlı web uygulamalarında oturum açan kullanıcılar için kullanılan [örtük akış](implicit-flow-single-page-application.md)kullanılarak yapılır. Kimlik belirteçleri `/token` bitiş noktasından edinildiğinde, belirteci tarayıcıdan gizli tutan yetkilendirme kodu [akışı](openid-connect.md#get-a-token)kullanılarak yapılır.

## <a name="claims"></a>Talepler

Azure AD B2C'yi kullandığınızda, belirteçlerinizin içeriği üzerinde ince ayarlı denetime sahip olursunuz. Uygulamanız için gerekli olan taleplerde belirli kullanıcı veri [kümelerini](custom-policy-overview.md) göndermek için [kullanıcı akışlarını](user-flow-overview.md) ve özel ilkeleri yapılandırabilirsiniz. Bu talepler **displayName** ve **emailAddress**gibi standart özellikleri içerebilir. Uygulamalarınız, kullanıcıların ve isteklerin güvenli bir şekilde doğruluğunu doğrulamak için bu talepleri kullanabilir.

Kimlik belirteçleri deki talepler belirli bir sırada döndürülmez. Yeni talepler herhangi bir zamanda kimlik belirteçleri tanıtılabilir. Yeni talepler sunulduğuiçin başvurunuz bozulmamalıdır. Taleplerinize [özel kullanıcı öznitelikleri](user-flow-custom-attributes.md) de ekleyebilirsiniz.

Aşağıdaki tabloda, Azure AD B2C tarafından verilen kimlik belirteçleri ve erişim belirteçlerinde bekleyebileceğiniz talepler listelenebilebilir.

| Adı | İste | Örnek değer | Açıklama |
| ---- | ----- | ------------- | ----------- |
| Hedef kitle | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Belirteci amaçlanan alıcı tanımlar. Azure AD B2C için hedef kitle uygulama kimliğidir. Uygulamanız bu değeri doğrulamalı ve eşleşmezse belirteci reddetmelidir. İzleyici kaynakla eş anlamlıdır. |
| Veren | `iss` |`https://{tenant}.b2clogin.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Belirteci oluşturan ve döndüren güvenlik belirteci hizmetini (STS) tanımlar. Ayrıca, kullanıcının kimlik doğrulaması yapılan dizini de tanımlar. Başvurunuz, belirteçlerin uygun bitiş noktasından geldiğinden emin olmak için veren talebini doğrulamalıdır. |
| Verilen | `iat` | `1438535543` | Belirteci verildiği zaman, dönem içinde temsil edilir. |
| Sona erme süresi | `exp` | `1438539443` | Belirteç geçersiz hale gelir, dönem içinde temsil zaman. Başvurunuz, belirteç ömrünün geçerliliğini doğrulamak için bu talebi kullanmalıdır. |
| Daha önce değil | `nbf` | `1438535543` | Belirteç geçerli olur zaman, dönem içinde temsil edilir. Bu süre genellikle belirteç verilen saatle aynıdır. Başvurunuz, belirteç ömrünün geçerliliğini doğrulamak için bu talebi kullanmalıdır. |
| Sürüm | `ver` | `1.0` | Azure AD B2C tarafından tanımlandığı gibi kimlik belirteci sürümü. |
| Kod karma | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Yalnızca belirteç OAuth 2.0 yetki koduyla birlikte verildiğinde, kimlik belirtecine dahil edilen kod karma. Bir kod karma bir yetkilendirme kodunun orijinalliğini doğrulamak için kullanılabilir. Bu doğrulamanın nasıl gerçekleştirilebildiğini hakkında daha fazla bilgi için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html)bakın.  |
| Erişim belirteç karma | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Yalnızca belirteç OAuth 2.0 erişim belirteciyle birlikte verildiğinde, kimlik belirtecine dahil olan bir erişim belirteci hası. Erişim belirteci karma, bir erişim belirteciorijinalliğini doğrulamak için kullanılabilir. Bu doğrulamanın nasıl gerçekleştirilebildiğini öğrenmek için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html) bakın  |
| Nonce | `nonce` | `12345` | Nonce, belirteç tekrar saldırılarını azaltmak için kullanılan bir stratejidir. Başvurunuz, sorgu parametresini kullanarak yetkilendirme `nonce` isteğinde bir nonce belirtebilir. İstekte sağladığınız değer, yalnızca bir kimlik `nonce` belirteci talebinde değiştirilmeden yayılır. Bu talep, uygulamanızın istekte belirtilen değere göre değeri doğrulamasına olanak tanır. Başvurunuz, kimlik belirteci doğrulama işlemi sırasında bu doğrulamayı gerçekleştirmelidir. |
| Özne | `sub` | `884408e1-2918-4cz0-b12d-3aa027d7563b` | Belirteci hakkında bir uygulamanın kullanıcı gibi bilgi, iddia ana. Bu değer değişmez dir ve yeniden atanamaz veya yeniden kullanılamaz. Belirteç bir kaynağa erişmek için ne zaman kullanıldığı gibi yetkilendirme denetimlerini güvenli bir şekilde gerçekleştirmek için kullanılabilir. Varsayılan olarak, özne talebi dizindeki kullanıcının nesne kimliğiyle doldurulur. |
| Kimlik doğrulama bağlam ı sınıfı başvurusu | `acr` | Uygulanamaz | Yalnızca eski ilkelerle kullanılır. |
| Güven çerçeve ilkesi | `tfp` | `b2c_1_signupsignin1` | Kimlik belirteci elde etmek için kullanılan ilkeadı. |
| Kimlik doğrulama süresi | `auth_time` | `1438535543` | Bir kullanıcının dönem içinde temsil edilen kimlik bilgilerini en son girdiği saat. Kimlik doğrulamanın yeni bir oturum açma, tek bir oturum açma (SSO) oturumu veya başka bir oturum açma türü arasında hiçbir ayrım yoktur. Uygulamanın `auth_time` (veya kullanıcının) Azure AD B2C'ye karşı en son kimlik doğrulama denemesi başlatmasI dır. Kimlik doğrulaması için kullanılan yöntem farklılaşmaz. |
| Kapsam | `scp` | `Read`| Erişim jetonu için kaynağa verilen izinler. Verilen birden çok izin bir boşlukla ayrılır. |
| Yetkili Parti | `azp` | `975251ed-e4f5-4efd-abcb-5f1a8f566ab7` | İsteği başlatan istemci uygulamasının **başvuru kimliği.** |

## <a name="configuration"></a>Yapılandırma

Aşağıdaki özellikler, Azure AD B2C tarafından yayılan [güvenlik belirteçlerinin kullanım ömürlerini yönetmek](configure-tokens.md) için kullanılır:

- **& kimlik belirteç ömürleri (dakika)** - Korumalı bir kaynağa erişmek için kullanılan OAuth 2.0 taşıyıcı belirtecisinin kullanım ömrü. Varsayılan değer 60 dakikadır. Minimum (dahil) 5 dakikadır. Maksimum (dahil) 1440 dakikadır.

- **Belirteç ömrünü yenileme (gün)** - Yeni bir giriş belirteci yeni bir erişim veya kimlik belirteci elde etmek için kullanılabilir önce maksimum süre. Bu süre, `offline_access` başvurunuza kapsam verilmişse yeni bir yenileme belirteci edinmeyi de kapsar. Varsayılan değer 14 gündür. Minimum (dahil) bir gündür. Maksimum (dahil) 90 gündür.

- **Belirteç kaydırma penceresi nin kullanım ömrünü (gün) yenileme** - Bu süre geçtikten sonra kullanıcı, uygulama tarafından edinilen en son yenileme belirtecinin geçerlilik süresine bakılmaksızın yeniden kimlik doğrulamayapmak zorunda kalır. Yalnızca anahtar **Bounded**olarak ayarlanmışsa sağlanabilir. **Yenile belirteç ömrü (gün)** değerinden daha büyük veya eşit olması gerekir. Anahtar **Sınırsız**olarak ayarlanmışsa, belirli bir değer sağlayamazsınız. Varsayılan değer 90 gündür. Minimum (dahil) bir gündür. Maksimum (dahil) 365 gündür.

Aşağıdaki kullanım örnekleri bu özellikler kullanılarak etkinleştirilir:

- Kullanıcı uygulamada sürekli olarak etkin olduğu sürece, kullanıcının mobil uygulamada süresiz olarak oturum açmış olarak kalmasına izin verin. Oturum açma kullanıcı akışınızda **Belirteç kaydırma penceresini ömür boyu (gün)** **Sınırsız** olarak ayarlayabilirsiniz.
- Uygun erişim belirteç kullanım ömürlerini ayarlayarak endüstrinizin güvenlik ve uyumluluk gereksinimlerini karşılayın.

Bu ayarlar parola sıfırlama kullanıcı akışları için kullanılamaz.

## <a name="compatibility"></a>Uyumluluk

Aşağıdaki özellikler [belirteç uyumluluğunu yönetmek](configure-tokens.md)için kullanılır:

- **Veren (iss) talebi** - Bu özellik, belirteci veren Azure AD B2C kiracısını tanımlar. Varsayılan değer: `https://<domain>/{B2C tenant GUID}/v2.0/`. Hem Azure `https://<domain>/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/` AD B2C kiracısı hem de belirteç isteğinde kullanılan kullanıcı akışı için kullanılan iD'lerin değeri. Uygulamanızın veya kitaplığınuzun [OpenID Connect Discovery 1.0 spec](https://openid.net/specs/openid-connect-discovery-1_0.html)ile uyumlu olması için Azure AD B2C'ye ihtiyacı varsa, bu değeri kullanın.

- **Konu (alt) talep** - Bu özellik, belirteç bilgi iddia varlık tanımlar. Varsayılan değer **ObjectID'dir**ve `sub` bu değer kullanıcının nesne kimliğiyle birlikte belirteçteki talebi doldurur. **Desteklenmeyen'in** değeri yalnızca geriye dönük uyumluluk için sağlanır. Mümkün olur olmaz **ObjectID'ye** geçmeniz önerilir.

- **İlke Kimliğini temsil eden talep** - Bu özellik, belirteç isteğinde kullanılan ilke adının doldurulan talep türünü tanımlar. Varsayılan değer: `tfp`. Değeri `acr` yalnızca geriye dönük uyumluluk için sağlanır.

## <a name="pass-through"></a>Geçiş

Bir kullanıcı yolculuğu başladığında, Azure AD B2C bir kimlik sağlayıcısından bir erişim jetonu alır. Azure AD B2C bu belirteci kullanıcı hakkında bilgi almak için kullanır. [Kullanıcı akışınızda bir talebi etkinleştirir](idp-pass-through-user-flow.md) veya [özel politikanızda bir talep tanımlar](idp-pass-through-custom.md) ve belirteci Azure AD B2C'de kaydettiğiniz uygulamalara aktarırsınız. Başvurunuz, jetonu talep olarak aktarmak için [v2 kullanıcı akışı](user-flow-versions.md) kullanıyor olmalıdır.

Azure AD B2C şu anda yalnızca Facebook ve Google'ı içeren OAuth 2.0 kimlik sağlayıcılarının erişim jetonunun geçirilmesini desteklemektedir. Diğer tüm kimlik sağlayıcıları için talep boş döndürülür.

## <a name="validation"></a>Doğrulama

Bir belirteci doğrulamak için, uygulamanızın belirteci hem imzasını hem de taleplerini denetlemesi gerekir. Tercih ettiğiniz dile bağlı olarak, JWT'leri doğrulamak için birçok açık kaynak kitaplık mevcuttur. Kendi doğrulama mantığınızı uygulamak yerine bu seçenekleri keşfetmeniz önerilir.

### <a name="validate-signature"></a>İmzayı doğrulama

Bir JWT üç segmentleri, bir *üstbilgi,* bir *gövde*ve bir *imza*içerir. İmza kesimi, başvurunuz tarafından güvenilebilmek için belirteciorijinalliğini doğrulamak için kullanılabilir. Azure AD B2C belirteçleri RSA 256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanılarak imzalanır.

Belirteci üstbilgi, belirteci imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

**Alg** talebinin değeri belirteci imzalamak için kullanılan algoritmadır. **Çocuk** iddiasının değeri, jetonu imzalamak için kullanılan ortak anahtardır. Azure AD B2C herhangi bir zamanda, bir dizi genel özel anahtar çiftinden birini kullanarak bir belirteç imzalayabilir. Azure AD B2C olası anahtar kümesini düzenli aralıklarla döndürür. Başvurunuz bu önemli değişiklikleri otomatik olarak işlemek için yazılmalıdır. Azure AD B2C tarafından kullanılan ortak anahtarlara yapılan güncelleştirmeleri denetlemek için makul bir sıklık her 24 saatte bir dir.

Azure AD B2C'nin OpenID Connect meta veri bitiş noktası vardır. Bu bitiş noktasını kullanan uygulamalar, çalışma zamanında Azure AD B2C hakkında bilgi isteyebilir. Bu bilgiler uç noktaları, belirteç içeriğini ve belirteç imzalama anahtarlarını içerir. Azure AD B2C kiracınız her ilke için bir JSON meta veri belgesi içerir. Meta veri belgesi, birkaç yararlı bilgi parçası içeren bir JSON nesnesidir. Meta veriler, belirteçleri imzalamak için kullanılan ortak anahtarlar kümesinin konumunu veren **jwks_uri**içerir. Bu konum burada sağlanır, ancak meta veri belgesini kullanarak ve ayrıştırma **jwks_uri:**

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_signupsignin1
```
Bu URL'de bulunan JSON belgesi, belirli bir anda kullanılan tüm genel anahtar bilgilerini içerir. `kid` Uygulamanız, belirli bir belirteci imzalamak için kullanılan JSON belgesindeki ortak anahtarı seçmek için JWT üstbilgisindeki talebi kullanabilir. Daha sonra doğru ortak anahtar ve belirtilen algoritmayı kullanarak imza doğrulama gerçekleştirebilirsiniz.

`contoso.onmicrosoft.com` Kiracıdaki `B2C_1_signupsignin1` ilke için meta veri belgesi şu adreste bulunur:

```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_signupsignin1
```

Belirteci imzalamak için hangi iidi nin kullanıldığını (ve meta verileri istemek için nereye gideceğinizi) belirlemek için iki seçeneğiniz vardır. İlk olarak, ilke adı `acr` belirteci içinde talep dahildir. İddiaları, temel-64'ün gövdesini çözerek ve json dizesini deserialize ederek JWT'nin gövdesinden ayrışdırabilirsiniz. Talep, `acr` belirteci vermek için kullanılan ilkenin adıdır. Diğer seçenek, isteği siz yaptığınızda ilkeyi `state` parametrenin değerinde kodlamak ve sonra hangi iidi kullanılan belirlemek için deşifre etmektir. Her iki yöntem de geçerlidir.

İmza doğrulamanın nasıl gerçekleştirilebildiğini anlatan bir açıklama, bu belgenin kapsamı dışındadır. Bir belirteci doğrulamanıza yardımcı olmak için birçok açık kaynak kitaplık kullanılabilir.

### <a name="validate-claims"></a>Talepleri doğrulama

Uygulamalarınız veya API'niz bir kimlik belirteci aldığında, kimlik belirtecindeki taleplere karşı da birkaç denetim gerçekleştirmelidir. Aşağıdaki talepler kontrol edilmelidir:

- **hedef kitle** - Kimlik belirteci nin başvurunuza verilmesini amaçladığını doğrular.
- **önce** ve **son kullanma süresi** - Kimlik belirteci süresinin dolmadığını doğrular.
- **veren** - Belirteçlerin uygulamanız için Azure AD B2C tarafından verildiğini doğrular.
- **nonce** - belirteç tekrar saldırı azaltma için bir strateji.

Uygulamanızın gerçekleştirmesi gereken doğrulamaların tam listesi için [OpenID Connect belirtimine](https://openid.net)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Erişim belirteçlerini](access-tokens.md)nasıl kullanacağınız hakkında daha fazla bilgi edinin.

