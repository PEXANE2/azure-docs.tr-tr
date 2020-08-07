---
title: Self Servis kaydolma akışlarına API bağlayıcıları ekleme-Azure AD
description: Bir Web API 'sini Kullanıcı akışında kullanılacak şekilde yapılandırın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88270d51bf50b2b175d9d8761685a8a2a8ae19b1
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87910044"
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

8. API 'ye göndermek istediğiniz talepleri seçin.
9. API 'den geri almayı planladığınız talepler ' ı seçin.

   <!-- ![Set API connector claims](./media/self-service-sign-up-add-api-connector/api-connector-claims.png) -->

10. **Kaydet**’i seçin.

### <a name="selection-of-claims-to-send-and-claims-to-receive"></a>' Gönderilen talepler ' ve ' alacak talepler ' seçimi
> [!IMPORTANT]
> Aşağıda gösterildiği gibi, varsayılan olarak seçilen tüm talepleri görebilirsiniz. Tüm API bağlayıcıları bu şekilde davranacak şekilde güncelleştirilecektir. API 'niz tüm kullanılabilir talepleri alacak ve API Bağlayıcısı tanımında yapılandırmadan önce desteklenen talepleri geri gönderebilirler. 

![API Bağlayıcısı taleplerini ayarlama](./media/self-service-sign-up-add-api-connector/api-connector-claims-new.png)


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

[Kullanıcı akışında BIR API bağlayıcısını etkinleştirebileceğinizi](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow)öğrenin.

## <a name="request-sent-to-the-api"></a>API 'ye gönderilen istek

Bir API Bağlayıcısı, seçili talepleri bir JSON gövdesinde anahtar-değer çiftleri olarak göndererek bir HTTP POST isteği olarak kabul eder. Yanıtın de HTTP üstbilgisi olmalıdır `Content-Type: application/json` . Öznitelikler, Microsoft Graph Kullanıcı özniteliklerine benzer şekilde serileştirilir. <!--# TODO: Add link to MS Graph or create separate reference.-->

### <a name="example-request"></a>Örnek istek

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
 "postalCode": "33971",
 "extension_<extensions-app-id>_CustomAttribute1": "custom attribute value",
 "extension_<extensions-app-id>_CustomAttribute2": "custom attribute value",
 "ui_locales":"en-US"
}
```

**Kullanıcı arabirimi yerel ayarları (' ui_locales ')** talebi, tüm isteklerde varsayılan olarak gönderilir. Bu, bir kullanıcının yerel ayarlarını sağlar ve bu, API tarafından uluslararası ve uluslararası yanıtları döndürmek için kullanılabilir. API Yapılandırma bölmesinde görünmez.

Gönderilecek bir talebin API uç noktası çağrıldığında bir değeri yoksa, talep API 'ye gönderilmez.

**Extension_ \<extensions-app-id> _AttributeName** biçimi kullanılarak kullanıcı için özel öznitelikler oluşturulabilir. API 'nizin bu aynı serileştirilmiş biçimde talepler alması beklenir. API 'niz, veya olmadan talepler döndürebilir `<extensions-app-id>` . Özel öznitelikler hakkında daha fazla bilgi için bkz. [self servis kaydolma akışları için özel öznitelikler tanımlama](user-flow-add-custom-attributes.md).

> [!TIP] 
> [**kimlikler (' kimlikler ')**](https://docs.microsoft.com/graph/api/resources/objectidentity?view=graph-rest-1.0) ve **e-posta adresi (' e-posta ')** talepleri, kiracınızda bir hesaba girmeden önce bir kullanıcıyı tanımlamak için kullanılabilir. Bir kullanıcı Google veya Facebook ile kimlik doğrulaması yapıldığında ve ' e-posta ' her zaman gönderiliyorsa ' kimlikler ' talebi gönderilir.

## <a name="expected-response-types-from-the-web-api"></a>Web API 'sinden beklenen yanıt türleri

Web API 'SI, bir Kullanıcı akışı sırasında Azure AD 'den bir HTTP isteği aldığında, bu yanıtları döndürebilir:

- [Devamlılık yanıtı](#continuation-response)
- [Engelleme yanıtı](#blocking-response)
- [Doğrulama-hata yanıtı](#validation-error-response)

### <a name="continuation-response"></a>Devamlılık yanıtı

Devamlılık yanıtı, Kullanıcı akışının bir sonraki adıma devam etmesi gerektiğini gösterir. Devamlılık yanıtında, API talepleri döndürebilir.

Bir talep API tarafından döndürülür ve **alma talebi**olarak seçilirse, talep aşağıdakileri yapar:

- API Bağlayıcısı sayfa sunulmadan önce çağrılırsa, öznitelik koleksiyonu sayfasındaki ön doldurma girişi alanları.
- Talebe zaten atanmış olan tüm değerleri geçersiz kılar.
- Daha önce null ise talebe bir değer atar.

#### <a name="example-of-a-continuation-response"></a>Devamlılık yanıtı örneği

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

### <a name="blocking-response"></a>Engelleme yanıtı

Engelleme yanıtı Kullanıcı akışından çıkar. Kullanıcıya bir engelleme sayfası görüntüleyerek Kullanıcı akışının devamlılığını durdurmak için özellikle API tarafından verilebilir. Engelleme sayfası `userMessage` API tarafından sunulan ' i görüntüler.

Engelleme yanıtı örneği:

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

#### <a name="end-user-experience-with-a-blocking-response"></a>Engelleyici bir Yanıt ile son kullanıcı deneyimi

![Örnek engelleme sayfası](./media/api-connectors-overview/blocking-page-response.png)

### <a name="validation-error-response"></a>Doğrulama-hata yanıtı

Öznitelik koleksiyonu sayfasından sonra çağrılan bir API çağrısı, doğrulama hata yanıtı döndürebilir. Bunu yaparken, Kullanıcı akışı öznitelik toplama sayfasında kalır ve `userMessage` kullanıcıya görüntülenir. Kullanıcı daha sonra formu düzenleyip yeniden gönderebilir. Bu tür bir yanıt, giriş doğrulaması için kullanılabilir.

#### <a name="example-of-a-validation-error-response"></a>Doğrulama hatası yanıt örneği

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

#### <a name="end-user-experience-with-a-validation-error-response"></a>Doğrulama hatası yanıtıyla Son Kullanıcı deneyimi

![Örnek doğrulama sayfası](./media/api-connectors-overview/validation-error-postal-code.png)

### <a name="integration-with-azure-functions"></a>Azure İşlevleriyle tümleştirme
Azure Işlevleri 'nde, API Bağlayıcısı ile kullanmak üzere bir API oluşturmak için basit bir yol olarak bir HTTP tetikleyicisi kullanabilirsiniz. [Örneğin](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts), doğrulama mantığını gerçekleştirmek ve kayıt pencerelerini belirli etki alanlarına kısıtlamak Için Azure işlevini kullanın. Ayrıca diğer Web API 'Lerini, Kullanıcı depolarını ve diğer bulut hizmetlerini çağırıp çağırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

<!-- - Learn [where you can enable an API connector](api-connectors-overview.md#where-you-can-enable-an-api-connector-in-a-user-flow) -->
- [Self servis kaydolma 'ya özel bir onay iş akışı eklemeyi](self-service-sign-up-add-approvals.md) öğrenin
- [Azure işlevi hızlı başlangıç örneklerimize](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts)başlayın.
<!-- - Learn how to [use API connectors to verify a user identity](code-samples-self-service-sign-up.md#identity-verification) -->
