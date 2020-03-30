---
title: Örtük akışı kullanarak tek sayfaoturum açma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C ile OAuth 2.0 örtülü akışını kullanarak tek sayfalı oturum açma eklemeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/19/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 37b59c2a23a8f00e8376be2ac4a7b35a6d58aa28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399009"
---
# <a name="single-page-sign-in-using-the-oauth-20-implicit-flow-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'deki OAuth 2.0 örtük akışını kullanarak tek sayfaoturum açma

Birçok modern uygulama öncelikle JavaScript yazılmış bir tek sayfalı uygulama ön uç var. Genellikle, uygulama React, Angular veya Vue.js gibi bir çerçeve kullanılarak yazılır. Tek sayfalı uygulamalar ve öncelikle bir tarayıcıda çalışan diğer JavaScript uygulamaları nın kimlik doğrulama için bazı ek zorlukları vardır:

- Bu uygulamaların güvenlik özellikleri geleneksel sunucu tabanlı web uygulamalarından farklıdır.
- Birçok yetkilendirme sunucusu ve kimlik sağlayıcısı, kökenler arası kaynak paylaşımı (CORS) isteklerini desteklemez.
- Tam sayfa tarayıcı uygulamadan uzağa yönlendirir kullanıcı deneyimine invaziv olabilir.

