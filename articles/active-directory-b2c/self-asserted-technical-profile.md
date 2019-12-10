---
title: Özel ilkede kendi kendine onaylanan teknik profil tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede kendi kendine onaylanan bir teknik profil tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bfa8982fb49b31540d1926bdeb75a96dc1d79cf0
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74950910"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde kendi kendine onaylanan teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcının giriş sağlaması beklenen Azure Active Directory B2C (Azure AD B2C) içindeki tüm etkileşimler kendi kendine onaylanan teknik profillerdir. Örneğin, bir kaydolma sayfası, oturum açma sayfası veya parola sıfırlama sayfası.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği, kendi kendine onaylanan Azure AD B2C tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir: `Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnek, e-posta kaydı için kendi kendine onaylanan bir teknik profili göstermektedir:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```

## <a name="input-claims"></a>Giriş talepleri

Kendi kendine onaylanan bir teknik profilde, otomatik olarak onaylanan sayfada (çıkış talepleri) görünen taleplerin değerini önceden doldurmak için **ınputclaim** ve **ınputclaimstransformations** öğelerini kullanabilirsiniz. Örneğin, profil düzenleme ilkesinde Kullanıcı yolculuğu ilk olarak Azure AD B2C dizin hizmetinden kullanıcı profilini okur, ardından kendi kendini onaylanan teknik profil, giriş taleplerini Kullanıcı profilinde depolanan kullanıcı verileriyle ayarlar. Bu talepler Kullanıcı profilinden toplanır ve sonra var olan verileri düzenleyebilen kullanıcıya sunulur.

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
```


## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, kullanıcıdan veri toplamak için sunulacak taleplerin bir listesini içerir. Çıktı taleplerini bazı değerlerle önceden doldurmak için, daha önce açıklanan giriş taleplerini kullanın. Öğesi de varsayılan bir değer içerebilir. **Outputclaim** 'deki taleplerin sırası, Azure AD B2C ekrandaki talepleri işleme sırasını denetler. **DefaultValue** özniteliği yalnızca talep daha önce ayarlanmamışsa geçerli olur. Ancak, önceki bir düzenleme adımında daha önce ayarlandıysa, Kullanıcı değeri boş bırakırsa bile, varsayılan değer geçerli olmaz. Varsayılan bir değerin kullanımını zorlamak için, **Alwaysusedefaultvalue** özniteliğini `true`olarak ayarlayın. Kullanıcıyı belirli bir çıkış talebi için bir değer sağlamaya zorlamak için, **outputclaim** öğesinin **gerekli** özniteliğini `true`olarak ayarlayın.

**Outputclaim** koleksiyonundaki **ClaimType** öğesi, **userınputtype** öğesini `TextBox` veya `DropdownSingleSelect`gibi Azure AD B2C tarafından desteklenen herhangi bir kullanıcı giriş türüne ayarlamış olmalıdır. Ya da **Outputclaim** öğesinin bir **DefaultValue**ayarlaması gerekir.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Aşağıdaki çıkış talebi her zaman `live.com`olarak ayarlanmıştır:

```XML
<OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" AlwaysUseDefaultValue="true" />
```

### <a name="use-case"></a>Kullanım örneği

Çıkış talepleri için dört senaryo vardır:

- **Kullanıcıdan çıkış taleplerini toplama** -kullanıcıdan Doğum tarihi gibi bilgileri toplamanız gerektiğinde, isteği **outputclaim** koleksiyonuna eklemeniz gerekir. Kullanıcıya sunulan talepler `TextBox` veya `DropdownSingleSelect`gibi **Userınputtype**öğesini belirtmelidir. Kendi kendine onaylanan teknik profil, aynı talebi çıkaran bir doğrulama teknik profili içeriyorsa, Azure AD B2C kullanıcıya talep sunmaz. Kullanıcıya sunmak için herhangi bir çıkış talebi yoksa, Azure AD B2C teknik profili atlar.
- Kullanıcıdan verileri toplamadan veya doğrulama teknik profilinden verileri döndürmeden **bir çıkış talebinde varsayılan bir değer ayarlama** . `LocalAccountSignUpWithLogonEmail` kendi kendini onaylanan teknik profili, **yürütülmüş-SelfAsserted-Input** talebini `true`olarak ayarlar.
- **Bir doğrulama teknik profili, çıkış taleplerini döndürür** . Teknik profiliniz, bazı talepler döndüren bir doğrulama teknik profili çağırabilir. Talepleri kabarcığa eklemek ve bunları Kullanıcı yolculuğunda sonraki düzenleme adımlarına döndürmek isteyebilirsiniz. Örneğin, yerel bir hesapla oturum açarken, `SelfAsserted-LocalAccountSignin-Email` adlı kendi kendini onaylanan teknik profil, `login-NonInteractive`adlı doğrulama teknik profilini çağırır. Bu teknik profil, Kullanıcı kimlik bilgilerini doğrular ve kullanıcı profilini de döndürür. ' UserPrincipalName ', ' displayName ', ', ' ve ' soyadı ' gibi.
- **Talepler çıkış talebi dönüştürmesi aracılığıyla çıktı**

Aşağıdaki örnekte, `LocalAccountSignUpWithLogonEmail` kendinden onaylanan teknik profil, çıkış taleplerinin kullanımını gösterir ve **yürütülen-SelfAsserted-Input** ' ı `true`olarak ayarlar. `objectId`, `authenticationSource``newUser` talepleri `AAD-UserWriteUsingLogonEmail` doğrulama teknik profilinin çıktılardır ve kullanıcıya gösterilmez.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    <Item Key="language.button_continue">Create</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
    <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" />
    <OutputClaim ClaimTypeReferenceId="newUser" />

    <!-- Optional claims, to be collected from the user -->
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surName" />
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
  </ValidationTechnicalProfiles>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>

```

