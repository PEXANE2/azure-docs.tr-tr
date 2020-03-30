---
title: OAuth2.0 akışı adına hizmet-to-servis kimlik doğrulaması | Microsoft Dokümanlar
description: Bu makalede, OAuth2.0 On-Behalf akışı ile hizmet-servis kimlik doğrulaması uygulamak için HTTP iletileri nasıl kullanılacağı açıklanmaktadır.
services: active-directory
documentationcenter: .net
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviewer: hirsin, nacanuma
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: a301029f30a77f4e62ad3529aac488a81c12566e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80154534"
---
# <a name="service-to-service-calls-that-use-delegated-user-identity-in-the-on-behalf-of-flow"></a>On-Behalf akışında temsilci kullanıcı kimliğini kullanan servise çağrı

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

OAuth 2.0 On-Behalf-Of (OBO) akışı, kullanıcı kimlik doğrulamasını başka bir hizmete veya web API'sine geçirmek için bir hizmeti veya web API'sini çağıran bir uygulama sağlar. OBO akışı, temsilci kullanıcı kimliğini ve izinleri istek zinciri aracılığıyla yadar. Orta katman hizmetinin alt akış hizmetine kimlik doğrulama isteği nde bulunabilmesi için, kullanıcı adına Azure Active Directory'den (Azure AD) bir erişim belirteci sağlaması gerekir.

