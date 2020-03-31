---
title: Microsoft kimlik platformu kimlik belirteç başvurusu | Microsoft Dokümanlar
description: Azure AD v1.0 ve Microsoft kimlik platformu (v2.0) bitiş noktaları tarafından yayılan id_tokens nasıl kullanacağınızı öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms:custom: fasttrack-edit
ms.openlocfilehash: 1efd027edb85cabcfdc2a170771ef19182b5c9f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160959"
---
# <a name="microsoft-identity-platform-id-tokens"></a>Microsoft kimlik platformu kimlik belirteçleri

`id_tokens`[OpenID Connect](v2-protocols-oidc.md) akışının bir parçası olarak istemci uygulamasına gönderilir. Bunlar bir erişim belirteci yerine birlikte veya yerine gönderilebilir ve istemci tarafından kullanıcının kimliğini doğrulamak için kullanılır.

## <a name="using-the-id_token"></a>id_token kullanma

Kimlik Belirteçleri, bir kullanıcının iddia ettiği kişi olduğunu doğrulamak ve onlar hakkında ek yararlı bilgiler almak için kullanılmalıdır - [erişim jetonu](access-tokens.md)yerine yetkilendirme için kullanılmamalıdır. Sağladığı talepler, uygulamanızın içindeki UX için, veritabanında anahtar olarak ve istemci uygulamasına erişim sağlayan kullanılabilir.  Bir veritabanı için anahtar `idp` oluştururken, konuk senaryoları berbat çünkü kullanılmamalıdır.  Anahtarlama `sub` tek başına yapılmalıdır (her zaman `tid` benzersizdir), gerekirse yönlendirme için kullanılır.  Hizmetler arasında veri paylaşmanız `oid` + `sub` + `tid` gerekiyorsa, birden çok hizmet `oid`aynı olduğundan çalışacaktır.

## <a name="claims-in-an-id_token"></a>id_token Talepler

