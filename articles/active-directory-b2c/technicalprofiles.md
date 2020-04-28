---
title: TechnicalProfiles
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkenin teknisyen öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5196615b6b935e4d37565298be03ad315163d132
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79264316"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir **teknisyen** öğesi, talep sağlayıcısı tarafından desteklenen bir teknik profiller kümesi içerir. Her talep sağlayıcının, uç noktaları ve talep sağlayıcısıyla iletişim kurmak için gereken protokolleri tespit eden bir veya daha fazla teknik profili olmalıdır. Bir talep sağlayıcısının birden çok teknik profili olabilir.

```XML
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">URL of service</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <InputTokenFormat>JWT</InputTokenFormat>
      <OutputTokenFormat>JWT</OutputTokenFormat>
      <CryptographicKeys>
        <Key ID="Key identifier" StorageReferenceId="Storage key container identifier"/>
        ...
      </CryptographicKeys>
      <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <InputClaimsTransformations>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </PersistedClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <OutputClaimsTransformations>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="Technical profile identifier" />
        ...
      </ValidationTechnicalProfiles>
      <SubjectNamingInfo ClaimType="Claim type identifier" />
      <IncludeTechnicalProfile ReferenceId="Technical profile identifier" />
      <UseTechnicalProfileForSessionManagement ReferenceId="Technical profile identifier" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

**Teknisyen** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
|---------|---------|---------|
| Kimlik | Yes | Teknik profilin benzersiz tanıtıcısı. Teknik profile bu tanımlayıcı kullanılarak ilke dosyasındaki diğer öğelerden başvurulabilir. Örneğin, **Orchestrationsteps** ve **Validationteknisyen alprofile**. |

**Teknisyen** aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Domain | 0:1 | Teknik profilin etki alanı adı. Örneğin, teknik profiliniz Facebook kimlik sağlayıcısını belirtiyorsa, etki alanı adı Facebook.com olur. |
| DisplayName | 1:1 | Kullanıcılara görüntülenebilecek teknik profilin adı. |
| Açıklama | 0:1 | Kullanıcılara görüntülenebilecek teknik profilin açıklaması. |
| Protokol | 0:1 | Diğer tarafla iletişim için kullanılan protokol. |
| Meta Veriler | 0:1 | Bir işlem sırasında uç noktayla iletişim kurmak için protokol tarafından kullanılan anahtar/değer çiftleri koleksiyonu. |
| Inputtokenformat | 0:1 | Giriş belirtecinin biçimi. Olası değerler: `JSON`, `JWT`, `SAML11`veya `SAML2`. Değer `JWT` , IETF belirtimine göre JSON Web Token temsil eder. Değer `SAML11` , oassıs BELIRTIMINE göre SAML 1,1 güvenlik belirtecini temsil eder.  Değer `SAML2` , oassıs BELIRTIMINE göre SAML 2,0 güvenlik belirtecini temsil eder. |
| OutputTokenFormat | 0:1 | Çıkış belirtecinin biçimi. Olası değerler: `JSON`, `JWT`, `SAML11`veya `SAML2`. |
| CryptographicKeys | 0:1 | Teknik profilde kullanılan şifreleme anahtarlarının bir listesi. |
| Inputclaimstransformations | 0:1 | Talep dönüşümlerine veya bağlı olan tarafa herhangi bir talep gönderilmeden önce yürütülmesi gereken talep dönüştürmelerinin önceden tanımlanmış başvuruları listesi. |
| Inputclaims | 0:1 | Teknik profilde girdi olarak alınan talep türlerine daha önceden tanımlanmış başvuruların listesi. |
| PersistedClaims | 0:1 | Teknik profille ilgili talep sağlayıcısı tarafından kalıcı olan talep türlerine yönelik önceden tanımlanmış başvuruların listesi. |
| DisplayClaim | 0:1 | [Kendi kendine onaylanan teknik profille](self-asserted-technical-profile.md)ilgili talep sağlayıcısı tarafından sunulan talep türleri için önceden tanımlanmış başvuruların listesi. DisplayClaim özelliği şu anda **önizlemededir**. |
| Outputclaim | 0:1 | Teknik profilde çıkış olarak alınan talep türlerine daha önceden tanımlanmış başvuruların listesi. |
| OutputClaimsTransformations | 0:1 | Talep dönüşümlerinden talepler alındıktan sonra yürütülmesi gereken talep dönüştürmelerine yönelik önceden tanımlanmış başvuruların listesi. |
| Validationcealprofiles | 0: n | Teknik profilin doğrulama amacıyla kullandığı diğer teknik profillerin başvurularını içeren bir liste. Daha fazla bilgi için bkz. [doğrulama teknik profili](validation-technical-profile.md)|
| Subjectnamingınfo | 0:1 | Konu adının taleplerden ayrı olarak belirtildiği belirteçlerde konu adının üretimini denetler. Örneğin, OAuth veya SAML.  |
| Includeınsso | 0:1 |  Bu teknik profilin kullanımının oturum için çoklu oturum açma (SSO) davranışını uygulayıp uygulamamalıdır, bunun yerine açık etkileşim gerekir. Bu öğe yalnızca bir doğrulama teknik profili içinde kullanılan Selfasthted profillerinde geçerlidir. Olası değerler: `true` (varsayılan) veya `false`. |
| IncludeClaimsFromTechnicalProfile | 0:1 | Tüm giriş ve çıkış taleplerinin bu teknik profile eklenmesini istediğiniz teknik profilin bir tanımlayıcısı. Başvurulan teknik profil aynı ilke dosyasında tanımlanmalıdır. |
| Includetekchnicalprofile |0:1 | Bu teknik profile tüm verilerin eklenmesini istediğiniz teknik profilin bir tanımlayıcısı. |
| Useteknisyen Alprofileforsessionmanagement | 0:1 | Oturum yönetimi için kullanılacak farklı bir teknik profil. |
|Enabledforuser, neys| 0:1 |Teknik profilin bir Kullanıcı yolculuğunda yürütülüp yürütülmeyeceğini denetler.  |

## <a name="protocol"></a>Protokol

**Protokol** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Adı | Yes | Teknik profilin bir parçası olarak kullanılan Azure AD B2C tarafından desteklenen geçerli bir protokol adı. Olası değerler: `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `Proprietary`veya `None`. |
| İşleyici | Hayır | Protokol adı olarak `Proprietary`ayarlandığında, protokol işleyicisini belirlemek için Azure AD B2C tarafından kullanılan derlemenin tam nitelikli adını belirtin. |

