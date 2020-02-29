---
title: Özel ilkede bir Azure AD teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede Azure Active Directory teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/13/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 06c9e79a68540cb10557b0951b743bf841963057
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78190271"
---
# <a name="define-an-azure-active-directory-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde Azure Active Directory teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) Azure Active Directory Kullanıcı yönetimi için destek sağlar. Bu makalede, bu standartlaştırılmış protokolü destekleyen bir talep sağlayıcısıyla etkileşim kurmaya yönelik teknik bir profilin ayrıntıları açıklanmaktadır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği `Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`protokol işleyicisi derlemesinin tam adını içermelidir.

Tüm Azure AD teknik profilleri **AAD ortak** teknik profilini içerir. Aşağıdaki teknik profiller protokol belirtmediğinden protokol **AAD ortak** teknik profilinde yapılandırılmıştır:

- **AAD-userreadusingalternativesecurityıd** ve **AAD-Userreadusingalternativesecurityıd-NOERROR** -dizinde sosyal hesap arama.
- **AAD-Userwriteusingalternativesecurityıd** -yeni bir sosyal hesap oluşturun.
- **AAD-Userreadusıngemapostaadresi** -dizinde yerel bir hesap arar.
- **AAD-UserWriteUsingLogonEmail** -yeni bir yerel hesap oluşturun.
- **AAD-UserWritePasswordUsingObjectId** -bir yerel hesabın parolasını güncelleştirin.
- **AAD-Userwriteprofileusingobjectıd** -yerel veya sosyal hesabın kullanıcı profilini güncelleştirin.
- **AAD-Userreadusingobjectıd** -yerel veya sosyal hesabın kullanıcı profilini okuyun.
- **AAD-Userwritephonenumberusingobjectıd** -yerel veya sosyal hesabın MFA telefon numarasını yazın

Aşağıdaki örnekte **AAD ortak** teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />

  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>

  <!-- We need this here to suppress the SelfAsserted provider from invoking SSO on validation profiles. -->
  <IncludeInSso>false</IncludeInSso>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Giriş talepleri

Aşağıdaki teknik profiller sosyal ve yerel hesaplar için **ınputclaim** içerir:

- Sosyal hesap teknik profilleri **AAD-Userreadusingalternativesecurityıd** ve **AAD-Userwriteusingalternativesecurityıd** , **alternativesecurityıd** talebini içerir. Bu talep, sosyal hesap kullanıcı tanımlayıcısını içerir.
- Yerel hesap teknik profilleri **AAD-Userreadusıngemapostaadresi** ve **AAD-UserWriteUsingLogonEmail** , **e-posta** talebi içerir. Bu talep, yerel hesabın oturum açma adını içerir.
- Birleştirilmiş (yerel ve sosyal) Teknik profiller **AAD-Userreadusingobjectıd**, **AAD-Userwritepasswordusingobjectıd**, **AAD-userwriteprofileusingobjectıd**ve AAD- **Userwritephonenumberusingobjectıd** , **ObjectID** talebini içerir. Bir hesabın benzersiz tanımlayıcısı.

**Inputclaimstransformations** öğesi, giriş taleplerini değiştirmek veya yenilerini oluşturmak Için kullanılan **inputclaimstransreference** öğelerinin bir koleksiyonunu içerebilir.

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi, Azure AD teknik profili tarafından döndürülen taleplerin bir listesini içerir. İlkenizde tanımlanan talebin adını Azure Active Directory tanımlı adla eşlemeniz gerekebilir. `DefaultValue` özniteliğini ayarladığınız sürece Azure Active Directory tarafından döndürülmeyen talepleri de ekleyebilirsiniz.

**Outputclaimstransformations** öğesi, çıkış taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir.

Örneğin, **AAD-UserWriteUsingLogonEmail** teknik profili yerel bir hesap oluşturur ve aşağıdaki talepleri döndürür:

- Yeni hesabın tanımlayıcısı olan **ObjectID**
- **Newuser**, kullanıcının yeni olup olmadığını belirtir
- kimlik doğrulamasını `localAccountAuthentication` ayarlayan **Authenticationsource**
- Yeni hesabın kullanıcı asıl adı olan **userPrincipalName**
- Hesap oturum açma adı olan ve **e-posta** girişi talebine benzeyen **signınnames. emapostaadı**

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="objectId" />
  <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
  <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
  <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
</OutputClaims>
```

## <a name="persistedclaims"></a>PersistedClaims

**PersistedClaims** öğesi, Ilke ve Azure AD öznitelik adındaki ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türü arasındaki olası eşleme bilgileri Ile Azure AD tarafından kalıcı olması gereken tüm değerleri içerir.

Yeni yerel hesap oluşturan **AAD-UserWriteUsingLogonEmail** teknik profili, sonrasında aşağıdaki talepler devam ediyor:

```XML
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
    <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password"/>
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />

    <!-- Optional claims. -->
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
```

Azure AD öznitelik adını içeren **Partnerclaimtype** özniteliği belirtilmediği takdirde talebin adı Azure ad özniteliğinin adıdır.

## <a name="requirements-of-an-operation"></a>Bir işlemin gereksinimleri

- Tüm Azure AD teknik profillerinin talep paketinde tam olarak bir **ınputclaim** öğesi olması gerekir.
- İşlem `Write` veya `DeleteClaims`, ayrıca bir **PersistedClaims** öğesinde da görünmelidir.
- **UserPrincipalName** talebinin değeri `user@tenant.onmicrosoft.com`biçiminde olmalıdır.
- **DisplayName** talebi gereklidir ve boş bir dize olamaz.

## <a name="azure-ad-technical-provider-operations"></a>Azure AD teknik sağlayıcı işlemleri

### <a name="read"></a>Okuma

**Okuma** işlemi, tek bir kullanıcı hesabı hakkındaki verileri okur. Kullanıcı verilerini okumak için, **ObjectID**, **userPrincipalName**, **signınnames** (herhangi bir tür, Kullanıcı adı ve e-posta tabanlı hesap) ya da **alternativesecurityıd**gibi bir giriş talebi olarak bir anahtar sağlamanız gerekir.

Aşağıdaki teknik profil, kullanıcının ObjectID 'sini kullanarak bir kullanıcı hesabı hakkındaki verileri okur:

```XML
<TechnicalProfile Id="AAD-UserReadUsingObjectId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims>

    <!-- Required claims -->
    <OutputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />

    <!-- Optional claims -->
    <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="write"></a>Yazma

