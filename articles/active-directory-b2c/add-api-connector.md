---
title: Kullanıcı akışlarına API bağlayıcıları ekleme (Önizleme)
description: Bir Kullanıcı akışında kullanılacak bir API bağlayıcısını yapılandırın.
services: active-directory-b2c
ms.service: active-directory
ms.subservice: B2C
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.openlocfilehash: 857429ab5fd2e2ea9a0cb0173015ceba4bb0bacb
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92504120"
---
# <a name="add-an-api-connector-to-a-sign-up-user-flow-preview"></a>Kaydolma Kullanıcı akışına API Bağlayıcısı ekleme (Önizleme)

> [!IMPORTANT]
> Kaydolma için API bağlayıcıları, Azure AD B2C genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir [API bağlayıcısını](api-connectors-overview.md)kullanmak IÇIN önce API bağlayıcısını oluşturun ve ardından Kullanıcı akışında etkinleştirin.

## <a name="create-an-api-connector"></a>API Bağlayıcısı oluşturma

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Azure hizmetleri**altında **Azure AD B2C**' yi seçin.
4. **API bağlayıcıları (Önizleme)** öğesini seçin ve ardından **yeni API Bağlayıcısı**' nı seçin.

   ![Yeni API Bağlayıcısı ekleme](./media/add-api-connector/api-connector-new.png)

