---
title: Yerelleştirme-Azure Active Directory B2C
description: Azure Active Directory B2C bir özel ilkenin yerelleştirme öğesini belirtin.
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
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79126764"
---
# <a name="localization"></a>Yerelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yerelleştirme** öğesi, Kullanıcı yolculukları için ilkedeki birden çok yerel ayarı veya dili desteketmenize olanak tanır. İlkelerde yerelleştirme desteği şunları yapmanıza olanak sağlar:

- Bir ilkede desteklenen dillerin açık listesini ayarlayın ve varsayılan bir dil seçin.
- Dile özgü dizeler ve koleksiyonlar sağlayın.

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
| Etkin | Hayır | Olası değerler: `true` veya `false`. |

**Yerelleştirme** Öğesı aşağıdaki XML öğelerini içerir

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1: n | Desteklenen dillerin listesi. |
| LocalizedResources | 0: n | Yerelleştirilmiş kaynakların listesi. |

## <a name="supportedlanguages"></a>SupportedLanguages

**Supportedlanguages** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| DefaultLanguage | Yes | Yerelleştirilmiş kaynaklar için varsayılan olarak kullanılacak dil. |
| MergeBehavior | Hayır | Aynı tanımlayıcıya sahip bir üst ilkede bulunan tüm ClaimType ile birlikte birleştirilmiş değerlerin sabit listesi değerleri. Temel ilkede belirtilen bir talebin üzerine yazdığınızda bu özniteliği kullanın. Olası değerler: `Append`, `Prepend`veya `ReplaceAll`. `Append` değeri, var olan veri koleksiyonunun üst ilkede belirtilen koleksiyonun sonuna eklenmesi gerektiğini belirtir. `Prepend` değeri, var olan veri koleksiyonunun üst ilkede belirtilen koleksiyondan önce eklenmesi gerektiğini belirtir. `ReplaceAll` değeri, üst ilkede tanımlanan veri koleksiyonunun, bunun yerine geçerli ilkede tanımlanan veriler kullanılarak yoksayılacağını belirtir. |

### <a name="supportedlanguages"></a>SupportedLanguages

**Supportedlanguages** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1: n | Dilleri tanımlamak için RFC 5646-Tags başına bir dil etiketine uyan içeriği görüntüler. |

## <a name="localizedresources"></a>LocalizedResources

**Localizedresources** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Yes | Yerelleştirilmiş kaynakları benzersiz şekilde tanımlamak için kullanılan bir tanımlayıcı. |

**Localizedresources** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0: n | Çeşitli kültürlerde tüm koleksiyonları tanımlar. Bir koleksiyon çeşitli kültürler için farklı sayıda öğe ve farklı dize içerebilir. Koleksiyon örnekleri, talep türlerinde görünen numaralandırmaları içerir. Örneğin, bir ülke/bölge listesi kullanıcıya bir açılan listede gösterilir. |
| LocalizedStrings | 0: n | Çeşitli kültürlerde koleksiyonlarda görünen dizeler hariç tüm dizeleri tanımlar. |

### <a name="localizedcollections"></a>LocalizedCollections

**Localizedcollections** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1: n | Desteklenen dillerin listesi. |

#### <a name="localizedcollection"></a>LocalizedCollection

**Localizedcollection** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Yes | İlke dosyasında bir ClaimType öğesine veya bir kullanıcı arabirimi öğesine başvurur. |
| ElementID | Yes | **ElementType** bir ClaimType olarak ayarlandıysa kullanılan ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türüne başvuru içeren bir dize. |
| TargetCollection | Yes | Hedef koleksiyon. |

**Localizedcollection** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0: n | Kullanıcının, açılan menüdeki bir değer gibi kullanıcı arabirimindeki bir talep için seçim yapmak üzere kullanılabilir bir seçenek tanımlar. |

**Öğe** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Metin | Yes | Bu seçenek için Kullanıcı arabiriminde kullanıcıya gösterilmesi gereken kullanıcı dostu görüntüleme dizesi. |
| Değer | Yes | Bu seçeneği belirleyerek ilişkili dize talep değeri. |
| SelectByDefault | Hayır | Bu seçeneğin Kullanıcı arabiriminde varsayılan olarak seçilmesinin gerekip gerekmediğini gösterir. Olası değerler: true veya false. |

Aşağıdaki örnek, **Localizedcollections** öğesinin kullanımını gösterir. Biri Ingilizce ve diğeri Ispanyolca için olmak üzere iki **Localizedcollection** öğesi içerir. Her ikisi de **, bir dizi `Gender`, İngilizce** ve İspanyolca bir öğe listesiyle ayarlayın.

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

