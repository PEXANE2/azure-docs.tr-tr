---
title: İddialarSchema - Azure Active Directory B2C | Microsoft Dokümanlar
description: Azure Active Directory B2C'de özel bir ilkenin ClaimsSchema öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/05/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 4c3b3318e941723ec333597c7e4b3e48710152d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78397794"
---
# <a name="claimsschema"></a>ClaimsSchema

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**ClaimsSchema** öğesi, ilkenin bir parçası olarak başvurulabilecek talep türlerini tanımlar. İddialar şema, iddialarınızı beyan ettiğiniz yerdir. Bir talep ad, soyad, görüntü adı, telefon numarası ve daha fazlası olabilir. ClaimsSchema öğesi **ClaimType** öğelerinin listesini içerir. **ClaimType** öğesi, talep adı olan **Id** özniteliğini içerir.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="Id">
      <DisplayName>Surname</DisplayName>
      <DataType>string</DataType>
      <DefaultPartnerClaimTypes>
        <Protocol Name="OAuth2" PartnerClaimType="family_name" />
        <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
        <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
      </DefaultPartnerClaimTypes>
      <UserHelpText>Your surname (also known as family name or last name).</UserHelpText>
      <UserInputType>TextBox</UserInputType>
```

## <a name="claimtype"></a>Claimtype

**ClaimType** öğesi aşağıdaki öznitelik içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Evet | Talep türü için kullanılan bir tanımlayıcı. Diğer öğeler bu tanımlayıcıyı ilkede kullanabilir. |

**ClaimType** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| DisplayName | 1:1 | Çeşitli ekranlarda kullanıcılara görüntülenen başlık. Değer [yerelleştirilebilir.](localization.md) |
| DataType | 1:1 | İddianın türü. |
| VarsayılanOrtak Talep Türleri | 0:1 | İş ortağı varsayılan talep türleri belirtilen bir protokol için kullanılacak. Değer, Giriş Talebi veya **OutputClaim** öğelerinde belirtilen **PartnerClaimType'da** üzerine yazılabilir. **InputClaim** Bir protokolün varsayılan adını belirtmek için bu öğeyi kullanın.  |
| Maskeleme | 0:1 | Talebi görüntülerken uygulanabilecek isteğe bağlı maskeleme karakterleri dizisi. Örneğin, telefon numarası 324-232-4343 XXX-XXX-4343 olarak maskelenebilir. |
| UserHelpText | 0:1 | Kullanıcıların amacını anlamalarına yardımcı olabilecek talep türünün açıklaması. Değer [yerelleştirilebilir.](localization.md) |
| UserInputType | 0:1 | Talep türüiçin talep verilerini el ile girerken kullanıcıtarafından kullanılabilir olması gereken giriş denetimi türü. Daha sonra bu sayfada tanımlanan kullanıcı giriş türlerine bakın. |
| AdminHelpText | 0:1 | Yöneticilerin amacını anlamalarına yardımcı olabilecek talep türünün açıklaması. |
| Kısıtlama | 0:1 | Normal bir ifade (Regex) veya kabul edilebilir değerlerin listesi gibi bu talep için değer kısıtlamaları. Değer [yerelleştirilebilir.](localization.md) |
YüklemValidationReferans| 0:1 | **Bir PredicateValidationsInput** öğesine bir başvuru. **PredicateValidationReference** öğeleri, yalnızca düzgün biçimlendirilmiş verilerin girilmesini sağlamak için bir doğrulama işlemi gerçekleştirmenize olanak tanır. Daha fazla bilgi için [Bkz. Predicates](predicates.md). |



### <a name="datatype"></a>DataType

**DataType** öğesi aşağıdaki değerleri destekler:

| Tür | Açıklama |
| ------- | ----------- |
|boole|Boolean (`true` veya `false`) değerini temsil eder.|
|date| Genellikle bir günün tarihi olarak ifade edilen, zaman içinde bir an temsil eder. Tarihin değeri ISO 8601 kuralını takip eder.|
|tarih saat|Genellikle günün tarihi ve saati olarak ifade edilen bir zamanı temsil eder. Tarihin değeri ISO 8601 kuralını takip eder.|
|süre|Yıl, ay, gün, saat, dakika ve saniye cinsinden bir zaman aralığını temsil eder. `PnYnMnDTnHnMnS`Biçimi, pozitif `P` veya negatif değer `N` için gösterir. `nY`yıl sayısı gerçek bir takip `Y`edilir. `nMo`bir harfi `Mo`takip ay sayısıdır. `nD`bir harfi `D`takip gün sayısıdır. Örnekler: `P21Y` 21 yılı temsil eder. `P1Y2Mo`bir yıl ve iki ayı temsil eder. `P1Y2Mo5D`bir yıl, iki ay ve beş günü temsil eder.  `P1Y2M5DT8H5M620S`bir yıl, iki ay, beş gün, sekiz saat, beş dakika ve yirmi saniyeyi temsil eder.  |
|Phonenumber|Bir telefon numarasını temsil eder. |
|int| -2.147.483.648 ile 2.147.483.647 arasındaki sayıyı temsil eder.|
|long| -9.223.372.036.854.775.808 ile 9.223.372.036.854.775.807 arasındaki rakamı temsil eder. |
|string| Metni UTF-16 kod birimlerinin sırası olarak temsil eder.|
|Stringcollection|Bir koleksiyon `string`temsil eder.|
|kullanıcıKimliği| Kullanıcı kimliğini temsil eder.|
|userIdentityCollection|Bir koleksiyon `userIdentity`temsil eder.|

### <a name="defaultpartnerclaimtypes"></a>VarsayılanOrtak Talep Türleri

**DefaultPartnerClaimTypes** aşağıdaki öğeyi içerebilir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Protokol | 1:n | Varsayılan ortak talep türü adı ile protokollerin listesi. |

**Protokol** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Adı | Evet | Azure AD B2C tarafından desteklenen geçerli bir protokolün adı. Olası değerler şunlardır: OAuth1, OAuth2, SAML2, OpenIdConnect. |
| OrtakClaimType | Evet | Kullanılacak talep türü adı. |

Aşağıdaki örnekte, Kimlik Deneyimi Çerçevesi bir SAML2 kimlik sağlayıcısı veya güvenerek **surname** taraf uygulaması ile `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`etkileşime girdiğinde, soyadı talebi OpenIdConnect ve OAuth2 ile eşlenir, talep `family_name`.

```XML
<ClaimType Id="surname">
  <DisplayName>Surname</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OAuth2" PartnerClaimType="family_name" />
    <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
    <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
  </DefaultPartnerClaimTypes>
