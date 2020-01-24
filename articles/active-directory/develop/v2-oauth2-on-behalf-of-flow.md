---
title: Microsoft Identity platform &, OAuth 2.0 adına akış) | Mavisi
description: Bu makalede, Service to-on-the OAuth 2.0 kullanarak hizmet kimlik doğrulamasına hizmet uygulamak için HTTP iletilerinin nasıl kullanılacağı açıklanır.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 1/3/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 92e4376108de02b912c05459411adfacf926c448
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76700473"
---
# <a name="microsoft-identity-platform-and-oauth-20-on-behalf-of-flow"></a>Microsoft Identity platform ve OAuth 2,0-adına akış

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

OAuth 2,0 on-of-the Flow (OBO), bir uygulamanın bir hizmet/Web API 'sini çağırdığı, yani başka bir hizmet/Web API çağrısı yapması gereken kullanım durumunu görür. Amaç, temsilci kullanıcı kimliğini ve izinleri istek zinciri aracılığıyla yaymanız önerilir. Orta katman hizmetin, aşağı akış hizmetine kimliği doğrulanmış istekleri yapması için Kullanıcı adına Microsoft Identity platformundan bir erişim belirtecinin güvenli hale getirme ihtiyacı vardır.

Bu makalede, uygulamanızdaki protokolde doğrudan programlanın nasıl yapılacağı açıklanır.  Mümkün olduğunda, [belirteçleri edinmek ve güvenli Web API 'lerini çağırmak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)Için desteklenen Microsoft kimlik doğrulama KITAPLıKLARıNı (msal) kullanmanızı öneririz.  Ayrıca [, msal kullanan örnek uygulamalara](sample-v2-code.md)göz atın.

> [!NOTE]
>
> - Microsoft Identity platform uç noktası tüm senaryoları ve özellikleri desteklemez. Microsoft Identity platform uç noktasını kullanmanız gerekip gerekmediğini öğrenmek için [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md)hakkında bilgi edinin. 
> - Mayıs 2018 itibariyle, bazı örtük akış türetilmiş `id_token` OBO Flow için kullanılamaz. Tek sayfalı uygulamalar (maça 'Lar), OBO akışlarını gerçekleştirmek için orta katmanlı gizli bir istemciye **erişim** belirteci iletmelidir. Hangi istemcilerin OBO çağrıları gerçekleştirebileceği hakkında daha fazla bilgi için bkz. [sınırlamalar](#client-limitations).

## <a name="protocol-diagram"></a>Protokol diyagramı

Kullanıcının, [OAuth 2,0 yetkilendirme kodu verme akışı](v2-oauth2-auth-code-flow.md) veya başka bir oturum akışı kullanarak bir uygulamada kimliğinin doğrulandığını varsayın. Bu noktada, uygulamanın, orta katman Web API 'sine (API A) erişim için kullanıcının taleplerini ve onayını içeren *API a* (belirteç a) için bir erişim belirteci vardır. Şimdi API A 'nın, aşağı akış Web API 'sine (API B) kimliği doğrulanmış bir istek yapması gerekir.

Aşağıdaki adımlar, OBO akışını oluşturur ve aşağıdaki diyagramın yardımıyla açıklanır.

![Şirket adına OAuth 2.0 akışını gösterir](./media/v2-oauth2-on-behalf-of-flow/protocols-oauth-on-behalf-of-flow.png)

1. İstemci uygulaması, a belirteci ile API a 'ya bir istek yapar (bir API A `aud` talebi ile).
1. API A, Microsoft Identity platform belirteci verme uç noktasında kimlik doğrular ve API B 'ye erişmek için bir belirteç ister.
1. Microsoft Identity platform belirteci verme uç noktası, API A 'nın kimlik bilgilerini ve A belirtecini doğrular ve API B 'nin (belirteç B) erişim belirtecini API A 'ya yayınlar.
1. Belirteç B, API B 'ye yönelik isteğin yetkilendirme üstbilgisindeki API A tarafından ayarlanır.
1. Güvenli kaynaktaki veriler API B tarafından API A 'ya ve istemciye buradan döndürülür.

> [!NOTE]
> Bu senaryoda, orta katman hizmeti kullanıcının aşağı akış API 'sine erişim iznini almak için Kullanıcı etkileşimi yoktur. Bu nedenle, aşağı akış API 'sine erişim izni verme seçeneği, kimlik doğrulama sırasında izin adımının bir parçası olarak önde sunulur. Uygulamanızı için nasıl ayarlayacağınızı öğrenmek için bkz. [Orta katmanlı uygulama için izin](#gaining-consent-for-the-middle-tier-application)alma.

## <a name="service-to-service-access-token-request"></a>Hizmetten hizmete erişim belirteci isteği

Erişim belirteci istemek için, kiracıya özgü Microsoft Identity platform belirteci uç noktasında aşağıdaki parametrelerle bir HTTP GÖNDERISI yapın.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

İstemci uygulamanın, paylaşılan bir gizli dizi ya da sertifikayla güvenli hale getirilme şekline bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: paylaşılan gizli dizi ile belirteç isteğine erişin

Paylaşılan bir gizli dizi kullanılırken hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| `grant_type` | Gereklidir | Belirteç isteği türü. JWT kullanan bir istek için, değerin `urn:ietf:params:oauth:grant-type:jwt-bearer`olması gerekir. |
| `client_id` | Gereklidir | [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `client_secret` | Gereklidir | Azure portal Uygulama kayıtları sayfasında uygulamanız için oluşturduğunuz istemci gizli anahtarı. |
| `assertion` | Gereklidir | İstekte kullanılan belirtecin değeri.  Bu belirteç, bu OBO isteğini (`client-id` alanı tarafından belirtilen uygulama) yapan uygulamanın hedef kitlesi olmalıdır. |
| `scope` | Gereklidir | Belirteç isteği için bir alan ayrılmış kapsam listesi. Daha fazla bilgi için bkz. [kapsamlar](v2-permissions-and-consent.md). |
| `requested_token_use` | Gereklidir | İsteğin nasıl işleneceğini belirtir. OBO akışında, değerin `on_behalf_of`olarak ayarlanması gerekir. |

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, https://graph.microsoft.com Web API 'SI için `user.read` kapsamlı bir erişim belirteci ve yenileme belirteci ister.

```
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE0OTM5MjA5MTYsIm5iZiI6MTQ5MzkyMDkxNiwiZXhwIjoxNDkzOTI0ODE2LCJhaW8iOiJBU1FBMi84REFBQUFnZm8vNk9CR0NaaFV2NjJ6MFFYSEZKR0VVYUIwRUlIV3NhcGducndMMnVrPSIsIm5hbWUiOiJOYXZ5YSBDYW51bWFsbGEiLCJvaWQiOiJkNWU5NzljNy0zZDJkLTQyYWYtOGYzMC03MjdkZDRjMmQzODMiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuYWNhbnVtYUBtaWNyb3NvZnQuY29tIiwic3ViIjoiZ1Q5a1FMN2hXRUpUUGg1OWJlX1l5dVZNRDFOTEdiREJFWFRhbEQzU3FZYyIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjN5U3F4UHJweUVPd0ZsTWFFMU1PQUEiLCJ2ZXIiOiIyLjAifQ.TPPJSvpNCSCyUeIiKQoLMixN1-M-Y5U0QxtxVkpepjyoWNG0i49YFAJC6ADdCs5nJXr6f-ozIRuaiPzy29yRUOdSz_8KqG42luCyC1c951HyeDgqUJSz91Ku150D9kP5B9-2R-jgCerD_VVuxXUdkuPFEl3VEADC_1qkGBiIg0AyLLbz7DTMp5DvmbC09DhrQQiouHQGFSk2TPmksqHm3-b3RgeNM1rJmpLThis2ZWBEIPx662pjxL6NJDmV08cPVIcGX4KkFo54Z3rfwiYg4YssiUc4w-w3NJUBQhnzfTl4_Mtq2d7cVlul9uDzras091vFy32tWkrpa970UvdVfQ
&scope=https://graph.microsoft.com/user.read+offline_access
&requested_token_use=on_behalf_of
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: bir sertifikayla erişim belirteci isteği

Bir sertifikaya sahip hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| `grant_type` | Gereklidir | Belirteç isteğinin türü. JWT kullanan bir istek için, değerin `urn:ietf:params:oauth:grant-type:jwt-bearer`olması gerekir. |
| `client_id` | Gereklidir |  [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `client_assertion_type` | Gereklidir | Değerin `urn:ietf:params:oauth:client-assertion-type:jwt-bearer`olması gerekir. |
| `client_assertion` | Gereklidir | Uygulamanız için kimlik bilgileri olarak kaydettiğiniz sertifikayı oluşturmanız ve oturum açmanız için gereken bir onaylama (JSON Web belirteci). Sertifikanızı ve onaylama biçiminizi nasıl kaydedeceğinizi öğrenmek için bkz. [sertifika kimlik bilgileri](active-directory-certificate-credentials.md). |
| `assertion` | Gereklidir | İstekte kullanılan belirtecin değeri. |
| `requested_token_use` | Gereklidir | İsteğin nasıl işleneceğini belirtir. OBO akışında, değerin `on_behalf_of`olarak ayarlanması gerekir. |
| `scope` | Gereklidir | Belirteç isteğine yönelik kapsamların boşlukla ayrılmış listesi. Daha fazla bilgi için bkz. [kapsamlar](v2-permissions-and-consent.md).|

Parametrelerin, `client_secret` parametresi iki parametre ile değiştirilmeleri dışında, paylaşılan gizliliğe göre istek durumuyla neredeyse aynı olduğuna dikkat edin: bir `client_assertion_type` ve `client_assertion`.

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, bir sertifikayla https://graph.microsoft.com Web API 'SI için `user.read` kapsamlı bir erişim belirteci ister.

```
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read+offline_access
```

## <a name="service-to-service-access-token-response"></a>Hizmetten hizmete erişim belirteci yanıtı

Başarı yanıtı, aşağıdaki parametrelere sahip bir JSON OAuth 2,0 yanıtdır.

| Parametre | Açıklama |
| --- | --- |
| `token_type` | Belirteç türü değerini gösterir. Microsoft Identity platformunun desteklediği tek tür `Bearer`. Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz. [OAuth 2,0 yetkilendirme çerçevesi: taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| `scope` | Belirteçte izin verilen erişim kapsamı. |
| `expires_in` | Erişim belirtecinin geçerli olduğu sürenin saniye cinsinden uzunluğu. |
| `access_token` | İstenen erişim belirteci. Çağıran hizmet, bu belirteci, alıcı hizmette kimlik doğrulaması yapmak için kullanabilir. |
| `refresh_token` | İstenen erişim belirtecinin yenileme belirteci. Çağıran hizmet, geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilir. Yenileme belirteci yalnızca `offline_access` kapsamı isteniyorsa sağlanır. |

### <a name="success-response-example"></a>Başarı yanıtı örneği

Aşağıdaki örnek, https://graph.microsoft.com Web API 'SI için bir erişim belirteci isteğine yönelik başarılı yanıtı gösterir.

```
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4AnqPnvFqnzS3GiikHr5wBM1bV1YyjH3nUeIhKhqJWGwqJFRqs2sE_rqUfz7__3J92JDpi6gDdCZNNaXgreQsH89kLCVNYZeN6kGuFGZrjwxp1wS2JYc97E_3reXBxkHrA09K5aR-WsSKCEjf6WI23FhZMTLhk_ZKOe_nWvcvLj13FyvSrTMZV2cmzyCZDqEHtPVLJgSoASuQlD2NXrfmtcmgWfc3uJSrWLIDSn4FEmVDA63X6EikNp9cllH3Gp7Vzapjlnws1NQ1_Ff5QrmBHp_LKEIwfzVKnLLrQXN0EzP8f6AX6fdVTaeKzm7iw6nH0vkPRpUeLc3q_aNsPzqcTOnFfgng7t2CXUsMAGH5wclAyFCAwL_Cds7KnyDLL7kzOS5AVZ3Mqk2tsPlqopAiHijZaJumdTILDudwKYCFAMpUeUwEf9JmyFjl2eIWPmlbwU7cHKWNvuRCOYVqbsTTpJthwh4PvsL5ov5CawH_TaV8omG_tV6RkziHG9urk9yp2PH9gl7Cv9ATa3Vt3PJWUS8LszjRIAJmyw_EhgHBfYCvEZ8U9PYarvgqrtweLcnlO7BfnnXYEC18z_u5wemAzNBFUje2ttpGtRmRic4AzZ708tBHva2ePJWGX6pgQbiWF8esOrvWjfrrlfOvEn1h6YiBW291M022undMdXzum6t1Y1huwxHPHjCAA"
}
```

> [!NOTE]
> Yukarıdaki erişim belirteci, v 1.0 biçimli bir belirteçtir. Bunun nedeni, belirtecin erişilmekte olan **kaynağa** göre sağlanması. Microsoft Graph, v 1.0 belirteçlerini kabul edecek şekilde ayarlanır. bu nedenle, bir istemci Microsoft Graph belirteçleri istediğinde, Microsoft Identity platform v 1.0 erişim belirteçleri üretir. Yalnızca uygulamalar erişim belirteçlerine bakmalıdır. İstemcilerin bunları incebir şekilde incelemesi **gerekir** .

### <a name="error-response-example"></a>Hata yanıtı örneği

Aşağı akış API 'sinde ayarlanmış bir koşullu erişim ilkesi (örneğin, Multi-Factor Authentication) varsa, bu belirteç uç noktası tarafından bir hata yanıtı döndürülür. Orta katman hizmeti, istemci uygulamanın koşullu erişim ilkesini karşılamak üzere kullanıcı etkileşimini sağlayabilmesi için bu hatayı istemci uygulamasına sunmalıdır.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Güvenli kaynağa erişmek için erişim belirtecini kullanma

Artık orta katman hizmeti, `Authorization` üstbilgisindeki belirteci ayarlayarak yukarı akış Web API 'sine kimliği doğrulanmış istekler yapmak için yukarıda alınan belirteci kullanabilir.

### <a name="example"></a>Örnek

```
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## <a name="gaining-consent-for-the-middle-tier-application"></a>Orta katmanlı uygulama için onay hakkı alma

Uygulamanızın mimarisine veya kullanımına bağlı olarak, OBO akışının başarılı olmasını sağlamaya yönelik farklı stratejiler göz önünde bulundurmayabilirsiniz. Her durumda, en son hedef, istemci uygulamanın orta katman uygulamayı çağırabilmesi ve orta katman uygulamanın arka uç kaynağını çağırma iznine sahip olması için doğru izin verilmesini sağlamaktır. 

> [!NOTE]
> Daha önce Microsoft hesabı sistemi (kişisel hesaplar) "bilinen istemci uygulaması" alanını desteklemez, ne de birleştirilmiş onay gösterebilir.  Bu eklendi ve Microsoft Identity platform 'daki tüm uygulamalar, OBO çağrıları için gettign onayı için bilinen istemci uygulaması yaklaşımını kullanabilir. 

### <a name="default-and-combined-consent"></a>/.exe varsayılan ve Birleşik onay

Orta katman uygulaması, istemcisini, bildiriminde bilinen istemci uygulamalar listesine ekler ve ardından istemci hem kendisi hem de orta katman uygulaması için bir Birleşik onay akışı tetikleyebilir. Microsoft Identity platform uç noktasında bu, [`/.default` kapsamı](v2-permissions-and-consent.md#the-default-scope)kullanılarak yapılır. Bilinen istemci uygulamalarını ve `/.default`kullanarak bir izin ekranını tetiklerken, onay ekranı **hem istemcinin hem** de orta katman API 'sine yönelik izinleri gösterir ve ayrıca orta katman API 'si için gerekli izinleri ister. Kullanıcı her iki uygulama için de onay sağlar ve ardından OBO akışı işe yarar.

### <a name="pre-authorized-applications"></a>Önceden yetkilendirilmiş uygulamalar

Kaynaklar, belirli bir uygulamanın her zaman belirli kapsamları alma iznine sahip olduğunu gösterebilir. Bu, öncelikli olarak bir ön uç istemcisi ile arka uç kaynağı arasındaki bağlantıları daha sorunsuz hale getirmek için yararlıdır. Bir kaynak, önceden yetkilendirilmiş birden çok uygulamayı bildirebilir. bu tür bir uygulama, bu izinleri bir OBO akışında isteyebilir ve Kullanıcı onay sağlamadan onları alabilir.

### <a name="admin-consent"></a>Yönetici onayı

Bir kiracı yöneticisi, uygulamanın orta katman uygulaması için yönetici onayı sağlayarak gereken API 'Leri çağırma iznine sahip olduğunu garanti edebilir. Bunu yapmak için, yönetici kendi kiracısında orta katman uygulamayı bulabilir, gerekli izinler sayfasını açabilir ve uygulama için izin vermek üzere seçim yapabilir. Yönetici onayı hakkında daha fazla bilgi için bkz. [onay ve izinler belgeleri](v2-permissions-and-consent.md).

### <a name="use-of-a-single-application"></a>Tek bir uygulamanın kullanımı

Bazı senaryolarda yalnızca tek bir orta katman ve ön uç istemcisi eşleştirmesi olabilir. Bu senaryoda, bir orta katman uygulaması gereksinimini tamamen ortadan, bu şekilde tek bir uygulama yapmak daha kolay olabilir. Ön uç ve Web API 'SI arasında kimlik doğrulaması yapmak için tanımlama bilgilerini, id_token veya uygulamanın kendisi için istenen erişim belirtecini kullanabilirsiniz. Ardından, bu tek uygulamadan arka uç kaynağına izin iste.

## <a name="client-limitations"></a>İstemci sınırlamaları

Bir istemci bir id_token almak için örtük akış kullanıyorsa ve bu istemcide bir yanıt URL 'sinde de joker karakterler varsa, bir OBO akışı için id_token kullanılamaz.  Ancak, örtülü izin akışı aracılığıyla alınan erişim belirteçleri, başlatma istemcisinde bir joker karakter yanıt URL 'SI kayıtlı olsa bile gizli bir istemci tarafından yine de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2,0 protokolü hakkında daha fazla bilgi edinin ve istemci kimlik bilgilerini kullanarak hizmet kimlik doğrulaması hizmeti gerçekleştirmek için başka bir yol öğrenin.

* [OAuth 2,0 istemci kimlik bilgileri Microsoft Identity platformunda izin ver](v2-oauth2-client-creds-grant-flow.md)
* [Microsoft Identity platformunda OAuth 2,0 kod akışı](v2-oauth2-auth-code-flow.md)
* [`/.default` kapsamını kullanma](v2-permissions-and-consent.md#the-default-scope)
