---
title: Özel ilkeleri kullanarak parola değişikliğini yapılandırma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeleri kullanarak kullanıcıların parolalarını nasıl değiştireceğinizi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 2c351f8a95110a32c53c68c5eb6095918578bc5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189183"
---
# <a name="configure-password-change-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak parola değişikliğini yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C'de (Azure AD B2C), yerel bir hesapla oturum açmış olan kullanıcıların e-posta doğrulaması yla orijinalliklerini kanıtlamalarına gerek kalmadan parolalarını değiştirmelerini sağlayabilirsiniz. Oturum, kullanıcı parola değişikliği akışına ulaştığında sona ererse, yeniden oturum açmaları istenir. Bu makalede, [özel ilkelerde](custom-policy-overview.md)parola değişikliği yapılandırmak için nasıl gösterir. Kullanıcı akışları için self [servis parola sıfırlamayı](user-flow-self-service-password-reset.md) yapılandırmak da mümkündür.

## <a name="prerequisites"></a>Ön koşullar

[Active Directory B2C'deki özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın.

## <a name="add-the-elements"></a>Öğeleri ekleme

1. *TrustframeworkExtensions.xml* dosyanızı açın ve [ClaimsSchema](claimsschema.md) öğesine tanımlayıcı `oldPassword` sıyrık ile aşağıdaki **ClaimType** öğesini ekleyin:

    ```XML
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="oldPassword">
          <DisplayName>Old Password</DisplayName>
          <DataType>string</DataType>
          <UserHelpText>Enter password</UserHelpText>
          <UserInputType>Password</UserInputType>
        </ClaimType>
      </ClaimsSchema>
    </BuildingBlocks>
    ```

2. [ClaimsProvider](claimsproviders.md) öğesi, kullanıcının kimliğini doğrulayan teknik profili içerir. Aşağıdaki talep sağlayıcılarını **Talep Sağlayıcıları** öğesine ekleyin:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="login-NonInteractive-PasswordChange">
            <DisplayName>Local Account SignIn</DisplayName>
            <Protocol Name="OpenIdConnect" />
            <Metadata>
              <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
              <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
              <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
              <Item Key="ProviderName">https://sts.windows.net/</Item>
              <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
              <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
              <Item Key="response_types">id_token</Item>
              <Item Key="response_mode">query</Item>
              <Item Key="scope">email openid</Item>
              <Item Key="UsePolicyInRedirectUri">false</Item>
              <Item Key="HttpBinding">POST</Item>
              <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
              <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
            </Metadata>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
              <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
              <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
              <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
              <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
            <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
              <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
              <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
              <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
              <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
              <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
              <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
            </OutputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
      <ClaimsProvider>
        <DisplayName>Local Account Password Change</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
            <DisplayName>Change password (username)</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
              <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
            </Metadata>
            <CryptographicKeys>
              <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
            </CryptographicKeys>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="objectId" />
            </InputClaims>
            <OutputClaims>
              <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
              <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
              <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
            </OutputClaims>
            <ValidationTechnicalProfiles>
              <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
              <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
            </ValidationTechnicalProfiles>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

    Ön `IdentityExperienceFrameworkAppId` koşul öğreticisinde oluşturduğunuz IdentityExperienceFramework uygulamasının uygulama kimliğini değiştirin. Daha `ProxyIdentityExperienceFrameworkAppId` önce oluşturduğunuz ProxyIdentityExperienceFramework uygulamasının uygulama kimliğiyle değiştirin.

3. [UserJourney](userjourneys.md) öğesi, kullanıcının uygulamanızla etkileşimde yken izlediği yolu tanımlar. Olarak tanımlanan UserJourney ile yoksa `PasswordChange` **UserJourneys** öğesini ekleyin: **UserJourney**

    ```XML
    <UserJourneys>
      <UserJourney Id="PasswordChange">
        <OrchestrationSteps>
          <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
              <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
          </OrchestrationStep>
          <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="3" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
        </OrchestrationSteps>
        <ClientDefinition ReferenceId="DefaultWeb" />
      </UserJourney>
    </UserJourneys>
    ```

4. *TrustFrameworkExtensions.xml* ilke dosyasını kaydedin.
5. Başlangıç paketi ile indirdiğiniz *ProfileEdit.xml* dosyasını kopyalayın ve *profileEditPasswordChange.xml*adını.
6. Yeni dosyayı açın ve **PolicyId** özniteliğini benzersiz bir değerle güncelleştirin. Bu değer, ilkenizin adıdır. Örneğin, *B2C_1A_profile_edit_password_change.*
7. **Oluşturduğunuz** yeni kullanıcı `<DefaultUserJourney>` yolculuğunun kimliğiyle eşleşecek şekilde ReferenceId özniteliğini değiştirin. Örneğin, *PasswordChange*.
8. Yaptığınız değişiklikleri kaydedin.

Örnek politikayı [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change)bulabilirsiniz.

## <a name="test-your-policy"></a>İlkinizi test edin

Uygulamalarınızı Azure AD B2C'de sınarken, azure AD B2C belirtecinin iade edilebilmek için `https://jwt.ms` azure AD B2C belirteci nin bu araçtaki talepleri gözden geçirebilmesi yararlı olabilir.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. Özel İlkeler sayfasında, **Yükle İlkesi'ni**tıklatın.
6. **Varsa politikanın Üzerine Yaz'ı**seçin ve ardından *TrustframeworkExtensions.xml* dosyasını arayın ve seçin.
7. **Karşıya Yükle**'ye tıklayın.
8. *ProfileEditPasswordChange.xml*gibi güvenen taraf dosyası için 5'ten 7'ye kadar olan adımları yineleyin.

### <a name="run-the-policy"></a>İlkeyi çalıştırın

1. Değiştirdiğiniz ilkeyi açın. Örneğin, *B2C_1A_profile_edit_password_change.*
2. **Uygulama**için, daha önce kaydolduğunuz başvurunuzu seçin. Belirteci görmek için **Yanıt URL'sinin** göstermesi `https://jwt.ms`gerekir.
3. **Şimdi çalıştır**’a tıklayın. Daha önce oluşturduğunuz acouunt ile oturum açın. Artık parolayı değiştirme fırsatınız olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C'de özel ilkeleri kullanarak parola karmaşıklığını nasıl yapılandırabileceğiniz](custom-policy-password-complexity.md)hakkında bilgi edinin.
