---
title: Özel ilkelere talep ekleme ve kullanıcı girdisi özelleştirme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'deki kaydolma veya kaydolma yolculuğuna kullanıcı girişini nasıl özelleştirip talep ekleyin öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 85f2ab6f8c3e5edda027e44eeda13a3279a88321
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473685"
---
#  <a name="add-claims-and-customize-user-input-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeler kullanarak talep ekleme ve kullanıcı girdilerini özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C'deki (Azure AD B2C) kayıt yolculuğunuz sırasında yeni bir öznitelik toplarsınız. Kullanıcıların şehrini elde eder, açılır olarak yapılandıracak ve sağlanıp sağlanmadığını tanımlarsınız.

> [!NOTE]
> Bu örnek, yerleşik iddia 'şehir' kullanır. Bunun yerine, desteklenen [Azure AD B2C yerleşik özniteliklerden](user-profile-attributes.md) birini veya özel bir öznitelik seçebilirsiniz. Özel bir öznitelik kullanmak [için, ilkenizde özel öznitelikleri etkinleştirin.](custom-policy-custom-attributes.md) Farklı bir yerleşik veya özel öznitelik kullanmak için, 'şehir'i seçtiğiniz öznitelikle (örneğin yerleşik öznitelik *işiBaşlığı* veya *extension_loyaltyId*gibi özel bir öznitelik gibi değiştirin.  

Kaydolma veya kaydolma kullanıcı yolculuğunu kullanarak kullanıcılarınızdan ilk verileri toplayabilirsiniz. Ek talepler daha sonra bir profil düzenleme kullanıcı yolculuğu kullanılarak toplanabilir. Azure AD B2C bilgileri doğrudan kullanıcıdan etkileşimli olarak toplayınca, Kimlik Deneyimi Çerçevesi [kendi kendine öne süren teknik profilini](self-asserted-technical-profile.md)kullanır. Bu örnekte, şunları

1. Bir "şehir" iddiası tanımlayın. 
1. Kullanıcıdan kendi şehirlerini isteyin.
1. Azure AD B2C dizinindeki kullanıcı profiline şehri devam edin.
1. Her oturum açmada Azure AD B2C dizininden şehir iddiasını okuyun.
1. Oturum açtıktan veya kaydolduktan sonra şehri güvenen parti başvurunuza iade edin.  

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Sosyal ve yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.

## <a name="define-a-claim"></a>Bir talep tanımlama

Talep, Azure AD B2C ilke yürütmesi sırasında geçici bir veri depolama sağlar. [İddia şeması,](claimsschema.md) iddialarınızı beyan ettiğiniz yerdir. Talebi tanımlamak için aşağıdaki öğeler kullanılır:

- **DisplayName** - Kullanıcıya bakan etiketi tanımlayan bir dize.
- [DataType](claimsschema.md#datatype) - Talebin türü.
- **UserHelpText** - Kullanıcının ne gerektiğini anlamasını sağlar.
- [UserInputType](claimsschema.md#userinputtype) - Metin kutusu, radyo seçimi, açılır liste veya birden çok seçim gibi giriş denetimi türü.

İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.

1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Şehir iddiasını **ClaimsSchema** öğesine ekleyin.  

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

Aşağıdaki teknik profiller, kullanıcının girdi sağlaması beklenirken çağrılan kendi [kendine ileri](self-asserted-technical-profile.md)edilir:

- **LocalAccountSignWithLogonEmail** - Yerel hesap kayıt akışı.
- **SelfAsserted-Social** - Federe hesap ilk kez kullanıcı oturum açma.
- **SelfAsserted-ProfileUpdate** - Profil akışını edin.

Kayıt sırasında şehir talebini toplamak için, `LocalAccountSignUpWithLogonEmail` teknik profile çıktı talebi olarak eklenmelidir. Uzantı dosyasındaki bu teknik profili geçersiz kılın. Taleplerin ekranda sunulduğu sırayı denetlemek için çıktı taleplerinin tüm listesini belirtin. İddia **Sağlayıcıları** öğesini bulun. Aşağıdaki gibi yeni bir ClaimsProviders ekleyin:

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

Federe bir hesapla ilk oturum dan sonra şehir talebini tahsil etmek için, `SelfAsserted-Social` teknik profile çıktı talebi olarak eklenmelidir. Yerel ve federe hesap kullanıcılarının profil verilerini daha sonra edinebilmeleri `SelfAsserted-ProfileUpdate` için çıktı talebini teknik profile ekleyin. Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. Taleplerin ekranda sunulduğu sırayı denetlemek için çıktı taleplerinin tüm listesini belirtin. İddia **Sağlayıcıları** öğesini bulun. Aşağıdaki gibi yeni bir ClaimsProviders ekleyin:

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

## <a name="read-and-write-a-claim"></a>Bir iddiayı okuma ve yazma

Aşağıdaki teknik profiller, Azure Active Directory'ye veri okuyup yazan [Active Directory teknik profilleridir.](active-directory-technical-profile.md)  
Kullanıcı `PersistedClaims` profiline veri yazmak `OutputClaims` ve ilgili Active Directory teknik profillerindeki kullanıcı profilindeki verileri okumak için kullanın.

Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. İddia **Sağlayıcıları** öğesini bulun.  Aşağıdaki gibi yeni bir ClaimsProviders ekleyin:

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

## <a name="include-a-claim-in-the-token"></a>Belirteci bir talep ekleme 

Şehir talebini güvenilen taraf uygulamasına geri döndürmek <em> `SocialAndLocalAccounts/` </em> için, dosyaya bir çıktı talebi ekleyin. Çıktı talebi, başarılı bir kullanıcı yolculuğundan sonra belirteciiçine eklenir ve uygulamaya gönderilir. Bir çıktı talebi olarak şehir eklemek için güvenilen parti bölümündeki teknik profil öğesini değiştirin.
 
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

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. **Özel İlke Yükle'yi**seçin ve ardından değiştirdiğiniz iki ilke dosyasını yükleyin.
2. Yüklediğiniz kaydolma veya kaydolma ilkesini seçin ve **Şimdi Çalıştır** düğmesini tıklatın.
3. Bir e-posta adresi kullanarak kaydolmalısınız.

Kayıt ekranı aşağıdaki ekran görüntüsüne benzer olmalıdır:

![Değiştirilmiş kayıt seçeneğinin ekran görüntüsü](./media/custom-policy-configure-user-input/signup-with-city-claim-dropdown-example.png)

Başvurunuza geri gönderilen `city` belirteç, talebi içerir.

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

- IEF referansındaki [ClaimsSchema](claimsschema.md) öğesi hakkında daha fazla bilgi edinin.
- [Özel profil edit ilkesinde özel öznitelikleri](custom-policy-custom-attributes.md)nasıl kullanacağınızı öğrenin.
