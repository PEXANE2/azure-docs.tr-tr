---
title: Azure AD 'de öznitelik eşlemeleri için Ifadeler yazma
description: İfade eşlemeleri otomatik Azure Active Directory'de SaaS uygulama nesnelerin sağlama sırasında öznitelik değerleri kabul edilebilir bir biçime dönüştürmek için kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f1880a79f7fdb27b407ecb7ed1b761493fe850d
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74274017"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory'de öznitelik eşlemeleri için ifadeler yazma
Bir SaaS uygulaması için sağlama yapılandırdığınızda, belirtebilmeniz için öznitelik eşlemelerini türdeki bir ifade eşleme biridir. Bu, kullanıcılarınızın verileri fazla SaaS uygulaması için kabul edilebilir biçimlere dönüştürme olanak tanıyan bir betik gibi ifade yazmanız gerekir.

## <a name="syntax-overview"></a>Söz dizimi genel bakış
Öznitelik eşlemeleri için ifadeler sözdizimi Applications (VBA) işlevleri için Visual Basic reminiscent aşağıdaki gibidir.

* Tüm ifade İşlevler, bağımsız değişkenleri parantez içinde bir adından oluşur bakımından tanımlanmış olması gerekir: <br>
  *Fonksiyonadı (`<<argument 1>>`,`<<argument N>>`)*
* İçindeki diğer işlevleri iç içe. Örneğin: <br> *FunctionOne (FunctionTwo (`<<argument1>>`))*
* İşlevlere üç farklı türde bağımsız değişkenler geçirebilirsiniz:
  
  1. Öznitelik, köşeli ayraçlar içine alınmalıdır. Örneğin: [attributeName]
  2. Dize sabitleri çift tırnak içine alınmalıdır. Örneğin: "ABD"
  3. Diğer işlevler. Örneğin: FunctionOne (`<<argument1>>`, FunctionTwo (`<<argument2>>`))
