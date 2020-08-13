---
title: Self Servis kaydolma akışlarına özel onaylar ekleme-Azure AD
description: Dış kimliklerinizde özel onay iş akışları için API bağlayıcıları ekleme-Azure Active Directory (Azure AD)
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
ms.openlocfilehash: d664d7cd169593924917bb02a0220e4047eb0cdb
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88165267"
---
# <a name="add-a-custom-approval-workflow-to-self-service-sign-up"></a>Self Servis kaydolma 'ya özel bir onay iş akışı ekleme

[API bağlayıcıları](api-connectors-overview.md)sayesinde, kiracınızda hangi Konuk Kullanıcı hesaplarının oluşturulduğunu yönetebilmeniz için self servis kaydolma ile kendi özel onay iş akışlarınızla tümleştirebilirsiniz.

Bu makale, bir onay sistemiyle nasıl tümleştirileceğini gösteren bir örnek sağlar. Bu örnekte, self servis kaydolma Kullanıcı akışı, oturum açma işlemi sırasında kullanıcı verilerini toplar ve bunu onay sisteminize geçirir. Ardından, onay sistemi şunları yapabilir:

- Kullanıcıyı otomatik olarak onaylayın ve Azure AD 'nin Kullanıcı hesabını oluşturmasına izin verin.
- El ile inceleme tetikleyin. İstek onaylanırsa, onay sistemi kullanıcı hesabını sağlamak için Microsoft Graph kullanır. Onay sistemi, kullanıcıya hesaplarının oluşturulduğunu da bildirir.

## <a name="register-an-application-for-your-approval-system"></a>Onay sisteminiz için bir uygulamayı kaydetme

Azure AD 'de kimlik doğrulaması yapabilmek ve Kullanıcı oluşturma iznine sahip olmak için onay sisteminizi Azure AD kiracınızda bir uygulama olarak kaydetmeniz gerekir. [Microsoft Graph için kimlik doğrulama ve yetkilendirme temelleri](https://docs.microsoft.com/graph/auth/auth-concepts)hakkında daha fazla bilgi edinin.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol menüden **uygulama kayıtları**' yi seçin ve ardından **Yeni kayıt**' ı seçin.
4. Uygulama için bir **ad** girin, örneğin _kaydolma onayları_.

   <!-- ![Register an application for the approval system](./self-service-sign-up-add-approvals/approvals/register-an-approvals-application.png) -->

5. **Kaydet**’i seçin. Diğer alanları varsayılan olarak bırakabilirsiniz.

   ![Uygulama sayfası kaydetme](media/self-service-sign-up-add-approvals/register-approvals-app.png)

6. Sol menüdeki **Yönet** ' in altında, **API izinleri**' ni seçin ve **izin Ekle**' yi seçin.
7. **API Izinleri iste** sayfasında, **Microsoft Graph**' yi seçin ve ardından **Uygulama izinleri**' ni seçin.
8. **Izinleri Seç**' ın altında **Kullanıcı**' yı genişletin ve ardından **User. ReadWrite. All** onay kutusunu seçin. Bu izin onay sisteminin onay sonrasında Kullanıcı oluşturmasına izin verir. Ardından **Izin Ekle**' yi seçin.

   ![Uygulama sayfası kaydetme](media/self-service-sign-up-add-approvals/request-api-permissions.png)

9. **API izinleri** sayfasında, **(kiracı adınız) Için yönetici onayı ver**' i seçin ve ardından **Evet**' i seçin.
10. Sol menüdeki **Yönet** ' ın altında **Sertifikalar & gizlilikler**' ı seçin ve ardından **yeni istemci parolası**' nı seçin.
11. Gizlilik için bir **Açıklama** girin, örneğin _onaylar istemci parolası_ve Istemci parolasının **süresinin dolacağı**süreyi seçin. Ardından **Ekle**'yi seçin.
12. İstemci parolasının değerini kopyalayın.

    ![İstemci parolasını onay sisteminde kullanılmak üzere kopyalama](media/self-service-sign-up-add-approvals/client-secret-value-copy.png)

13. Onay sisteminizi, istemci KIMLIĞI ve Azure AD kimlik doğrulaması için oluşturduğunuz **istemci gizli anahtarı** olarak **uygulama kimliğini** kullanacak şekilde yapılandırın.

## <a name="create-the-api-connectors"></a>API bağlayıcıları oluşturma

