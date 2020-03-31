---
title: TechnicalProfiles
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkenin Teknik Profiller öğesini belirtin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264316"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**TechnicalProfiles** öğesi, talep sağlayıcısı tarafından desteklenen bir dizi teknik profil içerir. Her talep sağlayıcısının, uç noktaları ve talep sağlayıcıyla iletişim kurmak için gereken protokolleri belirleyen bir veya daha fazla teknik profili olmalıdır. Talep sağlayıcının birden çok teknik profili olabilir.

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

**TechnicalProfile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
|---------|---------|---------|
| Kimlik | Evet | Teknik profilin benzersiz bir tanımlayıcısı. Teknik profil, ilke dosyasındaki diğer öğelerden bu tanımlayıcı kullanılarak başvurulabilir. Örneğin, **OrkestrasyonAdımları** ve **DoğrulamaTeknik Profil**. |

**TechnicalProfile** aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Domain | 0:1 | Teknik profilin etki alanı adı. Örneğin, teknik profiliniz Facebook kimlik sağlayıcısını belirtse, alan adı Facebook.com. |
| DisplayName | 1:1 | Kullanıcılara görüntülenebilen teknik profilin adı. |
| Açıklama | 0:1 | Kullanıcılara görüntülenebilen teknik profilin açıklaması. |
| Protokol | 0:1 | Diğer tarafla iletişim için kullanılan protokol. |
| Meta Veriler | 0:1 | Bir işlem in sEçeneğinde son noktayla iletişim kurmak için protokol tarafından kullanılan anahtar/değer çiftleri koleksiyonu. |
| InputTokenFormat | 0:1 | Giriş belirteci biçimi. Olası `JSON`değerler: `JWT` `SAML11`, `SAML2`, , veya . Değer, `JWT` IETF belirtimine göre bir JSON Web Belirteci'ni temsil eder. Değer, `SAML11` OASIS belirtimine göre bir SAML 1.1 güvenlik belirteci temsil eder.  Değer, `SAML2` OASIS belirtimine göre saml 2.0 güvenlik belirteci temsil eder. |
| OutputTokenFormat | 0:1 | Çıkış belirteci biçimi. Olası `JSON`değerler: `JWT` `SAML11`, `SAML2`, , veya . |
| CryptographicKeys | 0:1 | Teknik profilde kullanılan şifreleme anahtarlarının listesi. |
| GirişTalepleriDönüşümler | 0:1 | Herhangi bir hak talebi sağlayıcısına veya bağlı tarafa gönderilmeden önce yürütülmesi gereken talep dönüşümlerine daha önce tanımlanmış başvuruların listesi. |
| Giriş Talepleri | 0:1 | Teknik profilde girdi olarak alınan talep türlerine daha önce tanımlanmış başvuruların listesi. |
| PersistedClaims | 0:1 | Teknik profille ilgili talep sağlayıcı tarafından kalıcı olarak verilen talep türlerine daha önce tanımlanmış başvuruların listesi. |
| Görüntü talepleri | 0:1 | Kendi [kendine ileri edilen teknik profille](self-asserted-technical-profile.md)ilgili talep sağlayıcı tarafından sunulan talep türlerine daha önce tanımlanmış başvuruların listesi. DisplayClaims özelliği şu anda **önizlemede.** |
| Çıktılar Talepleri | 0:1 | Teknik profilde çıktı olarak alınan talep türlerine daha önce tanımlanmış başvuruların listesi. |
| OutputClaimsDönüşümler | 0:1 | Talep sağlayıcıdan alındıktan sonra yürütülmesi gereken talep dönüşümlerine daha önce tanımlanmış başvuruların listesi. |
| DoğrulamaTeknik Profiller | 0:n | Teknik profilin doğrulama amacıyla kullandığı diğer teknik profillere yapılan başvuruların listesi. Daha fazla bilgi için [doğrulama teknik profiline](validation-technical-profile.md) bakın|
| Konu NamingInfo | 0:1 | Özne adının taleplerden ayrı olarak belirtildiği belirteçlerde özne adının üretimini denetler. Örneğin, OAuth veya SAML.  |
| IncludeInSso | 0:1 |  Bu teknik profilin kullanımının oturum için tek oturum açma (SSO) davranışı mı yoksa açık etkileşim mi gerektirmesi gerektiği. Bu öğe yalnızca Doğrulama teknik profili içinde kullanılan SelfAsserted profillerinde geçerlidir. Olası değerler: `true` (varsayılan) veya `false`. |
| Teknik Profilden Gelen İddiaları Dahil | 0:1 | Tüm giriş ve çıktı taleplerinin bu teknik profile eklenmesini istediğiniz teknik profilin tanımlayıcısı. Başvurulan teknik profil aynı ilke dosyasında tanımlanmalıdır. |
| Teknik Profil Dahil |0:1 | Tüm verilerin bu teknik profile eklenmesini istediğiniz teknik profilin tanımlayıcısı. |
| KullanımTeknikProfilForSessionManagement | 0:1 | Oturum yönetimi için kullanılacak farklı bir teknik profil. |
|EnabledForUserYolculuklar| 0:1 |Teknik profilin kullanıcı yolculuğunda yürütülüp yürütülmeyişmeyeolmadığını denetler.  |