`id_tokens`Bir Microsoft kimliği için [JWTs](https://tools.ietf.org/html/rfc7519)vardır, onlar bir üstbilgi, yük ve imza bölümü oluşur anlamına gelir. Yük istemciniz tarafından talep edilen kullanıcı hakkında bilgi içerirken, belirteç orijinalliğini doğrulamak için üstbilgi ve imzakullanabilirsiniz. Belirtilen durumlar dışında, burada listelenen tüm talepler hem v1.0 hem de v2.0 belirteçlerinde görünür.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

bu v1.0 örnek belirteci [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q)görüntüleyin.

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

bu v2.0 örnek belirteci [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw)görüntüleyin.

### <a name="header-claims"></a>Üstbilgi talepleri

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`typ` | String - her zaman "JWT" | Belirteç bir JWT olduğunu gösterir.|
|`alg` | Dize | Belirteci imzalamak için kullanılan algoritmayı gösterir. Örnek: "RS256" |
|`kid` | Dize | Bu belirteci imzalamak için kullanılan ortak anahtarın parmak izi. Hem v1.0 hem de v2.0'da `id_tokens`yayılan. |
|`x5t` | Dize | Aynı (kullanım ve değer) `kid`olarak. Ancak, bu uyumluluk amacıyla yalnızca v1.0'da `id_tokens` yayılan eski bir taleptir. |

### <a name="payload-claims"></a>Yük talepleri

Bu liste, varsayılan olarak en id_tokens olan talepleri gösterir (not edilendurumlar hariç).  Ancak, uygulamanız id_token ek talepler de talep etmek için [isteğe bağlı talepler](active-directory-optional-claims.md) kullanabilir.  `groups` Bunlar, talepten kullanıcının adı hakkındaki bilgilere kadar değişebilir.

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`aud` |  String, bir Uygulama Kimliği URI | Belirteci amaçlanan alıcı tanımlar. `id_tokens`Hedef kitle, Azure portalında uygulamanız için atanan uygulamanızın Uygulama Kimliğidir. Uygulamanız bu değeri doğrulamalı ve değer eşleşmiyorsa belirteci reddetmelidir. |
|`iss` |  String, bir STS URI | Belirteci oluşturan ve döndüren güvenlik belirteci hizmetini (STS) ve kullanıcının kimliğinin doğrulandığı Azure AD kiracısını tanımlar. Belirteç v2.0 bitiş noktası tarafından verilmişse, `/v2.0`URI .  Kullanıcının bir Microsoft hesabından bir tüketici kullanıcısı `9188040d-6c67-4c5b-b112-36a304b66dad`olduğunu belirten GUID. Uygulamanız, varsa uygulamada oturum açabilecek kiracı kümesini kısıtlamak için talebin GUID bölümünü kullanmalıdır. |
|`iat` |  int, bir UNIX zaman damgası | "Verilen At", bu belirteç için kimlik doğrulamasının ne zaman gerçekleştiğini gösterir.  |
|`idp`|String, genellikle bir STS URI | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. Bu değer, kullanıcı hesabı ihraççıyla aynı kiracıda olmadığı sürece, örneğin, İhraççı talebinin değeriyle aynıdır. Talep yoksa, bunun yerine değerinin `iss` kullanılabileceğini anlamına gelir.  Kuruluş bağlamında kullanılan kişisel hesaplar için (örneğin, Azure AD kiracısına davet `idp` edilen kişisel bir hesap), talep Microsoft hesabı `9188040d-6c67-4c5b-b112-36a304b66dad`kiracısını içeren bir 'live.com' veya BIR STS URI olabilir. |
|`nbf` |  int, bir UNIX zaman damgası | "Nbf" (daha önce değil) iddiası, JWT'nin işleme alınmak üzere kabul edilmemesi gereken zamanı tanımlar.|
|`exp` |  int, bir UNIX zaman damgası | "Exp" (son kullanma süresi) talebi, JWT'nin işleme için kabul edilmemesi gereken son kullanma süresini tanımlar.  Bir kaynağın belirteci bu tarihten önce de reddedebileceğini unutmayın - örneğin, kimlik doğrulamasında bir değişiklik gerekiyorsa veya bir belirteç iptali algılanmışsa. |
| `c_hash`| Dize |Kod karma, kimlik belirteci oAuth 2.0 yetki kodu ile verildiğinde yalnızca kimlik belirteçleri dahil edilir. Yetkilendirme kodunun orijinalliğini doğrulamak için kullanılabilir. Bu doğrulamanın gerçekleştirilmesi yle ilgili ayrıntılar için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html)bakın. |
|`at_hash`| Dize |Erişim belirteç hası, yalnızca kimlik belirteci OAuth 2.0 erişim jetonu yla birlikte verildiğinde kimlik belirteci belirteçlerine dahildir. Bir erişim belirteci orijinalliğini doğrulamak için kullanılabilir. Bu doğrulamanın gerçekleştirilmesi yle ilgili ayrıntılar için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html)bakın. |
|`aio` | Opak Dize | Azure AD tarafından belirteç yeniden kullanımı için veri kaydetmek için kullanılan dahili bir talep. Göz ardı edilmelidir.|
|`preferred_username` | Dize | Kullanıcıyı temsil eden birincil kullanıcı adı. Belirli bir biçim olmadan bir e-posta adresi, telefon numarası veya genel bir kullanıcı adı olabilir. Değeri değişkendir ve zaman içinde değişebilir. Mutable olduğundan, bu değer yetkilendirme kararları vermek için kullanılmamalıdır. Kapsam `profile` bu talebi almak için gereklidir.|
|`email` | Dize | Talep, `email` e-posta adresi olan konuk hesapları için varsayılan olarak mevcuttur.  `email` [Uygulamanız, isteğe bağlı talebi](active-directory-optional-claims.md)kullanarak yönetilen kullanıcılar (kaynakla aynı kiracıdan gelenler) için e-posta talebi isteyebilir.  v2.0 bitiş noktasında, uygulamanız `email` OpenID Connect kapsamını da talep edebilir - talebi almak için hem isteğe bağlı talep hem de kapsamı talep etmeniz gerekmez.  E-posta talebi yalnızca kullanıcının profil bilgilerinden adreslenebilir postaları destekler. |
|`name` | Dize | Talep, `name` belirteci konusunu tanımlayan bir insan tarafından okunabilir bir değer sağlar. Değerbenzersiz olduğu garanti değildir, bu mutable, ve yalnızca görüntüleme amaçlı kullanılmak üzere tasarlanmıştır. Kapsam `profile` bu talebi almak için gereklidir. |
|`nonce`| Dize | Nonce, IDP'ye yapılan orijinal /authorize isteğinde yer alan parametreyle eşleşir. Eşleşmiyorsa, başvurunuz belirteci reddetmelidir. |
|`oid` | String, bir GUID | Microsoft kimlik sistemindeki bir nesnenin değişmez tanımlayıcısı, bu durumda bir kullanıcı hesabı. Bu kimlik, uygulamaları genelinde kullanıcıyı benzersiz olarak tanımlar - aynı kullanıcıda oturum `oid` imzalayan iki farklı uygulama talepte aynı değeri alır. Microsoft Graph bu kimliği belirli `id` bir kullanıcı hesabının özelliği olarak döndürür. Birden `oid` çok uygulamanın kullanıcıları ilişkilendirmesine `profile` izin verdiğinden, bu talebi almak için kapsam gereklidir. Tek bir kullanıcı birden çok kiracıda varsa, kullanıcı her kiracıda farklı bir nesne kimliği içerir - kullanıcı her hesaba aynı kimlik bilgileriyle giriş yapsa bile farklı hesaplar olarak kabul edilir. Talep `oid` bir GUID'dir ve yeniden kullanılamaz. |
|`roles`| Dize dizileri | Oturum açan kullanıcıya atanan roller kümesi. |
|`rh` | Opak Dize |Azure tarafından belirteçleri yeniden geçersiz kılınması için kullanılan dahili bir talep. Göz ardı edilmelidir. |
|`sub` | String, bir GUID | Belirteci hakkında bir uygulamanın kullanıcı gibi bilgileri, iddia ana. Bu değer değişmez dir ve yeniden atanamaz veya yeniden kullanılamaz. Konu çift yönlü bir tanımlayıcıdır - belirli bir uygulama kimliğine özgüdür. Tek bir kullanıcı iki farklı istemci dis'i kullanarak iki farklı uygulamaya imza larsa, bu uygulamalar konu talebi için iki farklı değer alır. Bu, mimari ve gizlilik gereksinimlerinize bağlı olarak isdenedilebilir veya isdenolmayabilir. |
|`tid` | String, bir GUID | Kullanıcının geldiği Azure AD kiracısını temsil eden bir GUID. İş ve okul hesapları için GUID, kullanıcının ait olduğu kuruluşun değişmez kiracı kimliğidir. Kişisel hesaplar için değer. `9188040d-6c67-4c5b-b112-36a304b66dad` Kapsam `profile` bu talebi almak için gereklidir. |
|`unique_name` | Dize | Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değer zaman içinde herhangi bir noktada benzersizdir, ancak e-postalar ve diğer tanımlayıcılar yeniden kullanılabildığından, bu değer diğer hesaplarda yeniden görüntülenebilir ve bu nedenle yalnızca görüntüleme amacıyla kullanılmalıdır. Sadece v1.0 `id_tokens`verilir. |
|`uti` | Opak Dize | Azure tarafından belirteçleri yeniden geçersiz kılınması için kullanılan dahili bir talep. Göz ardı edilmelidir. |
|`ver` | Dize, 1.0 veya 2.0 | id_token sürümünü gösterir. |


