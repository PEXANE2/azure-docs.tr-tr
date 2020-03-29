---
title: Özel ilkeleri kullanarak SSO ve belirteç özelleştirmesini yönetme
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeleri kullanarak SSO ve belirteç özelleştirmesini yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: ff9f57af92c50c0df6f628113bd9490ca83e1310
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189302"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de özel ilkeleri kullanarak SSO ve belirteç özelleştirmesini yönetme

Bu makalede, Azure Etkin Dizin B2C'de (Azure AD B2C) [özel ilkeleri](custom-policy-overview.md) kullanarak belirteç, oturum ve tek oturum (SSO) yapılandırmalarınızı nasıl yönetebileceğiniz hakkında bilgi verilmektedir.

## <a name="token-lifetimes-and-claims-configuration"></a>Belirteç ömürleri ve talep yapılandırması

Belirteç yaşam sürenizdeki ayarları değiştirmek için, etkilemek istediğiniz politikanın güvenilen taraf dosyasına bir [ClaimsProviders](claimsproviders.md) öğesi eklersiniz.  **ClaimsProviders** öğesi [TrustFrameworkPolicy](trustframeworkpolicy.md) öğesinin bir alt öğesidir.

BasePolicy öğesi ile güvenilen taraf dosyasının RelyingParty öğesi arasına İddia Sağlayıcıları öğesini ekleyin.

İçinde, belirteç ömürünüzü etkileyen bilgileri koymanız gerekir. XML aşağıdaki örneğe benzer:

```XML
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

Aşağıdaki değerler önceki örnekte ayarlanır:

- **Erişim belirteç ömürleri** - Erişim belirteç yaşam süresi değeri **token_lifetime_secs** meta veri öğesi ile ayarlanır. Varsayılan değer 3600 saniyedir (60 dakika).
- **Kimlik belirteç ömrü** - Kimlik belirteç ömür boyu değeri **id_token_lifetime_secs** meta veri öğesi ile ayarlanır. Varsayılan değer 3600 saniyedir (60 dakika).
- **Belirteç ömrünü yenileyin** - Belirteç kullanım ömrü değerini refresh_token_lifetime_secs **meta** veri öğesiyle birlikte yenileyin. Varsayılan değer 1209600 saniyedir (14 gün).
- **Belirteç kaydırma penceresi nin kullanım ömrünü yenileyin** - Yenileme belirtecinize kayan bir pencere ömrü ayarlamak istiyorsanız, **rolling_refresh_token_lifetime_secs** meta veri öğesinin değerini ayarlayın. Varsayılan değer 7776000 (90 gün) idi. Kayan bir pencere ömrünü zorlamak istemiyorsanız, öğeyi `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`' yi .
- **Veren (iss) iddiası** - Veren (iss) talebi **IssuanceClaimPattern** meta veri öğesi ile ayarlanır. Geçerli değerler `AuthorityAndTenantGuid` ve `AuthorityWithTfp`.
- **İlke Kimliğini temsil eden talep ayarlama** - `TFP` Bu değeri ayarlama `ACR` seçenekleri (güven çerçevesi ilkesi) ve (kimlik doğrulama bağlamı başvurusu). `TFP`önerilen değerdir. **Kimlik DoğrulamaBağlamReferansClaimPattern** değerini `None`ayarlayın.

    **ClaimsSchema** öğesine şu öğeyi ekleyin:

    ```XML
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```

    Çıktı **Talepleri** öğenize şu öğeyi ekleyin:

    ```XML
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    ACR için **AuthenticationContextReferenceClaimPattern** öğesini kaldırın.

- **Konu (alt) talep** - Bu ayarı `Not Supported`bu satırı değiştirmek için değiştirmek istiyorsanız, bu seçenek ObjectID varsayılan:

    ```XML
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```

    bu satırile:

    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="session-behavior-and-sso"></a>Oturum davranışı ve SSO

Oturum davranışınızı ve SSO yapılandırmalarınızı değiştirmek [için, RelyingParty](relyingparty.md) öğesinin içine bir **UserJourneyBehaviors** öğesi eklersiniz.  **UserJourneyBehaviors** öğesi hemen **DefaultUserJourney**takip etmelidir. **UserJourneyBehavors** öğenizin içi aşağıdaki örnek gibi görünmelidir:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

Aşağıdaki değerler önceki örnekte yapılandırılır:

- **Tek oturum açma (SSO)** - Tek oturum açma **SingleSignOn**ile yapılandırılır. Geçerli değerler `Tenant`, `Application` `Policy`, `Suppressed`, ve .
- **Web uygulaması oturumu zaman aşımı** - Web uygulaması oturum zaman aşımı **SessionExpiryType** öğesi ile ayarlanır. Geçerli değerler `Absolute` ve `Rolling`.
- **Web uygulaması oturumu ömür boyu** - Web uygulaması oturum ömrü **SessionExpiryInSeconds** öğesi ile ayarlanır. Varsayılan değer 86400 saniyedir (1440 dakika).