5. Çağrı için bir görünen ad belirtin. Örneğin, **Kullanıcı bilgilerini doğrulayın**.
6. API çağrısının **uç nokta URL 'sini** sağlayın.
7. API için kimlik doğrulama bilgilerini sağlayın.

   - Şu anda yalnızca temel kimlik doğrulaması destekleniyor. Geliştirme amacıyla temel kimlik doğrulaması olmadan bir API kullanmak istiyorsanız, API 'nizin yoksaymasına yönelik bir ' kukla ' **Kullanıcı adı** ve **parola** girmeniz yeterlidir. API anahtarı olan bir Azure Işleviyle birlikte kullanmak için, kodu **uç nokta URL 'sine** bir sorgu parametresi olarak ekleyebilirsiniz (örneğin, https []() ://contoso.azurewebsites.net/api/Endpoint<b>? Code = 0123456789</b>).

   ![Yeni bir API Bağlayıcısı yapılandırma](./media/add-api-connector/api-connector-config.png)
8. **Kaydet**’i seçin.

## <a name="the-request-sent-to-your-api"></a>API 'nize gönderilen istek
Bir API Bağlayıcısı, bir JSON gövdesinde anahtar-değer çiftleri olarak Kullanıcı öznitelikleri (' talepler ') gönderen bir **http post** isteği olarak yürütülür. Öznitelikler, [Microsoft Graph](https://docs.microsoft.com/graph/api/resources/user#properties) Kullanıcı özelliklerine benzer şekilde serileştirilir. 

**Örnek istek**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

İstekte yalnızca Kullanıcı özellikleri ve **Azure AD B2C**  >  **Kullanıcı öznitelikleri** deneyiminde listelenen özel öznitelikler kullanılabilir.

Özel öznitelikler dizindeki **extension_ \<extensions-app-id> _CustomAttribute**  biçiminde bulunur. API 'nizin bu aynı serileştirilmiş biçimde talepler alması beklenir. Özel öznitelikler hakkında daha fazla bilgi için, bkz. [Azure Active Directory B2C özel öznitelikleri tanımlama](user-flow-custom-attributes.md).

Ayrıca, **Kullanıcı arabirimi yerel ayarları (' ui_locales ')** talebi tüm isteklerde varsayılan olarak gönderilir. Bu, bir kullanıcının kendi cihazında yapılandırılan, uluslararası yanıtları döndürmek için API tarafından kullanılabilecek olan yerel ayarları sağlar.

> [!IMPORTANT]
> Bir talebin API uç noktası çağrıldığında bir değeri yoksa, talep API 'ye gönderilmez. API 'niz istek içinde olmayan bir talebi açıkça denetlemek ve işlemek için tasarlanmalıdır.

> [!TIP] 
> [**kimlikler (' kimlikler ')**](https://docs.microsoft.com/graph/api/resources/objectidentity) ve **e-posta adresi (' e-posta ')** talepleri, kiracınızda bir hesabı olmadan önce BIR kullanıcıyı tanımlamak için API 'niz tarafından kullanılabilir. 

## <a name="enable-the-api-connector-in-a-user-flow"></a>Kullanıcı akışında API bağlayıcısını etkinleştirme

Kaydolma Kullanıcı akışına bir API Bağlayıcısı eklemek için bu adımları izleyin.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Azure hizmetleri**altında **Azure AD B2C**' yi seçin.
4. **Kullanıcı akışları**' nı seçin ve ardından API bağlayıcısını eklemek istediğiniz kullanıcı akışını seçin.
5. **API bağlayıcıları**' nı seçin ve ardından Kullanıcı akışında aşağıdaki adımlarda ÇAĞıRMAK istediğiniz API uç noktalarını seçin:

   - **Bir kimlik sağlayıcısıyla oturum açtıktan sonra**
   - **Kullanıcı oluşturmadan önce**

   ![Kullanıcı akışına API ekleme](./media/add-api-connector/api-connectors-user-flow-select.png)

6. **Kaydet**’i seçin.

## <a name="after-signing-in-with-an-identity-provider"></a>Bir kimlik sağlayıcısıyla oturum açtıktan sonra

Kaydolma işleminde bu adımdaki bir API Bağlayıcısı, Kullanıcı kimlik sağlayıcısıyla kimlik doğrulamasından sonra (Google, Facebook, & Azure AD) hemen çağrılır. Bu adım, kullanıcıya kullanıcı özniteliklerinin toplanması için sunulan form olan **_öznitelik koleksiyonu sayfasından_* önce gelir. Bir kullanıcı yerel hesapla kayıt alıyorsa bu adım çağrılmaz.

### <a name="example-request-sent-to-the-api-at-this-step"></a>Bu adımda API 'ye gönderilen örnek isteği
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ 
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "lastName":"Smith",
 "ui_locales":"en-US"
}
```

API 'ye gönderilen tam talepler, kimlik sağlayıcısı tarafından hangi bilgilerin sağlandığını bağlıdır. ' e-posta ' her zaman gönderilir.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Bu adımda Web API 'sinden beklenen yanıt türleri

Web API 'SI, bir Kullanıcı akışı sırasında Azure AD 'den bir HTTP isteği aldığında, bu yanıtları döndürebilir:

- Devamlılık yanıtı
- Engelleme yanıtı

#### <a name="continuation-response"></a>Devamlılık yanıtı

Devamlılık yanıtı, Kullanıcı akışının sonraki adıma devam etmesi gerektiğini belirtir: öznitelik koleksiyonu sayfası.

Devamlılık yanıtında, API talepleri döndürebilir. API tarafından bir talep döndürülürse talep şunları yapar:

- Öznitelik koleksiyonu sayfasındaki giriş alanını önceden doldurur.

[Devamlılık yanıtı](#example-of-a-continuation-response)örneğine bakın.

#### <a name="blocking-response"></a>Engelleme yanıtı

Engelleme yanıtı Kullanıcı akışından çıkar. Kullanıcıya bir engelleme sayfası görüntüleyerek Kullanıcı akışının devamlılığını durdurmak için özellikle API tarafından verilebilir. Engelleme sayfası `userMessage` API tarafından sunulan ' i görüntüler.

[Engelleme yanıtı](#example-of-a-blocking-response)örneğine bakın.

## <a name="before-creating-the-user"></a>Kullanıcı oluşturmadan önce

Kaydolma işleminde bu adımda bulunan bir API Bağlayıcısı, varsa öznitelik toplama sayfasından sonra çağrılır. Bu adım, Kullanıcı hesabı oluşturulmadan önce her zaman çağrılır.

<!-- The following are examples of scenarios you might enable at this point during sign-up: -->
<!-- 
- Validate user input data and ask a user to resubmit data.
- Block a user sign-up based on data entered by the user.
- Perform identity verification.
- Query external systems for existing data about the user and overwrite the user-provided value. -->

### <a name="example-request-sent-to-the-api-at-this-step"></a>Bu adımda API 'ye gönderilen örnek isteği

```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "givenName":"John",
 "surname":"Smith",
 "jobTitle":"Supplier",
 "streetAddress":"1000 Microsoft Way",
 "city":"Seattle",
 "postalCode": "12345",
 "state":"Washington",
 "country":"United States",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```
API 'ye gönderilen tam talepler, kullanıcıdan hangi bilgilerin toplandığına veya kimlik sağlayıcısı tarafından sağlandığına bağlıdır.

### <a name="expected-response-types-from-the-web-api-at-this-step"></a>Bu adımda Web API 'sinden beklenen yanıt türleri

Web API 'SI, bir Kullanıcı akışı sırasında Azure AD 'den bir HTTP isteği aldığında, bu yanıtları döndürebilir:

- Devamlılık yanıtı
- Engelleme yanıtı
- Doğrulama yanıtı

#### <a name="continuation-response"></a>Devamlılık yanıtı

Devamlılık yanıtı, Kullanıcı akışının bir sonraki adıma devam etmesi gerektiğini gösterir: kullanıcıyı dizinde oluşturun.

Devamlılık yanıtında, API talepleri döndürebilir. API tarafından bir talep döndürülürse talep şunları yapar:

- Öznitelik koleksiyonu sayfasından talebe zaten atanmış olan tüm değerleri geçersiz kılar.

[Devamlılık yanıtı](#example-of-a-continuation-response)örneğine bakın.

#### <a name="blocking-response"></a>Engelleme yanıtı
Engelleme yanıtı Kullanıcı akışından çıkar. Kullanıcıya bir engelleme sayfası görüntüleyerek Kullanıcı akışının devamlılığını durdurmak için özellikle API tarafından verilebilir. Engelleme sayfası `userMessage` API tarafından sunulan ' i görüntüler.

[Engelleme yanıtı](#example-of-a-blocking-response)örneğine bakın.

### <a name="validation-error-response"></a>Doğrulama-hata yanıtı
 API bir doğrulama hata yanıtıyla yanıt verdiğinde, Kullanıcı akışı öznitelik toplama sayfasında kalır ve `userMessage` kullanıcıya görüntülenir. Kullanıcı daha sonra formu düzenleyip yeniden gönderebilir. Bu tür bir yanıt, giriş doğrulaması için kullanılabilir.

[Doğrulama hata yanıtı](#example-of-a-validation-error-response)örneğine bakın.

## <a name="example-responses"></a>Örnek yanıtlar

### <a name="example-of-a-continuation-response"></a>Devamlılık yanıtı örneği

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue",
    "postalCode": "12349", // return claim
    "extension_<extensions-app-id>_CustomAttribute": "value" // return claim
}
```

| Parametre                                          | Tür              | Gerekli | Açıklama                                                                                                                                                                                                                                                                            |
| -------------------------------------------------- | ----------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sürüm                                            | Dize            | Yes      | API sürümü.                                                                                                                                                                                                                                                                |
| eylem                                             | Dize            | Yes      | Değer olmalıdır `Continue` .                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Hayır       | Döndürülen değerler, bir kullanıcıdan toplanan değerlerin üzerine yazabilir. Ayrıca, bir _ * uygulama talebi * * olarak seçilirse belirtece da döndürülebilecek.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Hayır       | Talebin içermesi gerekmez `_<extensions-app-id>_` . Döndürülen değerler, bir kullanıcıdan toplanan değerlerin üzerine yazabilir. Ayrıca, bir **uygulama talebi**olarak seçilirse belirtece de döndürülebilir.  |

### <a name="example-of-a-blocking-response"></a>Engelleme yanıtı örneği

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
}

```

| Parametre   | Tür   | Gerekli | Açıklama                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| sürüm     | Dize | Yes      | API sürümü.                                                    |
| eylem      | Dize | Yes      | Değer olmalıdır `ShowBlockPage`                                              |
| userMessage | Dize | Yes      | Kullanıcıya görüntülenecek ileti.                                            |

**Engelleyici bir Yanıt ile son kullanıcı deneyimi**

![Örnek engelleme sayfası](./media/add-api-connector/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Doğrulama hatası yanıt örneği



```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code."
}
```

| Parametre   | Tür    | Gerekli | Açıklama                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| sürüm     | Dize  | Yes      | API sürümü.                                                    |
| eylem      | Dize  | Yes      | Değer olmalıdır `ValidationError` .                                           |
| durum      | Tamsayı | Yes      | `400`Bir ValidationError yanıtı için değer olmalıdır.                        |
| userMessage | Dize  | Yes      | Kullanıcıya görüntülenecek ileti.                                            |

> [!NOTE]
> Yanıt gövdesinde "durum" değerine ek olarak HTTP durum kodu "400" olmalıdır.

**Doğrulama hatası yanıtıyla Son Kullanıcı deneyimi**

![Örnek doğrulama sayfası](./media/add-api-connector/validation-error-postal-code.png)


## <a name="best-practices-and-how-to-troubleshoot"></a>En iyi uygulamalar ve sorun giderme

### <a name="using-serverless-cloud-functions"></a>Sunucusuz bulut işlevlerini kullanma
Azure Işlevlerinde HTTP Tetikleyicileri gibi sunucusuz işlevler, API Bağlayıcısı ile kullanmak üzere API uç noktaları oluşturma basit bir yol sağlar. [Örneğin](code-samples.md#api-connectors), doğrulama mantığını gerçekleştirmek ve belirli e-posta etki alanları için oturum açma işlemleri kısıtlamak gibi sunucusuz bulut işlevini kullanabilirsiniz. Sunucusuz bulut işlevi ayrıca daha karmaşık senaryolar için diğer Web API 'Lerini, Kullanıcı depolarını ve diğer bulut hizmetlerini çağırıp çağırabilir.

### <a name="best-practices"></a>En iyi uygulamalar
Aşağıdakileri doğrulayın:
* API 'niz, yukarıda özetlenen API isteği ve yanıt sözleşmelerini takip eden bir. 
* API bağlayıcısının **uç nokta URL 'si** doğru API uç noktasını işaret eder.
* API 'niz alınan taleplerin null değerlerini açıkça denetler.
* Akıcı bir kullanıcı deneyimi sağlamak için API 'niz mümkün olduğunca çabuk yanıt verir.
    * Sunucusuz bir işlev veya ölçeklenebilir Web hizmeti kullanıyorsanız, API 'YI "uyanık" veya "normal" olarak tutan bir barındırma planı kullanın. Üretimde. Azure Işlevleri için [Premium planı](../azure-functions/functions-scale.md) kullanmanız önerilir


### <a name="use-logging"></a>Günlüğe kaydetmeyi kullanma
Genel olarak, API 'nizi beklenmedik hata kodları, özel durumlar ve düşük performans için izlemek üzere [Application Insights](../azure-functions/functions-monitoring.md)gıbı Web API hizmetiniz tarafından etkinleştirilen günlük araçlarını kullanmak yararlı olacaktır.
* HTTP 200 veya 400 olmayan HTTP durum kodlarını izleyin.
* 401 veya 403 HTTP durum kodu genellikle kimlik doğrulamalarınız ile ilgili bir sorun olduğunu gösterir. API 'nin kimlik doğrulama katmanını ve API Bağlayıcısı 'nda karşılık gelen yapılandırmayı iki kez kontrol edin.
* Gerekirse geliştirmede daha Agresif günlük düzeyi (örn. "Trace" veya "Debug") kullanın.
* API 'nizi uzun yanıt süreleri için izleyin.

## <a name="next-steps"></a>Sonraki adımlar
<!-- - Learn how to [add a custom approval workflow to sign-up](add-approvals.md) -->
- [Örneklerimizi](code-samples.md#api-connectors)kullanmaya başlayın.
