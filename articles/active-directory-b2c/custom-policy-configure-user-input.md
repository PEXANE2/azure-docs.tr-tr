---
title: Özel ilkelerde talepler ekleme ve Kullanıcı girişini özelleştirme
titleSuffix: Azure AD B2C
description: Kullanıcı girişini özelleştirmeyi ve Azure Active Directory B2C kaydolma veya oturum açma yolculuğuna talepler ekleme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 56a3478f1c0dbc05eba07a5109f5bb6ba89b79d0
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79079889"
---
#  <a name="add-claims-and-customize-user-input-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeler kullanarak talepler ekleyin ve Kullanıcı girişini özelleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, kayıt seyahatinizi Azure Active Directory B2C (Azure AD B2C) sırasında yeni bir öznitelik topladığınızda. Kullanıcıların şehrini alacak, açılan olarak yapılandıracaksınız ve sağlanması gerekip gerekmediğini tanımlayacaksınız.

Kayıt veya oturum açma Kullanıcı yolculuğu kullanarak kullanıcılarınızla ilk verileri toplayabilirsiniz. Ek talepler, daha sonra bir profil düzenleme Kullanıcı yolculuğu kullanılarak toplanabilir. Azure AD B2C her zaman doğrudan kullanıcıdan etkileşimli olarak bilgi toplar, kimlik deneyimi çerçevesi [kendi kendine onaylanan teknik profilini](self-asserted-technical-profile.md)kullanır. Bu örnekte şunları yapabilirsiniz:

1. "Şehir" talebi tanımlayın.
1. Kullanıcıdan şehrini sorun.
1. Şehri Azure AD B2C dizinindeki Kullanıcı profiline kalıcı hale getirin.
1. Her oturum açma üzerinde Azure AD B2C dizininden şehir talebini okuyun.
1. Oturum açtıktan veya kaydolduktan sonra şehri bağlı olan taraf uygulamanıza döndürün.  

## <a name="prerequisites"></a>Önkoşullar

[Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Kaydolma ve oturum açma için sosyal ve yerel hesaplarla çalışan bir özel ilkenize sahip olmanız gerekir.

## <a name="define-a-claim"></a>Talep tanımlama

Bir talep, Azure AD B2C ilkesi yürütmesi sırasında verilerin geçici bir şekilde depolanmasını sağlar. [Talep şeması](claimsschema.md) , taleplerinizi bildirdiğiniz yerdir. Talebi tanımlamak için aşağıdaki öğeler kullanılır:

- **DisplayName** -kullanıcıya yönelik etiketi tanımlayan bir dize.
- [Veri türü](claimsschema.md#datatype) -talebin türü.
- **Userhelptext** -kullanıcının nelerin gerekli olduğunu anlamasına yardımcı olur.
- [Userınputtype](claimsschema.md#userinputtype) -metin kutusu, radyo seçimi, açılan liste veya birden çok seçim gibi giriş denetiminin türü.

İlkenizin uzantıları dosyasını açın. Örneğin, <em>**`TrustFrameworkExtensions.xml`**`SocialAndLocalAccounts/`</em>.

1. [Buildingblocks](buildingblocks.md) öğesi için arama yapın. Öğe yoksa, ekleyin.
1. [Claimsschema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Bir şehir talebini **Claimsschema** öğesine ekleyin.  

```xml
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-a-claim-to-the-user-interface"></a>Kullanıcı arabirimine talep ekleme

Aşağıdaki teknik [profiller, bir](self-asserted-technical-profile.md)kullanıcının giriş sağlaması beklendiğinde çağrılır:

- **Localaccountsignupwithlogonemail** -yerel hesap kaydolma akışı.
- **Selfasted-sosyal** -Federasyon hesabı ilk kez Kullanıcı oturum açma.
- **Selfasserted-ProfileUpdate** -profil akışını Düzenle.

Kaydolma sırasında şehir talebini toplamak için, `LocalAccountSignUpWithLogonEmail` teknik profiline bir çıkış talebi olarak eklenmelidir. Uzantı dosyasındaki bu teknik profili geçersiz kılın. Taleplerin ekranda sunulduğu sırayı denetlemek için tüm çıkış talepleri listesini belirtin. **Claimsproviders** öğesini bulun. Yeni bir ClaimsProviders şu şekilde ekleyin:

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <!--Local account sign-up page-->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
      <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
       <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
       <OutputClaim ClaimTypeReferenceId="displayName" />
       <OutputClaim ClaimTypeReferenceId="givenName" />
       <OutputClaim ClaimTypeReferenceId="surName" />
       <OutputClaim ClaimTypeReferenceId="city"/>
     </OutputClaims>
   </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
<ClaimsProvider>
```

Bir Federasyon hesabıyla ilk oturum açtıktan sonra şehir talebini toplamak için, `SelfAsserted-Social` teknik profiline bir çıkış talebi olarak eklenmelidir. Yerel ve Federal hesap kullanıcılarının profil verilerini daha sonra düzenleyebilmeleri için, `SelfAsserted-ProfileUpdate` teknik profiline çıkış talebini ekleyin. Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. Talepler ekranda sunulan sırayı denetlemek için çıkış taleplerinin tamamının listesini belirtin. **Claimsproviders** öğesini bulun. Yeni bir ClaimsProviders şu şekilde ekleyin:

```xml
  <DisplayName>Self Asserted</DisplayName>
  <TechnicalProfiles>
    <!--Federated account first-time sign-in page-->
    <TechnicalProfile Id="SelfAsserted-Social">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName"/>
        <OutputClaim ClaimTypeReferenceId="surname"/>
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
    <!--Edit profile page-->
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName"/>
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="city"/>
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="read-and-write-a-claim"></a>Talep okuma ve yazma

Aşağıdaki teknik profiller, Azure Active Directory verileri okuyan ve yazan [Teknik profillerdir Active Directory](active-directory-technical-profile.md).  
Kullanıcı profiline veri yazmak `OutputClaims` ve ilgili Active Directory teknik profillerinin içindeki kullanıcı profilinden veri okumak için `PersistedClaims` kullanın.

Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. **Claimsproviders** öğesini bulun.  Yeni bir ClaimsProviders şu şekilde ekleyin:

```xml
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <!-- Write data during a local account sign-up flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during a federated account first-time sign-in flow. -->
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Write data during edit profile flow. -->
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="city"/>
      </PersistedClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a local account. -->
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
    <!-- Read data after user authenticates with a federated account. -->
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <OutputClaims>  
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="include-a-claim-in-the-token"></a>Belirtece bir talep ekleyin 

Şehir talebini bağlı olan taraf uygulamasına geri döndürmek için <em>`SocialAndLocalAccounts/`**`SignUpOrSignIn.xml`**</em> dosyasına bir çıkış talebi ekleyin. Çıkış talebi, başarılı bir Kullanıcı yolculuğuna sonra belirtece eklenir ve uygulamaya gönderilir. Şehri çıkış talebi olarak eklemek için bağlı olan taraf bölümündeki teknik profil öğesini değiştirin.
 
```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
      <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
      <OutputClaim ClaimTypeReferenceId="city" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini SEÇIP Azure AD kiracınızı içeren dizini seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. **Özel Ilkeyi karşıya yükle**' yi seçin ve ardından değiştirdiğiniz iki ilke dosyasını karşıya yükleyin.
2. Karşıya yüklediğiniz kaydolma veya oturum açma ilkesini seçin ve **Şimdi Çalıştır** düğmesine tıklayın.
3. Bir e-posta adresi kullanarak kaydolabilirsiniz.

Kaydolma ekranı aşağıdaki ekran görüntüsüne benzer görünmelidir:

![Değiştirilen kaydolma seçeneğinin ekran görüntüsü](./media/custom-policy-configure-user-input/signup-with-city-claim-dropdown-example.png)

Uygulamanıza geri gönderilen belirteç `city` talebi içerir.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1583500140,
  "nbf": 1583496540,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1583496540,
  "auth_time": 1583496540,
  "name": "Emily Smith",
  "email": "joe@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "city": "Bellevue"
  ...
}
```

## <a name="next-steps"></a>Sonraki adımlar

- IEF başvurusunda [Claimsschema](claimsschema.md) öğesi hakkında daha fazla bilgi edinin.
- Özel [bir profil düzenleme ilkesinde özel öznitelikleri nasıl kullanacağınızı](custom-policy-custom-attributes.md)öğrenin.
