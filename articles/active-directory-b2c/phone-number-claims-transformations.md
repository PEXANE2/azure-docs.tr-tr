---
title: Telefon numarası özel ilkelerde dönüşüm talep ediyor
titleSuffix: Azure AD B2C
description: Azure AD B2C'de telefon numarası talepleri için özel ilke başvurusu.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: bd26b2b475e293a1fda1b007289ba7c3eef35136
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183949"
---
# <a name="define-phone-number-claims-transformations-in-azure-ad-b2c"></a>Azure AD B2C'de telefon numarası talep dönüşümlerini tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının telefon numarası talep dönüşümlerini kullanmak için başvuru ve örnekler verilmektedir. Genel olarak talep dönüşümleri hakkında daha fazla bilgi [için, Bkz.](claimstransformations.md)

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="convertphonenumberclaimtostring"></a>ConvertPhoneNumberclaimtoString

Veri `phoneNumber` türünü `string` veri türüne dönüştürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | Phonenumber | Phonenumber |  Bir dize dönüştürmek için ClaimType. |
| ÇıktılarTalep | telefonNumberString | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bu örnekte, bir değer türü `phoneNumber` ile cellPhoneNumber iddia bir değer türü ile `string`bir cep Telefonu iddiadönüştürülür .

```XML
<ClaimsTransformation Id="PhoneNumberToString" TransformationMethod="ConvertPhoneNumberClaimToString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="cellPhoneNumber" TransformationClaimType="phoneNumber" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="cellPhone" TransformationClaimType="phoneNumberString" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **telefonNumarası**: +11234567890 (telefonNumarası)
- Çıktı talepleri:
  - **phoneNumberString**: +11234567890 (dize)


## <a name="convertstringtophonenumberclaim"></a>ConvertStringToPhoneNumberClaim

Bu talep dönüştürme telefon numarasının biçimini doğrular. Geçerli bir biçimdeyse, Azure AD B2C tarafından kullanılan standart bir biçime değiştirin. Sağlanan telefon numarası geçerli bir biçimde değilse, bir hata iletisi döndürülür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | telefonNumberString | string |  Telefon numarası için string iddiası. Telefon numarası, önde gelen bir "+" ve ülke kodu ile birlikte uluslararası formatta olmalıdır. Giriş talebi `country` sağlanıyorsa, telefon numarası yerel formattadır (ülke kodu olmadan). |
| Girişİddia | ülke | string | [İsteğe bağlı] ISO3166 formatında telefon numarasının ülke kodu (iki harfli ISO-3166 ülke kodu) için dize talebi. |
| ÇıktılarTalep | outputClaim | Phonenumber | Bu talep dönüşüm sonucu. |

**ConvertStringToPhoneNumberClaim** dönüşüm her zaman [kendini ileri teknik profil](self-asserted-technical-profile.md) veya ekran [denetimi](display-controls.md)tarafından çağrılan bir doğrulama [teknik profilden](validation-technical-profile.md) yürütülür iddia ediyor. **UserMessageIfClaimsTransformationInvalidPhoneNumber** kendi kendine ileri teknik profil meta veri kullanıcıya sunulan hata iletisini denetler.

![Hata iletisi yürütme yolu diyagramı](./media/phone-authentication/assert-execution.png)

Sağlanan dize talebinin geçerli bir telefon numarası olduğundan emin olmak için bu talep dönüştürmesini kullanabilirsiniz. Değilse, bir hata iletisi atılır. Aşağıdaki örnek, **phoneString** ClaimType'ın gerçekten geçerli bir telefon numarası olup olmadığını denetler ve ardından telefon numarasını standart Azure AD B2C biçiminde döndürür. Aksi takdirde, bir hata iletisi atılır.

```XML
<ClaimsTransformation Id="ConvertStringToPhoneNumber" TransformationMethod="ConvertStringToPhoneNumberClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneString" TransformationClaimType="phoneNumberString" />
    <InputClaim ClaimTypeReferenceId="countryCode" TransformationClaimType="country" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

Bu talep dönüştürmesini içeren doğrulama teknik profilini çağıran kendi kendini ileri süren teknik profil hata iletisini tanımlayabilir.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignup-Phone">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationInvalidPhoneNumber">Custom error message if the phone number is not valid.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

### <a name="example-1"></a>Örnek 1

- Giriş talepleri:
  - **telefonNumberString**: 033 456-7890
  - **ülke**: DK
