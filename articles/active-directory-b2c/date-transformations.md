---
title: Özel ilkeler için tarih talebi dönüştürme örnekleri
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için tarih talep dönüştürme örnekleri.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/16/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c02ac9392d6f3f95deef38ff86250e96dfb76d96
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79476697"
---
# <a name="date-claims-transformations"></a>Tarih talebi dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının Tarih talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="assertdatetimeisgreaterthan"></a>AssertDateTimeIsGreaterThan

Bir tarih ve saat talebinin (dize veri türü) ikinci bir tarih ve saat talebine (dize veri türü) göre daha sonra olduğunu denetler ve bir özel durum oluşturur.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | leftOperand | string | İkinci talepten daha sonra olması gereken ilk talebin türü. |
| Inputclaim | rightOperand | string | İkinci talebin türü, ilk talepten daha önce olmalıdır. |
| InputParameter | Assertıequalto | boole | Sol işlenen sağ işlenene eşitse bu onay işaretinin geçmesi gerekip gerekmediğini belirtir. |
| InputParameter | AssertIfRightOperandIsNotPresent | boole | Sağ işlenen eksik ise bu onay geçişinin geçmesi gerekip gerekmediğini belirtir. |
| InputParameter | TreatAsEqualIfWithinMillseconds | int | İki tarih ile aynı süreyi göz önünde bulundurmaya izin veren milisaniye sayısını belirtir (örneğin, saat eğme için hesaba). |

