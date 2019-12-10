---
title: Özel ilkeler kullanarak parola karmaşıklığını yapılandırma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 'de özel bir ilke kullanarak parola karmaşıklığı gereksinimlerini yapılandırma.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e8718a04f9d63897b2d2472dd0cdffb196c41435
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74949799"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanarak parola karmaşıklığını yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) ' de, bir hesap oluştururken bir kullanıcı tarafından temin edilen parolaların karmaşıklık gereksinimlerini yapılandırabilirsiniz. Varsayılan olarak, Azure AD B2C **güçlü** parolalar kullanır. Bu makalede, [özel ilkelerde](active-directory-b2c-overview-custom.md)parola karmaşıklığının nasıl yapılandırılacağı gösterilir. [Kullanıcı akışlarında](active-directory-b2c-reference-password-complexity.md)parola karmaşıklığı yapılandırmak da mümkündür.

## <a name="prerequisites"></a>Önkoşullar

[Active Directory B2C özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md)bölümündeki adımları uygulayın.

## <a name="add-the-elements"></a>Öğeleri ekleyin

1. Başlangıç paketiyle indirdiğiniz *Signuporsignın. xml* dosyasını kopyalayın ve bu dosyayı *Singuporsignınpasswordkarmaşıklık. xml*olarak adlandırın.
2. *Singuporsignınpasswordkarmaşıklık. xml* dosyasını açın ve **PolicyId** ve **publicpolicyuri** ' i yeni bir ilke adıyla değiştirin. Örneğin, *B2C_1A_signup_signin_password_complexity*.
3. Aşağıdaki **ClaimType** öğelerini `newPassword` ve `reenterPassword`tanımlayıcılarıyla ekleyin:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="newPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
      <ClaimType Id="reenterPassword">
        <InputValidationReference Id="PasswordValidation" />
      </ClaimType>
    </ClaimsSchema>
    ```

4. [Koşullarda](predicates.md) `IsLengthRange` veya `MatchesRegex`metot türleri vardır. `MatchesRegex` türü bir normal ifadeyle eşleştirmek için kullanılır. `IsLengthRange` türü en az ve en fazla dize uzunluğu alır. Aşağıdaki **koşul** öğeleriyle birlikte yoksa **buildingblocks** öğesine bir **doðrulama** öğesi ekleyin:

    ```XML
    <Predicates>
      <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be a pin.">
        <Parameters>
          <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Length" Method="IsLengthRange" HelpText="The password must be between 8 and 16 characters.">
        <Parameters>
          <Parameter Id="Minimum">8</Parameter>
          <Parameter Id="Maximum">16</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

5. Her **ınputvalidation** öğesi, tanımlanan **koşul** öğeleri kullanılarak oluşturulur. Bu öğe, `and` ve `or`benzer Boole toplamaları gerçekleştirmenize olanak tanır. Aşağıdaki **ınputvalidation** öğesiyle birlikte yoksa **Buildingblocks** öğesine **inputdoğrulamaları** öğesi ekleyin:

    ```XML
    <InputValidations>
      <InputValidation Id="PasswordValidation">
        <PredicateReferences Id="LengthGroup" MatchAtLeast="1">
          <PredicateReference Id="Length" />
        </PredicateReferences>
        <PredicateReferences Id="3of4" MatchAtLeast="3" HelpText="You must have at least 3 of the following character classes:">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </InputValidation>
    </InputValidations>
    ```

6. **Policyprofile** teknik profilinin aşağıdaki öğeleri içerdiğinden emin olun:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignIn"/>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect"/>
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
        </InputClaims>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName"/>
          <OutputClaim ClaimTypeReferenceId="givenName"/>
          <OutputClaim ClaimTypeReferenceId="surname"/>
          <OutputClaim ClaimTypeReferenceId="email"/>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub"/>
      </TechnicalProfile>
    </RelyingParty>
    ```

7. İlke dosyasını kaydedin.

## <a name="test-your-policy"></a>İlkenizi test etme

Azure AD B2C ' de Uygulamalarınızı sınarken, içindeki talepleri gözden geçirebilmek için Azure AD B2C belirtecinin `https://jwt.ms` geri döndürüldüğünden yararlı olabilir.

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. Özel Ilkeler sayfasında, **Ilkeyi karşıya yükle**' ye tıklayın.
6. Varsa **Ilkenin üzerine yaz**' ı seçin ve ardından *Singuporsignınpasswordkarmaşıklık. xml* dosyasını arayıp seçin.
7. **Karşıya Yükle**'ye tıklayın.

### <a name="run-the-policy"></a>İlkeyi çalıştırma

1. Değiştirdiğiniz ilkeyi açın. Örneğin, *B2C_1A_signup_signin_password_complexity*.
2. **Uygulama**için, daha önce kaydetmiş olduğunuz uygulamanızı seçin. Belirteci görmek için, **yanıt URL 'si** `https://jwt.ms`göstermelidir.
3. **Şimdi çalıştır**’a tıklayın.
4. **Şimdi kaydolun**' ı seçin, bir e-posta adresi girin ve yeni bir parola girin. Yönergeler, parola kısıtlamalarına göre sunulmuştur. Kullanıcı bilgilerini girmeyi ve ardından **Oluştur**' u tıklatın. Döndürülen belirtecin içeriğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C özel ilkeleri kullanarak parola değişikliğini yapılandırmayı](active-directory-b2c-reference-password-change-custom.md)öğrenin.