> [!NOTE]
> V1 ve v2 id_token yukarıdaki örneklerden görüldüğü gibi taşıyacakları bilgi miktarında farklılıklar vardır. Sürüm, Azure AD platformu bitiş noktasını verildiği yerden belirtir. [Azure AD Oauth uygulaması](https://docs.microsoft.com/azure/active-directory/develop/about-microsoft-identity-platform) yıllar içinde gelişti. Şu anda AzureAD uygulamaları için iki farklı oAuth uç noktamız var. V2 veya v1 olduğu söylenen eski uç noktalardan herhangi birini kullanabilirsiniz. Her ikisi için de Oauth uç noktaları farklıdır. V2 bitiş noktası, v1 uç noktasının tüm özelliklerini geçirmeye çalıştığımız ve yeni geliştiricilere v2 bitiş noktasını kullanmalarını önerdiğimiz yeni noktadır. 
> - V1: Azure Etkin Dizin Bitiş Noktaları:`https://login.microsoftonline.com/common/oauth2/authorize`
> - V2: Microsoft Kimlik Platformu Bitiş Noktaları:`https://login.microsoftonline.com/common/oauth2/v2.0/authorize`

## <a name="validating-an-id_token"></a>Bir id_token doğrulama

Bir girişi `id_token` [doğrulamak, bir erişim belirtecisini doğrulamanın](access-tokens.md#validating-tokens) ilk adımına benzer - istemciniz doğru verenin belirteci geri gönderdiğini ve değiştirilmediğini doğrulamalıdır. Her `id_tokens` zaman bir JWT olduğundan, birçok kütüphane bu belirteçleri doğrulamak için var - biz yerine kendiniz yapıyor bunlardan birini kullanmanızı öneririz.

Belirteci el ile doğrulamak için, [erişim belirteci doğrulama](access-tokens.md#validating-tokens)adım ayrıntılarıbakın. Belirteçteki imza yı doğruladıktan sonra, aşağıdaki taleplerin id_token doğrulanması gerekir (bunlar belirteç doğrulama kitaplığınız tarafından da yapılabilir):

* Zaman damgaları: `iat` `nbf`zaman `exp` damgaları ve zaman damgaları uygun olduğu şekilde geçerli zamandan önce veya sonra düşmelidir. 
* Hedef Kitle: `aud` Talep, uygulamanız için uygulama kimliğiyle eşleşmelidir.
* Nonce: `nonce` yükteki talep, ilk istek sırasında /authorize bitiş noktasına geçirilen nonce parametresi ile eşleşmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* Erişim [belirteçleri](access-tokens.md) hakkında bilgi edinin
* İsteğe bağlı talepleri kullanarak id_token'nizdeki [talepleri özelleştirin.](active-directory-optional-claims.md)
