---
title: Microsoft kimlik platformunda OAuth 2.0 istemci kimlik bilgileri akışı | Azure
description: OAuth 2.0 kimlik doğrulama protokolünün Microsoft kimlik platformu uygulamasını kullanarak web uygulamaları oluşturun.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 6021348160b338bc3a153764968f6c846a644cd1
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535868"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-client-credentials-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 istemci kimlik bilgileri akışı

RFC 6749'da belirtilen, bazen *iki bacaklı OAuth*olarak adlandırılan [OAuth 2.0 istemci kimlik bilgilerini,](https://tools.ietf.org/html/rfc6749#section-4.4) bir uygulamanın kimliğini kullanarak web tarafından barındırılan kaynaklara erişmek için kullanabilirsiniz. Bu hibe türü genellikle, kullanıcıyla hemen etkileşim edilmeden arka planda çalışması gereken sunucudan sunucuya etkileşimler için kullanılır. Bu tür uygulamalar genellikle *daemons* veya *hizmet hesapları*olarak adlandırılır.

Bu makalede, uygulamanızdaki protokole karşı doğrudan programlama nın nasıl yapılacağını açıklanmaktadır. Mümkün olduğunda, [belirteçleri elde etmek ve güvenli web API'lerini aramak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)yerine desteklenen Microsoft Kimlik Doğrulama Kitaplıklarını (MSAL) kullanmanızı öneririz.  Ayrıca [MSAL kullanan örnek uygulamalara](sample-v2-code.md)da göz atın.

OAuth 2.0 istemci kimlik bilgileri hibe akışı, bir web hizmetinin (gizli istemci) başka bir web hizmetini ararken kimlik doğrulaması yapmak yerine, kullanıcı kimliği kullanmak yerine kendi kimlik bilgilerini kullanmasına izin verir. Bu senaryoda, istemci genellikle bir orta katman web hizmeti, bir daemon hizmeti veya bir web sitesidir. Daha yüksek bir güvence düzeyi için, Microsoft kimlik platformu arama hizmetinin bir sertifikayı (paylaşılan bir sır yerine) kimlik bilgisi olarak kullanmasına da izin verir.

> [!NOTE]
> Microsoft kimlik platformu bitiş noktası tüm Azure REKLAM senaryolarını ve özelliklerini desteklemez. Microsoft kimlik platformu bitiş noktasını kullanıp kullanmadığınızı belirlemek için [Microsoft kimlik platformu sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

Daha tipik *üç bacaklı OAuth,* bir istemci uygulaması belirli bir kullanıcı adına bir kaynağa erişmek için izin verilir. İzin, kullanıcıdan uygulamaya, genellikle [onay](v2-permissions-and-consent.md) işlemi sırasında devredilir. Ancak, istemci kimlik bilgileri *(iki ayaklı OAuth)* akışında, izinler doğrudan uygulamanın kendisine verilir. Uygulama bir kaynağa bir belirteç sunduğunda, kaynak uygulamanın kullanıcıya değil, eylem gerçekleştirme yetkisine sahip olduğunu zorlar.

## <a name="protocol-diagram"></a>Protokol diyagramı

Tüm istemci kimlik bilgileri akışı aşağıdaki diyagrama benzer görünüyor. Adımların her birini bu makalede daha sonra açıklıyoruz.

![İstemci kimlik bilgilerini gösteren diyagram](./media/v2-oauth2-client-creds-grant-flow/convergence-scenarios-client-creds.svg)

## <a name="get-direct-authorization"></a>Doğrudan yetkilendirme alın

Bir uygulama genellikle bir kaynağa erişmek için iki şekilde doğrudan yetkilendirme alır:

* [Kaynaktaki erişim denetim listesi (ACL) aracılığıyla](#access-control-lists)
* [Azure AD'de uygulama izni ataması yoluyla](#application-permissions)

Bu iki yöntem Azure AD'de en yaygın olanlardır ve bunları istemci kimlik bilgileri akışını gerçekleştiren istemciler ve kaynaklar için öneririz. Bir kaynak, istemcilerini başka şekillerde yetkilendirmeyi de seçebilir. Her kaynak sunucusu, uygulaması için en mantıklı yöntemi seçebilir.

### <a name="access-control-lists"></a>Erişim denetimi listeleri

Kaynak sağlayıcısı, bildiği ve belirli bir erişim düzeyi verdiği uygulama (istemci) iliklerinin listesini temel alan bir yetkilendirme denetimi uygulayabilir. Kaynak Microsoft kimlik platformu bitiş noktasından bir belirteç aldığında, belirteci çözebilir ve istemcinin uygulama kimliğini `appid` ve `iss` taleplerinden ayıklayabilir. Daha sonra uygulamayı koruduğu bir erişim denetim listesiyle (ACL) karşılaştırır. ACL'nin tanecikliliği ve yöntemi kaynaklar arasında önemli ölçüde değişebilir.

Yaygın kullanım örneği, bir web uygulaması veya bir web API için testleri çalıştırmak için bir ACL kullanmaktır. Web API yalnızca belirli bir istemciye tam izinlerin bir alt kümesi verebilir. API'de uçtan uca testler çalıştırmak için, Microsoft kimlik platformu bitiş noktasından belirteçler alan ve bunları API'ye gönderen bir test istemcisi oluşturun. API daha sonra API'nin tüm işlevselliğine tam erişim için test istemcisinin uygulama kimliğini n için ACL'yi denetler. Bu tür Bir ACL kullanıyorsanız, yalnızca arayanın `appid` değerini doğrulamakla kalmadığınızdan değil, belirteç `iss` değerinin güvenilir olduğunu da doğruladığını unutmayın.

Bu tür yetkilendirme, kişisel Microsoft hesapları olan tüketici kullanıcılarının sahip olduğu verilere erişebilen daemon'lar ve hizmet hesapları için yaygındır. Kuruluşlara ait veriler için, uygulama izinleri aracılığıyla gerekli yetkilendirmeyi almanızı öneririz.

### <a name="application-permissions"></a>Uygulama izinleri

ABM'ler kullanmak yerine, bir dizi uygulama iznini ortaya çıkarmak için **API'leri**kullanabilirsiniz. Uygulama izni bir kuruluşun yöneticisi tarafından bir uygulamaya verilir ve yalnızca bu kuruluş ve çalışanlarıtarafından sahip olunan verilere erişmek için kullanılabilir. Örneğin, Microsoft Graph aşağıdakileri yapmak için çeşitli uygulama izinlerini ortaya çıkarır:

* Tüm posta kutularında posta okuma
* Tüm posta kutularında posta okuma ve yazma
* Herhangi bir kullanıcı olarak posta gönderme
* Dizin verilerini oku

Uygulama izinleri hakkında daha fazla bilgi için [Microsoft Graph'a](https://developer.microsoft.com/graph)gidin.

Uygulamanızda uygulama izinlerini kullanmak için sonraki bölümlerde tartışılan adımları izleyin.


> [!NOTE]
> Bir uygulama olarak kimlik doğrulaması yaparken, kullanıcının aksine, "temsilciizinler" (kullanıcı tarafından verilen kapsamlar) kullanamazsınız.  Uygulama için bir yönetici tarafından verilen "roller" olarak da bilinen "uygulama izinleri"ni kullanmanız gerekir (veya web API tarafından ön yetkilendirme yoluyla).


#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Uygulama kayıt portalındaki izinleri isteme

1. Yeni [Uygulama kayıtları (Önizleme) deneyimi](quickstart-register-app.md)yle bir uygulama kaydolun ve oluşturun.
2. Uygulama kayıtları (Önizleme) deneyiminde uygulamanıza gidin. **Sertifikalar & sırlar** bölümüne gidin ve yeni bir **istemci sırrı**ekleyin, çünkü belirteç istemek için en az bir istemci sırrına ihtiyacınız olacak.
3. **API izinleri** bölümünü bulun ve ardından uygulamanızın gerektirdiği **uygulama izinlerini** ekleyin.
4. Uygulama kaydını **kaydedin.**

#### <a name="recommended-sign-the-user-into-your-app"></a>Önerilen: Kullanıcıyı uygulamanızda oturum

Genellikle, uygulama izinlerini kullanan bir uygulama oluşturduğunuzda, uygulama, yöneticinin uygulamanın izinlerini onayladığı bir sayfa veya görünüm gerektirir. Bu sayfa, uygulamanın oturum açma akışının, uygulamaayarlarının bir parçası veya özel bir "bağlantı" akışı olabilir. Çoğu durumda, uygulamanın bu "bağlantı" görünümünü yalnızca bir kullanıcı bir iş veya okul Microsoft hesabıyla oturum açtındıktan sonra göstermesi mantıklıdır.

Kullanıcıyı uygulamanızda oturum alarsanız, kullanıcıdan uygulama izinlerini onaylamasını istemeden önce kullanıcının ait olduğu kuruluşu tanımlayabilirsiniz. Kesinlikle gerekli olmasa da, kullanıcılarınız için daha sezgisel bir deneyim oluşturmanıza yardımcı olabilir. Kullanıcıyı oturum alabilmek için [Microsoft kimlik platformu protokol eğitimlerimizi](active-directory-v2-protocols.md)uygulayın.

#### <a name="request-the-permissions-from-a-directory-admin"></a>İzinleri bir dizin yöneticisinden isteme

Kuruluşun yöneticisinden izin istemeye hazır olduğunuzda, kullanıcıyı Microsoft kimlik platformu yönetici onayı *bitiş noktasına*yönlendirebilirsiniz.

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin! (En iyi sonuçlar için kendi uygulama kimliğinizi kullanın - öğretici uygulama yararlı izinler istemeyenaz.) [Postacı'da bu isteği çalıştırmayı deneyin ![](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser.
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | İzin istemek istediğiniz dizin kiracısı. Bu GUID veya dostu ad biçiminde olabilir. Kullanıcının hangi kiracıya ait olduğunu bilmiyorsanız ve herhangi bir kiracıyla oturum `common`açmalarına izin vermek istiyorsanız, 'yi kullanın. |
| `client_id` | Gerekli | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `redirect_uri` | Gerekli | UYGULAMANIZIN işlemesi için yanıtın gönderilmesini istediğiniz uri'yi yeniden yönlendirin. Portalda kaydettiğiniz yönlendirme URL'lerinden biriyle tam olarak eşleşmesi gerekir, ancak URL'nin kodlanmış olması gerekir ve ek yol segmentleri olabilir. |
| `state` | Önerilen | Belirteç yanıtında döndürülen istekte yer alan bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için kullanılır(örneğin, üzerinde oldukları sayfa veya görünüm). |

Bu noktada, Azure AD, isteği tamamlamak için yalnızca bir kiracı yöneticinin oturum açabileceğini zorlar. Yöneticiden uygulama kayıt portalında uygulamanız için talep ettiğiniz tüm doğrudan uygulama izinlerini onaylaması istenir.

##### <a name="successful-response"></a>Başarılı yanıt

Yönetici uygulamanızın izinlerini onaylarsa, başarılı yanıt aşağıdaki gibi görünür:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametre | Açıklama |
| --- | --- |
| `tenant` | Başvurunuza istediği izinleri GUID biçiminde veren dizin kiracısı. |
| `state` | Belirteç yanıtında da döndürülen isteğe dahil edilen bir değer. İstediğiniz herhangi bir içerik dizisi olabilir. Durum, kimlik doğrulama isteği oluşmadan önce kullanıcının durumu yla ilgili bilgileri uygulamada kodlamak için kullanılır(örneğin, üzerinde oldukları sayfa veya görünüm). |
| `admin_consent` | **True'ya**ayarlayın. |

##### <a name="error-response"></a>Hata yanıtı

Yönetici uygulamanızın izinlerini onaylamazsa, başarısız yanıt aşağıdaki gibi görünür:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Hata türlerini sınıflandırmak için kullanabileceğiniz ve hatalara tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| `error_description` | Bir hatanın temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |

Uygulama sağlama bitiş noktasından başarılı bir yanıt aldıktan sonra, uygulamanız istediği doğrudan uygulama izinlerini kazanmıştır. Artık istediğiniz kaynak için bir belirteç isteyebilirsiniz.

## <a name="get-a-token"></a>Bir belirteç alın

Başvurunuz için gerekli yetkilendirmeyi aldıktan sonra, API'ler için erişim belirteçleri edinmeye devam edin. İstemci kimlik bilgileri hibesini kullanarak bir belirteç `/token` almak için, Microsoft kimlik platformu bitiş noktasına bir POST isteği gönderin:

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin! (En iyi sonuçlar için kendi uygulama kimliğinizi kullanın - öğretici uygulama yararlı izinler istemeyenaz.) [Postacı'da bu isteği çalıştırmayı deneyin ![](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: Paylaşılan bir sırla giriş belirteç isteği

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

```
// Replace {tenant} with your tenant!
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token'
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Uygulamanın guid veya etki alanı adı biçiminde çalışmaya planladığı dizin kiracı. |
| `client_id` | Gerekli | Uygulamanız için atanan uygulama kimliği. Bu bilgileri uygulamanızı kaydettiğiniz portalda bulabilirsiniz. |
| `scope` | Gerekli | Bu istekteki `scope` parametre için geçirilen değer, sonekile `.default` yapıştırılmış istediğiniz kaynağın kaynak tanımlayıcısı (uygulama kimliği URI) olmalıdır. Microsoft Graph örneğinde, değer `https://graph.microsoft.com/.default`. <br/>Bu değer, Microsoft kimlik platformu bitiş noktasına, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri arasında, bitiş noktasının kullanmak istediğiniz kaynakla ilişkili olanlar için bir belirteç vermesi gerektiğini söyler. `/.default` Kapsam hakkında daha fazla bilgi edinmek için [onay belgelerine](v2-permissions-and-consent.md#the-default-scope)bakın. |
| `client_secret` | Gerekli | Uygulama kayıt portalında uygulamanız için oluşturduğunuz istemci sırrı. İstemci sırrı gönderilmeden önce URL kodlanmış olmalıdır. |
| `grant_type` | Gerekli | Ayarlanmış `client_credentials`olmalı. |

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Sertifikayla giriş isteği

```
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Uygulamanın guid veya etki alanı adı biçiminde çalışmaya planladığı dizin kiracı. |
| `client_id` | Gerekli |Uygulamanız için atanan uygulama (istemci) kimliği. |
| `scope` | Gerekli | Bu istekteki `scope` parametre için geçirilen değer, sonekile `.default` yapıştırılmış istediğiniz kaynağın kaynak tanımlayıcısı (uygulama kimliği URI) olmalıdır. Microsoft Graph örneğinde, değer `https://graph.microsoft.com/.default`. <br/>Bu değer, Microsoft kimlik platformu bitiş noktasını, uygulamanız için yapılandırdığınız tüm doğrudan uygulama izinleri arasında kullanmak istediğiniz kaynakla ilişkili olanlar için bir belirteç vermesi gerektiğini bildirir. `/.default` Kapsam hakkında daha fazla bilgi edinmek için [onay belgelerine](v2-permissions-and-consent.md#the-default-scope)bakın. |
| `client_assertion_type` | Gerekli | `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`Değer' in ' olarak ayarlanabilmesi gerekir. |
| `client_assertion` | Gerekli | Uygulamanız için kimlik bilgileri olarak kaydettiğiniz sertifikayı oluşturmanız ve imzalamanız gereken bir iddia (JSON web belirteci). Sertifikanızı ve belgebiçiminizi nasıl kaydedebilirsiniz öğrenmek için [sertifika kimlik bilgilerini](active-directory-certificate-credentials.md) okuyun.|
| `grant_type` | Gerekli | Ayarlanmış `client_credentials`olmalı. |

Client_secret parametrenin iki parametreyle değiştirilmesi dışında, parametrenin paylaşılan gizli istek durumunda ki parametrelerle hemen hemen aynı olduğuna dikkat edin: client_assertion_type ve client_assertion.

### <a name="successful-response"></a>Başarılı yanıt

Başarılı bir yanıt şöyle görünür:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametre | Açıklama |
| --- | --- |
| `access_token` | İstenen erişim jetonu. Uygulama, web API gibi güvenli kaynağa kimlik doğrulamak için bu belirteci kullanabilir. |
| `token_type` | Belirteç türü değerini gösterir. Microsoft kimlik platformunun desteklediği `bearer`tek tür. |
| `expires_in` | Erişim belirtecinin geçerli olduğu süre (saniye cinsinden). |

### <a name="error-response"></a>Hata yanıtı

Bir hata yanıtı şuna benzer:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametre | Açıklama |
| --- | --- |
| `error` | Oluşan hata türlerini sınıflandırmak ve hatalara tepki vermek için kullanabileceğiniz bir hata kodu dizesi. |
| `error_description` | Kimlik doğrulama hatasının temel nedenini belirlemenize yardımcı olabilecek belirli bir hata iletisi. |
| `error_codes` | Tanılama ile yardımcı olabilecek STS'ye özgü hata kodlarının listesi. |
| `timestamp` | Hatanın oluştuğu saat. |
| `trace_id` | Tanılama ile yardımcı olmak için istek için benzersiz bir tanımlayıcı. |
| `correlation_id` | Bileşenler arasında tanılama ile yardımcı olmak için istek için benzersiz bir tanımlayıcı. |

## <a name="use-a-token"></a>Belirteç kullanma

Artık bir belirteç edindiğinize göre, kaynağa istekte bulunmak için belirteci kullanın. Belirteç süresi dolduğunda, yeni `/token` bir erişim belirteci elde etmek için isteği bitiş noktasına kadar yineleyin.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try the following command! (Replace the token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...." 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-samples-and-other-documentation"></a>Kod örnekleri ve diğer belgeler

Microsoft Kimlik Doğrulama Kitaplığı'ndan [istemci kimlik bilgilerine genel bakış belgelerini](https://aka.ms/msal-net-client-credentials) okuyun

| Örnek | Platform |Açıklama |
|--------|----------|------------|
|[aktif-dizin-dotnetcore-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) | .NET Core 2.1 Konsol | Bir kullanıcı adına değil, uygulamanın kimliğini kullanarak Microsoft Graph sorgulayan bir kiracı nın kullanıcılarını görüntüleyen basit bir .NET Core uygulaması. Örnek, kimlik doğrulaması için sertifikaları kullanarak varyasyonu da göstermektedir. |
|[aktif-dizin-dotnet-daemon-v2](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)|ASP.NET MVC | Microsoft Graph'taki verileri kullanıcı adına değil, uygulamanın kimliğini kullanarak senkronize eden bir web uygulaması. |