## <a name="protocol"></a>Protokol

**Protokol** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Adı | Evet | Teknik profilin bir parçası olarak kullanılan Azure AD B2C tarafından desteklenen geçerli bir protokolün adı. `OAuth1`Olası değerler: `OAuth2` `SAML2`, `OpenIdConnect` `Proprietary`, `None`, , , veya . |
| İşleyici | Hayır | Protokol adı `Proprietary`ayarlandığında, protokol işleyicisini belirlemek için Azure AD B2C tarafından kullanılan derlemenin tam nitelikli adını belirtin. |

## <a name="metadata"></a>Meta Veriler

**Meta veri** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0:n | Teknik profille ilgili meta veriler. Her teknik profil türünde farklı bir meta veri öğesi kümesi vardır. Daha fazla bilgi için teknik profil türleri bölümüne bakın. |

### <a name="item"></a>Öğe

**Meta veri** öğesinin **Öğe** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Anahtar | Evet | Meta veri anahtarı. Meta veri öğeleri listesi için her teknik profil türüne bakın. |

## <a name="cryptographickeys"></a>CryptographicKeys

**CryptographicKeys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Anahtar | 1:n | Bu teknik profilde kullanılan bir şifreleme anahtarı. |

### <a name="key"></a>Anahtar

**Anahtar** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Hayır | İlke dosyasındaki diğer öğelerden başvurulan belirli bir anahtar çiftinin benzersiz tanımlayıcısı. |
| DepolamaReferenceId | Evet | İlke dosyasındaki diğer öğelerden başvurulan bir depolama anahtarı kapsayıcısının identiferi. |

## <a name="inputclaimstransformations"></a>GirişTalepleriDönüşümler

**InputClaimsTransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| GirişTalepleriDönüşüm | 1:n | Herhangi bir hak talebi sağlayıcısına veya bağlı tarafa gönderilmeden önce yürütülmesi gereken bir talep dönüşümü tanımlayıcısı. Talep dönüştürmesi, varolan ClaimsSchema taleplerini değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

### <a name="inputclaimstransformation"></a>GirişTalepleriDönüşüm

**InputClaimsTransformation** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir talep dönüştürmesinin tanımlayıcısı. |

## <a name="inputclaims"></a>Giriş Talepleri

**InputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Girişİddia | 1:n | Beklenen giriş talebi türü. |

### <a name="inputclaim"></a>Girişİddia

**InputClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı. |
| Varsayılan Değer | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, elde edilen talep teknik profil tarafından Giriş Talebi olarak kullanılabilecek şekilde talep oluşturmak için kullanılacak varsayılan değer. |
| OrtakClaimType | Hayır | Belirtilen ilke talebi türü eşler dış ortağın talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı adı taşıyan ortak talep türüne eşlenir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, ilk talep adı 'givenName' iken, ortak 'first_name' adlı bir talep kullanır. |

## <a name="displayclaims"></a>Görüntü talepleri

**DisplayClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Görüntütalebi | 1:n | Beklenen giriş talebi türü. |

DislayClaims özelliği şu anda **önizlemede.**

### <a name="displayclaim"></a>Görüntütalebi

**DisplayClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Hayır | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı. |
| DisplayControlReferenceId | Hayır | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir [görüntü denetiminin](display-controls.md) tanımlayıcısı. |
| Gerekli | Hayır | Görüntü talebinin gerekli olup olmadığını gösterir. |

**DisplayClaim,** bir `ClaimTypeReferenceId` veya `DisplayControlReferenceId`.

### <a name="persistedclaims"></a>PersistedClaims

**PersistedClaims** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| PersistedClaim | 1:n | Devam etmek için talep türü. |

### <a name="persistedclaim"></a>PersistedClaim

**PersistedClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı. |
| Varsayılan Değer | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, elde edilen talep teknik profil tarafından Giriş Talebi olarak kullanılabilecek şekilde talep oluşturmak için kullanılacak varsayılan değer. |
| OrtakClaimType | Hayır | Belirtilen ilke talebi türü eşler dış ortağın talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı adı taşıyan ortak talep türüne eşlenir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, ilk talep adı 'givenName' iken, ortak 'first_name' adlı bir talep kullanır. |

