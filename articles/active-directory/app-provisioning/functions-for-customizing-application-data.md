---
title: Azure AD 'de öznitelik eşlemeleri için Ifadeler yazma
description: İfade eşlemeleri otomatik Azure Active Directory'de SaaS uygulama nesnelerin sağlama sırasında öznitelik değerleri kabul edilebilir bir biçime dönüştürmek için kullanmayı öğrenin.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd78c78a711b64c58290f09eb2ee52263375002f
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77522518"
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
[Append](#append) &nbsp;&nbsp;&nbsp;&nbsp; [BitAnd](#bitand) &nbsp;&nbsp;&nbsp;&nbsp; [CBool](#cbool) &nbsp;&nbsp;&nbsp;&nbsp; [Coalesce](#coalesce) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToBase64](#converttobase64) &nbsp;&nbsp;&nbsp;&nbsp; [ConvertToUTF8Hex](#converttoutf8hex) &nbsp;&nbsp;&nbsp;&nbsp; [Count](#count) &nbsp;&nbsp;&nbsp;&nbsp; [CStr](#cstr) &nbsp;&nbsp;&nbsp;&nbsp; [DateFromNum](#datefromnum) &nbsp;[FormatDateTime](#formatdatetime) &nbsp;&nbsp;&nbsp;&nbsp; [Guid](#guid) &nbsp;&nbsp;&nbsp;&nbsp; [InStr](#instr) &nbsp;&nbsp;&nbsp;&nbsp; [IsNull](#isnull) &nbsp;&nbsp;&nbsp;&nbsp; [IsNullOrEmpty](#isnullorempty) &nbsp;&nbsp;&nbsp;&nbsp; [IsPresent](#ispresent) &nbsp;&nbsp;&nbsp;&nbsp; [IsString](#isstring) &nbsp;&nbsp;&nbsp;&nbsp; [Item](#item) &nbsp;&nbsp;&nbsp;&nbsp; [Join](#join) &nbsp;&nbsp;&nbsp;&nbsp; [Left](#left) &nbsp;&nbsp;&nbsp;&nbsp; [Mid](#mid) &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; [NormalizeDiacritics](#normalizediacritics) [Not](#not) &nbsp;&nbsp;&nbsp;&nbsp; [NumFromDate](#numfromdate) &nbsp;&nbsp;&nbsp;&nbsp;[RemoveDuplicates](#removeduplicates) &nbsp;&nbsp;&nbsp;&nbsp; [Replace](#replace) &nbsp;&nbsp;&nbsp;&nbsp; [SelectUniqueValue](#selectuniquevalue)&nbsp;&nbsp;&nbsp;&nbsp; [SingleAppRoleAssignment](#singleapproleassignment)&nbsp;&nbsp;&nbsp;&nbsp; [Split](#split)&nbsp;&nbsp;&nbsp;&nbsp;[StripSpaces](#stripspaces) &nbsp;&nbsp;&nbsp;&nbsp; [Switch](#switch)&nbsp;&nbsp;&nbsp;&nbsp; [ToLower](#tolower)&nbsp;&nbsp;&nbsp;&nbsp; [ToUpper](#toupper)&nbsp;&nbsp;&nbsp;&nbsp; [Word](#word)

---
### <a name="append"></a>Ekle
**Çalışmayacaktır**<br> Append(Source, Suffix)

**Açıklaması**<br> Kaynak dize değerini alır ve soneki, sonuna ekler.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle kaynak nesneden özniteliğin adı. |
| **önekini** |Gerekli |String |Kaynak değerin sonuna istediğiniz dize. |

---
### <a name="bitand"></a>BitAnd
**Çalışmayacaktır**<br> BitAnd (değer1, değer2)

**Açıklaması**<br> Bu işlev, her iki parametreyi de ikili gösterimine dönüştürür ve bir bit şu şekilde ayarlar:

0-değer1 ve değer2 içindeki karşılık gelen bitlerin biri veya her ikisi 0 ise                                                  
1-karşılık gelen bitlerin her ikisi de 1 ' dir.                                    

Diğer bir deyişle, her iki parametrenin de karşılık gelen bitlerinin 1 olduğu durumlar dışında her durumda 0 döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gerekli |numaraları |Değer2 ile birlikte olması gereken sayısal değer|
| **value2** |Gerekli |numaraları |Değer1 ile birlikte olması gereken sayısal değer|

**Örnek:**<br>
BitAnd (& HF, & HF7)                                                                                
11110111 ve 00000111 = 00000111, bitve 7 değerini, 00000111 ikili değerini döndürür

---
### <a name="cbool"></a>CBool
**Çalışmayacaktır**<br> CBool (Ifade)

**Açıklaması**<br> CBool, değerlendirilen ifadeye göre Boole değeri döndürür. İfade sıfır olmayan bir değer olarak değerlendirilirse, CBool true değerini döndürür, aksi takdirde false döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli | expression | Herhangi bir geçerli ifade |

**Örnek:**<br>
CBool ([Attribute1] = [attribute2])                                                                    
Her iki öznitelik de aynı değere sahip olduğunda true döndürür.

---
### <a name="coalesce"></a>Coalesce
**Çalışmayacaktır**<br> Birleşim (Source1, SOURCE2,..., defaultValue)

**Açıklaması**<br> NULL olmayan ilk kaynak değeri döndürür. Tüm bağımsız değişkenler NULL ve defaultValue varsa, defaultValue döndürülür. Tüm bağımsız değişkenler NULL ve defaultValue yoksa, birleşim NULL döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **source1 ... Kaynakcen** | Gerekli | String |Gerekli, değişken sayısı. Genellikle kaynak nesneden özniteliğin adı. |
| **Değerinin** | İsteğe bağlı | String | Tüm kaynak değerleri NULL olduğunda kullanılacak varsayılan değer. Boş bir dize olabilir ("").

---
### <a name="converttobase64"></a>ConvertToBase64
**Çalışmayacaktır**<br> ConvertToBase64 (kaynak)

**Açıklaması**<br> ConvertToBase64 işlevi bir dizeyi Unicode Base64 dizesine dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |64 tabanına dönüştürülecek dize|

**Örnek:**<br>
ConvertToBase64 ("Hello World!")                                                                                                        
"Sablagwabebek Vacaadilevahiababkacea" döndürür

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Çalışmayacaktır**<br> ConvertToUTF8Hex (kaynak)

**Açıklaması**<br> ConvertToUTF8Hex işlevi bir dizeyi UTF8 onaltılık kodlanmış bir değere dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |UTF8 onaltılı olarak dönüştürülecek dize|

**Örnek:**<br>
ConvertToUTF8Hex ("Hello World!")                                                                                                         
48656C6C6F20776F726C6421 döndürür

---
### <a name="count"></a>Sayısı
**Çalışmayacaktır**<br> Count (öznitelik)

**Açıklaması**<br> Count işlevi, birden çok değerli bir öznitelikteki öğelerin sayısını döndürür

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |özniteliği |Sayılacak öğeleri olan çok değerli öznitelik|

---
### <a name="cstr"></a>CStr
**Çalışmayacaktır**<br> CStr (değer)

**Açıklaması**<br> CStr işlevi bir değeri dize veri türüne dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | sayısal, başvuru veya Boole | Sayısal bir değer, başvuru özniteliği veya Boole olabilir. |

**Örnek:**<br>
CStr ([DN])                                                            
"CN = ali, DC = contoso, DC = com" döndürür

---
### <a name="datefromnum"></a>Tarih Fromnum
**Çalışmayacaktır**<br> DateFromNum (değer)

**Açıklaması**<br> DateFromNum işlevi, AD 'nin tarih biçimindeki bir değeri bir tarih saat türüne dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | Tarih | Tarih saat türüne dönüştürülecek AD tarihi |

**Örnek:**<br>
DateFromNum ([lastLogonTimestamp])                                                                                                   
DateFromNum (129699324000000000)                                                            
2012-01-01 23:00:00 temsil eden bir tarih saat döndürür

---
### <a name="formatdatetime"></a>formatDateTime
**Çalışmayacaktır**<br> FormatDateTime (kaynak, inputFormat, outputFormat)

**Açıklaması**<br> Bir tarih dizesini bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle kaynak nesneden özniteliğin adı. |
| **InPutFormat** |Gerekli |String |Kaynak değeri beklenen biçimi. Desteklenen biçimler için bkz. [https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Gerekli |String |Çıkış tarih biçimi. |

---
### <a name="guid"></a>Guid
**Çalışmayacaktır**<br> GUID ()

**Açıklaması**<br> İşlev GUID 'Si yeni bir rastgele GUID oluşturur

---
### <a name="instr"></a>InStr
**Çalışmayacaktır**<br> InStr (değer1, değer2, Start, compareType)

**Açıklaması**<br> InStr işlevi bir dizedeki alt dizenin ilk oluşumunu bulur

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gerekli |String |Aranacak dize |
| **value2** |Gerekli |String |Bulunan dize |
| **start** |İsteğe bağlı |Tamsayı |Alt dizeyi bulmak için başlangıç konumu|
| **compareType** |İsteğe bağlı |Enum |VbTextCompare veya vbBinaryCompare olabilir |

**Örnek:**<br>
InStr ("hızlı kahverengi Fox", "Quick")                                                                             
Evalues 'a 5

InStr ("yineleniyor", "e", 3, vbBinaryCompare)                                                                                  
7 olarak değerlendirilir

---
### <a name="isnull"></a>IsNull
**Çalışmayacaktır**<br> IsNull (Ifade)

**Açıklaması**<br> İfade null olarak değerlendirilirse, IsNull işlevi true döndürür. Bir öznitelik için, bir null özniteliğin yokluğuna göre ifade edilir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |expression |Değerlendirilecek ifade |

**Örnek:**<br>
IsNull ([displayName])                                                                                                
Öznitelik yoksa true döndürür

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**Çalışmayacaktır**<br> IsNullOrEmpty (Ifade)

**Açıklaması**<br> İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true değerini döndürür. Özniteliği için, öznitelik yoksa veya varsa ancak boş bir dize ise, bu true olarak değerlendirilir.
Bu işlevin tersi ısun olarak adlandırılmıştır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |expression |Değerlendirilecek ifade |

**Örnek:**<br>
IsNullOrEmpty ([displayName])                                               
Öznitelik yoksa veya boş bir dize ise true döndürür

---
### <a name="ispresent"></a>Olmasına
**Çalışmayacaktır**<br> Isun (Ifade)

**Açıklaması**<br> İfade null olmayan ve boş olmayan bir dize olarak değerlendirilirse, ıssun işlevi true değerini döndürür. Bu işlevin tersi IsNullOrEmpty olarak adlandırılmıştır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |expression |Değerlendirilecek ifade |

**Örnek:**<br>
Anahtar (ısunu ([directManager]), [directManager], ısun ([skiplevelManager]), [skiplevelManager], ıssun ([Yönetmen]), [Yönetmen])

---
### <a name="isstring"></a>Isstrıng
**Çalışmayacaktır**<br> IsString (Ifade)

**Açıklaması**<br> İfade bir dize türü olarak değerlendirilebiliyorsa, IsString işlevi true olarak değerlendirilir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |expression |Değerlendirilecek ifade |

---
### <a name="item"></a>Öğe
**Çalışmayacaktır**<br> Öğe (öznitelik, dizin)

**Açıklaması**<br> Item işlevi, birden çok değerli dize/öznitelikten bir öğe döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |Öznitelik |Aranacak çok değerli öznitelik |
| **indeks** |Gerekli |Tamsayı | Çoklu değerli dizedeki bir öğenin dizini|

**Örnek:**<br>
Öğe ([proxyAddresses], 1)

---
### <a name="join"></a>Birleştir
**Çalışmayacaktır**<br> Birleştirme (ayırıcı, kaynak1, kaynak2,...)

**Açıklaması**<br> JOIN (), birden çok **kaynak** dize değerini tek bir dizeye birleştirebildiğinden ve her bir değer bir **ayırıcı** dizeyle ayrılabilmesi dışında Append () ile benzerdir.

Kaynak değerlerinden biri çok değerli bir öznitelik ise, bu öznitelikteki her değer, ayırıcı değeriyle ayırarak birlikte birleştirilir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ayırıcı** |Gerekli |String |Kaynak değerler bir dizeye birleştirilmiş, ayırmak için kullanılan dize. Olabilir "" ayırıcı gerekiyorsa. |
| **source1 ... Kaynakcen** |Değişken-sayısı gerekli |String |Değerleri birlikte birleştirilecek dize. |

---
### <a name="left"></a>Tarafta
**Çalışmayacaktır**<br> Left (dize, NumChars)

**Açıklaması**<br> Left işlevi bir dizenin sol tarafında belirtilen sayıda karakteri döndürür. NumChars = 0 ise boş bir dize döndürür.
NumChars 0 <, giriş dizesi döndürür.
Dize null ise boş dize döndürün.
Dize Numchar 'lar içinde belirtilen sayıdan daha az karakter içeriyorsa, dize ile özdeş bir dize (yani, 1 parametresindeki tüm karakterleri içeren) döndürülür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Dizisinde** |Gerekli |Öznitelik | Karakterlerin döndürdüğü dize |
| **NumChars** |Gerekli |Tamsayı | Dizenin başından (solda) döndürülecek karakter sayısını tanımlayan sayı|

**Örnek:**<br>
Sol ("John tikan", 3)                                                            
"Joh" döndürür

---
### <a name="mid"></a>Orta
**Çalışmayacaktır**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklaması**<br> Kaynak değerin bir alt dizeyi döndürür. Bir alt dizenin yalnızca bazı kaynak dizeden karakterleri içeren bir dizedir.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle özniteliğin adı. |
| **start** |Gerekli |integer |Alt dizenin başlaması gereken **kaynak** dizedeki dizin. Dizedeki ilk karakter dizini 1 gerekir, ikinci karakter 2 dizine sahip ve benzeri. |
| **uzunluklu** |Gerekli |integer |Alt dizenin uzunluğu. Uzunluk **kaynak** dizenin dışında biterse, işlev **Başlangıç** dizininden **kaynak** dizenin sonuna kadar alt dize döndürür. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Çalışmayacaktır**<br> NormalizeDiacritics(source)

**Açıklaması**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm Aksan karakteriyle değiştirildiğini aksanlı eşdeğer karakterlerle. Genellikle kullanıcı asıl adı, SAM hesap adlarını ve e-posta adresleri gibi çeşitli kullanıcı tanımlayıcılarını kullanılabilir yasal değerlerine adlarını ve soyadlarını aksanlı karakterleri (vurgu işaretleri) içeren dönüştürmek için kullanılır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String | Genellikle ad veya soyadı özniteliği. |

---
### <a name="not"></a>değil
**Çalışmayacaktır**<br> Not(Source)

**Açıklaması**<br> **Kaynağın**Boole değerini çevirir. **Kaynak** değer "*true*" ise, "*false*" döndürür. Aksi takdirde "*true*" döndürür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Boole dizesi |Beklenen **kaynak** değerleri "true" veya "false" şeklindedir. |

---
### <a name="numfromdate"></a>NumFromDate
**Çalışmayacaktır**<br> NumFromDate (değer)

**Açıklaması**<br> NumFromDate işlevi, bir DateTime değerini [AccountExpires](https://docs.microsoft.com/windows/win32/adschema/a-accountexpires)gibi öznitelikleri ayarlamak için gereken Active Directory biçime dönüştürür. Workday ve başarılı faktörlerdeki bulut HR uygulamalarından alınan DateTime değerlerini eşdeğer AD gösterimlerine dönüştürmek için bu işlevi kullanın. 

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | String | Desteklenen biçimdeki tarih saat dizesi. Desteklenen biçimler için bkz. https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx. |

**Örnek:**<br>
* Workday örneği <br>
  AD 'nin *2020-12-31-08:00* Ile *AccountExpires* alanında olan Workday 'den *sözleşmeli tenddate* özniteliğini eşlemek istediğinizi varsayarsak, bu işlevi kullanabilir ve saat dilimi sapmasını yerel ayarınızdan eşleşecek şekilde değiştirebilirsiniz. 
  `NumFromDate(Join("", FormatDateTime([ContractEndDate], "yyyy-MM-ddzzz", "yyyy-MM-dd"), "T23:59:59-08:00"))`

* Başarılı etken örneği <br>
  AD içinde *M/d/yyyy HH: mm: ss TT* Ile *AccountExpires* alanı biçimindeki *EndDate* özniteliğini başarılı bir şekilde eşlemek istediğinizi varsayarsak, bu işlevi kullanabilir ve saat dilimi sapmasını yerel ayarınızdan eşleşecek şekilde değiştirebilirsiniz.
  `NumFromDate(Join("",FormatDateTime([endDate],"M/d/yyyy hh:mm:ss tt","yyyy-MM-dd"),"T23:59:59-08:00"))`


---
### <a name="removeduplicates"></a>RemoveDuplicates
**Çalışmayacaktır**<br> Removeyinelemelerini (öznitelik)

**Açıklaması**<br> Removeyinelemelerini işlevi, birden çok değerli dizeyi alır ve her değerin benzersiz olduğundan emin olur.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |Çoklu değerli öznitelik |Yinelenen öğeleri kaldırılacak birden çok değerli öznitelik|

**Örnek:**<br>
Removeyinelemelerini ([proxyAddresses])                                                                                                       
Tüm yinelenen değerlerin kaldırıldığı bir ayıklanmış proxyAddress özniteliği döndürür

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

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle **kaynak** nesneden özniteliğin adı. |
| **oldValue** |İsteğe bağlı |String |**Kaynak** veya **şablonda**değiştirilmekte olan değer. |
| **Regexmodel** |İsteğe bağlı |String |**Kaynakta**değiştirilmekte olan değer için Regex stili. Ya da **Replacementpropertyname** kullanıldığında, **replacementpropertyname**öğesinden değer çıkarmak için bir model. |
| **regexGroupName** |İsteğe bağlı |String |**Regexmodel**içindeki grubun adı. Yalnızca **Replacementpropertyname** kullanıldığında, bu grubun değerini **replacementpropertyname**öğesinden **replacementvalue** olarak ayıklayacağız. |
| **replacementValue** |İsteğe bağlı |String |Eski değiştirmek için yeni değeri. |
| **replacementAttributeName** |İsteğe bağlı |String |Değiştirme değeri için kullanılacak özniteliğin adı |
| **şablonlarını** |İsteğe bağlı |String |**Şablon** değeri sağlandığında, şablon içinde **OldValue** aranacağı ve bunu **kaynak** değerle değiştirecek. |

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

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **uniqueValueRule1 ... uniqueValueRuleN** |En az 2 gerekli, en üst sınır. |String | Değerlendirilecek benzersiz değer oluşturma kurallarının listesi. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Çalışmayacaktır**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklaması**<br> Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür. Approtaatamalar nesnesini tek bir rol adı dizesine dönüştürmek için bu işlev gereklidir. En iyi yöntem, tek seferde bir kullanıcıya yalnızca bir Approelatama atandığından emin olmak ve birden çok rol atanmamışsa döndürülen rol dizesinin tahmin edilebilir olamayacağını unutmayın. 

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **AppRoleAssignments** |Gerekli |String |**[Approtaatamalar]** nesnesi. |

---
### <a name="split"></a>Bölme
**Çalışmayacaktır**<br> Böl (kaynak, sınırlayıcı)

**Açıklaması**<br> Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Güncelleştirilecek **kaynak** değeri. |
| **ayırıcı** |Gerekli |String |Dizeyi ayırmak için kullanılacak karakteri belirtir (örneğin: ",") |

---
### <a name="stripspaces"></a>StripSpaces
**Çalışmayacaktır**<br> StripSpaces(source)

**Açıklaması**<br> Tüm alan kaldırır ("") karakter kaynak dizesi.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Güncelleştirilecek **kaynak** değeri. |

---
### <a name="switch"></a>Anahtar
**Çalışmayacaktır**<br> Anahtar (kaynak, defaultValue, key1, value1, key2, value2,...)

**Açıklaması**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtara eşleşmezse, **DefaultValue**döndürür.  **Anahtar** ve **değer** parametrelerinin her zaman çiftler halinde gelmesi gerekir. İşlev her zaman bir çift sayı parametre bekliyor. İşlev, yönetici gibi başvuru öznitelikleri için kullanılmamalıdır. 

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Güncelleştirilecek **kaynak** değeri. |
| **Değerinin** |İsteğe bağlı |String |Tüm anahtarları kaynak eşleşmediğinde kullanılacak varsayılan değeri. Boş bir dize olabilir (""). |
| **anahtar** |Gerekli |String |**Kaynak** değeri Karşılaştırılacak **anahtar** . |
| **value** |Gerekli |String |Anahtarla eşleşen **kaynak** için değiştirme değeri. |

---
### <a name="tolower"></a>toLower
**Çalışmayacaktır**<br> ToLower (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle kaynak nesneden özniteliğin adı |
| **ayarı** |İsteğe bağlı |String |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="toupper"></a>toUpper
**Çalışmayacaktır**<br> ToUpper (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |String |Genellikle kaynak nesneden özniteliğin adı. |
| **ayarı** |İsteğe bağlı |String |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="word"></a>Word
**Çalışmayacaktır**<br> Sözcük (dize, WordNumber, sınırlayıcılar)

**Açıklaması**<br> Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük numarasını açıklayan parametrelere göre dize içinde içerilen bir sözcük döndürür. Sınırlayıcıdaki karakterlerden biri tarafından ayrılan dizedeki her karakter dizesi, sözcük olarak tanımlanır:

Sayı < 1 ise boş bir dize döndürür.
Dize null ise, boş bir dize döndürür.
Dize sayı olan sözcüklerden daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan herhangi bir sözcük içermiyorsa, boş bir dize döndürülür.

**Parametrelere**<br> 

| Name | Gerekli / yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Dizisinde** |Gerekli |Çoklu değerli öznitelik |Bir sözcüğün döndürdüğü dize.|
| **WordNumber** |Gerekli | Tamsayı | Hangi sözcük sayısının dönmesi gerektiğini tanımlayan sayı|
| **ıcı** |Gerekli |String| Sözcükleri tanımlamak için kullanılması gereken sınırlayıcıları temsil eden bir dize|

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

### <a name="flow-mail-value-if-not-null-otherwise-flow-userprincipalname"></a>NULL değilse Flow posta değeri, aksi takdirde Flow userPrincipalName
Varsa posta özniteliğini akışa almak istiyorsunuz. Aksi takdirde, bunun yerine userPrincipalName değerini akmasını istersiniz.

**İfadesini** <br>
`Coalesce([mail],[userPrincipalName])`

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): null
* **Giriş** (UserPrincipalName): "John.Doe@contoso.com"
* **Çıkış**: "John.Doe@contoso.com"

## <a name="related-articles"></a>İlgili makaleler
* [SaaS uygulamalarına Kullanıcı sağlamasını/sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md)
* [Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirme](../app-provisioning/customize-application-attributes.md)
* [Kullanıcı hazırlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](../app-provisioning/use-scim-to-provision-users-and-groups.md)
* [Hesap sağlama bildirimleri](../app-provisioning/user-provisioning.md)
* [SaaS uygulamalarının nasıl tümleştirileceği hakkında öğreticiler listesi](../saas-apps/tutorial-list.md)