**Yazma** işlemi tek bir kullanıcı hesabı oluşturur veya güncelleştirir. Bir kullanıcı hesabı yazmak için **ObjectID**, **userPrincipalName**, **Signınnames. Emapostaadı**veya **alternativesecurityıd**gibi bir giriş talebi olarak bir anahtar sağlamanız gerekir.

Aşağıdaki teknik profil yeni sosyal hesap oluşturur:

```XML
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Write</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  <IncludeInSso>false</IncludeInSso>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateOtherMailsFromEmail" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <!-- Required claims -->
    <PersistedClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <PersistedClaim ClaimTypeReferenceId="userPrincipalName" />
    <PersistedClaim ClaimTypeReferenceId="mailNickName" DefaultValue="unknown" />
    <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />

    <!-- Optional claims -->
    <PersistedClaim ClaimTypeReferenceId="otherMails" />
    <PersistedClaim ClaimTypeReferenceId="givenName" />
    <PersistedClaim ClaimTypeReferenceId="surname" />
  </PersistedClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>
```

### <a name="deleteclaims"></a>Deleteclaim

**Deleteclaim** işlemi, bilgileri, belirtilen talepler listesinden temizler. Taleplerden bilgileri silmek için, **ObjectID**, **userPrincipalName**, **Signınnames. Emadresi** veya **alternativesecurityıd**gibi bir giriş talebi olarak bir anahtar sağlamanız gerekir.

Aşağıdaki teknik profilde talepler silinir:

```XML
<TechnicalProfile Id="AAD-DeleteClaimsUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaims</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <PersistedClaims>
    <PersistedClaim ClaimTypeReferenceId="objectId" />
    <PersistedClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="strongAuthenticationPhoneNumber" />
  </PersistedClaims>
  <OutputClaims />
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

### <a name="deleteclaimsprincipal"></a>DeleteClaimsPrincipal

**DeleteClaimsPrincipal** işlemi, dizinden tek bir kullanıcı hesabını siler. Bir kullanıcı hesabını silmek için, **ObjectID**, **userPrincipalName**, **Signınnames. Emadresi** veya **alternativesecurityıd**gibi bir giriş talebi olarak bir anahtar sağlamalısınız.

Aşağıdaki teknik profil, Kullanıcı asıl adını kullanarak bir kullanıcı hesabını dizinden siler:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingObjectId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

Aşağıdaki teknik profil, bir sosyal Kullanıcı hesabını **Alternativesecurityıd**kullanarak siler:

```XML
<TechnicalProfile Id="AAD-DeleteUserUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">DeleteClaimsPrincipal</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims/>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```
## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İşlem | Yes | Gerçekleştirilecek işlem. Olası değerler: `Read`, `Write`, `DeleteClaims`veya `DeleteClaimsPrincipal`. |
| RaiseErrorIfClaimsPrincipalDoesNotExist | Hayır | Kullanıcı nesnesi dizinde yoksa bir hata oluştur. Olası değerler: `true` veya `false`. |
| UserMessageIfClaimsPrincipalDoesNotExist | Hayır | Bir hata ortaya çıkarılmalıdır (bkz. RaiseErrorIfClaimsPrincipalDoesNotExist Attribute Description), Kullanıcı nesnesi yoksa kullanıcıya gösterilecek iletiyi belirtin. Değer [yerelleştirilmiş](localization.md)olabilir.|
| RaiseErrorIfClaimsPrincipalAlreadyExists | Hayır | Kullanıcı nesnesi zaten mevcutsa bir hata oluştur. Olası değerler: `true` veya `false`.|
| Usermessageifclaimsprincıpalalreadyexists | Hayır | Bir hata ortaya çıkarılmalıdır (bkz. RaiseErrorIfClaimsPrincipalAlreadyExists Attribute Description), Kullanıcı nesnesi zaten varsa kullanıcıya gösterilecek iletiyi belirtin. Değer [yerelleştirilmiş](localization.md)olabilir.|
| Applicationobjectıd | Hayır | Uzantı öznitelikleri için uygulama nesne tanımlayıcısı. Değer: bir uygulamanın ObjectID 'si. Daha fazla bilgi için bkz. özel [bir profil düzenleme ilkesinde özel öznitelikler kullanma](custom-policy-custom-attributes.md). |
| ClientId | Hayır | Kiracıya üçüncü taraf olarak erişmek için istemci tanımlayıcısı. Daha fazla bilgi için bkz. özel [bir profil düzenleme ilkesinde özel öznitelikler kullanma](custom-policy-custom-attributes.md) |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıkış talepleri için, [talep çözümlemenin](claim-resolver-overview.md) teknik profile dahil edilip edilmeyeceğini belirtir. Olası değerler: `true`veya `false` (varsayılan). Teknik profilde bir talep çözümleyici kullanmak istiyorsanız bunu `true`olarak ayarlayın. |














