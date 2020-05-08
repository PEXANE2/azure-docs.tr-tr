---
title: Microsoft Identity platform KIMLIĞI belirteç başvurusu
description: Azure AD v 1.0 ve Microsoft Identity platform (v 2.0) uç noktaları tarafından yayılan id_tokens nasıl kullanacağınızı öğrenin.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40
ms:custom: fasttrack-edit
ms.openlocfilehash: 0d1a5ee3ae56e8b5c4886308624159853c52b52c
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690174"
---
# <a name="microsoft-identity-platform-id-tokens"></a>Microsoft Identity platform KIMLIĞI belirteçleri

`id_tokens`, bir [OpenID Connect](v2-protocols-oidc.md) akışının parçası olarak istemci uygulamasına gönderilir. Bunlar, bir erişim belirteci yerine ya da üzerinden gönderilebilir ve istemci tarafından kullanıcının kimliğini doğrulamak için kullanılır.

## <a name="using-the-id_token"></a>İd_token kullanma

KIMLIK belirteçleri, bir kullanıcının iddia ettikleri kim olduğunu doğrulamak ve bunlarla ilgili yararlı bilgiler almak için kullanılmalıdır; bir [erişim belirtecinin](access-tokens.md)yerine yetkilendirme için kullanılmamalıdır. Sağladığı talepler, uygulamanızın içindeki UX için, bir veritabanında anahtarlar olarak ve istemci uygulamasına erişim sağlamak için kullanılabilir.  Bir veritabanı için anahtar oluştururken, `idp` Konuk senaryolarını kullanırken kullanılmamalıdır.  Anahtar oluşturma, gerektiğinde yönlendirme `sub` için `tid` kullanılan tek başına (her zaman benzersiz) yapılmalıdır.  Hizmetler `oid` + `sub` + `tid` arasında veri paylaşmanız gerekiyorsa, birden çok hizmetin hepsi aynı `oid`olduğundan çalışacaktır.

## <a name="claims-in-an-id_token"></a>İd_token talepler

