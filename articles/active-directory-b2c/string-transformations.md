---
title: Özel ilkeler için dize talep dönüştürme örnekleri
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C Identity Experience Framework (ıEF) şeması için dize talep dönüştürme örnekleri.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 678385d9ed16a9821fc61be476e7eb9eaf6fd4f1
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78183712"
---
# <a name="string-claims-transformations"></a>Dize talep dönüştürmeleri

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory B2C (Azure AD B2C) öğesinde kimlik deneyimi çerçevesi şemasının dize talep dönüştürmelerinin kullanılmasına yönelik örnekler sağlanmaktadır. Daha fazla bilgi için bkz. [Claimstransformations](claimstransformations.md).

## <a name="assertstringclaimsareequal"></a>Assertstringclaimsareeşittir

İki talebi karşılaştırın ve belirtilen karşılaştırma inputClaim1, inputClaim2 ve stringComparison öğesine göre eşit değilse bir özel durum oluşturun.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | inputClaim1 | string | Karşılaştırılacak ilk talep türü. |
| ınputclaim | inputClaim2 | string | Karşılaştırılan ikinci talebin türü. |
| InputParameter | stringComparison | string | dize karşılaştırma, değerlerden biri: Ordinal, OrdinalIgnoreCase. |

**Assertstringclaimsareeþitclaim** dönüştürmesi, her zaman [otomatik olarak onaylanan bir teknik profille](self-asserted-technical-profile.md)veya bir [displayconrtol](display-controls.md)tarafından çağrılan bir [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. Otomatik olarak onaylanan bir teknik profilin `UserMessageIfClaimsTransformationStringsAreNotEqual` meta verileri, kullanıcıya sunulan hata iletisini denetler.


![Assertstringclaimsareeşittir yürütme](./media/string-transformations/assert-execution.png)

Bu talep dönüşümünü, iki ClaimTypes değerinin aynı değere sahip olduğundan emin olmak için kullanabilirsiniz. Aksi takdirde, bir hata iletisi oluşturulur. Aşağıdaki örnek, **Strongauthenticationemapostaadresi** ClaimType 'ın **e-posta** ClaimType değerine eşit olduğunu denetler. Aksi takdirde bir hata iletisi oluşturulur.

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


**Oturum açma etkileşimli** doğrulama teknik profili **Assertemaılandstrongauthenticationemaıladdressareeþitclaim** dönüşümünü çağırır.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Otomatik olarak onaylanan teknik profil, doğrulama **oturum açma-etkileşimsiz** teknik profilini çağırır.

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
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- Giriş parametreleri:
  - **StringComparison**: OrdinalIgnoreCase
- Sonuç: hata oluştu

## <a name="changecase"></a>Değişiklik durumu

, İşlecine bağlı olarak, belirtilen talebin durumunu küçük veya büyük harf olarak değiştirir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | inputClaim1 | string | Değiştirilecek ClaimType. |
| InputParameter | toCase | string | Şu değerlerden biri: `LOWER` veya `UPPER`. |
| OutputClaim | OutputClaim | string | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. |

Herhangi bir dize ClaimType 'ı küçük veya büyük harfle değiştirmek için bu talep dönüşümünü kullanın.

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
  - **e-posta**: SomeOne@contoso.com
- Giriş parametreleri:
    - **Tocase**: daha düşük
- Çıkış talepleri:
  - **e-posta**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim

Dönüşümde belirtilen giriş parametresinden bir dize talebi oluşturur.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
|----- | ----------------------- | --------- | ----- |
| InputParameter | değer | string | Ayarlanacak dize. Bu giriş parametresi [dize talep dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler. |
| OutputClaim | createdClaim | string | Bu talep dönüştürmesinin ardından üretilen ClaimType, giriş parametresinde belirtilen değerle çağırılır. |

Bir dize ClaimType değeri ayarlamak için bu talep dönüşümünü kullanın.

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
    - **değer**: contoso hizmet koşulları...
- Çıkış talepleri:
    - **Createdclaim**: TOS ClaimType "contoso hizmet koşulları..." içerir deeri.

## <a name="compareclaims"></a>CompareClaims

Tek bir dize talebinin diğerine eşit olup olmadığını belirleme. Sonuç, `true` veya `false`değerine sahip yeni bir Boole ClaimType değeridir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | inputClaim1 | string | Karşılaştırılacak ilk talep türü. |
| ınputclaim | inputClaim2 | string | Karşılaştırılacak ikinci talep türü. |
| InputParameter | operator | string | Olası değerler: `EQUAL` veya `NOT EQUAL`. |
| InputParameter | ignoreCase | boole | Bu karşılaştırmanın karşılaştırılan dizelerin durumunu yoksayıp saymayacağını belirtir. |
| OutputClaim | OutputClaim | boole | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. |

Bir talebin başka bir talebe eşit olup olmadığını denetlemek için bu talep dönüşümünü kullanın. Örneğin, aşağıdaki talep dönüştürmesi, **e-posta** talebinin değerinin **doğrulanmış. email** talebine eşit olup olmadığını denetler.

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
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- Giriş parametreleri:
    - **işleç**: eşit değil
    - **IgnoreCase**: true
- Çıkış talepleri:
    - **Outputclaim**: true

## <a name="compareclaimtovalue"></a>CompareClaimToValue

Bir talep değerinin giriş parametresi değerine eşit olup olmadığını belirler.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | inputClaim1 | string | Karşılaştırılan talep türü. |
| InputParameter | operator | string | Olası değerler: `EQUAL` veya `NOT EQUAL`. |
| InputParameter | compareTo | string | dize karşılaştırma, değerlerden biri: Ordinal, OrdinalIgnoreCase. |
| InputParameter | ignoreCase | boole | Bu karşılaştırmanın karşılaştırılan dizelerin durumunu yoksayıp saymayacağını belirtir. |
| OutputClaim | OutputClaim | boole | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. |

Bu talep dönüşümünü, bir talebin belirttiğiniz değere eşit olup olmadığını denetlemek için kullanabilirsiniz. Örneğin, aşağıdaki talep dönüştürmesi, **Termsofuseconsentversion** talebinin değerinin `v1`eşit olup olmadığını denetler.

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
    - **CompareTo**: v1
    - **işleç**: eşittir
    - **IgnoreCase**: true
- Çıkış talepleri:
    - **Outputclaim**: true

## <a name="createrandomstring"></a>CreateRandomString

Rastgele sayı oluşturucuyu kullanarak rastgele bir dize oluşturur. Rastgele sayı Oluşturucu `integer`türünde ise, isteğe bağlı olarak bir çekirdek parametresi ve en büyük sayı sağlanmış olabilir. İsteğe bağlı bir dize biçim parametresi, çıktının kullanılarak biçimlendirilmesini sağlar ve isteğe bağlı bir Base64 parametresi çıktının Base64 kodlamalı Rasgelegeneratortype [GUID, Integer] outputClaim (dize) olup olmadığını belirtir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | Rasgelegeneratortype | string | Oluşturulacak rastgele değeri, `GUID` (genel benzersiz KIMLIK) veya `INTEGER` (bir sayı) belirtir. |
| InputParameter | stringFormat | string | Seçim Rastgele değeri biçimlendirin. |
| InputParameter | biçiminde | boole | Seçim Rastgele değeri Base64 olarak dönüştürür. Dize biçimi uygulanmışsa dize biçiminden sonraki değer Base64 olarak kodlanır. |
| InputParameter | maximumNumber | int | Seçim Yalnızca `INTEGER` Rasgelegeneratortype. En yüksek sayıyı belirtin. |
| InputParameter | çekirdek  | int | Seçim Yalnızca `INTEGER` Rasgelegeneratortype. Rastgele değer için çekirdek belirtin. Note: aynı çekirdek aynı rastgele sayı dizisini verir. |
| OutputClaim | OutputClaim | string | Bu talep dönüşümünde oluşturulacak ClaimTypes. Rastgele değer. |

Aşağıdaki örnek, genel benzersiz bir KIMLIK üretir. Bu talep dönüştürmesi, rastgele UPN (Kullanıcı asıl adı) oluşturmak için kullanılır.

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
    - **Randomgeneratortype**: GUID
- Çıkış talepleri:
    - **Outputclaim**: bc8bedd2-aaa3-411E-bdee-2f1810b73dfc

Aşağıdaki örnek 0 ile 1000 arasında bir tamsayı rastgele değeri üretir. Değer, {Random Value} OTP_ olarak biçimlendirilir.

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
    - **Randomgeneratortype**: tamsayı
    - **Maximumnumber**: 1000
    - **StringFormat**: OTP_{0}
    - **Base64**: yanlış
- Çıkış talepleri:
    - **Outputclaim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

Bir talebi, belirtilen biçim dizesine göre biçimlendirin. Bu dönüşüm C# `String.Format` yöntemini kullanır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim |string |Dize biçimi {0} parametresi olarak davranan ClaimType. |
| InputParameter | stringFormat | string | {0} parametresi dahil dize biçimi. Bu giriş parametresi [dize talep dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler.  |
| OutputClaim | OutputClaim | string | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. |

Tek parametreli {0}dize biçimlendirmek için bu talep dönüşümünü kullanın. Aşağıdaki örnek bir **userPrincipalName**oluşturur. `Facebook-OAUTH` gibi tüm sosyal kimlik sağlayıcısı teknik profilleri, bir **userPrincipalName**oluşturmak Için **createuserprincipalname** ' i çağırır.

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
    - **ınputclaim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- Giriş parametreleri:
    - **StringFormat**: cpim_{0}@ {RelyingPartyTenantId}
- Çıkış talepleri:
  - **Outputclaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>Formatstringmultipleclaim

Belirtilen biçim dizesine göre iki talebi biçimlendirin. Bu dönüşüm C# `String.Format` yöntemini kullanır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim |string | Dize biçimi {0} parametresi olarak davranan ClaimType. |
| ınputclaim | ınputclaim | string | Dize biçimi {1} parametresi olarak davranan ClaimType. |
| InputParameter | stringFormat | string | {0} ve {1} parametreleri de dahil olmak üzere dize biçimi. Bu giriş parametresi [dize talep dönüştürme ifadelerini](string-transformations.md#string-claim-transformations-expressions)destekler.   |
| OutputClaim | OutputClaim | string | Bu talep dönüştürmesinin ardından üretilen ClaimType çağırılır. |

Bu talep dönüşümünü, iki parametreli bir dize biçimlendirmek için kullanın, {0} ve {1}. Aşağıdaki örnek, belirtilen biçimde bir **DisplayName** oluşturuyor:

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
    - **inputClaim1**: ali
    - **inputClaim2**: Fernando
- Giriş parametreleri:
    - **StringFormat**: {0} {1}
- Çıkış talepleri:
    - **Outputclaim**: ali Fernando

## <a name="getlocalizedstringstransformation"></a>Getlocalizedstringstranssize

Yerelleştirilmiş dizeleri talebe kopyalar.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | Yerelleştirilmiş dizenin adı | string | Bu talep dönüştürme işlemi yapıldıktan sonra üretilen talep türlerinin listesi. |

Getlocalizedstringstrans, talep dönüşümünü kullanmak için:

1. Bir [Yerelleştirme dizesi](localization.md) tanımlayın ve [kendini onaylanan teknik profille](self-asserted-technical-profile.md)ilişkilendirin.
1. `LocalizedString` öğesinin `ElementType` `GetLocalizedStringsTransformationClaimType`olarak ayarlanmalıdır.
1. `StringId`, tanımladığınız benzersiz bir tanımlayıcıdır ve daha sonra talep dönüşümünüzün içinde kullanın.
1. Talep dönüşümünde, yerelleştirilmiş dizeyle ayarlanacak taleplerin listesini belirtin. `ClaimTypeReferenceId`, ilkedeki ClaimsSchema bölümünde zaten tanımlanmış olan bir ClaimType başvurusu. `TransformationClaimType`, `LocalizedString` öğesinin `StringId` tanımlanan yerelleştirilmiş dizenin adıdır.
1. [Kendi kendine onaylanan bir teknik profilde](self-asserted-technical-profile.md)veya bir [görüntüleme denetim](display-controls.md) girişi veya çıkış talebi dönüştürmesi ' nde talep dönüşümünüze bir başvuru yapın.

![Getlocalizedstringstranssize](./media/string-transformations/get-localized-strings-transformation.png)

Aşağıdaki örnekte e-posta konusu, gövde, kod iletiniz ve e-postanın imzası yerelleştirilmiş dizelerdir. Bu talepler daha sonra özel e-posta doğrulama şablonu tarafından kullanılır.

Ingilizce (varsayılan) ve Ispanyolca için yerelleştirilmiş dizeleri tanımlayın.

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

Talep dönüştürmesi, `StringId` *Email_Subject*değeri ile *ilgili* talep türünün değerini ayarlar.

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

- Çıkış talepleri:
  - **Konu**: contoso hesabı e-posta doğrulama kodu
  - **ileti**: hesabınız doğrulanırken teşekkürler!
  - **Codebir giriş**: kodunuz
  - **imza**: saygılarımla


## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Talep **kısıtlama** koleksiyonundan bir öğe aranıyor.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | Mapfromclaım | string | **Kısıtlama** koleksiyonuyla **restrictionValueClaim** taleplerde aranacak metni içeren talep.  |
| OutputClaim | restrictionValueClaim | string | **Kısıtlama** toplamayı içeren talep. Talep dönüştürme çağrıldıktan sonra, bu talebin değeri seçili öğenin değerini içerir. |

Aşağıdaki örnek hata anahtarına bağlı olarak hata iletisi açıklamasını arar. **Responsemsg** talebi, son kullanıcıya sunmak veya bağlı olan tarafa gönderilmek üzere bir hata iletileri koleksiyonu içerir.

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
Talep dönüştürmesi öğenin metnini arar ve değerini döndürür. Kısıtlama `<LocalizedCollection>`kullanılarak yerelleştirilir, talep dönüştürmesi yerelleştirilmiş değeri döndürür.

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
    - **Mapfromclaım**: B2C_V1_90001
- Çıkış talepleri:
    - **restrictionValueClaim**: küçük olduğunuz için oturum açılamıyor.

## <a name="lookupvalue"></a>LookupValue

Bir değer listesinden, başka bir talebin değerine göre bir talep değeri arama.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputparameterıd | string | Arama değerini içeren talep |
| InputParameter | |string | InputParameters koleksiyonu. |
| InputParameter | errorOnFailedLookup | boole | Eşleşen arama olmadığında bir hatanın döndürülüp döndürülmeyeceğini denetleme. |
| OutputClaim | ınputparameterıd | string | Bu talep dönüşümünde oluşturulacak ClaimTypes. Eşleşen `Id`değeri. |

Aşağıdaki örnek, InputParameters koleksiyonlarından birinde etki alanı adını arar. Talep dönüştürmesi, Tanımlayıcıdaki etki alanı adını arar ve değerini (bir uygulama KIMLIĞI) döndürür.

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
    - **ınputparameterıd**: test.com
- Giriş parametreleri:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **Microsoft.com**: 0213308f-17cb-4398-b97e-01dav7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **Erroronfailedlookup**: false
- Çıkış talepleri:
    - **Outputclaim**: c7026f88-4299-4cdb-965d-3f166464b8a9

`errorOnFailedLookup` giriş parametresi `true`olarak ayarlandığında, **lookupvalue** talep dönüştürmesi her zaman [otomatik olarak onaylanan bir teknik profil](self-asserted-technical-profile.md)veya bir [displayconrtol](display-controls.md)tarafından çağrılan [doğrulama teknik profilinden](validation-technical-profile.md) yürütülür. Otomatik olarak onaylanan bir teknik profilin `LookupNotFound` meta verileri, kullanıcıya sunulan hata iletisini denetler.

![Assertstringclaimsareeşittir yürütme](./media/string-transformations/assert-execution.png)

Aşağıdaki örnek, InputParameters koleksiyonlarından birinde etki alanı adını arar. Talep dönüştürmesi, Tanımlayıcıdaki etki alanı adını arar ve değerini (bir uygulama KIMLIĞI) döndürür ya da bir hata mesajı oluşturur.

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
    - **ınputparameterıd**: Live.com
- Giriş parametreleri:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **Microsoft.com**: 0213308f-17cb-4398-b97e-01dav7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **Erroronfailedlookup**: true
- Hata:
    - Giriş parametresi kimlikleri listesinde giriş talep değeri için eşleşme bulunamadı ve errorOnFailedLookup doğru.


## <a name="nullclaim"></a>NullClaim

Belirli bir talebin değerini temizleyin.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | claim_to_null | string | Talebin değeri NULL olarak ayarlandı. |

Bu talep dönüşümünü, oturum tanımlama bilgisinin daha küçük olması için talep özelliği çantasından gereksiz verileri kaldırmak için kullanın. Aşağıdaki örnek `TermsOfService` talep türünün değerini kaldırır.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Giriş talepleri:
    - **Outputclaim**: contoso uygulamasına hoş geldiniz. Bu Web sitesine gözatıp kullanmaya devam ederseniz, kabul etmiş ve aşağıdaki hüküm ve koşullara göre bağlanmaları gerekir...
- Çıkış talepleri:
    - **Outputclaim**: null

## <a name="parsedomain"></a>ParseDomain

Bir e-posta adresinin etki alanı kısmını alır.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | emailAddress | string | E-posta adresini içeren ClaimType. |
| OutputClaim | alanını | string | Bu talep dönüştürme işleminden sonra üretilen ClaimType, etki alanı. |

Kullanıcının @ simgesinden sonra etki alanı adını ayrıştırmak için bu talep dönüşümünü kullanın. Aşağıdaki talep dönüşümünde, bir **e-posta** talebinde etki alanı adının nasıl ayrıştırılacağını gösterilmektedir.

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
  - **Emaadresi**: joe@outlook.com
- Çıkış talepleri:
    - **etki alanı**: Outlook.com

## <a name="setclaimsifregexmatch"></a>Setclaimsıfregexmatch

Bir dize talebi `claimToMatch` ve `matchTo` giriş parametresinin eşit olup olmadığını denetler ve `outputClaimIfMatched` giriş parametresinde bulunan değerle birlikte çıkış taleplerini, karşılaştırma sonucuna göre `true` veya `false` olarak ayarlanacak şekilde karşılaştırın.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | claimToMatch | string | Karşılaştırılacak talep türü. |
| InputParameter | matchTo | string | Eşleştirilecek normal ifade. |
| InputParameter | Outputclaimifeşleşti | string | Dizeler eşitse ayarlanacak değer. |
| OutputClaim | OutputClaim | string | Normal ifade eşleşiyorsa, bu çıkış talebi `outputClaimIfMatched` giriş parametresinin değerini içerir. Ya da eşleşme yoksa null. |
| OutputClaim | regexCompareResultClaim | boole | `true` olarak ayarlanacak veya eşleştirme sonucuna göre `false` olarak ayarlanacak olan normal ifade, sonuç çıkış talep türü ile eşleşir. |

Örneğin, telefon numarası normal ifade düzenine göre, girilen telefon numarasının geçerli olup olmadığını denetler.

```XML
<ClaimsTransformation Id="SetIsPhoneRegex" TransformationMethod="setClaimsIfRegexMatch">
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

### <a name="example"></a>Örnek

- Giriş talepleri:
    - **claimToMatch**: "64854114520"
- Giriş parametreleri:
    - **eşleşme**: "^ [0-9]{4,16}$"
    - **Outputclaimifeşleşti**: "ısphone"
- Çıkış talepleri:
    - **Outputclaim**: "ısphone"
    - **regexCompareResultClaim**: true

## <a name="setclaimsifstringsareequal"></a>Setclaimsıfstringsareeşittir

Bir dize talebi ve `matchTo` giriş parametresinin eşit olup olmadığını denetler ve `stringMatchMsg` ve `stringMatchMsgCode` giriş parametrelerinde bulunan değer ile birlikte çıkış taleplerini, karşılaştırma sonucuna göre `true` veya `false` olarak ayarlanacak şekilde karşılaştırın.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | string | Karşılaştırılacak talep türü. |
| InputParameter | matchTo | string | `inputClaim`Karşılaştırılacak dize. |
| InputParameter | stringComparison | string | Olası değerler: `Ordinal` veya `OrdinalIgnoreCase`. |
| InputParameter | stringMatchMsg | string | Dizeler eşitse ayarlanacak ilk değer. |
| InputParameter | stringMatchMsgCode | string | Dizeler eşitse ayarlanacak ikinci değer. |
| OutputClaim | outputClaim1 | string | Dizeler eşitse, bu çıkış talebi `stringMatchMsg` giriş parametresinin değerini içerir. |
| OutputClaim | outputClaim2 | string | Dizeler eşitse, bu çıkış talebi `stringMatchMsgCode` giriş parametresinin değerini içerir. |
| OutputClaim | stringCompareResultClaim | boole | Karşılaştırma sonucuna göre `true` veya `false` olarak ayarlanacak karşılaştırma sonucu çıkış talep türü. |

Bu talep dönüşümünü, bir talebin belirttiğiniz değere eşit olup olmadığını denetlemek için kullanabilirsiniz. Örneğin, aşağıdaki talep dönüştürmesi, **Termsofuseconsentversion** talebinin değerinin `v1`eşit olup olmadığını denetler. Yanıt Evet ise, değeri `v2`olarak değiştirin.

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
    - **ınputclaim**: v1
- Giriş parametreleri:
    - **eşleşme**: v1
    - **StringComparison**: OrdinalIgnoreCase
    - **Stringmatchmsg**: B2C_V1_90005
    - **Stringmatchmsgcode**: TOS v2 'ye yükseltildi
- Çıkış talepleri:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: TOS v2 'ye yükseltilir
    - **stringCompareResultClaim**: true

## <a name="setclaimsifstringsmatch"></a>Setclaimsıfstringsmatch

Bir dize talebi ve `matchTo` giriş parametresinin eşit olup olmadığını denetler ve `outputClaimIfMatched` giriş parametresinde bulunan değer ile birlikte çıkış taleplerini, karşılaştırma sonucuna göre `true` veya `false` olarak ayarlanacak şekilde karşılaştırın.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | claimToMatch | string | Karşılaştırılacak talep türü. |
| InputParameter | matchTo | string | Inputclaim ile Karşılaştırılacak dize. |
| InputParameter | stringComparison | string | Olası değerler: `Ordinal` veya `OrdinalIgnoreCase`. |
| InputParameter | Outputclaimifeşleşti | string | Dizeler eşitse ayarlanacak değer. |
| OutputClaim | OutputClaim | string | Dizeler eşitse, bu çıkış talebi `outputClaimIfMatched` giriş parametresinin değerini içerir. Ya da dizeler eşleşmiyorsa null. |
| OutputClaim | stringCompareResultClaim | boole | Karşılaştırma sonucuna göre `true` veya `false` olarak ayarlanacak karşılaştırma sonucu çıkış talep türü. |

Örneğin, aşağıdaki talep dönüştürmesi, **Agegroup** talebinin değerinin `Minor`eşit olup olmadığını denetler. Yanıt Evet ise `B2C_V1_90001`için değeri döndürün.

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
    - **claimToMatch**: küçük
- Giriş parametreleri:
    - **eşleşme**: küçük
    - **StringComparison**: OrdinalIgnoreCase
    - **Outputclaimifeşleşti**: B2C_V1_90001
- Çıkış talepleri:
    - **isMinorResponseCode**: B2C_V1_90001
    - **ısminor**: doğru


## <a name="stringcontains"></a>StringContains

Belirtilen bir alt dizenin giriş talebi içinde oluşup oluşmadığını belirleme. Sonuç, `true` veya `false`değerine sahip yeni bir Boole ClaimType değeridir. değer parametresi bu dize içinde gerçekleşirse `true`, aksi durumda `false`.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | string | Arama yapılacak talep türü. |
|InputParameter|içerir|string|Aranacak değer.|
|InputParameter|ignoreCase|string|Bu karşılaştırmanın karşılaştırılan dizenin durumunu yoksayıp saymayacağını belirtir.|
| OutputClaim | OutputClaim | string | Bu Claimstransbir şekilde üretilen ClaimType çağırılır. Giriş talebi içinde alt dize gerçekleşirse Boole göstergesi. |

Bir dize talep türünün bir alt dize içerip içermesinin olup olmadığını denetlemek için bu talep dönüşümünü kullanın. Aşağıdaki örnek, `roles` dize talep türünün **yönetici**değerini içerip içermediğini denetler.

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
    - **ınputclaim**: "Yönetici, onaylayan, düzenleyici"
- Giriş parametreleri:
    - **şunu içerir**: "Yönetici"
    - **IgnoreCase**: true
- Çıkış talepleri:
    - **Outputclaim**: true

## <a name="stringsubstring"></a>StringSubstring

Bir dize talep türünün parçalarını, belirtilen konumdaki karakterden başlayarak ayıklar ve belirtilen sayıda karakteri döndürür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | string | Dizeyi içeren talep türü. |
| InputParameter | startIndex | int | Bu örnekteki alt dizenin sıfır tabanlı başlangıç karakter konumu. |
| InputParameter | {1&gt;length&lt;1} | int | Alt dizeden karakter sayısı. |
| OutputClaim | OutputClaim | boole | Bu örnekte startIndex değerinde başlayan uzunluğunun alt dizesi ile eşdeğer veya startIndex, bu örneğin uzunluğuna eşitse ve length sıfır olduğunda boş olan bir dize. |

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
    - **ınputclaim**: "+ 1644114520"
- Giriş parametreleri:
    - **startIndex**: 0
    - **uzunluk**: 2
- Çıkış talepleri:
    - **Outputclaim**: "+ 1"

## <a name="stringreplace"></a>StringReplace

Belirtilen değer için bir talep türü dizesi arar ve geçerli dizedeki belirtilen dizenin tüm oluşumlarının belirtilen başka bir dizeyle değiştirildiği yeni bir talep türü dizesi döndürür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | string | Dizeyi içeren talep türü. |
| InputParameter | oldValue | string | Aranacak dize. |
| InputParameter | Değer | string | Tüm `oldValue` tekrarlarının yerini alacak dize |
| OutputClaim | OutputClaim | boole | Tüm oldValue örnekleri newValue ile değiştirilmeleri dışında, geçerli dize ile eşdeğer bir dize. OldValue geçerli örnekte bulunamazsa, yöntemi geçerli örneği değişmeden döndürür. |

Örneğin, `-` karakterleri kaldırarak bir telefon numarasını normalleştirin


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
    - **ınputclaim**: "+ 164-411-452-054"
- Giriş parametreleri:
    - **OldValue**: "-"
    - **uzunluk**: ""
- Çıkış talepleri:
    - **Outputclaim**: "+ 164411452054"

## <a name="stringjoin"></a>Stringjoın

Her öğe veya üye arasındaki belirtilen ayırıcıyı kullanarak belirtilen dize koleksiyonu talep türünün öğelerini birleştirir.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | stringCollection | Birleştirilecek dizeleri içeren bir koleksiyon. |
| InputParameter | ayırıcı | string | Virgül `,`gibi bir ayırıcı olarak kullanılacak dize. |
| OutputClaim | OutputClaim | string | `delimiter` giriş parametresiyle ayrılmış `inputClaim` dize koleksiyonunun üyelerinden oluşan bir dize. |

Aşağıdaki örnek, bir Kullanıcı rolleri dize koleksiyonunu alır ve bunu bir virgül sınırlayıcı dizesine dönüştürür. Bu yöntemi, bir dize koleksiyonunu Azure AD Kullanıcı hesabında depolamak için kullanabilirsiniz. Daha sonra, dizinden hesabı okuduğunuzda, virgül sınırlayıcı dizesini dize koleksiyonuna geri dönüştürmek için `StringSplit` kullanın.

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
  - **ınputclaim**: ["admin", "author", "Reader"]
- Giriş parametreleri:
  - **sınırlayıcı**: ","
- Çıkış talepleri:
  - **Outputclaim**: "Yönetici, yazar, okuyucu"


## <a name="stringsplit"></a>StringSplit

Bu örnekte belirtilen bir dizenin öğeleriyle ayrılmış alt dizeleri içeren bir dize dizisi döndürür.

| Öğe | Dönüştürme Tionclaimtype | Veri Türü | Notlar |
| ---- | ----------------------- | --------- | ----- |
| ınputclaim | ınputclaim | string | Bölünecek alt dizeleri içeren bir dize talep türü. |
| InputParameter | ayırıcı | string | Virgül `,`gibi bir ayırıcı olarak kullanılacak dize. |
| OutputClaim | OutputClaim | stringCollection | Öğeleri bu dizedeki `delimiter` giriş parametresiyle ayrılmış alt dizeleri içeren bir dize koleksiyonu. |

Aşağıdaki örnek, Kullanıcı rollerinin virgül sınırlayıcısı dizesini alır ve bunu bir dize koleksiyonuna dönüştürür.

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
  - **ınputclaim**: "Yönetici, yazar, okuyucu"
- Giriş parametreleri:
  - **sınırlayıcı**: ","
- Çıkış talepleri:
  - **Outputclaim**: ["admin", "author", "Reader"]

## <a name="string-claim-transformations-expressions"></a>Dize talep dönüştürmeleri ifadeleri
Azure AD B2C özel ilkelerindeki talep dönüştürmeleri ifadeleri, kiracı KIMLIĞI ve teknik profil KIMLIĞIYLE ilgili bağlam bilgilerini sağlar.

  | Expression | Açıklama | Örnek |
 | ----- | ----------- | --------|
 | `{TechnicalProfileId}` | Teknik ProfileId adı. | Facebook-OAUTH |
 | `{RelyingPartyTenantId}` | Bağlı olan taraf ilkesinin kiracı KIMLIĞI. | your-tenant.onmicrosoft.com |
 | `{TrustFrameworkTenantId}` | Güven çerçevesinin kiracı KIMLIĞI. | your-tenant.onmicrosoft.com |
