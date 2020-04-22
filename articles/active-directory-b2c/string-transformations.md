---
title: Özel ilkeler için string talepleri dönüşüm örnekleri
titleSuffix: Azure AD B2C
description: String, Azure Active Directory B2C'nin Kimlik Deneyimi Çerçevesi (IEF) şeması için dönüşüm örnekleri talep eder.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: f08107874598a68fb5ce2a1a8a98b6a81d7b94d4
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81756784"
---
# <a name="string-claims-transformations"></a>String talepleri dönüşümleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Etkin Dizin B2C (Azure AD B2C) kimlik deneyimi çerçevesi şemasının dize talep dönüşümlerini kullanmak için örnekler verilmektedir. Daha fazla bilgi için [Bkz.](claimstransformations.md)

## <a name="assertstringclaimsareequal"></a>AssertstringClaimsAreEqual

İki talebi karşılaştırın ve belirtilen karşılaştırma girişiclaim1, inputClaim2 ve stringComparison'a göre eşit değilse bir özel durum atın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim1 | string | İlk iddianın türü, karşılaştırılacak olan. |
| Girişİddia | inputClaim2 | string | İkinci iddianın türü, karşılaştırılacak olan. |
| ınputparameter | Stringcomparison | string | string karşılaştırma, değerlerden biri: Ordinal, OrdinalIgnoreCase. |

