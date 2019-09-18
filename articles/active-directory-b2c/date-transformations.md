---
title: Azure Active Directory B2C Identity Experience Framework şeması için tarih talep dönüştürme örnekleri | Microsoft Docs
description: Azure Active Directory B2C Identity Experience Framework şeması için tarih talep dönüştürme örnekleri.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 077915705c242805d3709b5d52d445288fa5336a
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064343"
---
# <a name="date-claims-transformations"></a>Tarih talebi dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının Tarih talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="assertdatetimeisgreaterthan"></a>AssertDateTimeIsGreaterThan

Bir tarih ve saat talebinin (dize veri türü) ikinci bir tarih ve saat talebine (dize veri türü) göre daha sonra olduğunu denetler ve bir özel durum oluşturur.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | leftOperand | dize | İkinci talepten daha sonra olması gereken ilk talebin türü. |
| Inputclaim | rightOperand | dize | İkinci talebin türü, ilk talepten daha önce olmalıdır. |
| InputParameter | Assertıequalto | boolean | Sol işlenen sağ işlenene eşitse bu onay işaretinin geçmesi gerekip gerekmediğini belirtir. |
| InputParameter | AssertIfRightOperandIsNotPresent | boolean | Sağ işlenen eksik ise bu onay geçişinin geçmesi gerekip gerekmediğini belirtir. |
| InputParameter | TreatAsEqualIfWithinMillseconds | int | İki tarih ile aynı süreyi göz önünde bulundurmaya izin veren milisaniye sayısını belirtir (örneğin, saat eğme için hesaba). |

**AssertDateTimeIsGreaterThan** talep dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profille](self-asserted-technical-profile.md)çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. **DateTimeGreaterThan** kendiliğinden onaylanan teknik profil meta verileri, teknik profilin kullanıcıya sunduğu hata iletisini denetler.

![Assertstringclaimsareeşittir yürütme](./media/date-transformations/assert-execution.png)

Aşağıdaki örnek talebi `approvedDateTime` talep ile `currentDateTime` karşılaştırır. Sonrasında bir hata oluşur `currentDateTime`. `approvedDateTime` Dönüştürme, 5 dakika (30000 milisaniye) farklılık içinde olan değerleri eşit olarak değerlendirir.

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

Doğrulama teknik profili `AssertApprovedDateTimeLaterThanCurrentDateTime` talep dönüşümünü çağırır. `login-NonInteractive`
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
    - **leftOperand**: 2018-10-01T15:00:00.0000000 Z
    - **rightOperand**: 2018-10-01T14:00:00.0000000 Z
- Kaynaklanan Hata oluştu

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

**Tarih** ClaimType 'ı bir **DateTime** ClaimType 'a dönüştürür. Talep dönüştürmesi saat biçimini dönüştürür ve tarihe 12:00:00 ÖÖ ekler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | date | Dönüştürülecek ClaimType. |
| outputClaim | outputClaim | Tarih/saat | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

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
    - **ınputclaim**: 2019-06-01
- Çıkış talepleri:
    - **Outputclaim**: 1559347200 (1 Haziran 2019 12:00:00)

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Geçerli UTC Tarih ve saatini alın ve değeri bir ClaimType 'a ekleyin.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| outputClaim | currentDateTime | Tarih/saat | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

* Çıkış talepleri:
    * **CurrentDateTime**: 1534418820 (16 Ağustos 2018 11:27:00)

## <a name="datetimecomparison"></a>DateTimeComparison

Bir tarih saat değerinin daha sonra, daha önce veya diğer bir değere eşit olup olmadığını belirleme. Sonuç, `true` veya `false`değerine sahip yeni bir Boole ClaimType Boole değeridir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | firstDateTime | Tarih/saat | İkinci tarih saatten daha önce veya sonra olup olmadığını karşılaştırmak için ilk tarih saat. Null değer bir özel durum oluşturur. |
| Inputclaim | secondDateTime | Tarih/saat | İlk tarih saatten daha önce veya sonra olup olmadığını karşılaştırmak için ikinci tarih saat. Null değer geçerli datetTime olarak değerlendirilir. |
| InputParameter | operator | dize | Şu değerlerden biri: aynı, daha sonra veya daha önceki bir sürüm. |
| InputParameter | Timespanınseconds | int | TimeSpan öğesini ilk tarih/saate ekleyin. |
| outputClaim | Sonuç | boolean | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. |

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
    - **Firstdatetime**: 2018-01-01T00:00:00.100000 Z
    - **Seconddatetime**: 2018-04-01T00:00:00.100000 Z
- Giriş parametreleri:
    - **işleç**: daha sonra
    - **Timespanınseconds**: 7776000 (90 gün)
- Çıkış talepleri:
    - **sonuç**: true
