---
title: Yüklemler ve Yüklemler
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkeler kullanarak azure AD B2C kiracınıza bozuk veri eklenmesini önleyin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 887c9432f04cce775e045bb6da83f0af4a4a4bce
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396881"
---
# <a name="predicates-and-predicatevalidations"></a>Yüklemler ve Yüklemler

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**Yüklemler** ve **Yüklemler Özdeyişler** öğeleri, Azure Active Directory B2C (Azure AD B2C) kiracınıza yalnızca düzgün biçimlendirilmiş verilerin girilmesini sağlamak için bir doğrulama işlemi gerçekleştirmenize olanak tanır.

Aşağıdaki diyagram, öğeler arasındaki ilişkiyi gösterir:

![Yüklemleri ve Yüklem Doğrulamalarını İlişkisi Gösteren Diyagram](./media/predicates/predicates.png)

## <a name="predicates"></a>Koşullar

**Yüklem** öğesi, bir talep türünün değerini denetlemek için temel `true` bir `false`doğrulama tanımlar ve geri verir veya . Doğrulama, belirtilen bir **Yöntem** öğesi ve yöntemle ilgili **bir parametre** öğesi kümesi kullanılarak yapılır. Örneğin, bir yüklem, bir dize talep değerinin uzunluğunun belirtilen minimum ve maksimum parametreler aralığında olup olmadığını veya bir dize talep değerinin bir karakter kümesi ni içerip içermediğini denetleyebilir. **UserHelpText** öğesi, denetim başarısız olursa kullanıcılar için bir hata iletisi sağlar. **UserHelpText** öğesinin değeri [dil özelleştirme](localization.md)kullanılarak yerelleştirilebilir.

**Yüklemler** [öğesi, BuildingBlocks](buildingblocks.md) öğesi içindeki **ClaimsSchema** öğesini doğrudan takip etmelidir.

**Yüklemler** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yüklemi | 1:n | Yüklemlerin listesi. |

