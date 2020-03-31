---
title: Özel ilkeleri kullanarak parola karmaşıklığını yapılandırma
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilke kullanarak parola karmaşıklığı gereksinimlerini yapılandırma.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b16790e288f6569f08ce14e5a7c751bbd8083faf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79138443"
---
# <a name="configure-password-complexity-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak parola karmaşıklığını yapılandırma

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Etkin Dizin B2C'de (Azure AD B2C), hesap oluştururken kullanıcı tarafından sağlanan parolalar için karmaşıklık gereksinimlerini yapılandırabilirsiniz. Varsayılan olarak, Azure AD B2C **Güçlü** parolalar kullanır. Bu makalede, [özel ilkelerde](custom-policy-overview.md)parola karmaşıklığını nasıl yapılandırabileceğiniz gösterilmektedir. Kullanıcı akışlarında parola karmaşıklığını yapılandırmak da [mümkündür.](user-flow-password-complexity.md)

## <a name="prerequisites"></a>Ön koşullar

[Özel ilkelerle başlayın](custom-policy-get-started.md)adımlarını tamamlayın. Yerel hesaplarla kaydolmak ve oturum açmak için çalışan bir özel politikanız olmalıdır.


## <a name="add-the-elements"></a>Öğeleri ekleme

Parola karmaşıklığını yapılandırmak için, `newPassword` `reenterPassword` [yüklem doğrulamalarına](predicates.md#predicatevalidations)başvuruda bulunarak ve [talep türlerini](claimsschema.md) geçersiz kılın. Yüklemler öğesi, bir talep türüne uygulanabilecek bir kullanıcı giriş doğrulaması oluşturmak için bir yüklem kümesini gruplandırmaz. İlkinizin uzantılar dosyasını açın. Örneğin, <em> `SocialAndLocalAccounts/` </em>.

1. [BuildingBlocks](buildingblocks.md) öğesini arayın. Öğe yoksa, ekleyin.
1. [ClaimsSchema](claimsschema.md) öğesini bulun. Öğe yoksa, ekleyin.
1. `newPassword` `reenterPassword` **İddialarSchema** öğesine ve iddiaları ekleyin.

    ```XML
    <ClaimType Id="newPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    <ClaimType Id="reenterPassword">
      <PredicateValidationReference Id="CustomPassword" />
    </ClaimType>
    ```

1. [Yüklemler,](predicates.md) bir talep türünün değerini denetlemek için temel bir doğrulama tanımlar ve doğru veya yanlış döndürür. Doğrulama, belirtilen bir yöntem öğesi ve yöntemle ilgili bir dizi parametre kullanılarak yapılır. Öğenin kapatılmasından hemen sonra BuildingBlocks öğesine aşağıdaki yüklemleri ekleyin: **BuildingBlocks** `</ClaimsSchema>`

    ```XML
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

1. `</Predicates>` Öğenin kapatılmasından hemen sonra **BuildingBlocks** öğesine aşağıdaki yüklem doğrulamalarını ekleyin:

    ```XML
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

1. Aşağıdaki teknik profiller, Azure Active Directory'ye veri okuyup yazan [Active Directory teknik profilleridir.](active-directory-technical-profile.md) Uzantı dosyasındaki bu teknik profilleri geçersiz kılın. `PersistedClaims` Güçlü parola ilkesini devre dışı kullanabilirsiniz. İddia **Sağlayıcıları** öğesini bulun.  Aşağıdaki talep sağlayıcılarını aşağıdaki gibi ekleyin:

    ```XML
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

## <a name="test-your-policy"></a>İlkinizi test edin

### <a name="upload-the-files"></a>Dosyaları karşıya yükleme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Kimlik Deneyimi Çerçevesi'ni**seçin.
5. Özel İlkeler sayfasında, **Yükle İlkesi'ni**tıklatın.
6. **Varsa politikanın Üzerine Yaz'ı**seçin ve ardından *TrustFrameworkExtensions.xml* dosyasını arayın ve seçin.
7. **Karşıya Yükle**'ye tıklayın.

### <a name="run-the-policy"></a>İlkeyi çalıştırın

1. Kaydolma veya kaydolma ilkesini açın. Örneğin, *B2C_1A_signup_signin.*
2. **Uygulama**için, daha önce kaydolduğunuz başvurunuzu seçin. Belirteci görmek için **Yanıt URL'sinin** göstermesi `https://jwt.ms`gerekir.
3. **Şimdi çalıştır**’a tıklayın.
4. **Şimdi Kaydol'u**seçin, bir e-posta adresi girin ve yeni bir parola girin. Kılavuz, parola kısıtlamaları nda sunulur. Kullanıcı bilgilerini girmeyi bitirin ve ardından **Oluştur'u**tıklatın. Döndürülen belirteç içeriğini görmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory B2C'de özel ilkeleri kullanarak parola değişikliğini](custom-policy-password-change.md)nasıl yapılandırıştırmayı öğrenin.
- IEF referansındaki [Yüklemler](predicates.md) ve [Yüklemler](predicates.md#predicatevalidations) öğeleri hakkında daha fazla bilgi edinin.
