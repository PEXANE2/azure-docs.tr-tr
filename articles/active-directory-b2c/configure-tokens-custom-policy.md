---
title: Özel ilkeleri kullanarak SSO ve belirteç özelleştirmesini yönetme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C özel ilkeleri kullanarak SSO ve belirteç özelleştirmeyi yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/07/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9f3cd5c3280308f6da15a52361857fa02567d595
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88505470"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel ilkeleri kullanarak SSO ve belirteç özelleştirmesini yönetme

Bu makalede belirteç, oturum ve çoklu oturum açma (SSO) yapılandırmalardan Azure Active Directory B2C (Azure AD B2C) [özel ilkelerini](custom-policy-overview.md) kullanarak nasıl yönetebileceğiniz hakkında bilgi sağlanır.

## <a name="jwt-token-lifetimes-and-claims-configuration"></a>JWT belirteci yaşam süreleri ve talep yapılandırması

Belirteç yaşam sürelerinin ayarlarını değiştirmek için, etkilemek istediğiniz ilkenin bağlı olan taraf dosyasına bir [Claimsproviders](claimsproviders.md) öğesi eklersiniz.  **Claimsproviders** öğesi, [TrustFrameworkPolicy](trustframeworkpolicy.md) öğesinin bir alt öğesidir.

BasePolicy öğesi ve bağlı olan taraf dosyasının RelyingParty öğesi arasına ClaimsProviders öğesini ekleyin.

İçinde, belirteç ömürlerinizi etkileyen bilgileri yerleştirmeniz gerekir. XML şu örneğe benzer şekilde görünür:

```xml
<ClaimsProviders>
  <ClaimsProvider>
    <DisplayName>Token Issuer</DisplayName>
    <TechnicalProfiles>
      <TechnicalProfile Id="JwtIssuer">
        <Metadata>
          <Item Key="token_lifetime_secs">3600</Item>
          <Item Key="id_token_lifetime_secs">3600</Item>
          <Item Key="refresh_token_lifetime_secs">1209600</Item>
          <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
          <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
          <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
        </Metadata>
      </TechnicalProfile>
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProviders>
```

Önceki örnekte aşağıdaki değerler ayarlanır:

- **Erişim belirteci yaşam süreleri** -erişim belirteci yaşam süresi değeri **token_lifetime_secs** meta veri öğesiyle ayarlanır. Varsayılan değer 3600 saniyedir (60 dakika).
- **Kimlik belirteci ömrü** -kimlik belirtecinin yaşam süresi değeri **id_token_lifetime_secs** meta veri öğesiyle ayarlanır. Varsayılan değer 3600 saniyedir (60 dakika).
- **Belirteç ömrünü Yenile** -yenileme belirteci yaşam süresi değeri **refresh_token_lifetime_secs** meta veri öğesiyle ayarlanır. Varsayılan değer 1209600 saniyedir (14 gün).
- **Yenileme belirteci kayan pencere ömrü** -yenileme belirtecinize bir kayan pencere ömrü ayarlamak isterseniz, **rolling_refresh_token_lifetime_secs** meta veri öğesi değerini ayarlayın. Varsayılan değer 7776000 ' dir (90 gün). Bir kayan pencere ömrü zorlamak istemiyorsanız, öğesini ile değiştirin `<Item Key="allow_infinite_rolling_refresh_token">True</Item>` .
- **Veren (ISS) talebi** -veren (İSS) talebi **ıssuanceclaımpattern** meta veri öğesiyle ayarlanır. Geçerli değerler `AuthorityAndTenantGuid` ve ' dir `AuthorityWithTfp` .
- **Ilke kimliğini temsil eden talep ayarlama** -bu değeri ayarlama seçenekleri `TFP` (güven Framework ilkesi) ve `ACR` (kimlik doğrulama bağlamı başvurusu). `TFP` önerilen değerdir. **Authenticationcontextreferenceclaımpattern** değerini olarak ayarlayın `None` .

    **Claimsschema** öğesinde şu öğeyi ekleyin:

    ```xml
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    **Outputclaim** öğesinde şu öğeyi ekleyin:

    ```xml
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    ACR için **Authenticationcontextreferenceclaımpattern** öğesini kaldırın.

- **Subject (Sub) talebi** -Bu seçenek, bu ayarı değiştirmek istiyorsanız varsayılan olarak ObjectID olarak `Not Supported` değiştirilir, şu satırı değiştirin:

    ```xml
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    Bu satırla:

    ```xml
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C oturum](session-overview.md)hakkında daha fazla bilgi edinin.
- [Özel ilkelerde oturum davranışını yapılandırmayı](session-behavior-custom-policy.md)öğrenin.
- Başvuru: [Jwtissuer](jwt-issuer-technical-profile.md).
