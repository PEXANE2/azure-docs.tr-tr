---
title: Microsoft kimlik platformu erişim belirteçleri başvuru | Azure
description: Azure AD v1.0 ve Microsoft kimlik platformu (v2.0) uç noktaları tarafından yayılan erişim belirteçleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 3/27/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 417829389a4b3a6bb55dcff9bfe59c2bc8693ca0
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80383217"
---
# <a name="microsoft-identity-platform-access-tokens"></a>Microsoft kimlik platformu erişim belirteçleri

Erişim belirteçleri, istemcilerin Azure tarafından korunan API'leri güvenli bir şekilde aramasını sağlar. Microsoft kimlik platformu erişim [belirteçleri JWTs](https://tools.ietf.org/html/rfc7519), Base64 Kodlanmış JSON nesneleri Azure tarafından imzalanmıştır. Belirteç içeriği yalnızca kaynak için tasarlanmıştır gibi istemciler, opak dizeleri olarak erişim belirteçleri ele almalıdır. Doğrulama ve hata ayıklama amacıyla, geliştiriciler [jwt.ms](https://jwt.ms)gibi bir site kullanarak JWTs deşifre edebilirsiniz. Müşteriniz çeşitli protokoller kullanarak v1.0 bitiş noktasından veya v2.0 bitiş noktasından bir erişim belirteci alabilir.

Müşteriniz bir erişim jetonu istediğinde, Azure AD uygulamanızın tüketimine ilişkin erişim jetonu yla ilgili bazı meta verileri de döndürür. Bu bilgiler, erişim belirtecinin son kullanma süresini ve geçerli olduğu kapsamları içerir. Bu veriler, uygulamanızın erişim belirteci girişini ayrıştırmak zorunda kalmadan akıllı erişim belirteçleri önbelleğe alma masına olanak tanır.

Uygulamanız, istemcilerin erişim isteyebileceği bir kaynaksa (web API) erişim talebinde bulununca, erişim belirteçleri kullanıcı, istemci, veren, izinler ve daha fazlası gibi kimlik doğrulama ve yetkilendirmede kullanılmak üzere yararlı bilgiler sağlar.

Bir kaynağın bir erişim jetonundaki talepleri nasıl doğrulayabildiğini ve kullanabileceğini öğrenmek için aşağıdaki bölümlere bakın.

