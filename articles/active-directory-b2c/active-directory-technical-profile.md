---
title: Azure AD teknik profilini özel bir ilkede tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke de bir Azure Active Directory teknik profilini tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 7db47eda47850c1c080b6a49256c8a0b37bb0d3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80330379"
---
# <a name="define-an-azure-active-directory-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde Azure Active Directory teknik profilini tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), Azure Active Directory kullanıcı yönetimi için destek sağlar. Bu makalede, bu standart protokolü destekleyen bir talep sağlayıcısı yla etkileşim kurmak için teknik profilin özellikleri açıklanmaktadır.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, protokol işleyicisi derlemesinin `Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`tam nitelikli adını içermelidir.

[Özel ilke başlangıç paketini](custom-policy-get-started.md#custom-policy-starter-pack) takiben Azure AD teknik profilleri, **AAD-Common** teknik profilini içerir. Protokol **AAD-Common** teknik profilinde yapılandırıldığından, Azure AD teknik profilleri protokolü belirtmez:
 
- **AAD-UserReadUsingAlternativeSecurityId** ve **AAD-UserReadUsingAlternativeSecurityId-NoError** - Dizinde bir sosyal hesap arayın.
- **AAD-UserWriteUsingAlternativeSecurityId** - Yeni bir sosyal hesap oluşturun.
- **AAD-UserReadUsingEmailAddress** - Dizindeki yerel bir hesaba bakın.
- **AAD-UserWriteUsingLogonEmail** - Yeni bir yerel hesap oluşturun.
- **AAD-UserWritePasswordUsingObjectId** - Yerel bir hesabın parolalarını güncelleştirin.
- **AAD-UserWriteProfileUsingObjectId** - Yerel veya sosyal bir hesabın kullanıcı profilini güncelleştirin.
- **AAD-UserReadUsingObjectId** - Yerel veya sosyal bir hesabın kullanıcı profilini okuyun.
- **AAD-UserWritePhoneNumberUsingObjectId** - Yerel veya sosyal bir hesabın MFA telefon numarasını yazın

Aşağıdaki örnek, **AAD-Ortak** teknik profilini gösterir:

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

## <a name="inputclaims"></a>Giriş Talepleri

InputClaims öğesi, dizindeki bir hesabı aramak veya yeni bir hesap oluşturmak için kullanılan bir talep içerir. Tüm Azure AD teknik profilleri için giriş talepleri koleksiyonunda tam olarak bir Giriş Claim öğesi olmalıdır. İlkenizde tanımlanan talep adını Azure Etkin Dizini'nde tanımlanan adla eşlemeniz gerekebilir.

Varolan bir kullanıcı hesabını okumak, güncelleştirmek veya silmek için giriş talebi, Azure AD dizinindeki hesabı benzersiz olarak tanımlayan bir anahtardır. Örneğin, **objectId**, **userPrincipalName**, **signInNames.emailAddress**, **signInNames.userName**, veya **alternatifSecurityId**. 

Yeni bir kullanıcı hesabı oluşturmak için, giriş talebi yerel veya federe hesabı benzersiz olarak tanımlayan bir anahtardır. Örneğin, yerel hesap: **signInNames.emailAddress**veya **signInNames.userName**. Federe bir hesap için: **alternatifSecurityId**.

[InputClaimsTransformations](technicalprofiles.md#inputclaimstransformations) öğesi, giriş talebini değiştirmek veya yenisini oluşturmak için kullanılan giriş talepleri dönüştürme öğeleri koleksiyonunu içerebilir.

## <a name="outputclaims"></a>Çıktılar Talepleri

**Çıktılar Için Ler** öğesi, Azure AD teknik profili tarafından döndürülen taleplerin listesini içerir. İlkenizde tanımlanan talep adını Azure Etkin Dizini'nde tanımlanan adla eşlemeniz gerekebilir. Ayrıca, özniteliği ayarladığınız `DefaultValue` sürece Azure Etkin Dizini tarafından döndürülen talepleri de ekleyebilirsiniz.

[OutputClaimsTransformations](technicalprofiles.md#outputclaimstransformations) öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir.

Örneğin, **AAD-UserWriteUsingLogonEmail** teknik profili yerel bir hesap oluşturur ve aşağıdaki talepleri döndürür:

- **objectId**, yeni hesabın tanımlayıcısı
- **newUser**, kullanıcının yeni olup olmadığını gösterir
- **kimlik doğrulamaKaynak**, hangi kimlik doğrulaması ayarlar`localAccountAuthentication`
- **userPrincipalName**, yeni hesabın kullanıcı ana adıdır
- **signInNames.emailAddress**, hangi hesap oturum adı, **e-posta** giriş iddiabenzer

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

**Kalıcı Talepler** öğesi, ilkedeki [ClaimsSchema](claimsschema.md) bölümünde zaten tanımlanmış bir talep türü ile Azure AD öznitelik adı arasında olası eşleme bilgileriyle Azure AD tarafından kalıcı olarak kalıcı olması gereken tüm değerleri içerir.

Yeni yerel hesap oluşturan **AAD-UserWriteUsingLogonEmail** teknik profili aşağıdaki iddialara devam eder:

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

Azure AD özniteliği adını içeren **PartnerClaimType** özniteliği belirtilmedikçe, talebin adı Azure AD özniteliğinin adıdır.

## <a name="requirements-of-an-operation"></a>Bir operasyonun gereksinimleri

- Tüm Azure AD teknik profilleri için talep çantasında tam olarak bir **InputClaim** öğesi olmalıdır.
- [Kullanıcı profili öznitelikleri makalede,](user-profile-attributes.md) giriş taleplerinde, çıktı taleplerinde ve kalıcı taleplerde kullanabileceğiniz desteklenen Azure AD B2C kullanıcı profili öznitelikleri açıklanmaktadır. 
- İşlem `Write` veya `DeleteClaims`, o zaman da bir **PersistedClaims** öğesi görünmesi gerekir.
- **UserPrincipalName** talebinin değeri `user@tenant.onmicrosoft.com`.
- **DisplayName** claim gereklidir ve boş bir dize olamaz.

## <a name="azure-ad-technical-provider-operations"></a>Azure AD teknik sağlayıcı işlemleri

### <a name="read"></a>Okuma

**Read** işlemi, tek bir kullanıcı hesabıyla ilgili verileri okur. Aşağıdaki teknik profil, kullanıcının objectId'ini kullanarak bir kullanıcı hesabı hakkındaki verileri okur:

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

**Yazma** işlemi tek bir kullanıcı hesabı oluşturur veya güncelleştirir. Aşağıdaki teknik profil yeni bir sosyal hesap oluşturur:

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

### <a name="deleteclaims"></a>Talepleri Silme

**DeleteClaims** işlemi, bilgileri verilen talep listesinden temizler. Aşağıdaki teknik profil talepleri siler:

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

**DeleteClaimsPrincipal** işlemi tek bir kullanıcı hesabını dizinden siler. Aşağıdaki teknik profil, kullanıcı anaadını kullanarak bir kullanıcı hesabını dizinden siler:

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

Aşağıdaki teknik profil **alternativeSecurityId**kullanarak bir sosyal kullanıcı hesabını siler:

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
| İşlem | Evet | Yapılacak işlem. Olası `Read`değerler: `Write` `DeleteClaims`, `DeleteClaimsPrincipal`, , veya . |
| RaiseErrorifClaimsPrincipalDoesExist | Hayır | Dizinde kullanıcı nesnesi yoksa hata kaldırın. Olası `true` değerler: `false`veya . |
| RaiseErrorifClaimsPrincipalAlreadyExists | Hayır | Kullanıcı nesnesi zaten varsa bir hata kaldırın. Olası `true` değerler: `false`veya .|
| UygulamaObjectId | Hayır | Uzantı öznitelikleri için uygulama nesnesi tanımlayıcısı. Değer: Bir uygulamanın ObjectId'i. Daha fazla bilgi için bkz. [Özel profil edit ilkesinde özel öznitelikleri kullan.](custom-policy-custom-attributes.md) |
| ClientId | Hayır | Kiracıya üçüncü taraf olarak erişmek için istemci tanımlayıcısı. Daha fazla bilgi için bkz: [Özel profil edit ilkesinde özel öznitelikleri kullan](custom-policy-custom-attributes.md) |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |

### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri
 
Aşağıdaki ayarlar, hata üzerine görüntülenen hata iletisini yapılandırmak için kullanılabilir. Meta [veriler, kendi kendine ileri süren](self-asserted-technical-profile.md) teknik profilde yapılandırılmalıdır. Hata iletileri [yerelleştirilebilir.](localization.md)

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| UserMessageIfClaimsPrincipalAlreadyAlreadyExists | Hayır | Bir hata yükseltilecekse (bkz. RaiseErrorIfClaimsPrincipalAlreadyExists öznitelik açıklaması), kullanıcı nesnesi zaten varsa kullanıcıya gösterilecek iletiyi belirtin. |
| UserMessageIfClaimsPrincipalDoesExist | Hayır | Bir hata yükseltilecekse (bkz. RaiseErrorIfClaimsPrincipalDoesExist öznitelik açıklaması), kullanıcı nesnesi yoksa kullanıcıya gösterilecek iletiyi belirtin. |


## <a name="next-steps"></a>Sonraki adımlar

Azure AD teknik profilini kullanma gibi aşağıdaki makaleye bakın:

- [Azure Active Directory B2C'de özel ilkeler kullanarak talep ekleme ve kullanıcı girdilerini özelleştirme](custom-policy-configure-user-input.md)