</ClaimType>
```

Sonuç olarak, Azure AD B2C tarafından verilen JWT belirteci, ClaimType ad `family_name` **soyadı**yerine yayır.

```JSON
{
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  "auth_time": 1535013501,
  "given_name": "David",
  "family_name": "Williams",
  "name": "David Williams",
}
```

### <a name="mask"></a>Maskeleme

**Maske** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| `Type` | Evet | Talep maskesinin türü. Olası `Simple` değerler: `Regex`veya . Değer, `Simple` dize talebinin önde gelen bölümüne basit bir metin maskesi uygulandığını gösterir. Değer, dize `Regex` talebine bir bütün olarak normal bir ifadenin uygulandığını gösterir.  `Regex` Değer belirtilirse, kullanılacak normal ifadeyle isteğe bağlı bir öznitelik de tanımlanmalıdır. |
| `Regex` | Hayır | **`Type`** Ayarlanmışsa, kullanılacak normal ifadeyi belirtin. `Regex`

Aşağıdaki örnek, maskeyle **bir PhoneNumber** iddiasını yapılandırır: `Simple`

```XML
<ClaimType Id="PhoneNumber">
  <DisplayName>Phone Number</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Simple">XXX-XXX-</Mask>
  <UserHelpText>Your telephone number.</UserHelpText>
</ClaimType>
```

Kimlik Deneyimi Çerçevesi, ilk altı basamağı gizlerken telefon numarasını işler:

![X'ler tarafından maskeli ilk altı basamaklı tarayıcıda gösterilen telefon numarası iddiası](./media/claimsschema/mask.png)

Aşağıdaki örnek, maskeyle birlikte Bir `Regex` Alternatif **E-posta** iddiasını yapılandırır:

```XML
<ClaimType Id="AlternateEmail">
  <DisplayName>Please verify the secondary email linked to your account</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Regex" Regex="(?&lt;=.).(?=.*@)">*</Mask>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```

Kimlik Deneyimi Çerçevesi, yalnızca e-posta adresinin ilk harfini ve e-posta alan adını işler:

![Yıldız işaretleri tarafından maskeli karakterlerle tarayıcıda gösterilen e-posta talebi](./media/claimsschema/mask-regex.png)


### <a name="restriction"></a>Kısıtlama

**Kısıtlama** öğesi aşağıdaki öznitelik içerebilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Birleştirme Davranışı | Hayır | Numaralandırma değerlerini bir üst ilkedeki ClaimType ile aynı tanımlayıcıyla birleştirmek için kullanılan yöntem. Temel ilkede belirtilen bir talebin üzerine yazarken bu özniteliği kullanın. Olası `Append`değerler: `Prepend`, `ReplaceAll`, veya . Değer, `Append` üst ilkede belirtilen koleksiyonun sonuna ekilmesi gereken bir veri topluluğudur. Değer, `Prepend` üst ilkede belirtilen koleksiyondan önce eklenmesi gereken bir veri topluluğudur. Değer, `ReplaceAll` üst ilkede belirtilen ve yoksayılması gereken bir veri topluluğudur. |

