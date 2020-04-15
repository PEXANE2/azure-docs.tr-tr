---
title: OAuth yetkilendirme kodu akışı - Microsoft kimlik platformu | Azure
description: OAuth 2.0 kimlik doğrulama protokolünün Microsoft kimlik platformu uygulamasını kullanarak web uygulamaları oluşturun.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 5241089ff3cc7826216fcadd6fd94116ee4a2c89
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81309449"
---
# <a name="microsoft-identity-platform-and-oauth-20-authorization-code-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 yetkilendirme kodu akışı

OAuth 2.0 yetkilendirme kodu hibesi, web API'leri gibi korunan kaynaklara erişmek için aygıta yüklenen uygulamalarda kullanılabilir. OAuth 2.0'ın Microsoft kimlik platformu uygulamasını kullanarak mobil ve masaüstü uygulamalarınıza oturum açma ve API erişimi ekleyebilirsiniz. Bu kılavuz dilden bağımsızdır ve Azure açık kaynak kimlik doğrulama [kitaplıklarından](reference-v2-libraries.md)herhangi birini kullanmadan HTTP iletilerinin nasıl gönderilip alınılacağını açıklar.

Bu makalede, uygulamanızdaki protokole karşı doğrudan programlama nın nasıl yapılacağını açıklanmaktadır.  Mümkün olduğunda, [belirteçleri elde etmek ve güvenli web API'lerini aramak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)yerine desteklenen Microsoft Kimlik Doğrulama Kitaplıklarını (MSAL) kullanmanızı öneririz.  Ayrıca [MSAL kullanan örnek uygulamalara](sample-v2-code.md)da göz atın.

> [!NOTE]
> Tüm Azure Active Directory senaryoları & özellikleri Microsoft kimlik platformu bitiş noktası tarafından desteklenmez. Microsoft kimlik platformu bitiş noktasını kullanmanız gerekip gerekip gerekmeden karar vermek için [Microsoft kimlik platformu sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

OAuth 2.0 yetkilendirme kodu akışı, [OAuth 2.0 belirtiminin 4.1 bölümünde](https://tools.ietf.org/html/rfc6749)açıklanmıştır. [Web uygulamaları](v2-app-types.md#web-apps) ve [yerel olarak yüklenmiş uygulamalar](v2-app-types.md#mobile-and-native-apps)da dahil olmak üzere uygulama türlerinin çoğunda kimlik doğrulama ve yetkilendirme gerçekleştirmek için kullanılır. Akış, uygulamaların Microsoft kimlik platformu bitiş noktası tarafından güvence altına alınarak güvenilen kaynaklara erişmek için kullanılabilecek access_tokens güvenli bir şekilde edinmesini sağlar.

## <a name="protocol-diagram"></a>Protokol diyagramı

Yüksek düzeyde, bir yerel/mobil uygulama için tüm kimlik doğrulama akışı biraz şuna benzer:

![OAuth Auth Kod Akışı](./media/v2-oauth2-auth-code-flow/convergence-scenarios-native.svg)

## <a name="request-an-authorization-code"></a>Yetkilendirme kodu isteme

Yetkilendirme kodu akışı, istemcinin kullanıcıyı `/authorize` bitiş noktasına yönlendirmesiyle başlar. Bu istekte, istemci `openid`kullanıcıdan `offline_access` `https://graph.microsoft.com/mail.read `izin ve izin ister.  Bazı izinler, örneğin bir kuruluşun dizinine kullanarak veri yazmak `Directory.ReadWrite.All`gibi yönetici tarafından kısıtlanır. Uygulamanız bu izinlerden birine kuruluş kullanıcısından erişmesini isterse, kullanıcı uygulamanızın izinlerini onaylama yetkisine sahip olmadığını belirten bir hata iletisi alır. Yönetici tarafından kısıtlanmış kapsamlara erişim isteğinde bulunmak için bunları doğrudan bir şirket yöneticisinden istemeniz gerekir.  Daha fazla bilgi için [Yönetici tarafından kısıtlanmış izinleri](v2-permissions-and-consent.md#admin-restricted-permissions)okuyun.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Bu isteği yürütmek için aşağıdaki bağlantıyı tıklayın! Oturum açmadan sonra tarayıcınız adres `https://localhost/myapp/` çubuğundaki `code` bir çubukla yeniden yönlendirilmelidir.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametre    | Gerekli/isteğe bağlı | Açıklama |
|--------------|-------------|--------------|
| `tenant`    | gerekli    | İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın.  |
| `client_id`   | gerekli    | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.**  |
| `response_type` | gerekli    | Yetkilendirme `code` kodu akışı için içermelidir.       |
| `redirect_uri`  | gerekli | Uygulamanızın, kimlik doğrulama yanıtlarının uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portala kaydettiğiniz redirect_uris tam olarak biriyle eşleşmesi gerekir, ancak url'nin kodlanmış olması gerekir. Yerel & mobil uygulamalar için varsayılan değeri `https://login.microsoftonline.com/common/oauth2/nativeclient`kullanmalısınız.   |
| `scope`  | gerekli    | Kullanıcının onay etmesini istediğiniz [kapsamların](v2-permissions-and-consent.md) alanayrılmış listesi.  İsteğin `/authorize` bacağı için bu, uygulamanızın aramak istediğiniz birden çok web API'sı için onay almasına olanak tanıyan birden çok kaynağı kapsayabilir. |
| `response_mode`   | Önerilen | Ortaya çıkan belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Aşağıdakilerden biri olabilir:<br/><br/>- `query`<br/>- `fragment`<br/>- `form_post`<br/><br/>`query`kodu, yeniden yönlendirme URI'nizde sorgu dize parametresi olarak sağlar. Örtük akışı kullanarak bir kimlik belirteci talep ediyorsanız, `query` [OpenID spec'te](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations)belirtildiği gibi kullanamazsınız. Sadece kodu talep ediyorsanız, `query`, `fragment`veya `form_post`. `form_post`kodu içeren bir POST'u yeniden yönlendirme URI'nize yürütür. Daha fazla bilgi için [OpenID Connect protokolüne](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code)bakın.  |
| `state`                 | Önerilen | Belirteç yanıtında da döndürülecek isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Rasgele oluşturulan benzersiz değer genellikle [çapraz site isteği sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Değer, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri de uygulamada kodlayabilir( örneğin, üzerinde oldukları sayfa veya görünüm). |
| `prompt`  | isteğe bağlı    | Gerekli kullanıcı etkileşimi türünü gösterir. Şu anda tek geçerli `login` `none`değerler `consent`, , ve .<br/><br/>- `prompt=login`kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve tek oturum açmayı olumsuzlar.<br/>- `prompt=none`tam tersidir - kullanıcıya herhangi bir etkileşimli istemi nin sunulmamasını sağlar. İstek tek oturum açma yoluyla sessizce tamamlanamazsa, Microsoft kimlik platformu `interaction_required` bitiş noktası bir hata döndürecektir.<br/>- `prompt=consent`kullanıcı oturum açtıktan sonra OAuth onay iletişim kutusunu tetikler ve kullanıcıdan uygulamaya izin vermesini ister.<br/>- `prompt=select_account`oturumda veya hatırlanan tüm hesapları listeleyen tek oturumda hesap seçimi deneyimini veya tamamen farklı bir hesap kullanmayı seçme seçeneğini kesintiye uğratır.<br/> |
| `login_hint`  | isteğe bağlı    | Kullanıcı adını önceden biliyorsanız, oturum açma sayfasının kullanıcı adı/e-posta adresi alanını önceden doldurmak için kullanılabilir. Genellikle uygulamalar, `preferred_username` talebi kullanarak önceki bir oturum açma dan kullanıcı adını ayıklamış olan yeniden kimlik doğrulama sırasında bu parametreyi kullanır.   |
| `domain_hint`  | isteğe bağlı    | Biri olabilir `consumers` ya `organizations`da .<br/><br/>Dahil edilirse, kullanıcının oturum açma sayfasında geçtiği e-posta tabanlı bulma işlemini atlar ve bu da biraz daha kolay laştırılmış bir kullanıcı deneyimine yol açacaktır. Uygulamalar genellikle, önceki oturum açma `tid` işlemlerinden bu parametreyi ayıklayarak yeniden kimlik doğrulaması sırasında kullanır. `tid` Talep değeri ise, `9188040d-6c67-4c5b-b112-36a304b66dad`kullanmalısınız. `domain_hint=consumers` Aksi takdirde, kullanın. `domain_hint=organizations`  |
| `code_challenge_method` | isteğe bağlı    | `code_challenge` Parametreyi `code_verifier` kodlamak için kullanılan yöntem. Aşağıdaki değerlerden biri olabilir:<br/><br/>- `plain` <br/>- `S256`<br/><br/>Dışlanırsa, `code_challenge` dahil edilirse düz metin `code_challenge` olarak kabul edilir. Microsoft kimlik platformu `plain` `S256`hem destekler ve . Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636)bakın. |
| `code_challenge`  | isteğe bağlı | Yerel bir istemciden Proof Key for Code Exchange (PKCE) aracılığıyla yetkilendirme kodu hibelerini güvence altına almak için kullanılır. Dahil `code_challenge_method` edilirse gereklidir. Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636)bakın. |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft kimlik platformu bitiş noktası, kullanıcının `scope` sorgu parametresinde belirtilen izinleri kabul etmesini de sağlar. Kullanıcı bu izinlerden herhangi birini kabul etmediyse, kullanıcıdan gerekli izinleri onaylamasını ister. [İzinlerin, izinlerin ve çok kiracılı uygulamaların ayrıntıları burada verilmiştir.](v2-permissions-and-consent.md)

Kullanıcı kimlik doğrulaması yaptıktan ve onay verdikten sonra, Microsoft kimlik platformu bitiş `redirect_uri` `response_mode` noktası, parametrede belirtilen yöntemi kullanarak belirtilen uygulamanıza bir yanıt döndürür.

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir `response_mode=query` yanıt kullanarak gibi görünüyor:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametre | Açıklama  |
|-----------|--------------|
| `code` | Uygulamanın istediği authorization_code. Uygulama, hedef kaynak için bir erişim jetonu istemek için yetkilendirme kodunu kullanabilir. Authorization_codes kısa ömürlüdür, genellikle yaklaşık 10 dakika sonra sona erer. |
| `state` | İsteğe bir durum parametresi dahil edilirse, yanıtta aynı değer in görünmesi gerekir. Uygulama, istek ve yanıttaki durum değerlerinin aynı olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Hata yanıtları, uygulamanın `redirect_uri` bunları uygun şekilde işleyebilmeleri için de gönderilebilir:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama  |
|----------|------------------|
| `error`  | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme bitiş noktası hataları için hata kodları

Aşağıdaki tabloda hata yanıtı `error` parametresi döndürülebilir çeşitli hata kodları açıklanır.

| Hata Kodu  | Açıklama    | İstemci Eylemi   |
|-------------|----------------|-----------------|
| `invalid_request` | Eksik gerekli parametre gibi protokol hatası. | İsteği düzeltin ve yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır. |
| `unauthorized_client` | İstemci uygulamasının yetkilendirme kodu istemesine izin verilmez. | Bu hata genellikle istemci uygulaması Azure AD'de kayıtlı değilse veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| `access_denied`  | Kaynak sahibi nin rızası reddedildi  | İstemci uygulaması, kullanıcı izin vermedikçe devam edilemeyecek kullanıcıya bildirebilir. |
| `unsupported_response_type` | Yetkilendirme sunucusu istekteki yanıt türünü desteklemez. | İsteği düzeltin ve yeniden gönderin. Bu genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır.  |
| `server_error`  | Sunucu beklenmeyen bir hatayla karşılaştı.| İsteği yeniden deneyin. Bu hatalar geçici koşullardan kaynaklanabilir. İstemci uygulaması kullanıcıya yanıtının geçici bir hata nedeniyle geciktiğini açıklayabilir. |
| `temporarily_unavailable`   | Sunucu geçici olarak isteği işlemek için çok meşgul. | İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtının geciktiğini kullanıcıya açıklayabilir. |
| `invalid_resource`  | Hedef kaynak yok, Azure AD bulamadığı veya doğru şekilde yapılandırılamadığı için geçersizdir. | Bu hata, varsa kaynağın kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| `login_required` | Çok fazla veya hiç kullanıcı bulunamadı | İstemci sessiz kimlik`prompt=none`doğrulaması istedi ( ), ancak tek bir kullanıcı bulunamadı. Bu, oturumda etkin olan veya kullanıcı olmayan birden çok kullanıcı olduğu anlamına gelebilir. Bu, seçilen kiracıyı dikkate alır (örneğin, iki Azure AD hesabı etkin `consumers` ve bir Microsoft hesabı varsa ve seçilirse, sessiz kimlik doğrulaması çalışır). |
| `interaction_required` | İstek kullanıcı etkileşimi gerektirir. | Ek bir kimlik doğrulama adımı veya onayı gereklidir. İsteği olmadan `prompt=none`yeniden deneyin. |

## <a name="request-an-access-token"></a>Erişim jetonu isteme

Artık bir authorization_code edindiğinize ve kullanıcı tarafından izin aldığınıza `code` göre, `access_token` istediğiniz kaynak için kullanabilirsiniz. Bunu, bitiş `POST` noktasına bir istek göndererek yapın: `/token`

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps. This secret needs to be URL-Encoded.
```

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin! (Değiştirmeyi unutmayın `code`) [Postacı'da bu isteği çalıştırmayı deneyin ![](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

| Parametre  | Gerekli/isteğe bağlı | Açıklama     |
|------------|-------------------|----------------|
| `tenant`   | gerekli   | İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın.  |
| `client_id` | gerekli  | Uygulama (istemci) kimliği, [Azure portalı – Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atanır. |
| `grant_type` | gerekli   | Yetkilendirme `authorization_code` kodu akışı için olmalıdır.   |
| `scope`      | gerekli   | Alan ayrılmış kapsamlistesi. Bu bacakta istenen kapsamlar, ilk bacakta istenen kapsamların bir alt kümesine veya bir alt kümesine eşdeğer olmalıdır. Kapsamların tümü, OIDC kapsamları ile birlikte tek`profile`bir `openid` `email`kaynaktan olmalıdır ( , , . Kapsamların daha ayrıntılı bir açıklaması için [izinlere, onaya ve kapsamlara](v2-permissions-and-consent.md)bakın. |
| `code`          | gerekli  | Akışın ilk ayağında elde ettiğiniz authorization_code. |
| `redirect_uri`  | gerekli  | authorization_code elde etmek için kullanılan aynı redirect_uri değeri. |
| `client_secret` | web uygulamaları için gerekli | Uygulamanız için uygulama kayıt portalında oluşturduğunuz uygulama sırrı. client_secrets cihazlarda güvenilir bir şekilde depolanamayacağından, uygulama sırrını yerel bir uygulamada kullanmamalısınız. Client_secret sunucu tarafında güvenli bir şekilde depolama olan web uygulamaları ve web API'leri için gereklidir.  İstemci sırrı gönderilmeden önce URL kodlanmış olmalıdır. Daha fazla bilgi için [buraya](https://tools.ietf.org/html/rfc3986#page-12)tıklayın. |
| `code_verifier` | isteğe bağlı  | Authorization_code elde etmek için kullanılan aynı code_verifier. Yetki kodu hibe isteğinde PKCE kullanılıyorsa gereklidir. Daha fazla bilgi için [PKCE RFC'ye](https://tools.ietf.org/html/rfc7636)bakın. |

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir belirteç yanıtı gibi görünecektir:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```

| Parametre     | Açıklama   |
|---------------|------------------------------|
| `access_token`  | İstenen erişim jetonu. Uygulama, web API gibi güvenli kaynağa kimlik doğrulamak için bu belirteci kullanabilir.  |
| `token_type`    | Belirteç türü değerini gösterir. Azure AD'nin desteklediği tek tür Taşıyıcıdır |
| `expires_in`    | Erişim belirteci nin ne kadar süre geçerli olduğu (saniye cinsinden). |
| `scope`         | access_token için geçerli olan kapsamlar. |
| `refresh_token` | Bir OAuth 2.0 yenileme belirteci. Uygulama, geçerli erişim belirteci süresi dolduktan sonra ek erişim belirteçleri edinme bu belirteci kullanabilirsiniz. Refresh_tokens uzun ömürlüdür ve uzun süre kaynaklara erişimi korumak için kullanılabilir. Erişim belirteci yenileme hakkında daha fazla ayrıntı için [aşağıdaki bölüme](#refresh-the-access-token)bakın. <br> **Not:** Yalnızca kapsam `offline_access` isteniyorsa sağlanır. |
| `id_token`      | Bir JSON Web Belirteci (JWT). Uygulama, oturum giren kullanıcı hakkında bilgi istemek için bu belirteç segmentlerinin şifresini çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bunlara güvenmemelidir. id_tokens hakkında daha fazla [`id_token reference`](id-tokens.md)bilgi için bkz. <br> **Not:** Yalnızca kapsam `openid` isteniyorsa sağlanır. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları gibi görünecektir:

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre         | Açıklama    |
|-------------------|----------------|
| `error`       | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |
| `error_codes` | Tanılamada yardımcı olabilecek STS'ye özgü hata kodlarının listesi.  |
| `timestamp`   | Hatanın oluştuğu saat. |
| `trace_id`    | Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | Bileşenler arasında tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |

### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç bitiş noktası hataları için hata kodları

| Hata Kodu         | Açıklama        | İstemci Eylemi    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Eksik gerekli parametre gibi protokol hatası. | İsteği düzeltme ve yeniden gönderme   |
| `invalid_grant`    | Yetkilendirme kodu veya PKCE kod doğrulayıcısı geçersiz veya süresi dolmuş. | `/authorize` Bitiş noktasına yeni bir istek deneyin ve code_verifier parametresinin doğru olduğunu doğrulayın.  |
| `unauthorized_client` | Kimliği doğrulanan istemci, bu yetkilendirme hibe türünü kullanma yetkisine izin vermez. | Bu genellikle istemci uygulaması Azure AD'de kayıtlı değilse veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir. |
| `invalid_client` | İstem kimlik doğrulaması başarısız oldu.  | İstemci kimlik bilgileri geçerli değil. Düzeltmek için, uygulama yöneticisi kimlik bilgilerini güncelleştirir.   |
| `unsupported_grant_type` | Yetkilendirme sunucusu yetkilendirme hibe türünü desteklemez. | İstekteki hibe türünü değiştirin. Bu tür hatalar yalnızca geliştirme sırasında ortaya çıkmalıdır ve ilk sınama sırasında algılanmalıdır. |
| `invalid_resource` | Hedef kaynak yok, Azure AD bulamadığı veya doğru şekilde yapılandırılamadığı için geçersizdir. | Bu, varsa kaynağın kiracıda yapılandırılmadığını gösterir. Uygulama, kullanıcıdan uygulamayı yüklemesi ve Azure AD'ye eklemesi için talimat isteyebilir.  |
| `interaction_required` | İstek kullanıcı etkileşimi gerektirir. Örneğin, ek bir kimlik doğrulama adımı gereklidir. | İsteği aynı kaynakla yeniden deneyin.  |
| `temporarily_unavailable` | Sunucu geçici olarak isteği işlemek için çok meşgul. | İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtının geciktiğini kullanıcıya açıklayabilir. |

## <a name="use-the-access-token"></a>Erişim belirteci kullanma

Artık başarılı bir şekilde bir `access_token`, web API'lerine gelen isteklerde belirteci `Authorization` üstbilgiye ekleyerek kullanabilirsiniz:

> [!TIP]
> Postacı bu isteği yürütmek! (Önce `Authorization` üstbilgi değiştir) [Postacı'da bu isteği çalıştırmayı deneyin ![](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Erişim belirteciyenileme

Access_tokens kısa ömürlüdür ve kaynaklara erişmeye devam etmek için süresi dolduktan sonra bunları yenilemeniz gerekir. Bunu, bitiş noktasına başka `POST` bir istek göndererek yapabilirsiniz, `refresh_token` bu kez yerine sağlayan `code` `/token`  Yenileme belirteçleri, istemcinizin zaten onay aldığı tüm izinler için geçerlidir - `scope=mail.read` bu nedenle, bir istek üzerine `scope=api://contoso.com/api/UseResource`verilen bir yenileme belirteci için yeni bir erişim belirteci istemek için kullanılabilir.  

Belirteçleri yenilemenin belirli kullanım ömürleri yoktur. Genellikle, yenileme belirteçleri yaşam sürelerini nispeten uzun. Ancak, bazı durumlarda, yenileme belirteçleri sona erer, iptal edilir veya istenen eylem için yeterli ayrıcalıkları yoksun. Uygulamanızın [belirteç verme bitiş noktası tarafından döndürülen hataları](#error-codes-for-token-endpoint-errors) doğru şekilde beklemesi ve işlemesi gerekir. 

Yeni erişim belirteçleri elde etmek için kullanıldığında yeni belirteçleri iptal olmasa da, eski yenileme belirtecisini atmanız beklenir. [OAuth 2.0 spec](https://tools.ietf.org/html/rfc6749#section-6) diyor ki: "Yetkilendirme sunucusu MAY, bu durumda istemci eski yenileme belirteci atmak ve yeni yenileme belirteci ile değiştirmek gerekir yeni bir yenileme belirteci sorunu. Yetkilendirme sunucusu, istemciye yeni bir yenileme belirteci verdikten sonra eski yenileme belirteci iptal edebilir."  

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps. This secret needs to be URL-Encoded
```

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin! (Değiştirmeyi unutmayın `refresh_token`) [Postacı'da bu isteği çalıştırmayı deneyin ![](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)
> 

| Parametre     |                | Açıklama        |
|---------------|----------------|--------------------|
| `tenant`        | gerekli     | İstek `{tenant}` yolundaki değer, uygulamada kimlerin oturum açabileceğini denetlemek için kullanılabilir. İzin verilen `common`değerler `organizations` `consumers`, , , ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için [protokol temellerine](active-directory-v2-protocols.md#endpoints)bakın.   |
| `client_id`     | gerekli    | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `grant_type`    | gerekli    | Yetkilendirme `refresh_token` kodu akışının bu ayağı için olmalı. |
| `scope`         | gerekli    | Alan ayrılmış kapsamlistesi. Bu bacakta istenen kapsamlar, özgün authorization_code istek bacağında istenen kapsamların bir alt kümesine veya bir alt kümesine eşdeğer olmalıdır. Bu istekte belirtilen kapsamlar birden çok kaynak sunucusuna yayılıyorsa, Microsoft kimlik platformu bitiş noktası ilk kapsamda belirtilen kaynak için bir belirteç döndürecektir. Kapsamların daha ayrıntılı bir açıklaması için [izinlere, onaya ve kapsamlara](v2-permissions-and-consent.md)bakın. |
| `refresh_token` | gerekli    | Akıntının ikinci ayağında elde refresh_token. |
| `client_secret` | web uygulamaları için gerekli | Uygulamanız için uygulama kayıt portalında oluşturduğunuz uygulama sırrı. client_secrets cihazlarda güvenilir bir şekilde depolanamayacağından, yerel bir uygulamada kullanılmamalıdır. Client_secret sunucu tarafında güvenli bir şekilde depolama olan web uygulamaları ve web API'leri için gereklidir. Bu gizli URL-Kodlanmış olması gerekir, daha fazla bilgi için [buraya](https://tools.ietf.org/html/rfc3986#page-12)tıklayın. |

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir belirteç yanıtı gibi görünecektir:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre     | Açıklama         |
|---------------|-------------------------------------------------------------|
| `access_token`  | İstenen erişim jetonu. Uygulama, web API gibi güvenli kaynağa kimlik doğrulamak için bu belirteci kullanabilir. |
| `token_type`    | Belirteç türü değerini gösterir. Azure AD'nin desteklediği tek tür Taşıyıcıdır |
| `expires_in`    | Erişim belirteci nin ne kadar süre geçerli olduğu (saniye cinsinden).   |
| `scope`         | access_token için geçerli olan kapsamlar.    |
| `refresh_token` | Yeni bir OAuth 2.0 yenileme belirteci. Yenileme belirteçlerinizin mümkün olduğunca uzun süre geçerli kalmasını sağlamak için eski yenileme belirteciyle yeni alınan bu yenileme belirteciyle değiştirmelisiniz. <br> **Not:** Yalnızca kapsam `offline_access` isteniyorsa sağlanır.|
| `id_token`      | İmzasız Bir JSON Web Belirteci (JWT). Uygulama, oturum giren kullanıcı hakkında bilgi istemek için bu belirteç segmentlerinin şifresini çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bunlara güvenmemelidir. id_tokens hakkında daha fazla [`id_token reference`](id-tokens.md)bilgi için bkz. <br> **Not:** Yalnızca kapsam `openid` isteniyorsa sağlanır. |

#### <a name="error-response"></a>Hata yanıtı

```json
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre         | Açıklama                                                                                        |
|-------------------|----------------------------------------------------------------------------------------------------|
| `error`           | Oluşan hata türlerini sınıflandırmak için kullanılabilecek ve hatalara tepki vermek için kullanılabilecek bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının temel nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi.           |
| `error_codes` |Tanılamada yardımcı olabilecek STS'ye özgü hata kodlarının listesi. |
| `timestamp` | Hatanın oluştuğu saat. |
| `trace_id` | Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | Bileşenler arasında tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |

Hata kodlarının ve önerilen istemci eyleminin açıklaması [için belirteç bitiş noktası hataları için hata kodlarına](#error-codes-for-token-endpoint-errors)bakın.
