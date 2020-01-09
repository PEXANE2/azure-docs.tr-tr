---
title: OAuth yetkilendirme kodu akışı-Microsoft Identity platform | Mavisi
description: OAuth 2,0 kimlik doğrulama protokolünün Microsoft Identity platform uygulamasını kullanarak Web uygulamaları oluşturun.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f49e93d6dd405e849dec3806c979b1ea2fc1d37
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75423266"
---
# <a name="microsoft-identity-platform-and-oauth-20-authorization-code-flow"></a>Microsoft Identity platform ve OAuth 2,0 yetkilendirme kodu akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

OAuth 2,0 yetkilendirme kodu verme, Web API 'Leri gibi korumalı kaynaklara erişim kazanmak için bir cihaza yüklenen uygulamalarda kullanılabilir. OAuth 2,0 Microsoft Identity platform uygulamasını kullanarak, mobil ve Masaüstü uygulamalarınıza oturum açma ve API erişimi ekleyebilirsiniz. Bu kılavuz dilden bağımsızdır ve [Azure açık kaynaklı kimlik doğrulama kitaplıklarının](reference-v2-libraries.md)HIÇBIRINI kullanmadan http iletilerinin nasıl gönderileceğini ve alınacağını açıklar.

Bu makalede, uygulamanızdaki protokolde doğrudan programlanın nasıl yapılacağı açıklanır.  Mümkün olduğunda, [belirteçleri edinmek ve güvenli Web API 'lerini çağırmak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)Için desteklenen Microsoft kimlik doğrulama KITAPLıKLARıNı (msal) kullanmanızı öneririz.  Ayrıca [, msal kullanan örnek uygulamalara](sample-v2-code.md)göz atın.

> [!NOTE]
> Tüm Azure Active Directory senaryoları & Özellikler Microsoft Identity platform uç noktası tarafından desteklenmez. Microsoft Identity platform uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

OAuth 2,0 yetkilendirme kodu akışı, [oauth 2,0 belirtiminin 4,1 bölümünde](https://tools.ietf.org/html/rfc6749)açıklanmaktadır. [Web uygulamaları](v2-app-types.md#web-apps) ve [yerel olarak yüklenen uygulamalar](v2-app-types.md#mobile-and-native-apps)dahil olmak üzere uygulama türlerinin çoğunda kimlik doğrulama ve yetkilendirme gerçekleştirmek için kullanılır. Flow, uygulamaların Microsoft Identity platform uç noktası tarafından güvenliği sağlanmış kaynaklara erişmek için kullanılabilecek access_tokens güvenli bir şekilde almasına olanak sağlar.

## <a name="protocol-diagram"></a>Protokol diyagramı

Yüksek düzeyde, yerel/mobil uygulama için tüm kimlik doğrulama akışı şuna benzer bir bit:

![OAuth kimlik doğrulama kod akışı](./media/v2-oauth2-auth-code-flow/convergence-scenarios-native.svg)

## <a name="request-an-authorization-code"></a>Yetkilendirme kodu iste

Yetkilendirme kodu akışı, kullanıcıyı `/authorize` uç noktasına yönlendiren istemciyle başlar. Bu istekte istemci, kullanıcıdan almaları gereken izinleri gösterir:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&state=12345
```

> [!TIP]
> Bu isteği yürütmek için aşağıdaki bağlantıya tıklayın! Oturum açtıktan sonra tarayıcınızın, adres çubuğunda bir `code` `https://localhost/myapp/` yeniden yönlendirilmesi gerekir.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fuser.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parametre    | Gerekli/isteğe bağlı | Açıklama |
|--------------|-------------|--------------|
| `tenant`    | {1&gt;gerekli&lt;1}    | İsteğin yolundaki `{tenant}` değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints).  |
| `client_id`   | {1&gt;gerekli&lt;1}    | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminin uygulamanıza atandığı **uygulama (istemci) kimliği** .  |
| `response_type` | {1&gt;gerekli&lt;1}    | , Yetkilendirme kodu akışı için `code` içermelidir.       |
| `redirect_uri`  | {1&gt;gerekli&lt;1} | Uygulamanızın, kimlik doğrulama yanıtlarının sizin uygulamanız tarafından gönderilebileceği ve alınabileceği redirect_uri. Portalın, URL kodlamalı olması dışında, portalda kaydettiğiniz redirect_uris biriyle tam olarak eşleşmesi gerekir. Yerel & Mobile Apps için `https://login.microsoftonline.com/common/oauth2/nativeclient`varsayılan değerini kullanmanız gerekir.   |
| `scope`  | {1&gt;gerekli&lt;1}    | Kullanıcının onay vermesini istediğiniz [kapsamların](v2-permissions-and-consent.md) , boşlukla ayrılmış bir listesi.  İsteğin `/authorize` baması için, bu birden fazla kaynağı kapsayabilir ve uygulamanızın çağırmak istediğiniz birden çok Web API 'si için onay almasını sağlar. |
| `response_mode`   | önerilen | Elde edilen belirteci uygulamanıza geri göndermek için kullanılması gereken yöntemi belirtir. Aşağıdakilerden biri olabilir:<br/><br/>- `query`<br/>- `fragment`<br/>- `form_post`<br/><br/>`query`, yeniden yönlendirme URI 'niz üzerinde bir sorgu dizesi parametresi olarak kodu sağlar. Örtük akışı kullanarak bir KIMLIK belirteci isteğinde bulunduğsanız, [OpenID belirtiminde](https://openid.net/specs/oauth-v2-multiple-response-types-1_0.html#Combinations)belirtilen şekilde `query` kullanamazsınız. Yalnızca kod isteğinde bulunduğsanız `query`, `fragment`veya `form_post`kullanabilirsiniz. `form_post` yeniden yönlendirme URI 'nize kodu içeren bir GÖNDERI yürütür. Daha fazla bilgi için bkz. [OpenID Connect Protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  |
| `state`                 | önerilen | İsteğin belirteç yanıtında de döndürülecek bir değer. Bu, istediğiniz herhangi bir içerik dizesi olabilir. Rastgele oluşturulan benzersiz bir değer genellikle [siteler arası istek sahteciliği saldırılarını önlemek](https://tools.ietf.org/html/rfc6749#section-10.12)için kullanılır. Bu değer Ayrıca, kullanıcının uygulamadaki durumuyla ilgili bilgileri, kimlik doğrulama isteği gerçekleştirilmeden önce (örneğin, bulunan sayfa veya Görünüm) kodlayabilir. |
| `prompt`  | isteğe bağlı    | Gerekli kullanıcı etkileşiminin türünü gösterir. Şu anda yalnızca geçerli değerler `login`, `none`ve `consent`.<br/><br/>- `prompt=login`, kullanıcıyı bu istek üzerine kimlik bilgilerini girmeye zorlar ve tek oturum açma 'yı yok eder.<br/>- `prompt=none` bunun tersidir; kullanıcının herhangi bir etkileşimli istem ile sunulmayacağını garanti eder. İstek, tek oturum açma yoluyla sessizce tamamlanamayacak, Microsoft Identity platform uç noktası bir `interaction_required` hatası döndürür.<br/>- `prompt=consent`, Kullanıcı oturum açtıktan sonra kullanıcıdan uygulamaya izin vermesini isteyen OAuth onay iletişim kutusunu tetikleyecektir. |
| `login_hint`  | isteğe bağlı    | Kullanıcı adının bir süre önce bilinerek Kullanıcı için oturum açma sayfasının Kullanıcı adı/e-posta adresi alanını önceden doldurmanız için kullanılabilir. Genellikle uygulamalar bu parametreyi yeniden kimlik doğrulama sırasında kullanır ve `preferred_username` talebini kullanarak önceki bir oturum açma işleminden zaten Kullanıcı adını ayıklamış olur.   |
| `domain_hint`  | isteğe bağlı    | `consumers` veya `organizations`biri olabilir.<br/><br/>Dahil edilse, kullanıcının oturum açma sayfasında, daha kolay bir kullanıcı deneyimi için önde gelen e-posta tabanlı bulma işlemini atlar. Genellikle uygulamalar, önceki bir oturum açma işleminden `tid` çıkararak bu parametreyi yeniden kimlik doğrulama sırasında kullanacaktır. `tid` talep değeri `9188040d-6c67-4c5b-b112-36a304b66dad`, `domain_hint=consumers`kullanmanız gerekir. Aksi takdirde `domain_hint=organizations`kullanın.  |
| `code_challenge_method` | isteğe bağlı    | `code_challenge` parametresi için `code_verifier` kodlamak için kullanılan yöntem. Aşağıdaki değerlerden biri olabilir:<br/><br/>- `plain` <br/>- `S256`<br/><br/>Dışlanmazsa, `code_challenge` dahil edildikçe `code_challenge` düz metin olarak kabul edilir. Microsoft Identity platform hem `plain` hem de `S256`destekler. Daha fazla bilgi için bkz. [Pkce RFC](https://tools.ietf.org/html/rfc7636). |
| `code_challenge`  | isteğe bağlı | Yerel bir istemciden kod değişimi (PKCE) için kanıt anahtarı aracılığıyla yetkilendirme kodu yetkisini güvenli hale getirmek için kullanılır. `code_challenge_method` varsa gereklidir. Daha fazla bilgi için bkz. [Pkce RFC](https://tools.ietf.org/html/rfc7636). |

Bu noktada, kullanıcıdan kimlik bilgilerini girmesi ve kimlik doğrulamasını tamamlaması istenir. Microsoft Identity platform uç noktası ayrıca kullanıcının `scope` sorgu parametresinde belirtilen izinlere onay aldığından emin olur. Kullanıcı bu izinlerden herhangi birine onay vermediyseniz, kullanıcıdan gerekli izinleri onaylaması istenir. [İzinlerin, izin ve çok kiracılı uygulamaların ayrıntıları burada verilmiştir](v2-permissions-and-consent.md).

Kullanıcı kimlik doğrulamasından ve izin verdiğinde, Microsoft Identity platform uç noktası, `response_mode` parametresinde belirtilen yöntemi kullanarak belirtilen `redirect_uri`uygulamanıza bir yanıt döndürür.

#### <a name="successful-response"></a>Başarılı yanıt

`response_mode=query` kullanarak başarılı bir yanıt şöyle görünür:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametre | Açıklama  |
|-----------|--------------|
| `code` | Uygulamanın istediği authorization_code. Uygulama, hedef kaynak için bir erişim belirteci istemek üzere yetkilendirme kodunu kullanabilir. Authorization_codes kısa süreli, genellikle yaklaşık 10 dakika sonra süresi dolacak. |
| `state` | İsteğe bir durum parametresi dahil edilir, yanıtta aynı değer görünmelidir. Uygulama, istek ve yanıtta durum değerlerinin özdeş olduğunu doğrulamalıdır. |

#### <a name="error-response"></a>Hata yanıtı

Ayrıca, uygulamanın bunları uygun şekilde işleyebilmesi için `redirect_uri` hata yanıtları da gönderilebilir:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametre | Açıklama  |
|----------|------------------|
| `error`  | Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Yetkilendirme uç noktası hataları için hata kodları

Aşağıdaki tabloda hata yanıtının `error` parametresinde döndürülebilecek çeşitli hata kodları açıklanmaktadır.

| Hata Kodu  | Açıklama    | İstemci eylemi   |
|-------------|----------------|-----------------|
| `invalid_request` | Eksik gerekli bir parametre gibi protokol hatası. | İsteği onarın ve yeniden gönderin. Bu, genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır. |
| `unauthorized_client` | İstemci uygulamasının bir yetkilendirme kodu istemesine izin verilmez. | Bu hata genellikle istemci uygulaması Azure AD 'de kayıtlı olmadığında veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama kullanıcıya uygulamayı yükleme ve Azure AD 'ye ekleme yönergesini isteyebilir. |
| `access_denied`  | Kaynak sahibi reddedildi onayı  | İstemci uygulaması, kullanıcıya Kullanıcı tarafından bağlanmadığı takdirde devam edemediği konusunda bildirimde bulunabilir. |
| `unsupported_response_type` | Yetkilendirme sunucusu istekteki yanıt türünü desteklemiyor. | İsteği onarın ve yeniden gönderin. Bu, genellikle ilk sınama sırasında yakalanan bir geliştirme hatasıdır.  |
| `server_error`  | Sunucu beklenmeyen bir hatayla karşılaştı.| İsteği yeniden deneyin. Bu hatalar geçici koşullardan kaynaklanabilir. İstemci uygulaması, yanıtı geçici bir hataya geciktiğinde kullanıcıya açıklayabilir. |
| `temporarily_unavailable`   | Sunucu, isteği işlemek için geçici olarak çok meşgul. | İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtı geciktirildiği kullanıcıya açıklanmayabilir. |
| `invalid_resource`  | Hedef kaynak geçersiz, çünkü mevcut değil, Azure AD bu dosyayı bulamıyor veya doğru şekilde yapılandırılmamış. | Bu hata, varsa kaynağın kiracıda yapılandırılmamış olduğunu gösterir. Uygulama kullanıcıya uygulamayı yükleme ve Azure AD 'ye ekleme yönergesini isteyebilir. |
| `login_required` | Çok fazla veya hiç Kullanıcı bulunamadı | İstemci sessiz kimlik doğrulaması istedi (`prompt=none`), ancak tek bir Kullanıcı bulunamadı. Bu, oturumda etkin olan veya Kullanıcı olmayan birden çok kullanıcı olduğu anlamına gelebilir. Bu, kiracının seçtiği hesaba (örneğin, iki Azure AD hesabı etkin ve bir Microsoft hesabı ve `consumers` seçilirse, sessiz kimlik doğrulaması çalışacaktır) uygulanır. |
| `interaction_required` | İstek, Kullanıcı etkileşimi gerektirir. | Ek bir kimlik doğrulama adımı veya onayı gerekir. `prompt=none`olmadan isteği yeniden deneyin. |

## <a name="request-an-access-token"></a>Erişim belirteci isteme

Artık authorization_code edindiniz ve Kullanıcı tarafından izin verildiğinden, istenen kaynağa bir `access_token` `code` kullanabilirsiniz. Bunu, `/token` uç noktasına bir `POST` isteği göndererek yapın:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Bu isteği Postman 'da yürütmeyi deneyin! (`code`değiştirmeyi unutmayın) [![bu Isteği Postman 'da çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

| Parametre  | Gerekli/isteğe bağlı | Açıklama     |
|------------|-------------------|----------------|
| `tenant`   | {1&gt;gerekli&lt;1}   | İsteğin yolundaki `{tenant}` değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints).  |
| `client_id` | {1&gt;gerekli&lt;1}  | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atandığı uygulama (ISTEMCI) kimliği. |
| `grant_type` | {1&gt;gerekli&lt;1}   | Yetkilendirme kodu akışı için `authorization_code` olmalıdır.   |
| `scope`      | {1&gt;gerekli&lt;1}   | Kapsamların boşlukla ayrılmış listesi. Bu Bata istenen kapsamlar, ilk Bata istenen kapsamların alt kümesiyle veya buna eşit olmalıdır. Kapsamların hepsi, OıDC kapsamlarıyla birlikte (`profile`, `openid`, `email`) tek bir kaynaktan olmalıdır. Kapsamların daha ayrıntılı bir açıklaması için [izinler, onay ve kapsamlar](v2-permissions-and-consent.md)' a bakın. |
| `code`          | {1&gt;gerekli&lt;1}  | Akışın ilk Bata elde ettiğiniz authorization_code. |
| `redirect_uri`  | {1&gt;gerekli&lt;1}  | Authorization_code elde etmek için kullanılan aynı redirect_uri değeri. |
| `client_secret` | Web uygulamaları için gerekli | Uygulamanız için uygulama kayıt portalında oluşturduğunuz uygulama gizli anahtarı. Client_secrets cihazlarda güvenilir bir şekilde depolanamadığı için, yerel bir uygulamada uygulama gizli anahtarını kullanmamanız gerekir. Client_secret sunucu tarafında güvenli bir şekilde depolayabilme özelliğine sahip Web uygulamaları ve Web API 'Leri için gereklidir.  İstemci parolası gönderilmeden önce URL kodlamalı olmalıdır.  |
| `code_verifier` | isteğe bağlı  | Authorization_code elde etmek için kullanılan aynı code_verifier. Yetkilendirme kodu verme isteğinde PKCE kullanılmışsa gereklidir. Daha fazla bilgi için bkz. [Pkce RFC](https://tools.ietf.org/html/rfc7636). |

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir belirteç yanıtı şöyle görünür:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```

| Parametre     | Açıklama   |
|---------------|------------------------------|
| `access_token`  | İstenen erişim belirteci. Uygulama, bir Web API 'SI gibi güvenli kaynak üzerinde kimlik doğrulaması yapmak için bu belirteci kullanabilir.  |
| `token_type`    | Belirteç türü değerini gösterir. Azure AD 'nin desteklediği tek tür taşıyıcı |
| `expires_in`    | Erişim belirtecinin geçerli olduğu süre (saniye cinsinden). |
| `scope`         | Access_token için geçerli olan kapsamlar. |
| `refresh_token` | Bir OAuth 2,0 yenileme belirteci. Uygulama bu belirteci kullanabilir ve geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçleri elde edebilir. Refresh_tokens uzun ömürlü ve uzun süre boyunca kaynaklara erişimi sürdürmek için kullanılabilir. Erişim belirtecini yenileme hakkında daha fazla ayrıntı için [aşağıdaki bölüme](#refresh-the-access-token)bakın. <br> **Note:** Yalnızca `offline_access` kapsam isteniyorsa sağlandı. |
| `id_token`      | Bir JSON Web Token (JWT). Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere dayanmamalıdır. İd_tokens hakkında daha fazla bilgi için bkz. [`id_token reference`](id-tokens.md). <br> **Note:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |

### <a name="error-response"></a>Hata yanıtı

Hata yanıtları şöyle görünür:

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
| `error`       | Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi. |
| `error_codes` | Tanılamada yardımcı olabilecek STS 'ye özgü hata kodlarının bir listesi.  |
| `timestamp`   | Hatanın oluştuğu zaman. |
| `trace_id`    | Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | İsteğe ait, bileşenler genelinde tanılamada yardımcı olabilecek benzersiz bir tanımlayıcı. |

### <a name="error-codes-for-token-endpoint-errors"></a>Belirteç uç noktası hataları için hata kodları

| Hata Kodu         | Açıklama        | İstemci eylemi    |
|--------------------|--------------------|------------------|
| `invalid_request`  | Eksik gerekli bir parametre gibi protokol hatası. | İsteği onarın ve yeniden gönderin   |
| `invalid_grant`    | Yetkilendirme kodu veya PKCE kod doğrulayıcısı geçersiz veya süresi doldu. | `/authorize` uç noktasına yeni bir istek deneyin ve code_verifier parametresinin doğru olduğunu doğrulayın.  |
| `unauthorized_client` | Kimliği doğrulanmış istemci, bu yetkilendirme verme türünü kullanma yetkisine sahip değil. | Bu durum genellikle istemci uygulaması Azure AD 'de kayıtlı olmadığında veya kullanıcının Azure AD kiracısına eklenmediğinde oluşur. Uygulama kullanıcıya uygulamayı yükleme ve Azure AD 'ye ekleme yönergesini isteyebilir. |
| `invalid_client` | İstemci kimlik doğrulaması başarısız oldu.  | İstemci kimlik bilgileri geçerli değil. Bu uygulamayı onarmak için, uygulama Yöneticisi kimlik bilgilerini güncelleştirir.   |
| `unsupported_grant_type` | Yetkilendirme sunucusu yetkilendirme verme türünü desteklemiyor. | İstekteki izin türünü değiştirin. Bu tür bir hata yalnızca geliştirme sırasında ve ilk test sırasında algılanarak gerçekleştirilmelidir. |
| `invalid_resource` | Hedef kaynak geçersiz, çünkü mevcut değil, Azure AD bu dosyayı bulamıyor veya doğru şekilde yapılandırılmamış. | Bu, varsa kaynağın kiracıda yapılandırılmamış olduğunu gösterir. Uygulama kullanıcıya uygulamayı yükleme ve Azure AD 'ye ekleme yönergesini isteyebilir.  |
| `interaction_required` | İstek, Kullanıcı etkileşimi gerektirir. Örneğin, ek bir kimlik doğrulama adımı gereklidir. | Aynı kaynakla isteği yeniden deneyin.  |
| `temporarily_unavailable` | Sunucu, isteği işlemek için geçici olarak çok meşgul. | İsteği yeniden deneyin. İstemci uygulaması, geçici bir durum nedeniyle yanıtı geciktirildiği kullanıcıya açıklanmayabilir. |

## <a name="use-the-access-token"></a>Erişim belirtecini kullanma

Artık `access_token`başarıyla edindiniz, `Authorization` üst bilgisine dahil ederek Web API 'Lerine yönelik isteklerde belirteç kullanabilirsiniz:

> [!TIP]
> Bu isteği Postman 'da yürütün! (Önce `Authorization` üst bilgisini değiştir) [![bu Isteği Postman 'da çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Erişim belirtecini Yenile

Access_tokens kısa ömürlü ve kaynaklara erişmeye devam etmek için süreleri dolduktan sonra yenilemeniz gerekir. Bunu, `/token` uç noktasına başka bir `POST` isteği göndererek yapabilirsiniz, bu kez `code`yerine `refresh_token` sağlar.  Yenileme belirteçleri, istemcinizin zaten izin aldığı tüm izinler için geçerlidir. bu nedenle, `scope=mail.read` için bir istekte verilen yenileme belirteci, `scope=api://contoso.com/api/UseResource`için yeni bir erişim belirteci istemek üzere kullanılabilir.  

Yenileme belirteçlerinin belirtilen ömürleri yok. Genellikle, yenileme belirteçlerinin yaşam süreleri nispeten uzundur. Ancak, bazı durumlarda belirteçleri yenileme süre sonu, iptal etme veya istenen eylem için yeterli ayrıcalıklara sahip değil. Uygulamanızın [belirteç verme uç noktası tarafından döndürülen hataları](#error-codes-for-token-endpoint-errors) beklemesi ve işlemesi gerekir. 

Yeni erişim belirteçleri elde etmek için kullanıldığında yenileme belirteçleri iptal edilmese de, eski yenileme belirtecini atmayı bekliyorcaksınız. [OAuth 2,0 belirtimi](https://tools.ietf.org/html/rfc6749#section-6) şöyle diyor: "yetkilendirme sunucusu yeni bir yenileme belirteci verebilir, bu durumda istemcinin eski yenileme belirtecini atılması ve yeni yenileme belirteci Ile değiştirmeniz gerekir. Yetkilendirme sunucusu, istemciye yeni bir yenileme belirteci gönderdikten sonra eski yenileme belirtecini iptal edebılır. "  

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fuser.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Bu isteği Postman 'da yürütmeyi deneyin! (`refresh_token`değiştirmeyi unutmayın) [![bu Isteği Postman 'da çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)
> 

| Parametre     |                | Açıklama        |
|---------------|----------------|--------------------|
| `tenant`        | {1&gt;gerekli&lt;1}     | İsteğin yolundaki `{tenant}` değeri, uygulamada kimlerin oturum açmasını denetlemek için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve kiracı tanımlayıcılarıdır. Daha fazla ayrıntı için bkz. [protokol temelleri](active-directory-v2-protocols.md#endpoints).   |
| `client_id`     | {1&gt;gerekli&lt;1}    | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminin uygulamanıza atandığı **uygulama (istemci) kimliği** . |
| `grant_type`    | {1&gt;gerekli&lt;1}    | Yetkilendirme kodu akışının bu bacağı için `refresh_token` olmalıdır. |
| `scope`         | {1&gt;gerekli&lt;1}    | Kapsamların boşlukla ayrılmış listesi. Bu Bata istenen kapsamlar, özgün authorization_code isteği balarında istenen kapsamların bir alt kümesiyle eşit olmalıdır. Bu istekte belirtilen kapsamlar birden çok kaynak sunucusuna yayılsa, Microsoft Identity platform uç noktası ilk kapsamda belirtilen kaynak için bir belirteç döndürür. Kapsamların daha ayrıntılı bir açıklaması için [izinler, onay ve kapsamlar](v2-permissions-and-consent.md)' a bakın. |
| `refresh_token` | {1&gt;gerekli&lt;1}    | Akışın ikinci Bada elde ettiğiniz refresh_token. |
| `client_secret` | Web uygulamaları için gerekli | Uygulamanız için uygulama kayıt portalında oluşturduğunuz uygulama gizli anahtarı. Client_secrets, cihazlarda güvenilir bir şekilde depolanamadığı için yerel bir uygulamada kullanılmamalıdır. Client_secret sunucu tarafında güvenli bir şekilde depolayabilme özelliğine sahip Web uygulamaları ve Web API 'Leri için gereklidir. |

#### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir belirteç yanıtı şöyle görünür:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fuser.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre     | Açıklama         |
|---------------|-------------------------------------------------------------|
| `access_token`  | İstenen erişim belirteci. Uygulama, bir Web API 'SI gibi güvenli kaynak üzerinde kimlik doğrulaması yapmak için bu belirteci kullanabilir. |
| `token_type`    | Belirteç türü değerini gösterir. Azure AD 'nin desteklediği tek tür taşıyıcı |
| `expires_in`    | Erişim belirtecinin geçerli olduğu süre (saniye cinsinden).   |
| `scope`         | Access_token için geçerli olan kapsamlar.    |
| `refresh_token` | Yeni bir OAuth 2,0 yenileme belirteci. Yenileme belirteçlerinizin mümkün olduğunca geçerli kalmasını sağlamak için eski yenileme belirtecini bu yeni alınan yenileme belirteciyle değiştirmelisiniz. <br> **Note:** Yalnızca `offline_access` kapsam isteniyorsa sağlandı.|
| `id_token`      | İşaretsiz bir JSON Web Token (JWT). Uygulama, oturum açan kullanıcı hakkında bilgi istemek için bu belirtecin segmentlerinin kodunu çözebilir. Uygulama değerleri önbelleğe alabilir ve görüntüleyebilir, ancak herhangi bir yetkilendirme veya güvenlik sınırları için bu değere dayanmamalıdır. İd_tokens hakkında daha fazla bilgi için bkz. [`id_token reference`](id-tokens.md). <br> **Note:** Yalnızca `openid` kapsam isteniyorsa sağlandı. |

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
| `error`           | Oluşan hata türlerini sınıflandırmak için kullanılabilen ve hatalara yanıt vermek için kullanılabilen bir hata kodu dizesi. |
| `error_description` | Bir geliştiricinin kimlik doğrulama hatasının kök nedenini belirlemesine yardımcı olabilecek belirli bir hata iletisi.           |
| `error_codes` |Tanılamada yardımcı olabilecek STS 'ye özgü hata kodlarının bir listesi. |
| `timestamp` | Hatanın oluştuğu zaman. |
| `trace_id` | Tanılamada yardımcı olabilecek istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | İsteğe ait, bileşenler genelinde tanılamada yardımcı olabilecek benzersiz bir tanımlayıcı. |

Hata kodlarının ve önerilen istemci eyleminin açıklaması için bkz. [belirteç uç noktası hataları Için hata kodları](#error-codes-for-token-endpoint-errors).