## <a name="persist-claims"></a>Kalıcı talepler

**PersistedClaims** öğesi yoksa, kendi kendini onaylanan teknik profil Azure AD B2C verileri kalıcı hale etmez. Bunun yerine, verileri kalıcı hale getirmekten sorumlu bir doğrulama teknik profiline çağrı yapılır. Örneğin kaydolma ilkesi, yeni kullanıcı profilini toplamak için `LocalAccountSignUpWithLogonEmail` kendi kendine onaylanan teknik profilini kullanır. `LocalAccountSignUpWithLogonEmail` teknik profili, hesabı Azure AD B2C oluşturmak için doğrulama teknik profilini çağırır.

## <a name="validation-technical-profiles"></a>Doğrulama teknik profilleri

Bir doğrulama teknik profili, başvuru yapan teknik profilin bazı veya tüm çıkış taleplerini doğrulamak için kullanılır. Doğrulama teknik profilinin giriş talepleri, kendi kendini onaylanan teknik profilin çıkış taleplerinde görünmelidir. Doğrulama teknik profili, Kullanıcı girişini doğrular ve kullanıcıya bir hata döndürebilir.

Doğrulama teknik profili, ilkede [Azure Active Directory](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md) teknik profilleri gibi herhangi bir teknik profil olabilir. Önceki örnekte `LocalAccountSignUpWithLogonEmail` teknik profili, Signınname 'in dizinde mevcut olmadığını doğrular. Aksi takdirde, doğrulama teknik profili yerel bir hesap oluşturur ve objectID, authenticationSource, newUser öğesini döndürür. `SelfAsserted-LocalAccountSignin-Email` teknik profili, Kullanıcı kimlik bilgilerini doğrulamak için `login-NonInteractive` doğrulama teknik profilini çağırır.

Ayrıca kurumsal iş kolu uygulamasıyla daha fazla tümleştirme yaparak iş mantığınızla birlikte REST API teknik bir profil çağırabilir, giriş taleplerinin üzerine yazabilir veya zenginleştirme Kullanıcı verileri sağlayabilirsiniz. Daha fazla bilgi için bkz. [doğrulama teknik profili](validation-technical-profile.md)

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gereklidir | Açıklama |
| --------- | -------- | ----------- |
| Setting. Showdevam düğmesi | Hayır | Devam düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| Setting. showCancelButton | Hayır | İptal düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| Setting. operatingMode | Hayır | Oturum açma sayfası için bu özellik, giriş doğrulaması ve hata iletileri gibi Kullanıcı adı alanının davranışını denetler. Beklenen değerler: `Username` veya `Email`. |
| Contentdefinitionreferenceıd | Yes | Bu teknik profille ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. |
| Enforceemaildoğrulaması | Hayır | Kaydolma veya profil düzenleme için, e-posta doğrulamasını uygular. Olası değerler: `true` (varsayılan) veya `false`. |
| . Showsignyukarı ayarlanıyor | Hayır | Kaydolma düğmesini görüntüler. Olası değerler: `true` (varsayılan) veya `false` |
| . retryLimit ayarlanıyor | Hayır | Bir kullanıcının bir doğrulama teknik profiline karşı denetlenen verileri sağlamaya kaç kez denendiğini denetler. Örneğin, bir kullanıcı zaten var olan bir hesapla kaydolmaya çalışır ve sınıra ulaşılana kadar denemeye devam eder.
| SignUpTarget | Hayır | Kaydolma hedefi değişim tanımlayıcısı. Kullanıcı kaydolma düğmesine tıkladığında, Azure AD B2C belirtilen Exchange tanımlayıcısını yürütür. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**Cryptographickeys** öğesi kullanılmıyor.













