---
title: Özel ilkeler kullanarak LinkedIn hesabıyla oturum açma'yı ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak Bir LinkedIn hesabıyla kaydolma ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 80bd1b65d04ea49fc742033e1850d95a85021c9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188180"
---
# <a name="set-up-sign-in-with-a-linkedin-account-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak linkedin hesabıyla oturum açma'yı ayarlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak bir LinkedIn hesabından kullanıcılar için oturum açma yı nasıl etkinleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure Active [Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.
- LinkedIn hesabı - Zaten hesabınız yoksa, [bir hesap oluşturun.](https://www.linkedin.com/start/join)
- LinkedIn Page - Bir sonraki bölümde oluşturduğunuz LinkedIn uygulamasıyla ilişkilendirmek için bir [LinkedIn Sayfasına](https://www.linkedin.com/help/linkedin/answer/710/creating-a-linkedin-company-page) ihtiyacınız var.

## <a name="create-an-application"></a>Uygulama oluşturma

LinkedIn'i Azure AD B2C'de kimlik sağlayıcısı olarak kullanmak için bir LinkedIn uygulaması oluşturmanız gerekir.

### <a name="create-app"></a>Uygulama oluşturma

1. LinkedIn hesap kimlik bilgilerinizle [Birlikte LinkedIn uygulama yönetimi](https://www.linkedin.com/secure/developer?newapp=) web sitesinde oturum açın.
1. **Uygulama oluştur**'u seçin.
1. Bir **Uygulama adı**girin.
1. LinkedIn sayfa adına karşılık gelen bir **Şirket** adı girin. Zaten bir Bağlantı Sayfanız yoksa Bir LinkedIn Sayfası oluşturun.
1. (İsteğe bağlı) Bir **Gizlilik ilkesi URL'si**girin. Geçerli bir URL olmalıdır, ancak ulaşılabilir bir bitiş noktası olması gerekmez.
1. Bir **İş e-postası**girin.
1. Uygulama **logosu** resmi yükleyin. Logo görüntüsü kare olmalı ve boyutları en az 100x100 piksel olmalıdır.
1. **Ürünler** bölümünde varsayılan ayarları bırakın.
1. **Yasal terimlerle**sunulan bilgileri gözden geçirin. Şartları kabul ederseniz, kutuyu işaretleyin.
1. **Uygulama oluştur**'u seçin.

### <a name="configure-auth"></a>Kimlik doğrulamasını yapılandırma

1. **Auth** sekmesini seçin.
1. **İstemci Kimliğini**kaydedin.
1. **Müşteri Sırrını**ortaya çıkarın ve kaydedin.
1. **OAuth 2.0 ayarları**altında, aşağıdaki **Yönlendirme URL'sini**ekleyin. Kiracınızın adıyla değiştirin. `your-tenant` Azure AD B2C'de büyük harflerle tanımlanmış olsa bile kiracı adı için **tüm küçük harfleri** kullanın.

    `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/oauth2/authresp`

## <a name="create-a-policy-key"></a>İlke anahtarı oluşturma

Azure AD B2C kiracınızda daha önce kaydettiğiniz istemci sırrını depolamanız gerekir.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve kiracınızı içeren dizini seçin.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. Genel Bakış sayfasında Kimlik **Deneyimi Çerçevesi'ni**seçin.
5. **İlke tuşlarını** seçin ve sonra **Ekle'yi**seçin.
6. **Seçenekler**için `Manual`, seçin.
7. İlke anahtarı için bir **Ad** girin. Örneğin, `LinkedInSecret`. *B2C_1A_* önek anahtarınızın adına otomatik olarak eklenir.
8. **Gizli**' de, daha önce kaydettiğiniz istemci sırrını girin.
9. **Anahtar kullanımı**için `Signature`.
10. **Oluştur'u**tıklatın.

## <a name="add-a-claims-provider"></a>Talep sağlayıcı ekleme

Kullanıcıların linkedin hesabı kullanarak oturum açmasını istiyorsanız, hesabı Azure AD B2C'nin bir bitiş noktası aracılığıyla iletişim kurabileceği bir talep sağlayıcısı olarak tanımlamanız gerekir. Bitiş noktası, azure AD B2C tarafından belirli bir kullanıcının kimliğinin doğrulanmış olduğunu doğrulamak için kullanılan bir talep kümesi sağlar.

Bir LinkedIn hesabını, politikanızın uzantı dosyasındaki **Talep Sağlayıcıları** öğesine ekleyerek talep sağlayıcıolarak tanımlayın.

1. *SocialAndLocalAccounts/**TrustFrameworkExtensions.xml*** dosyasını editörünüzde açın. Bu dosya, ön koşullardan birinin bir parçası olarak indirdiğiniz [Özel ilke başlangıç paketinde][starter-pack] yer almaktadır.
1. İddia **Sağlayıcıları** öğesini bulun. Yoksa, kök öğesinin altına ekleyin.
1. Aşağıdaki gibi yeni bir **ClaimsProvider** ekleyin:

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v2/me</Item>
            <Item Key="scope">r_emailaddress r_liteprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="external_user_identity_claim_id">id</Item>
            <Item Key="BearerTokenTransmissionMethod">AuthorizationHeader</Item>
            <Item Key="ResolveJsonPathsInJsonTokens">true</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <InputClaims />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName.localized" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName.localized" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="ExtractGivenNameFromLinkedInResponse" />
            <OutputClaimsTransformation ReferenceId="ExtractSurNameFromLinkedInResponse" />
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. **client_id** değerini daha önce kaydettiğiniz LinkedIn uygulamasının istemci kimliğiyle değiştirin.
1. Dosyayı kaydedin.

### <a name="add-the-claims-transformations"></a>Talep dönüşümlerini ekleme

LinkedIn teknik profili, **ExtractGivenNameFromLinkedInResponse** ve **ExtractSurNameFromLinkedInResponse** talepleri dönüşümleri Talepleri Dönüşümler listesine eklenecek gerektirir. Dosyanızda tanımlı bir **ClaimsTransformations** öğesi yoksa, aşağıda gösterildiği gibi üst XML öğelerini ekleyin. Talep dönüşümleri de **nullStringClaim**adlı tanımlanan yeni bir talep türü gerekir.

*TrustFrameworkExtensions.xml* dosyasının üst **yakınBuildingBlocks** öğesini ekleyin. Bir örnek için *TrustFrameworkBase.xml* bakın.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <!-- Claim type needed for LinkedIn claims transformations -->
    <ClaimType Id="nullStringClaim">
      <DisplayName>nullClaim</DisplayName>
      <DataType>string</DataType>
      <AdminHelpText>A policy claim to store output values from ClaimsTransformations that aren't useful. This claim should not be used in TechnicalProfiles.</AdminHelpText>
      <UserHelpText>A policy claim to store output values from ClaimsTransformations that aren't useful. This claim should not be used in TechnicalProfiles.</UserHelpText>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <!-- Claim transformations needed for LinkedIn technical profile -->
    <ClaimsTransformation Id="ExtractGivenNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
    <ClaimsTransformation Id="ExtractSurNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="surname" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

### <a name="upload-the-extension-file-for-verification"></a>Doğrulama için uzantı dosyasını yükleme

Azure AD B2C'nin LinkedIn hesabınızla nasıl iletişim kurabileceğinizi bilmesi için artık yapılandırılan bir ilkeniz var. Şimdiye kadar herhangi bir sorun olmadığını doğrulamak için politikanızın uzantı dosyasını yüklemeyi deneyin.

1. Azure AD B2C kiracınızdaki **Özel İlkeler** sayfasında **Yükle İlkesi'ni**seçin.
2. **Varsa politikanın Üzerine Yaz'ı**etkinleştirin ve *ardından TrustFrameworkExtensions.xml* dosyasına göz atın ve seçin.
3. **Karşıya Yükle**'ye tıklayın.

## <a name="register-the-claims-provider"></a>Talep sağlayıcıyı kaydedin

Bu noktada, kimlik sağlayıcı ayarlanmış, ancak kaydolma veya kaydolma ekranlarının hiçbirinde kullanılamaz. Kullanılabilir hale getirmek için, varolan bir şablon kullanıcı yolculuğunun bir kopyasını oluşturur sunuz ve ardından LinkedIn kimlik sağlayıcısına sahip olacak şekilde değiştirebilirsiniz.

1. Başlangıç paketinde *TrustFrameworkBase.xml* dosyasını açın.
2. **UserJourney** öğesinin tüm içeriğini bulun `Id="SignUpOrSignIn"`ve kopyalayın.
3. *TrustFrameworkExtensions.xml'i* açın ve **UserJourneys** öğesini bulun. Öğe yoksa, bir tane ekleyin.
4. **UserJourneys** öğesinin alt öğesi olarak kopyaladığınız **UserJourney** öğesinin tüm içeriğini yapıştırın.
5. Kullanıcı yolculuğunun kimliğini yeniden adlandırın. Örneğin, `SignUpSignInLinkedIn`.

### <a name="display-the-button"></a>Düğmeyi görüntüleme

**ClaimsProviderSelection** öğesi, kaydolma veya oturum açma ekranındaki bir kimlik sağlayıcı düğmesine benzer. LinkedIn hesabı için Bir **ClaimsProviderSelection** öğesi eklerseniz, bir kullanıcı sayfaya düştüğünde yeni bir düğme belirar.

1. Oluşturduğunuz kullanıcı yolculuğunda `Order="1"` yer alan **OrchestrationStep** öğesini bulun.
2. **ClaimsProviderselections**altında, aşağıdaki öğeyi ekleyin. **TargetClaimsExchangeId** değerini uygun bir değere ayarlayın, örneğin: `LinkedInExchange`

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Düğmeyi eyleme bağlama

Artık bir düğmeniz olduğuna göre, düğmeyi bir eyleme bağlamanız gerekir. Bu durumda, işlem Azure AD B2C'nin bir LinkedIn hesabıyla iletişim kurarak bir belirteç almasıdır.

1. Kullanıcı yolculuğunda yer `Order="2"` alan **OrchestrationStep'i** bulun.
2. **TargetClaimsExchangeId**için kullandığınız kimlik için aynı değeri kullandığınızdan emin olmak için aşağıdaki **ClaimsExchange** öğesini ekleyin:

    ```XML
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
    ```

    **TechnicalProfileReferenceId'in** değerini daha önce oluşturduğunuz teknik profilin kimliğiyle güncelleştirin. Örneğin, `LinkedIn-OAUTH`.

3. *TrustFrameworkExtensions.xml* dosyasını kaydedin ve doğrulama için yeniden yükleyin.

## <a name="create-an-azure-ad-b2c-application"></a>Azure AD B2C uygulaması oluşturma

Azure AD B2C ile iletişim, B2C kiracınıza kaydettiğiniz bir uygulama aracılığıyla gerçekleşir. Bu bölümde, daha önce yapmadıysanız bir test uygulaması oluşturmak için tamamlayabileceğiniz isteğe bağlı adımlar listeleniz.

[!INCLUDE [active-directory-b2c-appreg-idp](../../includes/active-directory-b2c-appreg-idp.md)]

## <a name="update-and-test-the-relying-party-file"></a>Güvenilen parti dosyasını güncelleştirme ve test etme

Oluşturduğunuz kullanıcı yolculuğunu başlatan güvenen taraf (RP) dosyasını güncelleştirin.

1. Çalışma dizininizde *SignUpOrSignIn.xml'in* bir kopyasını yapın ve yeniden adlandırın. Örneğin, *SignUpSignInLinkedIn.xml*olarak yeniden adlandırın.
2. Yeni dosyayı açın ve **TrustFrameworkPolicy** için **PolicyId** özniteliğinin değerini benzersiz bir değerle güncelleştirin. Örneğin, `SignUpSignInLinkedIn`.
3. İlke için URI ile **PublicPolicyUri** değerini güncelleştirin. Örneğin,`http://contoso.com/B2C_1A_signup_signin_linkedin`
4. Oluşturduğunuz yeni kullanıcı yolculuğunun kimliğiyle eşleşecek şekilde **DefaultUserJourney'deki** **ReferenceId** özniteliğinin değerini güncelleştirin (SignUpSignLinkedIn).
5. Değişikliklerinizi kaydedin, dosyayı yükleyin ve ardından listedeki yeni ilkeyi seçin.
6. Oluşturduğunuz Azure AD B2C uygulamasının Select **uygulama** alanında seçildiğinden emin olun ve **şimdi Çalıştır'ı**tıklatarak uygulamayı test edin.

## <a name="migration-from-v10-to-v20"></a>v1.0'dan v2.0'a geçiş

LinkedIn son zamanlarda [v1.0 v2.0 kendi API'ler güncelledi.](https://engineering.linkedin.com/blog/2018/12/developer-program-updates) Varolan yapılandırmanızı yeni yapılandırmaya geçirmek için, teknik profildeki öğeleri güncelleştirmek için aşağıdaki bölümlerdeki bilgileri kullanın.

### <a name="replace-items-in-the-metadata"></a>Meta verilerdeki öğeleri değiştirme

**TechnicalProfile'ın**varolan **Meta veri** öğesinde aşağıdaki **Öğe** öğelerini güncelleştirin:

```XML
<Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
<Item Key="scope">r_emailaddress r_basicprofile</Item>
```

Hedef:

```XML
<Item Key="ClaimsEndpoint">https://api.linkedin.com/v2/me</Item>
<Item Key="scope">r_emailaddress r_liteprofile</Item>
```

### <a name="add-items-to-the-metadata"></a>Meta verilere öğe ekleme

**TechnicalProfile'ın** **Meta verilerine** aşağıdaki **Öğe** öğelerini ekleyin:

```XML
<Item Key="external_user_identity_claim_id">id</Item>
<Item Key="BearerTokenTransmissionMethod">AuthorizationHeader</Item>
<Item Key="ResolveJsonPathsInJsonTokens">true</Item>
```

### <a name="update-the-outputclaims"></a>Çıktı Taleplerini Güncelleştir

**TechnicalProfile'ın**mevcut **Çıktı Talepleri'nde** aşağıdaki **OutputClaim** öğelerini güncelleyin:

```XML
<OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
<OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
```

Hedef:

```XML
<OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName.localized" />
<OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName.localized" />
```

### <a name="add-new-outputclaimstransformation-elements"></a>Yeni ÇıktılarEklemeTalepleriDönüşüm öğeleri

**Teknik Profilin** **Çıktı TalepleriDönüşümleri'nde,** aşağıdaki **OutputClaimsTransformation** öğelerini ekleyin:

```XML
<OutputClaimsTransformation ReferenceId="ExtractGivenNameFromLinkedInResponse" />
<OutputClaimsTransformation ReferenceId="ExtractSurNameFromLinkedInResponse" />
```

### <a name="define-the-new-claims-transformations-and-claim-type"></a>Yeni talep dönüşümlerini ve talep türünü tanımlayın

Son adımda, tanımlanması gereken yeni talep dönüşümleri eklediniz. Talep dönüşümlerini tanımlamak için, bunları **Talep Dönüşümleri**listesine ekleyin. Dosyanızda tanımlı bir **ClaimsTransformations** öğesi yoksa, aşağıda gösterildiği gibi üst XML öğelerini ekleyin. Talep dönüşümleri de **nullStringClaim**adlı tanımlanan yeni bir talep türü gerekir.

**BuildingBlocks** öğesi dosyanın üst yanına eklenmelidir. *TrustframeworkBase.xml'e* örnek olarak bakın.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <!-- Claim type needed for LinkedIn claims transformations -->
    <ClaimType Id="nullStringClaim">
      <DisplayName>nullClaim</DisplayName>
      <DataType>string</DataType>
      <AdminHelpText>A policy claim to store unuseful output values from ClaimsTransformations. This claim should not be used in a TechnicalProfiles.</AdminHelpText>
      <UserHelpText>A policy claim to store unuseful output values from ClaimsTransformations. This claim should not be used in a TechnicalProfiles.</UserHelpText>
    </ClaimType>
  </ClaimsSchema>

  <ClaimsTransformations>
    <!-- Claim transformations needed for LinkedIn technical profile -->
    <ClaimsTransformation Id="ExtractGivenNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
    <ClaimsTransformation Id="ExtractSurNameFromLinkedInResponse" TransformationMethod="GetSingleItemFromJson">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputJson" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="nullStringClaim" TransformationClaimType="key" />
        <OutputClaim ClaimTypeReferenceId="surname" TransformationClaimType="value" />
      </OutputClaims>
    </ClaimsTransformation>
  </ClaimsTransformations>
</BuildingBlocks>
```

### <a name="obtain-an-email-address"></a>E-posta adresi alma

V1.0'dan v2.0'a LinkedIn geçişinin bir parçası olarak, e-posta adresini almak için başka bir API'ye ek bir çağrı gereklidir. Kayıt sırasında e-posta adresini almanız gerekiyorsa, aşağıdakileri yapın:

1. Azure AD B2C'nin linkedin ile federate kurarak kullanıcının oturum açmasına izin vermek için yukarıdaki adımları tamamlayın. Federasyonun bir parçası olarak Azure AD B2C LinkedIn için erişim jetonunu alır.
2. LinkedIn erişim jetonunu bir hak talebine kaydedin. [Burada talimatları bakın.](idp-pass-through-custom.md)
3. LinkedIn `/emailAddress` API'sine istekte bulunan aşağıdaki talep sağlayıcısını ekleyin. Bu isteği yetkilendirmek için LinkedIn erişim jetonuna ihtiyacınız var.

    ```XML
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="API-LinkedInEmail">
          <DisplayName>Get LinkedIn email</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
              <Item Key="ServiceUrl">https://api.linkedin.com/v2/emailAddress?q=members&amp;projection=(elements*(handle~))</Item>
              <Item Key="AuthenticationType">Bearer</Item>
              <Item Key="UseClaimAsBearerToken">identityProviderAccessToken</Item>
              <Item Key="SendClaimsIn">Url</Item>
              <Item Key="ResolveJsonPathsInJsonTokens">true</Item>
          </Metadata>
          <InputClaims>
              <InputClaim ClaimTypeReferenceId="identityProviderAccessToken" />
          </InputClaims>
          <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="elements[0].handle~.emailAddress" />
          </OutputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Kullanıcı yolculuğunuza aşağıdaki düzenleme adımını ekleyin, böylece bir kullanıcı LinkedIn'i kullanarak işe girdiğinde API talep sağlayıcısı tetiklenir. Numarayı uygun şekilde `Order` güncelleştirdiğinden emin olun. LinkedIn teknik profilini tetikleyen düzenleme adımından hemen sonra bu adımı ekleyin.

    ```XML
    <!-- Extra step for LinkedIn to get the email -->
    <OrchestrationStep Order="3" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
          <Value>identityProvider</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="false">
          <Value>identityProvider</Value>
          <Value>linkedin.com</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="GetEmail" TechnicalProfileReferenceId="API-LinkedInEmail" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ```

Kayıt sırasında LinkedIn'den e-posta adresi nin alınması isteğe bağlıdır. E-postayı LinkedIn'den almayı değil de kaydolma sırasında bir tane isterseniz, kullanıcının e-posta adresini el ile girmesi ve doğrulanması gerekir.

LinkedIn kimlik sağlayıcısını kullanan bir ilkenin tam örneği için [Özel İlke Başlangıç Paketi'ne](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/linkedin-identity-provider)bakın.

<!-- Links - EXTERNAL -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
