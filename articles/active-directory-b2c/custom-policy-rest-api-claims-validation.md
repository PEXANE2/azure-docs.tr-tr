---
title: REST API doğrulama olarak değişimleri talep
titleSuffix: Azure AD B2C
description: RESTful hizmetleriyle etkileşimedebilen bir Azure AD B2C kullanıcı yolculuğu oluşturmak için bir yol için bir yol.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: a4902e96cd41a02953b6686b5d52d7912b27809f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80330830"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-to-validate-user-input"></a>Walkthrough: Kullanıcı girişini doğrulamak için REST API taleplerini Azure AD B2C kullanıcı yolculuğunuzda tümleştirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C'nin (Azure AD B2C) temelini oluşturan Kimlik Deneyimi Çerçevesi (IEF), kimlik geliştiricilerin kullanıcı yolculuğunda yeniden bir API ile etkileşimi tümleştirmelerine olanak tanır.  Bu iznin sonunda, kullanıcı girişini doğrulamak için [yeniden kullanılabilir hizmetlerle](custom-policy-rest-api-intro.md) etkileşimedebilen bir Azure AD B2C kullanıcı yolculuğu oluşturabilirsiniz.

Bu senaryoda, kullanıcıların Azure AD B2C kayıt sayfasına bir sadakat numarası girme özelliğini ekleyeceğiz. Bu e-posta ve sadakat numarası kombinasyonunun bir PROMOSYON KODUna eşlenip eşlenmediğini, bu verileri BIR REST API'sine göndererek doğrulayacağız. REST API bu kullanıcı için bir promosyon kodu bulursa, Azure AD B2C'ye döndürülür. Son olarak, promosyon kodu, uygulamanın tüketilmesi için belirteç taleplerine eklenir.

Etkileşimi bir orkestrasyon adımı olarak da tasarlayabilirsiniz. Bu, REST API'sinin ekrandaki verileri doğrulatmayacak ve her zaman talepleri iade etmeyecektir. Daha fazla bilgi için [Walkthrough: Azure AD B2C kullanıcı yolculuğunuzdaki REST API talep alışverişini bir düzenleme adımı olarak tümleştirin.](custom-policy-rest-api-claims-exchange.md)

## <a name="prerequisites"></a>Ön koşullar

