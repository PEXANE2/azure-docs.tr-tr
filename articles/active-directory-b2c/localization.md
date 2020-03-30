---
title: Yerelleştirme - Azure Etkin Dizini B2C
description: Azure Etkin Dizin B2C'de özel bir ilkenin Yerelleştirme öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/11/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: e73eae4d66f4ff94a48dfa27e258f8ba8ef87633
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79126764"
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
| Elementtype | Evet | İlkedeki bir talep türü öğesine veya kullanıcı arabirimi öğesine başvuru. `ClaimType`Olası değerler: `UxElement` `ErrorMessage`, `Predicate`, `GetLocalizedStringsTransformationClaimType`, veya . Değer, `ClaimType` StringId'de belirtildiği gibi, talep özniteliklerinden birini yerelleştirmek için kullanılır. Değer, `UxElement` StringId'de belirtildiği gibi kullanıcı arabirimi öğelerinden birini yerelleştirmek için kullanılır. Değer, `ErrorMessage` StringId'de belirtildiği gibi sistem hata iletilerinden birini yerelleştirmek için kullanılır. Değer, `Predicate` StringId'de belirtildiği gibi [Yüklem](predicates.md) hata iletilerinden birini yerelleştirmek için kullanılır. Değer, `InputValidation` StringId'de belirtildiği gibi [PredicateValidation](predicates.md) grup hata iletilerinden birini yerelleştirmek için kullanılır. Değer, `GetLocalizedStringsTransformationClaimType` yerelleştirilmiş dizeleri taleplere kopyalamak için kullanılır. Daha fazla bilgi için [GetLocalizedStringsTransformation talepleri dönüşümüne](string-transformations.md#getlocalizedstringstransformation) bakın  | 
| Elementıd | Evet | **Eğer ElementType** `ClaimType`, `Predicate`, `InputValidation`, veya , bu öğe zaten ClaimsSchema bölümünde tanımlanan bir talep türüiçin bir başvuru içerir. |
| StringId | Evet | **ElementType** `ClaimType`ayarlanırsa, bu öğe bir talep türü bir öznitelik için bir başvuru içerir. Olası `DisplayName`değerler: `AdminHelpText`, `PatternHelpText`, veya . Değer, `DisplayName` talep görüntüleme adını ayarlamak için kullanılır. Değer, `AdminHelpText` talep kullanıcısının yardım metin adını ayarlamak için kullanılır. Değer, `PatternHelpText` talep deseni yardım metnini ayarlamak için kullanılır. **ElementType** `UxElement`ayarlanmışsa, bu öğe bir kullanıcı arabirimi öğesinin özniteliğiiçin bir başvuru içerir. **ElementType** `ErrorMessage`ayarlanmışsa, bu öğe bir hata iletisinin tanımlayıcısını belirtir. Tanımlayıcıların tam listesi için [Yerelleştirme dize kimliklerini](localization-string-ids.md) görün. `UxElement`|


Aşağıdaki örnekte yerelleştirilmiş bir kayıt sayfası gösterilmektedir. İlk üç **LocalizedString** değerleri talep özniteliğini ayarlar. Üçüncü devam düğmesinin değerini değiştirir. Sonuncusu hata iletisini değiştirir.

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

Aşağıdaki örnekte, Id ile **Yüklemin** `IsLengthBetween8And64` **UserHelp Text'i** yerelleştirilmiş bir şekilde gösterilmektedir. Ve `StrongPassword`Id ile **PredicateValidation** Id `CharacterClasses` ile **PredicateGroup** yerelleştirilmiş **UserHelpText** .

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>
```

## <a name="set-up-localization"></a>Yerelleştirmeyi ayarlama

Bu makalede, kullanıcı yolculukları için ilkedeki birden çok yerel alanı veya dili nasıl destekleyeceğiniz gösterilmektedir. Yerelleştirme üç adım gerektirir: desteklenen dillerin açık listesini ayarlayın, dile özgü dizeleri ve koleksiyonları sağlayın ve sayfa için ContentDefinition'ı güncelleyin.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Desteklenen dillerin açık listesini ayarlama

**BuildingBlocks** öğesinin altında, desteklenen dillerin listesini içeren **Yerelleştirme** öğesini ekleyin. Aşağıdaki örnek, hem İngilizce (varsayılan) hem de İspanyolca için yerelleştirme desteğinin nasıl tanımlandığını gösterir:

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

## <a name="next-steps"></a>Sonraki adımlar

Yerelleştirme örnekleri için aşağıdaki makalelere bakın:

- [Azure Active Directory B2C'de özel ilke ile dil özelleştirme](custom-policy-localization.md)
- [Azure Active Directory B2C'de kullanıcı akışlarıyla dil özelleştirme](user-flow-language-customization.md)
