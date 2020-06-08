---
title: Özel ilkeler için Boole talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için Boole talep dönüştürme örnekleri.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/06/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 02d488108474084346d9e37d5cc6ecbe3a8a05c6
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84484287"
---
# <a name="boolean-claims-transformations"></a>Boole talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının Boolean talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="andclaims"></a>Veilgili

İki Boolean ınputclaim için bir ve işlemi gerçekleştirir ve outputClaim 'i işlemin sonucu olarak ayarlar.

| Öğe  | Dönüştürme Tionclaimtype  | Veri Türü  | Notlar |
|-------| ------------------------ | ---------- | ----- |
| Inputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Inputclaim | inputClaim2  | boole | Değerlendirilecek ikinci ClaimType. |
|OutputClaim | outputClaim | boole | Bu talep dönüştürmesinin ardından üretilecek olan ClaimTypes (true veya false). |

Aşağıdaki talep dönüşümünde, ve iki Boolean ClaimTypes gösterilmektedir: `isEmailNotExist` , ve `isSocialAccount` . `presentEmailSelfAsserted` `true` Giriş taleplerinin her ikisi de değeri ise, çıkış talebi olarak ayarlanır `true` . Bir düzenleme adımında, yalnızca sosyal hesap e-postası boş ise, otomatik olarak onaylanan bir sayfayı önceden ayarlamak için bir ön koşul kullanabilirsiniz.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="AndClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isEmailNotExist" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isSocialAccount" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentEmailSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example-of-andclaims"></a>Andclaim örneği

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Çıkış talepleri:
    - **Outputclaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

İki talebin Boole değerlerinin eşit olduğunu denetler ve yoksa bir özel durum oluşturur.

| Öğe | Dönüştürme Tionclaimtype  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| ınputclaim | ınputclaim | boole | Belirtilme ClaimType. |
| InputParameter |valueToCompareTo | boole | Karşılaştırılacak değer (true veya false). |

**AssertBooleanClaimIsEqualToValue** talep dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profille](self-asserted-technical-profile.md)çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** kendiliğinden onaylanan teknik profil meta verileri, teknik profilin kullanıcıya sunduğu hata iletisini denetler. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#claims-transformations-error-messages).

![Assertstringclaimsareeşittir yürütme](./media/boolean-transformations/assert-execution.png)

Aşağıdaki talep dönüşümünde, bir Boolean ClaimType değerinin bir değerle nasıl kontrol yapılacağı gösterilmektedir `true` . `accountEnabled`ClaimType değeri false ise, bir hata iletisi oluşturulur.

```XML
<ClaimsTransformation Id="AssertAccountEnabledIsTrue" TransformationMethod="AssertBooleanClaimIsEqualToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="accountEnabled" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
</ClaimsTransformation>
```


`login-NonInteractive`Doğrulama teknik profili `AssertAccountEnabledIsTrue` talep dönüşümünü çağırır.

```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Otomatik olarak onaylanan teknik profil, doğrulama **oturum açma-etkileşimsiz** teknik profilini çağırır.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationBooleanValueIsNotEqual">Custom error message if account is disabled.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example-of-assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue örneği

- Giriş talepleri:
    - **ınputclaim**: false
    - **Valuetocompareto**: true
- Sonuç: hata oluştu

## <a name="comparebooleanclaimtovalue"></a>CompareBooleanClaimToValue

Bir talebin Boole değerinin veya değerine eşit olduğunu denetler `true` `false` ve sıkıştırmanın sonucunu döndürür.

| Öğe | Dönüştürme Tionclaimtype  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| Inputclaim | ınputclaim | boole | Belirtilme ClaimType. |
| InputParameter |valueToCompareTo | boole | Karşılaştırılacak değer (true veya false). |
| OutputClaim | compareResult | boole | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Aşağıdaki talep dönüşümünde, bir Boolean ClaimType değerinin bir değerle nasıl kontrol yapılacağı gösterilmektedir `true` . `IsAgeOver21Years`ClaimType değeri öğesine eşitse `true` , talep dönüştürmesi, `true` Aksi takdirde döndürür `false` .

```XML
<ClaimsTransformation Id="AssertAccountEnabled" TransformationMethod="CompareBooleanClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="IsAgeOver21Years" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="valueToCompareTo" DataType="boolean" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim  ClaimTypeReferenceId="accountEnabled" TransformationClaimType="compareResult"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example-of-comparebooleanclaimtovalue"></a>CompareBooleanClaimToValue örneği

- Giriş talepleri:
    - **ınputclaim**: false
- Giriş parametreleri:
    - **Valuetocompareto**: true
- Çıkış talepleri:
    - **compareResult**: false

## <a name="notclaims"></a>Notclaim

Boolean ınputclaim ' in olmayan bir işlemini gerçekleştirir ve outputClaim 'i işlemin sonucu olarak ayarlar.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | ınputclaim | boole | İşletilen talep. |
| OutputClaim | outputClaim | boole | Bu Claimstranssetting sonrasında üretilen ClaimTypes (true veya false) çağırılır. |

Bir talep üzerinde mantıksal olumsuzlama gerçekleştirmek için bu talep dönüşümünü kullanın.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example-of-notclaims"></a>Notclaim örnekleri

- Giriş talepleri:
    - **ınputclaim**: false
- Çıkış talepleri:
    - **Outputclaim**: true

## <a name="orclaims"></a>Ortalepler

İki Boole ınputclaim veya birini hesaplar ve outputClaim 'yi işlem sonucuyla ayarlar.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Inputclaim | inputClaim2 | boole | Değerlendirilecek ikinci ClaimType. |
| OutputClaim | outputClaim | boole | Bu Claimstranssize çağrıldıktan sonra üretilecek olan ClaimTypes (true veya false). |

Aşağıdaki talep dönüşümünde `Or` iki Boolean ClaimTypes gösterilmektedir. Düzenleme adımında, taleplerden birinin değeri ise, otomatik olarak onaylanan bir sayfayı önceden ayarlamak için bir ön koşul kullanabilirsiniz `true` .

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="OrClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedNotExists" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="presentTOSSelfAsserted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example-of-orclaims"></a>Orclaim örneği

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Çıkış talepleri:
    - **Outputclaim**: true
