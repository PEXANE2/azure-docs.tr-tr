---
title: Özel ilkelerdeki telefon numarası talep dönüşümleri
titleSuffix: Azure AD B2C
description: Azure AD B2C 'de telefon numarası talep dönüştürmeleri için özel ilke başvurusu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c43e3386886456eed0c58fefd0fb1212795db66c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480170"
---
# <a name="define-phone-number-claims-transformations-in-azure-ad-b2c"></a>Azure AD B2C 'de telefon numarası talep dönüşümlerini tanımlayın

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) ' de kimlik deneyimi çerçevesi şemasının telefon numarası talep dönüştürmelerinin kullanılmasına yönelik başvuru ve örnekler sağlanmaktadır. Genel olarak talep dönüştürmeleri hakkında daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="convertstringtophonenumberclaim"></a>ConvertStringToPhoneNumberClaim

Bu talep telefon numarasının biçimini doğrular. Geçerli bir biçimde ise, Azure AD B2C tarafından kullanılan standart bir biçimde değiştirin. Belirtilen telefon numarası geçerli bir biçimde değilse, bir hata iletisi döndürülür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | Inputclaim | string | Dönüştürme dize türü talebi. |
| OutputClaim | OutputClaim | string | Bu talep dönüştürmesinin sonucu. |

**Convertstringtophonenumberclaim** talep dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profil](self-asserted-technical-profile.md) veya [görüntüleme denetimi](display-controls.md)tarafından çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. **Usermessageifclaimstransformationınvalidphonenumber** kendi kendine onaylanan teknik profil meta verileri, kullanıcıya sunulan hata iletisini denetler.

![Hata iletisi yürütme yolu diyagramı](./media/phone-authentication/assert-execution.png)

Bu talep dönüşümünü, belirtilen dize talebinin geçerli bir telefon numarası olduğundan emin olmak için kullanabilirsiniz. Aksi takdirde, bir hata iletisi oluşturulur. Aşağıdaki örnek **Phonestring** ClaimType 'ın gerçekten geçerli bir telefon numarası olduğunu denetler ve ardından standart Azure AD B2C biçimindeki telefon numarasını döndürür. Aksi takdirde, bir hata iletisi oluşturulur.

```XML
<ClaimsTransformation Id="ConvertStringToPhoneNumber" TransformationMethod="ConvertStringToPhoneNumberClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneString" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

Bu talep dönüşümünü içeren doğrulama teknik profilini çağıran kendi kendini onaylanan teknik profil, hata iletisini tanımlayabilir.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignup-Phone">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationInvalidPhoneNumber">Custom error message if the phone number is not valid.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **ınputclaim**: + 1 (123) 456-7890
- Çıkış talepleri:
  - **Outputclaim**: + 11234567890

## <a name="getnationalnumberandcountrycodefromphonenumberstring"></a>Getülke Alnumberandcountrycodefromphonenumberstring

Bu, ülke kodunu ve ulusal numarayı giriş talebine ayıklar ve isteğe bağlı olarak, sağlanan telefon numarası geçerli değilse bir özel durum oluşturur.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Inputclaim | phoneNumber | string | Telefon numarasının dize talebi. Telefon numarasının uluslararası biçimde olması gerekir ve önünde bir "+" ve ülke kodu olmalıdır. |
| InputParameter | throwExceptionOnFailure | boole | Seçim Telefon numarası geçerli olmadığında bir özel durumun oluşturulup oluşturulmayacağını gösteren bir parametre. Varsayılan değer false 'dur. |
| InputParameter | countryCodeType | string | Seçim Çıkış talebinde ülke kodu türünü gösteren bir parametre. Kullanılabilir değerler, **Callingcode** (bir ülkenin uluslararası çağrı kodu, örneğin + 1) veya **ISO3166** (iki harfli ISO-3166 ülke kodu). |
| OutputClaim | Ülke Alnumarası | string | Telefon numarası Ulusal numarası için dize talebi. |
| OutputClaim | countryCode | string | Telefon numarasının ülke kodu için dize talebi. |


**Getülke Alnumberandcountrycodefromphonenumberstring** talep dönüşümü, [kendi kendine onaylanan bir teknik profil](self-asserted-technical-profile.md) veya bir [görüntüleme denetimi eylemi](display-controls.md#display-control-actions)tarafından çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülürse, **usermessageifphonenumberparsefailure** kendini onaylanan teknik profil meta verileri kullanıcıya sunulan hata iletisini denetler.

![Hata iletisi yürütme yolu diyagramı](./media/phone-authentication/assert-execution.png)

Bu talep dönüşümünü, tam telefon numarasını ülke koduna ve ulusal numaraya bölmek için kullanabilirsiniz. Girilen telefon numarası geçerli değilse, bir hata iletisi oluşturma seçeneğini belirleyebilirsiniz.

Aşağıdaki örnek telefon numarasını Ulusal numara ve ülke koduna bölmeye çalışır. Telefon numarası geçerliyse, telefon numarası Ulusal numara tarafından geçersiz kılınır. Telefon numarası geçerli değilse, bir özel durum oluşturulmaz ve telefon numarası hala özgün değerine sahip olur.

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

Bu talep dönüşümünü içeren doğrulama teknik profilini çağıran kendi kendini onaylanan teknik profil, hata iletisini tanımlayabilir.

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
  - **PhoneNumber**: + 49 (123) 456-7890
- Giriş parametreleri:
  - **throwExceptionOnFailure**: false
  - **countrycodeType**: ISO3166
- Çıkış talepleri:
  - **ülke Alnumarası**: 1234567890
  - **CountryCode**: de

### <a name="example-2"></a>Örnek 2

- Giriş talepleri:
  - **PhoneNumber**: + 49 (123) 456-7890
- Giriş parametreleri
  - **throwExceptionOnFailure**: false
  - **countrycodeType**: callingcode
- Çıkış talepleri:
  - **ülke Alnumarası**: 1234567890
  - **CountryCode**: + 49
