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
ms.date: 02/03/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 65f5b9bc09bb74ccbc63a457ba8e5681de8eefab
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78189950"
---
# <a name="boolean-claims-transformations"></a>Boole talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının Boolean talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="andclaims"></a>Veilgili

İki Boolean ınputclaim için bir ve işlemi gerçekleştirir ve outputClaim 'i işlemin sonucu olarak ayarlar.

| Öğe  | Dönüştürme Tionclaimtype  | Veri Türü  | Notlar |
|-------| ------------------------ | ---------- | ----- |
| ınputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| ınputclaim | inputClaim2  | boole | Değerlendirilecek ikinci ClaimType. |
|OutputClaim | OutputClaim | boole | Bu talep dönüştürmesinin ardından üretilecek olan ClaimTypes (true veya false). |

Aşağıdaki talep dönüşümünde, ve iki Boolean ClaimTypes gösterilmektedir: `isEmailNotExist`ve `isSocialAccount`. Giriş taleplerinin her ikisi de `true`ise, çıkış talebi `presentEmailSelfAsserted` `true` olarak ayarlanır. Bir düzenleme adımında, yalnızca sosyal hesap e-postası boş ise, otomatik olarak onaylanan bir sayfayı önceden ayarlamak için bir ön koşul kullanabilirsiniz.

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

### <a name="example"></a>Örnek

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

**AssertBooleanClaimIsEqualToValue** talep dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profille](self-asserted-technical-profile.md)çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** kendiliğinden onaylanan teknik profil meta verileri, teknik profilin kullanıcıya sunduğu hata iletisini denetler.

![Assertstringclaimsareeşittir yürütme](./media/boolean-transformations/assert-execution.png)

Aşağıdaki talep dönüşümünde, bir `true` değeri ile bir Boole ClaimType değerinin nasıl denetlenecek gösterilmektedir. `accountEnabled` ClaimType değeri false ise, bir hata iletisi oluşturulur.

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


`login-NonInteractive` doğrulaması teknik profili `AssertAccountEnabledIsTrue` talep dönüşümünü çağırır.
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

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **ınputclaim**: false
    - **Valuetocompareto**: true
- Sonuç: hata oluştu

## <a name="comparebooleanclaimtovalue"></a>CompareBooleanClaimToValue

Taleplerin Boolean değerinin `true` veya `false`eşit olduğunu denetler ve sıkıştırmanın sonucunu döndürür.

| Öğe | Dönüştürme Tionclaimtype  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| ınputclaim | ınputclaim | boole | Belirtilme ClaimType. |
| InputParameter |valueToCompareTo | boole | Karşılaştırılacak değer (true veya false). |
| OutputClaim | compareResult | boole | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |


Aşağıdaki talep dönüşümünde, bir `true` değeri ile bir Boole ClaimType değerinin nasıl denetlenecek gösterilmektedir. `IsAgeOver21Years` ClaimType değeri `true`eşitse, talep dönüştürmesi `true`döndürür, aksi takdirde `false`.

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

### <a name="example"></a>Örnek

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
| ınputclaim | ınputclaim | boole | İşletilen talep. |
| OutputClaim | OutputClaim | boole | Bu Claimstranssetting sonrasında üretilen ClaimTypes (true veya false) çağırılır. |

Bir talep üzerinde mantıksal olumsuzlama gerçekleştirmek için bu talep dönüşümünü kullanın.

```XML
<ClaimsTransformation Id="CheckWhetherEmailBePresented" TransformationMethod="NotClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="inputClaim" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userExists" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **ınputclaim**: false
- Çıkış talepleri:
    - **Outputclaim**: true

## <a name="orclaims"></a>Ortalepler

İki Boole ınputclaim veya birini hesaplar ve outputClaim 'yi işlem sonucuyla ayarlar.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| ınputclaim | inputClaim2 | boole | Değerlendirilecek ikinci ClaimType. |
| OutputClaim | OutputClaim | boole | Bu Claimstranssize çağrıldıktan sonra üretilecek olan ClaimTypes (true veya false). |

Aşağıdaki talep dönüşümünde iki Boolean ClaimTypes `Or` gösterilmektedir. Düzenleme adımında, taleplerden birinin değeri `true`, otomatik olarak onaylanan bir sayfayı önceden ayarlamak için bir ön koşul kullanabilirsiniz.

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
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: false
- Çıkış talepleri:
    - **Outputclaim**: true
