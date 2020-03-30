---
title: Özel ilkeler için tarih talepleri dönüşüm örnekleri
description: Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için tarih talepleri dönüşüm örnekleri.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476697"
---
# <a name="date-claims-transformations"></a>Tarih talepleri dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Etkin Dizin B2C(Azure AD B2C) schema'daki Kimlik Deneyimi Çerçevesi şemasının tarih talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="assertdatetimeisgreaterthan"></a>Assertdatetimeisgreaterthan

Bir tarih ve saat talebinin (dize veri türü) ikinci bir tarih ve saat talebinden (string veri türü) daha geç olduğunu denetler ve bir özel durum oluşturur.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | leftOperand | string | İlk iddianın türü, ikinci iddiadan daha geç olması gerekir. |
| Girişİddia | rightOperand | string | İkinci iddianın türü, ilk iddiadan daha önce olması gerekir. |
| ınputparameter | Assertifequalto | boole | Sol operand sağ operand eşit ise bu iddia nın geçip geçmeyeceğini belirtir. |
| ınputparameter | AssertIfRightOperandIsNotPresent | boole | Doğru operand eksikse bu iddianın geçip geçmeyeceğini belirtir. |
| ınputparameter | TreatAsEqualIfWithinMillseconds | int | İki tarih zamanı arasında izin verilebilen milisaniye sayısını, süreleri eşit olarak kabul etmek için belirtir (örneğin, saat çarpıklığı için hesaba katmak için). |

**AssertDateTimeIsGreaterThan** talepleri dönüştürme her zaman kendini ileri teknik bir [profil](self-asserted-technical-profile.md)tarafından çağrılan bir doğrulama teknik [profilden](validation-technical-profile.md) yürütülür. **DateTimeGreaterThan** kendi kendine ileri teknik profil meta veri teknik profil kullanıcıya sunduğu hata iletisini denetler. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#claims-transformations-error-messages)

![AssertStringClaimsAreEşit yürütme](./media/date-transformations/assert-execution.png)

Aşağıdaki örnek, `currentDateTime` `approvedDateTime` iddiaile talebi karşılaştırır. Daha sonra `currentDateTime` ise bir `approvedDateTime`hata atılır. Dönüştürme, değerleri 5 dakika (30000 milisaniye) fark içindeyse eşit olarak değerlendirir.

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

Doğrulama `login-NonInteractive` teknik profili, `AssertApprovedDateTimeLaterThanCurrentDateTime` talep dönüşümlerini çağırır.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertApprovedDateTimeLaterThanCurrentDateTime" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Kendi kendini öne süren teknik profil, doğrulama **girişi-NonInteractive** teknik profilini çağırır.

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
    - **leftOperand**: 2020-03-01T15:00:00.000000Z
    - **rightOperand**: 2020-03-01T14:00:00.000000Z
- Sonuç: Hata atıldı

## <a name="convertdatetodatetimeclaim"></a>ConvertDateToDateTimeClaim

**Tarih** Talep Türünü **DateTime** ClaimType'a dönüştürür. Talepler dönüştürme zaman biçimini dönüştürür ve tarihe 12:00:00 ekler.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | date | Dönüştürülecek Talep Türü. |
| ÇıktılarTalep | outputClaim | tarih saat | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

Aşağıdaki örnek, talebin `dateOfBirth` (tarih veri türü) başka `dateOfBirthWithTime` bir iddiaya (dateTime veri türü) dönüştürülmesini gösterir.

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
    - **inputClaim**: 2020-15-03
- Çıktı talepleri:
    - **outputClaim**: 2020-15-03T00:00:00.000000Z

## <a name="convertdatetimetodateclaim"></a>ConvertdatetimeToDateClaim

**DateTime** ClaimType'ı **Date** ClaimType'a dönüştürür. Talepler dönüştürme tarihten zaman biçimini kaldırır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | tarih saat | Dönüştürülecek Talep Türü. |
| ÇıktılarTalep | outputClaim | date | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

Aşağıdaki örnek, talebin `systemDateTime` (dateTime veri türü) başka `systemDate` bir iddiaya (tarih veri türü) dönüştürülmesini gösterir.

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
  - **giriş**: 2020-15-03T11:34:22.000000Z
- Çıktı talepleri:
  - **outputTalep**: 2020-15-03

## <a name="getcurrentdatetime"></a>GetCurrentDateTime

Geçerli UTC tarih ve saatini alın ve bir ClaimType'a değer ekleyin.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ÇıktılarTalep | currentDateTime | tarih saat | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

```XML
<ClaimsTransformation Id="GetSystemDateTime" TransformationMethod="GetCurrentDateTime">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="systemDateTime" TransformationClaimType="currentDateTime" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

* Çıktı talepleri:
    * **currentTimeTime**: 2020-15-03T11:40:35.000000Z

## <a name="datetimecomparison"></a>DateTimeKarşılaştırma

Bir dateTime'ın daha sonra mı, daha önce mi yoksa diğerine eşit mi olduğunu belirleyin. Sonuç bir değeri `true` veya `false`ile yeni bir boolean ClaimType boolean olduğunu.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | ilkDateTime | tarih saat | İlk dateTime önce veya daha sonra ikinci dateTime daha önce veya daha geç olup olmadığını karşılaştırmak için. Null değeri bir özel durum atar. |
| Girişİddia | secondDateTime | tarih saat | İlk dateTime'dan önce mi yoksa daha geç mi olduğunu karşılaştırmak için ikinci dateTime. Null değeri geçerli datetTime olarak kabul edilir. |
| ınputparameter | operator | string | Aşağıdaki değerlerden biri: aynı, daha sonra veya daha önce. |
| ınputparameter | timeSpanInSeconds | int | Zaman dilimini ilk datetime ekleyin. |
| ÇıktılarTalep | sonuç | boole | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. |

İki ClaimTypes'ın birbirinden eşit, daha sonra mı yoksa daha önce mi olduğunu belirlemek için bu talep dönüşümlerini kullanın. Örneğin, bir kullanıcı nın hizmet şartlarınızı (TOS) en son kabul ettiği zamanı depolayabilirsiniz. 3 ay sonra, kullanıcıdan TOS'a tekrar erişmelerini isteyebilirsiniz.
Talep dönüşümlerini çalıştırmak için, önce geçerli dateTime'ı almanız ve ayrıca kullanıcının TOS'u son kez kabul etmesi gerekir.

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
    - **ilkTarih :** 2020-01-01T00:00:00.100000Z
    - **secondDateTime**: 2020-04-01T00:00:00.100000Z
- Giriş parametreleri:
    - **operator**: daha sonra
    - **timeSpanInSeconds:** 7776000 (90 gün)
- Çıktı talepleri:
    - **sonuç**: doğru