> [!IMPORTANT]
> Erişim belirteçleri belirteçleri, belirteçteki kapsamların sahibi olan uygulama anlamına gelen *belirteç kitlesine* göre oluşturulur.  Bu şekilde uygulama `accessTokenAcceptedVersion` [bildiriminde](reference-app-manifest.md#manifest-reference) bir `2` kaynak ayarı v1.0 bitiş noktasını arayan bir istemcinin v2.0 erişim jetonunu almasına olanak tanır.  Benzer şekilde, bu nedenle istemciniz için erişim belirteç [isteğe bağlı talepleri](active-directory-optional-claims.md) değiştirme kaynağına `user.read`ait bir belirteç için istendiğinde alınan erişim belirteci değiştirmez.
> Aynı nedenle, istemci başvurunuzu kişisel bir hesapla (hotmail.com veya outlook.com gibi) sınarken, istemciniz tarafından alınan erişim belirteciopak bir dize olduğunu görebilirsiniz. Bunun nedeni, erişilen kaynağın şifrelenmiş ve istemci tarafından anlaşılamayan eski MSA (Microsoft hesabı) biletlerini istemesidir.

## <a name="sample-tokens"></a>Örnek belirteçler

v1.0 ve v2.0 belirteçleri benzer görünür ve aynı taleplerin çoğunu içerir. Her birinin bir örneği burada verilmiştir.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0.D3H6pMUtQnoJAGq6AHd
```

bu v1.0 belirteci [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0.D3H6pMUtQnoJAGq6AHd)görüntüleyin.

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt
```

[bu](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt)v2.0 belirteci JWT.ms görüntüleyin.

## <a name="claims-in-access-tokens"></a>Erişim belirteçlerinde talepler

JWT'ler üç parçaya ayrılır:

* **Üstbilgi** - Belirteci türü ve nasıl imzalandığı hakkında bilgi de dahil olmak üzere [belirteci nasıl doğrulanması](#validating-tokens) hakkında bilgi sağlar.
* **Yük** - Hizmetinizi aramaya çalışan kullanıcı veya uygulama yla ilgili tüm önemli verileri içerir.
* **İmza** - Belirteci doğrulamak için kullanılan hammaddedir.

Her parça bir döneme`.`ayrılır ( ) ve ayrı ayrı Base64 kodlanır.

Talepler yalnızca onu doldurmak için bir değer varsa bulunur. Bu nedenle, uygulamanız mevcut bir hak talebine bağımlı olmamalıdır. Örnekler `pwd_exp` arasında (her kiracıparolaların süresinin `family_name` dolmasını gerektirmez) veya (istemci kimlik bilgisi ([v1.0](../azuread-dev/v1-oauth2-client-creds-grant-flow.md), [v2.0](v2-oauth2-client-creds-grant-flow.md)) akışları adları olmayan uygulamalar adına dır). Erişim belirteç doğrulaması için kullanılan talepler her zaman geçerli olacaktır.

> [!NOTE]
> Bazı talepler, Yeniden kullanım durumunda Azure AD'nin jetonları güvenli hale almasına yardımcı olmak için kullanılır. Bunlar, "Opak" olarak tanımda kamu tüketimi için değil olarak işaretlenmiştir. Bu talepler bir belirteçte görünebilir veya görünmeyebilir ve yenileri önceden haber verilmeden eklenebilir.

### <a name="header-claims"></a>Üstbilgi talepleri

|İste | Biçimlendir | Açıklama |
|--------|--------|-------------|
| `typ` | String - her zaman "JWT" | Belirteç bir JWT olduğunu gösterir.|
| `nonce` | Dize | Belirteç tekrar saldırılarına karşı korumak için kullanılan benzersiz bir tanımlayıcı. Kaynağınız tekrarlara karşı korumak için bu değeri kaydedebilir. |
| `alg` | Dize | Belirteci imzalamak için kullanılan algoritmayı gösterir, örneğin, "RS256" |
| `kid` | Dize | Bu belirteci imzalamak için kullanılan ortak anahtarın parmak izini belirtir. Hem v1.0 hem de v2.0 erişim jetonlarında yayılır. |
| `x5t` | Dize | İşlevler aynı (kullanım ve `kid`değer) olarak. `x5t`uyumluluk amacıyla yalnızca v1.0 erişim jetonlarında yayılan eski bir taleptir. |

### <a name="payload-claims"></a>Yük talepleri

| İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
| `aud` | String, bir Uygulama Kimliği URI | Belirteci amaçlanan alıcı tanımlar. Kimlik belirteçlerinde, hedef kitle uygulamanızın Azure portalında uygulamanıza atanan Uygulama Kimliğidir. Uygulamanız bu değeri doğrulamalı ve değer eşleşmiyorsa belirteci reddetmelidir. |
| `iss` | String, bir STS URI | Belirteci oluşturan ve döndüren güvenlik belirteci hizmetini (STS) ve kullanıcının kimliğinin doğrulandığı Azure AD kiracısını tanımlar. Verilen belirteç v2.0 belirteci ise `ver` (iddiaya bakın), `/v2.0`URI . Kullanıcının bir Microsoft hesabından bir tüketici kullanıcısı `9188040d-6c67-4c5b-b112-36a304b66dad`olduğunu belirten GUID. Uygulamanız, varsa uygulamada oturum açabilecek kiracı kümesini kısıtlamak için talebin GUID bölümünü kullanmalıdır. |
|`idp`| String, genellikle bir STS URI | Belirtecin öznesinin kimliğini doğrulayan kimlik sağlayıcısını kaydeder. Bu değer, kullanıcı hesabı ihraççıyla aynı kiracıda olmadığı sürece, örneğin, İhraççı talebinin değeriyle aynıdır. Talep yoksa, bunun yerine değerinin `iss` kullanılabileceğini anlamına gelir.  Kuruluş bağlamında kullanılan kişisel hesaplar için (örneğin, Azure AD kiracısına davet `idp` edilen kişisel bir hesap), talep Microsoft hesabı `9188040d-6c67-4c5b-b112-36a304b66dad`kiracısını içeren bir 'live.com' veya BIR STS URI olabilir. |
| `iat` | int, bir UNIX zaman damgası | "Verilen At", bu belirteç için kimlik doğrulamasının ne zaman gerçekleştiğini gösterir. |
| `nbf` | int, bir UNIX zaman damgası | "Nbf" (daha önce değil) iddiası, JWT'nin işleme alınmak üzere kabul edilmemesi gereken zamanı tanımlar. |
| `exp` | int, bir UNIX zaman damgası | "Exp" (son kullanma süresi) talebi, JWT'nin işleme için kabul edilmemesi gereken son kullanma süresini tanımlar. Bir kaynağın, kimlik doğrulamasında bir değişiklik yapılması veya belirteç iptali algılanması gibi belirteci bu tarihten önce de reddedebileceğini unutmayın. |
| `aio` | Opak Dize | Azure AD tarafından belirteç yeniden kullanımı için veri kaydetmek için kullanılan dahili bir talep. Kaynaklar bu talebi kullanmamalıdır. |
| `acr` | Dize, "0" veya "1" | Sadece v1.0 belirteçleri mevcut. "Kimlik doğrulama bağlamı sınıfı" iddiası. "0" değeri, son kullanıcı kimlik doğrulamasının ISO/IEC 29115 gereksinimlerini karşılamadığını gösterir. |
| `amr` | JSON dizeleri dizisi | Sadece v1.0 belirteçleri mevcut. Belirteci öznesinin nasıl doğrulandığını tanımlar. Daha fazla bilgi için [amr talep bölümüne](#the-amr-claim) bakın. |
| `appid` | String, bir GUID | Sadece v1.0 belirteçleri mevcut. Belirteci kullanarak istemcinin uygulama kimliği. Uygulama kendisi veya bir kullanıcı adına hareket edebilir. Uygulama kimliği genellikle bir uygulama nesnesini temsil eder, ancak Azure AD'de bir hizmet ana nesnesini de temsil edebilir. |
| `appidacr` | "0", "1", "2" | Sadece v1.0 belirteçleri mevcut. İstemcinin kimliğinin nasıl doğrulandığını gösterir. Genel istemci için değer "0"dır. İstemci kimliği ve istemci sırrı kullanılırsa, değer "1" olur. Kimlik doğrulaması için bir istemci sertifikası kullanıldıysa, değer "2" olur. |
| `azp` | String, bir GUID | Sadece v2.0 belirteçleri mevcut, `appid`bir yedek . Belirteci kullanarak istemcinin uygulama kimliği. Uygulama kendisi veya bir kullanıcı adına hareket edebilir. Uygulama kimliği genellikle bir uygulama nesnesini temsil eder, ancak Azure AD'de bir hizmet ana nesnesini de temsil edebilir. |
| `azpacr` | "0", "1", "2" | Sadece v2.0 belirteçleri mevcut, `appidacr`bir yedek . İstemcinin kimliğinin nasıl doğrulandığını gösterir. Genel istemci için değer "0"dır. İstemci kimliği ve istemci sırrı kullanılırsa, değer "1" olur. Kimlik doğrulaması için bir istemci sertifikası kullanıldıysa, değer "2" olur. |
| `preferred_username` | Dize | Kullanıcıyı temsil eden birincil kullanıcı adı. Belirli bir biçim olmadan bir e-posta adresi, telefon numarası veya genel bir kullanıcı adı olabilir. Değeri değişkendir ve zaman içinde değişebilir. Mutable olduğundan, bu değer yetkilendirme kararları vermek için kullanılmamalıdır.  Ancak kullanıcı adı ipuçları için kullanılabilir. Bu `profile` talebi almak için kapsam gereklidir. |
| `name` | Dize | Belirteci konusunu tanımlayan bir insan tarafından okunabilir bir değer sağlar. Değerbenzersiz olduğu garanti edilmez, bu mutable, ve yalnızca görüntüleme amaçlı kullanılmak üzere tasarlanmıştır. Bu `profile` talebi almak için kapsam gereklidir. |
| `scp` | String, kapsamların ayrılmış bir boşluk listesi | Müşteri başvurusunun onay istediği (ve aldığı) başvurunuzun maruz kaçtığı kapsamlar kümesi. Uygulamanız, bu kapsamların uygulamanız tarafından açığa çıkarılan geçerli kapsamlar olduğunu doğrulamalı ve bu kapsamların değerine göre yetkilendirme kararları vermelidir. Yalnızca kullanıcı [belirteçleri](#user-and-application-tokens)için dahildir. |
| `roles` | Dizeleri dizi, izinler listesi | Başvurunuz tarafından ortaya çıkarılan ve isteyen uygulamaya veya kullanıcıya arama izni verildiği açıkolan izinler kümesi. [Uygulama belirteçleri](#user-and-application-tokens)için, bu kullanıcı kapsamları yerine istemci kimlik bilgisi akışı[(v1.0](../azuread-dev/v1-oauth2-client-creds-grant-flow.md), [v2.0](v2-oauth2-client-creds-grant-flow.md)) sırasında kullanılır.  [Kullanıcı belirteçleri](#user-and-application-tokens) için bu, kullanıcının hedef uygulamada atandığı rollerle doldurulur. |
| `wids` | [RoleTemplateID](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#role-template-ids) GUIDs Dizisi | [Yönetici rolleri sayfasında](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#role-template-ids)bulunan roller bölümünden, bu kullanıcıya atanan kiracı çapındaki rolleri gösterir.  Bu talep, uygulama `groupMembershipClaims` [bildiriminin](reference-app-manifest.md)özelliği üzerinden, uygulama başına bazda yapılandırılır.  "Tümü" veya "DirectoryRole" olarak ayarlanması gereklidir.  Belirteç uzunluğu endişeleri nedeniyle örtük akış yoluyla elde edilen belirteçlerde mevcut olmayabilir. |
| `groups` | GUIDs JSON dizi | Öznenin grup üyeliklerini temsil eden nesne iT'ler sağlar. Bu değerler benzersizdir (Bkz. Nesne Kimliği) ve bir kaynağa erişmek için yetkilendirme zorlama gibi erişimi yönetmek için güvenle kullanılabilir. Talep edilen gruplar, uygulama `groupMembershipClaims` [bildiriminin](reference-app-manifest.md)özelliği üzerinden uygulama başına yapılandırılır. Null değeri tüm grupları hariç tutar, "SecurityGroup" değeri yalnızca Active Directory Security Group üyeliklerini içerir ve "Tümü" değeri hem Güvenlik Grupları'nı hem de Office 365 Dağıtım Listelerini içerir. <br><br>Örtülü `hasgroups` hibe ile `groups` iddiayı kullanma hakkında ayrıntılar için aşağıdaki iddiaya bakın. <br>Diğer akışlar için, kullanıcının içinde bulunduğu grup sayısı bir sınırı aşarsa (SAML için 150, JWT için 200), kullanıcı için grupların listesini içeren Microsoft Graph bitiş noktasını gösteren talep kaynaklarına bir fazlalık talebi eklenir. |
| `hasgroups` | Boole | Varsa, her `true`zaman, kullanıcıyı gösteren en az bir gruptadır. Tam grupların URI `groups` parçasını URL uzunluk sınırlarının (şu anda 6 veya daha fazla grup) ötesine taşıyacağını iddia ederse, örtülü hibe akışlarında JWT'ler için talep yerine kullanılır. İstemcinin kullanıcı gruplarını belirlemek için Microsoft Graph API'sini kullanması gerektiğini gösterir (`https://graph.microsoft.com/v1.0/users/{userID}/getMemberObjects`). |
| `groups:src1` | JSON nesnesi | Uzunluk sınırlı olmayan belirteç istekleri `hasgroups` için (yukarıya bakın) ancak belirteç için yine de çok büyük olan kullanıcı için tam gruplar listesine bir bağlantı eklenecektir. Dağıtılmış bir talep olarak JWTs için, `groups` iddia yerine yeni bir iddia olarak SAML için. <br><br>**Örnek JWT Değeri**: <br> `"groups":"src1"` <br> `"_claim_sources`: `"src1" : { "endpoint" : "https://graph.microsoft.com/v1.0/users/{userID}/getMemberObjects" }` |
| `sub` | String, bir GUID | Belirteci hakkında bir uygulamanın kullanıcı gibi bilgileri, iddia ana. Bu değer değişmez dir ve yeniden atanamaz veya yeniden kullanılamaz. Belirteç bir kaynağa erişmek için kullanıldığında gibi yetkilendirme denetimlerini güvenli bir şekilde gerçekleştirmek için kullanılabilir ve veritabanı tablolarında anahtar olarak kullanılabilir. Konu, Azure AD'nin verdiği belirteçlerde her zaman mevcut olduğundan, bu değeri genel amaçlı bir yetkilendirme sisteminde kullanmanızı öneririz. Konu, ancak, bir çift yönlü tanımlayıcı - belirli bir uygulama kimliği için benzersizdir. Bu nedenle, tek bir kullanıcı iki farklı istemci dis'i kullanarak iki farklı uygulamaya imza larsa, bu uygulamalar konu talebi için iki farklı değer alır. Bu, mimari ve gizlilik gereksinimlerinize bağlı olarak istilebilir veya istilemeyebilir. Ayrıca talep `oid` (kiracı içindeki uygulamalar arasında aynı kalır). |
| `oid` | String, bir GUID | Microsoft kimlik platformundaki bir nesnenin değişmez tanımlayıcısı, bu durumda bir kullanıcı hesabı. Yetkilendirme denetimlerini güvenli bir şekilde ve veritabanı tablolarında anahtar olarak gerçekleştirmek için de kullanılabilir. Bu kimlik, uygulamaları genelinde kullanıcıyı benzersiz olarak tanımlar - aynı kullanıcıda oturum `oid` imzalayan iki farklı uygulama talepte aynı değeri alır. Bu `oid` nedenle, Microsoft Graph gibi Microsoft çevrimiçi hizmetlerine sorgu yaparken kullanılabilir. Microsoft Graph bu kimliği belirli `id` bir [kullanıcı hesabının](/graph/api/resources/user)özelliği olarak döndürür. Birden `oid` çok uygulamanın kullanıcıları ilişkilendirmesine `profile` izin verdiğinden, bu talebi almak için kapsam gereklidir. Tek bir kullanıcı birden çok kiracıda varsa, kullanıcı her kiracıda farklı bir nesne kimliği içerir - kullanıcı her hesaba aynı kimlik bilgileriyle giriş yapsa bile farklı hesaplar olarak kabul edilir. |
| `tid` | String, bir GUID | Kullanıcının geldiği Azure AD kiracısını temsil eder. İş ve okul hesapları için GUID, kullanıcının ait olduğu kuruluşun değişmez kiracı kimliğidir. Kişisel hesaplar için değer. `9188040d-6c67-4c5b-b112-36a304b66dad` Bu `profile` talebi almak için kapsam gereklidir. |
| `unique_name` | Dize | Sadece v1.0 belirteçleri mevcut. Belirtecin konusunu tanımlayan ve okunabilir bir değer sunar. Bu değerin kiracı içinde benzersiz olduğu garanti edilmez ve yalnızca görüntüleme amacıyla kullanılmalıdır. |
| `uti` | Opak Dize | Azure tarafından belirteçleri yeniden geçersiz kılınması için kullanılan dahili bir talep. Kaynaklar bu talebi kullanmamalıdır. |
| `rh` | Opak Dize | Azure tarafından belirteçleri yeniden geçersiz kılınması için kullanılan dahili bir talep. Kaynaklar bu talebi kullanmamalıdır. |
| `ver` | Dize, `1.0` ya da`2.0` | Erişim belirteci sürümünü gösterir. |

**Grup fazlası talebi**

Azure AD, belirteç boyutunun HTTP üstbilgi boyutu sınırlarını aşmadığından emin olmak için, grupların iddia ettiği nesne Kimlikleri sayısını sınırlar. Bir kullanıcı büyük sınırdan (SAML belirteçleri için 150, JWT belirteçleri için 200) daha fazla gruba üyeyse, Azure AD belirteçteki grupları iddia etmez. Bunun yerine, kullanıcının grup üyeliğini almak için Microsoft Graph API'yi sorgulayan uygulamaya işareteden bir fazlalık talebi içerir.

```JSON
{
  ...
  "_claim_names": {
   "groups": "src1"
    },
    {
  "_claim_sources": {
    "src1": {
        "endpoint":"[Url to get this user's group membership from]"
        }
       }
     }
  ...
 }
 ```

Aşırı yaşın senaryolarını test etmeye yardımcı olmak için [App Creation Scripts](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-2-Groups/AppCreationScripts) klasöründe sağlanan kullanım ışıklayıcı `BulkCreateGroups.ps1`

#### <a name="v10-basic-claims"></a>v1.0 temel iddiaları

Aşağıdaki talepler varsa v1.0 belirteçlerine dahil edilir, ancak varsayılan olarak v2.0 belirteçlerine dahil edilmeyecektir. v2.0 kullanıyorsanız ve bu taleplerden birine ihtiyacınız varsa, [bunları isteğe bağlı talepleri](active-directory-optional-claims.md)kullanarak isteyin.

| İste | Biçimlendir | Açıklama |
|-----|--------|-------------|
| `ipaddr`| Dize | Kullanıcının kimlik doğrulaması IP adresi. |
| `onprem_sid`| String, [SID formatında](https://docs.microsoft.com/windows/desktop/SecAuthZ/sid-components) | Kullanıcının şirket içi kimlik doğrulaması olduğu durumlarda, bu talep KENDI SID'sini sağlar. Eski uygulamalarda yetkilendirme için kullanabilirsiniz. `onprem_sid`|
| `pwd_exp`| int, bir UNIX zaman damgası | Kullanıcının parolasının süresinin ne zaman dolduğunu gösterir. |
| `pwd_url`| Dize | Kullanıcıların parolalarını sıfırlamak için gönderilebildiği bir URL. |
| `in_corp`| boole | İstemci şirket ağından oturum açıyorsa sinyaller. Değilse, talep dahil değildir. |
| `nickname`| Dize | Kullanıcı için ad veya soyaddan ayrı ek bir ad.|
| `family_name` | Dize | Kullanıcı nesnesi üzerinde tanımlandığı şekilde kullanıcının soyadını, soyadını veya soyadını sağlar. |
| `given_name` | Dize | Kullanıcı nesnesi üzerinde ayarlanan kullanıcının ilk veya verilen adını sağlar. |
| `upn` | Dize | Kullanıcının kullanıcı adı. Bir telefon numarası, e-posta adresi veya biçimlendirilmemiş dize olabilir. Yalnızca görüntü amaçlı ve yeniden kimlik doğrulama senaryolarında kullanıcı adı ipuçları sağlamak için kullanılmalıdır. |

#### <a name="the-amr-claim"></a>İddia `amr`

Microsoft kimlikleri, uygulamanızla alakalı olabilecek farklı şekillerde kimlik doğrulayabilir. Talep, `amr` `["mfa", "rsa", "pwd"]`hem parola hem de Kimlik Doğrulayıcı uygulaması kullanan bir kimlik doğrulaması için birden çok öğe içerebilen bir dizidir.

| Değer | Açıklama |
|-----|-------------|
| `pwd` | Parola kimlik doğrulaması, bir kullanıcının Microsoft parolası veya uygulamanın istemci sırrı. |
| `rsa` | Kimlik doğrulama, örneğin [Microsoft Authenticator uygulamasında](https://aka.ms/AA2kvvu)bir RSA anahtarının kanıtına dayanıyordu. Buna, kimlik doğrulamanın x509 sertifikasına sahip bir hizmete sahip kendi imzalı bir JWT tarafından yapılıp yapılmaması da sürüyor. |
| `otp` | E-posta veya kısa mesaj kullanarak tek seferlik parola. |
| `fed` | Federe kimlik doğrulama iddiası (JWT veya SAML gibi) kullanıldı. |
| `wia` | Windows Tümleşik Kimlik Doğrulama |
| `mfa` | Çok faktörlü kimlik doğrulama kullanıldı. Bu durum mevcut olduğunda, diğer kimlik doğrulama yöntemleri de eklenecektir. |
| `ngcmfa` | Eşdeğer `mfa`, bazı gelişmiş kimlik bilgileri türlerinin sağlanması için kullanılır. |
| `wiaormfa`| Kullanıcı kimlik doğrulaması için Windows veya MFA kimlik bilgilerini kullandı. |
| `none` | Kimlik doğrulama yapılmadı. |

## <a name="validating-tokens"></a>Belirteçleri doğrulama

Bir id_token veya access_token doğrulamak için uygulamanızın hem belirteci imzasını hem de talepleri doğrulamalıdır. Erişim belirteçlerini doğrulamak için uygulamanızın vereni, hedef kitleyi ve imza belirteçlerini de doğrulamalıdır. Bunların OpenID bulma belgesindeki değerlere göre doğrulanması gerekir. Örneğin, belgenin kiracıdan bağımsız sürümü [https://login.microsoftonline.com/common/.well-known/openid-configuration](https://login.microsoftonline.com/common/.well-known/openid-configuration).

Azure AD ara yazılımı, erişim belirteçlerini doğrulamak için yerleşik özelliklere sahiptir ve seçtiğiniz dilde bir tane bulmak için [örneklerimize](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) göz atabilirsiniz.

Belirteç doğrulamasının nasıl işleyeceğini gösteren kitaplıklar ve kod örnekleri salıyoruz. Aşağıdaki bilgiler altta yatan süreci anlamak isteyenler için sağlanmaktadır. JWT doğrulama için birkaç üçüncü taraf açık kaynak kütüphaneleri de vardır - orada hemen hemen her platform ve dil için en az bir seçenek vardır. Azure AD kimlik doğrulama kitaplıkları ve kod örnekleri hakkında daha fazla bilgi için [v1.0 kimlik doğrulama kitaplıkları](../azuread-dev/active-directory-authentication-libraries.md) ve [v2.0 kimlik doğrulama kitaplıkları'na](reference-v2-libraries.md)bakın.

### <a name="validating-the-signature"></a>İmzayı doğrulama

JWT, `.` karakterle ayrılan üç bölüm içerir. İlk segment **üstbilgi**olarak bilinir, **gövde**olarak ikinci , ve **üçüncü imza**olarak . İmza kesimi, uygulamanız tarafından güvenilebilmek için belirteciorijinalliğini doğrulamak için kullanılabilir.

Azure AD tarafından verilen belirteçler RS256 gibi endüstri standardı asimetrik şifreleme algoritmaları kullanılarak imzalanır. JWT üstbilgi, belirteci imzalamak için kullanılan anahtar ve şifreleme yöntemi hakkında bilgi içerir:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk",
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

Talep `alg` belirteci imzalamak için kullanılan algoritmayı `kid` gösterirken, talep belirteci doğrulamak için kullanılan belirli ortak anahtarı gösterir.

Belirli bir zamanda, Azure AD belirli bir ortak anahtar çiftlerinden herhangi birini kullanarak bir id_token imzalayabilir. Azure AD olası anahtar kümesini periyodik olarak döndürür, bu nedenle uygulamanız bu anahtar değişikliklerini otomatik olarak işlemek için yazılmalıdır. Azure AD tarafından kullanılan ortak anahtarlara yapılan güncelleştirmeleri denetlemek için makul bir sıklık her 24 saatte bir dir.

Aşağıdaki adreste bulunan [OpenID Connect meta veri belgesini](v2-protocols-oidc.md#fetch-the-openid-connect-metadata-document) kullanarak imzayı doğrulamak için gereken imzalama anahtar verilerini edinebilirsiniz:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Bir tarayıcıda bu [URL](https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration) deneyin!

Bu meta veri belgesi:

* OpenID Connect kimlik doğrulaması yapmak için gereken çeşitli uç noktaların konumu gibi birkaç yararlı bilgi parçası içeren bir JSON nesnesidir.
* Belirteçleri imzalamak için kullanılan ortak anahtarlar kümesinin konumunu veren bir `jwks_uri`, içerir. JSON Web Key (JWK) bulunan `jwks_uri` zaman içinde belirli bir anda kullanılan tüm genel anahtar bilgileri içerir.  JWK formatı [RFC 7517'de](https://tools.ietf.org/html/rfc7517)açıklanmıştır.  Uygulamanız, bu `kid` belgede belirli bir belirteç oturuma hangi ortak anahtarın kullanıldığını seçmek için JWT üstbilgisindeki talebi kullanabilir. Daha sonra doğru ortak anahtar ve belirtilen algoritmayı kullanarak imza doğrulama yapabilirsiniz.

> [!NOTE]
> v1.0 bitiş noktası hem `x5t` `kid` talepleri hem de talepleri döndürür, v2.0 bitiş noktası ise yalnızca `kid` taleple yanıt verir. İleriye dönük olarak, `kid` belirtecinizi doğrulamak için talebi kullanmanızı öneririz.

İmza doğrulama yapmak bu belgenin kapsamı dışındadır - gerekirse bunu yapmanıza yardımcı olmak için birçok açık kaynak kitaplığı vardır.  Ancak, Microsoft Identity platformunda standartlara bir belirteç imzalama uzantısı vardır - özel imzalama anahtarları.

Uygulamanızda [talep eşleme](active-directory-claims-mapping.md) özelliğini kullanmanın sonucu olarak özel imzalama anahtarları `appid` varsa, uygulamanızın doğrulama için `jwks_uri` kullanılması gereken imzalama anahtar bilgilerine işaret almak için uygulama kimliğini içeren bir sorgu parametresi eklemeniz gerekir. Örneğin: `https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration?appid=6731de76-14a6-49ae-97bc-6eba6914391e` bir `jwks_uri` `https://login.microsoftonline.com/{tenant}/discovery/keys?appid=6731de76-14a6-49ae-97bc-6eba6914391e`içerir.

### <a name="claims-based-authorization"></a>Taleplere dayalı yetkilendirme

Uygulamanızın iş mantığı bu adımı belirleyecek, bazı ortak yetkilendirme yöntemleri aşağıda konur.

* Mevcut `scp` tüm `roles` kapsamların API'niz tarafından açığa çıkarılanlarla eşleşip eşleşmediğini doğrulamak için veya talepte bulunun ve istemcinin istenen eylemi yapmasına izin verin.
* Arama istemcisinin talebi kullanarak API'nizi `appid` aramasına izin verildiğinden emin olun.
* Arama istemcisinin kimlik doğrulama durumunu `appidacr` kullanarak doğrulayın - ortak istemcilerin API'nizi aramasına izin verilmiyorsa bu 0 olmamalıdır.
* Belirteci yeniden `nonce` oynatılmıyor doğrulamak için geçmiş taleplerin listesine karşı kontrol edin.
* API'nizi `tid` aramasına izin verilen kiracıyla eşleşin.
* Kullanıcının `acr` MFA gerçekleştirdiğini doğrulamak için talebi kullanın. Bu [Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)kullanılarak uygulanmalıdır.
* Erişim jetonundaki `roles` veya `groups` talepleri istediyseniz, kullanıcının bu eylemi yapmasına izin verilen grupta olduğunu doğrulayın.
  * Örtük akış kullanılarak alınan belirteçler için, belirtecisığmayacak kadar büyük olduğundan, büyük olasılıkla bu veriler için [Microsoft Graph'ı](https://developer.microsoft.com/graph/) sorgulamanız gerekir.

## <a name="user-and-application-tokens"></a>Kullanıcı ve uygulama belirteçleri

Uygulamanız bir kullanıcı adına (olağan akış) veya doğrudan bir uygulamadan (istemci kimlik bilgisi akışı[(v1.0](../azuread-dev/v1-oauth2-client-creds-grant-flow.md), [v2.0)](v2-oauth2-client-creds-grant-flow.md)yoluyla belirteçler alabilir. Bu uygulama belirteçleri, bu aramanın bir uygulamadan geldiğini ve kullanıcının bu çağrıyı desteklediğini gösterir. Bu belirteçleri büyük ölçüde aynı, bazı farklılıklar ile işlenir:

* Yalnızca uygulama belirteçleri bir `scp` hak talebinde bulunmaz `roles` ve bunun yerine bir hak talebinde bulunabilir. Uygulama izni (temsilci izinlerinin aksine) kaydedilecek. Temsilci ve uygulama izinleri hakkında daha fazla bilgi için izin ve onay[(v1.0](../azuread-dev/v1-permissions-consent.md), [v2.0)](v2-permissions-and-consent.md)bakın.
* İnsana özgü birçok iddia, örneğin `name` `upn`eksik olacaktır.
* `sub` Ve `oid` iddialar aynı olacaktır.

## <a name="token-revocation"></a>Belirteç iptali

Yenileme belirteçleri, farklı nedenlerle herhangi bir zamanda geçersiz kılınabilir veya iptal edilebilir. Bunlar iki ana kategoriye ayrılır: zaman zaman ları ve iptaller.

### <a name="token-timeouts"></a>Belirteç zaman ları

[Belirteç ömrü yapılandırması](active-directory-configurable-token-lifetimes.md)kullanılarak, yenileme belirteçlerinin kullanım ömrü değiştirilebilir.  Bazı belirteçlerin kullanılmadan gitmesi normaldir ve (örn. kullanıcı uygulamayı 3 ay boyunca açmaz) ve bu nedenle süresi nin dolması beklenir.  Uygulamalar, oturum açma sunucusunun yaşı nedeniyle yenilenme belirteci reddettiği senaryolarla karşılaşır. 

* MaxInactiveTime: Yenileme belirteci MaxInactiveTime tarafından dikte edilen süre içinde kullanılmamışsa, Tokeni Yenile artık geçerli olmayacaktır.
* MaxSessionAge: MaxAgeSessionMultiFactor veya MaxAgeSessionSingleFactor varsayılanları dışında bir şeye ayarlanmışsa (Ta ki iptal edilene kadar), MaxAgeSession* süresinde geçen süre sonunda yeniden kimlik doğrulaması gerekir.
* Örnekler:
  * Kiracının beş günlük Bir MaxInactiveTime'ı vardır ve kullanıcı bir hafta lığına tatile çıkmıştır ve bu nedenle Azure AD kullanıcıdan 7 gündür yeni bir belirteç isteği görmedi. Kullanıcı bir sonraki kez yeni bir belirteç istediğinde, Yenile Belirteci'nin iptal edildiğini ve kimlik bilgilerini yeniden girmeleri gerektiğini göreceksiniz.
  * Hassas bir uygulama bir gün maxAgeSessionSingleFactor vardır. Bir kullanıcı Pazartesi günü ve Salı günü (25 saat geçtikten sonra) oturum açarsa, yeniden kimlik doğrulaması gerekir.

### <a name="revocation"></a>Iptal

Yenileme belirteçleri, kimlik bilgilerindeki bir değişiklik veya kullanım veya yönetici eylemi nedeniyle sunucu tarafından iptal edilebilir.  Yenileme belirteçleri iki sınıfa ayrılır - gizli istemcilere verilenler (en sağ sütun) ve kamu istemcilerine verilenler (diğer tüm sütunlar).   

|   | Parola tabanlı çerez | Parola tabanlı belirteç | Parola tabanlı olmayan çerez | Parola tabanlı olmayan belirteç | Gizli istemci belirteci |
|---|-----------------------|----------------------|---------------------------|--------------------------|---------------------------|
| Parolanın süresi doluyor | Hayatta kalır | Hayatta kalır | Hayatta kalır | Hayatta kalır | Hayatta kalır |
| Kullanıcı tarafından değiştirilen parola | İptal Edildi | İptal Edildi | Hayatta kalır | Hayatta kalır | Hayatta kalır |
| Kullanıcı SSPR yok | İptal Edildi | İptal Edildi | Hayatta kalır | Hayatta kalır | Hayatta kalır |
| Admin parolayı sıfırlar | İptal Edildi | İptal Edildi | Hayatta kalır | Hayatta kalır | Hayatta kalır |
| Kullanıcı [PowerShell üzerinden](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) yenileme belirteçlerini iptal eder | İptal Edildi | İptal Edildi | İptal Edildi | İptal Edildi | İptal Edildi |
| Yönetici [PowerShell üzerinden](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) bir kullanıcı için tüm yenileme belirteçleri iptal eder | İptal Edildi | İptal Edildi |İptal Edildi | İptal Edildi | İptal Edildi |
| Web'de tek oturum açma ([v1.0](../azuread-dev/v1-protocols-openid-connect-code.md#single-sign-out), [v2.0](v2-protocols-oidc.md#single-sign-out) ) | İptal Edildi | Hayatta kalır | İptal Edildi | Hayatta kalır | Hayatta kalır |

> [!NOTE]
> "Parola tabanlı olmayan" giriş, kullanıcının parola yı almak için yazmadığı bir giriştir. Örneğin, yüzünüzü Windows Hello, bir FIDO2 tuşu veya PIN ile kullanmak.
>
> Windows 10'daki Birincil Yenileme Belirteçleri (PRT), kimlik bilgilerine göre ayrılmıştır. Örneğin, Windows Hello ve parolanın kendi İİT'leri birbirinden yalıtılmış olarak vardır. Bir kullanıcı Hello kimlik bilgisi (PIN veya biyometri) ile kaydolduğunda ve parolayı değiştirdiğinde, daha önce elde edilen parola tabanlı PRT iptal edilir. Parolayla geri oturum açma eski PRT'yi geçersiz klar ve yeni bir parola ister.
>
> Yeni bir erişim belirteci ve yenileme belirteci getirmek için kullanıldığında belirteçleri geçersiz kılınmaz veya iptal edilmez.  Ancak, uygulamanız kullanıldığı anda eskisini atmalı ve yeni belirteç yeni bir son kullanma süresine sahip olduğundan, yenisiyle değiştirmelidir. 

## <a name="next-steps"></a>Sonraki adımlar

* Azure [ `id_tokens` AD'de](id-tokens.md)hakkında bilgi edinin.
* İzin ve izin hakkında bilgi edinin ( [v1.0](../azuread-dev/v1-permissions-consent.md), [v2.0](v2-permissions-and-consent.md)).