## <a name="metadata"></a>Meta Veriler

**Meta veri** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0: n | Teknik profille ilgili meta veriler. Her bir teknik profil türünün farklı bir meta veri öğesi kümesi vardır. Daha fazla bilgi için teknik profil türleri bölümüne bakın. |

### <a name="item"></a>Öğe

**Meta veri** öğesinin **Item** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Anahtar | Yes | Meta veri anahtarı. Meta veri öğelerinin listesi için her teknik profil türüne bakın. |

## <a name="cryptographickeys"></a>CryptographicKeys

**Cryptographickeys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Anahtar | 1: n | Bu teknik profilde kullanılan bir şifreleme anahtarı. |

### <a name="key"></a>Anahtar

**Anahtar** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Hayır | İlke dosyasındaki diğer öğelerden başvurulan belirli bir anahtar çiftinin benzersiz tanıtıcısı. |
| Storagereferenceıd değerine | Yes | İlke dosyasındaki diğer öğelerden başvurulan bir depolama anahtarı kapsayıcısının tanımlayıcı değeri. |

## <a name="inputclaimstransformations"></a>Inputclaimstransformations

**Inputclaimstransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaimstranssize | 1: n | Talep sağlayıcısına veya bağlı olan tarafa herhangi bir talep gönderilmeden önce yürütülmesi gereken bir talep dönüştürmesi tanımlayıcısı. Bir talep dönüştürmesi, var olan ClaimsSchema taleplerini değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

### <a name="inputclaimstransformation"></a>Inputclaimstranssize

**Inputclaimstransbir** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir talep dönüşümünün tanımlayıcısı. |

## <a name="inputclaims"></a>Inputclaims

**Inputclaim** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaim | 1: n | Beklenen giriş talep türü. |

### <a name="inputclaim"></a>Inputclaim

**Inputclaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Yes | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı. |
| Değerinin | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, sonuçta elde edilen talebin teknik profille bir ınputclaim olarak kullanılabilmesi için bir talep oluşturmak için kullanılacak varsayılan değer. |
| PartnerClaimType | Hayır | Belirtilen ilke talep türünün eşlendiği dış iş ortağının talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı ada sahip iş ortağı talep türüne eşlenir. Talep türü adınız diğer taraftan farklıysa bu özelliği kullanın. Örneğin, ilk talep adı ' bir ' ise, iş ortağı ' first_name ' adlı bir talep kullanır. |

## <a name="displayclaims"></a>DisplayClaim

**DisplayClaim** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| DisplayClaim | 1: n | Beklenen giriş talep türü. |

Dislayclaim özelliği şu anda **önizlemededir**.

### <a name="displayclaim"></a>DisplayClaim

**DisplayClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Hayır | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı. |
| Displaycontrolreferenceıd | Hayır | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir [görüntüleme denetiminin](display-controls.md) tanımlayıcısı. |
| Gerekli | Hayır | Görüntüleme talebinin gerekli olup olmadığını belirtir. |

**DisplayClaim** , `ClaimTypeReferenceId` ya `DisplayControlReferenceId`da belirtmenizi gerektirir.

### <a name="persistedclaims"></a>PersistedClaims

**PersistedClaims** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| PersistedClaim | 1: n | Kalıcı yapılacak talep türü. |

