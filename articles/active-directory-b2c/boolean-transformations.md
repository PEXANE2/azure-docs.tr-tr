---
title: Boolean özel ilkeler için dönüşüm örnekleri iddia ediyor
titleSuffix: Azure AD B2C
description: Boolean, Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için dönüşüm örnekleri talep ediyor.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/01/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 007d613a1f170a0ee278a838c92ade2fce9c6dec
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529195"
---
# <a name="boolean-claims-transformations"></a>Boolean dönüşümleri iddia ediyor

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının boolean talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="andclaims"></a>AndClaims

İki boolean girişinin çalışmasını gerçekleştirir ve işlem sonucunda outputClaim'i ayarlar.

| Öğe  | DönüşümTalep Türü  | Veri Türü  | Notlar |
|-------| ------------------------ | ---------- | ----- |
| Girişİddia | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Girişİddia | inputClaim2  | boole | İkinci ClaimType değerlendirmek için. |
|ÇıktılarTalep | outputClaim | boole | Bu talep dönüşümünden sonra üretilecek Olan Talep Türleri çağrıldı (doğru veya yanlış). |

Aşağıdaki talep dönüşümü nasıl Ve iki boolean `isEmailNotExist`ClaimTypes `isSocialAccount`gösterir: ve . Çıktı talebi, `presentEmailSelfAsserted` `true` her iki giriş talebinin değeri `true`. Bir düzenleme adımında, yalnızca bir sosyal hesap e-postası boşsa, kendi kendini öne süren bir sayfayı önceden ayarlamak için bir ön koşul kullanabilirsiniz.

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
    - **inputClaim2**: yanlış
- Çıktı talepleri:
    - **outputClaim**: false


## <a name="assertbooleanclaimisequaltovalue"></a>AssertBooleanClaimIsEqualToValue

İki talebin boolean değerlerinin eşit olduğunu denetler ve değilse bir özel durum atar.

| Öğe | DönüşümTalep Türü  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| inputClaim | inputClaim | boole | İddia Edilecek Talep Türü. |
| ınputparameter |valueToCompareTo | boole | Karşılaştırılacak değer (doğru veya yanlış). |

**AssertBooleanClaimIsEqualToValue** talepleri dönüşümü her zaman kendini ileri süren teknik [profil](self-asserted-technical-profile.md)tarafından çağrılan bir [doğrulama teknik profilden](validation-technical-profile.md) yürütülür. **UserMessageIfClaimsTransformationBooleanValueIsNotEqual** self-asserted teknik profil meta veri teknik profil kullanıcıya sunduğu hata iletisini denetler. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#claims-transformations-error-messages)

![AssertStringClaimsAreEşit yürütme](./media/boolean-transformations/assert-execution.png)

Aşağıdaki talep dönüştürmesi, `true` değeri olan bir boolean ClaimType'ın değerini nasıl denetlergösteriz gösterir. ClaimType değeri `accountEnabled` yanlışsa, bir hata iletisi atılır.

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


Doğrulama `login-NonInteractive` teknik profili, `AssertAccountEnabledIsTrue` talep dönüşümlerini çağırır.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertAccountEnabledIsTrue" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Kendi kendini öne süren teknik profil, doğrulama **girişi-NonInteractive** teknik profilini çağırır.

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
    - **inputClaim**: yanlış
    - **valueToCompareTo**: true
- Sonuç: Hata atıldı

## <a name="comparebooleanclaimtovalue"></a>KarşılaştırBooleanClaimToValue

Bir talebin boolean değerinin eşit `true` `false`olduğunu veya sıkıştırma sonucunu döndürecek lerini denetler.

| Öğe | DönüşümTalep Türü  | Veri Türü  | Notlar |
| ---- | ------------------------ | ---------- | ----- |
| Girişİddia | inputClaim | boole | İddia Edilecek Talep Türü. |
| ınputparameter |valueToCompareTo | boole | Karşılaştırılacak değer (doğru veya yanlış). |
| ÇıktılarTalep | compareResult | boole | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |


Aşağıdaki talep dönüştürmesi, `true` değeri olan bir boolean ClaimType'ın değerini nasıl denetlergösteriz gösterir. ClaimType değeri eşitse `true`, talep dönüşümü `true`döndürür , aksi takdirde `false`. `IsAgeOver21Years`

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
    - **inputClaim**: yanlış
- Giriş parametreleri:
    - **valueToCompareTo**: true
- Çıktı talepleri:
    - **compareResult**: false



## <a name="notclaims"></a>NotTalepleri

Boolean girişiNot işlemini GerçekleştirirClaim ve işlem sonucu outputClaim'i ayarlar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | boole | İşletilmesi iddiası. |
| ÇıktılarTalep | outputClaim | boole | Bu Talep Dönüşümünden sonra üretilen ClaimTypes çağrıldı (doğru veya yanlış). |

Bir talepüzerinde mantıksal olumsuzlama gerçekleştirmek için bu talep dönüştürmesini kullanın.

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

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim**: yanlış
- Çıktı talepleri:
    - **outputClaim**: true

## <a name="orclaims"></a>Orclaims

İki boolean inputClaims'in Bir Veya'sini hesaplar ve işlem sonucunda outputClaim'i ayarlar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim1 | boole | Değerlendirilecek ilk ClaimType. |
| Girişİddia | inputClaim2 | boole | İkinci ClaimType değerlendirmek için. |
| ÇıktılarTalep | outputClaim | boole | Bu Talep Dönüşümünden sonra üretilecek Olan Talep Türleri çağrılmıştır (doğru veya yanlış). |

Aşağıdaki talep dönüşümü, iki `Or` boolean ClaimTypes'ın nasıl Düzenleme adımında, taleplerden birinin değeri `true`.

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

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: true
    - **inputClaim2**: yanlış
- Çıktı talepleri:
    - **outputClaim**: true
