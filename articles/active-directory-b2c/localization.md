---
title: Yerelleştirme - Azure Etkin Dizini B2C
description: Azure Etkin Dizin B2C'de özel bir ilkenin Yerelleştirme öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 94ff7ddda41f2df2634d927a7dbf8a5a0d4fc1d8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681409"
---
# <a name="localization"></a>Yerelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi, kullanıcı yolculukları için ilkedeki birden çok yerel alanı veya dili desteklemenize olanak tanır. İlkelerdeki yerelleştirme desteği şunları yapmanızı sağlar:

- Bir ilkede desteklenen dillerin açık listesini ayarlayın ve varsayılan bir dil seçin.
- Dile özel dizeleri ve koleksiyonları sağlayın.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

**Yerelleştirme** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Etkin | Hayır | Olası `true` değerler: `false`veya . |

**Yerelleştirme** öğesi aşağıdaki XML öğelerini içerir

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Desteklenen Diller | 1:n | Desteklenen diller listesi. |
| Yerelleştirilmiş Kaynaklar | 0:n | Yerelleştirilmiş kaynakların listesi. |

## <a name="supportedlanguages"></a>Desteklenen Diller

**Desteklenen Diller** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DefaultLanguage | Evet | Yerelleştirilmiş kaynaklar için varsayılan olarak kullanılacak dil. |
| Birleştirme Davranışı | Hayır | Bir üst politikada aynı tanımlayıcıya sahip herhangi bir ClaimType ile birleştirilen değerlerin numaralandırma değerleri. Temel ilkede belirtilen bir talebin üzerine yazarken bu özniteliği kullanın. Olası `Append`değerler: `Prepend`, `ReplaceAll`, veya . Değer, `Append` var olan veri toplamanın üst politikada belirtilen koleksiyonun sonuna eklendiğini belirtir. Değer, `Prepend` ana ilkede belirtilen koleksiyondan önce mevcut veri toplamanın eklenmesi gerektiğini belirtir. Değer, `ReplaceAll` geçerli ilkede tanımlanan verileri kullanarak, ana ilkede tanımlanan veri toplamanın yoksayılması gerektiğini belirtir. |

### <a name="supportedlanguages"></a>Desteklenen Diller

**Desteklenen Diller** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Desteklenen Dil | 1:n | RFC 5646 - Dilleri Tanımlama Etiketleri başına bir dil etiketine uygun içeriği görüntüler. |

## <a name="localizedresources"></a>Yerelleştirilmiş Kaynaklar

**Yerelleştirilmiş Kaynaklar** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yerelleştirilmiş kaynakları benzersiz olarak tanımlamak için kullanılan bir tanımlayıcı. |

**Yerelleştirilmiş Kaynaklar** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yerelleştirilmiş Koleksiyonlar | 0:n | Çeşitli kültürlerdeki tüm koleksiyonları tanımlar. Bir koleksiyon, çeşitli kültürler için farklı sayıda öğeye ve farklı dizeleri ne olabilir. Koleksiyonlara örnek olarak, talep türlerinde görünen sayısallaştırmalar verilebilir. Örneğin, bir ülke/bölge listesi açılır listede kullanıcıya gösterilir. |
| Yerelleştirilmiş Strings | 0:n | Koleksiyonlarda görünen dizeleri dışında, çeşitli kültürlerde tüm dizeleri tanımlar. |

### <a name="localizedcollections"></a>Yerelleştirilmiş Koleksiyonlar

**Yerelleştirilmiş Koleksiyonlar** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yerelleştirilmiş Koleksiyon | 1:n | Desteklenen diller listesi. |

#### <a name="localizedcollection"></a>Yerelleştirilmiş Koleksiyon

**LocalizedCollection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Elementtype | Evet | İlke dosyasında bir ClaimType öğesine veya kullanıcı arabirimi öğesine başvurur. |
| Elementıd | Evet | **ElementType** Bir ClaimType olarak ayarlanmışsa kullanılan ClaimsSchema bölümünde zaten tanımlanmış bir talep türüne başvuru içeren bir dize. |
| Targetcollection | Evet | Hedef toplama. |

**LocalizedCollection** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0:n | Kullanıcının, açılır bırakmadaki değer gibi kullanıcı arabirimindeki bir talep için seçmesi için kullanılabilir bir seçenek tanımlar. |

**Madde** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Metin | Evet | Bu seçenek için kullanıcı arabiriminde kullanıcıya gösterilmesi gereken kullanıcı dostu ekran dizesi. |
| Değer | Evet | Bu seçeneğin seçilmesiile ilişkili dize talep değeri. |
| SelectbyDefault | Hayır | UI'de bu seçeneğin varsayılan olarak seçilip seçilmemesi gerektiğini gösterir. Olası değerler: Doğru veya Yanlış. |

