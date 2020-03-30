---
title: Özel bir ilkede telefon faktörü teknik profilini tanımlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkede bir telefon faktörü teknik profili tanımlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3a0511a19477f3d76baf9c453316c5348cc31397
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332666"
---
# <a name="define-a-phone-factor-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C özel ilkesinde telefon faktörü teknik profilini belirleme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C), telefon numaralarını kaydetme ve doğrulama desteği sağlar. Bu teknik profil:

- Kullanıcı ile etkileşim kurmak için bir kullanıcı arabirimi sağlar.
- Görünümü ve hissini kontrol etmek için içerik tanımını kullanır.
- Telefon numarasını doğrulamak için hem telefon görüşmelerini hem de kısa mesajları destekler.
- Birden çok telefon numarası destekler. Kullanıcı doğrulamak için telefon numaralarından birini seçebilir.  
- Bir telefon numarası sağlanırsa, telefon faktörü kullanıcı arabirimi kullanıcıdan telefon numarasını doğrulamasını ister. Sağlanmazsa, kullanıcıdan yeni bir telefon numarası kaydetmesini ister.
- Kullanıcının yeni bir telefon numarası sağlayıp sağlamadığını belirten bir talep verir. Bu talebi, telefon numarasının Azure AD kullanıcı profilinde kalıcı olup olmadığına karar vermek için kullanabilirsiniz.  

## <a name="protocol"></a>Protokol

**Protokol** öğesinin `Proprietary` **Ad** özniteliğinin . **Işleyici** özniteliği, telefon faktörü için Azure AD B2C tarafından kullanılan protokol işleyicisi derlemesinin tam nitelikli adını içermelidir:`Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnekte, kayıt ve doğrulama için bir telefon faktörü teknik profili gösterilmektedir:

```XML
<TechnicalProfile Id="PhoneFactor-InputOrVerify">
  <DisplayName>PhoneFactor</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.PhoneFactorProtocolProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="input-claims"></a>Giriş talepleri

InputClaims öğesi aşağıdaki iddiaları içermelidir. Ayrıca, talep ettiğiniz adı telefon faktörü teknik profilinde tanımlanan ada eşleyebilirsiniz. 

```XML
<InputClaims>
  <!--A unique identifier of the user. The partner claim type must be set to `UserId`. -->
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <!--A claim that contains the phone number. If the claim is empty, Azure AD B2C asks the user to enroll a new phone number. Otherwise, it asks the user to verify the phone number. -->
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
</InputClaims>
```

Aşağıdaki örnek, birden çok telefon numarası nın kullanılmasını gösterir. Daha fazla bilgi için [örnek ilke](https://github.com/azure-ad-b2c/samples/tree/master/policies/mfa-add-secondarymfa)ye bakın.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
  <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
  <InputClaim ClaimTypeReferenceId="secondaryStrongAuthenticationPhoneNumber" />
</InputClaims>
```

InputClaimsTransformations öğesi, giriş taleplerini değiştirmek veya telefon faktörü sayfasına sunmadan önce yenilerini oluşturmak için kullanılan InputClaimsTransformation öğelerikoleksiyonunu içerebilir.

## <a name="output-claims"></a>Çıktı talepleri

OutputClaims öğesi, telefon faktörü teknik profili tarafından döndürülen taleplerin listesini içerir.

```xml
<OutputClaims>
  <!-- The verified phone number. The partner claim type must be set to `Verified.OfficePhone`. -->
  <OutputClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber" PartnerClaimType="Verified.OfficePhone" />
  <!-- Indicates whether the new phone number has been entered by the user. The partner claim type must be set to `newPhoneNumberEntered`. -->
  <OutputClaim ClaimTypeReferenceId="newPhoneNumberEntered" PartnerClaimType="newPhoneNumberEntered" />
</OutputClaims>
```

OutputClaimsTransformations öğesi, çıktı taleplerini değiştirmek veya yenilerini oluşturmak için kullanılan OutputClaimsTransformation öğelerikoleksiyonunu içerebilir.

## <a name="cryptographic-keys"></a>Şifreleme tuşları

**CryptographicKeys** öğesi kullanılmaz.


## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| İçerikDefinitionReferenceId | Evet | Bu teknik profille ilişkili [içerik tanımının](contentdefinitions.md) tanımlayıcısı. |
| ManualPhoneNumberEntryAllowed| Hayır | Bir kullanıcının bir telefon numarasını el ile girmesine izin verilip verilmediğini belirtin. Olası `true` değerler: `false` veya (varsayılan).|

### <a name="ui-elements"></a>Kullanıcı arabirimi öğeleri

Telefon faktörü kimlik doğrulama sayfası kullanıcı arabirimi öğeleri [yerelleştirilmiş](localization-string-ids.md#azure-mfa-error-messages)olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [MFA](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/SocialAndLocalAccountsWithMfa) başlangıç paketi ile sosyal ve yerel hesapları kontrol edin.

