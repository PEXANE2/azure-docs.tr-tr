---
title: Self Servis kaydolma akışlarına API bağlayıcıları ekleme-Azure AD
description: Bir Web API 'sini Kullanıcı akışında kullanılacak şekilde yapılandırın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: article
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f241fd038d0d7309d8e1e5578dd77f950261b68
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88165184"
---
# <a name="add-an-api-connector-to-a-user-flow"></a>Kullanıcı akışına API Bağlayıcısı ekleme

Bir [API bağlayıcısını](api-connectors-overview.md)kullanmak IÇIN önce API bağlayıcısını oluşturun ve ardından Kullanıcı akışında etkinleştirin.

## <a name="create-an-api-connector"></a>API Bağlayıcısı oluşturma

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Tüm API bağlayıcıları (Önizleme)** öğesini seçin ve ardından **yeni API Bağlayıcısı**' nı seçin.

   ![Yeni API Bağlayıcısı ekleme](./media/self-service-sign-up-add-api-connector/api-connector-new.png)

5. Çağrı için bir görünen ad belirtin. Örneğin, **onay durumunu kontrol edin**.
6. API çağrısının **uç nokta URL 'sini** sağlayın.
7. API için kimlik doğrulama bilgilerini sağlayın.

   - Şu anda yalnızca temel kimlik doğrulaması destekleniyor. Geliştirme amacıyla temel kimlik doğrulaması olmadan bir API kullanmak istiyorsanız, API 'nizin yoksaymasına yönelik bir kukla **Kullanıcı adı** ve **parola** girmeniz yeterlidir. API anahtarı olan bir Azure Işleviyle birlikte kullanmak için, kodu **uç nokta URL 'sine** bir sorgu parametresi olarak ekleyebilirsiniz (örneğin, https []() ://contoso.azurewebsites.net/api/Endpoint<b>? Code = 0123456789</b>).

   ![Yeni API Bağlayıcısı ekleme](./media/self-service-sign-up-add-api-connector/api-connector-config.png)
8. **Kaydet**’i seçin.

> [!IMPORTANT]
> Daha önce, API 'ye hangi kullanıcı özniteliklerinin gönderileceğini (' gönderilen talepler ') ve API 'den hangi kullanıcı özniteliklerinin kabul edeceğini (' alma talepleri ') yapılandırmanız gerekiyordu. Artık, bir değer varsa ve bir ' devamlılık ' yanıtında API tarafından herhangi bir kullanıcı özniteliği döndürülebilecek tüm Kullanıcı öznitelikleri varsayılan olarak gönderilir.

## <a name="the-request-sent-to-your-api"></a>API 'nize gönderilen istek
Bir API Bağlayıcısı, bir JSON gövdesinde anahtar-değer çiftleri olarak Kullanıcı öznitelikleri (' talepler ') gönderen bir **http post** isteği olarak yürütülür. Öznitelikler, [Microsoft Graph](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0#properties) Kullanıcı özelliklerine benzer şekilde serileştirilir. 

**Örnek istek**
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
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

Yalnızca **Azure Active Directory**  >  **dış kimlikler**  >  **Özel Kullanıcı öznitelikleri** deneyiminde listelenen kullanıcı özellikleri ve özel öznitelikler, istekte gönderilebilir.

Özel öznitelikler dizindeki **extension_ \<extensions-app-id> _AttributeName** biçiminde bulunur. API 'nizin bu aynı serileştirilmiş biçimde talepler alması beklenir. Özel öznitelikler hakkında daha fazla bilgi için bkz. [self servis kaydolma akışları için özel öznitelikler tanımlama](user-flow-add-custom-attributes.md).

Ayrıca, **Kullanıcı arabirimi yerel ayarları (' ui_locales ')** talebi tüm isteklerde varsayılan olarak gönderilir. Bu, bir kullanıcının kendi cihazında yapılandırılan, uluslararası yanıtları döndürmek için API tarafından kullanılabilecek olan yerel ayarları sağlar.

> [!IMPORTANT]
> Gönderilecek bir talebin API uç noktası çağrıldığında bir değeri yoksa, talep API 'ye gönderilmez. API 'niz, beklediği değeri açıkça denetleyecek şekilde tasarlanmalıdır.

> [!TIP] 
> [**kimlikler (' kimlikler ')**](https://docs.microsoft.com/graph/api/resources/objectidentity?view=graph-rest-1.0) ve **e-posta adresi (' e-posta ')** talepleri, kiracınızda bir hesabı olmadan önce BIR kullanıcıyı tanımlamak için API 'niz tarafından kullanılabilir. ' Kimlikler ' talebi, Kullanıcı Google veya Facebook gibi bir kimlik sağlayıcısı ile kimlik doğrulaması yapıldığında gönderilir. ' e-posta ' her zaman gönderilir.

## <a name="enable-the-api-connector-in-a-user-flow"></a>Kullanıcı akışında API bağlayıcısını etkinleştirme

Self Servis kaydolma Kullanıcı akışına bir API Bağlayıcısı eklemek için bu adımları izleyin.

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Kullanıcı akışları ' nı (Önizleme)** seçin ve ardından API bağlayıcısını eklemek istediğiniz kullanıcı akışını seçin.
5. **API bağlayıcıları**' nı seçin ve ardından Kullanıcı akışında aşağıdaki adımlarda ÇAĞıRMAK istediğiniz API uç noktalarını seçin:

   - **Bir kimlik sağlayıcısıyla oturum açtıktan sonra**
   - **Kullanıcı oluşturmadan önce**

   ![Kullanıcı akışına API ekleme](./media/self-service-sign-up-add-api-connector/api-connectors-user-flow-select.png)

6. **Kaydet**’i seçin.

## <a name="after-signing-in-with-an-identity-provider"></a>Bir kimlik sağlayıcısıyla oturum açtıktan sonra

Kaydolma işleminde bu adımdaki bir API Bağlayıcısı, Kullanıcı kimlik sağlayıcısıyla (Google, Facebook, Azure AD) kimliğini doğruladıktan hemen sonra çağrılır. Bu adım, kullanıcı özniteliklerinin toplanması için kullanıcıya sunulan form olan ***öznitelik koleksiyonu sayfasından***önce gelir. 

<!-- The following are examples of API connector scenarios you may enable at this step:
- Use the email or federated identity that the user provided to look up claims in an existing system. Return these claims from the existing system, pre-fill the attribute collection page, and make them available to return in the token.
- Validate whether the user is included in an allow or deny list, and control whether they can continue with the sign-up flow. -->

### <a name="example-request-sent-to-the-api-at-this-step"></a>Bu adımda API 'ye gönderilen örnek isteği
```http
POST <API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "identities": [ //Sent for Google and Facebook identity providers
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

Kaydolma işleminde bu adımda bulunan bir API Bağlayıcısı, varsa öznitelik toplama sayfasından sonra çağrılır. Bu adım, Azure AD 'de bir kullanıcı hesabı oluşturulmadan önce her zaman çağrılır. 

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
 "identities": [ //Sent for Google and Facebook identity providers
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
| sürüm                                            | Dize            | Evet      | API sürümü.                                                                                                                                                                                                                                                                |
| eylem                                             | Dize            | Evet      | Değer olmalıdır `Continue` .                                                                                                                                                                                                                                                              |
| \<builtInUserAttribute>                            | \<attribute-type> | Hayır       | Bir Kullanıcı akışı için API Bağlayıcısı yapılandırmasında ve **Kullanıcı özniteliklerinde** **alma talebi** olarak seçilirse, değerler dizinde depolanabilir. Bir **uygulama talebi**olarak seçilirse, belirteçte değerler döndürülür.                                              |
| \<extension\_{extensions-app-id}\_CustomAttribute> | \<attribute-type> | Hayır       | Döndürülen talebin içermesi gerekmez `_<extensions-app-id>_` . Bir Kullanıcı akışı için API Bağlayıcısı yapılandırmasında ve **Kullanıcı özniteliğinde** **alma talebi** olarak seçilirse değerler dizinde depolanır. Özel öznitelikler belirtece geri gönderilemez. |

### <a name="example-of-a-blocking-response"></a>Engelleme yanıtı örneği

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "There was a problem with your request. You are not able to sign up at this time.",
    "code": "CONTOSO-BLOCK-00"
}

```

| Parametre   | Tür   | Gerekli | Açıklama                                                                |
| ----------- | ------ | -------- | -------------------------------------------------------------------------- |
| sürüm     | Dize | Evet      | API sürümü.                                                    |
| eylem      | Dize | Evet      | Değer olmalıdır`ShowBlockPage`                                              |
| userMessage | Dize | Evet      | Kullanıcıya görüntülenecek ileti.                                            |
| kod        | Dize | Hayır       | Hata kodu. Hata ayıklama amacıyla kullanılabilir. Kullanıcıya gösterilmez. |

**Engelleyici bir Yanıt ile son kullanıcı deneyimi**

![Örnek engelleme sayfası](./media/api-connectors-overview/blocking-page-response.png)

### <a name="example-of-a-validation-error-response"></a>Doğrulama hatası yanıt örneği

```http
HTTP/1.1 400 Bad Request
Content-type: application/json

{
    "version": "1.0.0",
    "status": 400,
    "action": "ValidationError",
    "userMessage": "Please enter a valid Postal Code.",
    "code": "CONTOSO-VALIDATION-00"
}
```

| Parametre   | Tür    | Gerekli | Açıklama                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| sürüm     | Dize  | Evet      | API sürümü.                                                    |
| eylem      | Dize  | Evet      | Değer olmalıdır `ValidationError` .                                           |
| durum      | Tamsayı | Evet      | `400`Bir ValidationError yanıtı için değer olmalıdır.                        |
| userMessage | Dize  | Evet      | Kullanıcıya görüntülenecek ileti.                                            |
| kod        | Dize  | Hayır       | Hata kodu. Hata ayıklama amacıyla kullanılabilir. Kullanıcıya gösterilmez. |

**Doğrulama hatası yanıtıyla Son Kullanıcı deneyimi**

![Örnek doğrulama sayfası](./media/api-connectors-overview/validation-error-postal-code.png)

## <a name="using-azure-functions"></a>Azure İşlevlerini kullanma
Azure Işlevleri 'ndeki bir HTTP tetikleyicisini, API Bağlayıcısı ile kullanmak üzere bir API uç noktası oluşturmak için basit bir yöntem olarak kullanabilirsiniz. [Örneğin](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts), doğrulama mantığını gerçekleştirmek ve kayıt pencerelerini belirli etki alanlarına kısıtlamak Için Azure işlevini kullanın. Ayrıca, kapsamlı senaryolar için Azure Işlevinizden diğer Web API 'Lerini, Kullanıcı depolarını ve diğer bulut hizmetlerini çağırıp çağırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

<!-- - Learn [where you can enable an API connector](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow) -->
- [Self servis kaydolma 'ya özel bir onay iş akışı eklemeyi](self-service-sign-up-add-approvals.md) öğrenin
- [Azure işlevi hızlı başlangıç örneklerimize](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts)başlayın.
<!-- - Learn how to [use API connectors to verify a user identity](code-samples-self-service-sign-up.md#identity-verification) -->
