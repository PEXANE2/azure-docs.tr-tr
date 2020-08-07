---
title: Microsoft Identity platform ve OAuth 2.0 adına akış | Mavisi
titleSuffix: Microsoft identity platform
description: Bu makalede, Service to-on-the OAuth 2.0 kullanarak hizmet kimlik doğrulamasına hizmet uygulamak için HTTP iletilerinin nasıl kullanılacağı açıklanır.
services: active-directory
author: hpsin
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/7/2020
ms.author: hirsin
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 3abef3324bee61f2d7eb96c80750ad589b15f342
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87987044"
---
# <a name="microsoft-identity-platform-and-oauth-20-on-behalf-of-flow"></a>Microsoft Identity platform ve OAuth 2,0-adına akış


OAuth 2,0 on-of-the Flow (OBO), bir uygulamanın bir hizmet/Web API 'sini çağırdığı, yani başka bir hizmet/Web API çağrısı yapması gereken kullanım durumunu görür. Amaç, temsilci kullanıcı kimliğini ve izinleri istek zinciri aracılığıyla yaymanız önerilir. Orta katman hizmetin, aşağı akış hizmetine kimliği doğrulanmış istekleri yapması için Kullanıcı adına Microsoft Identity platformundan bir erişim belirtecinin güvenli hale getirme ihtiyacı vardır.

