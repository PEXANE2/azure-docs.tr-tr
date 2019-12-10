---
title: Talep dönüştürmesi teknik profili tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C bir özel ilkede talep dönüştürme teknik profili tanımlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bf06fe7d4e529eb04b156a2d61011198a6fe0978
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74949432"
---
# <a name="define-a-claims-transformation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde talep dönüştürme teknik profili tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Talep dönüştürme teknik profili, talep değerlerini işlemek, talepleri doğrulamak veya bir çıkış talepleri kümesi için varsayılan değerleri ayarlamak üzere çıkış talep dönüştürmelerini çağırmanızı sağlar.

## <a name="protocol"></a>Protokol

**Protokol** öğesinin **Name** özniteliğinin `Proprietary`olarak ayarlanması gerekir. **Handler** özniteliği Azure AD B2C: `Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`tarafından kullanılan protokol işleyici derlemesinin tam adını içermelidir.

Aşağıdaki örnekte bir talep dönüştürme teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="Facebook-OAUTH-UnLink">
    <DisplayName>Unlink Facebook</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.ClaimsTransformationProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="output-claims"></a>Çıkış talepleri

**Outputclaim** öğesi zorunludur. Teknik profil tarafından döndürülen en az bir çıkış talebi sağlamanız gerekir. Aşağıdaki örnek, çıkış talepleri için varsayılan değerlerin nasıl ayarlanacağını gösterir:

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="ageGroup" DefaultValue="Undefined" />
  <OutputClaim ClaimTypeReferenceId="ageGroupValueChanged" DefaultValue="false" />
</OutputClaims>
```

## <a name="output-claims-transformations"></a>Çıkış talepleri dönüşümleri

**Outputclaimstransformations** öğesi, talepleri değiştirmek veya yenilerini oluşturmak için kullanılan bir **outputclaimstransreference** öğeleri koleksiyonu içerebilir. Aşağıdaki teknik profil **Removealternativesecurityıdbyıdentityprovider** talep dönüşümünü çağırır. Bu talep dönüştürmesi, **değişim kimliği**koleksiyonundan bir sosyal kimlik belirlemeyi kaldırır. Bu teknik profilin çıkış talepleri, facebook.com kimliği kaldırıldıktan sonra bu kullanıcıyla ilişkili sosyal kimliklerin listesini içeren `facebook.com`ve **Alternativesecurityıds**' **identityProvider2**' a ayarlanmıştır.

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

Talep dönüştürme teknik profili, herhangi bir Kullanıcı yolculuğunun düzenleme adımından bir talep dönüştürmesi çalıştırmanızı sağlar. Aşağıdaki örnekte, düzenleme adımı, Kaldır **-Facebook-OAUTH**gibi teknik profillerin bağlantısını Kaldır ' ı çağırır. Bu teknik profil, Kullanıcı sosyal kimlik listesini içeren yeni bir **AlternativeSecurityIds2** talebi oluşturan,, Facebook kimliği koleksiyonlarından kaldırılırken, talep dönüştürme teknik profili **Removealternativesecurityıdbyıdentityprovider**' ı çağırır.

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

## <a name="use-a-validation-technical-profile"></a>Doğrulama teknik profili kullanma

Bilgileri doğrulamak için bir talep dönüştürme teknik profili kullanılabilir. Aşağıdaki örnekte, **Localaccountsignupwithlogonemail** adlı [kendi kendini onaylanan teknik profil](self-asserted-technical-profile.md) kullanıcıdan e-postayı iki kez girmesini ister, ardından e-posta adını doğrulamak için **Validate-email** adlı [doğrulama teknik profilini](validation-technical-profile.md) çağırır. **Validate-e-posta** teknik profili, iki talep **e-postasını** ve **emailrepeat**'ı karşılaştırmak Için talepler dönüşümünü **assertemaılareeþiti** çağırır ve belirtilen karşılaştırmaya göre eşit değilse bir özel durum oluşturur.

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

Talep dönüştürme teknik profili, Kullanıcı tarafından sunulan e-postaların aynı olduğunu belirleyen **Assertemailareeþitclaim** dönüşümünü çağırır.

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

Otomatik olarak onaylanan bir teknik profil, doğrulama teknik profilini çağırabilir ve **Usermessageifclaimstransformationstringsarenotequal** meta verilerinde belirtilen hata iletisini gösterebilir.

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