Aşağıdaki örnekte **Yerelleştirilmiş Koleksiyonlar** öğesinin kullanımı gösterilmektedir. Biri İngilizce, diğeri İspanyolca olmak üzere iki **LocalizedCollection** öğesi içerir. Her ikisi de İngilizce `Gender` ve İspanyolca öğelerin bir listesi ile **iddianın Kısıtlama** toplama ayarlayın.

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>Yerelleştirilmiş Strings

**LocalizedStrings** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yerelleştirilmiş String | 1:n | Yerelleştirilmiş bir dize. |

**LocalizedString** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Elementtype | Evet | Olası değerler: [ClaimsProvider](#claimsprovider), [ClaimType](#claimtype), [ErrorMessage](#errormessage), [GetLocalizedStringsTransformationClaimType](#getlocalizedstringstransformationclaimtype), [Yüklem](#predicate), [InputValidation](#inputvalidation), veya [UxElement](#uxelement).   | 
| Elementıd | Evet | **Eğer ElementType** `ClaimType`, `Predicate`, `InputValidation`, veya , bu öğe zaten ClaimsSchema bölümünde tanımlanan bir talep türüiçin bir başvuru içerir. |
| StringId | Evet | **ElementType** `ClaimType`ayarlanırsa, bu öğe bir talep türü bir öznitelik için bir başvuru içerir. Olası `DisplayName`değerler: `AdminHelpText`, `PatternHelpText`, veya . Değer, `DisplayName` talep görüntüleme adını ayarlamak için kullanılır. Değer, `AdminHelpText` talep kullanıcısının yardım metin adını ayarlamak için kullanılır. Değer, `PatternHelpText` talep deseni yardım metnini ayarlamak için kullanılır. **ElementType** `UxElement`ayarlanmışsa, bu öğe bir kullanıcı arabirimi öğesinin özniteliğiiçin bir başvuru içerir. **ElementType** `ErrorMessage`ayarlanmışsa, bu öğe bir hata iletisinin tanımlayıcısını belirtir. Tanımlayıcıların tam listesi için [Yerelleştirme dize kimliklerini](localization-string-ids.md) görün. `UxElement`|

## <a name="elementtype"></a>Elementtype

Bir talep türüne, talep dönüşümüne veya yerelleştirilecek ilkedeki kullanıcı arabirimi öğesine öğe türü başvurusu.

| Yerelleştirmek için öğe | Elementtype | Elementıd |StringId |
| --------- | -------- | ----------- |----------- |
| Kimlik sağlayıcı adı |`ClaimsProvider`| | ClaimsExchange öğesinin kimliği|
| Talep türü öznitelikleri|`ClaimType`|Talep türünün adı| Talebin yerelleştirilmesi özniteliği. Olası `AdminHelpText`değerler: `DisplayName` `PatternHelpText`, `UserHelpText`, , ve .|
|Hata iletisi|`ErrorMessage`||Hata iletisinin kimliği |
|Yerelleştirilmiş dizeleri taleplere kopyalar|`GetLocalizedStringsTra nsformationClaimType`||Çıktı talebinin adı|
|Yüklem kullanıcı iletisi|`Predicate`|Yüklemin adı| Yüklemin yerelleştirilmesi özniteliği. Olası değerler: `HelpText`.|
|Yüklem grubu kullanıcı iletisi|`InputValidation`|Yüklem öğesinin kimliği.|PredicateGroup öğesinin kimliği. Yüklem grubu, ElementId'de tanımlandığı gibi yüklem doğrulama öğesinin bir alt öğesi olmalıdır.|
|Kullanıcı arabirimi öğeleri |`UxElement` | | Yerelleştirilecek kullanıcı arabirimi öğesinin kimliği.|

## <a name="examples"></a>Örnekler

### <a name="claimsprovider"></a>İddia Sağlayıcı

ClaimsProvider değeri, talep sağlayıcılardan birinin görüntü adını yerelleştirmek için kullanılır. 

```xml
<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
    <ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAUTH" />
  </ClaimsExchanges>
</OrchestrationStep>

```

Aşağıdaki örnek, talep sağlayıcılarının görüntü adlarını nasıl yerelleştirileni gösterir.

```xml
<LocalizedString ElementType="ClaimsProvider" StringId="FacebookExchange">Facebook</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="GoogleExchange">Google</LocalizedString>
<LocalizedString ElementType="ClaimsProvider" StringId="LinkedInExchange">LinkedIn</LocalizedString>
```

### <a name="claimtype"></a>Claimtype

ClaimType değeri, talep özniteliklerinden birini yerelleştirmek için kullanılır. 

```xml
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

Aşağıdaki örnek, e-posta talebi türünün DisplayName, UserHelpText ve PatternHelpText özniteliklerinin nasıl yerelleştirilebildiğini gösterir.

```XML
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
<LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
```

### <a name="errormessage"></a>Hata İletisi

Hata İletisi değeri, sistem hata iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalAlreadyExists">You are already registered, please press the back button and sign in instead.</Item>
  </Metadata>
  ...
</TechnicalProfile>
```

Aşağıdaki örnek, UserMessageIfClaimsPrincipalAlreadyExists hata iletisinin nasıl yerelleştirilebildiğini gösterir.


```XML
<LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
```

### <a name="getlocalizedstringstransformationclaimtype"></a>GetLocalizedStringsTransformationClaimType

GetLocalizedStringsTransformationClaimType değeri, yerelleştirilmiş dizeleri taleplere kopyalamak için kullanılır. Daha fazla bilgi için [GetLocalizedStringsTransformation talepleri dönüşümüne](string-transformations.md#getlocalizedstringstransformation) bakın


```xml
<ClaimsTransformation Id="GetLocalizedStringsForEmail" TransformationMethod="GetLocalizedStringsTransformation">
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="subject" TransformationClaimType="email_subject" />
    <OutputClaim ClaimTypeReferenceId="message" TransformationClaimType="email_message" />
    <OutputClaim ClaimTypeReferenceId="codeIntro" TransformationClaimType="email_code" />
    <OutputClaim ClaimTypeReferenceId="signature" TransformationClaimType="email_signature" />
   </OutputClaims>
</ClaimsTransformation>
```

Aşağıdaki örnek, GetLocalizedStringsTransformation taleplerinin dönüşümünün çıktı taleplerini nasıl yerelleştirilengösterir.

```xml
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_subject">Contoso account email verification code</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_message">Thanks for verifying your account!</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_code">Your code is</LocalizedString>
<LocalizedString ElementType="GetLocalizedStringsTransformationClaimType" StringId="email_signature">Sincerely</LocalizedString>
```

### <a name="predicate"></a>Yüklemi

Yüklem değeri, [Yüklem](predicates.md) hata iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<Predicates>
  <Predicate Id="LengthRange" Method="IsLengthRange"  HelpText="The password must be between 6 and 64 characters.">
    <Parameters>
      <Parameter Id="Minimum">6</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>
  <Predicate Id="Uppercase" Method="IncludesCharacters" HelpText="an uppercase letter">
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Aşağıdaki örnekte, yüklemlerin nasıl yerelleştirilen yardımcı metin gösterilmektedir.

```xml
<LocalizedString ElementType="Predicate" ElementId="LengthRange" StringId="HelpText">The password must be between 6 and 64 characters.</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Lowercase" StringId="HelpText">a lowercase letter</LocalizedString>
<LocalizedString ElementType="Predicate" ElementId="Uppercase" StringId="HelpText">an uppercase letter</LocalizedString>
```

### <a name="inputvalidation"></a>Giriş Geçersiz Liği

InputValidation [değeri, Yüklem Geçersiz lik](predicates.md) grup iletilerinden birini yerelleştirmek için kullanılır. 

```xml
<PredicateValidations>
  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences MatchAtLeast="1">
          <PredicateReference Id="LengthRange" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Aşağıdaki örnek, bir yüklem doğrulama grubu yardım metnini nasıl yerelleştirini gösterir.

```XML
<LocalizedString ElementType="InputValidation" ElementId="CustomPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>
```

### <a name="uxelement"></a>UxElement

UxElement değeri, kullanıcı arabirimi öğelerinden birini yerelleştirmek için kullanılır. Aşağıdaki örnekte, continue ve cancel düğmelerinin nasıl yerelleştirilengösterilmektedir.

```XML
<LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
<LocalizedString ElementType="UxElement" StringId="button_cancel">Cancel</LocalizedString>
```

## <a name="next-steps"></a>Sonraki adımlar

Yerelleştirme örnekleri için aşağıdaki makalelere bakın:

- [Azure Active Directory B2C'de özel ilke ile dil özelleştirme](custom-policy-localization.md)
- [Azure Active Directory B2C'de kullanıcı akışlarıyla dil özelleştirme](user-flow-language-customization.md)