Bu uygulamaları desteklemek için Azure Active Directory B2C (Azure AD B2C), OAuth 2.0 örtük akışını kullanır. OAuth 2.0 yetki örtülü hibe [akışı, OAuth 2.0 belirtiminin 4.2 bölümünde](https://tools.ietf.org/html/rfc6749)açıklanmıştır. Örtülü akışta, uygulama doğrudan Azure Active Directory 'den (Azure AD) herhangi bir sunucu-sunucu değişimi olmaksızın bitiş noktası yetkilendirmesinden belirteçler alır. Tüm kimlik doğrulama mantığı ve oturum işleme tamamen JavaScript istemcisinde bir sayfa yönlendirme veya açılır kutu ile yapılır.

Azure AD B2C, standart OAuth 2.0 örtük akışını basit kimlik doğrulama ve yetkilendirmeden daha fazlasına genişletir. Azure AD B2C [ilke parametresini](user-flow-overview.md)tanıtir. İlke parametresi ile, uygulamanıza kaydolma, kaydolma ve profil yönetimi kullanıcı akışları gibi ilkeler eklemek için OAuth 2.0'ı kullanabilirsiniz. Bu makaledeki HTTP istekleri örneğinde **, {tenant}.onmicrosoft.com** örnek olarak kullanılır. Varsa `{tenant}` ve aynı zamanda bir kullanıcı akışı oluşturduysanız kiracınızın adı ile değiştirin.

Örtük oturum açma akışı aşağıdaki şekilde bir şeye benzer. Her adım daha sonra makalede ayrıntılı olarak açıklanmıştır.

![OpenID Connect örtük akışını gösteren şerit tarzı diyagram](./media/implicit-flow-single-page-application/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Kimlik doğrulama istekleri gönderme

Web uygulamanızın kullanıcının kimliğini doğrulaması ve kullanıcı akışını çalıştırması gerektiğinde, `/authorize` kullanıcıyı bitiş noktasına yönlendirebilir. Kullanıcı akışına bağlı olarak harekete geçer.

Bu istekte, istemci `scope` parametrede kullanıcıdan alması gereken izinleri ve çalışacak kullanıcı akışını gösterir. İsteğin nasıl çalıştığı hakkında bilgi almak için, isteği bir tarayıcıya yapıştırmayı ve çalıştırmayı deneyin. Azure `{tenant}` AD B2C kiracınızın adıyla değiştirin. Kiracınızda daha önce kaydolduğunuz uygulamanın uygulama kimliğiyle değiştirin. `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` Örneğin, `{policy}` `b2c_1_sign_in`kiracınızda oluşturduğunuz bir ilkenin adı ile değiştirin.

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
|{kiracı}| Evet | Azure AD B2C kiracınızın adı|
|{ilke}| Evet| Çalıştırılacak kullanıcı akışı. Azure AD B2C kiracınızda oluşturduğunuz kullanıcı akışının adını belirtin. Örneğin: `b2c_1_sign_in`, `b2c_1_sign_up`, `b2c_1_edit_profile`veya . |
| client_id | Evet | [Azure portalının](https://portal.azure.com/) uygulamanıza atadığı uygulama kimliği. |
| response_type | Evet | OpenID `id_token` Connect oturum açma için şunları içermelidir. Ayrıca yanıt türünü `token`de içerebilir. `token`Kullanırsanız, uygulamanız yetkili bitiş noktasına ikinci bir istekte bulunmadan hemen yetkili bitiş noktasından bir erişim belirteci alabilir.  `token` Yanıt türünü kullanırsanız, `scope` parametre nin belirteci için hangi kaynağı düzenleyecek kaynağı gösteren bir kapsam içermesi gerekir. |
| redirect_uri | Hayır | Kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabildiği uygulamanızın URI'yi yönlendirmesi. URL kodlanmış olması dışında, portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. |
| response_mode | Hayır | Ortaya çıkan belirteci uygulamanıza geri göndermek için kullanılacak yöntemi belirtir.  Örtük akışlar `fragment`için. |
| scope | Evet | Alan ayrılmış kapsamlistesi. Tek bir kapsam değeri, istenen izinlerin her ikisini de Azure AD'ye gösterir. Kapsam, `openid` kullanıcıda oturum açma ve kullanıcı hakkında kimlik belirteçleri şeklinde veri alma iznini gösterir. Kapsam `offline_access` web uygulamaları için isteğe bağlıdır. Uygulamanızın kaynaklara uzun süreli erişim için yeni bir belirteci gerektiğini belirtir. |
| durum | Hayır | Belirteç yanıtında da döndürülen isteğe dahil edilen bir değer. Kullanmak istediğiniz herhangi bir içerik dizisi olabilir. Genellikle, rasgele oluşturulan, benzersiz bir değer, çapraz site isteği sahtecilik saldırılarını önlemek için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri de uygulamada kodlamak için kullanılır. |
| Nonce | Evet | Talepte (uygulama tarafından oluşturulan) yer alan ve talep olarak ortaya çıkan kimlik belirtecine dahil edilen bir değer. Uygulama daha sonra belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Genellikle, değer isteğin kaynağını tanımlamak için kullanılabilecek randomize, benzersiz bir dizedir. |
| Istemi | Hayır | Gerekli kullanıcı etkileşimi türü. Şu anda, tek `login`geçerli değer . Bu parametre, kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar. Tek oturum açma etkili olmaz. |

Bu noktada, kullanıcıdan ilkenin iş akışını tamamlaması istenir. Kullanıcının kullanıcı adını ve parolasını girmesi, sosyal kimlikle oturum açması, dizine kaydolması veya başka sayıda adım atması gerekebilir. Kullanıcı eylemleri, kullanıcı akışının nasıl tanımlandığına bağlıdır.

Kullanıcı kullanıcı akışını tamamladıktan sonra, Azure AD uygulamanız için `redirect_uri`kullandığınız değerden bir yanıt döndürür. `response_mode` Parametrede belirtilen yöntemi kullanır. Yanıt, yürütülen kullanıcı akışından bağımsız olarak, her kullanıcı eylem senaryosu için tam olarak aynıdır.

### <a name="successful-response"></a>Başarılı yanıt
Okunabilirlik için `response_mode=fragment` satır `response_type=id_token+token` sonları ile aşağıdakileri kullanan ve görünen başarılı bir yanıt:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parametre | Açıklama |
| --------- | ----------- |
| access_token | Uygulamanın istediği erişim belirteci. |
| token_type | Belirteç türü değeri. Azure AD'nin desteklediği tek tür Taşıyıcı'dır. |
| expires_in | Erişim belirtecinin geçerli olduğu süre uzunluğu (saniye cinsinden). |
| scope | Belirteç için geçerli olan kapsamlar. Daha sonra kullanmak üzere belirteçleri önbelleğe almak için kapsamları da kullanabilirsiniz. |
| id_token | Uygulamanın istediği kimlik belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için kimlik belirtecinizi kullanabilirsiniz. Kimlik belirteçleri ve içerikleri hakkında daha fazla bilgi için [Azure AD B2C belirteç başvurusuna](tokens-overview.md)bakın. |
| durum | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |

### <a name="error-response"></a>Hata yanıtı
Hata yanıtları, uygulamanın bunları uygun şekilde işleyebilmeleri için uri yönlendirmesine de gönderilebilir:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametre | Açıklama |
| --------- | ----------- |
| error | Oluşan hata türlerini sınıflandırmak için kullanılan bir kod. |
| error_description | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| durum | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır.|

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama

Kimlik belirteci almak, kullanıcının kimliğini doğrulamak için yeterli değildir. Kimlik belirteci nin imzasını doğrulayın ve uygulamanızın gereksinimlerine göre belirteçteki talepleri doğrulayın. Azure AD B2C, belirteçleri imzalamak ve geçerli olduklarını doğrulamak için [JSON Web Belirteçleri (JWT'ler)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve ortak anahtar şifrelemesini kullanır.

Kullanmayı tercih ettiğiniz dile bağlı olarak, jwt'leri doğrulamak için birçok açık kaynak kitaplık kullanılabilir. Kendi doğrulama mantığınızı uygulamak yerine kullanılabilir açık kaynak kitaplıklarını keşfetmeyi düşünün. Bu kitaplıkları düzgün bir şekilde kullanmayı öğrenmenize yardımcı olmak için bu makaledeki bilgileri kullanabilirsiniz.

Azure AD B2C'nin OpenID Connect meta veri bitiş noktası vardır. Bir uygulama, çalışma zamanında Azure AD B2C hakkında bilgi almak için bitiş noktasını kullanabilir. Bu bilgiler uç noktaları, belirteç içeriğini ve belirteç imzalama anahtarlarını içerir. Azure AD B2C kiracınızdaki her kullanıcı akışı için bir JSON meta veri belgesi vardır. Örneğin, fabrikamb2c.onmicrosoft.com kiracıb2c_1_sign_in kullanıcı akışı için meta veri belgesi şu adreste bulunur:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/v2.0/.well-known/openid-configuration
```

Bu yapılandırma belgesinin özelliklerinden `jwks_uri`biri. Aynı kullanıcı akışının değeri:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/discovery/v2.0/keys
```

Kimlik belirteci imzalamak için hangi kullanıcı akışının kullanıldığını (ve meta verileri nereden getireceğiniz) belirlemek için iki seçeneğiniz var. İlk olarak, kullanıcı akışı adı `acr` talep `id_token`dahil edilir. Bir kimlik belirtecindeki talepleri nasıl ayrışdırırsa ayrıştınız hakkında bilgi için [Azure AD B2C belirteci başvurusuna](tokens-overview.md)bakın. Diğer seçeneğiniz, isteği yaptığınızda kullanıcı akışını `state` parametrenin değerinde kodlamaktır. Ardından, hangi `state` kullanıcı akışının kullanıldığını belirlemek için parametreyi çözün. Her iki yöntem de geçerlidir.

OpenID Connect meta veri bitiş noktasından meta veri belgesini aldıktan sonra, kimlik belirteci imzasını doğrulamak için RSA-256 ortak anahtarlarını (bu uç noktada bulunan) kullanabilirsiniz. Herhangi bir zamanda bu bitiş noktasında listelenen birden çok anahtar `kid`olabilir, her biri bir . Üstbilgi `id_token` de bir `kid` talep içerir. Bu anahtarlardan hangisinin kimlik belirteci imzalamak için kullanıldığını gösterir. [Belirteçleri doğrulama](tokens-overview.md)hakkında bilgi edinmek de dahil olmak üzere daha fazla bilgi için [Azure AD B2C belirteç başvurusuna](tokens-overview.md)bakın.
<!--TODO: Improve the information on this-->

Kimlik belirteci imzasını doğruladıktan sonra, çeşitli talepler doğrulama gerektirir. Örnek:

* Belirteç yeniden oynatma saldırılarını önlemek için `nonce` talebi doğrulayın. Değeri oturum açma isteğinde belirttiğiniz değer olmalıdır.
* Uygulamanız `aud` için kimlik belirteci verildiğinden emin olmak için talebi doğrulayın. Değeri uygulamanızın uygulama kimliği olmalıdır.
* Kimlik `iat` belirteci süresinin dolmadığından emin olmak için ve `exp` talepleri doğrulayın.

Gerçekleştirmeniz gereken birkaç doğrulama daha [OpenID Connect Core Spec'te](https://openid.net/specs/openid-connect-core-1_0.html)ayrıntılı olarak açıklanmıştır. Senaryonuza bağlı olarak ek talepleri doğrulamak da isteyebilirsiniz. Bazı yaygın doğrulamalar şunlardır:

* Kullanıcının veya kuruluşun uygulamaya kaydolmasını sağlamak.
* Kullanıcının uygun yetkilendirme ve ayrıcalıklara sahip olmasını sağlamak.
* Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak gibi belirli bir kimlik doğrulama gücünün oluşmasını sağlamak.

Kimlik belirtecindeki talepler hakkında daha fazla bilgi için [Azure AD B2C belirteci başvurusuna](tokens-overview.md)bakın.

Kimlik belirteci doğruladıktan sonra, kullanıcıyla bir oturum başlatabilirsiniz. Uygulamanızda, kullanıcı hakkında bilgi almak için kimlik belirtecindeki talepleri kullanın. Bu bilgiler görüntüleme, kayıt, yetkilendirme ve benzeri için kullanılabilir.

## <a name="get-access-tokens"></a>Erişim jetonları alın
Web uygulamalarınızın yapması gereken tek şey kullanıcı akışlarını yürütmekse, sonraki birkaç bölümü atlayabilirsiniz. Aşağıdaki bölümlerdeki bilgiler yalnızca bir web API'sine doğrulanmış aramalar yapması gereken ve Azure AD B2C tarafından korunan web uygulamaları için geçerlidir.

Kullanıcıyı tek sayfalı uygulamanızda oturum açtığınıza göre, Azure AD tarafından güvenli web API'lerini aramak için erişim belirteçleri alabilirsiniz. `token` Yanıt türünü kullanarak zaten bir belirteç almış olsanız bile, kullanıcıyı yeniden oturum açmaya yönlendirmeden ek kaynaklar için belirteçler elde etmek için bu yöntemi kullanabilirsiniz.

Tipik bir web uygulaması akışında, `/token` bitiş noktasına bir istekte bulunabilirsiniz. Ancak, bitiş noktası CORS isteklerini desteklemez, bu nedenle yenileme belirteci almak için AJAX aramaları yapmak bir seçenek değildir. Bunun yerine, diğer web API'leri için yeni belirteçler almak için gizli bir HTML iframe öğesinde örtük akışı kullanabilirsiniz. Aşağıda, okunabilirlik için satır sonları ile ilgili bir örnek verilmiştir:

```HTTP
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
```

| Parametre | Gerekli mi? | Açıklama |
| --- | --- | --- |
|{kiracı}| Gerekli | Azure AD B2C kiracınızın adı|
{ilke}| Gerekli| Çalıştırılacak kullanıcı akışı. Azure AD B2C kiracınızda oluşturduğunuz kullanıcı akışının adını belirtin. Örneğin: `b2c_1_sign_in`, `b2c_1_sign_up`, `b2c_1_edit_profile`veya . |
| client_id |Gerekli |[Azure portalında](https://portal.azure.com)uygulamanız için atanan uygulama kimliği. |
| response_type |Gerekli |OpenID `id_token` Connect oturum açma için şunları içermelidir.  Ayrıca yanıt türünü `token`de içerebilir. Burada kullanırsanız, `token` uygulamanız yetkili bitiş noktasına ikinci bir istekte bulunmadan hemen yetkili bitiş noktasından bir erişim belirteci alabilir. `token` Yanıt türünü kullanırsanız, `scope` parametre nin belirteci için hangi kaynağı düzenleyecek kaynağı gösteren bir kapsam içermesi gerekir. |
| redirect_uri |Önerilen |Kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabildiği uygulamanızın URI'yi yönlendirmesi. URL kodlanmış olması dışında, portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. |
| scope |Gerekli |Alan ayrılmış kapsamlistesi.  Belirteçleri almak için, amaçlanan kaynak için gereksinim duyduğunuz tüm kapsamları ekleyin. |
| response_mode |Önerilen |Ortaya çıkan belirteci uygulamanıza geri göndermek için kullanılan yöntemi belirtir. Örtük akış `fragment`için, kullanın. Diğer iki mod belirtilebilir `query` `form_post`ve , ancak örtük akış çalışmıyor. |
| durum |Önerilen |Belirteç yanıtında döndürülen isteğe dahil edilen bir değer.  Kullanmak istediğiniz herhangi bir içerik dizisi olabilir.  Genellikle, rasgele oluşturulan, benzersiz bir değer, çapraz site isteği sahtecilik saldırılarını önlemek için kullanılır.  Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için de kullanılır. Örneğin, kullanıcının üzerinde olduğu sayfa veya görünüm. |
| Nonce |Gerekli |Talepte yer alan ve uygulama tarafından oluşturulan ve talep olarak ortaya çıkan kimlik belirtecine dahil edilen bir değer.  Uygulama daha sonra belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Genellikle, değer isteğin kaynağını tanımlayan rasgele, benzersiz bir dizedir. |
| Istemi |Gerekli |Belirteçleri gizli bir iframe'de `prompt=none` yenilemek ve almak için, iframe'in oturum açma sayfasında takılıp kalmadığından ve hemen geri döndüğünden emin olmak için kullanın. |
| login_hint |Gerekli |Gizli bir iframe'de belirteçleri yenilemek ve almak için, kullanıcının belirli bir anda sahip olabileceği birden çok oturumu ayırt etmek için bu ipucuna kullanıcı adını ekleyin. `preferred_username` Talebi kullanarak daha önceki bir oturum açmadan kullanıcı adını `profile` ayıklayabilirsiniz `preferred_username` (talebi almak için kapsam gereklidir). |
| domain_hint |Gerekli |`consumers` veya `organizations` olabilir.  Gizli bir iframe'de belirteçleri yenilemek ve almak için istekteki `domain_hint` değeri ekleyin.  `tid` Hangi değerin kullanılacağını belirlemek için talebi daha önceki bir oturum `profile` açmanın kimlik belirtecinden ayıklayın `tid` (talebi almak için kapsam gereklidir). `tid` Talep değeri ise, `9188040d-6c67-4c5b-b112-36a304b66dad` `domain_hint=consumers`kullanın.  Aksi takdirde, kullanın. `domain_hint=organizations` |

Parametreyi `prompt=none` ayarlayarak, bu istek hemen başarılı olur veya başarısız olur ve uygulamanıza geri döner.  `response_mode` Parametrede belirtilen yöntem kullanılarak belirtilen yeniden yönlendirme URI'de uygulamanıza başarılı bir yanıt gönderilir.

### <a name="successful-response"></a>Başarılı yanıt
Kullanarak `response_mode=fragment` başarılı bir yanıt şu örneğe benzer:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parametre | Açıklama |
| --- | --- |
| access_token |Uygulamanın istediği belirteç. |
| token_type |Belirteç türü her zaman Bearer olacaktır. |
| durum |İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |
| expires_in |Erişim belirteci nin ne kadar süre geçerli olduğu (saniye cinsinden). |
| scope |Erişim belirteci için geçerli olan kapsamlar. |

### <a name="error-response"></a>Hata yanıtı
Hata yanıtları, uygulamanın bunları uygun şekilde işleyebilmeleri için uri yönlendirmesine de gönderilebilir.  Için `prompt=none`, beklenen bir hata şu örnek gibi görünüyor:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametre | Açıklama |
| --- | --- |
| error |Oluşan hata türlerini sınıflandırmak için kullanılabilecek bir hata kodu dizesi. Hataları tepkilemek için dizeyi de kullanabilirsiniz. |
| error_description |Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Bu hatayı iframe isteğinde alırsanız, yeni bir belirteç almak için kullanıcının etkileşimli olarak yeniden oturum açması gerekir.

## <a name="refresh-tokens"></a>Belirteçleri yenileyin
Kimlik belirteçleri ve erişim belirteçleri kısa bir süre sonra sona erer. Uygulamanız bu belirteçleri düzenli olarak yenilemek için hazır olmalıdır.  Her iki belirteç türünü yenilemek için, Azure AD adımlarını denetlemek `prompt=none` için parametreyi kullanarak daha önceki bir örnekte kullandığımız gizli iframe isteğini gerçekleştirin.  Yeni `id_token` bir değer almak için, `response_type=id_token` `scope=openid`bir `nonce` parametre ve kullandığınızdan emin olun.

## <a name="send-a-sign-out-request"></a>Oturum açma isteği gönderme
Kullanıcıyı uygulamadan çıkarmak istediğinizde, oturum u imzalamak için kullanıcıyı Azure AD'ye yönlendirin. Kullanıcıyı yeniden yönlendirmezseniz, Azure AD ile geçerli bir oturum açma oturumları olduğundan, kimlik bilgilerini tekrar girmeden uygulamanız için yeniden kimlik doğrulaması yapabilir.

Kullanıcıyı, `end_session_endpoint` [Kimliği Doğrula belirtecinde](#validate-the-id-token)açıklanan aynı OpenID Connect meta veri belgesinde listelenene yönlendirebilirsiniz. Örnek:

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/logout?post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametre | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| {kiracı} | Evet | Azure AD B2C kiracınızın adı |
| {ilke} | Evet | Kullanıcıyı uygulamanızdan çıkarmak için kullanmak istediğiniz kullanıcı akışı. |
| post_logout_redirect_uri | Hayır | Başarılı bir şekilde oturum dışı attıktan sonra kullanıcının yönlendirilmesi gereken URL. Azure AD B2C, kullanıcıya genel bir ileti gösterir. |
| durum | Hayır | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve `state` yanıttaki değerlerin aynı olduğunu doğrulamalıdır. |


> [!NOTE]
> Kullanıcıyı Azure AD `end_session_endpoint` B2C ile kullanıcının tek oturum açma durumundan bazılarını temize çıkarmak. Ancak, kullanıcının sosyal kimlik sağlayıcı oturumunun dışına imza vermez. Kullanıcı sonraki oturum açma sırasında aynı kimlik sağlayıcısını seçerse, kullanıcı kimlik bilgilerini girmeden yeniden kimlik doğrulaması yapılır. Bir kullanıcı Azure AD B2C uygulamanızdan çıkış yapmak istiyorsa, bu, örneğin Facebook hesabından tamamen oturum unuzu imzalamak istediği anlamına gelmez. Ancak, yerel hesaplar için kullanıcının oturumu düzgün bir şekilde sonlanacaktır.
>

## <a name="next-steps"></a>Sonraki adımlar

### <a name="code-sample-azure-ad-b2c-with-microsoft-authentication-library-for-javascript"></a>Kod örneği: JavaScript için Microsoft Kimlik Doğrulama Kitaplığı ile Azure AD B2C

Azure AD B2C (GitHub) [için msal.js ile oluşturulmuş tek sayfalık uygulama][github-msal-js-example]

GitHub'daki bu örnek, [msal.js][github-msal-js] ile oluşturulmuş ve pop-up tarzı kimlik doğrulaması kullanarak oluşturulmuş basit bir web uygulamasında Azure AD B2C'ye başlamanıza yardımcı olmayı amaçlamaktadır.

<!-- Links - EXTERNAL -->
[github-msal-js-example]: https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp
[github-msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