### <a name="localizedstrings"></a>LocalizedStrings

**Localizedstrings** öğesi aşağıdaki öğeleri içerir:

| Öğe | Öğeleri | Açıklama |
| ------- | ----------- | ----------- |
| LocalizedString | 1: n | Yerelleştirilmiş bir dize. |

**LocalizedString** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ElementType | Yes | İlkede bir talep türü öğesine veya bir kullanıcı arabirimi öğesine başvuru. Olası değerler: `ClaimType`, `UxElement`, `ErrorMessage`, `Predicate`veya `GetLocalizedStringsTransformationClaimType`. `ClaimType` değeri, Strıngıd içinde belirtildiği gibi talep özniteliklerinden birini yerelleştirmek için kullanılır. `UxElement` değeri, Strıngıd içinde belirtilen kullanıcı arabirimi öğelerinden birini yerelleştirmek için kullanılır. `ErrorMessage` değeri, Strıngıd içinde belirtilen sistem hatası iletilerinden birini yerelleştirmek için kullanılır. `Predicate` değeri, Strıngıd içinde belirtilen [koşul](predicates.md) hatası iletilerinden birini yerelleştirmek için kullanılır. `InputValidation` değeri, Strıngıd içinde belirtilen tahmine [Atevalidation](predicates.md) Group hata iletilerinden birini yerelleştirmek için kullanılır. `GetLocalizedStringsTransformationClaimType` değeri, yerelleştirilmiş dizeleri taleplerine kopyalamak için kullanılır. Daha fazla bilgi için bkz. [Getlocalizedstringstrans, talepler dönüştürmesi](string-transformations.md#getlocalizedstringstransformation)  | 
| ElementID | Yes | **ElementType** `ClaimType`, `Predicate`veya `InputValidation`olarak ayarlandıysa, bu öğe ClaimsSchema bölümünde zaten tanımlanmış olan bir talep türüne başvuru içerir. |
| StringID | Yes | **ElementType** `ClaimType`olarak ayarlandıysa, bu öğe bir talep türünün özniteliğine bir başvuru içerir. Olası değerler: `DisplayName`, `AdminHelpText`veya `PatternHelpText`. `DisplayName` değeri, talep görünen adını ayarlamak için kullanılır. `AdminHelpText` değeri, talep kullanıcısının yardım metni adını ayarlamak için kullanılır. `PatternHelpText` değeri, talep deseninin yardım metnini ayarlamak için kullanılır. **ElementType** `UxElement`olarak ayarlandıysa, bu öğe bir kullanıcı arabirimi öğesinin özniteliğine bir başvuru içerir. **ElementType** `ErrorMessage`olarak ayarlandıysa, bu öğe bir hata iletisinin tanımlayıcısını belirtir. `UxElement` tanımlayıcılarının tüm listesi için bkz. [Yerelleştirme dize kimlikleri](localization-string-ids.md) .|


Aşağıdaki örnekte, yerelleştirilmiş kaydolma sayfası gösterilmektedir. İlk üç **LocalizedString** değeri talep özniteliğini ayarlar. Üçüncü, devam düğmesinin değerini değiştirir. Son bir hata iletisini değiştirir.

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

Aşağıdaki örnek, kimliği `IsLengthBetween8And64`olan koşulun **Userhelptext** öğesini yerelleştirilmiş olarak gösterir. Ve kimlik `StrongPassword`tahmine **Atevalidation** `CharacterClasses` kimliği olan **predicategroup** 'un yerelleştirilmiş **userhelptext** .

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

Bu makalede, Kullanıcı bağlantısı için ilkedeki birden çok yerel ayarı veya dili nasıl destekleyeceği gösterilmektedir. Yerelleştirme üç adım gerektirir: desteklenen dillerin açık listesini ayarlayın, dile özgü dizeler ve koleksiyonlar sağlayın ve sayfa için ContentDefinition 'ı düzenleyin.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Desteklenen dillerin açık listesini ayarlama

**Buildingblocks** öğesi altında, desteklenen dillerin listesine sahip **Yerelleştirme** öğesini ekleyin. Aşağıdaki örnek, hem Ingilizce (varsayılan) hem de Ispanyolca için yerelleştirme desteğinin nasıl tanımlanacağını göstermektedir:

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

- [Azure Active Directory B2C özel ilkeyle dil özelleştirmesi](custom-policy-localization.md)
- [Azure Active Directory B2C Kullanıcı akışları ile dil özelleştirmesi](user-flow-language-customization.md)