> [!IMPORTANT]
> Mayıs 2018 itibariyle, `id_token` On-Behalf akışı için bir kullanılamaz.  Tek sayfalı uygulamaların (SPA'lar) OBO akışlarını gerçekleştirmek için bir erişim jetonundan orta katmanlı gizli istemciye geçmesi gerekir. Ad-Behalf aramalarını gerçekleştirebilen istemciler hakkında daha fazla ayrıntı için [sınırlamalar](#client-limitations)bakın.

## <a name="on-behalf-of-flow-diagram"></a>On-Behalf-Of akış diyagramı

OBO akışı, kullanıcı [oauth 2.0 yetkilendirme kodu hibe akışını](v1-protocols-oauth-code.md)kullanan bir uygulamada kimlik doğrulaması yapıldıktan sonra başlar. Bu noktada, uygulama, kullanıcının taleplerini ve API A'ya erişim iznini içeren orta katman web API'sine (API A) bir erişim jetonu (belirteç A) gönderir. Ardından, API A, akış aşağı web API'si (API B) için kimlik doğrulaması isteği yapar.

Bu adımlar On-Behalf-Of akışı ![oluşturur: OAuth2.0 On-Behalf-Of akış adımları gösterir](./media/v1-oauth2-on-behalf-of-flow/active-directory-protocols-oauth-on-behalf-of-flow.png)

1. İstemci uygulaması, A belirteciyle API A'ya bir istekte bulundu.
1. API A, Azure AD belirteci verme bitiş noktasına doğrudoğru doğrular ve API B'ye erişmek için bir belirteç ister.
1. Azure AD belirteç verme bitiş noktası, API A'nın kimlik bilgilerini belirteç A ile doğrular ve API B (token B) için erişim belirteci çıkarır.
1. API B isteği yetkilendirme üstbilgisinde Belirteç B içerir.
1. API B, güvenli kaynaktan gelen verileri döndürür.

>[!NOTE]
>Bir giriş hizmeti için belirteç istemek için kullanılan bir erişim jetonundaki hedef kitle talebi, OBO isteğini yapan hizmetin kimliği olmalıdır. Belirteç ayrıca Azure Active Directory global imza anahtarıyla da imzalanmalıdır (portaldaki **Uygulama kayıtları** üzerinden kayıtlı uygulamalar için varsayılan dır).

## <a name="register-the-application-and-service-in-azure-ad"></a>Uygulamayı ve hizmeti Azure AD'de kaydedin

Hem orta katman hizmetini hem de istemci uygulamasını Azure AD'de kaydedin.

### <a name="register-the-middle-tier-service"></a>Orta katman hizmetini kaydedin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst teki çubukta, hesabınızı seçin ve uygulamanız için bir Etkin Dizin kiracısı seçmek için **Dizin** listesinin altına bakın.
1. Sol bölmede **Daha Fazla Hizmet** seçin ve Azure Etkin **Dizini'ni**seçin.
1. **Uygulama kayıtlarını** seçin ve ardından **Yeni kayıt.**
1. Uygulama için uygun bir ad girin ve uygulama türünü seçin.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
1. URI'yi temel URL'ye yönlendirmeyi ayarlayın.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Azure portalından çıkmadan önce bir istemci sırrı oluşturun.
1. Azure portalında uygulamanızı seçin ve **Sertifikalar & sırları**seçin.
1. **Yeni istemci sırrını** seçin ve bir veya iki yıllık bir süre ile bir sır ekleyin.
1. Bu sayfayı kaydettiğinizde, Azure portalı gizli değeri görüntüler. Gizli değeri güvenli bir yerde kopyalayın ve kaydedin.

> [!IMPORTANT]
> Uygulamanızdaki uygulama ayarlarını yapılandırmak için gizli ye ihtiyacınız vardır. Bu gizli değer yeniden görüntülenmez ve başka yollarla alınamaz. Azure portalında görünür olduğu anda kaydedin.

### <a name="register-the-client-application"></a>İstemci başvurusunu kaydedin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst teki çubukta, hesabınızı seçin ve uygulamanız için bir Etkin Dizin kiracısı seçmek için **Dizin** listesinin altına bakın.
1. Sol bölmede **Daha Fazla Hizmet** seçin ve Azure Etkin **Dizini'ni**seçin.
1. **Uygulama kayıtlarını** seçin ve ardından **Yeni kayıt.**
1. Uygulama için uygun bir ad girin ve uygulama türünü seçin.
1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
1. URI'yi temel URL'ye yönlendirmeyi ayarlayın.
1. Uygulamayı kaydetmek için **Kaydet**'i seçin.
1. Uygulamanız için izinleri yapılandırın. **API izinlerinde,** **izin ekle'yi** ve ardından **API'lerimi**seçin.
1. Metin alanına orta katman hizmetinin adını yazın.
1. **İzinleri Seç'i** seçin ve ardından **Access \<hizmet adını>** seçin.

### <a name="configure-known-client-applications"></a>Bilinen istemci uygulamalarını yapılandırma

Bu senaryoda, orta katman hizmetinin, kullanıcı etkileşimi olmadan akış aşağı API'sine erişmek için kullanıcının onayını alması gerekir. Alt akış API'sine erişim izni verme seçeneği, kimlik doğrulama sırasında onay adımının bir parçası olarak ön sunulmalıdır.

İstemci uygulamasının Azure AD'deki kaydını orta katman hizmetinin kaydıyla açıkça bağlamak için aşağıdaki adımları izleyin. Bu işlem, hem istemci hem de orta katman tarafından gerekli olan onayı tek bir iletişim kutusunda birleştirir.

1. Orta katman hizmet kaydına gidin ve bildirim düzenleyicisini açmak için **Bildirim'i** seçin.
1. Dizi `knownClientApplications` özelliğini bulun ve istemci uygulamasının istemci kimliğini bir öğe olarak ekleyin.
1. Kaydet'i seçerek bildirimi **kaydet.**

## <a name="service-to-service-access-token-request"></a>Servisten hizmete erişim jeton isteği

Bir erişim jetonu istemek için, kiracıya özgü Azure AD bitiş noktasına aşağıdaki parametrelerle bir HTTP POST gönderin:

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```

İstemci uygulaması paylaşılan bir sır veya sertifika ile güvence altına alınır.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>İlk durum: Paylaşılan bir sırla giriş belirteç isteği

Paylaşılan bir gizli kullanırken, servisten hizmete erişim belirteç isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli | Belirteç isteğinin türü. OBO isteği bir JSON Web Belirteci (JWT) kullanır, bu nedenle değer **urn:ietf:params:oauth:grant-type:jwt-bearer**olmalıdır. |
| Iddia |gerekli | İstekte kullanılan erişim belirteci değeri. |
| client_id |gerekli | Azure AD'ye kayıt sırasında arama hizmetine atanan uygulama kimliği. Azure portalında uygulama kimliğini bulmak için **Active Directory'yi**seçin, dizini seçin ve ardından uygulama adını seçin. |
| client_secret |gerekli | Anahtar, Azure AD'de arama hizmeti için kaydedilmiş. Bu değer, kayıt sırasında belirtilmiş olmalıdır. |
| kaynak |gerekli | Alıcı hizmetin uygulama kimliği URI (güvenli kaynak). Uygulama Kimliği URI'yi Azure portalında bulmak için **Active Directory'yi** seçin ve dizini seçin. Uygulama adını seçin, **Tüm ayarları**seçin ve ardından **Özellikler'i**seçin. |
| requested_token_use |gerekli | İsteğin nasıl işlenmesi gerektiğini belirtir. On-Behalf-Of akışında, değer **on_behalf_of**olmalıdır. |
| scope |gerekli | Belirteç isteği için kapsamların ayrılmış bir boşluk listesi. OpenID Connect için **openid** kapsamı belirtilmelidir.|

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, web API'sı için bir erişim belirteci talep ediyor. https://graph.microsoft.com Erişim `client_id` belirteci isteyen hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.microsoft.com
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.ewogICJhdWQiOiAiaHR0cHM6Ly9ncmFwaC5taWNyb3NvZnQuY29tIiwKICAiaXNzIjogImh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLAogICJpYXQiOiAxNDkzNDIzMTY4LAogICJuYmYiOiAxNDkzNDIzMTY4LAogICJleHAiOiAxNDkzNDY2OTUxLAogICJhY3IiOiAiMSIsCiAgImFpbyI6ICJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsCiAgImFtciI6IFsKICAgICJwd2QiCiAgXSwKICAiYXBwaWQiOiAiNjI1MzkxYWYtYzY3NS00M2U1LThlNDQtZWRkM2UzMGNlYjE1IiwKICAiYXBwaWRhY3IiOiAiMSIsCiAgImVfZXhwIjogMzAyNjgzLAogICJmYW1pbHlfbmFtZSI6ICJUZXN0IiwKICAiZ2l2ZW5fbmFtZSI6ICJOYXZ5YSIsCiAgImlwYWRkciI6ICIxNjcuMjIwLjEuMTc3IiwKICAibmFtZSI6ICJOYXZ5YSBUZXN0IiwKICAib2lkIjogIjFjZDRiY2FjLWI4MDgtNDIzYS05ZTJmLTgyN2ZiYjFiYjczOSIsCiAgInBsYXRmIjogIjMiLAogICJwdWlkIjogIjEwMDMzRkZGQTEyRUQ3RkUiLAogICJzY3AiOiAiVXNlci5SZWFkIiwKICAic3ViIjogIjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLAogICJ0aWQiOiAiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwKICAidW5pcXVlX25hbWUiOiAibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLAogICJ1cG4iOiAibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLAogICJ1dGkiOiAieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsCiAgInZlciI6ICIxLjAiCn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
&requested_token_use=on_behalf_of
&scope=openid
```

### <a name="second-case-access-token-request-with-a-certificate"></a>İkinci durum: Sertifikayla giriş isteği

Sertifikalı servisten hizmete erişim belirteç isteği aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli | Belirteç isteğinin türü. OBO isteği, bir JWT erişim belirteci kullanır, bu nedenle değer **urn:ietf:params:oauth:grant-type:jwt-bearer**olmalıdır. |
| Iddia |gerekli | İstekte kullanılan belirteç değeri. |
| client_id |gerekli | Azure AD'ye kayıt sırasında arama hizmetine atanan uygulama kimliği. Azure portalında uygulama kimliğini bulmak için **Active Directory'yi**seçin, dizini seçin ve ardından uygulama adını seçin. |
| client_assertion_type |gerekli |Değer,`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |gerekli | Uygulamanız için kimlik bilgileri olarak kaydettiğiniz sertifikayı oluşturduğunuz ve imzaladığınız bir JSON Web Belirteci. İddia biçimi ve sertifikanızı nasıl kaydedebilirsiniz hakkında bilgi edinmek için [sertifika kimlik bilgilerine](../develop/active-directory-certificate-credentials.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) bakın.|
| kaynak |gerekli | Alıcı hizmetin uygulama kimliği URI (güvenli kaynak). Uygulama Kimliği URI'yi Azure portalında bulmak için **Active Directory'yi** seçin ve dizini seçin. Uygulama adını seçin, **Tüm ayarları**seçin ve ardından **Özellikler'i**seçin. |
| requested_token_use |gerekli | İsteğin nasıl işlenmesi gerektiğini belirtir. On-Behalf-Of akışında, değer **on_behalf_of**olmalıdır. |
| scope |gerekli | Belirteç isteği için kapsamların ayrılmış bir boşluk listesi. OpenID Connect için **openid** kapsamı belirtilmelidir.|

Bu parametreler, paylaşılan gizli tarafından istek ile hemen `client_secret parameter` hemen aynıdır iki `client_assertion_type` parametre ile değiştirilir dışında: ve `client_assertion`.

#### <a name="example"></a>Örnek

Aşağıdaki HTTP POST, sertifikalı web https://graph.microsoft.com API'si için bir erişim belirteci talep ediyor. Erişim `client_id` belirteci isteyen hizmeti tanımlar.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.microsoft.com
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## <a name="service-to-service-access-token-response"></a>Servise hizmete erişim belirteç yanıtı

Bir başarı yanıtı aşağıdaki parametreleri ile bir JSON OAuth 2.0 yanıtıdır:

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değerini gösterir. Azure AD'nin desteklediği **Bearer**tek tür Taşıyıcı'dır. Taşıyıcı belirteçleri hakkında daha fazla bilgi için [OAuth 2.0 Yetkilendirme Çerçevesi: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt)adresine bakın. |
| scope |Belirteçte verilen erişim kapsamı. |
| expires_in |Erişim belirteci nin geçerli olduğu süre (saniye cinsinden). |
| expires_on |Erişim belirteci süresinin dolduğu saat. Tarih, 1970-01-01T0:0:0Z UTC'den son kullanma tarihine kadar olan saniye sayısı olarak temsil edilir. Bu değer, önbelleğe alınmış belirteçlerin kullanım ömrünü belirlemek için kullanılır. |
| kaynak |Alıcı hizmetin uygulama kimliği URI (güvenli kaynak). |
| access_token |İstenen erişim jetonu. Arama hizmeti, alıcı hizmetin kimliğini doğrulamak için bu belirteci kullanabilir. |
| id_token |İstenen kimlik belirteci. Arama hizmeti, kullanıcının kimliğini doğrulamak ve kullanıcıyla oturum başlatmak için bu belirteci kullanabilir. |
| refresh_token |İstenen erişim belirteci için yenileme belirteci. Arama hizmeti, geçerli erişim belirteci sona erdikten sonra başka bir erişim belirteci istemek için bu belirteci kullanabilirsiniz. |

### <a name="success-response-example"></a>Başarı yanıtı örneği

Aşağıdaki örnek, web API'si için erişim https://graph.microsoft.com belirteci isteğine verilen başarı yanıtını gösterir.

```json
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.microsoft.com",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### <a name="error-response-example"></a>Hata yanıtı örneği

Azure AD belirteci bitiş noktası, Koşullu Erişim ilkesiyle ayarlanmış bir akış aşağı API'si (örneğin, çok faktörlü kimlik doğrulama) için bir erişim belirteci elde etmeye çalıştığında bir hata yanıtı döndürür. Orta katman hizmeti, istemci uygulamasının Koşullu Erişim ilkesini karşılamak için kullanıcı etkileşimini sağlayabilmek için bu hatayı istemci uygulamasına sunmalıdır.

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

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Güvenli kaynağa erişmek için erişim belirteci'ni kullanma

Orta katman hizmeti, üstbilgideki belirteci ayarlayarak alt akış web API'sine kimlik doğrulama `Authorization` isteklerini yapmak için edinilmiş erişim belirteci'ni kullanabilir.

### <a name="example"></a>Örnek

```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="saml-assertions-obtained-with-an-oauth20-obo-flow"></a>OAuth2.0 OBO akışı ile elde edilen SAML iddiaları

Bazı OAuth tabanlı web hizmetlerinin etkileşimli olmayan akışlarda SAML iddialarını kabul eden diğer web hizmeti API'lerine erişmeleri gerekir. Azure Active Directory, saml tabanlı bir web hizmetini hedef kaynak olarak kullanan Bir Ad-Behalf akışına yanıt olarak bir SAML iddiası sağlayabilir.

>[!NOTE]
>Bu, OAuth2 tabanlı bir uygulamanın SAML belirteçlerini tüketen web hizmeti API uç noktalarına erişmesine olanak tanıyan OAuth 2.0 On-Behalf-Of akışının standart dışı bir uzantısıdır.

> [!TIP]
> Ön uç web uygulamasından SAML korumalı bir web hizmetini aradiğinizde, yalnızca API'yi arayabilir ve kullanıcının mevcut oturumuyla normal bir etkileşimli kimlik doğrulama akışı başlatabilirsiniz. Yalnızca bir servis-servis çağrısı kullanıcı bağlamı sağlamak için bir SAML belirteci gerektiriyorsa OBO akışını kullanmanız gerekir.

### <a name="obtain-a-saml-token-by-using-an-obo-request-with-a-shared-secret"></a>Paylaşılan bir gizli ile OBO isteği kullanarak bir SAML belirteci edinme

SAML iddiası için servise bir istek aşağıdaki parametreleri içerir:

| Parametre |  | Açıklama |
| --- | --- | --- |
| grant_type |gerekli | Belirteç isteğinin türü. JWT kullanan bir istek için, değer **urn:ietf:params:oauth:grant-type:jwt-bearer**olmalıdır. |
| Iddia |gerekli | İstekte kullanılan erişim belirteci değeri.|
| client_id |gerekli | Azure AD'ye kayıt sırasında arama hizmetine atanan uygulama kimliği. Azure portalında uygulama kimliğini bulmak için **Active Directory'yi**seçin, dizini seçin ve ardından uygulama adını seçin. |
| client_secret |gerekli | Anahtar, Azure AD'de arama hizmeti için kaydedilmiş. Bu değer, kayıt sırasında belirtilmiş olmalıdır. |
| kaynak |gerekli | Alıcı hizmetin uygulama kimliği URI (güvenli kaynak). Bu, SAML belirteci Hedef Kitlesi olacak kaynaktır. Uygulama Kimliği URI'yi Azure portalında bulmak için **Active Directory'yi** seçin ve dizini seçin. Uygulama adını seçin, **Tüm ayarları**seçin ve ardından **Özellikler'i**seçin. |
| requested_token_use |gerekli | İsteğin nasıl işlenmesi gerektiğini belirtir. On-Behalf-Of akışında, değer **on_behalf_of**olmalıdır. |
| requested_token_type | gerekli | İstenen belirteç türünü belirtir. Değer, erişilen kaynağın gereksinimlerine bağlı olarak **urn:ietf:params:oauth:token-type:saml2** veya **urn:ietf:params:oauth:token-type:saml1** olabilir. |

Yanıt UTF8 ve Base64url kodlanmış bir SAML belirteci içerir.

- **OBO çağrısından kaynaklanan bir SAML iddiası için SubjectConfirmationData**: Hedef uygulama **SubjectConfirmationData'da**alıcı değeri gerektiriyorsa, değer kaynak uygulaması yapılandırmasında joker olmayan bir Yanıt URL'si olmalıdır.
- **SubjectConfirmationData düğümü**: Bir SAML yanıtının parçası olmadığı için düğüm **InResponseTo** özniteliği içeremez. SAML belirteci alan uygulama, **INResponseTo** özniteliği olmadan SAML iddiasını kabul edebilmelidir.

- **Onay**: OAuth akışında kullanıcı verilerini içeren bir SAML belirteci almak için izin verilmiş olmalıdır. İzinler ve yönetici onayı alma hakkında bilgi için Azure [Etkin Dizin v1.0 bitiş noktasındaki İzinler ve onay](https://docs.microsoft.com/azure/active-directory/azuread-dev/v1-permissions-consent)bilgisine bakın.

### <a name="response-with-saml-assertion"></a>SAML iddiası ile yanıt

| Parametre | Açıklama |
| --- | --- |
| token_type |Belirteç türü değerini gösterir. Azure AD'nin desteklediği **Bearer**tek tür Taşıyıcı'dır. Taşıyıcı belirteçleri hakkında daha fazla bilgi için Bkz. [OAuth 2.0 Yetkilendirme Çerçevesi: Taşıyıcı Belirteç Kullanımı (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| scope |Belirteçte verilen erişim kapsamı. |
| expires_in |Erişim belirteci nin geçerli olduğu süre (saniye cinsinden). |
| expires_on |Erişim belirteci süresinin dolduğu saat. Tarih, 1970-01-01T0:0:0Z UTC'den son kullanma tarihine kadar olan saniye sayısı olarak temsil edilir. Bu değer, önbelleğe alınmış belirteçlerin kullanım ömrünü belirlemek için kullanılır. |
| kaynak |Alıcı hizmetin uygulama kimliği URI (güvenli kaynak). |
| access_token |SAML iddiasını döndüren parametre. |
| refresh_token |Yenileme belirteci. Arama hizmeti, geçerli SAML iddiasının süresi dolduktan sonra başka bir erişim belirteci istemek için bu belirteci kullanabilir. |

- token_type: Taşıyıcı
- expires_in: 3296
- ext_expires_in: 0
- expires_on: 1529627844
- Kaynak:`https://api.contoso.com`
- access_token: \<SAML iddiası\>
- issued_token_type: vazo:ietf:params:oauth:token-tipi:saml2
- refresh_token: \<Belirteci yenileyin\>

## <a name="client-limitations"></a>İstemci sınırlamaları

Joker karakter yanıt URL'si olan ortak `id_token` istemciler OBO akışları için bir kullanamaz. Ancak, gizli bir istemci, kamu istemcisi URI'yi yeniden yönlendiren bir joker kartı olsa bile, örtülü hibe akışı yoluyla edinilen **erişim** jetonları'nı yine de kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

OAuth 2.0 protokolü ve istemci kimlik bilgilerini kullanan hizmet-hizmet kimlik doğrulaması gerçekleştirmenin başka bir yolu hakkında daha fazla bilgi edinin:

* [Azure AD'deki OAuth 2.0 istemci kimlik bilgilerini kullanarak kimlik doğrulaması hizmeti verme hizmeti](v1-oauth2-client-creds-grant-flow.md)
* [Azure AD'de OAuth 2.0](v1-protocols-oauth-code.md)