- Çıktı talepleri:
  - **outputTalep**: +450334567890

### <a name="example-2"></a>Örnek 2

- Giriş talepleri:
  - **telefonNumberString**: +1 (123) 456-7890
- Çıktı talepleri:
  - **outputTalep**: +11234567890


## <a name="getnationalnumberandcountrycodefromphonenumberstring"></a>GetnationalNumberAndCountryCodeFromPhoneNumberString

Bu, giriş talebinden ülke kodunu ve ulusal numarayı ayıklar ve sağlanan telefon numarası geçerli değilse isteğe bağlı olarak bir özel durum atar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | Phonenumber | string | Telefon numarasının string iddiası. Telefon numarası, önde gelen bir "+" ve ülke kodu ile birlikte uluslararası formatta olmalıdır. |
| ınputparameter | atmaExceptionOnFailure | boole | [İsteğe bağlı] Telefon numarası geçerli olmadığında özel durum atılıp atılmadığını belirten bir parametre. Varsayılan değer yanlıştır. |
| ınputparameter | ülkeCodeType | string | [İsteğe bağlı] Çıktı talebindeki ülke kodu türünü gösteren bir parametre. Kullanılabilir değerler **CallingCode** (örneğin +1) veya **ISO3166** (iki harfli ISO-3166 ülke kodu) bir ülkenin uluslararası çağrı kodudur. |
| ÇıktılarTalep | nationalNumber | string | Telefon numarasının ulusal numarası için string iddiası. |
| ÇıktılarTalep | ülkeKodu | string | Telefon numarasının ülke kodu için string iddiası. |


**GetNationalNumberAndCountryCodeFromPhoneNumberString,** [kendi kendine öne çıkan](self-asserted-technical-profile.md) teknik profil veya ekran kontrol [eylemi](display-controls.md#display-control-actions)tarafından çağrılan bir doğrulama [teknik profilinden](validation-technical-profile.md) dönüşüm talep ederse, **UserMessageIfPhoneNumberParseFailure** kendi kendini öne süren teknik profil meta verileri kullanıcıya sunulan hata iletisini denetler.

![Hata iletisi yürütme yolu diyagramı](./media/phone-authentication/assert-execution.png)

Bu talep dönüşümünden tam telefon numarasını ülke koduna ve ulusal numaraya bölmek için kullanabilirsiniz. Sağlanan telefon numarası geçerli değilse, bir hata iletisi atmayı seçebilirsiniz.

Aşağıdaki örnekte, telefon numarasını ulusal numaraya ve ülke koduna bölmeye çalışır. Telefon numarası geçerliyse, telefon numarası ulusal numara tarafından geçersiz kılınacaktır. Telefon numarası geçerli değilse, bir özel durum atılmaz ve telefon numarası hala orijinal değerine sahiptir.

```XML
<ClaimsTransformation Id="GetNationalNumberAndCountryCodeFromPhoneNumberString" TransformationMethod="GetNationalNumberAndCountryCodeFromPhoneNumberString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="phoneNumber" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="throwExceptionOnFailure" DataType="boolean" Value="false" />
    <InputParameter Id="countryCodeType" DataType="string" Value="ISO3166" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="nationalNumber" TransformationClaimType="nationalNumber" />
    <OutputClaim ClaimTypeReferenceId="countryCode" TransformationClaimType="countryCode" />
  </OutputClaims>
</ClaimsTransformation>
```

Bu talep dönüştürmesini içeren doğrulama teknik profilini çağıran kendi kendini ileri süren teknik profil hata iletisini tanımlayabilir.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignup-Phone">
  <Metadata>
    <Item Key="UserMessageIfPhoneNumberParseFailure">Custom error message if the phone number is not valid.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

### <a name="example-1"></a>Örnek 1

- Giriş talepleri:
  - **telefon Numarası**: +49 (123) 456-7890
- Giriş parametreleri:
  - **throwExceptionOnFailure**: false
  - **countryCodeType**: ISO3166
- Çıktı talepleri:
  - **nationalNumber**: 1234567890
  - **countryCode**: DE

### <a name="example-2"></a>Örnek 2

- Giriş talepleri:
  - **telefon Numarası**: +49 (123) 456-7890
- Giriş parametreleri
  - **throwExceptionOnFailure**: false
  - **countryCodeType**: CallingCode
- Çıktı talepleri:
  - **nationalNumber**: 1234567890
  - **ülkeKodu**: +49
