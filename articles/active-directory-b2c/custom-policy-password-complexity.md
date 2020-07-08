---
title: Özel ilkeler kullanarak parola karmaşıklığını yapılandırma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C 'de özel bir ilke kullanarak parola karmaşıklığı gereksinimlerini yapılandırma.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4ab196e894fc53b1243ac363f9863d5c7d4e328f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85389012"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanarak parola karmaşıklığını yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) ' de, bir hesap oluştururken bir kullanıcı tarafından temin edilen parolaların karmaşıklık gereksinimlerini yapılandırabilirsiniz. Varsayılan olarak, Azure AD B2C **güçlü** parolalar kullanır. Bu makalede, [özel ilkelerde](custom-policy-overview.md)parola karmaşıklığının nasıl yapılandırılacağı gösterilir. [Kullanıcı akışlarında](user-flow-password-complexity.md)parola karmaşıklığı yapılandırmak da mümkündür.

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle çalışmaya başlama](custom-policy-get-started.md)bölümündeki adımları uygulayın. Kaydolma ve yerel hesaplarla oturum açma için çalışan bir özel ilkenize sahip olmanız gerekir.


## <a name="add-the-elements"></a>Öğeleri ekleyin

Parola karmaşıklığını yapılandırmak için, `newPassword` ve `reenterPassword` [talep türlerini](claimsschema.md) [koşul doğrulamaları](predicates.md#predicatevalidations)başvurusuyla geçersiz kılın. Predicatevalidation öğesi, bir talep türüne uygulanabilen bir kullanıcı girişi doğrulaması oluşturmak için bir koşullar kümesi gruplandırır. İlkenizin uzantıları dosyasını açın. Örneğin, <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em> .

1. [Buildingblocks](buildingblocks.md) öğesi için arama yapın. Öğe yoksa, ekleyin.
1. [Claimsschema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. `newPassword`Ve `reenterPassword` taleplerini **Claimsschema** öğesine ekleyin.

    ```xml
    <ClaimType Id="newPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    <ClaimType Id="reenterPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    ```

1. [Koşullar](predicates.md) , bir talep türünün değerini denetlemek için temel bir doğrulama tanımlar ve true veya false değerini döndürür. Doğrulama, belirtilen bir yöntem öğesi ve yöntemiyle ilgili bir dizi parametre kullanılarak yapılır. Aşağıdaki koşulları, öğesinin kapanışından hemen sonra **Buildingblocks** öğesine ekleyin `</ClaimsSchema>` :

    ```xml
    <Predicates>
      <Predicate Id="LengthRange" Method="IsLengthRange">
        <UserHelpText>The password must be between 6 and 64 characters.</UserHelpText>
        <Parameters>
          <Parameter Id="Minimum">6</Parameter>
          <Parameter Id="Maximum">64</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Lowercase" Method="IncludesCharacters">
        <UserHelpText>a lowercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">a-z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Uppercase" Method="IncludesCharacters">
        <UserHelpText>an uppercase letter</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">A-Z</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Number" Method="IncludesCharacters">
        <UserHelpText>a digit</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">0-9</Parameter>
        </Parameters>
      </Predicate>
      <Predicate Id="Symbol" Method="IncludesCharacters">
        <UserHelpText>a symbol</UserHelpText>
        <Parameters>
          <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
        </Parameters>
      </Predicate>
    </Predicates>
    ```

1. Aşağıdaki koşul doğrulamaları, öğesinin kapanışından hemen sonra **Buildingblocks** öğesine ekleyin `</Predicates>` :

    ```xml
    <PredicateValidations>
      <PredicateValidation Id="CustomPassword">
        <PredicateGroups>
          <PredicateGroup Id="LengthGroup">
            <PredicateReferences MatchAtLeast="1">
              <PredicateReference Id="LengthRange" />
            </PredicateReferences>
          </PredicateGroup>
          <PredicateGroup Id="CharacterClasses">
            <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
            <PredicateReferences MatchAtLeast="3">
              <PredicateReference Id="Lowercase" />
              <PredicateReference Id="Uppercase" />
              <PredicateReference Id="Number" />
              <PredicateReference Id="Symbol" />
            </PredicateReferences>
          </PredicateGroup>
        </PredicateGroups>
      </PredicateValidation>
    </PredicateValidations>
    ```

1. Aşağıdaki teknik profiller, Azure Active Directory verileri okuyan ve yazan [Teknik profillerdir Active Directory](active-directory-technical-profile.md). Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. `PersistedClaims`Güçlü parola ilkesini devre dışı bırakmak için kullanın. **Claimsproviders** öğesini bulun.  Aşağıdaki talep sağlayıcılarını şu şekilde ekleyin:

    ```xml
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
          <PersistedClaims>
            <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration, DisableStrongPassword"/>
          </PersistedClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. İlke dosyasını kaydedin.

## <a name="test-your-policy"></a>İlkenizi test etme

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
5. Özel Ilkeler sayfasında, **Ilkeyi karşıya yükle**' ye tıklayın.
6. Varsa **Ilkenin üzerine yaz**' ı seçin ve ardından *TrustFrameworkExtensions.xml* dosyasını arayıp seçin.
7. **Karşıya Yükle**'ye tıklayın.

### <a name="run-the-policy"></a>İlkeyi çalıştırma

1. Kaydolma veya oturum açma ilkesini açın. Örneğin, *B2C_1A_signup_signin*.
2. **Uygulama**için, daha önce kaydetmiş olduğunuz uygulamanızı seçin. Belirteci görmek için, **yanıt URL 'sinin** gösterilmesi gerekir `https://jwt.ms` .
3. **Şimdi çalıştır**’a tıklayın.
4. **Şimdi kaydolun**' ı seçin, bir e-posta adresi girin ve yeni bir parola girin. Yönergeler, parola kısıtlamalarına göre sunulmuştur. Kullanıcı bilgilerini girmeyi ve ardından **Oluştur**' u tıklatın. Döndürülen belirtecin içeriğini görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C özel ilkeleri kullanarak parola değişikliğini yapılandırmayı](custom-policy-password-change.md)öğrenin.
- IEF başvurusunda [koşullar](predicates.md) ve [predicatedoğrulamaları](predicates.md#predicatevalidations) öğeleri hakkında daha fazla bilgi edinin.
