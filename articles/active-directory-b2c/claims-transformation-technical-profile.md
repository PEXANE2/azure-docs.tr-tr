---
title: Talepler dönüşüm teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede Talepler dönüştürme teknik profilini tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/13/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 84c1cf798e88e4067da8a495c1591143d2ee1bd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189795"
---
# <a name="define-a-claims-transformation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde talep dönüştürme teknik profilini tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Talep dönüştürme teknik profili, talep değerlerini işlemek, talepleri doğrulamak veya çıktı talepleri kümesi için varsayılan değerleri ayarlamak için çıktı talepleri dönüşümlerini aramanızı sağlar.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir: `Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

Aşağıdaki örnek, talep dönüşümü teknik profilini gösterir:

```XML
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="output-claims"></a>Çıktı talepleri

**OutputClaims** öğesi zorunludur. Teknik profil tarafından döndürülen en az bir çıktı talebi sağlamanız gerekir. Aşağıdaki örnek, çıktı taleplerinde varsayılan değerlerin nasıl ayarlanılsüreceğini gösterir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="ageGroup" DefaultValue="Undefined" />
  <OutputClaim ClaimTypeReferenceId="ageGroupValueChanged" DefaultValue="false" />
</OutputClaims>
```

## <a name="output-claims-transformations"></a>Çıktı talepleri dönüşümleri

**OutputClaimsTransformations** öğesi, talepleri değiştirmek veya yenilerini oluşturmak için kullanılan **OutputClaimsTransformation** öğelerikoleksiyonunu içerebilir. Aşağıdaki teknik profil **RemoveAlternativeSecurityIdByIdentityProvider** dönüşüm talepleri çağırır. Bu iddia dönüşüm **AlternativeSecurityIds**koleksiyonundan bir sosyal kimlik kaldırır. Bu teknik profilin çıktı iddiaları **kimlik Sağlayıcı2** `facebook.com`, hangi ayarlanır , ve **AlternativeSecurityIds**, facebook.com kimlik kaldırıldıktan sonra bu kullanıcı ile ilişkili sosyal kimliklerin listesini içeren.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="RemoveAlternativeSecurityIdByIdentityProvider"
TransformationMethod="RemoveAlternativeSecurityIdByIdentityProvider">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="IdentityProvider2"
TransformationClaimType="identityProvider" />
      <InputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="AlternativeSecurityIds"
TransformationClaimType="collection" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
...
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider2" DefaultValue="facebook.com" AlwaysUseDefaultValue="true" />
    </OutputClaims>
    <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="RemoveAlternativeSecurityIdByIdentityProvider" />
    </OutputClaimsTransformations>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Talep dönüştürme teknik profili, herhangi bir kullanıcı yolculuğunun düzenleme adımından bir talep dönüşümü gerçekleştirmenize olanak tanır. Aşağıdaki örnekte, orkestrasyon adımı **UnLink-Facebook-OAUTH**gibi bağlantısız teknik profillerden birini çağırır. Bu teknik profil, facebook kimliğini koleksiyonlardan kaldırırken, kullanıcı sosyal kimliklerinin listesini içeren yeni bir **AlternativeSecurityIds2** iddiası oluşturan, talep dönüşümü teknik profilini **RemoveAlternativeSecurityIdByIdentityProvider**olarak adlandırır.

```XML
<UserJourney Id="AccountUnLink">
  <OrchestrationSteps>
    ...
    <OrchestrationStep Order="8" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="UnLinkFacebookExchange" TechnicalProfileReferenceId="UnLink-Facebook-OAUTH" />
        <ClaimsExchange Id="UnLinkMicrosoftExchange" TechnicalProfileReferenceId="UnLink-Microsoft-OAUTH" />
        <ClaimsExchange Id="UnLinkGitHubExchange" TechnicalProfileReferenceId="UnLink-GitHub-OAUTH" />
      </ClaimsExchanges>
    </OrchestrationStep>
    ...
  </OrchestrationSteps>
</UserJourney>
```

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| IncludeClaimResolvingInClaimsHandling  | Hayır | Giriş ve çıktı talepleri [için, talep çözümlübir](claim-resolver-overview.md) çözümünü teknik profilde bulunup bulunmayacağını belirtir. Olası değerler: `true` `false`  , veya (varsayılan). Teknik profilde bir talep çözümleyicisi kullanmak istiyorsanız, bunu ' ya `true`göre ayarlayın. |

## <a name="use-a-validation-technical-profile"></a>Doğrulama teknik profili kullanma

Bilgileri doğrulamak için talep dönüştürme teknik profili kullanılabilir. Aşağıdaki örnekte, **LocalAccountSignWithLogonEmail** adlı [kendi kendini öne süren teknik profil,](self-asserted-technical-profile.md) kullanıcıdan e-postayı iki kez girmesini ister ve e-postaları doğrulamak için **Doğrulama-E-posta** adlı [doğrulama teknik profilini](validation-technical-profile.md) arar. **Doğrula-E-posta** teknik profili iki iddia **e-posta** ve **e-posta yineleme**karşılaştırmak için iddia dönüşümü **AssertEmailAreEqual** çağırır , ve belirtilen karşılaştırma göre eşit değilse bir özel durum atmak.

```XML
<ClaimsTransformations>
  <ClaimsTransformation Id="AssertEmailAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
      <InputClaim ClaimTypeReferenceId="emailRepeat" TransformationClaimType="inputClaim2" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    </InputParameters>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Talep dönüşümü teknik profili, kullanıcı tarafından sağlanan e-postaların aynı olduğunu iddia eden **AssertEmailAreEqual** talepleri dönüşümlerini çağırır.

```XML
<TechnicalProfile Id="Validate-Email">
  <DisplayName>Unlink Facebook</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="emailRepeat" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
  </OutputClaims>
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAreEqual" />
  </OutputClaimsTransformations>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

Kendi kendini öne süren bir teknik profil doğrulama teknik profilini arayabilir ve **UserMessageIfClaimsTransformationStringsAreNotEqual** meta verisinde belirtildiği gibi hata iletisini gösterebilir.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>User ID signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    ...
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">The email addresses you provided are not the same</Item>
  </Metadata>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="emailRepeat" />
    ...
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="Validate-Email" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```