**AssertDateTimeIsGreaterThan** talep dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profille](self-asserted-technical-profile.md)çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. **DateTimeGreaterThan** kendiliğinden onaylanan teknik profil meta verileri, teknik profilin kullanıcıya sunduğu hata iletisini denetler. Hata iletileri [yerelleştirilebilecek](localization-string-ids.md#claims-transformations-error-messages).

![Assertstringclaimsareeşittir yürütme](./media/date-transformations/assert-execution.png)

Aşağıdaki örnek talebi `approvedDateTime` talep ile `currentDateTime` karşılaştırır. Sonrasında bir hata oluşur `currentDateTime` `approvedDateTime`. Dönüştürme, 5 dakika (30000 milisaniye) farklılık içinde olan değerleri eşit olarak değerlendirir.

```XML
<ClaimsTransformation Id="AssertApprovedDateTimeLaterThanCurrentDateTime" TransformationMethod="AssertDateTimeIsGreaterThan">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="approvedDateTime" TransformationClaimType="leftOperand" />
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="rightOperand" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
    <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
    <InputParameter Id="TreatAsEqualIfWithinMillseconds" DataType="int" Value="300000" />
  </InputParameters>
</ClaimsTransformation>
```

`login-NonInteractive` Doğrulama teknik profili `AssertApprovedDateTimeLaterThanCurrentDateTime` talep dönüşümünü çağırır.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertApprovedDateTimeLaterThanCurrentDateTime" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Otomatik olarak onaylanan teknik profil, doğrulama **oturum açma-etkileşimsiz** teknik profilini çağırır.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="DateTimeGreaterThan">Custom error message if the provided left operand is greater than the right operand.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **leftOperand**: 2020-03-01T15:00:00.0000000 z
    - **rightOperand**: 2020-03-01T14:00:00.0000000 z
- Sonuç: hata oluştu

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

**Tarih** ClaimType 'ı bir **DateTime** ClaimType 'a dönüştürür. Talep dönüştürmesi saat biçimini dönüştürür ve tarihe 12:00:00 ÖÖ ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | ınputclaim | date | Dönüştürülecek ClaimType. |
| OutputClaim | outputClaim | tarih saat | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Aşağıdaki örnek, talebin `dateOfBirth` (Tarih veri türü) başka bir talebe `dateOfBirthWithTime` (TarihSaat veri türü) dönüştürülmesini gösterir.

```XML
  <ClaimsTransformation Id="ConvertToDateTime" TransformationMethod="ConvertDateToDateTimeClaim">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="dateOfBirth" TransformationClaimType="inputClaim" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="dateOfBirthWithTime" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **ınputclaim**: 2020-15-03
- Çıkış talepleri:
    - **Outputclaim**: 2020-15-03T00:00:00.0000000 z

## <a name="convertdatetimetodateclaim"></a>ConvertDateTimeToDateClaim

Bir **DateTime** ClaimType 'ı bir **Tarih** ClaimType 'a dönüştürür. Talep dönüştürmesi saat biçimini tarihten itibaren kaldırır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | ınputclaim | tarih saat | Dönüştürülecek ClaimType. |
| OutputClaim | outputClaim | date | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

Aşağıdaki örnek, talebin `systemDateTime` (TarihSaat veri türü) başka bir talebe `systemDate` (Tarih veri türü) dönüştürülmesini gösterir.

```XML
<ClaimsTransformation Id="ConvertToDate" TransformationMethod="ConvertDateTimeToDateClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDate" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **ınputclaim**: 2020-15-03T11:34:22.0000000 z
- Çıkış talepleri:
  - **Outputclaim**: 2020-15-03

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Geçerli UTC Tarih ve saatini alın ve değeri bir ClaimType 'a ekleyin.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | currentDateTime | tarih saat | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

* Çıkış talepleri:
    * **CurrentDateTime**: 2020-15-03T11:40:35.0000000 z

## <a name="datetimecomparison"></a>DateTimeComparison

Bir tarih saat değerinin daha sonra, daha önce veya diğer bir değere eşit olup olmadığını belirleme. Sonuç, `true` veya `false`değerine sahip yeni bir Boole ClaimType Boole değeridir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | firstDateTime | tarih saat | İkinci tarih saatten daha önce veya sonra olup olmadığını karşılaştırmak için ilk tarih saat. Null değer bir özel durum oluşturur. |
| Inputclaim | secondDateTime | tarih saat | İlk tarih saatten daha önce veya sonra olup olmadığını karşılaştırmak için ikinci tarih saat. Null değer geçerli datetTime olarak değerlendirilir. |
| InputParameter | operator | string | Şu değerlerden biri: aynı, daha sonra veya daha önceki bir sürüm. |
| InputParameter | Timespanınseconds | int | TimeSpan öğesini ilk tarih/saate ekleyin. |
| OutputClaim | sonuç | boole | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

İki ClaimTypes 'ın eşit, daha sonra veya daha önceki bir sürümü olup olmadığını anlamak için bu talep dönüşümünü kullanın. Örneğin, bir kullanıcının hizmet koşullarınızı (TOS) en son kabul ettiği zamanı saklayabilirsiniz. 3 aydan sonra kullanıcıdan TOS 'a yeniden erişmesini isteyebilirsiniz.
Talep dönüşümünü çalıştırmak için, ilk olarak geçerli tarih saat ve ayrıca Kullanıcı TOS 'ı kabul eden son zamanı almanız gerekir.

```XML
<ClaimsTransformation Id="CompareLastTOSAcceptedWithCurrentDateTime" TransformationMethod="DateTimeComparison">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="currentDateTime" TransformationClaimType="firstDateTime" />
    <InputClaim ClaimTypeReferenceId="extension_LastTOSAccepted" TransformationClaimType="secondDateTime" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="later than" />
    <InputParameter Id="timeSpanInSeconds" DataType="int" Value="7776000" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isLastTOSAcceptedGreaterThanNow" TransformationClaimType="result" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **Firstdatetime**: 2020-01-01T00:00:00.100000 z
    - **Seconddatetime**: 2020-04-01T00:00:00.100000 z
- Giriş parametreleri:
    - **işleç**: daha sonra
    - **Timespanınseconds**: 7776000 (90 gün)
- Çıkış talepleri:
    - **sonuç**: true