### <a name="persistedclaim"></a>PersistedClaim

**PersistedClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Yes | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı. |
| Değerinin | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, sonuçta elde edilen talebin teknik profille bir ınputclaim olarak kullanılabilmesi için bir talep oluşturmak için kullanılacak varsayılan değer. |
| PartnerClaimType | Hayır | Belirtilen ilke talep türünün eşlendiği dış iş ortağının talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı ada sahip iş ortağı talep türüne eşlenir. Talep türü adınız diğer taraftan farklıysa bu özelliği kullanın. Örneğin, ilk talep adı ' bir ' ise, iş ortağı ' first_name ' adlı bir talep kullanır. |

## <a name="outputclaims"></a>Outputclaim

**Outputclaim** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| OutputClaim | 1: n | Beklenen çıkış talep türü. |

### <a name="outputclaim"></a>OutputClaim

**Outputclaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Yes | İlke dosyası veya üst ilke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı. |
| Değerinin | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, sonuçta elde edilen talebin teknik profille bir ınputclaim olarak kullanılabilmesi için bir talep oluşturmak için kullanılacak varsayılan değer. |
|AlwaysUseDefaultValue |Hayır |Varsayılan değerin kullanımını zorunlu kılın.  |
| PartnerClaimType | Hayır | Belirtilen ilke talep türünün eşlendiği dış iş ortağının talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı ada sahip iş ortağı talep türüne eşlenir. Talep türü adınız diğer taraftan farklıysa bu özelliği kullanın. Örneğin, ilk talep adı ' bir ' ise, iş ortağı ' first_name ' adlı bir talep kullanır. |

## <a name="outputclaimstransformations"></a>OutputClaimsTransformations

**Outputclaimstransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Outputclaimstranssize | 1: n | Talepler, talep sağlayıcısına veya bağlı olan tarafa gönderilmeden önce yürütülmesi gereken talep dönüştürmelerinin tanımlayıcıları. Bir talep dönüştürmesi, var olan ClaimsSchema taleplerini değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

### <a name="outputclaimstransformation"></a>Outputclaimstranssize

**Outputclaimstransbir** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir talep dönüşümünün tanımlayıcısı. |

## <a name="validationtechnicalprofiles"></a>Validationcealprofiles

**Validation, Alprofiles** öğesi aşağıdaki öğeyi içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Validationteknisyen Alprofıle | 1: n | Kullanılan teknik profillerin tanımlayıcıları, başvuru yapan teknik profilin bazı veya tüm çıktı taleplerini doğrular. Başvurulan teknik profilin tüm giriş talepleri, başvuru yapan teknik profilin çıkış taleplerinden gelmelidir. |

### <a name="validationtechnicalprofile"></a>Validationteknisyen Alprofıle

**Validation, Alprofile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir teknik profilin tanımlayıcısı. |

## <a name="subjectnaminginfo"></a>Subjectnamingınfo

**Subjectnamingınfo** aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimType | Yes | İlke dosyasında ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türünün tanımlayıcısı. |

## <a name="includetechnicalprofile"></a>Includetekchnicalprofile

**Includetekchnicalprofile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir teknik profilin tanımlayıcısı. |

## <a name="usetechnicalprofileforsessionmanagement"></a>Useteknisyen Alprofileforsessionmanagement

**Useteknisyen Alprofileforsessionmanagement** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Yes | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir teknik profilin tanımlayıcısı. |

## <a name="enabledforuserjourneys"></a>Enabledforuser, neys

Kullanıcı yolculuğunda bulunan **Claimsproviderselection** , talep sağlayıcı seçim seçeneklerinin listesini ve bunların sıralarını tanımlar. Kullanıcı tarafından kullanılabilir olan **Enabledforuser, neys** öğesiyle filtrelemeniz gerekir. **Enabledforuser, neys** öğesi aşağıdaki değerlerden birini içerir:

- **Her zaman**teknik profili yürütün.
- **Hiçbir**şekilde teknik profili atlayın.
- **OnClaimsExistence** yalnızca teknik profilde belirtilen belirli bir talep varsa yürütülür.
- **OnItemExistenceInStringCollectionClaim**, yalnızca bir dize koleksiyonu talebinde bir öğe varsa yürütülür.
- **Onitemabsenceınstringcollectionclaim** yalnızca bir öğe dize koleksiyonu talebinde yoksa yürütülür.

**OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim** veya **Onitemabsenceınstringcollectionclaim**kullanarak, aşağıdaki meta verileri sağlamanızı gerektirir: **claimtypeondenhtoenable** , değerlendirilecek olan talebin türünü belirtir, **claimvalueondihtoenable** , karşılaştırılacak değeri belirtir.

Aşağıdaki teknik profil yalnızca **IdentityProviders** dize koleksiyonu şu değeri içeriyorsa yürütülür `facebook.com`:

```XML
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
      <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
      <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>
```