- [Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.
- [REST API talep alışverişini Azure AD B2C özel politikanızda](custom-policy-rest-api-intro.md)nasıl entegre edebilirsiniz öğrenin.

## <a name="prepare-a-rest-api-endpoint"></a>REST API bitiş noktası hazırlama

Bu izbin için, bir e-posta adresinin bir sadakat kimliğiyle arka uç sisteminizde kayıtlı olup olmadığını doğrulayan bir REST API'niz olmalıdır. Kayıtlıysa, REST API'si, müşterinin uygulamanız dahilinde mal satın almak için kullanabileceği bir kayıt tanıtım kodu döndürmelidir. Aksi takdirde, REST API bir HTTP 409 hata iletisi döndürmelidir: "Sadakat kimliği '{loyalty ID}' '{email}' e-posta adresiyle ilişkili değildir.".

Aşağıdaki JSON kodu, Azure AD B2C'nin REST API bitiş noktanıza göndereceği verileri gösterir. 

```json
{
    "email": "User email address",
    "language": "Current UI language",
    "loyaltyId": "User loyalty ID"
}
```

REST API'niz verileri doğruladıktan sonra, aşağıdaki JSON verileriyle birlikte bir HTTP 200 (Ok) döndürmesi gerekir:

```json
{
    "promoCode": "24534"
}
```

Doğrulama başarısız olduysa, REST API'nin `userMessage` JSON öğesiyle birlikte bir HTTP 409 (Çakışma) döndürmesi gerekir. IEF, REST `userMessage` API'nin geri döndüğü iddiasını bekliyor. Doğrulama başarısız olursa, bu talep kullanıcıya bir dize olarak sunulacaktır.

```json
{
    "version": "1.0.1",
    "status": 409,
    "userMessage": "LoyaltyId ID '1234' is not associated with 'david@contoso.com' email address."
}
```

REST API bitiş noktasının kurulumu bu makalenin kapsamı dışındadır. Bir Azure [İşlevler](https://docs.microsoft.com/azure/azure-functions/functions-reference) örneği oluşturduk. Tüm Azure işlev koduna [GitHub'dan](https://github.com/azure-ad-b2c/rest-api/tree/master/source-code/azure-function)erişebilirsiniz.

## <a name="define-claims"></a>Talepleri tanımlama

Talep, Azure AD B2C ilke yürütmesi sırasında verilerin geçici olarak depolanmasını sağlar. [İddialar şeması](claimsschema.md) bölümünde ki talepleri beyan edebilirsiniz. 

1. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.
1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. Aşağıdaki iddiaları **ClaimsSchema** öğesine ekleyin.  

```xml
<ClaimType Id="loyaltyId">
  <DisplayName>Your loyalty ID</DisplayName>
  <DataType>string</DataType>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
<ClaimType Id="promoCode">
  <DisplayName>Your promo code</DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
</ClaimType>
  <ClaimType Id="userLanguage">
  <DisplayName>User UI language (used by REST API to return localized error messages)</DisplayName>
  <DataType>string</DataType>
</ClaimType>
```

## <a name="configure-the-restful-api-technical-profile"></a>RESTful API teknik profilini yapılandırma 

[Dinlendirici teknik profil,](restful-technical-profile.md) kendi RESTful hizmetinize ara vermek için destek sağlar. Azure AD B2C, bir `InputClaims` koleksiyondaki RESTful hizmetine veri `OutputClaims` gönderir ve bir koleksiyondaki verileri geri alır. İddia **Sağlayıcıları** öğesini bulun ve aşağıdaki gibi yeni bir talep sağlayıcısı ekleyin:

```xml
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="REST-ValidateProfile">
      <DisplayName>Check loyaltyId Azure Function web hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://your-account.azurewebsites.net/api/ValidateProfile?code=your-code</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <!-- Set AuthenticationType to Basic or ClientCertificate in production environments -->
        <Item Key="AuthenticationType">None</Item>
        <!-- REMOVE the following line in production environments -->
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <!-- Claims sent to your REST API -->
        <InputClaim ClaimTypeReferenceId="loyaltyId" />
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="userLanguage" PartnerClaimType="lang" DefaultValue="{Culture:LCID}" AlwaysUseDefaultValue="true" />
      </InputClaims>
      <OutputClaims>
        <!-- Claims parsed from your REST API -->
        <OutputClaim ClaimTypeReferenceId="promoCode" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Bu örnekte, `userLanguage` JSON yükü içinde `lang` olduğu gibi REST hizmetine gönderilecektir. Talebin `userLanguage` değeri geçerli kullanıcı dili kimliğini içerir. Daha fazla bilgi [için, talep çözümleyicisi](claim-resolver-overview.md)bakın.

Yukarıdaki `AuthenticationType` açıklamaları `AllowInsecureAuthInProduction` belirtin ve bir üretim ortamına taşınırken yaptığınız değişiklikleri belirtin. Yeniden üretim için yeniden ifl eksalarınızı nasıl güvenli hale erdireceğimize yönelik bilgi için Secure [RESTful API'ye](secure-rest-api.md)bakın.

## <a name="validate-the-user-input"></a>Kullanıcı girişini doğrulama

Kayıt sırasında kullanıcının sadakat numarasını elde etmek için, kullanıcının bu verileri ekrana girmesine izin vermelisiniz. **LoyaltyId** çıktı iddiasını, varolan kaydolma teknik profil bölümünün `OutputClaims` öğesine ekleyerek kaydolma sayfasına ekleyin. Taleplerin ekranda sunulduğu sırayı denetlemek için çıktı taleplerinin tüm listesini belirtin.  

Kayıt teknik profiline doğrulama teknik profil referansı ekleyin. `REST-ValidateProfile` Yeni doğrulama teknik profili, temel ilkede `<ValidationTechnicalProfiles>` tanımlanan koleksiyonun üst bölümüne eklenir. Bu davranış, yalnızca başarılı doğrulamadan sonra Azure AD B2C'nin dizinde hesabı oluşturmak için harekete geçtiği anlamına gelir.   

1. İddia **Sağlayıcıları** öğesini bulun. Aşağıdaki gibi yeni bir talep sağlayıcısı ekleyin:

    ```xml
    <ClaimsProvider>
      <DisplayName>Local Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true"/>
            <OutputClaim ClaimTypeReferenceId="displayName"/>
            <OutputClaim ClaimTypeReferenceId="givenName"/>
            <OutputClaim ClaimTypeReferenceId="surName"/>
            <!-- Required to present the text box to collect the data from the user -->
            <OutputClaim ClaimTypeReferenceId="loyaltyId"/>
            <!-- Required to pass the promoCode returned from "REST-ValidateProfile" 
            to subsequent orchestration steps and token issuance-->
            <OutputClaim ClaimTypeReferenceId="promoCode" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="REST-ValidateProfile" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Self Asserted</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SelfAsserted-Social">
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" />
          </InputClaims>
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="displayName"/>
            <OutputClaim ClaimTypeReferenceId="givenName"/>
            <OutputClaim ClaimTypeReferenceId="surname"/>
            <!-- Required to present the text box to collect the data from the user -->
            <OutputClaim ClaimTypeReferenceId="loyaltyId"/>
            <!-- Required to pass the promoCode returned from "REST-ValidateProfile" 
            to subsequent orchestration steps and token issuance-->
            <OutputClaim ClaimTypeReferenceId="promoCode" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="REST-ValidateProfile"/>
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

## <a name="include-a-claim-in-the-token"></a>Belirteci bir talep ekleme 

Promosyon kodu talebini güvenen taraf uygulamasına geri döndürmek <em> `SocialAndLocalAccounts/` </em> için, dosyaya bir çıktı talebi ekleyin. Çıktı talebi, talebin başarılı bir kullanıcı yolculuğundan sonra jetonun içine eklenmesine olanak sağlar ve uygulamaya gönderilir. Çıktı talebi olarak eklemek için güvenilen `promoCode` taraf bölümündeki teknik profil öğesini değiştirin.
 
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
      <OutputClaim ClaimTypeReferenceId="promoCode" DefaultValue="" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve Azure AD kiracınızı içeren dizin seçerek Azure AD kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
1. **Kimlik Deneyimi Çerçevesi'ni**seçin.
1. **Özel Politika Yükle'yi**seçin ve sonra değiştirdiğiniz ilke dosyalarını yükleyin: *TrustFrameworkExtensions.xml*ve *SignUpOrSignin.xml*. 
1. Yüklediğiniz kaydolma veya kaydolma ilkesini seçin ve **Şimdi Çalıştır** düğmesini tıklatın.
1. Bir e-posta adresi kullanarak kaydolmalısınız.
1. **Şimdi Kaydol** bağlantısına tıklayın.
1. Sadakat **kimliğinizde**, 1234 yazın ve **Devam et'i**tıklatın. Bu noktada, bir doğrulama hatası iletisi almalısınız.
1. Başka bir değere değiştirin ve **Devam et'i**tıklatın.
1. Başvurunuza geri gönderilen `promoCode` belirteç, talebi içerir.

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1584295703,
  "nbf": 1584292103,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1584292103,
  "auth_time": 1584292103,
  "name": "Emily Smith",
  "email": "emily@outlook.com",
  "given_name": "Emily",
  "family_name": "Smith",
  "promoCode": "84362"
  ...
}
```

## <a name="next-steps"></a>Sonraki adımlar

API'lerinizi nasıl güvene aldığınızı öğrenmek için aşağıdaki makalelere bakın:

- [Walkthrough: Azure AD B2C kullanıcı yolculuğunuzdaki REST API taleplerini bir orkestrasyon adımı olarak tümleştirin](custom-policy-rest-api-claims-exchange.md)
- [RESTful API'nizi emniyete alasınız](secure-rest-api.md)
- [Referans: RESTful teknik profili](restful-technical-profile.md)
