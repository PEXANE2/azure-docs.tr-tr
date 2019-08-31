---
title: Tarayıcıda daha az cihazda oturum açmak için Microsoft Identity platform 'u kullanın | Mavisi
description: Cihaz kodu vermeyi kullanarak gömülü ve tarayıcı tarafından daha az kimlik doğrulama akışları oluşturun.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fdd99899494e9f7b3c0caa4e83f18803b969db1e
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70192724"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-device-code-flow"></a>Microsoft Identity platform ve OAuth 2,0 cihaz kod akışı

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Microsoft Identity platformu, kullanıcıların akıllı TV, IoT cihazı veya yazıcı gibi giriş kısıtlı cihazlarda oturum açmasına olanak tanıyan [cihaz kodu vermeyi](https://tools.ietf.org/html/draft-ietf-oauth-device-flow-12)destekler.  Bu akışı etkinleştirmek için, cihazda kullanıcının oturum açmasını sağlamak üzere başka bir cihazdaki tarayıcıda bir Web sayfasını ziyaret ettiği bir cihaz vardır.  Kullanıcı oturum açtıktan sonra cihaz, erişim belirteçlerini alabilir ve belirteçleri gerektiği şekilde yenileyebilir.  

> [!IMPORTANT]
> Şu anda, Microsoft Identity platform uç noktası yalnızca Azure AD kiracılarının cihaz akışını destekler, ancak kişisel hesaplara yönelik değildir.  Bu, bir kiracı veya `organizations` uç nokta olarak ayarlanmış bir uç nokta kullanmanız gerektiği anlamına gelir.  Bu destek yakında etkinleştirilecek. 
>
> Bir Azure AD kiracısına davet edilen kişisel hesaplar cihaz akışı iznini yalnızca kiracı bağlamında kullanabilir.
>
> Ek bir not olarak, `verification_uri_complete` yanıt alanı şu anda dahil değildir veya desteklenmez.  Bunun nedeni, bkz. Standart, cihaz kod akışı standardının isteğe `verification_uri_complete` bağlı bir parçası olarak listelenmiş görürsünüz.

> [!NOTE]
> Microsoft Identity platform uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. Microsoft Identity platform uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin.

## <a name="protocol-diagram"></a>Protokol diyagramı

Tüm cihaz kod akışı, sonraki diyagrama benzer şekilde görünür. Bu makalenin ilerleyen kısımlarında bulunan adımların her birini açıklıyoruz.

![Cihaz kod akışı](./media/v2-oauth2-device-code/v2-oauth-device-flow.svg)

## <a name="device-authorization-request"></a>Cihaz yetkilendirme isteği

İstemcinin, kimlik doğrulamasını başlatmak için kullanılan bir cihaz ve Kullanıcı kodu için kimlik doğrulama sunucusunu denetlemesi gerekir. İstemci bu isteği `/devicecode` uç noktadan toplar. Bu istekte, istemci ayrıca kullanıcıdan almaları gereken izinleri de içermelidir. Bu isteğin gönderildiği andan itibaren, kullanıcının oturum açması için yalnızca 15 dakika sürer (için `expires_in`normal değer), bu nedenle yalnızca Kullanıcı oturum açmaya hazırsanız bu isteği yapın.

> [!TIP]
> Bu isteği Postman 'da yürütmeyi deneyin!
> [![Bu isteği Postman 'da çalıştırmayı deneyin](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)

```
// Line breaks are for legibility only.

POST https://login.microsoftonline.com/{tenant}/oauth2/v2.0/devicecode
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
scope=user.read%20openid%20profile

```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli |İzin istemek istediğiniz dizin kiracısı. Bu, GUID veya kolay ad biçiminde olabilir.  |
| `client_id` | Gerekli | [Azure Portal – uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyiminin uygulamanıza atandığı **uygulama (istemci) kimliği** . |
| `scope` | Önerilen | Kullanıcının onay vermesini istediğiniz [kapsamların](v2-permissions-and-consent.md) , boşlukla ayrılmış bir listesi.  |

### <a name="device-authorization-response"></a>Cihaz yetkilendirme yanıtı

Başarılı bir yanıt, kullanıcının oturum açmasını sağlamak için gerekli bilgileri içeren bir JSON nesnesi olacaktır.  

| Parametre | Biçimi | Açıklama |
| ---              | --- | --- |
|`device_code`     | Dize | İstemci ile yetkilendirme sunucusu arasındaki oturumu doğrulamak için kullanılan uzun bir dize. İstemci, yetkilendirme sunucusundan erişim belirtecini istemek için bu parametreyi kullanır. |
|`user_code`       | Dize | Bir ikincil cihazda oturumu tanımlamak için kullanılan kullanıcıya gösterilen kısa bir dize.|
|`verification_uri`| URI | Kullanıcının oturum açmak için uygulamasına `user_code` gitmesi gereken URI. |
|`expires_in`      | int | `device_code` Ve`user_code` süresi dolmadan önce geçmesi gereken saniye sayısı. |
|`interval`        | int | İstemcinin yoklama istekleri arasında beklemesi gereken saniye sayısı. |
| `message`        | Dize | Kullanıcı yönergelerine sahip, insan tarafından okunabilen bir dize. Bu, formun `?mkt=xx-XX`isteğine bir **sorgu parametresi** eklenerek, uygun dil kültür kodu doldurularak yerelleştirilebilecek. |

## <a name="authenticating-the-user"></a>Kullanıcının kimliğini doğrulama

`user_code` Ve`verification_uri`aldıktan sonra istemci bunları kullanıcıya görüntüler ve bunları mobil telefonlarını veya bilgisayar tarayıcısını kullanarak oturum açmasını ister.  Ayrıca, istemci, Kullanıcı `verfication_uri_complete` `user_code` için girilme adımını alacak bir QR kodu veya benzer bir mekanizma kullanarak öğesini görüntüleyebilir.

Kullanıcı, `verification_uri`üzerinde kimlik doğrulaması yaparken, istemci, istenen belirtecin `/token` uç noktasını kullanarak `device_code`yoklamalıdır.

``` 
POST https://login.microsoftonline.com/tenant/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

grant_type: urn:ietf:params:oauth:grant-type:device_code
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8
```

| Parametre | Gerekli | Açıklama|
| -------- | -------- | ---------- |
| `grant_type` | Gerekli | Olmalıdır`urn:ietf:params:oauth:grant-type:device_code`|
| `client_id`  | Gerekli | İlk istekte `client_id` kullanılan ile aynı olmalıdır. |
| `device_code`| Gerekli | Cihaz yetkilendirme isteğinde döndürüldü. `device_code`  |

### <a name="expected-errors"></a>Beklenen hatalar

Cihaz kod akışı bir yoklama protokolüdür, bu nedenle kullanıcının kimlik doğrulaması tamamlanmadan önce hata alması beklenir.  

| Hata | Açıklama | İstemci eylemi |
| ------ | ----------- | -------------|
| `authorization_pending` | Kullanıcı kimlik doğrulamasını tamamlamadı, ancak akışı iptal edilmedi. | İsteği en az `interval` saniye sonra tekrarlayın. |
| `authorization_declined` | Son Kullanıcı Yetkilendirme isteğini reddetti.| Yoklamayı durdurun ve kimliği doğrulanmamış bir duruma dönün.  |
| `bad_verification_code`| Uçnoktaya`/token`gönderilentanınmadı. `device_code` | İstemcinin istekte doğru `device_code` şekilde gönderdiğini doğrulayın. |
| `expired_token` | En az `expires_in` saniye geçti ve kimlik doğrulaması artık bu `device_code`ile mümkün değil. | Yoklamayı durdurun ve kimliği doğrulanmamış bir duruma dönün. |

### <a name="successful-authentication-response"></a>Başarılı kimlik doğrulama yanıtı

Başarılı bir belirteç yanıtı şöyle görünür:

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

| Parametre | Biçimi | Açıklama |
| --------- | ------ | ----------- |
| `token_type` | Dize| Always "taşıyıcı. |
| `scope` | Boşlukla ayrılmış dizeler | Erişim belirteci döndürülürse, bu, erişim belirtecinin geçerli olduğu kapsamları listeler. |
| `expires_in`| int | Dahil edilen erişim belirtecinin geçerli olması için geçmesi gereken saniye sayısı. |
| `access_token`| Donuk dize | İstenen [kapsamlar](v2-permissions-and-consent.md) için verildi.  |
| `id_token`   | JWT | Özgün `scope` parametre`openid` kapsamı içeriyorsa verildi.  |
| `refresh_token` | Donuk dize | Özgün `scope` parametre dahil `offline_access`ise verildi.  |

Yenileme belirtecini, [OAuth kod akışı belgelerinde](v2-oauth2-auth-code-flow.md#refresh-the-access-token)belgelenen akışı kullanarak yeni erişim belirteçleri elde etmek ve belirteçleri yenilemek için kullanabilirsiniz.  
