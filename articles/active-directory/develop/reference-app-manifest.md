---
title: Azure Active Directory uygulama bildirimini anlama | Microsoft Dokümanlar
description: Bir Azure AD kiracısında bir uygulamanın kimlik yapılandırmasını temsil eden ve OAuth yetkilendirmesini, onay deneyimini ve daha fazlasını kolaylaştırmak için kullanılan Azure Active Directory uygulama bildiriminin ayrıntılı kapsamı.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/23/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: sureshja
ms.openlocfilehash: 6d9a4af5ee814282589959fcf840c1061358ca18
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80383948"
---
# <a name="azure-active-directory-app-manifest"></a>Azure Active Directory uygulama bildirimi

Uygulama bildirimi, Microsoft kimlik platformundaki bir uygulama nesnesinin tüm özniteliklerinin tanımını içerir. Ayrıca, uygulama nesnesini güncelleştirmek için bir mekanizma olarak da hizmet vermektedir. Uygulama varlığı ve şeması hakkında daha fazla bilgi için [Grafik API Uygulama varlık belgelerine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)bakın.

Bir uygulamanın özniteliklerini Azure portalı üzerinden veya [REST API](https://docs.microsoft.com/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity) veya [PowerShell'i](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#applications)kullanarak programlı olarak yapılandırabilirsiniz. Ancak, bir uygulamanın özniteliğini yapılandırmak için uygulama bildirimini yeniden yapılandırmanız gereken bazı senaryolar vardır. Bu senaryolar şunlardır:

* Uygulamayı Azure AD çok kiracılı ve kişisel Microsoft hesapları olarak kaydettiyseniz, Kullanıcı Arabirimi'nde desteklenen Microsoft hesaplarını değiştiremezsiniz. Bunun yerine, desteklenen hesap türünü değiştirmek için uygulama bildirimi düzenleyicisini kullanmanız gerekir.
* Uygulamanızın desteklediği izinleri ve rolleri tanımlamanız gerekiyorsa, uygulama bildirimini değiştirmeniz gerekir.

## <a name="configure-the-app-manifest"></a>Uygulama bildirimini yapılandırma

Uygulama bildirimini yapılandırmak için:

1. [Azure portalına](https://portal.azure.com)gidin. **Azure Active Directory** hizmetini arayın ve seçin.
1. **Uygulama kayıtları**'nı seçin.
1. Yapılandırmak istediğiniz uygulamayı seçin.
1. Uygulamanın **Genel Bakış** sayfasında, **Bildirim** bölümünü seçin. Web tabanlı bir manifesto düzenleyicisi açılır ve portal içindeki bildirimi yeniden atasınız. İsteğe bağlı olarak, bildirimi yerel olarak yeniden yüklemek için **İndir'i** seçebilir ve ardından uygulamanıza yeniden uygulamak için **Yükle'yi** kullanabilirsiniz.

## <a name="manifest-reference"></a>Bildirim başvurusu

Bu bölümde, uygulama bildiriminde bulunan öznitelikler açıklanmaktadır.

### <a name="accesstokenacceptedversion-attribute"></a>accessTokenAcceptedVersion özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| accessTokenAcceptedVersion | Nullable Int32 |

Kaynak tarafından beklenen erişim belirteç sürümünü belirtir. Bu parametre, erişim belirteci istemek için kullanılan bitiş noktası veya istemciden bağımsız olarak üretilen JWT'nin sürümünü ve biçimini değiştirir.

Kullanılan bitiş noktası, v1.0 veya v2.0, istemci tarafından seçilir ve yalnızca id_tokens sürümünü etkiler. Kaynakların, desteklenen erişim `accesstokenAcceptedVersion` belirteci biçimini belirtmek için açıkça yapılandırması gerekir.

Olası `accesstokenAcceptedVersion` değerler 1, 2 veya null'dür. Değer null ise, bu parametre varsayılan 1, v1.0 bitiş noktasına karşılık gelir.

Ise, `signInAudience` değer olmalıdır. `2` `AzureADandPersonalMicrosoftAccount`

Örnek:

```json
    "accessTokenAcceptedVersion": 2,
```

### <a name="addins-attribute"></a>addIns özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Addins | Koleksiyon |

Tüketen bir hizmetin bir uygulamayı belirli bağlamlarda çağırmak için kullanabileceği özel davranışı tanımlar. Örneğin, dosya akışlarını işleyebilecek uygulamalar `addIns` özelliği "FileHandler" işlevi için ayarlayabilir. Bu parametre, Office 365 gibi hizmetlerin, kullanıcının üzerinde çalıştığı belge bağlamında uygulamayı aramasına izin verecektir.

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

### <a name="allowpublicclient-attribute"></a>publicclient özniteliğine izin

| Anahtar | Değer türü |
| :--- | :--- |
| allowPublicClient | Boole |

Geri dönüş uygulama türünü belirtir. Azure AD varsayılan olarak yanıturlsWithType'tan uygulama türünü çıkartıyor. Azure AD'nin istemci uygulama türünü belirleyemeyebileceği belirli senaryolar vardır. Örneğin, bu tür senaryolardan biri, HTTP isteğinin URL yeniden yönlendirmesi olmadan gerçekleştiği [ROPC](https://tools.ietf.org/html/rfc6749#section-4.3) akışıdır). Bu gibi durumlarda, Azure AD uygulama türünü bu özelliğin değerine göre yorumlanır. Bu değer doğru ayarlanırsa, geri dönüş uygulama türü, mobil aygıtta çalışan yüklü bir uygulama gibi ortak istemci olarak ayarlanır. Varsayılan değer yanlıştır, bu da geri dönüş uygulama türünün web uygulaması gibi gizli istemci olduğu anlamına gelir.

Örnek:

```json
    "allowPublicClient": false,
```

### <a name="availabletoothertenants-attribute"></a>availableToOtherTenants özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| kullanılabilirToOtherTenants | Boole |

Uygulama diğer kiracılarla paylaşılırsa doğru olarak ayarlayın; aksi takdirde, yanlış.

> [!NOTE]
> Bu özellik yalnızca Uygulama **kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `signInAudience` deneyiminde değiştirilir.

### <a name="appid-attribute"></a>appId özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| appId | Dize |

Azure AD tarafından bir uygulamaya atanan uygulamaiçin benzersiz tanımlayıcıyı belirtir.

Örnek:

```json
    "appId": "601790de-b632-4f57-9523-ee7cb6ceba95",
```

### <a name="approles-attribute"></a>appRoles özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| appRoles | Koleksiyon |

Bir uygulamanın beyan edebileceği rollerin toplanmasını belirtir. Bu roller kullanıcılara, gruplara veya hizmet ilkelerine atanabilir. Daha fazla örnek ve bilgi için, [uygulamanızda uygulama rollerini ekle ve bunları belirteç olarak alın.](howto-add-app-roles-in-azure-ad-apps.md)

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

Uygulamanın görüntü adı.

> [!NOTE]
> Bu özellik yalnızca Uygulama **kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `name` deneyiminde değiştirilir.

### <a name="errorurl-attribute"></a>errorUrl öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| errorUrl | Dize |

Desteklenmez.

### <a name="groupmembershipclaims-attribute"></a>groupÜyelikClaims öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
|grupÜyelikTalepleri | Dize |

Bir kullanıcı `groups` veya Uygulamanın beklediği OAuth 2.0 erişim belirtecinde verilen talebi yapılandırır. Bu özniteliği ayarlamak için aşağıdaki geçerli dize değerlerinden birini kullanın:

- `"None"`
- `"SecurityGroup"`(güvenlik grupları ve Azure AD rolleri için)
- `"All"`(bu, oturum açmış kullanıcının üyesi olduğu tüm güvenlik gruplarını, dağıtım gruplarını ve Azure AD dizin rollerini alır.

Örnek:

```json
    "groupMembershipClaims": "SecurityGroup",
```

### <a name="homepage-attribute"></a>ana sayfa özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| Ana sayfa |Dize |

Uygulamanın ana sayfasının URL'si.

> [!NOTE]
> Bu özellik yalnızca Uygulama **kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `signInUrl` deneyiminde değiştirilir.

### <a name="objectid-attribute"></a>objectId özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
|Objectıd | Dize |

Dizindeki uygulama için benzersiz tanımlayıcı.

Bu yalnızca Uygulama **kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `id` deneyiminde değiştirilir.

Örnek:

```json
    "objectId": "f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd",
```

### <a name="optionalclaims-attribute"></a>isteğe bağlıİddialar öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| isteğe bağlı Talepler | Dize |

İsteğe bağlı talepler, bu özel uygulama için güvenlik belirteci hizmeti tarafından belirteç olarak döndürülür.

Şu anda, hem kişisel hesapları hem de Azure AD'yi (uygulama kayıt portalı üzerinden kaydedilmiş) destekleyen uygulamalar isteğe bağlı talepleri kullanamaz. Ancak, v2.0 bitiş noktasını kullanarak yalnızca Azure AD için kayıtlı uygulamalar, bildirimde talep ettikleri isteğe bağlı talepleri alabilir. Daha fazla bilgi için [isteğe bağlı taleplere](active-directory-optional-claims.md)bakın.

Örnek:

```json
    "optionalClaims": null,
```

### <a name="id-attribute"></a>id özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| id | Dize |

Dizindeki uygulama için benzersiz tanımlayıcı. Bu kimlik, herhangi bir protokol hareketinde uygulamayı tanımlamak için kullanılan tanımlayıcı değildir. Dizin sorgularında nesneye başvurmak için kullanılır.

Örnek:

```json
    "id": "f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd",
```

### <a name="identifieruris-attribute"></a>identifierUris özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| identifierUris | Dize Dizisi |

Azure AD kiracısı içinde veya uygulama çok kiracılıysa, doğrulanmış özel etki alanı içinde bir Web uygulamasını benzersiz olarak tanımlayan kullanıcı tanımlı URI(ler).

Örnek:

```json
    "identifierUris": "https://MyRegisteredApp",
```

### <a name="informationalurls-attribute"></a>informationalUrls öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| bilgialUrls | Dize |

Uygulamanın hizmet koşulları ve gizlilik bildirimine bağlantılar belirtir. Hizmet koşulları ve gizlilik bildirimi, kullanıcı onayı deneyimi aracılığıyla kullanıcılara açıklanır. Daha fazla bilgi [için bkz: Kayıtlı Azure REKLAM uygulamaları için Hizmet Şartları ve gizlilik bildirimi ekleyin.](howto-add-terms-of-service-privacy-statement.md)

Örnek:

```json
    "informationalUrls": {
        "termsOfService": "https://MyRegisteredApp/termsofservice",
        "support": "https://MyRegisteredApp/support",
        "privacy": "https://MyRegisteredApp/privacystatement",
        "marketing": "https://MyRegisteredApp/marketing"
    },
```

### <a name="keycredentials-attribute"></a>keyCredentials öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| keyCredentials | Koleksiyon |

Uygulama tarafından atanan kimlik bilgilerine, dize tabanlı paylaşılan sırlara ve X.509 sertifikalarına başvurular tutar. Bu kimlik bilgileri, erişim belirteçleri talep ederken (uygulama bir kaynak olarak değil, bir istemci olarak hareket ederken) kullanılır.

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

### <a name="knownclientapplications-attribute"></a>bilinenMüşteri Uygulamaları özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| bilinenMüşteri Uygulamaları | Dize Dizisi |

İki bölümden biri olan bir çözüm varsa, istemci uygulaması ve özel bir web API uygulaması için kullanılır. Bu değere istemci uygulamasının appID'ini girerseniz, kullanıcının yalnızca bir kez istemci uygulamasına onay vermek zorunda kalacaktır. Azure AD, istemciye rıza göstermenin web API'sini dolaylı olarak kabul etmek anlamına geldiğini bilir. Hem istemci hem de web API'sı için servis ilkelerini aynı anda otomatik olarak sağlayacaktır. Hem istemci hem de web API uygulaması aynı kiracıya kayıtlı olmalıdır.

Örnek:

```json
    "knownClientApplications": ["f7f9acfc-ae0c-4d6c-b489-0a81dc1652dd"],
```

### <a name="logourl-attribute"></a>logoUrl öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| logoUrl | Dize |

Portala yüklenen logoya CDN URL'sini gösteren yalnızca değeri okuyun.

Örnek:

```json
    "logoUrl": "https://MyRegisteredAppLogo",
```

### <a name="logouturl-attribute"></a>logoutUrl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| logoutUrl | Dize |

Uygulamadan çıkış yapmak için URL.

Örnek:

```json
    "logoutUrl": "https://MyRegisteredAppLogout",
```

### <a name="name-attribute"></a>ad özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| ad | Dize |

Uygulamanın görüntü adı.

Örnek:

```json
    "name": "MyRegisteredApp",
```

### <a name="oauth2allowimplicitflow-attribute"></a>oauth2AllowImplicitFlow özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2AllowImplicitFlow | Boole |

Bu web uygulamasının OAuth2.0 örtülü akış erişim belirteçleri isteyip istemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır. Daha fazla bilgi `OAuth 2.0 implicit grant flow` edinmek için içindekiler tablosuna girin ve örtük akışla ilgili konulara bakın.

Örnek:

```json
    "oauth2AllowImplicitFlow": false,
```

### <a name="oauth2allowidtokenimplicitflow-attribute"></a>oauth2AllowIdTokenImplicitFlow öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2AllowIdTokenImplicitFlow | Boole |

Bu web uygulamasının OAuth2.0 örtülü akış kimliği belirteçleri isteyip istemeyeceğini belirtir. Varsayılan değer false. Bu bayrak, JavaScript tek sayfalı uygulamalar gibi tarayıcı tabanlı uygulamalar için kullanılır.

Örnek:

```json
    "oauth2AllowIdTokenImplicitFlow": false,
```

### <a name="oauth2permissions-attribute"></a>oauth2Permissions öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2İzinler | Koleksiyon |

Web API (kaynak) uygulamasının istemci uygulamalarına maruz kdığı OAuth 2.0 izin kapsamlarının toplanmasını belirtir. Bu izin kapsamları, onay sırasında istemci uygulamalarına verilebilir.

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

### <a name="oauth2requiredpostresponse-attribute"></a>oauth2RequiredPostResponse öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| oauth2RequiredPostResponse | Boole |

OAuth 2.0 belirteç isteklerinin bir parçası olarak Azure AD'nin GET isteklerinin aksine POST isteklerine izin verip vermeyeceği belirtilir. Varsayılan, yalnızca GET isteklerine izin verilemeyeceğini belirten yanlıştır.

Örnek:

```json
    "oauth2RequirePostResponse": false,
```

### <a name="parentalcontrolsettings-attribute"></a>parentalControlSettings özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| parentalControlSettings | Dize |

- `countriesBlockedForMinors`uygulamanın reşit olmayanlar için engellendiği ülkeleri belirtir.
- `legalAgeGroupRule`uygulamanın kullanıcıları için geçerli olan yasal yaş grubu kuralını belirtir. Ayarlanabilir `Allow`, , `RequireConsentForPrivacyServices` `RequireConsentForMinors` `RequireConsentForKids`, , `BlockMinors`veya .  

Örnek:

```json
    "parentalControlSettings": {
        "countriesBlockedForMinors": [],
        "legalAgeGroupRule": "Allow"
    },
```

### <a name="passwordcredentials-attribute"></a>passwordCredentials öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| passwordCredentials | Koleksiyon |

Tesisin açıklamasına `keyCredentials` bakın.

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

### <a name="preauthorizedapplications-attribute"></a>preAuthorizedApplications özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| önYetkili Uygulamalar | Koleksiyon |

Uygulamaları ve örtülü onay için istenen izinleri listeler. Yöneticinin uygulamaya onay vermiş olmasını gerektirir. ön Yetkili Uygulamalar, kullanıcının istenen izinleri onaylamasını gerektirmez. Ön Yetkili Uygulamalarda listelenen izinler kullanıcı onayı gerektirmez. Ancak, ön Yetkili Uygulamalarda listelenmemiş ek istenen izinler kullanıcı onayı gerektirir.

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

Bu uygulamanın ortak istemci olup olmadığını (mobil aygıtta çalışan yüklü bir uygulama gibi) belirtir. 

Bu özellik yalnızca **Uygulama kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `allowPublicClient` deneyiminde değiştirilir.

### <a name="publisherdomain-attribute"></a>publisherDomain özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| publisherDomain | Dize |

Uygulama için doğrulanmış yayımcı etki alanı. Salt okunur.

Örnek:

```json
    "publisherDomain": "https://www.contoso.com",
````

### <a name="replyurls-attribute"></a>replyUrls öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| cevapUrlleri | Dize dizisi |

Bu çok değerli özellik, Azure AD'nin jeton döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerin listesini tutar.

Bu özellik yalnızca **Uygulama kayıtları (Eski)** deneyiminde kullanılabilir. [Uygulama](https://go.microsoft.com/fwlink/?linkid=2083908) kayıtları `replyUrlsWithType` deneyiminde değiştirilir.

### <a name="replyurlswithtype-attribute"></a>replyUrlsWithType özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| cevapUrlsWithType | Koleksiyon |

Bu çok değerli özellik, Azure AD'nin jeton döndürürken hedef olarak kabul edeceği kayıtlı redirect_uri değerlerin listesini tutar. Her URI değeri ilişkili bir uygulama türü değeri içermelidir. Desteklenen tür değerleri şunlardır:

- `Web`
- `InstalledClient`

Daha fazla bilgi için [yanıtUrl kısıtlamaları ve sınırlamalarına](https://docs.microsoft.com/azure/active-directory/develop/reply-url)bakın.

Örnek:

```json
    "replyUrlsWithType": [
       {
          "url": "https://localhost:4400/services/office365/redirectTarget.html",
          "type": "InstalledClient"
       }
    ],
```

### <a name="requiredresourceaccess-attribute"></a>gerekliResourceAccess özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| gerekliResourceAccess | Koleksiyon |

Dinamik onay `requiredResourceAccess` ile, statik onay kullanan kullanıcılar için yönetici onayı deneyimini ve kullanıcı onayı deneyimini yönlendirir. Ancak, bu parametre genel servis talebi için kullanıcı onayı deneyimini yönlendirmez.

- `resourceAppId`uygulamanın erişilmesi gereken kaynak için benzersiz tanımlayıcıdır. Bu değer, hedef kaynak uygulamasında bildirilen appId'e eşit olmalıdır.
- `resourceAccess`oAuth2.0 izin kapsamlarını ve uygulama rollerini listeleyen bir dizidir. Belirtilen `id` kaynakların `type` değerlerini ve değerlerini içerir.

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

### <a name="samlmetadataurl-attribute"></a>samlMetadataUrl öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| samlMetadataUrl | Dize |

Uygulama için SAML meta verilerinin URL'si.

Örnek:

```json
    "samlMetadataUrl": "https://MyRegisteredAppSAMLMetadata",
```

### <a name="signinurl-attribute"></a>signInUrl özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| signInUrl | Dize |

URL'yi uygulamanın ana sayfasına belirtir.

Örnek:

```json
    "signInUrl": "https://MyRegisteredApp",
```

### <a name="signinaudience-attribute"></a>signInAudience özniteliği

| Anahtar | Değer türü |
| :--- | :--- |
| signInAudience | Dize |

Geçerli uygulama için Microsoft hesaplarının destekleniyi belirtir. Desteklenen değerler şunlardır:
- `AzureADMyOrg`- Kuruluşumun Azure AD kiracısında Microsoft çalışması veya okul hesabı olan kullanıcılar (örneğin, tek kiracı)
- `AzureADMultipleOrgs`- Herhangi bir kuruluşun Azure AD kiracısında Microsoft çalışması veya okul hesabı olan kullanıcılar (örneğin, çok kiracılı)
- `AzureADandPersonalMicrosoftAccount`- Herhangi bir kuruluşun Azure AD kiracısında kişisel Microsoft hesabı veya iş veya okul hesabı olan kullanıcılar
- `PersonalMicrosoftAccount`- Xbox ve Skype gibi hizmetlerde oturum açmada kullanılan kişisel hesaplar.

Örnek:

```json
    "signInAudience": "AzureADandPersonalMicrosoftAccount",
```

### <a name="tags-attribute"></a>etiketler öznitelik

| Anahtar | Değer türü |
| :--- | :--- |
| etiketler | Dize Dizisi  |

Uygulamayı kategorilere ayırmak ve tanımlamak için kullanılabilecek özel dizeleri.

Örnek:

```json
    "tags": [
       "ProductionApp"
    ],
```

## <a name="common-issues"></a>Genel sorunlar

### <a name="manifest-limits"></a>Bildirim sınırları

Uygulama bildirimi, koleksiyon lar olarak adlandırılan birden çok öznitelide dir; örneğin, appRoles, keyCredentials, knownClientApplications, identifierUris, redirectUris, requiredResourceAccess ve oauth2Permissions. Herhangi bir uygulama için tam başvuru bildirimi içinde, tüm koleksiyonlarda toplam giriş sayısı 1200 olarak sınırlanmıştır. Daha önce uygulama bildiriminde 100 yeniden yönlendirme URI belirtirseniz, bildirimi oluşturan diğer tüm koleksiyonlarda kullanmak üzere yalnızca 1100 kalan girişle kalırsınız.

> [!NOTE]
> Uygulama bildirimine 1200'den fazla giriş eklemeye çalışırsanız, **"Uygulama xxxxxx'i güncelleştirmek için başarısız olunması gereken bir hata görebilirsiniz. Hata ayrıntıları: Bildirimin boyutu sınırını aştı. Lütfen değer sayısını azaltın ve isteğinizi yeniden deneyin."**

### <a name="unsupported-attributes"></a>Desteklenmeyen öznitelikler

Uygulama bildirimi, Azure AD'deki temel uygulama modelinin şemasını temsil eder. Altta yatan şema geliştikçe, manifesto düzenleyicisi zaman zaman yeni şemayı yansıtacak şekilde güncellenir. Sonuç olarak, uygulama bildiriminde yeni özniteliklerin ortaya çıktığını fark edebilirsiniz. Nadir durumlarda, varolan özniteliklerde sözdizim veya anlamdeğişikliği fark edebilirsiniz veya daha önce var olan bir öznitelik artık desteklenmeyen bulabilirsiniz. Örneğin, Uygulama kayıtlarında farklı bir adla bilinen [Uygulama kayıtlarında](https://go.microsoft.com/fwlink/?linkid=2083908)(Eski) yeni öznitelikler görürsünüz.

| Uygulama kayıtları (Eski)| Uygulama kayıtları           |
|---------------------------|-----------------------------|
| `availableToOtherTenants` | `signInAudience`            |
| `displayName`             | `name`                      |
| `errorUrl`                | -                           |
| `homepage`                | `signInUrl`                 |
| `objectId`                | `Id`                        |
| `publicClient`            | `allowPublicClient`         |
| `replyUrls`               | `replyUrlsWithType`         |

Bu özniteliklere yönelik açıklamalar için [bildirim başvuru](#manifest-reference) bölümüne bakın.

Daha önce indirilen bir bildirimi yüklemeye çalıştığınızda, aşağıdaki hatalardan birini görebilirsiniz. Bu hata, bildirim düzenleyicisinin artık şemanın yüklemeye çalıştığınız sürümle eşleşmeyen daha yeni bir sürümünü desteklemesinden kaynaklanıyor.

* "xxxxxx uygulamasını güncelleştiremedi. Hata ayrıntısı: Geçersiz nesne tanımlayıcısı 'tanımsız'. []."
* "xxxxxx uygulamasını güncelleştiremedi. Hata ayrıntısı: Belirtilen bir veya daha fazla özellik değeri geçersizdir. []."
* "xxxxxx uygulamasını güncelleştiremedi. Hata ayrıntısı: Güncelleştirme için bu api sürümünde kullanılabilir ToOtherTenants ayarlamak için izin verilmez. []."
* "xxxxxx uygulamasını güncelleştiremedi. Hata ayrıntısı: Bu uygulama için 'yanıtUrl'ler' özelliğigüncelleştirmelerine izin verilmez. Bunun yerine 'replyUrlsWithType' özelliğini kullanın. []."
* "xxxxxx uygulamasını güncelleştiremedi. Hata ayrıntısı: Tür adı olmayan bir değer bulundu ve beklenen bir tür bulunamadı. Model belirtildiğinde, yükteki her değer, arayan tarafından açıkça veya üst değerden dolaylı olarak çıkarılabilen, yükte belirtilebilen bir türe sahip olmalıdır. []"

Bu hatalardan birini gördüğünüzde, aşağıdaki eylemleri öneririz:

1. Daha önce indirilen bir bildirimi yüklemek yerine öznitelikleri manifest düzenleyicisinde tek tek edin. İlgilendiğiniz öznitelikleri başarılı bir şekilde yeniden yapabilmek için eski ve yeni özniteliklerin sözdizimini ve anlambilimini anlamak için [bildirim başvuru](#manifest-reference) tablosunu kullanın. 
1. İş akışınız daha sonra kullanılmak üzere kaynak deponuzdaki bildirimleri kaydetmenizi gerektiriyorsa, deponuzdaki kaydedilen bildirimleri **Uygulama kayıtları** deneyiminde gördüğünüz bildirimle yeniden birleştirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir uygulamanın uygulaması ve hizmet temel nesnesi(ler) arasındaki ilişki hakkında daha fazla bilgi için [Azure AD'deki Uygulama ve hizmet temel nesneleri'ne](app-objects-and-service-principals.md)bakın.
* Bazı temel Microsoft kimlik platformu geliştirici kavramlarının tanımları için [Microsoft kimlik platformu geliştirici sözlüğüne](developer-glossary.md) bakın.

İçeriğimizi geliştirmeye ve şekillendirmeye yardımcı olan geri bildirim sağlamak için aşağıdaki yorumlar bölümünü kullanın.

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