**Yüklem** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yüklem için kullanılan bir tanımlayıcı. Diğer öğeler bu tanımlayıcıyı ilkede kullanabilir. |
| Yöntem | Evet | Doğrulama için kullanılacak yöntem türü. Olası değerler: [IsLengthRange](#islengthrange), [MatchesRegex](#matchesregex), [IçerirKarakterler](#includescharacters), veya [IsDateRange](#isdaterange).  |
| Helptext | Hayır | Denetim başarısız olursa kullanıcılar için bir hata iletisi. Bu dize [dil özelleştirme](localization.md) kullanılarak yerelleştirilebilir |

**Yüklem** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| UserHelpText | 0:1 | (Amortismana uğradı) Denetim başarısız olursa kullanıcılar için bir hata iletisi. |
| Parametreler | 1:1 | Dize doğrulama yöntemi türü için parametreler. |

**Parametreler** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Parametre | 1:n | Dize doğrulama yöntemi türü için parametreler. |

**Parametre** öğesi aşağıdaki öznitelikleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Kimlik | 1:1 | Parametrenin tanımlayıcısı. |

### <a name="predicate-methods"></a>Yüklem yöntemleri

#### <a name="islengthrange"></a>IslengthRange Aralığı

IsLengthRange yöntemi, bir dize talep değerinin uzunluğunun belirtilen minimum ve maksimum parametreler aralığında olup olmadığını denetler. Yüklem öğesi aşağıdaki parametreleri destekler:

| Parametre | Gerekli | Açıklama |
| ------- | ----------- | ----------- |
| Maksimum | Evet | Girilebilen maksimum karakter sayısı. |
| Minimum | Evet | Girilmesi gereken en az karakter sayısı. |


Aşağıdaki örnekte parametreleri `Minimum` içeren ve `Maximum` dize uzunluk aralığını belirten bir IsLengthRange yöntemi gösterilmektedir:

```XML
<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange" HelpText="The password must be between 8 and 64 characters.">
  <Parameters>
    <Parameter Id="Minimum">8</Parameter>
    <Parameter Id="Maximum">64</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="matchesregex"></a>KibritlerRegex

MatchesRegex yöntemi, bir dize talep değerinin normal bir ifadeyle eşleşip eşleşmediğini denetler. Yüklem öğesi aşağıdaki parametreleri destekler:

| Parametre | Gerekli | Açıklama |
| ------- | ----------- | ----------- |
| Düzenli İfade | Evet | Eşleşecek normal ifade deseni. |

Aşağıdaki örnekte, `MatchesRegex` parametreye `RegularExpression` sahip bir yöntem normal bir ifade gösterir:

```XML
<Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be numbers only.">
  <Parameters>
    <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="includescharacters"></a>Karakterleri Içerir

IncludesCharacters yöntemi, bir dize talep değerinin bir karakter kümesi ni içerip içermediğini denetler. Yüklem öğesi aşağıdaki parametreleri destekler:

| Parametre | Gerekli | Açıklama |
| ------- | ----------- | ----------- |
| Characterset | Evet | Girilebilen karakter kümesi. Örneğin, küçük karakterler, `a-z`büyük harfler `A-Z`karakterler, `0-9`basamaklar veya `@#$%^&amp;*\-_+=[]{}|\\:',?/~"();!`. |

Aşağıdaki örnek, `IncludesCharacters` karakter kümesini `CharacterSet` belirten parametreye sahip bir yöntem gösterir:

```XML
<Predicate Id="Lowercase" Method="IncludesCharacters" HelpText="a lowercase letter">
  <Parameters>
    <Parameter Id="CharacterSet">a-z</Parameter>
  </Parameters>
</Predicate>
```

#### <a name="isdaterange"></a>IsDateRange

IsDateRange yöntemi, tarih talep değerinin belirtilen minimum ve en büyük parametreler aralığı arasında olup olmadığını denetler. Yüklem öğesi aşağıdaki parametreleri destekler:

| Parametre | Gerekli | Açıklama |
| ------- | ----------- | ----------- |
| Maksimum | Evet | Girilebilen mümkün olan en büyük tarih. Tarihin biçimi, sözleşmeyi `yyyy-mm-dd` veya `Today`. |
| Minimum | Evet | Girilebilen mümkün olan en küçük tarih. Tarihin biçimi, sözleşmeyi `yyyy-mm-dd` veya `Today`.|

Aşağıdaki örnekte `IsDateRange` parametreleri `Minimum` olan `Maximum` bir yöntem gösterilmektedir ve `yyyy-mm-dd` tarih `Today`aralığını bir biçimve .

```XML
<Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 1970-01-01 and today.">
  <Parameters>
    <Parameter Id="Minimum">1970-01-01</Parameter>
    <Parameter Id="Maximum">Today</Parameter>
  </Parameters>
</Predicate>
```

## <a name="predicatevalidations"></a>YüklemLer

Yüklemler bir talep türüne karşı kontrol etmek için doğrulamayı tanımlasa **da, Yüklemler,** bir talep türüne uygulanabilecek bir kullanıcı giriş doğrulaması oluşturmak için bir yüklem kümesi gruplar. Her **YüklemValidasyon** öğesi, **bir**Yüklem'i işaret eden bir predicateReference öğesi kümesi içeren bir **PredicateGroup** öğesi kümesi içerir. **PredicateReference** Doğrulamayı geçmek için, talebin değeri, **YüklemReferans** öğeleri kümesiyle tüm **Yüklem Grubu** altında yapılan tüm testleri geçmelidir.

**Yüklemler** [öğesi, BuildingBlocks](buildingblocks.md) öğesi içindeki **Yüklemler** öğesini doğrudan izleyerek görünmelidir.

```XML
<PredicateValidations>
  <PredicateValidation Id="">
    <PredicateGroups>
      <PredicateGroup Id="">
        <UserHelpText></UserHelpText>
        <PredicateReferences MatchAtLeast="">
          <PredicateReference Id="" />
          ...
        </PredicateReferences>
      </PredicateGroup>
      ...
    </PredicateGroups>
  </PredicateValidation>
...
</PredicateValidations>
```

**Yüklemler** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Yüklem Validasyonu | 1:n | Yüklem doğrulama listesi. |

**YüklemÖz** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yüklem doğrulaması için kullanılan bir tanımlayıcı. **ClaimType** öğesi bu tanımlayıcıyı ilkede kullanabilir. |

**YüklemGeçersiz madde** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| YüklemGrupları | 1:n | Yüklem gruplarının listesi. |

**Yüklem Grupları** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| YüklemGrubu | 1:n | Yüklemlerin listesi. |

**PredicateGroup** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yüklem grubu için kullanılan bir tanımlayıcı.  |

**PredicateGroup** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| UserHelpText | 0:1 |  Kullanıcıların hangi değeri yazmaları gerektiğini bilmelerine yardımcı olabilecek yüklemin açıklaması. |
| YüklemReferanslar | 1:n | Yüklem başvurularının listesi. |

**Yüklemler** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| En uygun | Hayır | Girdinin kabul edilemesi için değerin en az bu kadar çok yüklem tanımıyla eşleşmesi gerektiğini belirtir. Belirtilmemişse, değer tüm yüklem tanımlarını eşleştirmelidir. |

**Yüklemler** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| YüklemReferans | 1:n | Bir yüklem için bir referans. |

**YüklemBaşvuru** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Yüklem doğrulaması için kullanılan bir tanımlayıcı.  |


## <a name="configure-password-complexity"></a>Parola karmaşıklığını yapılandırma

**Yüklemler** ve **PredicateValidationsInput** ile bir hesap oluştururken bir kullanıcı tarafından sağlanan parolalar için karmaşıklık gereksinimlerini kontrol edebilirsiniz. Varsayılan olarak, Azure AD B2C güçlü parolalar kullanır. Azure AD B2C, müşterilerin kullanabileceği parolaların karmaşıklığını denetlemek için yapılandırma seçeneklerini de destekler. Bu yüklem öğelerini kullanarak parola karmaşıklığını tanımlayabilirsiniz:

- **IsLengthBetween8And64** yöntemini `IsLengthRange` kullanarak, parola8 ve 64 karakter arasında olması gerektiğini doğrular.
- Yöntemi kullanarak **Küçük Harf,** parolanın küçük harf içerdiğini doğrular. `IncludesCharacters`
- Yöntemi kullanarak **Büyük harf,** parolabir büyük harf içerdiğini doğrular. `IncludesCharacters`
- `IncludesCharacters` Yöntemi kullanarak **sayı,** parola bir basamak içerdiğini doğrular.
- `IncludesCharacters` Yöntemi kullanan **sembol,** parolanın birkaç sembol karakterden birini içerdiğini doğrular.
- `MatchesRegex` Yöntemi kullanarak **PIN,** parolanın yalnızca sayılar içerdiğini doğrular.
- Yöntemin kullanılmasına `MatchesRegex` izin verilen **AADCharacters,** parolanın yalnızca geçersiz karakterin sağlandığını doğrular.
- **DisallowedWhitespace** `MatchesRegex` yöntemini kullanarak, parola nın bir beyaz boşluk karakteriyle başlamadığını veya bitmediğini doğrular.

```XML
<Predicates>
  <Predicate Id="IsLengthBetween8And64" Method="IsLengthRange" HelpText="The password must be between 8 and 64 characters.">
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
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

  <Predicate Id="Number" Method="IncludesCharacters" HelpText="a digit">
    <Parameters>
      <Parameter Id="CharacterSet">0-9</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Symbol" Method="IncludesCharacters" HelpText="a symbol">
    <Parameters>
      <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="PIN" Method="MatchesRegex" HelpText="The password must be numbers only.">
    <Parameters>
      <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="AllowedAADCharacters" Method="MatchesRegex" HelpText="An invalid character was provided.">
    <Parameters>
      <Parameter Id="RegularExpression">(^([0-9A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~"();! ]|(\.(?!@)))+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="DisallowedWhitespace" Method="MatchesRegex" HelpText="The password must not begin or end with a whitespace character.">
    <Parameters>
      <Parameter Id="RegularExpression">(^\S.*\S$)|(^\S+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>
```

Temel doğrulamaları tanımladıktan sonra, bunları birleştirebilir ve ilkenizde kullanabileceğiniz bir dizi parola ilkesi oluşturabilirsiniz:

- **SimplePassword** DisallowedWhitespace, AllowedAADCharacters ve IsLengthBetween8And64 doğrular
- **StrongPassword** DisallowedWhitespace, AllowedAADCharacters, IsLengthBetween8And64 doğrular. Son grup `CharacterClasses` `MatchAtLeast` 3'e ayarlanmış ek bir yüklem kümesi çalıştırın. Kullanıcı parolası 8 ile 16 karakter arasında ve aşağıdaki karakterlerden üçü arasında olmalıdır: Küçük Harf, Büyük harf, Sayı veya Sembol.
- **CustomPassword** yalnızca DisallowedWhitespace, AllowedAADCharacters doğrular. Bu nedenle, karakterler geçerli olduğu sürece kullanıcı herhangi bir uzunlukta herhangi bir parola sağlayabilir.

```XML
<PredicateValidations>
  <PredicateValidation Id="SimplePassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="StrongPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
       </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
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

  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Talep **türünüzde, PredicateValidationReference** öğesini ekleyin ve tanımlayıcıyı SimplePassword, StrongPassword veya CustomPassword gibi yüklem doğrulamalarından biri olarak belirtin.

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>Enter password</AdminHelpText>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
  <PredicateValidationReference Id="StrongPassword" />
</ClaimType>
```

Azure AD B2C hata iletisini görüntülediğinde öğelerin nasıl düzenlendiğini aşağıda gösterir:

![Yüklem diyagramı ve YüklemGrubu parola karmaşıklığı örneği](./media/predicates/predicates-pass.png)

## <a name="configure-a-date-range"></a>Tarih aralığını yapılandırma

**Yüklemler** ve **Yüklemler Özdeyişler** öğeleri ile **UserInputType'ın** minimum ve maksimum `DateTimeDropdown`tarih değerlerini bir . Bunu yapmak için, yöntemle bir `IsDateRange` **Yüklem** oluşturun ve minimum ve maksimum parametreleri sağlayın.

```XML
<Predicates>
  <Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 01-01-1980 and today.">
    <Parameters>
      <Parameter Id="Minimum">1980-01-01</Parameter>
      <Parameter Id="Maximum">Today</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Yükleme atıfta bulunarak bir **Yüklem Geçerliliği** ekleyin. `DateRange`

```XML
<PredicateValidations>
  <PredicateValidation Id="CustomDateRange">
    <PredicateGroups>
      <PredicateGroup Id="DateRangeGroup">
        <PredicateReferences>
          <PredicateReference Id="DateRange" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Talep türünüzde, **PredicateValidationReference** öğesini ekleyin ve tanımlayıcıyı `CustomDateRange`' olarak belirtin.

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date of Birth</DisplayName>
  <DataType>date</DataType>
  <AdminHelpText>The user's date of birth.</AdminHelpText>
  <UserHelpText>Your date of birth.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
  <PredicateValidationReference Id="CustomDateRange" />
</ClaimType>
 ```

## <a name="next-steps"></a>Sonraki adımlar

- Yüklem [doğrulamalarını kullanarak Azure Active Directory B2C'deki özel ilkeleri kullanarak parola karmaşıklığını](custom-policy-password-complexity.md) nasıl yapılandırıştırmayı öğrenin.