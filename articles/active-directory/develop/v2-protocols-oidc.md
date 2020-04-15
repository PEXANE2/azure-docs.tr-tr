---
title: OpenID Connect protokolü - Microsoft kimlik platformu | Azure
description: OpenID Connect kimlik doğrulama protokolünün Microsoft kimlik platformu uygulamasını kullanarak web uygulamaları oluşturun.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: d83e336c73d9288b97a0564472caa497ab64b4b1
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309235"
---
# <a name="microsoft-identity-platform-and-openid-connect-protocol"></a>Microsoft kimlik platformu ve OpenID Connect protokolü

OpenID Connect, OAuth 2.0'da bir kullanıcıyı bir web uygulamasında güvenli bir şekilde oturum açmak için kullanabileceğiniz bir kimlik doğrulama protokolüdür. Microsoft kimlik platformu bitiş noktasının OpenID Connect uygulamasını kullandığınızda, web tabanlı uygulamalarınıza oturum açma ve API erişimi ekleyebilirsiniz. Bu makalede, dilden bağımsız olarak nasıl yapılacağını gösterir ve Microsoft açık kaynak kitaplıklarını kullanmadan HTTP iletilerinin nasıl gönderilip alınılacağını açıklar.

> [!NOTE]
> Microsoft kimlik platformu bitiş noktası, tüm Azure Etkin Dizin (Azure AD) senaryolarını ve özelliklerini desteklemez. Microsoft kimlik platformu bitiş noktasını kullanıp kullanmadığınızı belirlemek için [Microsoft kimlik platformu sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

[OpenID Connect,](https://openid.net/specs/openid-connect-core-1_0.html) OAuth kullanarak tek oturum açma yapabilmeniz için OAuth 2.0 *yetkilendirme* protokolünü *kimlik doğrulama* protokolü olarak kullanmak üzere genişletir. OpenID Connect, istemcinin kullanıcının kimliğini doğrulamasına olanak tanıyan bir güvenlik belirteci olan *kimlik belirteci*kavramını sunar. Kimlik belirteci, kullanıcı hakkında temel profil bilgilerini de alır. OpenID Connect OAuth 2.0'ı genişletdiği için, uygulamalar bir yetkilendirme sunucusu tarafından güvenli kaynaklara erişmek için kullanılabilen *erişim belirteçlerini*güvenli bir şekilde [edinebilir.](active-directory-v2-protocols.md#the-basics) Microsoft kimlik platformu bitiş noktası, Azure AD'ye kayıtlı üçüncü taraf uygulamaların Web API'leri gibi güvenli kaynaklar için erişim belirteçleri yayınlamasına da olanak tanır. Erişim belirteçleri vermek için bir uygulamanın nasıl ayarlanılabilenhakkında daha fazla bilgi için, [microsoft kimlik platformu bitiş noktasına bir uygulamayı nasıl kaydedin.](quickstart-register-app.md) Sunucuda barındırılan ve tarayıcı üzerinden erişilen bir [web uygulaması](v2-app-types.md#web-apps) oluşturuyorsanız OpenID Connect'i kullanmanızı öneririz.

## <a name="protocol-diagram-sign-in"></a>Protokol diyagramı: Oturum açma

En temel oturum açma akışı, sonraki diyagramda gösterilen adımlara sahiptir. Her adım bu makalede ayrıntılı olarak açıklanmıştır.

![OpenID Connect protokolü: Oturum açma](./media/v2-protocols-oidc/convergence-scenarios-webapp.svg)

## <a name="fetch-the-openid-connect-metadata-document"></a>OpenID Connect meta veri belgesini getir

OpenID Connect, bir uygulamanın oturum açması için gereken bilgilerin çoğunu içeren bir meta veri belgesini açıklar. Bu, kullanılacak URL'ler ve hizmetin genel imzalama anahtarlarının konumu gibi bilgileri içerir. Microsoft kimlik platformu bitiş noktası için bu, kullanmanız gereken OpenID Connect meta veri belgesidir:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```
> [!TIP]
> Deneyin! Kiracı [https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) yapılandırmasını `common` görmek için tıklatın.

Dört `{tenant}` değerden birini alabilir:

| Değer | Açıklama |
| --- | --- |
| `common` |Azure AD'den hem kişisel Microsoft hesabı hem de iş veya okul hesabı olan kullanıcılar uygulamada oturum açabilir. |
| `organizations` |Uygulamada yalnızca Azure AD'den iş veya okul hesabı olan kullanıcılar oturum açabilir. |
| `consumers` |Uygulamada yalnızca kişisel Microsoft hesabı olan kullanıcılar oturum açabilir. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` veya `contoso.onmicrosoft.com` | Yalnızca belirli bir Azure AD kiracısından kullanıcılar (ister bir iş veya okul hesabı olan dizinde üye olsunlar veya kişisel bir Microsoft hesabı olan dizinde misafir olsunlar) uygulamada oturum açabilir. Azure AD kiracısının arkadaş çaları veya kiracının GUID tanımlayıcısı kullanılabilir. Ayrıca, `9188040d-6c67-4c5b-b112-36a304b66dad` `consumers` kiracı yerine, tüketici kiracı kullanabilirsiniz.  |

Meta veriler basit bir JavaScript Nesne Gösterimi (JSON) belgesidir. Bir örnek için aşağıdaki snippet bakın. Parçacık içeriği [OpenID Connect belirtiminde](https://openid.net/specs/openid-connect-discovery-1_0.html#rfc.section.4.2)tam olarak açıklanmıştır.

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/{tenant}\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/{tenant}\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/{tenant}\/discovery\/v2.0\/keys",

  ...

}
```

[Uygulamanızın talep eşleme](active-directory-claims-mapping.md) özelliğini kullanmanın sonucu olarak özel imzalama anahtarları `appid` varsa, uygulamanızın imzalama anahtarı bilgilerini `jwks_uri` işaretlemek için uygulama kimliğini içeren bir sorgu parametresi eklemeniz gerekir. Örneğin: `https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration?appid=6731de76-14a6-49ae-97bc-6eba6914391e` bir `jwks_uri` `https://login.microsoftonline.com/{tenant}/discovery/v2.0/keys?appid=6731de76-14a6-49ae-97bc-6eba6914391e`içerir.

Genellikle, bu meta veri belgesini bir OpenID Connect kitaplığını veya SDK'yı yapılandırmak için kullanırsınız; kitaplık, meta verileri işini yapmak için kullanır. Ancak, önceden oluşturulmuş bir OpenID Connect kitaplığı kullanmıyorsanız, Microsoft kimlik platformu bitiş noktasını kullanarak bir web uygulamasında oturum açmak için bu makalenin geri kalanındaki adımları izleyebilirsiniz.

## <a name="send-the-sign-in-request"></a>Oturum açma isteğini gönderme

Web uygulamanızın kullanıcının kimliğini niçin doğrulaması gerekiyorsa, `/authorize` kullanıcıyı bitiş noktasına yönlendirebilir. Bu istek, bu önemli ayrımlarla [OAuth 2.0 yetki kodu akışının](v2-oauth2-auth-code-flow.md)ilk ayağına benzer:

* `scope` İstek, `openid` kapsamı parametreye dahil etmelidir.
* `response_type` Parametre. `id_token`
* İstek parametreiçermelidir. `nonce`

> [!IMPORTANT]
> /authorization bitiş noktasından bir kimlik belirtecinin başarılı bir şekilde talep edilebilmesi için, [kayıt portalındaki](https://portal.azure.com) uygulama kaydının `oauth2AllowIdTokenImplicitFlow` Kimlik Doğrulama sekmesinde etkin leştirilmiş id_tokens dolaylı hibesine sahip olması gerekir [(uygulama bildiriminde](reference-app-manifest.md) bayrağı `true`ayarlar). Etkinleştirilmezse, bir `unsupported_response` hata döndürülür: "Bu istemci için 'response_type' giriş parametresi için sağlanan değere izin verilmez. Beklenen değer 'kod'"

Örneğin:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Bu isteği yürütmek için aşağıdaki bağlantıyı tıklatın. Oturum açmanızdan sonra tarayıcınız adres çubuğunda bir kimlik belirteciyle yeniden yönlendirilir. `https://localhost/myapp/` Bu isteğin `response_mode=fragment` kullandığını unutmayın (yalnızca gösteri amaçlı). Kullanmanızı `response_mode=form_post`öneririz.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Uygulamada kimlerin `{tenant}` oturum açabileceğini denetlemek için istek yolundaki değeri kullanabilirsiniz. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla bilgi için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın. |
| `client_id` | Gerekli | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `response_type` | Gerekli | OpenID `id_token` Connect oturum açma için şunları içermelidir. Ayrıca, diğer `response_type` değerleri de `code`içerebilir, örneğin. |
| `redirect_uri` | Önerilen | Kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabildiği uygulamanızın URI'yi yönlendirmesi. URL'nin kodlanmış olması dışında, portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmelidir. Yoksa, bitiş noktası kullanıcıyı geri göndermek için rasgele bir kayıtlı redirect_uri seçer. |
| `scope` | Gerekli | Alan ayrılmış kapsamlistesi. OpenID Connect için, onay `openid`UI'deki "Oturum aç" iznine çevrilen kapsamı içermelidir. Bu istekte onay istemek için başka kapsamlar da ekleyebilirsiniz. |
| `nonce` | Gerekli | Uygulama tarafından oluşturulan isteğe dahil edilen ve elde edilen id_token değere talep olarak dahil edilecek bir değer. Uygulama, belirteç yeniden oynatma saldırılarını azaltmak için bu değeri doğrulayabilir. Değer genellikle isteğin kaynağını tanımlamak için kullanılabilecek rasgele, benzersiz bir dizedir. |
| `response_mode` | Önerilen | Elde edilen yetkilendirme kodunu uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. `form_post` veya `fragment` olabilir. Web uygulamaları için, jetonların en güvenli şekilde uygulamanıza aktarılmasını sağlamak için kullanmanızı `response_mode=form_post`öneririz. |
| `state` | Önerilen | İstekte yer alan ve belirteç yanıtında da döndürülecek bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Rasgele oluşturulan benzersiz değer genellikle [siteleri arası istek sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için de kullanılır(örneğin, sayfa veya kullanıcının üzerinde olduğu görünüm). |
| `prompt` | İsteğe bağlı | Gerekli kullanıcı etkileşimi türünü gösterir. Şu anda tek geçerli `login` `none`değerler `consent`, , ve . Talep, `prompt=login` kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve bu da tek bir oturum açmayı geçersiz kLar. İddia `prompt=none` tam tersi. Bu talep, kullanıcıya herhangi bir etkileşimli istem sunulmamasını sağlar. İstek tek oturum açma yoluyla sessizce tamamlanamazsa, Microsoft kimlik platformu bitiş noktası bir hata döndürür. Talep, `prompt=consent` kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler. İletişim kutusu, kullanıcıdan uygulamaya izin vermesini ister. |
| `login_hint` | İsteğe bağlı | Kullanıcı adını önceden biliyorsanız, kullanıcı için oturum açma sayfasının kullanıcı adı ve e-posta adresi alanını önceden doldurmak için bu parametreyi kullanabilirsiniz. Genellikle, uygulamalar `preferred_username` bu parametreyi yeniden kimlik doğrulama sırasında, talebi kullanarak daha önceki bir oturum açma dan kullanıcı adını ayıkladıktan sonra kullanır. |
| `domain_hint` | İsteğe bağlı | Federe bir dizinde kullanıcının alemi.  Bu, biraz daha kolaylaştırılmış bir kullanıcı deneyimi için kullanıcının oturum açma sayfasında geçtiği e-posta tabanlı bulma işlemini atlar. AD FS gibi şirket içi bir dizini aracılığıyla federe olan kiracılar için, bu genellikle varolan oturum nedeniyle sorunsuz bir oturum açma ile sonuçlanır. |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft kimlik platformu bitiş noktası, kullanıcının `scope` sorgu parametresinde belirtilen izinleri onayladığını doğrular. Kullanıcı bu izinlerden herhangi birini kabul etmediyse, Microsoft kimlik platformu bitiş noktası kullanıcıdan gerekli izinleri onaylamasını ister. [İzinler, onay ve çok kiracılı uygulamalar](v2-permissions-and-consent.md)hakkında daha fazla bilgi edinebilirsiniz.

Kullanıcı kimlik doğrulaması yaptıktan ve onay verdikten sonra, Microsoft kimlik platformu bitiş `response_mode` noktası, parametrede belirtilen yöntemi kullanarak belirtilen yeniden yönlendirme URI'de uygulamanıza bir yanıt verir.

### <a name="successful-response"></a>Başarılı yanıt

Kullandığınızda `response_mode=form_post` başarılı bir yanıt şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `id_token` | Uygulamanın istediği kimlik belirteci. Kullanıcının `id_token` kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için parametreyi kullanabilirsiniz. Kimlik belirteçleri ve içerikleri hakkında daha fazla bilgi için [ `id_tokens` başvuruya](id-tokens.md)bakın. |
| `state` | İsteğe `state` bir parametre dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın bunları işleyebilmeleri için uri yönlendirmesine de gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hata türlerini sınıflandırmak ve hatalara tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| `error_description` | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme bitiş noktası hataları için hata kodları

Aşağıdaki tabloda hata yanıtı `error` parametresi döndürülebilir hata kodları açıklanır:

| Hata kodu | Açıklama | İstemci eylemi |
| --- | --- | --- |
| `invalid_request` | Eksik, gerekli parametre gibi protokol hatası. |İsteği düzeltin ve yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır. |
| `unauthorized_client` | İstemci uygulaması yetkilendirme kodu isteyemez. |Bu genellikle istemci uygulaması Azure AD'de kayıtlı değilse veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| `access_denied` | Kaynak sahibi onay ını reddetti. |İstemci uygulaması, kullanıcı izin vermedikçe devam edilemeyecek kullanıcıya bildirebilir. |
| `unsupported_response_type` |Yetkilendirme sunucusu istekteki yanıt türünü desteklemez. |İsteği düzeltin ve yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır. |
| `server_error` | Sunucu beklenmeyen bir hatayla karşılaştı. |İsteği yeniden deneyin. Bu hatalar geçici koşullardan kaynaklanabilir. İstemci uygulaması kullanıcıya yanıtının geçici bir hata nedeniyle geciktiğini açıklayabilir. |
| `temporarily_unavailable` | Sunucu geçici olarak isteği işlemek için çok meşgul. |İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtının geciktiğini kullanıcıya açıklayabilir. |
| `invalid_resource` | Hedef kaynak geçersizdir, çünkü bu kaynak yok, Azure AD bulamıyor veya doğru şekilde yapılandırılmıyor. |Bu, kaynağın varsa kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yükleme ve Azure AD'ye ekleme yönergelerini isteyebilir. |

## <a name="validate-the-id-token"></a>Kimlik belirteci doğrulama

Yalnızca bir id_token almak kullanıcının kimliğini doğrulamak için yeterli değildir; id_token imzasını doğrulamanız ve uygulamanızın gereksinimlerine göre belirteçteki talepleri doğrulamanız gerekir. Microsoft kimlik platformu bitiş noktası, belirteçleri imzalamak ve geçerli olduklarını doğrulamak için [JSON Web Belirteçleri (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ve ortak anahtar şifrelemesini kullanır.

İstemci `id_token` kodunu doğrulamayı seçebilirsiniz, ancak yaygın bir `id_token` uygulama arka uç sunucusuna göndermek ve doğrulamayı orada yapmaktır. id_token imzasını doğruladıktan sonra, doğrulamanız gereken birkaç iddia vardır. [Belirteçleri doğrulama](id-tokens.md#validating-an-id_token) ve [Anahtar Devrini İmzalama Hakkında Önemli Bilgiler](active-directory-signing-key-rollover.md)dahil olmak üzere daha fazla bilgi için [ `id_token` başvuruya](id-tokens.md) bakın. Belirteçleri ayrıştırmak ve doğrulamak için bir kütüphanekullanmanızı öneririz - çoğu dil ve platform için en az bir tane mevcuttur.

Ayrıca, senaryonuza bağlı olarak ek talepleri doğrulamak isteyebilirsiniz. Bazı yaygın doğrulamalar şunlardır:

* Kullanıcının/kuruluşun uygulamaya kaydolmasını sağlamak.
* Kullanıcının uygun yetkilendirmeye/ayrıcalıklara sahip olmasını sağlamak
* Çok faktörlü kimlik doğrulama gibi belirli bir kimlik doğrulama gücünün oluşmasını sağlamak.

id_token doğruladıktan sonra, kullanıcıyla bir oturum başlatabilir ve uygulamanızdaki kullanıcı hakkında bilgi almak için id_token'daki talepleri kullanabilirsiniz. Bu bilgiler görüntü, kayıt, kişiselleştirme vb. için kullanılabilir.

## <a name="send-a-sign-out-request"></a>Oturum açma isteği gönderme

Uygulamanızdan kullanıcıyı oturumunuzu imzalamak istediğinizde, uygulamanızın çerezlerini temizlemek veya kullanıcının oturumunu başka bir şekilde sonlamak yeterli değildir. Ayrıca, oturum sonlarını imzalamak için kullanıcıyı Microsoft kimlik platformu bitiş noktasına yönlendirmeniz gerekir. Bunu yapmazsanız, kullanıcı microsoft kimlik platformu bitiş noktası ile geçerli bir tek oturum oturumu olacak, çünkü yeniden kimlik bilgilerini girmeden uygulamanıza yeniden authenticates.

Kullanıcıyı OpenID Connect `end_session_endpoint` meta veri belgesinde listelenenlere yönlendirebilirsiniz:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parametre | Koşul | Açıklama |
| ----------------------- | ------------------------------- | ------------ |
| `post_logout_redirect_uri` | Önerilen | Başarılı bir şekilde oturum dışı ayrıldıktan sonra kullanıcının yönlendirilmeye yönlendirilmeye yönelik URL'si. Parametre dahil değilse, kullanıcıya Microsoft kimlik platformu bitiş noktası tarafından oluşturulan genel bir ileti gösterilir. Bu URL, uygulama kayıt portalında uygulamanız için kayıtlı yönlendirme URL'lerinden biriyle eşleşmelidir. |

## <a name="single-sign-out"></a>Çoklu oturum kapatma

Kullanıcıyı `end_session_endpoint`, Microsoft kimlik platformu bitiş noktasına yönlendirdiğinizde, kullanıcının oturumunu tarayıcıdan temizler. Ancak, kullanıcı kimlik doğrulaması için Microsoft hesaplarını kullanan diğer uygulamalarda oturum açmış olabilir. Bu uygulamaların kullanıcıyı aynı anda oturum alabilmesini sağlamak için, Microsoft kimlik `LogoutUrl` platformu bitiş noktası, kullanıcının şu anda oturum açmış olduğu tüm uygulamaların kayıtlısına bir HTTP GET isteği gönderir. Uygulamalar, kullanıcıyı tanımlayan herhangi bir oturumu temizleyerek ve `200` yanıt döndürerek bu isteğe yanıt vermelidir. Uygulamanızda tek oturum açma yı desteklemek istiyorsanız, uygulamanızın `LogoutUrl` kodunda böyle bir uygulama uygulamanız gerekir. Uygulamayı kayıt `LogoutUrl` portalından ayarlayabilirsiniz.

## <a name="protocol-diagram-access-token-acquisition"></a>Protokol diyagramı: Erişim belirteç edinimi

Birçok web uygulaması nın yalnızca kullanıcıyı oturum alabilmekle birlikte, OAuth'u kullanarak kullanıcı adına bir web hizmetine erişmesi de gerekir. Bu senaryo, kullanıcı kimlik doğrulaması için OpenID Connect'i birleştirirken, Aynı anda OAuth yetkilendirme kodu akışını kullanıyorsanız erişim belirteçlerini almak için kullanabileceğiniz bir yetkilendirme kodu alır.

Tam OpenID Connect oturum açma ve belirteç edinme akışı bir sonraki diyagrama benzer görünür. Her adımı makalenin sonraki bölümlerinde ayrıntılı olarak açıklıyoruz.

![OpenID Connect protokolü: Belirteç edinimi](./media/v2-protocols-oidc/convergence-scenarios-webapp-webapi.svg)

## <a name="get-access-tokens"></a>Erişim jetonları alın
Erişim belirteçleri elde etmek için oturum açma isteğini değiştirin:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'form_post' or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Bu isteği yürütmek için aşağıdaki bağlantıyı tıklatın. Oturum açmadan sonra tarayıcınız, adres `https://localhost/myapp/`çubuğunda bir kimlik belirteci ve bir kodla yeniden yönlendirilir. Bu isteğin `response_mode=fragment` yalnızca gösteri amaçları için kullandığını unutmayın. Kullanmanızı `response_mode=form_post`öneririz.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=fragment&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

İsteke izin kapsamları ekleyerek ve `response_type=id_token code`kullanarak, Microsoft kimlik platformu bitiş noktası, kullanıcının `scope` sorgu parametresinde belirtilen izinleri kabul etmesini sağlar. Bir erişim jetonu yla değiş tokuş etmek için uygulamanız için bir yetkilendirme kodu döndürür.

### <a name="successful-response"></a>Başarılı yanıt

Kullanarak `response_mode=form_post` başarılı bir yanıt şu gibi görünür:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametre | Açıklama |
| --- | --- |
| `id_token` | Uygulamanın istediği kimlik belirteci. Kullanıcının kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için kimlik belirtecinizi kullanabilirsiniz. Referansta kimlik belirteçleri ve içerikleri hakkında daha fazla ayrıntı bulabilirsiniz. [ `id_tokens` ](id-tokens.md) |
| `code` | Uygulamanın istediği yetkilendirme kodu. Uygulama, hedef kaynak için bir erişim jetonu istemek için yetkilendirme kodunu kullanabilir. Yetkilendirme kodu kısa ömürlüdür. Genellikle, yetkilendirme kodunun süresi yaklaşık 10 dakika içinde sona erer. |
| `state` | İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın bunları uygun şekilde işleyebilmeleri için uri yönlendirmesine de gönderilebilir. Bir hata yanıtı şuna benzer:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hata türlerini sınıflandırmak ve hatalara tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| `error_description` | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Olası hata kodlarının ve önerilen istemci yanıtlarının açıklaması [için, yetkilendirme bitiş noktası hataları için hata kodlarına](#error-codes-for-authorization-endpoint-errors)bakın.

Bir yetkilendirme kodunuz ve kimlik belirteciniz olduğunda, kullanıcıyı oturum açabilir ve onlar adına erişim belirteçleri alabilirsiniz. Kullanıcıyı oturum alabilmek için kimlik belirtecisini [tam olarak açıklandığı şekilde](id-tokens.md#validating-an-id_token)doğrulamanız gerekir. Erişim belirteçleri almak [için, OAuth kod akışı belgelerinde](v2-oauth2-auth-code-flow.md#request-an-access-token)açıklanan adımları izleyin.