Bu makalede, uygulamanızdaki protokolde doğrudan programlanın nasıl yapılacağı açıklanır.  Mümkün olduğunda, [belirteçleri edinmek ve güvenli Web API 'lerini çağırmak](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows)Için desteklenen Microsoft kimlik doğrulama KITAPLıKLARıNı (msal) kullanmanızı öneririz.  Ayrıca [, msal kullanan örnek uygulamalara](sample-v2-code.md)göz atın.

> [!NOTE]
> Mayıs 2018 itibariyle, bazı örtük akış türetilmiş `id_token` OBO akışı için kullanılamaz. Tek sayfalı uygulamalar (maça 'Lar), OBO akışlarını gerçekleştirmek için orta katmanlı gizli bir istemciye **erişim** belirteci iletmelidir. Hangi istemcilerin OBO çağrıları gerçekleştirebileceği hakkında daha fazla bilgi için bkz. [sınırlamalar](#client-limitations).

## <a name="protocol-diagram"></a>Protokol diyagramı

Kullanıcının, [OAuth 2,0 yetkilendirme kodu verme akışı](v2-oauth2-auth-code-flow.md) veya başka bir oturum akışı kullanarak bir uygulamada kimliğinin doğrulandığını varsayın. Bu noktada, uygulamanın, orta katman Web API 'sine (API A) erişim için kullanıcının taleplerini ve onayını içeren *API a* (belirteç a) için bir erişim belirteci vardır. Şimdi API A 'nın, aşağı akış Web API 'sine (API B) kimliği doğrulanmış bir istek yapması gerekir.

Aşağıdaki adımlar, OBO akışını oluşturur ve aşağıdaki diyagramın yardımıyla açıklanır.

![Şirket adına OAuth 2.0 akışını gösterir](./media/v2-oauth2-on-behalf-of-flow/protocols-oauth-on-behalf-of-flow.png)

1. İstemci uygulaması, belirteç a ile API a 'ya bir istek yapar (bir `aud` API a talebi ile).
1. API A, Microsoft Identity platform belirteci verme uç noktasında kimlik doğrular ve API B 'ye erişmek için bir belirteç ister.
1. Microsoft Identity platform belirteci verme uç noktası, API A 'nın kimlik bilgilerini ve A belirtecini doğrular ve API B 'nin (belirteç B) erişim belirtecini API A 'ya yayınlar.
1. Belirteç B, API B 'ye yönelik isteğin yetkilendirme üstbilgisindeki API A tarafından ayarlanır.
1. Güvenli kaynaktaki veriler API B tarafından API A 'ya ve istemciye buradan döndürülür.

> [!NOTE]
> Bu senaryoda, orta katman hizmeti kullanıcının aşağı akış API 'sine erişim iznini almak için Kullanıcı etkileşimi yoktur. Bu nedenle, aşağı akış API 'sine erişim izni verme seçeneği, kimlik doğrulama sırasında izin adımının bir parçası olarak önde sunulur. Uygulamanızı için nasıl ayarlayacağınızı öğrenmek için bkz. [Orta katmanlı uygulama için izin](#gaining-consent-for-the-middle-tier-application)alma.

## <a name="middle-tier-access-token-request"></a>Orta katman erişim belirteci isteği

Erişim belirteci istemek için, kiracıya özgü Microsoft Identity platform belirteci uç noktasında aşağıdaki parametrelerle bir HTTP GÖNDERISI yapın.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

İstemci uygulamanın, paylaşılan bir gizli dizi ya da sertifikayla güvenli hale getirilme şekline bağlı olarak iki durum vardır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: paylaşılan gizli dizi ile belirteç isteğine erişin

Paylaşılan bir gizli dizi kullanılırken hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre | Tür | Açıklama |
| --- | --- | --- |
| `grant_type` | Gerekli | Belirteç isteği türü. JWT kullanan bir istek için değer olmalıdır `urn:ietf:params:oauth:grant-type:jwt-bearer` . |
| `client_id` | Gerekli | [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `client_secret` | Gerekli | Azure portal Uygulama kayıtları sayfasında uygulamanız için oluşturduğunuz istemci gizli anahtarı. |
| `assertion` | Gerekli | Orta katman API 'sine gönderilen erişim belirteci.  Bu belirteç, `aud` Bu OBO isteğini yapan uygulamanın (alan tarafından belirtilen uygulama) bir hedef kitle () talebine sahip olmalıdır `client-id` . Uygulamalar, farklı bir uygulama için belirteç kullanamaz (yani, bir istemci bir API 'YI MS Graph için bir belirteç gönderirse, bu API 'yi OBO kullanarak kullanamaz.  Bunun yerine belirteci reddetmesi gerekir).  |
| `scope` | Gerekli | Belirteç isteği için bir alan ayrılmış kapsam listesi. Daha fazla bilgi için bkz. [kapsamlar](v2-permissions-and-consent.md). |
| `requested_token_use` | Gerekli | İsteğin nasıl işleneceğini belirtir. OBO akışında, değerin olarak ayarlanması gerekir `on_behalf_of` . |

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, `user.read` Web API 'si için bir erişim belirteci ve yenileme belirteci ister https://graph.microsoft.com .

```HTTP
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com/<tenant>
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyO{a lot of characters here}
&scope=https://graph.microsoft.com/user.read+offline_access
&requested_token_use=on_behalf_of
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: bir sertifikayla erişim belirteci isteği

Bir sertifikaya sahip hizmetten hizmete erişim belirteci isteği aşağıdaki parametreleri içerir:

| Parametre | Tür | Açıklama |
| --- | --- | --- |
| `grant_type` | Gerekli | Belirteç isteğinin türü. JWT kullanan bir istek için değer olmalıdır `urn:ietf:params:oauth:grant-type:jwt-bearer` . |
| `client_id` | Gerekli |  [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasının uygulamanıza atadığı uygulama (ISTEMCI) kimliği. |
| `client_assertion_type` | Gerekli | Değer olmalıdır `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` . |
| `client_assertion` | Gerekli | Uygulamanız için kimlik bilgileri olarak kaydettiğiniz sertifikayı oluşturmanız ve oturum açmanız için gereken bir onaylama (JSON Web belirteci). Sertifikanızı ve onaylama biçiminizi nasıl kaydedeceğinizi öğrenmek için bkz. [sertifika kimlik bilgileri](active-directory-certificate-credentials.md). |
| `assertion` | Gerekli |  Orta katman API 'sine gönderilen erişim belirteci.  Bu belirteç, `aud` Bu OBO isteğini yapan uygulamanın (alan tarafından belirtilen uygulama) bir hedef kitle () talebine sahip olmalıdır `client-id` . Uygulamalar, farklı bir uygulama için belirteç kullanamaz (yani, bir istemci bir API 'YI MS Graph için bir belirteç gönderirse, bu API 'yi OBO kullanarak kullanamaz.  Bunun yerine belirteci reddetmesi gerekir).  |
| `requested_token_use` | Gerekli | İsteğin nasıl işleneceğini belirtir. OBO akışında, değerin olarak ayarlanması gerekir `on_behalf_of` . |
| `scope` | Gerekli | Belirteç isteğine yönelik kapsamların boşlukla ayrılmış listesi. Daha fazla bilgi için bkz. [kapsamlar](v2-permissions-and-consent.md).|

Parametrelerin, paylaşılan gizli dizi tarafından, parametrenin iki parametre ile değiştirilmeleri dışında neredeyse, bu parametre, paylaşılan gizli anahtar ile aynı olduğunu fark ettiğini unutmayın `client_secret` : a `client_assertion_type` ve `client_assertion` .

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, `user.read` https://graph.microsoft.com bir sertifika Ile Web API 'si için kapsama sahip bir erişim belirteci ister.

```HTTP
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com/<tenant>
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiO{a lot of characters here}
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read+offline_access
```

## <a name="middle-tier-access-token-response"></a>Orta katman erişim belirteci yanıtı

Başarı yanıtı, aşağıdaki parametrelere sahip bir JSON OAuth 2,0 yanıtdır.

| Parametre | Açıklama |
| --- | --- |
| `token_type` | Belirteç türü değerini gösterir. Microsoft Identity platformunun desteklediği tek tür `Bearer` . Taşıyıcı belirteçleri hakkında daha fazla bilgi için bkz. [OAuth 2,0 yetkilendirme çerçevesi: taşıyıcı belirteç kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| `scope` | Belirteçte izin verilen erişim kapsamı. |
| `expires_in` | Erişim belirtecinin geçerli olduğu sürenin saniye cinsinden uzunluğu. |
| `access_token` | İstenen erişim belirteci. Çağıran hizmet, bu belirteci, alıcı hizmette kimlik doğrulaması yapmak için kullanabilir. |
| `refresh_token` | İstenen erişim belirtecinin yenileme belirteci. Çağıran hizmet, geçerli erişim belirtecinin süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilir. Yenileme belirteci yalnızca `offline_access` kapsam isteniyorsa sağlanır. |

### <a name="success-response-example"></a>Başarı yanıtı örneği

Aşağıdaki örnek, Web API 'SI için bir erişim belirteci isteğine yönelik başarılı yanıtı gösterir https://graph.microsoft.com .

```json
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4An{a lot of characters here}"
}
```

> [!NOTE]
> Yukarıdaki erişim belirteci, mikro Osft Graph için v 1.0 biçimli bir belirteçtir. Bunun nedeni, belirteç biçiminin erişilmekte olan **kaynağı** temel aldığı ve bunu istemek için kullanılan uç noktalarla ilgisiz olmasından kaynaklanır. Microsoft Graph, v 1.0 belirteçlerini kabul edecek şekilde ayarlanır. bu nedenle, bir istemci Microsoft Graph belirteçleri istediğinde, Microsoft Identity platform v 1.0 erişim belirteçleri üretir. Diğer uygulamalar, v 2.0-biçim belirteçleri, v 1.0-biçim belirteçleri, hatta özel veya şifreli belirteç biçimleri istediğini gösterebilir.  Hem v 1.0 hem de v 2.0 uç noktaları, belirteç biçimi oluşturabilir. bu şekilde kaynak, belirtecin istemci tarafından nasıl veya nerede istendiğine bakılmaksızın her zaman doğru belirteç biçimini alabilir. 
>
> Yalnızca uygulamalar erişim belirteçlerine bakmalıdır. İstemcilerin bunları incebir şekilde incelemesi **gerekir** . Kodunuzda diğer uygulamalar için erişim belirteçleri incelenirken, bu uygulama belirteçlerinin biçimini değiştirdiğinde veya şifrelemeyi başlattığında uygulamanızın beklenmedik bir şekilde bozmasına neden olur. 

### <a name="error-response-example"></a>Hata yanıtı örneği

Aşağı akış API 'sinde ayarlanmış bir koşullu erişim ilkesi (örneğin, [Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md)) varsa, bu belirteç uç noktası tarafından bir hata yanıtı döndürülür. Orta katman hizmeti, istemci uygulamanın koşullu erişim ilkesini karşılamak üzere kullanıcı etkileşimini sağlayabilmesi için bu hatayı istemci uygulamasına sunmalıdır.

```json
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

Artık orta katman hizmeti, üst bilgide belirteç ayarlayarak yukarı akış Web API 'sine kimliği doğrulanmış istekler yapmak için yukarıda alınan belirteci kullanabilir `Authorization` .

### <a name="example"></a>Örnek

```HTTP
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiO ... 0X2tnSQLEANnSPHY0gKcgw
```

## <a name="saml-assertions-obtained-with-an-oauth20-obo-flow"></a>SAML onayları bir OAuth 2.0 OBO akışıyla alındı

Bazı OAuth tabanlı Web hizmetlerinin, etkileşimli olmayan akışlarda SAML onayları kabul eden diğer Web hizmeti API 'Lerine erişmesi gerekir. Azure Active Directory, SAML tabanlı bir Web hizmetini hedef kaynak olarak kullanan şirket adına bir akışa yanıt olarak bir SAML onayı sağlayabilir.

>[!NOTE]
>Bu, OAuth2 tabanlı bir uygulamanın SAML belirteçlerini kullanan Web hizmeti API uç noktalarına erişmesine olanak tanıyan, OAuth 2,0 için standart olmayan bir uzantıdır.

> [!TIP]
> Bir ön uç Web uygulamasından SAML korumalı bir Web hizmeti çağırdığınızda, API 'yi çağırabilir ve kullanıcının mevcut oturumunda normal bir etkileşimli kimlik doğrulama akışı başlatabilirsiniz. Yalnızca bir hizmetten hizmete çağrı, Kullanıcı bağlamı sağlamak için bir SAML belirteci gerektirdiğinde bir OBO akışı kullanmanız gerekir.

## <a name="gaining-consent-for-the-middle-tier-application"></a>Orta katmanlı uygulama için onay hakkı alma

Uygulamanızın mimarisine veya kullanımına bağlı olarak, OBO akışının başarılı olmasını sağlamaya yönelik farklı stratejiler göz önünde bulundurmayabilirsiniz. Her durumda, en son hedef, istemci uygulamanın orta katman uygulamayı çağırabilmesi ve orta katman uygulamanın arka uç kaynağını çağırma iznine sahip olması için doğru izin verilmesini sağlamaktır.

> [!NOTE]
> Daha önce Microsoft hesabı sistemi (kişisel hesaplar) "bilinen istemci uygulaması" alanını desteklemez, ne de birleştirilmiş onay gösterebilir.  Bu eklendi ve Microsoft Identity platform 'daki tüm uygulamalar, OBO çağrılarına onay almak için bilinen istemci uygulaması yaklaşımını kullanabilir.

### <a name="default-and-combined-consent"></a>/.exe varsayılan ve Birleşik onay

Orta katman uygulaması, istemcisini, bildiriminde bilinen istemci uygulamalar listesine ekler ve ardından istemci hem kendisi hem de orta katman uygulaması için bir Birleşik onay akışı tetikleyebilir. Microsoft Identity platform uç noktasında, bu [ `/.default` kapsam](v2-permissions-and-consent.md#the-default-scope)kullanılarak yapılır. Bilinen istemci uygulamalarını kullanarak bir izin ekranını tetiklerken `/.default` , onay ekranı **hem istemcinin hem** de orta katman API 'sine yönelik izinleri gösterir ve ayrıca orta katman API 'si için gerekli izinleri ister. Kullanıcı her iki uygulama için de onay sağlar ve ardından OBO akışı işe yarar.

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
* [Kapsamı kullanma `/.default`](v2-permissions-and-consent.md#the-default-scope)