**Kısıtlama** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşum | Açıklama |
| ------- | ----------- | ----------- |
| Sabit Listesi | 1:n | Kullanıcının bir talep için seçmesi için kullanıcı arabirimindeki kullanılabilir seçenekler(örneğin, açılır düşüşteki bir değer). |
| Desen | 1:1 | Kullanılacak normal ifade. |

#### <a name="enumeration"></a>Sabit Listesi

**Numaralandırma** öğesi, kullanıcı arabirimindeki bir talep için kullanıcı nın seçebilmek için kullanılabilir `CheckboxMultiSelect` `DropdownSingleSelect`seçenekleri `RadioSingleSelect`tanımlar( örneğin, bir , veya . Alternatif olarak, [Yerelleştirilmiş Koleksiyonlar](localization.md#localizedcollections) öğesiyle kullanılabilir seçenekleri tanımlayabilir ve yerelleştirebilirsiniz. Bir talep Numaralandırma koleksiyonundan bir **öğeyi** aramak için [GetMappedValueFromLocalizedCollection](string-transformations.md#getmappedvaluefromlocalizedcollection) talepleri dönüştürmesini kullanın.

**Numaralandırma** öğesi aşağıdaki öznitelikleri içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Metin | Evet | Bu seçenek için kullanıcı arabiriminde kullanıcıya gösterilen görüntü dizesi. |
|Değer | Evet | Bu seçeneğin seçilmesiile ilişkili talep değeri. |
| SelectbyDefault | Hayır | UI'de bu seçeneğin varsayılan olarak seçilip seçilmemesi gerektiğini gösterir. Olası değerler: Doğru veya Yanlış. |

Aşağıdaki örnekte, **city** varsayılan değer kümesi olan bir şehir `New York`açılır listesi talebi yapılandırılır:

```XML
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

Varsayılan değeri New York olarak ayarlanmış açılır şehir listesi:

![Tarayıcıda işlenen ve varsayılan değeri gösteren açılır bırakma denetimi](./media/claimsschema/dropdownsingleselect.png)

### <a name="pattern"></a>Desen

**Desen** öğesi aşağıdaki öznitelikleri içerebilir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Düzenli İfade | Evet | Geçerli olabilmesi için bu tür iddiaların eşmesi gereken normal ifade. |
| Helptext | Hayır | Normal ifade denetimi başarısız olursa kullanıcılar için bir hata iletisi. |

Aşağıdaki örnek, normal ifade girişi doğrulama ve yardım metni ile bir **e-posta** talebi yapılandırır:

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OpenIdConnect" PartnerClaimType="email" />
  </DefaultPartnerClaimTypes>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9.!#$%&amp;'^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" HelpText="Please enter a valid email address." />
    </Restriction>
 </ClaimType>
```

Kimlik Deneyimi Çerçevesi, e-posta biçimi giriş doğrulamasıyla e-posta adresi talebini işler:

![Regex kısıtlaması tarafından tetiklenen hata iletisi gösteren TextBox](./media/claimsschema/pattern.png)

### <a name="userinputtype"></a>UserInputType

Azure AD B2C, talep türü için talep verilerini el ile girerken kullanılabilecek textbox, parola ve açılır bırakma listesi gibi çeşitli kullanıcı giriş türlerini destekler. Kullanıcıdan bilgi [topladığınızda, kendi kendine öne sürülen bir teknik profil](self-asserted-technical-profile.md) ve [görüntü denetimleri](display-controls.md)kullanarak **UserInputType'ı** belirtmeniz gerekir.

**UserInputType** öğesi kullanılabilir kullanıcı giriş türleri:

| UserInputType | Desteklenen Talep Türü | Açıklama |
| --------- | -------- | ----------- |
|Onay KutusuMultiSelect| `string` |Çok seçimli açılır kutu. Talep değeri, seçili değerlerin virgülde sınırlayıcı dizesinde temsil edilir. |
|DateTimeDropdown | `date`, `dateTime` |Bir gün, ay ve yıl seçmek için açılır düşüşler. |
|DropdownTekSeç |`string` |Tek seçim açılır kutusu. Talep değeri seçili değerdir.|
|EmailKutusu | `string` |E-posta giriş alanı. |
|Paragraf | `boolean`, `date`, `dateTime`, `duration`, `int`, `long`, `string`|Metni yalnızca bir paragraf etiketinde gösteren bir alan. |
|Parola | `string` |Parola metin kutusu.|
|RadioSingleSelect |`string` | Radyo düğmelerinin toplanması. Talep değeri seçili değerdir.|
|Readonly | `boolean`, `date`, `dateTime`, `duration`, `int`, `long`, `string`| Salt okunur metin kutusu. |
|TextBox |`boolean`, `int`, `string` |Tek satırlık metin kutusu. |


#### <a name="textbox"></a>TextBox

**TextBox** kullanıcı giriş türü, tek satırlık bir metin kutusu sağlamak için kullanılır.

![Talep türünde belirtilen özellikleri gösteren TextBox](./media/claimsschema/textbox.png)

```XML
<ClaimType Id="displayName">
  <DisplayName>Display Name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your display name.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

#### <a name="emailbox"></a>EmailKutusu

**EmailBox** kullanıcı giriş türü temel bir e-posta giriş alanı sağlamak için kullanılır.

![E-posta Kutusu talep türünde belirtilen özellikleri gösteren](./media/claimsschema/emailbox.png)

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>EmailBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+(?:\.[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?$" HelpText="Please enter a valid email address." />
  </Restriction>
</ClaimType>
```

#### <a name="password"></a>Parola

**Parola** kullanıcı girişi türü, kullanıcı tarafından girilen bir parolayı kaydetmek için kullanılır.

![Parolaile talep türünü kullanma](./media/claimsschema/password.png)

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
</ClaimType>
```

#### <a name="datetimedropdown"></a>DateTimeDropdown

**DateTimeDropdown** kullanıcı girişi türü, bir gün, ay ve yıl seçmek için açılır bırakma kümesi sağlamak için kullanılır. Minimum ve maksimum tarih değerlerini denetlemek için Yüklemler ve Yüklemler öğelerini kullanabilirsiniz. Daha fazla bilgi için, [Yüklemler ve Yüklemler'in](predicates.md) **tarih aralığını yapılandır** ın bölümüne bakın.

![Datetimedropdown ile talep türünü kullanma](./media/claimsschema/datetimedropdown.png)

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date Of Birth</DisplayName>
  <DataType>date</DataType>
  <UserHelpText>The date on which you were born.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
</ClaimType>
```

#### <a name="radiosingleselect"></a>RadioSingleSelect

**RadioSingleSelect** kullanıcı giriş türü, kullanıcının bir seçenek seçmesine olanak tanıyan radyo düğmeleri koleksiyonunu sağlamak için kullanılır.

![Radiodsingleselect ile talep türünü kullanma](./media/claimsschema/radiosingleselect.png)

```XML
<ClaimType Id="color">
  <DisplayName>Preferred color</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Blue" Value="Blue" SelectByDefault="false" />
    <Enumeration Text="Green " Value="Green" SelectByDefault="false" />
    <Enumeration Text="Orange" Value="Orange" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

#### <a name="dropdownsingleselect"></a>DropdownTekSeç

**DropdownSingleSelect** kullanıcı giriş türü, kullanıcının bir seçeneği seçmesini sağlayan açılır kutusağlamak için kullanılır.

![Dropdownsingleselect ile talep türünü kullanma](./media/claimsschema/dropdownsingleselect.png)

```XML
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

#### <a name="checkboxmultiselect"></a>Onay KutusuMultiSelect

**CheckboxMultiSelect** kullanıcı giriş türü, kullanıcının birden çok seçenek seçmesine olanak tanıyan bir onay kutusu koleksiyonu sağlamak için kullanılır.

![Onay kutusumultiselect ile talep türünü kullanma](./media/claimsschema/checkboxmultiselect.png)

```XML
<ClaimType Id="languages">
  <DisplayName>Languages you speak</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="English" Value="English" SelectByDefault="true" />
    <Enumeration Text="France " Value="France" SelectByDefault="false" />
    <Enumeration Text="Spanish" Value="Spanish" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

#### <a name="readonly"></a>Readonly

**Readonly** kullanıcı girişi türü, talep ve değeri görüntülemek için salt okunur bir alan sağlamak için kullanılır.

![Yalnızca readonly ile talep türünü kullanma](./media/claimsschema/readonly.png)

```XML
<ClaimType Id="membershipNumber">
  <DisplayName>Membership number</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your membership number (read only)</UserHelpText>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```


#### <a name="paragraph"></a>Paragraf

**Paragraf** kullanıcı giriş türü, yalnızca bir paragraf etiketinde metni gösteren bir alan sağlamak için kullanılır.  Örneğin, &lt;p&gt;&lt;metin&gt;/p . Kendi **Paragraph** kendine ileri `OutputClaim` teknik profil bir Paragraf kullanıcı giriş `Required` türü, öznitelik `false` (varsayılan) ayarlamak gerekir.

![Paragrafla talep türünü kullanma](./media/claimsschema/paragraph.png)

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>A claim responsible for holding response messages to send to the relying party</AdminHelpText>
  <UserHelpText>A claim responsible for holding response messages to send to the relying party</UserHelpText>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cannot sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