`id_tokens`bir Microsoft kimliği [için, bir](https://tools.ietf.org/html/rfc7519)üst bilgi, yük ve imza kısmından oluşan anlamına gelir. Belirtecin orijinalliğini doğrulamak için üst bilgi ve imzayı, yük ise istemciniz tarafından istenen kullanıcı hakkındaki bilgileri içerdiğinde de kullanabilirsiniz. Aksi belirtilmedikçe, burada listelenen tüm talepler v 1.0 ve v 2.0 belirteçlerinde görünür.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

Bu v 1.0 örnek belirtecini [JWT.MS](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q)içinde görüntüleyin.

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

Bu v 2.0 örnek belirtecini [JWT.MS](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTEyMjA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjkxMjIwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw)içinde görüntüleyin.

### <a name="header-claims"></a>Üst bilgi talepleri

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`typ` | Dize-Always "JWT" | Belirtecin JWT olduğunu gösterir.|
|`alg` | Dize | Belirteci imzalamak için kullanılan algoritmayı belirtir. Örnek: "RS256" |
|`kid` | Dize | Bu belirteci imzalamak için kullanılan ortak anahtar için parmak izi. Hem v 1.0 hem de v 2.0 `id_tokens`içinde yayınlanır. |
|`x5t` | Dize | Aynı (kullanımda ve değerde) olarak `kid`. Bununla birlikte, bu, uyumluluk amacıyla yalnızca v 1.0 `id_tokens` ile yayılan eski bir taleptür. |

### <a name="payload-claims"></a>Yük talepleri

Bu liste varsayılan olarak en çok id_tokens olan talepleri gösterir (aksi belirtilmedikçe).  Bununla birlikte, uygulamanız id_token ek talepler istemek için [isteğe bağlı talepler](active-directory-optional-claims.md) kullanabilir.  Bunlar, kullanıcıdan kullanıcının adı `groups` hakkında bilgi talep edebilir.

|İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
|`aud` |  Dize, uygulama KIMLIĞI URI 'SI | Belirtecin amaçlanan alıcısını tanımlar. ' `id_tokens`De hedef kitle, uygulamanızın Azure Portal uygulamanıza atanan uygulama kimliğidir. Uygulamanızın bu değeri doğrulaması ve değer eşleşmezse belirteci reddetmesi gerekir. |
|`iss` |  Dize, STS URI 'SI | Belirteci oluşturan ve döndüren güvenlik belirteci hizmetini (STS) ve kullanıcının kimlik doğrulamasının bulunduğu Azure AD kiracısını tanımlar. Belirteç v 2.0 uç noktası tarafından verildiyse, URI sona ermeyecektir `/v2.0`.  Kullanıcının Microsoft hesabı bir tüketici kullanıcısı olduğunu gösteren GUID `9188040d-6c67-4c5b-b112-36a304b66dad`. Uygulamanız, varsa uygulamada oturum açmak için gereken kiracılar kümesini kısıtlamak için talebin GUID kısmını kullanmalıdır. |
|`iat` |  int, UNIX zaman damgası | "Çıkarılan", bu belirteç için kimlik doğrulamanın ne zaman oluştuğunu gösterir.  |
|`idp`|Dize, genellikle STS URI 'SI | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. Bu değer, Kullanıcı hesabı veren konukiyle aynı kiracıda olmadığı sürece veren talebinin değeriyle aynıdır, örneğin. Talep yoksa, bunun yerine, değerinin `iss` kullanılabileceği anlamına gelir.  Bir kuruluş bağlamında kullanılan kişisel hesaplar için (örneğin, bir Azure AD kiracısına davet edilen kişisel hesap), `idp` talep ' Live.com ' veya Microsoft hesabı kiracıyı `9188040d-6c67-4c5b-b112-36a304b66dad`içeren bir STS URI 'si olabilir. |
|`nbf` |  int, UNIX zaman damgası | "NBF" (before) talebi, JWT 'ın işlenmek üzere kabul edilmeden önce geçen süreyi tanımlar.|
|`exp` |  int, UNIX zaman damgası | "Exp" (sona erme saati) talebi, JWT 'ın işlenmek üzere kabul edilmemelidir.  Bir kaynağın bu süreden önce belirteci reddedebileceğini unutmayın. Örneğin, kimlik doğrulamasında bir değişiklik gerekliyse veya bir belirteç iptali algılanırsa. |
| `c_hash`| Dize |Kod karması, yalnızca KIMLIK belirteci bir OAuth 2,0 yetkilendirme koduyla verildiğinde KIMLIK belirteçlerine dahil edilir. Bir yetkilendirme kodunun gerçekliğini doğrulamak için kullanılabilir. Bu doğrulamayı gerçekleştirme hakkında daha fazla bilgi için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html)bakın. |
|`at_hash`| Dize |Erişim belirteci karması yalnızca KIMLIK belirteci bir OAuth 2,0 erişim belirteciyle verildiğinde KIMLIK belirteçlerine dahil edilir. Bir erişim belirtecinin orijinalliğini doğrulamak için kullanılabilir. Bu doğrulamayı gerçekleştirme hakkında daha fazla bilgi için [OpenID Connect belirtimine](https://openid.net/specs/openid-connect-core-1_0.html)bakın. |
|`aio` | Donuk dize | Azure AD tarafından belirteç yeniden kullanımı için veri kaydetmek üzere kullanılan bir iç talep. Göz ardı edilmelidir.|
|`preferred_username` | Dize | Kullanıcıyı temsil eden birincil Kullanıcı adı. Belirtilen biçim olmadan bir e-posta adresi, telefon numarası veya genel Kullanıcı adı olabilir. Değeri değişebilir ve zaman içinde değişebilir. Değişebilir olduğundan, bu değer yetkilendirme kararları almak için kullanılmamalıdır. `profile` Kapsam bu talebi almak için gereklidir.|
|`email` | Dize | Talep `email` , bir e-posta adresine sahip konuk hesapları için varsayılan olarak mevcuttur.  Uygulamanız, `email` [isteğe bağlı talebi](active-directory-optional-claims.md)kullanarak yönetilen kullanıcılar (kaynakla aynı kiracıya ait olanlar) için e-posta talebi talep edebilir.  V 2.0 uç noktasında, uygulamanız `email` OpenID Connect kapsamını da talep edebilir. talebi almak için hem isteğe bağlı talebi hem de kapsamı istemeniz gerekmez.  E-posta talebi yalnızca kullanıcının profil bilgilerinde adreslenebilir postaları destekler. |
|`name` | Dize | `name` Talep, belirtecin konusunu tanımlayan, insan tarafından okunabilen bir değer sağlar. Değerin benzersiz olması garanti edilmez, değişebilir ve yalnızca görüntüleme amacıyla kullanılmak üzere tasarlanmıştır. `profile` Kapsam bu talebi almak için gereklidir. |
|`nonce`| Dize | Nonce, ıDP 'ye özgün/Yetkilendir isteğine dahil edilen parametreyle eşleşir. Eşleşmiyorsa, uygulamanız belirteci reddetmelidir. |
|`oid` | Dize, GUID | Bu örnekte, bir kullanıcı hesabı olan Microsoft Identity sistemindeki bir nesne için sabit tanımlayıcı. Bu KIMLIK, kullanıcıyı uygulamalar arasında benzersiz şekilde tanımlar. aynı kullanıcı için iki farklı uygulama imzalanırken, `oid` talepteki aynı değer de alınır. Microsoft Graph, bu KIMLIĞI verilen kullanıcı hesabının `id` özelliği olarak döndürür. Birden çok `oid` uygulamanın kullanıcıları ilişkilendirme olanağı sağladığından, bu `profile` talebi almak için kapsamın olması gerekir. Tek bir Kullanıcı birden fazla kiracıda varsa, kullanıcının her kiracıda farklı bir nesne KIMLIĞI içerdiğini ve Kullanıcı aynı kimlik bilgilerine sahip her hesapta oturum açtığı halde farklı hesaplar olarak kabul edileceğini unutmayın. `oid` Talep bir GUID 'dir ve yeniden kullanılamaz. |
|`roles`| Dizeler dizisi | Oturum açan kullanıcıya atanan roller kümesi. |
|`rh` | Donuk dize |Belirteçleri yeniden doğrulamak için Azure tarafından kullanılan bir iç talep. Göz ardı edilmelidir. |
|`sub` | Dize, GUID | Belirtecin, uygulamanın kullanıcısı gibi bilgilerin hangi sorumluya ait olduğunu belirten sorumlu. Bu değer sabittir ve yeniden atanamaz veya tekrar kullanılamaz. Konu, belirli bir uygulama KIMLIĞI için benzersiz bir tanımlayıcıdır. Tek bir Kullanıcı iki farklı istemci kimliği kullanarak iki farklı uygulama üzerinde oturum açarsa, bu uygulamalar konu talebi için iki farklı değer alır. Bu, mimarinize ve gizlilik gereksinimlerinize bağlı olarak istenebilir veya olmayabilir. |
|`tid` | Dize, GUID | Kullanıcının ait olduğu Azure AD kiracısını temsil eden bir GUID. İş ve okul hesapları için GUID, kullanıcının ait olduğu kuruluşun sabit kiracı KIMLIĞIDIR. Kişisel hesaplar için değer `9188040d-6c67-4c5b-b112-36a304b66dad`. `profile` Kapsam bu talebi almak için gereklidir. |
|`unique_name` | Dize | Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değer, herhangi bir zamanda belirli bir noktada benzersizdir, ancak e-postalar ve diğer tanımlayıcılar yeniden kullanılabilir, bu değer diğer hesaplarda yeniden görüntülenebilir ve bu nedenle yalnızca görüntüleme amacıyla kullanılmalıdır. Yalnızca v 1.0 `id_tokens`'da verilir. |
|`uti` | Donuk dize | Belirteçleri yeniden doğrulamak için Azure tarafından kullanılan bir iç talep. Göz ardı edilmelidir. |
|`ver` | Dize, 1,0 ya da 2,0 | İd_token sürümünü gösterir. |


> [!NOTE]
> V1 ve v2 id_token, yukarıdaki örneklerden görüldüğü gibi taşıyabilecekleri bilgi miktarındaki farklılıklar vardır. Sürüm temelde, verildiği yerden Azure AD platformu uç noktasını belirtir. [Azure AD OAuth uygulamasında](https://docs.microsoft.com/azure/active-directory/develop/about-microsoft-identity-platform) yıllarca bir gelişmiştir. Şu anda AzureAD uygulamaları için iki farklı oAuth uç noktası sunuyoruz. V2 olarak sınıflandırılan yeni bitiş noktalarından herhangi birini ya da v1 olarak söyedilen eskileri kullanabilirsiniz. Her ikisine ait OAuth uç noktaları farklı. V2 uç noktası, v1 uç noktasının tüm özelliklerini geçirmeye çalıştığı ve yeni geliştiricilerin v2 uç noktasını kullanmasını önerdiğimiz yeni bir noktadır. 
> - V1: Azure Active Directory uç noktaları:`https://login.microsoftonline.com/common/oauth2/authorize`
> - V2: Microsoft Identity platform uç noktaları:`https://login.microsoftonline.com/common/oauth2/v2.0/authorize`

## <a name="validating-an-id_token"></a>İd_token doğrulama

Bir `id_token` [erişim belirtecini doğrulamaya](access-tokens.md#validating-tokens) yönelik ilk adımla benzer bir şekilde doğrulama, istemciniz doğru veren 'in belirteci geri gönderdiğini ve üzerinde oynanmadığını doğrulayabilmelidir. Her `id_tokens` zaman bir JWT olduğundan, bu belirteçleri doğrulamak için birçok kitaplık bulunur; bunlardan birini kendi başınıza yapmak yerine kullanmanızı öneririz.

Belirteci el ile doğrulamak için, [erişim belirtecini doğrulama](access-tokens.md#validating-tokens)konusunda bulunan adım ayrıntılarına bakın. Belirteçteki imza doğrulandıktan sonra, aşağıdaki talepler id_token doğrulanmalıdır (Bunlar ayrıca belirteç doğrulama kitaplığınız tarafından yapılabilir):

* Zaman damgaları: `iat`, `nbf`, ve `exp` zaman damgaları, uygun şekilde, geçerli zamandan önce veya sonra düşmelidir. 
* Hedef kitle: `aud` talebin, UYGULAMANıZıN uygulama kimliğiyle eşleşmesi gerekir.
* Nonce: yükteki `nonce` talep, ilk istek sırasında/yetkilendirmeli uç noktasına geçirilen nonce parametresiyle eşleşmelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Erişim belirteçleri](access-tokens.md) hakkında bilgi edinin
* [İsteğe bağlı talepler](active-directory-optional-claims.md)kullanarak id_token talepleri özelleştirin.
