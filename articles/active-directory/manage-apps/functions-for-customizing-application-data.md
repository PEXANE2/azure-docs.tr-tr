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
ms.openlocfilehash: 93b8387d4453a3d83bcce55c739548d914545f2f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75430079"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory’de Öznitelik Eşlemeleri için İfadeler Yazma
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
[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [BitAnd](#bitand) &nbsp;&nbsp;&nbsp;&nbsp; [CBool](#cbool) &nbsp;&nbsp;&nbsp;&nbsp; [Coalesce](#coalesce) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToBase64](#converttobase64) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToUTF8Hex](#converttoutf8hex) &nbsp;&nbsp;&nbsp;&nbsp; [Count](#count) &nbsp;&nbsp;&nbsp;&nbsp; [CStr](#cstr) &nbsp;&nbsp;&nbsp;&nbsp; [DateFromNum](#datefromnum) &nbsp;[FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Guid](#guid) &nbsp;&nbsp;&nbsp;&nbsp; [InStr](#instr) &nbsp;&nbsp;&nbsp;&nbsp; [IsNull](#isnull) &nbsp;&nbsp;&nbsp;&nbsp; [IsNullOrEmpty](#isnullorempty) &nbsp;&nbsp;&nbsp;&nbsp; [IsPresent](#ispresent) &nbsp;&nbsp;&nbsp;&nbsp; [IsString](#isstring) &nbsp;&nbsp;&nbsp;&nbsp; [Item](#item) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Left](#left) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; [NormalizeDiacritics](#normalizediacritics) [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [RemoveDuplicates](#removeduplicates) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [SelectUniqueValue](#selectuniquevalue)&nbsp;&nbsp;&nbsp;&nbsp; [SingleAppRoleAssignment](#singleapproleassignment)&nbsp;&nbsp;&nbsp;&nbsp; [Split](#split)&nbsp;&nbsp;&nbsp;&nbsp;[StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)&nbsp;&nbsp;&nbsp;&nbsp; [ToLower](#tolower)&nbsp;&nbsp;&nbsp;&nbsp; [ToUpper](#toupper)&nbsp;&nbsp;&nbsp;&nbsp; [Word](#word)

---
### <a name="append"></a>Ekle
**İşlev:**<br> Append(Source, Suffix)

**Açıklama:**<br> Kaynak dize değerini alır ve soneki, sonuna ekler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **suffix** |Gereklidir |Dize |Kaynak değerin sonuna istediğiniz dize. |

---
### <a name="bitand"></a>BitAnd
**İşlev:**<br> BitAnd (değer1, değer2)

**Açıklama:**<br> Bu işlev, her iki parametreyi de ikili gösterimine dönüştürür ve bir bit şu şekilde ayarlar:

0-değer1 ve değer2 içindeki karşılık gelen bitlerin biri veya her ikisi 0 ise                                                  
1-karşılık gelen bitlerin her ikisi de 1 ' dir.                                    

Diğer bir deyişle, her iki parametrenin de karşılık gelen bitlerinin 1 olduğu durumlar dışında her durumda 0 döndürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gereklidir |sayı |Değer2 ile birlikte olması gereken sayısal değer|
| **value2** |Gereklidir |sayı |Değer1 ile birlikte olması gereken sayısal değer|

**Örnek:**<br>
BitAnd (& HF, & HF7)                                                                                
11110111 ve 00000111 = 00000111, bitve 7 değerini, 00000111 ikili değerini döndürür

---
### <a name="cbool"></a>CBool
**İşlev:**<br> CBool (Ifade)

**Açıklama:**<br> CBool, değerlendirilen ifadeye göre Boole değeri döndürür. İfade sıfır olmayan bir değer olarak değerlendirilirse, CBool true değerini döndürür, aksi takdirde false döndürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gereklidir | expression | Herhangi bir geçerli ifade |

**Örnek:**<br>
CBool ([Attribute1] = [attribute2])                                                                    
Her iki öznitelik de aynı değere sahip olduğunda true döndürür.

---
### <a name="coalesce"></a>Coalesce
**İşlev:**<br> Birleşim (Source1, SOURCE2,..., defaultValue)

**Açıklama:**<br> NULL olmayan ilk kaynak değeri döndürür. Tüm bağımsız değişkenler NULL ve defaultValue varsa, defaultValue döndürülür. Tüm bağımsız değişkenler NULL ve defaultValue yoksa, birleşim NULL döndürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **... kaynak1 kaynakN** | Gereklidir | Dize |Gerekli, değişken sayısı. Genellikle kaynak nesneden özniteliğin adı. |
| **defaultValue** | İsteğe Bağlı | Dize | Tüm kaynak değerleri NULL olduğunda kullanılacak varsayılan değer. Boş bir dize olabilir ("").

---
### <a name="converttobase64"></a>ConvertToBase64
**İşlev:**<br> ConvertToBase64 (kaynak)

**Açıklama:**<br> ConvertToBase64 işlevi bir dizeyi Unicode Base64 dizesine dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |64 tabanına dönüştürülecek dize|

**Örnek:**<br>
ConvertToBase64 ("Hello World!")                                                                                                        
"Sablagwabebek Vacaadilevahiababkacea" döndürür

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**İşlev:**<br> ConvertToUTF8Hex (kaynak)

**Açıklama:**<br> ConvertToUTF8Hex işlevi bir dizeyi UTF8 onaltılık kodlanmış bir değere dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |UTF8 onaltılı olarak dönüştürülecek dize|

**Örnek:**<br>
ConvertToUTF8Hex ("Hello World!")                                                                                                         
48656C6C6F20776F726C6421 döndürür

---
### <a name="count"></a>Sayı
**İşlev:**<br> Count (öznitelik)

**Açıklama:**<br> Count işlevi, birden çok değerli bir öznitelikteki öğelerin sayısını döndürür

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **attribute** |Gereklidir |özniteliği |Sayılacak öğeleri olan çok değerli öznitelik|

---
### <a name="cstr"></a>CStr
**İşlev:**<br> CStr (değer)

**Açıklama:**<br> CStr işlevi bir değeri dize veri türüne dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gereklidir | sayısal, başvuru veya Boole | Sayısal bir değer, başvuru özniteliği veya Boole olabilir. |

**Örnek:**<br>
CStr ([DN])                                                            
"CN = ali, DC = contoso, DC = com" döndürür

---
### <a name="datefromnum"></a>Tarih Fromnum
**İşlev:**<br> DateFromNum (değer)

**Açıklama:**<br> DateFromNum işlevi, AD 'nin tarih biçimindeki bir değeri bir tarih saat türüne dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gereklidir | Tarih | Tarih saat türüne dönüştürülecek AD tarihi |

**Örnek:**<br>
DateFromNum ([lastLogonTimestamp])                                                                                                   
DateFromNum (129699324000000000)                                                            
2012-01-01 23:00:00 temsil eden bir tarih saat döndürür

---
### <a name="formatdatetime"></a>formatDateTime
**İşlev:**<br> FormatDateTime (kaynak, inputFormat, outputFormat)

**Açıklama:**<br> Bir tarih dizesini bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **inputFormat** |Gereklidir |Dize |Kaynak değeri beklenen biçimi. Desteklenen biçimler için bkz: [ https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx ](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Gereklidir |Dize |Çıkış tarih biçimi. |

---
### <a name="guid"></a>Guid
**İşlev:**<br> GUID ()

**Açıklama:**<br> İşlev GUID 'Si yeni bir rastgele GUID oluşturur

---
### <a name="instr"></a>InStr
**İşlev:**<br> InStr (değer1, değer2, Start, compareType)

**Açıklama:**<br> InStr işlevi bir dizedeki alt dizenin ilk oluşumunu bulur

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gereklidir |Dize |Aranacak dize |
| **value2** |Gereklidir |Dize |Bulunan dize |
| **start** |İsteğe Bağlı |Tamsayı |Alt dizeyi bulmak için başlangıç konumu|
| **compareType** |İsteğe Bağlı |Sabit Listesi |VbTextCompare veya vbBinaryCompare olabilir |

**Örnek:**<br>
InStr ("hızlı kahverengi Fox", "Quick")                                                                             
Evalues 'a 5

InStr ("yineleniyor", "e", 3, vbBinaryCompare)                                                                                  
7 olarak değerlendirilir

---
### <a name="isnull"></a>IsNull
**İşlev:**<br> IsNull (Ifade)

**Açıklama:**<br> İfade null olarak değerlendirilirse, IsNull işlevi true döndürür. Bir öznitelik için, bir null özniteliğin yokluğuna göre ifade edilir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gereklidir |expression |Değerlendirilecek ifade |

**Örnek:**<br>
IsNull ([displayName])                                                                                                
Öznitelik yoksa true döndürür

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**İşlev:**<br> IsNullOrEmpty (Ifade)

**Açıklama:**<br> İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true değerini döndürür. Özniteliği için, öznitelik yoksa veya varsa ancak boş bir dize ise, bu true olarak değerlendirilir.
Bu işlevin tersi ısun olarak adlandırılmıştır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gereklidir |expression |Değerlendirilecek ifade |

**Örnek:**<br>
IsNullOrEmpty ([displayName])                                               
Öznitelik yoksa veya boş bir dize ise true döndürür

---
### <a name="ispresent"></a>Olmasına
**İşlev:**<br> IsNullOrEmpty (Ifade)

**Açıklama:**<br> İfade null olmayan ve boş olmayan bir dize olarak değerlendirilirse, ıssun işlevi true değerini döndürür. Bu işlevin tersi IsNullOrEmpty olarak adlandırılmıştır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gereklidir |expression |Değerlendirilecek ifade |

**Örnek:**<br>
Anahtar (ısunu ([directManager]), [directManager], ısun ([skiplevelManager]), [skiplevelManager], ıssun ([Yönetmen]), [Yönetmen])

---
### <a name="isstring"></a>Isstrıng
**İşlev:**<br> IsString (Ifade)

**Açıklama:**<br> İfade bir dize türü olarak değerlendirilebiliyorsa, IsString işlevi true olarak değerlendirilir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gereklidir |expression |Değerlendirilecek ifade |

---
### <a name="item"></a>Öğe
**İşlev:**<br> Öğe (öznitelik, dizin)

**Açıklama:**<br> Item işlevi, birden çok değerli dize/öznitelikten bir öğe döndürür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **attribute** |Gereklidir |Öznitelik |Aranacak çok değerli öznitelik |
| **indeks** |Gereklidir |Tamsayı | Çoklu değerli dizedeki bir öğenin dizini|

**Örnek:**<br>
Öğe ([proxyAddresses], 1)

---
### <a name="join"></a>Birleştir
**İşlev:**<br> Birleştirme (ayırıcı, kaynak1, kaynak2,...)

**Açıklama:**<br> Birden çok birleştirebilirsiniz dışında join() Append() için benzer **kaynak** dize değerleri tek bir dize olarak ve her bir değeri ile ayrılmış bir **ayırıcı** dize.

Kaynak değerlerinden biri çok değerli bir öznitelik ise, bu öznitelikteki her değer, ayırıcı değeriyle ayırarak birlikte birleştirilir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Ayırıcı** |Gereklidir |Dize |Kaynak değerler bir dizeye birleştirilmiş, ayırmak için kullanılan dize. Olabilir "" ayırıcı gerekiyorsa. |
| **... kaynak1 kaynakN** |Değişken-sayısı gerekli |Dize |Değerleri birlikte birleştirilecek dize. |

---
### <a name="left"></a>Sol
**İşlev:**<br> Left (dize, NumChars)

**Açıklama:**<br> Left işlevi bir dizenin sol tarafında belirtilen sayıda karakteri döndürür. NumChars = 0 ise boş bir dize döndürür.
NumChars 0 <, giriş dizesi döndürür.
Dize null ise boş dize döndürün.
Dize Numchar 'lar içinde belirtilen sayıdan daha az karakter içeriyorsa, dize ile özdeş bir dize (yani, 1 parametresindeki tüm karakterleri içeren) döndürülür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **dize** |Gereklidir |Öznitelik | Karakterlerin döndürdüğü dize |
| **NumChars** |Gereklidir |Tamsayı | Dizenin başından (solda) döndürülecek karakter sayısını tanımlayan sayı|

**Örnek:**<br>
Sol ("John tikan", 3)                                                            
"Joh" döndürür

---
### <a name="mid"></a>Orta
**İşlev:**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklama:**<br> Kaynak değerin bir alt dizeyi döndürür. Bir alt dizenin yalnızca bazı kaynak dizeden karakterleri içeren bir dizedir.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle özniteliğin adı. |
| **start** |Gereklidir |integer |İçinde dizin **kaynak** alt dizenin başladığı dize. Dizedeki ilk karakter dizini 1 gerekir, ikinci karakter 2 dizine sahip ve benzeri. |
| **Uzunluğu** |Gereklidir |integer |Alt dizenin uzunluğu. Uzunluğu dışında sona ererse **kaynak** dize işlevi alt dizeyi döndürecektir **Başlat** sonuna kadar dizin **kaynak** dize. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**İşlev:**<br> NormalizeDiacritics(source)

**Açıklama:**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm Aksan karakteriyle değiştirildiğini aksanlı eşdeğer karakterlerle. Genellikle kullanıcı asıl adı, SAM hesap adlarını ve e-posta adresleri gibi çeşitli kullanıcı tanımlayıcılarını kullanılabilir yasal değerlerine adlarını ve soyadlarını aksanlı karakterleri (vurgu işaretleri) içeren dönüştürmek için kullanılır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize | Genellikle ad veya soyadı özniteliği. |

---
### <a name="not"></a>Not
**İşlev:**<br> Not(Source)

**Açıklama:**<br> Boole değeri çevirir **kaynak**. Varsa **kaynak** değeridir "*True*", döndürür "*False*". Aksi halde döndürür "*True*".

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Boole dizesi |Beklenen **kaynak** değerleri "true" veya "false" şeklindedir. |

---
### <a name="removeduplicates"></a>RemoveDuplicates
**İşlev:**<br> Removeyinelemelerini (öznitelik)

**Açıklama:**<br> Removeyinelemelerini işlevi, birden çok değerli dizeyi alır ve her değerin benzersiz olduğundan emin olur.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **attribute** |Gereklidir |Çoklu değerli öznitelik |Yinelenen öğeleri kaldırılacak birden çok değerli öznitelik|

**Örnek:**<br>
Removeyinelemelerini ([proxyAddresses])                                                                                                       
Tüm yinelenen değerlerin kaldırıldığı bir ayıklanmış proxyAddress özniteliği döndürür

---
### <a name="replace"></a>Değiştir
**İşlev:**<br> Değiştir (kaynak, oldValue, regexPattern, regexGroupName, replacementValue, replacementAttributeName, şablon)

**Açıklama:**<br>
Bir dize içindeki değerleri değiştirir. Sağlanan parametreler bağlı olarak farklı şekilde çalışır:

* Zaman **oldValue** ve **replacementValue** sağlanır:
  
  * **Kaynaktaki** tüm **OldValue** yinelemelerini **replacementvalue** ile değiştirir
* Zaman **oldValue** ve **şablon** sağlanır:
  
  * Tüm oluşumlarını değiştirir **oldValue** içinde **şablon** ile **kaynak** değeri
* **Regexmodel** ve **replacementvalue** sağlandığında:

  * İşlevi, **Regexmodel** öğesini **kaynak** dizeye uygular ve **replacementvalue** için dizeyi oluşturmak üzere Regex grup adlarını kullanabilirsiniz
* Zaman **regexPattern**, **regexGroupName**, **replacementValue** sağlanır:
  
  * İşlev, **kaynak** dizeye **regexmodel** uygular ve **regexgroupname** Ile eşleşen tüm değerleri **replacementvalue** ile değiştirir
* **Regexgroupname**, **replacementAttributeName** sağlandığında:
  
  * Varsa **kaynak** değeri **kaynak** döndürülür
  * **Kaynakta** bir değer varsa, Işlev **regexmodel** öğesini **kaynak** dizeye uygular ve **Regexgroupname** ile eşleşen tüm değerleri **replacementAttributeName** ile ilişkili değerle değiştirir

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle **kaynak** nesneden özniteliğin adı. |
| **oldValue** |İsteğe Bağlı |Dize |İçinde değiştirilecek değer **kaynak** veya **şablon**. |
| **regexPattern** |İsteğe Bağlı |Dize |Regex deseni olarak değiştirilecek değeri **kaynak**. Ya da **Replacementpropertyname** kullanıldığında, **replacementpropertyname**öğesinden değer çıkarmak için bir model. |
| **regexGroupName** |İsteğe Bağlı |Dize |Grup içinde adını **regexPattern**. Yalnızca **Replacementpropertyname** kullanıldığında, bu grubun değerini **replacementpropertyname**öğesinden **replacementvalue** olarak ayıklayacağız. |
| **replacementValue** |İsteğe Bağlı |Dize |Eski değiştirmek için yeni değeri. |
| **replacementAttributeName** |İsteğe Bağlı |Dize |Değiştirme değeri için kullanılacak özniteliğin adı |
| **Şablonu** |İsteğe Bağlı |Dize |**Şablon** değeri sağlandığında, şablon içinde **OldValue** aranacağı ve bunu **kaynak** değerle değiştirecek. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**İşlev:**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Açıklama:**<br> En az benzersiz değer oluşturma kuralları ifadeleri kullanılarak tanımlanmış olan iki bağımsız değişken gerektirir. İşlev, her kural değerlendirir ve ardından hedef uygulama/dizinindeki benzersizlik için oluşturulan değeri denetler. Bir döndürülen ilk benzersiz değeri bulundu. Tüm değerleri hedefte zaten varsa, giriş kalacakları ve nedeni denetim günlüklerinde günlüğe. Sağlanan bağımsız değişken sayısı için üst sınır yoktur.

> [!NOTE]
> - Bu üst düzey bir işlev, iç içe olamaz.
> - Bu işlev, eşleşen önceliği olan özniteliklere uygulanamaz.  
> - Bu işlev yalnızca girişi oluşturma için kullanılmak üzere tasarlanmıştır. Bir öznitelik ile kullanırken, ayarlayın **uygulamak eşleme** özelliğini **yalnızca nesne oluşturma sırasında**.
> - Bu işlev şu anda yalnızca "Workday for User sağlamasını Active Directory" için desteklenir. Diğer sağlama uygulamalarıyla birlikte kullanılamaz. 


**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **... uniqueValueRule1 uniqueValueRuleN** |En az 2 gerekli, en üst sınır. |Dize | Değerlendirilecek benzersiz değer oluşturma kurallarının listesi. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**İşlev:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklama:**<br> Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür. Approtaatamalar nesnesini tek bir rol adı dizesine dönüştürmek için bu işlev gereklidir. En iyi yöntem, tek seferde bir kullanıcıya yalnızca bir Approelatama atandığından emin olmak ve birden çok rol atanmamışsa döndürülen rol dizesinin tahmin edilebilir olamayacağını unutmayın. 

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **[appRoleAssignments]** |Gereklidir |Dize |**[appRoleAssignments]**  nesne. |

---
### <a name="split"></a>Bölme
**İşlev:**<br> Böl (kaynak, sınırlayıcı)

**Açıklama:**<br> Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |**Kaynak** güncelleştirmek için değer. |
| **ayırıcı** |Gereklidir |Dize |Dizeyi ayırmak için kullanılacak karakteri belirtir (örneğin: ",") |

---
### <a name="stripspaces"></a>StripSpaces
**İşlev:**<br> StripSpaces(source)

**Açıklama:**<br> Tüm alan kaldırır ("") karakter kaynak dizesi.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |**Kaynak** güncelleştirmek için değer. |

---
### <a name="switch"></a>Anahtar
**İşlev:**<br> Anahtar (kaynak, defaultValue, key1, value1, key2, value2,...)

**Açıklama:**<br> Zaman **kaynak** değeri eşleşen bir **anahtarı**, döndürür **değer** söz konusu **anahtar**. Varsa **kaynak** değeri döndürür bir anahtarı eşleşmiyor **defaultValue**.  **Anahtar** ve **değer** parametreleri her zaman çiftler halinde gelmelidir. İşlev her zaman bir çift sayı parametre bekliyor. İşlev, yönetici gibi başvuru öznitelikleri için kullanılmamalıdır. 

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |**Kaynak** güncelleştirmek için değer. |
| **defaultValue** |İsteğe Bağlı |Dize |Tüm anahtarları kaynak eşleşmediğinde kullanılacak varsayılan değeri. Boş bir dize olabilir (""). |
| **anahtar** |Gereklidir |Dize |**Anahtar** Karşılaştırılacak **kaynak** ile değeri. |
| **value** |Gereklidir |Dize |İçin değiştirme değeri **kaynak** anahtarıyla eşleşen. |

---
### <a name="tolower"></a>ToLower
**İşlev:**<br> ToLower (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="toupper"></a>ToUpper
**İşlev:**<br> ToUpper (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="word"></a>Word
**İşlev:**<br> Sözcük (dize, WordNumber, sınırlayıcılar)

**Açıklama:**<br> Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük numarasını açıklayan parametrelere göre dize içinde içerilen bir sözcük döndürür. Sınırlayıcıdaki karakterlerden biri tarafından ayrılan dizedeki her karakter dizesi, sözcük olarak tanımlanır:

Sayı < 1 ise boş bir dize döndürür.
Dize null ise, boş bir dize döndürür.
Dize sayı olan sözcüklerden daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan herhangi bir sözcük içermiyorsa, boş bir dize döndürülür.

**Parametreler:**<br> 

| Ad | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **dize** |Gereklidir |Çoklu değerli öznitelik |Bir sözcüğün döndürdüğü dize.|
| **WordNumber** |Gereklidir | Tamsayı | Hangi sözcük sayısının dönmesi gerektiğini tanımlayan sayı|
| **ıcı** |Gereklidir |Dize| Sözcükleri tanımlamak için kullanılması gereken sınırlayıcıları temsil eden bir dize|

**Örnek:**<br>
Word ("hızlı kahverengi Fox", 3, "")                                                                                       
"Kahverengi" döndürür

Word ("this, String! & çok sayıda ayırıcı içeriyor", 3, ",! & #")                                                                       
"İçerir" sonucunu döndürür

---

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Şerit bilinen etki alanı adı
Bir kullanıcı adı almak için bir kullanıcının e-posta bilinen etki alanı adından çıkarmanız gerekir. <br>
Örneğin, "contoso.com" etki alanı varsa, aşağıdaki ifade kullanabilirsiniz:

**İfade:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Giriş / Çıkış örneği:** <br>

* **Giriş** (posta): "john.doe@contoso.com"
* **Çıkış**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Kullanıcı adı için sabit bir sonek ekleme
Salesforce korumalı alan kullanıyorsanız, ek bir sonek eşitlemeden önce tüm kullanıcı adları eklemek gerekebilir.

**İfade:** <br>
`Append([userPrincipalName], ".test")`

**Örnek giriş/çıkış:** <br>

* **Giriş**: (userPrincipalName): "John.Doe@contoso.com"
* **ÇIKIŞ**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Birleştirme bölümleri adının ilk ve son tarafından kullanıcı diğer adı oluştur
Bir kullanıcı diğer kullanıcının adını, ilk 3 harf ve kullanıcının soyadını ilk 5 harfini yararlanarak oluşturmanız gerekiyor.

**İfade:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Örnek giriş/çıkış:** <br>

* **Giriş** (givenName): "John"
* **Giriş** (Soyadı): "Doe"
* **Çıkış**: "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Bir dizeden Aksanları Kaldır
Vurgu işaretlerinin içermeyen eşdeğer karakterlerle vurgu işaretleri içeren karakter değiştirmeniz gerekir.

**İfade:** <br>
NormalizeDiacritics([givenName])

**Örnek giriş/çıkış:** <br>

* **Giriş** (givenName): "Zoë"
* **Çıkış**: "Zoe"

### <a name="split-a-string-into-a-multi-valued-array"></a>Dizeyi çok değerli bir diziye bölme
Virgülle ayrılmış dizelerin bir listesini almanız ve bunları Salesforce 'ın PermissionSets özniteliği gibi bir çok değerli özniteliğe takılmış bir diziye bölmeniz gerekir. Bu örnekte, Azure AD 'de extensionAttribute5 'de izin kümelerinin bir listesi doldurulmuştur.

**İfade:** <br>
Böl ([extensionAttribute5], ",")

**Örnek giriş/çıkış:** <br>

* **Giriş** (extensionAttribute5): "PermissionSetOne, Izinleri Izinionsettingwo"
* **Çıkış**: ["Permissionsetone", "Permissionsettingwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Belirli bir biçimde bir dize olarak çıkış tarihi
Belirli bir biçimdeki bir SaaS uygulamasına tarihleri göndermek istediğiniz. <br>
Örneğin, tarihleri biçimlendirmek için ServiceNow istiyorsunuz.

**İfade:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Örnek giriş/çıkış:**

* **Giriş** (extensionAttribute1): "20150123105347.1Z"
* **ÇIKIŞ**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenekleri kümesi temel alınarak bir değeri değiştirin

Azure AD'de depolanan eyalet koduna göre kullanıcının saat dilimi tanımlamak gerekir. <br>
Önceden tanımlanmış seçeneklerden herhangi biri durum kodu eşleşmiyorsa, "Avustralya/Sidney" varsayılan değeri kullanın.

**İfade:** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Örnek giriş/çıkış:**

* **Giriş** (durum): "QLD"
* **Çıkış**: "Avustralya/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Normal ifade kullanarak karakterleri değiştirme
Normal ifade değeriyle eşleşen karakterler bulmanız ve bunları kaldırmanız gerekir.

**İfade:** <br>

Replace ([Mailtakma ad],, "[a-zA-Z_] *",, "",,)

**Örnek giriş/çıkış:**

* **Giriş** (mailtakma ad: "john_doe72"
* **Çıkış**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Oluşturulan userPrincipalName (UPN) değerini küçük harfe Dönüştür
Aşağıdaki örnekte, UPN değeri PreferredFirstName ve PreferredLastName kaynak alanları birleştirerek oluşturulur ve tüm karakterleri küçük harfe dönüştürmek için ToLower işlevi oluşturulan dize üzerinde çalışır. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Örnek giriş/çıkış:**

* **Giriş** (PreferredFirstName): "John"
* **Giriş** (PreferredLastName): "Smith"
* **Çıkış**: "john.smith@contoso.com"

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>UserPrincipalName (UPN) özniteliği için benzersiz bir değer oluşturur
Bağlı kullanıcının ilk adını, ikinci adı ve Soyadı, UPN özniteliği için değer atamadan önce hedef AD dizininde UPN özniteliği için bir değer ve kendi benzersizlik denetimi oluşturmanız gerekiyor.

**İfade:** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com"),
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Örnek giriş/çıkış:**

* **Giriş** (PreferredFirstName): "John"
* **Giriş** (PreferredLastName): "Smith"
* **Çıkış**: "John.Smith@contoso.com" değilse UPN değerini John.Smith@contoso.com dizininde zaten mevcut değil
* **Çıkış**: "J.Smith@contoso.com" değilse UPN değerini John.Smith@contoso.com dizinde zaten var.
* **Çıkış**: "Jo.Smith@contoso.com" Yukarıdaki iki UPN değeri dizinde zaten mevcutsa

### <a name="flow-mail-value-if-not-null-otherwise-flow-userprincipalname"></a>NULL değilse Flow posta değeri, aksi takdirde Flow userPrincipalName
Varsa posta özniteliğini akışa almak istiyorsunuz. Aksi takdirde, bunun yerine userPrincipalName değerini akmasını istersiniz.

**İfade:** <br>
`Coalesce([mail],[userPrincipalName])`

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): null
* **Giriş** (UserPrincipalName): "John.Doe@contoso.com"
* **ÇIKIŞ**: "John.Doe@contoso.com"

## <a name="related-articles"></a>İlgili makaleler
* [Kullanıcı sağlama/sağlamayı kaldırma SaaS uygulamaları için otomatik hale getirin](user-provisioning.md)
* [Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme](customize-application-attributes.md)
* [Kullanıcı sağlama için kapsam oluşturma filtresi](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](use-scim-to-provision-users-and-groups.md)
* [Hesap sağlama bildirimleri](user-provisioning.md)
* [SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
