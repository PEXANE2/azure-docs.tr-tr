---
title: Azure Active Directory uygulama bildirimini anlama
description: Uygulamanın Azure AD kiracısındaki kimlik yapılandırmasını temsil eden ve OAuth yetkilendirme, onay deneyimini ve daha fazlasını kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildiriminin ayrıntılı kapsamı.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/15/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: 9b2368cdd3fbd2699cb3579d922d81ad66efdc91
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84558585"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory uygulama bildirimi

Uygulama bildirimi Microsoft Identity platformunda bir uygulama nesnesinin tüm özniteliklerinin tanımını içerir. Ayrıca uygulama nesnesini güncelleştirmek için bir mekanizma işlevi görür. Uygulama varlığı ve şeması hakkında daha fazla bilgi için [Graph API uygulama varlığı belgelerine](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)bakın.

Bir uygulamanın özniteliklerini Azure portal veya [REST API](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) veya [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications)kullanarak program aracılığıyla yapılandırabilirsiniz. Ancak, bir uygulamanın özniteliğini yapılandırmak için uygulama bildirimini düzenlemeniz gereken bazı senaryolar vardır. Bu senaryolar şunlardır:

* Uygulamayı Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak kaydettiniz, Kullanıcı arabirimindeki desteklenen Microsoft hesaplarını değiştiremezsiniz. Bunun yerine, desteklenen hesap türünü değiştirmek için uygulama bildirimi düzenleyicisini kullanmanız gerekir.
* Uygulamanızın desteklediği izinleri ve rolleri tanımlamanız gerekiyorsa, uygulama bildirimini değiştirmeniz gerekir.

## <a name="configure-the-app-manifest"></a>Uygulama bildirimini yapılandırma

Uygulama bildirimini yapılandırmak için:

1. [Azure Portal](https://portal.azure.com)gidin. **Azure Active Directory** hizmetini arayıp seçin.
1. **Uygulama kayıtları**'nı seçin.
1. Yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Web tabanlı bir bildirim Düzenleyicisi açılır ve bu, portalı içindeki bildirimi düzenlemenize olanak tanır. İsteğe bağlı olarak, bildirimi yerel olarak düzenlemek için **İndir** ' i seçip uygulamanıza yeniden uygulamak Için **karşıya yükle** ' yi kullanabilirsiniz.

## <a name="manifest-reference"></a>Bildirim başvurusu

Bu bölümde uygulama bildiriminde bulunan öznitelikler açıklanmaktadır.

### <a name="id-attribute"></a>ID özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| kimlik | Dize |

Dizindeki uygulamanın benzersiz tanımlayıcısı. Bu KIMLIK, herhangi bir protokol işleminde uygulamayı tanımlamak için kullanılan tanımlayıcı değildir. Dizin sorgularında nesnesine başvurmak için kullanılır.

Örnek:

```json
    "id": "f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd",
```

### <a name="accesstokenacceptedversion-attribute"></a>accessTokenAcceptedVersion özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| accessTokenAcceptedVersion | Null yapılabilir Int32 |

Kaynak tarafından beklenen erişim belirteci sürümünü belirtir. Bu parametre, erişim belirtecini istemek için kullanılan uç noktadan veya istemciden bağımsız olarak üretilen JWT sürümünü ve biçimini değiştirir.

Kullanılan uç nokta, v 1.0 veya v 2.0, istemci tarafından seçilir ve yalnızca id_tokens sürümünü etkiler. Kaynakların, `accesstokenAcceptedVersion` desteklenen erişim belirteci biçimini belirtecek şekilde açıkça yapılandırılması gerekir.

İçin olası değerler `accesstokenAcceptedVersion` 1, 2 veya null. Değer null ise, bu parametre, v 1.0 uç noktasına karşılık gelen varsayılan olarak 1 ' dir.

`signInAudience`İse `AzureADandPersonalMicrosoftAccount` , değer olmalıdır `2` .

Örnek:

```json
    "accessTokenAcceptedVersion": 2,
```

### <a name="addins-attribute"></a>AddIns özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Eklentileri | Koleksiyon |

Bir tüketen hizmetin belirli bağlamlarda uygulama çağırmak için kullanabileceği özel davranışı tanımlar. Örneğin, dosya akışlarını işleyebilen uygulamalar `addIns` özelliği "FileHandler" işlevselliği için ayarlayabilir. Bu parametre, Office 365 gibi hizmetlerin uygulamayı kullanıcının üzerinde çalıştığı bir belge bağlamında çağırmasını sağlar.

Örnek:

```json
    "addIns": [
       {
        "id": "968A844F-7A47-430C-9163-07AE7C31D407",
        "type":" FileHandler",
        "properties": [
           {
              "key": "version",
              "value": "2"
           }
        ]
       }
    ],
```

### <a name="allowpublicclient-attribute"></a>allowPublicClient özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| allowPublicClient | Boole |

Geri dönüş uygulama türünü belirtir. Azure AD, varsayılan olarak, bir uygulama türünü replyUrlsWithType öğesinden algılar. Azure AD 'nin istemci uygulama türünü belirleyeleyemiyorsa bazı senaryolar vardır. Örneğin, bu tür bir senaryo HTTP isteğinin URL yeniden yönlendirmesi olmadan gerçekleştiği [Ropc](https://tools.ietf.org/html/rfc6749#section-4.3) akışsudur. Bu durumlarda, Azure AD, uygulama türünü bu özelliğin değerine göre yorumlayacak. Bu değer true olarak ayarlanırsa, geri dönüş uygulama türü, bir mobil cihazda çalışan yüklü uygulama gibi ortak istemci olarak ayarlanır. Varsayılan değer false 'dur. Bu, geri dönüş uygulama türünün Web uygulaması gibi gizli bir istemci olduğu anlamına gelir.

Örnek:

```json
    "allowPublicClient": false,
```

### <a name="availabletoothertenants-attribute"></a>Availabletootherkiracılar özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Availabletootherkiracılar | Boole |

Uygulama diğer kiracılar ile paylaşılmışsa, true olarak ayarlayın; Aksi takdirde, false.

> [!NOTE]
> Bu öznitelik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. `signInAudience` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

### <a name="appid-attribute"></a>AppID özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| appId | Dize |

Azure AD tarafından bir uygulamaya atanan uygulama için benzersiz tanımlayıcıyı belirtir.

Örnek:

```json
    "appId": "601790de-b632-4f57-9523-ee7cb6ceba95",
```

### <a name="approles-attribute"></a>appRoles özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| appRoles | Koleksiyon |

Bir uygulamanın bildirebilen rollerin koleksiyonunu belirtir. Bu roller kullanıcılara, gruplara veya hizmet sorumlularına atanabilir. Daha fazla örnek ve bilgi için bkz. [uygulamanıza uygulama rolleri ekleme ve bunları belirtece alma](howto-add-app-roles-in-azure-ad-apps.md).

Örnek:

```json
    "appRoles": [
        {
           "allowedMemberTypes": [
               "User"
           ],
           "description": "Read-only access to device information",
           "displayName": "Read Only",
           "id": "601790de-b632-4f57-9523-ee7cb6ceba95",
           "isEnabled": true,
           "value": "ReadOnly"
        }
    ],
```

### <a name="displayname-attribute"></a>displayName özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| displayName | Dize |

Uygulamanın görünen adı.

> [!NOTE]
> Bu öznitelik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. `name` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

### <a name="errorurl-attribute"></a>errorUrl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| errorUrl 'Si | Dize |

Desteklenmez.

### <a name="groupmembershipclaims-attribute"></a>Groupmembershipclaim özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
|Groupmembershipclaim | Dize |

`groups`Uygulamanın beklediği bir kullanıcı veya OAuth 2,0 erişim belirtecinde verilen talebi yapılandırır. Bu özniteliği ayarlamak için aşağıdaki geçerli dize değerlerinden birini kullanın:

- `"None"`
- `"SecurityGroup"`(güvenlik grupları ve Azure AD rolleri için)
- `"All"`(Bu, oturum açan kullanıcının üyesi olduğu tüm güvenlik gruplarını, dağıtım gruplarını ve Azure AD dizin rollerini alır.

Örnek:

```json
    "groupMembershipClaims": "SecurityGroup",
```

### <a name="homepage-attribute"></a>giriş sayfası özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| değiştirir |Dize |

Uygulamanın giriş sayfasının URL 'SI.

> [!NOTE]
> Bu öznitelik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. `signInUrl` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

### <a name="objectid-attribute"></a>ObjectID özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
|Uzantının | Dize |

Dizindeki uygulamanın benzersiz tanımlayıcısı.

Bu yalnızca **uygulama kayıtları (eski)** deneyimde mevcuttur. `id` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

Örnek:

```json
    "objectId": "f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd",
```

### <a name="optionalclaims-attribute"></a>Optionalclaim özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Optionalclaim | Dize |

Bu belirli uygulama için güvenlik belirteci hizmeti tarafından belirteçte döndürülen isteğe bağlı talepler.

Şu anda, hem kişisel hesapları hem de Azure AD 'yi (uygulama kayıt portalı üzerinden kaydedilir) destekleyen uygulamalar isteğe bağlı talepler kullanamaz. Ancak, yalnızca Azure AD için kaydedilmiş uygulamalar, v 2.0 uç noktasını kullanarak, bildirimde istedikleri isteğe bağlı talepleri alabilir. Daha fazla bilgi için bkz. [Isteğe bağlı talepler](active-directory-optional-claims.md).

Örnek:

```json
    "optionalClaims": null,
```



### <a name="identifieruris-attribute"></a>ıdentifieruris özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| ıdentifieruris | Dize dizisi |

Bir Web uygulamasını Azure AD kiracısı içinde benzersiz bir şekilde tanımlayan Kullanıcı tanımlı URI 'ler veya uygulama çok kiracılı ise doğrulanmış bir özel etki alanı içinde.

Örnek:

```json
    "identifierUris": "https://MyRegisteredApp",
```

### <a name="informationalurls-attribute"></a>ınformationalurls özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| ınformationalurls | Dize |

Uygulamanın hizmet koşulları ve gizlilik bildirimi bağlantılarını belirtir. Hizmet koşulları ve gizlilik bildirimi, kullanıcılar tarafından Kullanıcı onay deneyimi aracılığıyla ortaya çıkmış. Daha fazla bilgi için bkz. [nasıl yapılır: kayıtlı Azure AD uygulamaları için hizmet koşulları ve gizlilik bildirimi ekleme](howto-add-terms-of-service-privacy-statement.md).

Örnek:

```json
    "informationalUrls": {
        "termsOfService": "https://MyRegisteredApp/termsofservice",
        "support": "https://MyRegisteredApp/support",
        "privacy": "https://MyRegisteredApp/privacystatement",
        "marketing": "https://MyRegisteredApp/marketing"
    },
```

### <a name="keycredentials-attribute"></a>keyCredentials özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| keyCredentials | Koleksiyon |

Uygulama tarafından atanan kimlik bilgileri, dize tabanlı paylaşılan gizlilikler ve X. 509.440 sertifikalarına yönelik başvuruları tutar. Bu kimlik bilgileri, erişim belirteçleri istenirken kullanılır (uygulama bir kaynak olarak bir istemci olarak hareket etmekzaman).

Örnek:

```json
    "keyCredentials": [
        {
           "customKeyIdentifier":null,
           "endDate":"2018-09-13T00:00:00Z",
           "keyId":"<guid>",
           "startDate":"2017-09-12T00:00:00Z",
           "type":"AsymmetricX509Cert",
           "usage":"Verify",
           "value":null
        }
    ],
```

### <a name="knownclientapplications-attribute"></a>knownClientApplications özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| knownClientApplications | Dize dizisi |

İki bölümden oluşan bir çözümünüz varsa, bir istemci uygulaması ve özel bir Web API uygulaması varsa, bu izni paketleme için kullanılır. İstemci uygulamasının AppID 'sini bu değere girerseniz, kullanıcının istemci uygulamasına yalnızca bir kez onay girmesi gerekir. Azure AD, istemci ile ilgili bir bildirimin Web API 'sine örtülü olarak nasıl katılmadığını bilecektir. Hem istemci hem de Web API 'SI için aynı anda hizmet sorumlularını otomatik olarak sağlayacaktır. Hem istemci hem de Web API uygulaması aynı kiracıda kayıtlı olmalıdır.

Örnek:

```json
    "knownClientApplications": ["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"],
```

### <a name="logourl-attribute"></a>Logo URL özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Logo URL 'si | Dize |

Portalda karşıya yüklenen logoya yönelik CDN URL 'sine işaret eden salt okuma değeri.

Örnek:

```json
    "logoUrl": "https://MyRegisteredAppLogo",
```

### <a name="logouturl-attribute"></a>logoutUrl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| logoutUrl 'Si | Dize |

Uygulamanın oturumunu kapatmak için kullanılacak URL.

Örnek:

```json
    "logoutUrl": "https://MyRegisteredAppLogout",
```

### <a name="name-attribute"></a>ad özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| name | Dize |

Uygulamanın görünen adı.

Örnek:

```json
    "name": "MyRegisteredApp",
```

### <a name="oauth2allowimplicitflow-attribute"></a>oauth2AllowImplicitFlow özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2AllowImplicitFlow | Boole |

Bu Web uygulamasının OAuth 2.0 örtük akış erişim belirteçleri isteyip isteyemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır. Daha fazla bilgi edinmek için `OAuth 2.0 implicit grant flow` İçindekiler tablosuna girip örtük akış hakkındaki konulara bakın.

Örnek:

```json
    "oauth2AllowImplicitFlow": false,
```

### <a name="oauth2allowidtokenimplicitflow-attribute"></a>oauth2AllowIdTokenImplicitFlow özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2AllowIdTokenImplicitFlow | Boole |

Bu Web uygulamasının OAuth 2.0 örtük akış KIMLIĞI belirteçleri isteyip isteyemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır.

Örnek:

```json
    "oauth2AllowIdTokenImplicitFlow": false,
```

### <a name="oauth2permissions-attribute"></a>oauth2Permissions özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2Permissions | Koleksiyon |

Web API (kaynak) uygulamasının istemci uygulamalarına sunduğu OAuth 2,0 izin kapsamlarının koleksiyonunu belirtir. Bu izin kapsamları, izin sırasında istemci uygulamalarına verilebilir.

Örnek:

```json
    "oauth2Permissions": [
       {
          "adminConsentDescription": "Allow the app to access resources on behalf of the signed-in user.",
          "adminConsentDisplayName": "Access resource1",
          "id": "<guid>",
          "isEnabled": true,
          "type": "User",
          "userConsentDescription": "Allow the app to access resource1 on your behalf.",
          "userConsentDisplayName": "Access resources",
          "value": "user_impersonation"
        }
    ],
```

### <a name="oauth2requiredpostresponse-attribute"></a>oauth2RequiredPostResponse özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2RequiredPostResponse | Boole |

OAuth 2,0 belirteç isteklerinin bir parçası olarak, Azure AD 'nin istekleri almak yerine POST isteklerine izin verip vermeyeceğini belirtir. Varsayılan değer, yalnızca GET isteklerinin izin verileceğini belirten false şeklindedir.

Örnek:

```json
    "oauth2RequirePostResponse": false,
```

### <a name="parentalcontrolsettings-attribute"></a>parentalControlSettings özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| parentalControlSettings | Dize |

- `countriesBlockedForMinors`uygulamanın minors için engellediği ülkeleri/bölgeleri belirtir.
- `legalAgeGroupRule`uygulamanın kullanıcıları için geçerli olan geçerli yaş grubu kuralını belirtir. ,,, `Allow` Veya olarak `RequireConsentForPrivacyServices` ayarlanabilir `RequireConsentForMinors` `RequireConsentForKids` `BlockMinors` .  

Örnek:

```json
    "parentalControlSettings": {
        "countriesBlockedForMinors": [],
        "legalAgeGroupRule": "Allow"
    },
```

### <a name="passwordcredentials-attribute"></a>passwordCredentials özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| passwordCredentials | Koleksiyon |

Özelliğin açıklamasına bakın `keyCredentials` .

Örnek:

```json
    "passwordCredentials": [
      {
        "customKeyIdentifier": null,
        "endDate": "2018-10-19T17:59:59.6521653Z",
        "keyId": "<guid>",
        "startDate":"2016-10-19T17:59:59.6521653Z",
        "value":null
      }
    ],
```

### <a name="preauthorizedapplications-attribute"></a>Ön kimlik Uthorizedapplications özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Ön kimlik doğrulama | Koleksiyon |

Kapalı onay için uygulamaları ve istenen izinleri listeler. Uygulamaya onay sağlanması için bir yönetici gerekir. Ön kimlik onayı Kullanıcı tarafından istenen izinlere izin vermesini gerektirmez. Ön kimlik onayı ' nda listelenen izinler için Kullanıcı onayı gerekmez. Bununla birlikte, ön ek Uthorizedapplications içinde listelenmeyen ek istenen izinler için Kullanıcı onayı gerekir.

Örnek:

```json
    "preAuthorizedApplications": [
       {
          "appId": "abcdefg2-000a-1111-a0e5-812ed8dd72e8",
          "permissionIds": [
             "8748f7db-21fe-4c83-8ab5-53033933c8f1"
            ]
        }
    ],
```

### <a name="publicclient-attribute"></a>publicClient özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| publicClient | Boole|

Bu uygulamanın ortak bir istemci olup olmadığını belirtir (örneğin, bir mobil cihazda çalışan yüklü bir uygulama gibi). 

Bu özellik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. `allowPublicClient` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

### <a name="publisherdomain-attribute"></a>publisherDomain özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| publisherDomain | Dize |

Uygulama için doğrulanmış yayımcı etki alanı. Salt okunur.

Örnek:

```json
    "publisherDomain": "https://www.contoso.com",
````

### <a name="replyurls-attribute"></a>replyUrls özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| replyUrls | Dize dizisi |

Bu çoklu değer özelliği, Azure AD 'nin belirteçleri döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerinin listesini tutar.

Bu özellik yalnızca **uygulama kayıtları (eski)** deneyimde kullanılabilir. `replyUrlsWithType` [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) deneyimiyle değiştirilmiştir.

### <a name="replyurlswithtype-attribute"></a>replyUrlsWithType özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| replyUrlsWithType | Koleksiyon |

Bu çoklu değer özelliği, Azure AD 'nin belirteçleri döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerinin listesini tutar. Her URI değeri, ilişkili bir uygulama türü değeri içermelidir. Desteklenen tür değerleri şunlardır:

- `Web`
- `InstalledClient`

Daha fazla bilgi için bkz. [Replyurl kısıtlamaları ve sınırlamaları](https://docs.microsoft.com/azure/active-directory/develop/reply-url).

Örnek:

```json
    "replyUrlsWithType": [
       {
          "url": "https://localhost:4400/services/office365/redirectTarget.html",
          "type": "InstalledClient"
       }
    ],
```

### <a name="requiredresourceaccess-attribute"></a>requiredResourceAccess özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| requiredResourceAccess | Koleksiyon |

Dinamik onay ile, `requiredResourceAccess` Yönetici onay deneyimini ve statik onay kullanan kullanıcılar için Kullanıcı onay deneyimini de yürütür. Ancak, bu parametre genel durum için Kullanıcı onay deneyimini değil.

- `resourceAppId`, uygulamanın erişmesi gereken kaynak için benzersiz tanıtıcıdır. Bu değer, hedef kaynak uygulamasında belirtilen uygulama kimliğine eşit olmalıdır.
- `resourceAccess`, belirtilen kaynaktan uygulamanın gerektirdiği OAuth 2.0 izin kapsamlarını ve uygulama rollerini listeleyen bir dizidir. `id` `type` Belirtilen kaynakların ve değerlerini içerir.

Örnek:

```json
    "requiredResourceAccess": [
        {
            "resourceAppId": "00000002-0000-0000-c000-000000000000",
            "resourceAccess": [
                {
                    "id": "311a71cc-e848-46a1-bdf8-97ff7156d8e6",
                    "type": "Scope"
                }
            ]
        }
    ],
```

### <a name="samlmetadataurl-attribute"></a>samlMetadataUrl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| samlMetadataUrl 'Si | Dize |

Uygulamanın SAML meta verilerinin URL 'SI.

Örnek:

```json
    "samlMetadataUrl": "https://MyRegisteredAppSAMLMetadata",
```

### <a name="signinurl-attribute"></a>Signınurl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Signınurl | Dize |

Uygulamanın giriş sayfasına yönelik URL 'YI belirtir.

Örnek:

```json
    "signInUrl": "https://MyRegisteredApp",
```

### <a name="signinaudience-attribute"></a>Signınaudience özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Signınkitci | Dize |

Geçerli uygulama için hangi Microsoft hesaplarının desteklendiğini belirtir. Desteklenen değerler şunlardır:
- `AzureADMyOrg`-Kuruluşumun Azure AD kiracısında bir Microsoft iş veya okul hesabı olan kullanıcılar (örneğin, tek kiracılı)
- `AzureADMultipleOrgs`-Herhangi bir kuruluşun Azure AD kiracısında bir Microsoft iş veya okul hesabı olan kullanıcılar (örneğin, çok kiracılı)
- `AzureADandPersonalMicrosoftAccount`-Kişisel Microsoft hesabı olan veya herhangi bir kuruluşun Azure AD kiracısındaki bir iş veya okul hesabı olan kullanıcılar
- `PersonalMicrosoftAccount`-Xbox ve Skype gibi hizmetlerde oturum açmak için kullanılan kişisel hesaplar.

Örnek:

```json
    "signInAudience": "AzureADandPersonalMicrosoftAccount",
```

### <a name="tags-attribute"></a>Tags özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| etiketler | Dize dizisi  |

Uygulamayı kategorilere ayırmak ve tanımlamak için kullanılabilen özel dizeler.

Örnek:

```json
    "tags": [
       "ProductionApp"
    ],
```

## <a name="common-issues"></a>Genel sorunlar

### <a name="manifest-limits"></a>Bildirim limitleri

Uygulama bildiriminde koleksiyonlar olarak adlandırılan birden çok öznitelik vardır; Örneğin, appRoles, keyCredentials, knownClientApplications, ıdentifieruris, Redirecturo, requiredResourceAccess ve oauth2Permissions. Herhangi bir uygulama için tam uygulama bildiriminde, Birleşik tüm koleksiyonlardaki toplam giriş sayısı 1200 ' de kadık. Daha önce uygulama bildiriminde 100 yeniden yönlendirme URI 'si belirtirseniz, daha sonra yalnızca, bildirimi oluşturan diğer tüm koleksiyonlar genelinde kullanılacak 1100 kalan girişlerle birlikte kaldınız.

> [!NOTE]
> Uygulama bildiriminde 1200 'den fazla girdi eklemeye çalışırsanız, **"uygulama xxxxxx güncelleştirilemedi" hatasını görebilirsiniz. Hata ayrıntıları: bildirimin boyutu sınırı aştı. Lütfen değer sayısını azaltın ve isteğinizi yeniden deneyin. "**

### <a name="unsupported-attributes"></a>Desteklenmeyen öznitelikler

Uygulama bildirimi, Azure AD 'de temel alınan uygulama modelinin şemasını temsil eder. Temel alınan şema geliştikçe, bildirim Düzenleyicisi yeni şemayı zaman zaman yansıtacak şekilde güncelleştirilecektir. Sonuç olarak, uygulama bildiriminde yeni öznitelikler olduğunu fark edebilirsiniz. Nadir durumlarda, var olan özniteliklerde bir sözdizimsel veya anlamsal değişiklik fark edebilir veya daha önce artık desteklenmeyen bir öznitelik bulabilirsiniz. Örneğin, Uygulama kayıtları (eski) deneyiminde farklı bir adla bilinen [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)yeni öznitelikleri görürsünüz.

| Uygulama kayıtları (eski)| Uygulama kayıtları           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Bu özniteliklerin açıklamaları için [bildirim başvurusu](#manifest-reference) bölümüne bakın.

Daha önce indirilen bir bildirimi karşıya yüklemeye çalıştığınızda, aşağıdaki hatalardan birini görebilirsiniz. Bu hata, bildirim Düzenleyicisi 'nin artık bir şemanın daha yeni bir sürümünü desteklediğinden, bu nedenle karşıya yüklemeye çalıştığınız bir ile eşleşmediği için olasıdır.

* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: geçersiz nesne tanımlayıcısı ' undefined '. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: belirtilen bir veya daha fazla özellik değeri geçersiz. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: güncelleştirme için bu API sürümünde Availabletootherkiracılar ayarlanamaz. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: Bu uygulama için ' replyUrls ' özelliğine yönelik güncelleştirmelere izin verilmiyor. Bunun yerine ' replyUrlsWithType ' özelliğini kullanın. []."
* "Xxxxxx uygulamasının güncelleştirilmesi başarısız oldu. Hata ayrıntısı: tür adı olmayan bir değer bulundu ve beklenen tür yok. Model belirtildiğinde, yükteki her bir değerin, açıkça çağıran tarafından ya da üst değerden dolaylı olarak belirtilen bir türde olması gerekir. []"

Bu hatalardan birini gördüğünüzde, aşağıdaki işlemleri yapmanızı öneririz:

1. Daha önce indirilen bir bildirimi karşıya yüklemek yerine, öznitelikleri bildirim düzenleyicisinde tek tek düzenleyin. İlgilendiğiniz öznitelikleri başarıyla düzenleyebilmeniz için, eski ve yeni özniteliklerin sözdizimini ve semantiğini anlamak için [bildirim başvuru](#manifest-reference) tablosunu kullanın. 
1. İş akışınız, daha sonra kullanmak üzere bildirimleri kaynak deponuza kaydetmenizi gerektiriyorsa, kayıtlı bildirimleri deponuzda **uygulama kayıtları** deneyiminde gördüğünüz bir şekilde yeniden temellendirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulamanın uygulaması ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz. [Azure AD 'de uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).
* Bazı temel Microsoft kimlik platformu geliştirici kavramlarının tanımları için [Microsoft Identity Platform geliştirici sözlüğü](developer-glossary.md) ' ne bakın.

İçeriğimizi iyileştirmenize ve şekillendirmeye yardımcı olacak geri bildirimler sağlamak için aşağıdaki açıklamalar bölümünü kullanın.

<!--article references -->
[AAD-APP-OBJECTS]:app-objects-and-service-principals.md
[AAD-DEVELOPER-GLOSSARY]:developer-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]:quickstart-v1-integrate-apps-with-azure-ad.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]:v1-oauth2-implicit-grant-flow.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