## <a name="outputclaims"></a>Çıktılar Talepleri

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| ÇıktılarTalep | 1:n | Beklenen çıktı talep türü. |

### <a name="outputclaim"></a>ÇıktılarTalep

**OutputClaim** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyasındaki veya üst ilke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı. |
| Varsayılan Değer | Hayır | ClaimTypeReferenceId tarafından belirtilen talep yoksa, elde edilen talep teknik profil tarafından Giriş Talebi olarak kullanılabilecek şekilde talep oluşturmak için kullanılacak varsayılan değer. |
|Her Zaman Varsayılan Değeri Kullan |Hayır |Varsayılan değerin kullanımını zorlar.  |
| OrtakClaimType | Hayır | Belirtilen ilke talebi türü eşler dış ortağın talep türünün tanımlayıcısı. PartnerClaimType özniteliği belirtilmemişse, belirtilen ilke talep türü aynı adı taşıyan ortak talep türüne eşlenir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, ilk talep adı 'givenName' iken, ortak 'first_name' adlı bir talep kullanır. |

## <a name="outputclaimstransformations"></a>OutputClaimsDönüşümler

**OutputClaimsTransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| ÇıktılarTalepDönüşüm | 1:n | Herhangi bir hak talebi sağlayıcısına veya bağlı tarafa gönderilmeden önce yürütülmesi gereken talep dönüşümlerinin tanımlayıcıları. Talep dönüştürmesi, varolan ClaimsSchema taleplerini değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

### <a name="outputclaimstransformation"></a>ÇıktılarTalepDönüşüm

**OutputClaimsTransformation** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış olan bir talep dönüştürmesinin tanımlayıcısı. |

## <a name="validationtechnicalprofiles"></a>DoğrulamaTeknik Profiller

**DoğrulamaTeknik Profiller** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| DoğrulamaTeknikProfil | 1:n | Kullanılan teknik profillerin tanımlayıcıları, başvuru yapan teknik profilin çıktı taleplerinin bir kısmını veya tamamını doğrular. Başvurulan teknik profilin tüm giriş iddiaları, başvurulan teknik profilin çıktı taleplerinde görünmelidir. |

### <a name="validationtechnicalprofile"></a>DoğrulamaTeknikProfil

**DoğrulamaTeknikProfil** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış bir teknik profilin tanımlayıcısı. |

## <a name="subjectnaminginfo"></a>Konu NamingInfo

**SubjectNamingInfo** aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Claimtype | Evet | İlke dosyasındaki ClaimsSchema bölümünde zaten tanımlanmış bir talep türünün tanımlayıcısı. |

## <a name="includetechnicalprofile"></a>Teknik Profil Dahil

**IncludeTechnicalProfile** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış bir teknik profilin tanımlayıcısı. |

## <a name="usetechnicalprofileforsessionmanagement"></a>KullanımTeknikProfilForSessionManagement

**UseTechnicalProfileForSessionManagement** öğesi aşağıdaki özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosyasında veya üst ilke dosyasında zaten tanımlanmış bir teknik profilin tanımlayıcısı. |

## <a name="enabledforuserjourneys"></a>EnabledForUserYolculuklar

Kullanıcı yolculuğundaki **ClaimsProviderS seçimleri,** talep sağlayıcı seçim seçeneklerinin listesini ve siparişlerini tanımlar. **EnabledForUserJourneys** öğesi ile filtrelediğiniz, sağlayıcının kullanıcı tarafından kullanılabildiğini iddia eder. **EnabledForUserJourneys** öğesi aşağıdaki değerlerden birini içerir:

- **Her zaman,** teknik profili çalıştırın.
- **Asla,** teknik profili atlayın.
- **OnClaimsExistence** yalnızca teknik profilde belirtilen belirli bir talep olduğunda yürütülür.
- **OnItemExistenceInStringCollectionClaim**, yalnızca bir öğe bir dize toplama iddiasında varsa çalıştırın.
- **OnItemAbsenceInStringCollectionClaim** yalnızca bir öğe bir dize toplama iddiasında yoksa yürütün.

**OnClaimsExistence**kullanarak , **OnItemExistenceInStringCollectionClaim** veya **OnItemAbsenceInStringCollectionClaim**, aşağıdaki meta verileri sağlamanızı gerektirir: **ClaimTypeOnWhichToEnable** değerlendirilecek olan iddianın türünü belirtir, **ClaimValueOnWhichToEnable** karşılaştırılabilir değeri belirtir.

Aşağıdaki teknik profil yalnızca **identityProviders** dize koleksiyonu aşağıdakilerin değerini içeriyorsa `facebook.com`yürütülür:

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