* Dize sabitleri için bir ters eğik çizgi (\) veya tırnak işareti (") dizedeki gerekiyorsa, eğik çizgi (\) simgesiyle kaçınılmalıdır. Örneğin: "Şirket adı: \\" contoso\\""

## <a name="list-of-functions"></a>İşlevlerin listesi
[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [formatdatetime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [JOIN](#join) &nbsp;&nbsp;&nbsp;&nbsp; [orta](#mid) &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;[&nbsp;](#selectuniquevalue) &nbsp; [normalizediacritik](#normalizediacritics) [değil](#not) &nbsp;&nbsp;[&nbsp;&nbsp;](#replace) &nbsp;&nbsp; [Singleapprotaatama](#singleapproleassignment)&nbsp;&nbsp;&nbsp;&nbsp; [bölme](#split)&nbsp;&nbsp;&nbsp;&nbsp;[StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [anahtarı](#switch)&nbsp;&nbsp;&nbsp;&nbsp; [ToLower](#tolower)&nbsp;&nbsp;&nbsp;&nbsp; [ToUpper](#toupper)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

---
### <a name="append"></a>Ekle
**Çalışmayacaktır**<br> Append(Source, Suffix)

**Açıklaması**<br> Kaynak dize değerini alır ve soneki, sonuna ekler.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **önekini** |Gerekli |Dize |Kaynak değerin sonuna istediğiniz dize. |

---
### <a name="formatdatetime"></a>formatDateTime
**Çalışmayacaktır**<br> FormatDateTime (kaynak, inputFormat, outputFormat)

**Açıklaması**<br> Bir tarih dizesini bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **InPutFormat** |Gerekli |Dize |Kaynak değeri beklenen biçimi. Desteklenen biçimler için bkz. [https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Gerekli |Dize |Çıkış tarih biçimi. |

---
### <a name="join"></a>Birleştir
**Çalışmayacaktır**<br> Birleştirme (ayırıcı, kaynak1, kaynak2,...)

**Açıklaması**<br> JOIN (), birden çok **kaynak** dize değerini tek bir dizeye birleştirebildiğinden ve her bir değer bir **ayırıcı** dizeyle ayrılabilmesi dışında Append () ile benzerdir.

Kaynak değerlerinden biri çok değerli bir öznitelik ise, bu öznitelikteki her değer, ayırıcı değeriyle ayırarak birlikte birleştirilir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **ayırıcı** |Gerekli |Dize |Kaynak değerler bir dizeye birleştirilmiş, ayırmak için kullanılan dize. Olabilir "" ayırıcı gerekiyorsa. |
| **source1 ... Kaynakcen** |Değişken-sayısı gerekli |Dize |Değerleri birlikte birleştirilecek dize. |

---
### <a name="mid"></a>Orta
**Çalışmayacaktır**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklaması**<br> Kaynak değerin bir alt dizeyi döndürür. Bir alt dizenin yalnızca bazı kaynak dizeden karakterleri içeren bir dizedir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle özniteliğin adı. |
| **start** |Gerekli |integer |Alt dizenin başlaması gereken **kaynak** dizedeki dizin. Dizedeki ilk karakter dizini 1 gerekir, ikinci karakter 2 dizine sahip ve benzeri. |
| **uzunluklu** |Gerekli |integer |Alt dizenin uzunluğu. Uzunluk **kaynak** dizenin dışında biterse, işlev **Başlangıç** dizininden **kaynak** dizenin sonuna kadar alt dize döndürür. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Çalışmayacaktır**<br> NormalizeDiacritics(source)

**Açıklaması**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm Aksan karakteriyle değiştirildiğini aksanlı eşdeğer karakterlerle. Genellikle kullanıcı asıl adı, SAM hesap adlarını ve e-posta adresleri gibi çeşitli kullanıcı tanımlayıcılarını kullanılabilir yasal değerlerine adlarını ve soyadlarını aksanlı karakterleri (vurgu işaretleri) içeren dönüştürmek için kullanılır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize | Genellikle ad veya soyadı özniteliği. |

---
### <a name="not"></a>değil
**Çalışmayacaktır**<br> Not(Source)

**Açıklaması**<br> **Kaynağın**Boole değerini çevirir. **Kaynak** değer "*true*" ise, "*false*" döndürür. Aksi takdirde "*true*" döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Boole dizesi |Beklenen **kaynak** değerleri "true" veya "false" şeklindedir. |

---
### <a name="replace"></a>Değiştir
**Çalışmayacaktır**<br> Değiştir (kaynak, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, şablon)

**Açıklaması**<br>
Bir dize içindeki değerleri değiştirir. Sağlanan parametreler bağlı olarak farklı şekilde çalışır:

* **OldValue** ve **replacementvalue** sağlandığında:
  
  * **Kaynaktaki** tüm **OldValue** yinelemelerini **replacementvalue** ile değiştirir
* **OldValue** ve **şablon** sağlandığında:
  
  * **Şablondaki** **OldValue** tüm oluşumlarını **kaynak** değerle değiştirir
* **Regexmodel** ve **replacementvalue** sağlandığında:

  * İşlevi, **Regexmodel** öğesini **kaynak** dizeye uygular ve **replacementvalue** için dizeyi oluşturmak üzere Regex grup adlarını kullanabilirsiniz
* **Regexgroupname**, **replacementvalue** sağlandığında:
  
  * İşlev, **kaynak** dizeye **regexmodel** uygular ve **regexgroupname** Ile eşleşen tüm değerleri **replacementvalue** ile değiştirir
* **Regexgroupname**, **replacementAttributeName** sağlandığında:
  
  * **Kaynakta** değer yoksa, **kaynak** döndürülür
  * **Kaynakta** bir değer varsa, Işlev **regexmodel** öğesini **kaynak** dizeye uygular ve **Regexgroupname** ile eşleşen tüm değerleri **replacementAttributeName** ile ilişkili değerle değiştirir

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle **kaynak** nesneden özniteliğin adı. |
| **oldValue** |İsteğe Bağlı |Dize |**Kaynak** veya **şablonda**değiştirilmekte olan değer. |
| **Regexmodel** |İsteğe Bağlı |Dize |**Kaynakta**değiştirilmekte olan değer için Regex stili. Ya da **Replacementpropertyname** kullanıldığında, **replacementpropertyname**öğesinden değer çıkarmak için bir model. |
| **regexGroupName** |İsteğe Bağlı |Dize |**Regexmodel**içindeki grubun adı. Yalnızca **Replacementpropertyname** kullanıldığında, bu grubun değerini **replacementpropertyname**öğesinden **replacementvalue** olarak ayıklayacağız. |
| **replacementValue** |İsteğe Bağlı |Dize |Eski değiştirmek için yeni değeri. |
| **replacementAttributeName** |İsteğe Bağlı |Dize |Değiştirme değeri için kullanılacak özniteliğin adı |
| **şablonlarını** |İsteğe Bağlı |Dize |**Şablon** değeri sağlandığında, şablon içinde **OldValue** aranacağı ve bunu **kaynak** değerle değiştirecek. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Çalışmayacaktır**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Açıklaması**<br> En az benzersiz değer oluşturma kuralları ifadeleri kullanılarak tanımlanmış olan iki bağımsız değişken gerektirir. İşlev, her kural değerlendirir ve ardından hedef uygulama/dizinindeki benzersizlik için oluşturulan değeri denetler. Bir döndürülen ilk benzersiz değeri bulundu. Tüm değerleri hedefte zaten varsa, giriş kalacakları ve nedeni denetim günlüklerinde günlüğe. Sağlanan bağımsız değişken sayısı için üst sınır yoktur.

> [!NOTE]
> - Bu üst düzey bir işlev, iç içe olamaz.
> - Bu işlev, eşleşen önceliği olan özniteliklere uygulanamaz.  
> - Bu işlev yalnızca girişi oluşturma için kullanılmak üzere tasarlanmıştır. Bir özniteliğiyle birlikte kullanıldığında, **uygulamayı Uygula** özelliğini **yalnızca nesne oluşturma sırasında**olarak ayarlayın.
> - Bu işlev şu anda yalnızca "Workday for User sağlamasını Active Directory" için desteklenir. Diğer sağlama uygulamalarıyla birlikte kullanılamaz. 


**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **uniqueValueRule1 ... uniqueValueRuleN** |En az 2 gerekli, en üst sınır. |Dize | Değerlendirilecek benzersiz değer oluşturma kurallarının listesi. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Çalışmayacaktır**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklaması**<br> Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür. Approtaatamalar nesnesini tek bir rol adı dizesine dönüştürmek için bu işlev gereklidir. En iyi yöntem, tek seferde bir kullanıcıya yalnızca bir Approelatama atandığından emin olmak ve birden çok rol atanmamışsa döndürülen rol dizesinin tahmin edilebilir olamayacağını unutmayın. 

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **AppRoleAssignments** |Gerekli |Dize |**[Approtaatamalar]** nesnesi. |

---
### <a name="split"></a>Bölme
**Çalışmayacaktır**<br> Böl (kaynak, sınırlayıcı)

**Açıklaması**<br> Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |
| **ayırıcı** |Gerekli |Dize |Dizeyi ayırmak için kullanılacak karakteri belirtir (örneğin: ",") |

---
### <a name="stripspaces"></a>StripSpaces
**Çalışmayacaktır**<br> StripSpaces(source)

**Açıklaması**<br> Tüm alan kaldırır ("") karakter kaynak dizesi.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |

---
### <a name="switch"></a>Anahtar
**Çalışmayacaktır**<br> Anahtar (kaynak, defaultValue, key1, value1, key2, value2,...)

**Açıklaması**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtara eşleşmezse, **DefaultValue**döndürür.  **Anahtar** ve **değer** parametrelerinin her zaman çiftler halinde gelmesi gerekir. İşlev her zaman bir çift sayı parametre bekliyor.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |
| **Değerinin** |İsteğe Bağlı |Dize |Tüm anahtarları kaynak eşleşmediğinde kullanılacak varsayılan değeri. Boş bir dize olabilir (""). |
| **anahtar** |Gerekli |Dize |**Kaynak** değeri Karşılaştırılacak **anahtar** . |
| **value** |Gerekli |Dize |Anahtarla eşleşen **kaynak** için değiştirme değeri. |

---
### <a name="tolower"></a>ToLower
**Çalışmayacaktır**<br> ToLower (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="toupper"></a>ToUpper
**Çalışmayacaktır**<br> ToUpper (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Type | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Şerit bilinen etki alanı adı
Bir kullanıcı adı almak için bir kullanıcının e-posta bilinen etki alanı adından çıkarmanız gerekir. <br>
Örneğin, "contoso.com" etki alanı varsa, aşağıdaki ifade kullanabilirsiniz:

**İfadesini** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): "john.doe@contoso.com"
* **Çıkış**: "John. tikan"

### <a name="append-constant-suffix-to-user-name"></a>Kullanıcı adı için sabit bir sonek ekleme
Salesforce korumalı alan kullanıyorsanız, ek bir sonek eşitlemeden önce tüm kullanıcı adları eklemek gerekebilir.

**İfadesini** <br>
`Append([userPrincipalName], ".test")`

**Örnek giriş/çıkış:** <br>

* **Giriş**: (UserPrincipalName): "John.Doe@contoso.com"
* **Çıkış**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Birleştirme bölümleri adının ilk ve son tarafından kullanıcı diğer adı oluştur
Bir kullanıcı diğer kullanıcının adını, ilk 3 harf ve kullanıcının soyadını ilk 5 harfini yararlanarak oluşturmanız gerekiyor.

**İfadesini** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Örnek giriş/çıkış:** <br>

* **Giriş** ("John"): "John"
* **Giriş** (soyad): "tikan"
* **Çıkış**: "cantikan"

### <a name="remove-diacritics-from-a-string"></a>Bir dizeden Aksanları Kaldır
Vurgu işaretlerinin içermeyen eşdeğer karakterlerle vurgu işaretleri içeren karakter değiştirmeniz gerekir.

**İfadesini** <br>
NormalizeDiacritics([givenName])

**Örnek giriş/çıkış:** <br>

* **Giriş** (Zoë): ""
* **Çıkış**: "Bure"

### <a name="split-a-string-into-a-multi-valued-array"></a>Dizeyi çok değerli bir diziye bölme
Virgülle ayrılmış dizelerin bir listesini almanız ve bunları Salesforce 'ın PermissionSets özniteliği gibi bir çok değerli özniteliğe takılmış bir diziye bölmeniz gerekir. Bu örnekte, Azure AD 'de extensionAttribute5 'de izin kümelerinin bir listesi doldurulmuştur.

**İfadesini** <br>
Böl ([extensionAttribute5], ",")

**Örnek giriş/çıkış:** <br>

* **Giriş** (extensionAttribute5): "PermissionSetOne, Izinleri Izinionsettingwo"
* **Çıkış**: ["Permissionsetone", "Permissionsettingwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Belirli bir biçimde bir dize olarak çıkış tarihi
Belirli bir biçimdeki bir SaaS uygulamasına tarihleri göndermek istediğiniz. <br>
Örneğin, tarihleri biçimlendirmek için ServiceNow istiyorsunuz.

**İfadesini** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Örnek giriş/çıkış:**

* **Giriş** (extensionAttribute1): "20150123105347.1 z"
* **Çıkış**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenekleri kümesi temel alınarak bir değeri değiştirin

Azure AD'de depolanan eyalet koduna göre kullanıcının saat dilimi tanımlamak gerekir. <br>
Önceden tanımlanmış seçeneklerden herhangi biri durum kodu eşleşmiyorsa, "Avustralya/Sidney" varsayılan değeri kullanın.

**İfadesini** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Örnek giriş/çıkış:**

* **Giriş** (durum): "QLD"
* **Çıkış**: "Avustralya/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Normal ifade kullanarak karakterleri değiştirme
Normal ifade değeriyle eşleşen karakterler bulmanız ve bunları kaldırmanız gerekir.

**İfadesini** <br>

Replace ([Mailtakma ad],, "[a-zA-Z_] *",, "",,)

**Örnek giriş/çıkış:**

* **Giriş** (mailtakma ad: "john_doe72"
* **Çıkış**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Oluşturulan userPrincipalName (UPN) değerini küçük harfe Dönüştür
Aşağıdaki örnekte, UPN değeri PreferredFirstName ve PreferredLastName kaynak alanları birleştirerek oluşturulur ve tüm karakterleri küçük harfe dönüştürmek için ToLower işlevi oluşturulan dize üzerinde çalışır. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Örnek giriş/çıkış:**

* **Giriş** (preferredfirstname): "John"
* **Giriş** (preferredlastname): "Smith"
* **Çıkış**: "john.smith@contoso.com"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>UserPrincipalName (UPN) özniteliği için benzersiz bir değer oluşturur
Bağlı kullanıcının ilk adını, ikinci adı ve Soyadı, UPN özniteliği için değer atamadan önce hedef AD dizininde UPN özniteliği için bir değer ve kendi benzersizlik denetimi oluşturmanız gerekiyor.

**İfadesini** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com"),
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Örnek giriş/çıkış:**

* **Giriş** (preferredfirstname): "John"
* **Giriş** (preferredlastname): "Smith"
* **Çıkış**: John.Smith@contoso.com UPN değeri zaten dizinde yoksa "John.Smith@contoso.com"
* **Çıkış**: John.Smith@contoso.com UPN değeri zaten dizinde varsa "J.Smith@contoso.com"
* **Çıkış**: YUKARıDAKI iki UPN değeri dizinde zaten mevcutsa "Jo.Smith@contoso.com"

## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamalarına Kullanıcı sağlamasını/sağlamayı kaldırmayı otomatikleştirme](user-provisioning.md)
* [Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Kullanıcı hazırlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarının nasıl tümleştirileceği hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