Daha sonra self servis kaydolma Kullanıcı akışınız için [API bağlayıcıları oluşturacaksınız](self-service-sign-up-add-api-connector.md#create-an-api-connector) . Onay sistemi API 'niz, aşağıda gösterilen örneklerde olduğu gibi iki bağlayıcı ve karşılık gelen uç noktalar gerektirir. Bu API bağlayıcıları şunları yapın:

- Onay **durumunu kontrol edin**. Kullanıcı bir kimlik sağlayıcısı ile oturum açtıktan hemen sonra onay sistemine çağrı gönderin. Bu, kullanıcının var olan onay isteğine sahip olup olmadığını veya zaten reddedildiğini kontrol etmek için bir kimlik sağlayıcısıyla oturum açar. Onay sisteminiz yalnızca otomatik onay kararları alıyorsa, bu API Bağlayıcısı gerekmeyebilir. "Onay durumunu denetleme" API Bağlayıcısı örneği.

  ![Onay durumu API Bağlayıcısı yapılandırmasını denetle](./media/self-service-sign-up-add-approvals/check-approval-status-api-connector-config-alt.png)

- **Onay iste** -Kullanıcı, öznitelik toplama sayfasını tamamladıktan sonra, onay istemek için Kullanıcı hesabı oluşturulmadan önce onay sistemine çağrı gönderin. Onay isteği otomatik olarak verilebilir veya el ile incelenebilir. "Istek onayı" API Bağlayıcısı örneği. 

  ![Onay API Bağlayıcısı yapılandırması iste](./media/self-service-sign-up-add-approvals/create-approval-request-api-connector-config-alt.png)

Bu bağlayıcıları oluşturmak için [API Bağlayıcısı oluşturma](self-service-sign-up-add-api-connector.md#create-an-api-connector)' daki adımları izleyin.

## <a name="enable-the-api-connectors-in-a-user-flow"></a>Kullanıcı akışında API bağlayıcılarını etkinleştirme

Şimdi aşağıdaki adımlarla API bağlayıcılarını self servis kaydolma Kullanıcı akışına ekleyeceksiniz:

1. [Azure portalda](https://portal.azure.com/) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Kullanıcı akışları ' nı (Önizleme)** seçin ve ardından API bağlayıcısını etkinleştirmek istediğiniz kullanıcı akışını seçin.
5. **API bağlayıcıları**' nı seçin ve ardından Kullanıcı akışında aşağıdaki adımlarda ÇAĞıRMAK istediğiniz API uç noktalarını seçin:

   - **Bir kimlik sağlayıcısı ile oturum açtıktan sonra**: onay durumu API bağlayıcısını seçin, örneğin onay _durumunu kontrol edin_.
   - **Kullanıcı oluşturmadan önce**: onay isteği API bağlayıcısını, örneğin _istek onayı_' nı seçin.

   ![Kullanıcı akışına API ekleme](./media/self-service-sign-up-add-approvals/api-connectors-user-flow-api.png)

6. **Kaydet**’i seçin.

## <a name="control-the-sign-up-flow-with-api-responses"></a>API yanıtları ile kaydolma akışını denetleme

Onay sisteminiz, kaydolma akışını denetlemek için çağrıldığında yanıtlarını kullanabilir. 

### <a name="request-and-responses-for-the-check-approval-status-api-connector"></a>"Onay durumunu denetle" API Bağlayıcısı için istek ve yanıtlar

"Onay durumunu denetle" API bağlayıcısının API tarafından alınan istek örneği:

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

#### <a name="continuation-response-for-check-approval-status"></a>"Onay durumunu denetle" için devamlılık yanıtı

**Onay durumunu denetle** API uç noktası şu durumlarda bir devamlılık yanıtı döndürmelidir:

- Kullanıcı daha önce bir onay istemedi.

Devamlılık yanıtı örneği:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

#### <a name="blocking-response-for-check-approval-status"></a>"Onay durumunu denetle" yanıtı engelleniyor

**Onay durumunu denetle** API uç noktası şu durumlarda bir engelleme yanıtı döndürmelidir:

- Kullanıcı onayı beklemede.
- Kullanıcı reddedildi ve tekrar onay istemesine izin verilmiyor.

Yanıtları engelleme örnekleri aşağıda verilmiştir:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your access request is already processing. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-PENDING"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-DENIED"
}
```

### <a name="request-and-responses-for-the-request-approval-api-connector"></a>"Istek onayı" API Bağlayıcısı için istek ve yanıtlar

"Istek onayı" API bağlayıcısının API tarafından alınan bir HTTP isteği örneği:

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

#### <a name="continuation-response-for-request-approval"></a>"Istek onayı" için devamlılık yanıtı

**İstek onayı** API uç noktası şu durumlarda bir devamlılık yanıtı döndürmelidir:

- Kullanıcı **_otomatik olarak onaylanabilir_**.

Devamlılık yanıtı örneği:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

> [!IMPORTANT]
> Devamlılık yanıtı alınmışsa, Azure AD bir kullanıcı hesabı oluşturur ve kullanıcıyı uygulamaya yönlendirir.

#### <a name="blocking-response-for-request-approval"></a>"Onay ıste" yanıtı engelleniyor

**İstek onayı** API uç noktası, şu durumlarda bir engelleme yanıtı döndürmelidir:

- Bir Kullanıcı onay isteği oluşturuldu ve artık beklemede.
- Kullanıcı onay isteği otomatik olarak reddedildi.

Yanıtları engelleme örnekleri aşağıda verilmiştir:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your account is now waiting for approval. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-REQUESTED"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-AUTO-DENIED"
}
```

`userMessage`Yanıtta Kullanıcı görüntülenir, örneğin:

![Örnek onay sayfası bekleniyor](./media/self-service-sign-up-add-approvals/approval-pending.png)

## <a name="user-account-creation-after-manual-approval"></a>El ile onayladıktan sonra Kullanıcı hesabı oluşturma

El ile onay aldıktan sonra, özel onay sistemi [Microsoft Graph](https://docs.microsoft.com/graph/use-the-api)kullanarak bir [Kullanıcı](https://docs.microsoft.com/graph/azuread-users-concept-overview) hesabı oluşturur. Onay sisteminizin Kullanıcı hesabını sağlama yolu, Kullanıcı tarafından kullanılan kimlik sağlayıcısına bağlıdır.

### <a name="for-a-federated-google-or-facebook-user"></a>Federe bir Google veya Facebook kullanıcısı için

> [!IMPORTANT]
> Onay sistemi `identities` , `identities[0]` `identities[0].issuer` `identities[0].issuer` Bu yöntemi kullanmak için açıkça bu ve ' Facebook ' veya ' Google ' değerine eşit olup olmadığını denetlemelidir.

Kullanıcılarınız bir Google veya Facebook hesabıyla oturum açmışsa, [Kullanıcı oluşturma API](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0&tabs=http)'sini kullanabilirsiniz.

1. Onay sistemi kullanımları Kullanıcı akışından gelen HTTP isteğini alır.

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@outlook.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. Onay sistemi bir kullanıcı hesabı oluşturmak için Microsoft Graph kullanır.

```http
POST https://graph.microsoft.com/v1.0/users
Content-type: application/json

{
 "userPrincipalName": "johnsmith_outlook.com#EXT@contoso.onmicrosoft.com",
 "accountEnabled": true,
 "mail": "johnsmith@outlook.com",
 "userType": "Guest",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value"
}
```

| Parametre                                           | Gerekli | Açıklama                                                                                                                                                            |
| --------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| userPrincipalName                                   | Evet      | , `email` API 'ye gönderilen talep alınarak, `@` karakteri ile değiştirerek ve ile önceden bekleyerek oluşturulabilir `_` `#EXT@<tenant-name>.onmicrosoft.com` . |
| accountEnabled                                      | Evet      | Olarak ayarlanmalıdır `true` .                                                                                                                                                 |
| posta                                                | Evet      | `email`API 'ye gönderilen talebe denktir.                                                                                                               |
| userType                                            | Evet      | Olmalıdır `Guest` . Bu kullanıcıyı Konuk Kullanıcı olarak belirler.                                                                                                                 |
| lerinizde                                          | Evet      | Federal kimlik bilgileri.                                                                                                                                    |
| \<otherBuiltInAttribute>                            | Hayır       | , Ve gibi diğer yerleşik öznitelikler `displayName` `city` . Parametre adları, API Bağlayıcısı tarafından gönderilen parametrelerle aynıdır.                            |
| \<extension\_\{extensions-app-id}\_CustomAttribute> | Hayır       | Kullanıcı hakkındaki özel öznitelikler. Parametre adları, API Bağlayıcısı tarafından gönderilen parametrelerle aynıdır.                                                            |

### <a name="for-a-federated-azure-active-directory-user"></a>Federe Azure Active Directory Kullanıcı için

Bir Kullanıcı bir federasyon Azure Active Directory hesabıyla oturum açarsa, kullanıcıyı oluşturmak için [davet API](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-1.0) 'sini ve isteğe bağlı olarak Kullanıcı [güncelleştirme API](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0) 'sini kullanıcıya daha fazla öznitelik atamak için kullanmalısınız.

1. Onay sistemi, Kullanıcı akışından gelen HTTP isteğini alır.

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. Onay sistemi, `email` API Bağlayıcısı tarafından sağlanarak daveti oluşturur.

```http
POST https://graph.microsoft.com/v1.0/invitations
Content-type: application/json

{
    "invitedUserEmailAddress":"johnsmith@fabrikam.onmicrosoft.com",
    "inviteRedirectUrl" : "https://myapp.com"
}
```

Yanıt örneği:

```http
HTTP/1.1 201 OK
Content-type: application/json

{
    ...
    "invitedUser": {
        "id": "<generated-user-guid>"
    }
}
```

3. Onay sistemi, kullanıcının hesabını toplanan Kullanıcı öznitelikleriyle güncelleştirmek için davet edilen kullanıcının KIMLIĞINI kullanır (isteğe bağlı).

```http
PATCH https://graph.microsoft.com/v1.0/users/<generated-user-guid>
Content-type: application/json

{
    "displayName": "John Smith",
    "city": "Redmond",
    "extension_<extensions-app-id>_AttributeName": "custom attribute value"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure işlevi hızlı başlangıç örneklerimize](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts)başlayın.
- [El ile onay örneğiyle Konuk kullanıcılar için self servis kaydolma](code-samples-self-service-sign-up.md#custom-approval-workflows)işlemini kullanıma alın. 
