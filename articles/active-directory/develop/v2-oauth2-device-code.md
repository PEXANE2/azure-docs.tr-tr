---
title: OAuth 2.0 cihaz kodu akışı | Azure
titleSuffix: Microsoft identity platform
description: Tarayıcısı olmayan kullanıcıları oturum açın. Aygıt yetkilendirme hibesini kullanarak gömülü ve tarayıcısız kimlik doğrulama akışları oluşturun.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 2a39dbb3676df5ed916203bdcbbc51d5a0da32a4
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677844"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-device-authorization-grant-flow"></a>Microsoft kimlik platformu ve OAuth 2.0 cihaz yetkilendirme hibe akışı

Microsoft kimlik platformu, kullanıcıların akıllı TV, IoT aygıtı veya yazıcı gibi giriş kısıtlaması olan aygıtlarda oturum açmalarına olanak tanıyan [aygıt yetkilendirme iznini](https://tools.ietf.org/html/rfc8628)destekler.  Bu akışı etkinleştirmek için, kullanıcı oturum açmamak için başka bir cihazdaki tarayıcısındaki bir web sayfasını ziyaret etti.  Kullanıcı giriş yaptıktan sonra, aygıt erişim belirteçleri alabilir ve gerektiğinde belirteçleri yenileyebilir.

Bu makalede, uygulamanızdaki protokole karşı doğrudan programlama nın nasıl yapılacağını açıklanmaktadır.  Mümkün olduğunda, [belirteçleri elde etmek ve güvenli web API'lerini aramak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)yerine desteklenen Microsoft Kimlik Doğrulama Kitaplıklarını (MSAL) kullanmanızı öneririz.  Ayrıca [MSAL kullanan örnek uygulamalara](sample-v2-code.md)da göz atın.

## <a name="protocol-diagram"></a>Protokol diyagramı

Aygıt kodu akışının tamamı bir sonraki diyagrama benzer görünüyor. Adımların her birini bu makalede daha sonra açıklıyoruz.

![Cihaz kodu akışı](./media/v2-oauth2-device-code/v2-oauth-device-flow.svg)

## <a name="device-authorization-request"></a>Cihaz yetkilendirme isteği

İstemci önce kimlik doğrulama yı başlatmak için kullanılan bir aygıt ve kullanıcı kodu için kimlik doğrulama sunucusuna danışmalıdır. İstemci bu isteği `/devicecode` bitiş noktasından toplar. Bu istekte, istemci kullanıcıdan alması gereken izinleri de içermelidir. Bu istek gönderildiği andan itibaren, kullanıcının oturum açmak için `expires_in`yalnızca 15 dakikası vardır (her zamanki değer), bu isteği yalnızca kullanıcı oturum açmaya hazır olduğunu belirttiğinde yapın.

> [!TIP]
> Postacı bu isteği yürütmeyi deneyin!
> [![Postacı'da bu isteği çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
// Line breaks are for legibility only.

POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=user.read%20openid%20profile

```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | /ortak, /tüketiciler veya /kuruluşlar olabilir.  GUID veya kolay ad biçiminde izin istemek istediğiniz dizin kiracısı da olabilir.  |
| `client_id` | Gerekli | Azure portalı - Uygulama [kayıtlarının](https://go.microsoft.com/fwlink/?linkid=2083908) uygulamanıza atandığı **Uygulama (istemci) Kimliği.** |
| `scope` | Önerilen | Kullanıcının onay etmesini istediğiniz [kapsamların](v2-permissions-and-consent.md) alanayrılmış listesi.  |

### <a name="device-authorization-response"></a>Aygıt yetkilendirme yanıtı

Başarılı bir yanıt, kullanıcının oturum açmasına izin vermek için gerekli bilgileri içeren bir JSON nesnesi olacaktır.

| Parametre | Biçimlendir | Açıklama |
| ---              | --- | --- |
|`device_code`     | Dize | İstemci ve yetkilendirme sunucusu arasındaki oturumu doğrulamak için kullanılan uzun bir dize. İstemci, yetkilendirme sunucusundan erişim belirteci istemek için bu parametreyi kullanır. |
|`user_code`       | Dize | İkincil bir aygıttaki oturumu tanımlamak için kullanılan kullanıcıya gösterilen kısa dize.|
|`verification_uri`| URI | Uri kullanıcı oturum `user_code` açabilmek için gitmeli. |
|`expires_in`      | int | Önce saniye sayısı `device_code` ve `user_code` sona erer. |
|`interval`        | int | İstemcinin yoklama istekleri arasında beklemesi gereken saniye sayısı. |
| `message`        | Dize | Kullanıcı için talimatlar içeren insan tarafından okunabilir bir dize. Bu, formun `?mkt=xx-XX`isteğine bir **sorgu parametresi** eklenerek, uygun dil kültürü kodunu doldurarak yerelleştirilebilir. |

> [!NOTE]
> Yanıt `verification_uri_complete` alanı şu anda dahil edilmedi veya desteklenmiyor.  Bunu belirttik, çünkü gördüğünüz [standardı](https://tools.ietf.org/html/rfc8628) `verification_uri_complete` okursanız, aygıt kodu akış standardının isteğe bağlı bir parçası olarak listelenir.

## <a name="authenticating-the-user"></a>Kullanıcının kimliğini doğrulama

Aldıktan sonra `user_code` ve `verification_uri`, istemci kullanıcıya bu görüntüler, kendi cep telefonu veya PC tarayıcısı kullanarak oturum açmalarını emret.

Kullanıcı kişisel bir hesapla (/ortak veya /tüketicilerde) kimlik doğrulaması yaparsa, kimlik doğrulama durumunu cihaza aktarmak için yeniden oturum açmaları istenir.  Ayrıca, verilen izinlerden haberdar olmalarını sağlamak için izin vermeleri de istenecektir.  Bu, kimlik doğrulaması için kullanılan iş veya okul hesapları için geçerli değildir.

Kullanıcı kimlik doğrulaması `verification_uri`yaparken, istemci istenen belirteç `/token` için bitiş noktasını yoklaması `device_code`gerekir.

```
POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8...
```

| Parametre | Gerekli | Açıklama|
| -------- | -------- | ---------- |
| `tenant`  | Gerekli | İlk istekte kullanılan aynı kiracı veya kiracı diğer adı. |
| `grant_type` | Gerekli | Olmalıdır`urn:ietf:params:oauth:grant-type:device_code`|
| `client_id`  | Gerekli | İlk istekte `client_id` kullanılanla eşleşmelidir. |
| `device_code`| Gerekli | Aygıt `device_code` yetkilendirme isteğinde döndürülen.  |

### <a name="expected-errors"></a>Beklenen hatalar

Aygıt kodu akışı bir yoklama protokolüdür, bu nedenle istemcinizin kullanıcı kimlik doğrulaması tamamlanmadan önce hata almayı beklemesi gerekir.

| Hata | Açıklama | İstemci Eylemi |
| ------ | ----------- | -------------|
| `authorization_pending` | Kullanıcı kimlik doğrulamasını tamamlamadı, ancak akışı iptal etmedi. | İsteği en az `interval` saniye sonra yineleyin. |
| `authorization_declined` | Son kullanıcı yetkilendirme isteğini reddetti.| Yoklamayı durdurun ve kimlik doğrulaması olmayan bir duruma geri dön.  |
| `bad_verification_code`| `/token` Bitiş noktasına `device_code` gönderilen ler tanınmadı. | İstemcinin istekte `device_code` doğruyu gönderdiğini doğrulayın. |
| `expired_token` | En `expires_in` az saniye geçti ve kimlik doğrulaması `device_code`artık bu ile mümkün. | Yoklamayı durdurun ve kimlik doğrulamamış bir duruma geri dön. |

### <a name="successful-authentication-response"></a>Başarılı kimlik doğrulama yanıtı

Başarılı bir belirteç yanıtı gibi görünecektir:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parametre | Biçimlendir | Açıklama |
| --------- | ------ | ----------- |
| `token_type` | Dize| Her zaman "Taşıyıcı. |
| `scope` | Boşluk ayrılmış dizeleri | Bir erişim belirteci döndürüldüyse, bu, erişim belirteci için geçerli olan kapsamları listeler. |
| `expires_in`| int | Dahil edilen erişim jetonundan önceki saniye sayısı geçerlidir. |
| `access_token`| Opak dize | İstenen [kapsamlar](v2-permissions-and-consent.md) için verilir.  |
| `id_token`   | Jwt | Özgün `scope` parametre `openid` kapsamı dahil ise verilir.  |
| `refresh_token` | Opak dize | Orijinal `scope` parametre dahil sayılsa `offline_access`verilir.  |

Yeni erişim belirteçleri elde etmek ve [OAuth Code akış belgelerinde](v2-oauth2-auth-code-flow.md#refresh-the-access-token)belgelenen aynı akışı kullanarak belirteçleri yenilemek için yenileme belirteci kullanabilirsiniz.