**AssertStringClaimsAreEqual** talepleri dönüşüm her zaman kendi kendine ileri teknik [profil](self-asserted-technical-profile.md)veya [DisplayConrtol](display-controls.md)tarafından çağrılan bir [doğrulama teknik profilden](validation-technical-profile.md) yürütülür . Kendi `UserMessageIfClaimsTransformationStringsAreNotEqual` kendini ileri süren teknik profilin meta verileri, kullanıcıya sunulan hata iletisini denetler. Hata iletileri [yerelleştirilebilir.](localization-string-ids.md#claims-transformations-error-messages)


![AssertStringClaimsAreEşit yürütme](./media/string-transformations/assert-execution.png)

İki ClaimTypes'ın aynı değere sahip olduğundan emin olmak için bu talep dönüştürmesini kullanabilirsiniz. Değilse, bir hata iletisi atılır. Aşağıdaki örnek, **güçlü AuthenticationEmailAddress** ClaimType **e-posta** ClaimType eşit olup olmadığını denetler. Aksi takdirde bir hata iletisi atılır.

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


**Giriş-NonInteractive** doğrulama teknik profil **AssertEmailAndStrongAuthenticationEmailAddressAreEqual** talepleri dönüşüm çağırır.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Kendi kendini öne süren teknik profil, doğrulama **girişi-NonInteractive** teknik profilini çağırır.

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim1**:someone@contoso.com
  - **inputClaim2**:someone@outlook.com
- Giriş parametreleri:
  - **stringKarşılaştırma**: ordinalIgnoreCase
- Sonuç: Hata atıldı

## <a name="changecase"></a>Değişiklik Durumu

Operatöre bağlı olarak sağlanan talebin alt veya büyük harf eki durumunu değiştirir.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim1 | string | Değiştirilecek Talep Türü. |
| ınputparameter | toCase | string | Aşağıdaki değerlerden `LOWER` biri: `UPPER`veya . |
| ÇıktılarTalep | outputClaim | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Herhangi bir dize ClaimType'ı alt veya büyük harfolarak değiştirmek için bu talep dönüştürmesini kullanın.

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **e-posta**:SomeOne@contoso.com
- Giriş parametreleri:
    - **toCase**: ALT
- Çıktı talepleri:
  - **e-posta**:someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim

Dönüştürmede sağlanan giriş parametresinden bir dize iddiası oluşturur.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
|----- | ----------------------- | --------- | ----- |
| ınputparameter | value | string | Ayarlanacak dize. Bu giriş parametresi [dize talepleri dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler. |
| ÇıktılarTalep | createdClaim | string | Bu talep dönüştürmesinden sonra üretilen ClaimType, giriş parametresinde belirtilen değerle birlikte çağrıldı. |

Bir dize ClaimType değeri ayarlamak için bu talep dönüşümkullanın.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş parametresi:
    - **değer**: Contoso hizmet şartları...
- Çıktı talepleri:
    - **createdClaim**: TOS ClaimType "Contoso hizmet şartları..." Değer.

## <a name="compareclaims"></a>Karşılaştırma Talepleri

Bir dize iddiasının diğerine eşit olup olmadığını belirleyin. Sonuç, değeri `true` veya `false`.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim1 | string | Karşılaştırılacak olan ilk talep türü. |
| Girişİddia | inputClaim2 | string | Karşılaştırılacak ikinci talep türü. |
| ınputparameter | operator | string | Olası `EQUAL` değerler: `NOT EQUAL`veya . |
| ınputparameter | Ignorecase | boole | Bu karşılaştırmanın karşılaştırılan dizeleri durumunda yok sayması gerekip gerekmediğini belirtir. |
| ÇıktılarTalep | outputClaim | boole | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bir talebin başka bir iddiaya eşit olup olmadığını denetlemek için bu talep dönüşümlerini kullanın. Örneğin, **e-posta** talebinin değeri **Doğrulanmış.E-posta** talebine eşitse aşağıdaki talepler dönüşüm denetimi ne olur.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim1**:someone@contoso.com
  - **inputClaim2**:someone@outlook.com
- Giriş parametreleri:
    - **operatör**: EŞIT Değİl
    - **ignoreCase**: true
- Çıktı talepleri:
    - **outputClaim**: true

## <a name="compareclaimtovalue"></a>KarşılaştırmaClaimToValue

Talep değerinin giriş parametresi değerine eşit olup olmadığını belirler.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim1 | string | İddianın türü, karşılaştırılacak olan. |
| ınputparameter | operator | string | Olası `EQUAL` değerler: `NOT EQUAL`veya . |
| ınputparameter | compareTo | string | string karşılaştırma, değerlerden biri: Ordinal, OrdinalIgnoreCase. |
| ınputparameter | Ignorecase | boole | Bu karşılaştırmanın karşılaştırılan dizeleri durumunda yok sayması gerekip gerekmediğini belirtir. |
| ÇıktılarTalep | outputClaim | boole | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Bu talep dönüşümlerini, bir talebin belirttiğiniz değere eşit olup olmadığını denetlemek için kullanabilirsiniz. Örneğin, aşağıdaki talepler dönüşüm **koşullarıOfUseConsentVersion** talebinin değeri eşitse `v1`denetler .

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek
- Giriş talepleri:
    - **inputClaim1**: v1
- Giriş parametreleri:
    - **compareTo**: V1
    - **işleç**: EQUAL
    - **ignoreCase**: true
- Çıktı talepleri:
    - **outputClaim**: true

## <a name="createrandomstring"></a>RandomString oluşturma

Rasgele sayı üreteci kullanarak rasgele bir dize oluşturur. Rasgele sayı üreteci `integer`türünde ise, isteğe bağlı olarak bir tohum parametresi ve maksimum sayıda sağlanabilir. İsteğe bağlı dize biçimi parametresi, çıktının onu kullanarak biçimlendirilmesine izin verir ve isteğe bağlı bir base64 parametresi çıktının base64 kodlanmış randomGeneratorType [guid, integer] outputClaim (String) olup olmadığını belirtir.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputparameter | randomGeneratorType | string | Oluşturulacak rasgele değeri (genel `GUID` benzersiz kimlik) veya `INTEGER` (bir sayı) belirtir. |
| ınputparameter | Stringformat | string | [İsteğe bağlı] Rasgele değeri biçimlendirin. |
| ınputparameter | base64 | boole | [İsteğe bağlı] Rasgele değeri base64'e dönüştürün. Dize biçimi uygulanırsa, dize biçiminden sonraki değer base64'e kodlanır. |
| ınputparameter | maksimumSayı | int | [İsteğe bağlı] RandomGeneratorType `INTEGER` için sadece. Maksimum sayıyı belirtin. |
| ınputparameter | Tohum  | int | [İsteğe bağlı] RandomGeneratorType `INTEGER` için sadece. Rasgele değer için tohumu belirtin. Not: aynı tohum rasgele sayılar aynı sırayı verir. |
| ÇıktılarTalep | outputClaim | string | Bu talep dönüşümünden sonra üretilecek Olan Talep Türleri çağrıldı. Rasgele değer. |

Aşağıdaki örnek, genel benzersiz bir kimlik oluşturur. Bu talep dönüştürme rasgele UPN (kullanıcı ilkesi adı) oluşturmak için kullanılır.

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş parametreleri:
    - **randomGeneratorType**: GUID
- Çıktı talepleri:
    - **outputClaim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

Aşağıdaki örnek, 0 ile 1000 arasında bir bir ararasgele değer oluşturur. Değer OTP_{rasgele değer} olarak biçimlendirilir.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş parametreleri:
    - **randomGeneratorType**: INTEGER
    - **maksimum Sayı**: 1000
    - **stringFormat**: OTP_{0}
    - **base64**: yanlış
- Çıktı talepleri:
    - **outputClaim**: OTP_853


## <a name="formatstringclaim"></a>BiçimStringClaim

Bir talebi sağlanan biçim dizesine göre biçimlendirin. Bu dönüştürme C# `String.Format` yöntemini kullanır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim |string |String biçim {0} parametresi gibi davranan ClaimType. |
| ınputparameter | Stringformat | string | {0} Parametre de dahil olmak üzere dize biçimi. Bu giriş parametresi [dize talepleri dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler.  |
| ÇıktılarTalep | outputClaim | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Herhangi bir dizeyi tek bir {0}parametreyle biçimlendirmek için bu talep dönüştürmesini kullanın. Aşağıdaki örnekte bir **userPrincipalName**oluşturur. Tüm sosyal kimlik sağlayıcı teknik `Facebook-OAUTH` profilleri, örneğin **createUserPrincipalName'i** bir **userPrincipalName**oluşturmak için çağırır.

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **giriş :** 5164db16-3eee-4629-bfda-dcc3326790e9
- Giriş parametreleri:
    - **stringFormat**:{0}cpim_ @{RelyingPartyTenantId}
- Çıktı talepleri:
  - **outputClaim**:cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>BiçimStringMultipleClaims

Sağlanan biçim dizesine göre iki talebi biçimlendirin. Bu dönüştürme C# `String.Format` yöntemini kullanır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim |string | String biçim {0} parametresi gibi davranan ClaimType. |
| Girişİddia | inputClaim | string | String biçim {1} parametresi gibi davranan ClaimType. |
| ınputparameter | Stringformat | string | Dize biçimi, {0} ve {1} parametreleri de dahil olmak üzere. Bu giriş parametresi [dize talepleri dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler.   |
| ÇıktılarTalep | outputClaim | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı. |

Herhangi bir dizeyi iki parametreyle biçimlendirmek için bu talep dönüştürmesini kullanın {0} ve {1}. Aşağıdaki örnek, belirtilen biçime sahip bir **displayName** oluşturur:

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim1**: Joe
    - **inputClaim2**: Fernando
- Giriş parametreleri:
    - **stringFormat** {0} :{1}
- Çıktı talepleri:
    - **outputClaim**: Joe Fernando

## <a name="getlocalizedstringstransformation"></a>GetLocalizedStringsTransformation

Yerelleştirilmiş dizeleri taleplere kopyalar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ÇıktılarTalep | Yerelleştirilmiş dize adı | string | Bu talep dönüşümünden sonra üretilen talep türlerinin listesi çağrıldı. |

GetLocalizedStringsTransformation taleplerini dönüştürme kullanmak için:

1. Bir [yerelleştirme dizesi](localization.md) tanımlayın ve [kendi kendini ileri süren teknik profille](self-asserted-technical-profile.md)ilişkilendirin.
1. Öğenin `ElementType` `LocalizedString` adı `GetLocalizedStringsTransformationClaimType`' na ayarlanmalıdır.
1. Tanımladığınız `StringId` benzersiz bir tanımlayıcıdır ve daha sonra talep dönüşümünde bunu kullanır.
1. Talepler dönüşümünde, yerelleştirilmiş dizeyle ayarlanacak talep listesini belirtin. Bu, `ClaimTypeReferenceId` ilkede ClaimsSchema bölümünde zaten tanımlanmış bir ClaimType'a bir başvurudur. Bu `TransformationClaimType` `StringId` `LocalizedString` öğenin tanımlandığı gibi yerelleştirilmiş dize adıdır.
1. Kendi kendini öne süren bir [teknik profilde](self-asserted-technical-profile.md)veya [ekran denetimi](display-controls.md) girdisi veya çıktı talepleri dönüşümünde, talep dönüşümünüzün bir referansını yapın.

![GetLocalizedStringsTransformation](./media/string-transformations/get-localized-strings-transformation.png)

Aşağıdaki örnekte, yerelleştirilmiş dizeleri e-posta özne, gövde, kod iletisi ve e-postanın imzası bakar. Bu talepler daha sonra özel e-posta doğrulama şablonu tarafından kullanılır.

İngilizce (varsayılan) ve İspanyolca için yerelleştirilmiş dizeleri tanımlayın.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="Append">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
   </SupportedLanguages>

  <LocalizedResources Id="api.localaccountsignup.en">
    <LocalizedStrings>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_subject">Contoso account email verification code</LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_message">Thanks for verifying your account!</LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_code">Your code is</LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_signature">Sincerely</LocalizedString>
     </LocalizedStrings>
   </LocalizedResources>
   <LocalizedResources Id="api.localaccountsignup.es">
     <LocalizedStrings>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_subject">Código de verificación del correo electrónico de la cuenta de Contoso</LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_message">Gracias por comprobar la cuenta de </LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_code">Su código es</LocalizedString>
      <LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_signature">Atentamente</LocalizedString>
    </LocalizedStrings>
  </LocalizedResources>
</Localization>
```

Talep dönüştürme, `StringId` *email_subject*değeri ile talep türü *öznenin* değerini ayarlar.

```XML
<ClaimsTransformation Id="GetLocalizedStringsForEmail" TransformationMethod="GetLocalizedStringsTransformation">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="subject" TransformationClaimType="email_subject" />
    <OutputClaim ClaimTypeReferenceId="message" TransformationClaimType="email_message" />
    <OutputClaim ClaimTypeReferenceId="codeIntro" TransformationClaimType="email_code" />
    <OutputClaim ClaimTypeReferenceId="signature" TransformationClaimType="email_signature" />
   </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Çıktı talepleri:
  - **konu**: Contoso hesap e-posta doğrulama kodu
  - **mesaj**: Hesabınızı doğruladınız için teşekkürler!
  - **codeIntro**: Kodunuz
  - **imza**: Saygılarımla


## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Talep **Kısıtlaması** koleksiyonundan bir öğeyi arama.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | haritaFromClaim | string | **KısıtlamaDeğer Talebi** taleplerinde aranacak metni içeren talep, **Kısıtlama** koleksiyonu ile birlikte.  |
| ÇıktılarTalep | kısıtlamaValueClaim | string | **Kısıtlama** koleksiyonunu içeren talep. Talep dönüştürme çağrıldıktan sonra, bu talebin değeri seçili öğenin değerini içerir. |

Aşağıdaki örnekte hata anahtarını temel alan hata iletisi açıklaması bakmaktadır. **YanıtMsg** talebi, son kullanıcıya sunulması veya güvenen tarafa gönderilmek üzere bir hata iletisi koleksiyonu içerir.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cannot sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
Talepler dönüşümü öğenin metnini arar ve değerini döndürür. Kısıtlama kullanılarak `<LocalizedCollection>`yerelleştirilmişse, talep dönüşümü yerelleştirilmiş değeri döndürür.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />        
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **mapFromClaim**: B2C_V1_90001
- Çıktı talepleri:
    - **restrictionValueClaim**: Reşit değilseniz oturum açamazsınız.

## <a name="lookupvalue"></a>Lookupvalue

Başka bir talebin değerini temel alan değerler listesinden bir talep değeri arayın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputParameterId | string | Arama değerini içeren talep |
| ınputparameter | |string | GirişParametrelerinin toplanması. |
| ınputparameter | errorOnFailedLookup | boole | Eşleşen bir arama olmadığında bir hatanın döndürülüp döndürülmediğini denetleme. |
| ÇıktılarTalep | inputParameterId | string | Bu talep dönüşümünden sonra üretilecek Olan Talep Türleri çağrıldı. Eşleşen `Id`değeri . |

Aşağıdaki örnekte, girişParametreleri koleksiyonlarından birinde alan adı yukarı bakar. Talep dönüştürme tanımlayıcısında etki alanı adını arar ve değerini (uygulama kimliği) döndürür.

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputParameterId**: test.com
- Giriş parametreleri:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: yanlış
- Çıktı talepleri:
    - **çıkış İddia**: c7026f88-4299-4cdb-965d-3f166464b8a9

Giriş `errorOnFailedLookup` parametresi `true`ayarlandığında, **LookupValue** talepleri dönüştürme her zaman [kendi kendine ileri sürülen teknik profil](self-asserted-technical-profile.md)veya [DisplayConrtol](display-controls.md)tarafından çağrılan bir [doğrulama teknik profilden](validation-technical-profile.md) yürütülür. Kendi `LookupNotFound` kendini ileri süren teknik profilin meta verileri, kullanıcıya sunulan hata iletisini denetler.

![AssertStringClaimsAreEşit yürütme](./media/string-transformations/assert-execution.png)

Aşağıdaki örnekte, girişParametreleri koleksiyonlarından birinde alan adı yukarı bakar. Talepler dönüşümü tanımlayıcıdaki etki alanı adını arar ve değerini (uygulama kimliği) döndürür veya bir hata iletisi yükseltir.

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputParameterId**: live.com
- Giriş parametreleri:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: true
- Hata:
    - Giriş parametre kimlikleri ve hatasıonFailedLookup listesinde giriş talep değeri için bulunan hiçbir eşleşme doğrudur.


## <a name="nullclaim"></a>NullClaim

Belirli bir talebin değerini temizleyin.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ÇıktılarTalep | claim_to_null | string | Talebin değeri NULL olarak ayarlanır. |

Oturum çerezinin daha küçük olması için, talep özelliği çantasındaki gereksiz verileri kaldırmak için bu talep dönüştürmesini kullanın. Aşağıdaki örnek, `TermsOfService` talep türünün değerini kaldırır.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **outputClaim**: Contoso App'e hoş geldiniz. Bu web sitesine göz atmaya ve kullanmaya devam ederseniz, aşağıdaki hüküm ve koşullara uymayı ve bunlara bağlı olmayı kabul edersiniz...
- Çıktı talepleri:
    - **outputClaim**: NULL

## <a name="parsedomain"></a>Ayrışdırmalı Etki Alanı

Bir e-posta adresinin etki alanı bölümünü alır.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | Emailaddress | string | E-posta adresini içeren ClaimType. |
| ÇıktılarTalep | etki alanı | string | Bu talep dönüşümünden sonra üretilen ClaimType çağrıldı - etki alanı. |

Kullanıcının @ sembolünden sonra alan adını ayrışdırmak için bu talep dönüşümlerini kullanın. Aşağıdaki talep dönüşümü, alan adının **e-posta** talebinden nasıl ayrışdırılabildiğini gösterir.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **emailAdres**:joe@outlook.com
- Çıktı talepleri:
    - **domain**: outlook.com

## <a name="setclaimsifregexmatch"></a>SetClaimsIfRegexMatch

Bir dize `claimToMatch` talebi `matchTo` ve giriş parametresi eşit olup olmadığını denetler `outputClaimIfMatched` ve giriş parametresinde bulunan değerle çıktı taleplerini `true` ve `false` karşılaştırma sonucu olarak veya temel alınacak sonuç çıktısı iddiasını karşılaştırın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| inputClaim | iddiaToMatch | string | Karşılaştırılacak olan talep türü. |
| ınputparameter | matchTo | string | Eşleşmek için normal ifade. |
| ınputparameter | outputClaimIfMatched | string | Dizeleri eşitse ayarlanacak değer. |
| ınputparameter | extractGroups | boole | [İsteğe bağlı] Regex eşleşmesinin grup değerlerini ayıklayıp ayıklamayacağı belirtilir. Olası değerler: `true` `false` , veya (varsayılan). | 
| ÇıktılarTalep | outputClaim | string | Normal ifade eşleşirse, bu çıktı `outputClaimIfMatched` talebi giriş parametresinin değerini içerir. Ya da kibrit yoksa null. |
| ÇıktılarTalep | regexCompareResultClaim | boole | Normal ifade, eşleştirme sonucu olarak `true` veya `false` eşleçlik sonucuna göre ayarlanacak sonuç çıktısı talep türüyle eşleşir. |
| ÇıktılarTalep| İddianın adı| string | ExtractGroups giriş parametresi doğru ayarlanmışsa, bu talep dönüştürmeden sonra üretilen talep türlerinin listesi çağrılmıştır. ClaimType adı Regex grup adı eşleşmelidir. | 

### <a name="example-1"></a>Örnek 1

Sağlanan telefon numarasının geçerli olup olmadığını, telefon numarası normal ifade desenine göre denetler.

```XML
<ClaimsTransformation Id="SetIsPhoneRegex" TransformationMethod="SetClaimsIfRegexMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phone" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="^[0-9]{4,16}$" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="isPhone" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="validationResult" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isPhoneBoolean" TransformationClaimType="regexCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **claimToMatch**: "64854114520"
- Giriş parametreleri:
    - **matchTo**: "^[0-9]{4,16}$"
    - **outputClaimIfMatched**: "isPhone"
- Çıktı talepleri:
    - **outputClaim**: "isPhone"
    - **regexCompareResultClaim**: true

### <a name="example-2"></a>Örnek 2

Sağlanan e-posta adresinin geçerli olup olmadığını denetler ve e-posta takma adını döndürün.

```XML
<ClaimsTransformation Id="GetAliasFromEmail" TransformationMethod="SetClaimsIfRegexMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="(?&lt;mailAlias&gt;.*)@(.*)$" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="isEmail" />
    <InputParameter Id="extractGroups" DataType="boolean" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="validationResult" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isEmailString" TransformationClaimType="regexCompareResultClaim" />
    <OutputClaim ClaimTypeReferenceId="mailAlias" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **claimToMatch**:emily@contoso.com" "
- Giriş parametreleri:
    - **matchTo**:`(?&lt;mailAlias&gt;.*)@(.*)$`
    - **outputClaimIfMatched**: "isEmail"
    - **extractGroups**: true
- Çıktı talepleri:
    - **outputClaim**: "isEmail"
    - **regexCompareResultClaim**: true
    - **mailAlias**: emily
    
## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

Bir dize talebi `matchTo` ve giriş parametresi eşit olup olmadığını denetler `stringMatchMsg` `stringMatchMsgCode` ve çıktı taleplerini, karşılaştırma sonucu olarak `true` veya `false` temel alınacak karşılaştırma sonucunu karşılaştırma sonucuyla karşılaştırın, mevcut değer ve giriş parametreleriyle birlikte ayarlar.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | string | Karşılaştırılacak olan talep türü. |
| ınputparameter | matchTo | string | Ile karşılaştırıldığında dize `inputClaim`. |
| ınputparameter | Stringcomparison | string | Olası `Ordinal` değerler: `OrdinalIgnoreCase`veya . |
| ınputparameter | stringMatchMsg | string | Dizeleri eşitse ayarlanacak ilk değer. |
| ınputparameter | stringMatchMsgCode | string | Dizeleri eşitse ayarlanacak ikinci değer. |
| ÇıktılarTalep | outputClaim1 | string | Dizeleri eşitse, bu çıktı talebi `stringMatchMsg` giriş parametresi değerini içerir. |
| ÇıktılarTalep | outputClaim2 | string | Dizeleri eşitse, bu çıktı talebi `stringMatchMsgCode` giriş parametresi değerini içerir. |
| ÇıktılarTalep | stringCompareResultClaim | boole | Karşılaştırma sonucu olarak `true` veya `false` temel alınacak sonuç çıktısı talep türünü karşılaştırın. |

Bu talep dönüşümlerini, bir talebin belirttiğiniz değere eşit olup olmadığını denetlemek için kullanabilirsiniz. Örneğin, aşağıdaki talepler dönüşüm **koşullarıOfUseConsentVersion** talebinin değeri eşitse `v1`denetler . Evet ise, değeri `v2`' ye değdirin.

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim**: v1
- Giriş parametreleri:
    - **matchTo**: V1
    - **stringKarşılaştırma**: ordinalIgnoreCase
    - **stringMatchMsg**: B2C_V1_90005
    - **stringMatchMsgCode**: TOS v2'ye yükseltildi
- Çıktı talepleri:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: TOS v2'ye yükseltildi
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

Bir dize talebi `matchTo` ve giriş parametresi eşit olup olmadığını denetler `outputClaimIfMatched` ve giriş parametresinde bulunan değerle çıktı taleplerini `true` ve `false` karşılaştırma sonucu olarak veya temel alınacak sonuç çıktısı iddiasını karşılaştırın.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | iddiaToMatch | string | Karşılaştırılacak olan talep türü. |
| ınputparameter | matchTo | string | InputClaim ile karşılaştırıldığında dize. |
| ınputparameter | Stringcomparison | string | Olası `Ordinal` değerler: `OrdinalIgnoreCase`veya . |
| ınputparameter | outputClaimIfMatched | string | Dizeleri eşitse ayarlanacak değer. |
| ÇıktılarTalep | outputClaim | string | Dizeleri eşitse, bu çıktı talebi `outputClaimIfMatched` giriş parametresi değerini içerir. Ya da ipler uyuşmuyorsa, null. |
| ÇıktılarTalep | stringCompareResultClaim | boole | Karşılaştırma sonucu olarak `true` veya `false` temel alınacak sonuç çıktısı talep türünü karşılaştırın. |

Örneğin, **yaş Grubu** talebinin değeri `Minor`. Evet ise, değeri `B2C_V1_90001`' ye döndürün.

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **claimToMatch**: Minor
- Giriş parametreleri:
    - **matchTo**: Küçük
    - **stringKarşılaştırma**: ordinalIgnoreCase
    - **outputClaimIfMatched**: B2C_V1_90001
- Çıktı talepleri:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: doğru


## <a name="stringcontains"></a>StringContains

Giriş talebi içinde belirtilen bir alt dize oluşup oluşmadığını belirleyin. Sonuç, değeri `true` veya `false`. `true`değer parametresi bu dize içinde `false`oluşursa, aksi takdirde, .

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | string | Aranacak talep türü. |
|ınputparameter|içerir|string|Aranacak değer.|
|ınputparameter|Ignorecase|string|Bu karşılaştırmanın, karşılaştırıldığında dize durumunu yok sayması gerekip gerekmediğini belirtir.|
| ÇıktılarTalep | outputClaim | string | Bu Talep Dönüşümünden sonra üretilen ClaimType çağrıldı. Substring giriş iddiası içinde oluşursa bir boolean göstergesi. |

Bir dize talep türü bir alt dize içeriyorsa denetlemek için bu talep ler dönüşüm kullanın. Aşağıdaki örnek, `roles` dize talep türünün **yönetici**değerini bulunup içermediğini denetler.

```XML
<ClaimsTransformation Id="CheckIsAdmin" TransformationMethod="StringContains">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="inputClaim"/>
  </InputClaims>
  <InputParameters>
    <InputParameter  Id="contains" DataType="string" Value="admin"/>
    <InputParameter  Id="ignoreCase" DataType="string" Value="true"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isAdmin" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim**: "Yönetici, Onaylayıcı, Editör"
- Giriş parametreleri:
    - **içerir:**"admin,"
    - **ignoreCase**: true
- Çıktı talepleri:
    - **outputClaim**: true

## <a name="stringsubstring"></a>StringSubstring

Belirtilen konumdaki karakterden başlayarak bir dize talep türünün bölümlerini ayıklar ve belirtilen karakter sayısını döndürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | string | Dize içeren talep türü. |
| ınputparameter | Startındex | int | Bu örnekte bir alt dizenin sıfır tabanlı başlangıç karakteri konumu. |
| ınputparameter | length | int | Alt dizedeki karakter sayısı. |
| ÇıktılarTalep | outputClaim | boole | Bu örnekte startIndex'te başlayan uzunluk alt dizesine eşdeğer bir dize veya startIndex bu örneğin uzunluğuna eşitse ve uzunluk sıfırsa Boş. |

Örneğin, telefon numarası ülke önekini alın.


```XML
<ClaimsTransformation Id="GetPhonePrefix" TransformationMethod="StringSubstring">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="inputClaim" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="startIndex" DataType="int" Value="0" />
  <InputParameter Id="length" DataType="int" Value="2" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="phonePrefix" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim**: "+1644114520"
- Giriş parametreleri:
    - **startIndex**: 0
    - **uzunluk**: 2
- Çıktı talepleri:
    - **outputClaim**: "+1"

## <a name="stringreplace"></a>StringReplace

Belirli bir değer için bir talep türü dizesini arar ve geçerli dizedeki belirli bir dizedeki tüm oluşumların başka bir belirtilen dizeyle değiştirildiği yeni bir talep türü dizesini döndürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | string | Dize içeren talep türü. |
| ınputparameter | Oldvalue | string | Aranacak dize. |
| ınputparameter | Newvalue | string | Tüm oluşumları değiştirmek için dize`oldValue` |
| ÇıktılarTalep | outputClaim | boole | OldValue'ın tüm örneklerinin newValue ile değiştirilmesi dışında geçerli dizeyle eşdeğer bir dize. Geçerli örnekte oldValue bulunmazsa, yöntem geçerli örneği değiştirmeden döndürür. |

Örneğin, `-` karakterleri kaldırarak bir telefon numarasını normale


```XML
<ClaimsTransformation Id="NormalizePhoneNumber" TransformationMethod="StringReplace">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="inputClaim" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="oldValue" DataType="string" Value="-" />
  <InputParameter Id="newValue" DataType="string" Value="" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="phoneNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Örnek

- Giriş talepleri:
    - **inputClaim**: "+164-411-452-054"
- Giriş parametreleri:
    - **oldValue**: "-"
    - **uzunluk**: ""
- Çıktı talepleri:
    - **outputClaim**: "+164411452054"

## <a name="stringjoin"></a>StringJoin

Her öğe veya üye arasında belirtilen ayırıcıyı kullanarak, belirtilen bir dize koleksiyonu talep türünün öğelerini birleştirir.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | Stringcollection | Birleştirmek için dizeleri içeren bir koleksiyon. |
| ınputparameter | sınırlayıcı | string | Virgül gibi ayırıcı olarak kullanılacak `,`dize. |
| ÇıktılarTalep | outputClaim | string | `inputClaim` Giriş parametresi ile `delimiter` sınırlandırılmış dize koleksiyonunun üyelerinden oluşan bir dize. |

Aşağıdaki örnek, kullanıcı rollerinin bir dize koleksiyonunu alır ve virgülde sınırlayıcı dizedönüştürür. Bir dize koleksiyonunu Azure AD kullanıcı hesabında depolamak için bu yöntemi kullanabilirsiniz. Daha sonra, dizinden hesabı okuduğunuzda, `StringSplit` virgül delimiter dizesini dize koleksiyonuna dönüştürmek için kullanın.

```XML
<ClaimsTransformation Id="ConvertRolesStringCollectionToCommaDelimiterString" TransformationMethod="StringJoin">
  <InputClaims>
   <InputClaim ClaimTypeReferenceId="roles" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter DataType="string" Id="delimiter" Value="," />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="rolesCommaDelimiterConverted" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim**: [ "Yönetici", "Yazar", "Okuyucu" ]
- Giriş parametreleri:
  - **delimiter**: ","
- Çıktı talepleri:
  - **outputClaim**: "Yönetici,Yazar,Okuyucu"


## <a name="stringsplit"></a>StringSplit

Bu örnekte, belirtilen dize öğeleriyle sınırlandırılmış alt dizeleri içeren bir dize dizisini döndürür.

| Öğe | DönüşümTalep Türü | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| Girişİddia | inputClaim | string | Bölünecek alt dizeleri içeren bir dize talep türü. |
| ınputparameter | sınırlayıcı | string | Virgül gibi ayırıcı olarak kullanılacak `,`dize. |
| ÇıktılarTalep | outputClaim | Stringcollection | Bu dizedeki alt dizeleri içeren ve giriş parametresi ile `delimiter` sınırlandırılmış bir dize koleksiyonu. |

Aşağıdaki örnek, kullanıcı rollerinden oluşan virgül delimiter dizesini alır ve bir dize koleksiyonuna dönüştürür.

```XML
<ClaimsTransformation Id="ConvertRolesToStringCollection" TransformationMethod="StringSplit">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="rolesCommaDelimiter" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
  <InputParameter DataType="string" Id="delimiter" Value="," />
    </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="roles" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Örnek

- Giriş talepleri:
  - **inputClaim**: "Yönetici, Yazar,Okuyucu"
- Giriş parametreleri:
  - **delimiter**: ","
- Çıktı talepleri:
  - **outputClaim**: [ "Yönetici", "Yazar", "Okuyucu" ]

## <a name="string-claim-transformations-expressions"></a>String claim ifadeleri dönüşümleri
Azure AD B2C özel ilkelerindeki talep dönüşümleri ifadeleri, kiracı kimliği ve teknik profil kimliği hakkında bağlam bilgileri sağlar.

  | İfadeler | Açıklama | Örnek |
 | ----- | ----------- | --------|
 | `{TechnicalProfileId}` | Teknik profilId adı. | Facebook-OAUTH |
 | `{RelyingPartyTenantId}` | Güvenilen parti ilkesinin kiracı kimliği. | your-tenant.onmicrosoft.com |
 | `{TrustFrameworkTenantId}` | Güven çerçevesinin kiracı kimliği. | your-tenant.onmicrosoft.com |
