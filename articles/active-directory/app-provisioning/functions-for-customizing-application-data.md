---
title: Azure Active Directory öznitelik eşlemeleri için ifadeler yazma
description: Azure Active Directory ' de SaaS uygulama nesnelerinin otomatik sağlanması sırasında öznitelik değerlerini kabul edilebilir bir biçime dönüştürmek için ifade eşlemelerini nasıl kullanacağınızı öğrenin.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mimart
ms.openlocfilehash: c8573f9151ac59178b19bbf354da43990405b3e0
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82593701"
---
# <a name="how-to-write-expressions-for-attribute-mappings-in-azure-ad"></a>Nasıl yapılır: Azure AD 'de öznitelik eşlemeleri için ifadeler yazma

Sağlamayı bir SaaS uygulamasına yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemelerinin türlerinden biri bir ifade eşlemedir. Bunlar için, kullanıcılarınızın verilerini SaaS uygulaması için daha kabul edilebilir biçimlere dönüştürmenizi sağlayan bir betik benzeri ifade yazmalısınız.

## <a name="syntax-overview"></a>Sözdizimine genel bakış

Öznitelik eşlemeleri için Ifadeler söz dizimi Visual Basic for Applications (VBA) işlevlerinin bir rekidir.

* İfadenin tamamı, parantez içindeki bağımsız değişkenlerin ardında yer aldığı bir addan oluşan işlevler bakımından tanımlanmalıdır: <br>
  *Fonksiyonadı (`<<argument 1>>`,`<<argument N>>`)*
* İşlevleri birbirlerine iç içe yerleştirebilirsiniz. Örneğin: <br> *FunctionOne (FunctionTwo (`<<argument1>>`))*
* İşlevlere üç farklı türde bağımsız değişken geçirebilirsiniz:
  
  1. Köşeli ayraçlar içine alınması gereken öznitelikler. Örneğin: [attributeName]
  2. Çift tırnak içine alınması gereken dize sabitleri. Örneğin: "Birleşik Devletler"
  3. Diğer Işlevler. Örneğin: FunctionOne (`<<argument1>>`, functiontwo (`<<argument2>>`))
* Dize sabitleri için, dizede bir ters eğik çizgi (\) veya tırnak işareti (") gerekiyorsa, ters eğik çizgi (\) simgesiyle atlanmalıdır. Örneğin: "Şirket adı: \\" contoso\\""

## <a name="list-of-functions"></a>Işlevlerin listesi

[Append](#append) &nbsp; [Coalesce](#coalesce) &nbsp; [IIF](#iif) [ConvertToBase64](#converttobase64) &nbsp; [Replace](#replace) [Not](#not) [ConvertToUTF8Hex](#converttoutf8hex) &nbsp; [IsNullOrEmpty](#isnullorempty) [Count](#count) &nbsp; [Item](#item) [Left](#left) [IsNull](#isnull) [Join](#join) [Mid](#mid) [Guid](#guid) [BitAnd](#bitand) &nbsp; [CBool](#cbool) &nbsp; &nbsp; [CStr](#cstr) &nbsp; [RemoveDuplicates](#removeduplicates) [FormatDateTime](#formatdatetime) &nbsp; [IsPresent](#ispresent) [IsString](#isstring) [InStr](#instr) [DateFromNum](#datefromnum) &nbsp; [NormalizeDiacritics](#normalizediacritics) [SelectUniqueValue](#selectuniquevalue) bitand &nbsp; &nbsp; CBool &nbsp; JOIN ConvertToBase64 &nbsp; ConvertToUTF8Hex Count CStr &nbsp; datefromnum &nbsp; FormatDateTime &nbsp;Guid IIF instr &nbsp; IsNull IsNullOrEmpty &nbsp; issun &nbsp; IsString &nbsp; öğesi birleştirme &nbsp; sol &nbsp; orta &nbsp; normalizediactik Not &nbsp; removeyinelemelerini &nbsp; değiştirme selectuniquevalue &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [Switch](#switch) [Split](#split)&nbsp; [Word](#word) [SingleAppRoleAssignment](#singleapproleassignment) &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) [ToLower](#tolower) [ToUpper](#toupper) Singleapprotaatama&nbsp; Split &nbsp; StripSpaces anahtarı&nbsp; ToUpper&nbsp; Word&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

---
### <a name="append"></a>Ekle

**Çalışmayacaktır**<br> Append (kaynak, sonek)

**Açıklama:**<br> Bir kaynak dize değeri alır ve son eki bunun sonuna ekler.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **önekini** |Gerekli |Dize |Kaynak değerin sonuna eklemek istediğiniz dize. |

---
### <a name="bitand"></a>BitAnd
**Çalışmayacaktır**<br> BitAnd (değer1, değer2)

**Açıklama:**<br> Bu işlev, her iki parametreyi de ikili gösterimine dönüştürür ve bir bit şu şekilde ayarlar:

0-değer1 ve değer2 içindeki karşılık gelen bitlerin biri veya her ikisi 0 ise                                                  
1-karşılık gelen bitlerin her ikisi de 1 ' dir.                                    

Diğer bir deyişle, her iki parametrenin de karşılık gelen bitlerinin 1 olduğu durumlar dışında her durumda 0 döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gerekli |sayı |Değer2 ile birlikte olması gereken sayısal değer|
| **value2** |Gerekli |sayı |Değer1 ile birlikte olması gereken sayısal değer|

**Örneğinde**<br>
BitAnd (&HF, &HF7)                                                                                
11110111 ve 00000111 = 00000111, bitve 7 değerini, 00000111 ikili değerini döndürür

---
### <a name="cbool"></a>CBool
**Çalışmayacaktır**<br> CBool (Ifade)

**Açıklama:**<br> CBool, değerlendirilen ifadeye göre Boole değeri döndürür. İfade sıfır olmayan bir değer olarak değerlendirilirse, CBool true değerini döndürür, aksi takdirde false döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli | ifade | Herhangi bir geçerli ifade |

**Örneğinde**<br>
CBool ([Attribute1] = [attribute2])                                                                    
Her iki öznitelik de aynı değere sahip olduğunda true döndürür.

---
### <a name="coalesce"></a>Coalesce
**Çalışmayacaktır**<br> Birleşim (Source1, SOURCE2,..., defaultValue)

**Açıklama:**<br> NULL olmayan ilk kaynak değeri döndürür. Tüm bağımsız değişkenler NULL ve defaultValue varsa, defaultValue döndürülür. Tüm bağımsız değişkenler NULL ve defaultValue yoksa, birleşim NULL döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **source1 ... Kaynakcen** | Gerekli | Dize |Gerekli, değişken sayısı. Genellikle kaynak nesneden özniteliğin adı. |
| **Değerinin** | İsteğe Bağlı | Dize | Tüm kaynak değerleri NULL olduğunda kullanılacak varsayılan değer. Boş dize ("") olabilir.

---
### <a name="converttobase64"></a>ConvertToBase64
**Çalışmayacaktır**<br> ConvertToBase64 (kaynak)

**Açıklama:**<br> ConvertToBase64 işlevi bir dizeyi Unicode Base64 dizesine dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |64 tabanına dönüştürülecek dize|

**Örneğinde**<br>
ConvertToBase64 ("Hello World!")                                                                                                        
"Sablagwabebek Vacaadilevahiababkacea" döndürür

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Çalışmayacaktır**<br> ConvertToUTF8Hex (kaynak)

**Açıklama:**<br> ConvertToUTF8Hex işlevi bir dizeyi UTF8 onaltılık kodlanmış bir değere dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |UTF8 onaltılı olarak dönüştürülecek dize|

**Örneğinde**<br>
ConvertToUTF8Hex ("Hello World!")                                                                                                         
48656C6C6F20776F726C6421 döndürür

---
### <a name="count"></a>Sayı
**Çalışmayacaktır**<br> Count (öznitelik)

**Açıklama:**<br> Count işlevi, birden çok değerli bir öznitelikteki öğelerin sayısını döndürür

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |özniteliği |Sayılacak öğeleri olan çok değerli öznitelik|

---
### <a name="cstr"></a>CStr
**Çalışmayacaktır**<br> CStr (değer)

**Açıklama:**<br> CStr işlevi bir değeri dize veri türüne dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **deeri** |Gerekli | sayısal, başvuru veya Boole | Sayısal bir değer, başvuru özniteliği veya Boole olabilir. |

**Örneğinde**<br>
CStr ([DN])                                                            
"CN = ali, DC = contoso, DC = com" döndürür

---
### <a name="datefromnum"></a>Tarih Fromnum
**Çalışmayacaktır**<br> DateFromNum (değer)

**Açıklama:**<br> DateFromNum işlevi, AD 'nin tarih biçimindeki bir değeri bir tarih saat türüne dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **deeri** |Gerekli | Tarih | Tarih saat türüne dönüştürülecek AD tarihi |

**Örneğinde**<br>
DateFromNum ([lastLogonTimestamp])                                                                                                   
DateFromNum (129699324000000000)                                                            
2012-01-01 23:00:00 temsil eden bir tarih saat döndürür

---
### <a name="formatdatetime"></a>FormatDateTime
**Çalışmayacaktır**<br> FormatDateTime (kaynak, InPutFormat, outputFormat)

**Açıklama:**<br> Bir biçimden tarih dizesi alır ve onu farklı bir biçime dönüştürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **InPutFormat** |Gerekli |Dize |Kaynak değerinin biçimi bekleniyordu. Desteklenen biçimler için bkz [https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx).. |
| **outputFormat** |Gerekli |Dize |Çıkış tarihinin biçimi. |

---
### <a name="guid"></a>Guid
**Çalışmayacaktır**<br> GUID ()

**Açıklama:**<br> İşlev GUID 'Si yeni bir rastgele GUID oluşturur

---
### <a name="iif"></a>Mayan
**Çalışmayacaktır**<br> IıF (koşul, valueIfTrue, valueIfFalse)

**Açıklama:**<br> IıF işlevi, belirli bir koşula göre olası bir değer kümesinden birini döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **koşul** |Gerekli |Değişken veya Ifade |Doğru veya yanlış olarak değerlendirilebilen herhangi bir değer veya ifade. |
| **valueIfTrue** |Gerekli |Değişken veya dize | Koşul true olarak değerlendirilirse döndürülen değer. |
| **valueIfFalse** |Gerekli |Değişken veya dize |Koşul false olarak değerlendirilirse, döndürülen değer.|

**Örneğinde**<br>
IıF ([Country] = "USA", [Ülke], [departman])

---
### <a name="instr"></a>InStr
**Çalışmayacaktır**<br> InStr (değer1, değer2, Start, compareType)

**Açıklama:**<br> InStr işlevi bir dizedeki alt dizenin ilk oluşumunu bulur

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **value1** |Gerekli |Dize |Aranacak dize |
| **value2** |Gerekli |Dize |Bulunan dize |
| **başından** |İsteğe Bağlı |Tamsayı |Alt dizeyi bulmak için başlangıç konumu|
| **compareType** |İsteğe Bağlı |Sabit Listesi |VbTextCompare veya vbBinaryCompare olabilir |

**Örneğinde**<br>
InStr ("hızlı kahverengi Fox", "Quick")                                                                             
Evalues 'a 5

InStr ("yineleniyor", "e", 3, vbBinaryCompare)                                                                                  
7 olarak değerlendirilir

---
### <a name="isnull"></a>IsNull
**Çalışmayacaktır**<br> IsNull (Ifade)

**Açıklama:**<br> İfade null olarak değerlendirilirse, IsNull işlevi true döndürür. Bir öznitelik için, bir null özniteliğin yokluğuna göre ifade edilir.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |ifade |Değerlendirilecek ifade |

**Örneğinde**<br>
IsNull ([displayName])                                                                                                
Öznitelik yoksa true döndürür

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**Çalışmayacaktır**<br> IsNullOrEmpty (Ifade)

**Açıklama:**<br> İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true değerini döndürür. Özniteliği için, öznitelik yoksa veya varsa ancak boş bir dize ise, bu true olarak değerlendirilir.
Bu işlevin tersi ısun olarak adlandırılmıştır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |ifade |Değerlendirilecek ifade |

**Örneğinde**<br>
IsNullOrEmpty ([displayName])                                               
Öznitelik yoksa veya boş bir dize ise true döndürür

---
### <a name="ispresent"></a>Olmasına
**Çalışmayacaktır**<br> Isun (Ifade)

**Açıklama:**<br> İfade null olmayan ve boş olmayan bir dize olarak değerlendirilirse, ıssun işlevi true değerini döndürür. Bu işlevin tersi IsNullOrEmpty olarak adlandırılmıştır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |ifade |Değerlendirilecek ifade |

**Örneğinde**<br>
Anahtar (ısunu ([directManager]), [directManager], ısun ([skiplevelManager]), [skiplevelManager], ıssun ([Yönetmen]), [Yönetmen])

---
### <a name="isstring"></a>Isstrıng
**Çalışmayacaktır**<br> IsString (Ifade)

**Açıklama:**<br> İfade bir dize türü olarak değerlendirilebiliyorsa, IsString işlevi true olarak değerlendirilir.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ifadesini** |Gerekli |ifade |Değerlendirilecek ifade |

---
### <a name="item"></a>Öğe
**Çalışmayacaktır**<br> Öğe (öznitelik, dizin)

**Açıklama:**<br> Item işlevi, birden çok değerli dize/öznitelikten bir öğe döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |Öznitelik |Aranacak çok değerli öznitelik |
| **indeks** |Gerekli |Tamsayı | Çoklu değerli dizedeki bir öğenin dizini|

**Örneğinde**<br>
Öğe ([proxyAddresses], 1)

---
### <a name="join"></a>Birleştir
**Çalışmayacaktır**<br> JOIN (separator, source1, SOURCE2,...)

**Açıklama:**<br> JOIN (), birden çok **kaynak** dize değerini tek bir dizeye birleştirebildiğinden ve her bir değer bir **ayırıcı** dizeyle ayrılabilmesi dışında Append () ile benzerdir.

Kaynak değerlerinden biri çok değerli bir öznitelik ise, bu öznitelikteki her değer, ayırıcı değeriyle ayırarak birlikte birleştirilir.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **ayırıcı** |Gerekli |Dize |Tek bir dizede bitiştirildiği zaman kaynak değerlerini ayırmak için kullanılan dize. Hiçbir ayırıcı gerekmiyorsa "" olabilir. |
| **source1 ... Kaynakcen** |Gerekli, değişken sayısı |Dize |Birlikte birleştirilecek dize değerleri. |

---
### <a name="left"></a>Sol
**Çalışmayacaktır**<br> Left (dize, NumChars)

**Açıklama:**<br> Left işlevi bir dizenin sol tarafında belirtilen sayıda karakteri döndürür. NumChars = 0 ise boş bir dize döndürür.
NumChars 0 <, giriş dizesi döndürür.
Dize null ise boş dize döndürün.
Dize Numchar 'lar içinde belirtilen sayıdan daha az karakter içeriyorsa, dize ile özdeş bir dize (yani, 1 parametresindeki tüm karakterleri içeren) döndürülür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Dize** |Gerekli |Öznitelik | Karakterlerin döndürdüğü dize |
| **NumChars** |Gerekli |Tamsayı | Dizenin başından (solda) döndürülecek karakter sayısını tanımlayan sayı|

**Örneğinde**<br>
Sol ("John tikan", 3)                                                            
"Joh" döndürür

---
### <a name="mid"></a>Orta
**Çalışmayacaktır**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklama:**<br> Kaynak değerin alt dizesini döndürür. Alt dize, kaynak dizeden yalnızca bazı karakterleri içeren bir dizedir.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle özniteliğin adı. |
| **başından** |Gerekli |integer |Alt dizenin başlaması gereken **kaynak** dizedeki dizin. Dizedeki ilk karakter 1 dizinine sahip olacak, ikinci karakter dizin 2 ' ye sahip olur ve bu şekilde devam eder. |
| **uzunluklu** |Gerekli |integer |Alt dizenin uzunluğu. Uzunluk **kaynak** dizenin dışında biterse, işlev **Başlangıç** dizininden **kaynak** dizenin sonuna kadar alt dize döndürür. |

---
### <a name="normalizediacritics"></a>Normalizediacritika
**Çalışmayacaktır**<br> Normalizediacritika (kaynak)

**Açıklama:**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm aksanlı karakterlerle eşdeğer, aksanlı olmayan karakterler konur. Genellikle, aksanlı karakter (vurgu işaretleri) içeren ilk adları ve soyadlarını, Kullanıcı asıl adları, SAM hesap adları ve e-posta adresleri gibi çeşitli Kullanıcı tanımlayıcılarında kullanılabilecek geçerli değerlere dönüştürmek için kullanılır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize | Genellikle ad veya soyadı özniteliği. |

---
### <a name="not"></a>Not
**Çalışmayacaktır**<br> Not (kaynak)

**Açıklama:**<br> **Kaynağın**Boole değerini çevirir. **Kaynak** değer "*true*" ise, "*false*" döndürür. Aksi takdirde "*true*" döndürür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Boole dizesi |Beklenen **kaynak** değerleri "true" veya "false" şeklindedir. |

---
### <a name="numfromdate"></a>NumFromDate
**Çalışmayacaktır**<br> NumFromDate (değer)

**Açıklama:**<br> NumFromDate işlevi, bir DateTime değerini [AccountExpires](https://docs.microsoft.com/windows/win32/adschema/a-accountexpires)gibi öznitelikleri ayarlamak için gereken Active Directory biçime dönüştürür. Workday ve başarılı faktörlerdeki bulut HR uygulamalarından alınan DateTime değerlerini eşdeğer AD gösterimlerine dönüştürmek için bu işlevi kullanın. 

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **deeri** |Gerekli | Dize | Desteklenen biçimdeki tarih saat dizesi. Desteklenen biçimler için bkz https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx.. |

**Örneğinde**<br>
* Workday örneği <br>
  AD 'nin *2020-12-31-08:00* Ile *AccountExpires* alanında olan Workday 'den *sözleşmeli tenddate* özniteliğini eşlemek istediğinizi varsayarsak, bu işlevi kullanabilir ve saat dilimi sapmasını yerel ayarınızdan eşleşecek şekilde değiştirebilirsiniz. 
  `NumFromDate(Join("", FormatDateTime([ContractEndDate], "yyyy-MM-ddzzz", "yyyy-MM-dd"), "T23:59:59-08:00"))`

* Başarılı etken örneği <br>
  AD içinde *M/d/yyyy HH: mm: ss TT* Ile *AccountExpires* alanı biçimindeki *EndDate* özniteliğini başarılı bir şekilde eşlemek istediğinizi varsayarsak, bu işlevi kullanabilir ve saat dilimi sapmasını yerel ayarınızdan eşleşecek şekilde değiştirebilirsiniz.
  `NumFromDate(Join("",FormatDateTime([endDate],"M/d/yyyy hh:mm:ss tt","yyyy-MM-dd"),"T23:59:59-08:00"))`


---
### <a name="removeduplicates"></a>RemoveDuplicates
**Çalışmayacaktır**<br> Removeyinelemelerini (öznitelik)

**Açıklama:**<br> Removeyinelemelerini işlevi, birden çok değerli dizeyi alır ve her değerin benzersiz olduğundan emin olur.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **özniteliğe** |Gerekli |Çoklu değerli öznitelik |Yinelenen öğeleri kaldırılacak birden çok değerli öznitelik|

**Örneğinde**<br>
Removeyinelemelerini ([proxyAddresses])                                                                                                       
Tüm yinelenen değerlerin kaldırıldığı bir ayıklanmış proxyAddress özniteliği döndürür

---
### <a name="replace"></a>Değiştir
**Çalışmayacaktır**<br> Replace (kaynak, oldValue, regexGroupName, regexGroupName, replacementValue, replacementAttributeName, şablon)

**Açıklama:**<br>
Dize içindeki değerleri değiştirir. Belirtilen parametrelere göre farklı şekilde çalışır:

* **OldValue** ve **replacementvalue** sağlandığında:
  
  * **Kaynaktaki** tüm **OldValue** yinelemelerini **replacementvalue** ile değiştirir
* **OldValue** ve **şablon** sağlandığında:
  
  * **Şablondaki** **OldValue** tüm oluşumlarını **kaynak** değerle değiştirir
* **Regexmodel** ve **replacementvalue** sağlandığında:

  * İşlevi, **Regexmodel** öğesini **kaynak** dizeye uygular ve **replacementvalue** için dizeyi oluşturmak üzere Regex grup adlarını kullanabilirsiniz
* **Regexgroupname**, **regexGroupName** **replacementvalue** sağlandığında:
  
  * İşlev, **kaynak** dizeye **regexmodel** uygular ve **regexgroupname** Ile eşleşen tüm değerleri **replacementvalue** ile değiştirir
* **Regexgroupname**, **replacementAttributeName** sağlandığında: **regexGroupName**
  
  * **Kaynakta** değer yoksa, **kaynak** döndürülür
  * **Kaynakta** bir değer varsa, Işlev **regexmodel** öğesini **kaynak** dizeye uygular ve **Regexgroupname** ile eşleşen tüm değerleri **replacementAttributeName** ile ilişkili değerle değiştirir

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle **kaynak** nesneden özniteliğin adı. |
| **oldValue** |İsteğe Bağlı |Dize |**Kaynak** veya **şablonda**değiştirilmekte olan değer. |
| **Regexmodel** |İsteğe Bağlı |Dize |**Kaynakta**değiştirilmekte olan değer için Regex stili. Ya da **Replacementpropertyname** kullanıldığında, **replacementpropertyname**öğesinden değer çıkarmak için bir model. |
| **regexGroupName** |İsteğe Bağlı |Dize |**Regexmodel**içindeki grubun adı. Yalnızca **Replacementpropertyname** kullanıldığında, bu grubun değerini **replacementpropertyname**öğesinden **replacementvalue** olarak ayıklayacağız. |
| **replacementValue** |İsteğe Bağlı |Dize |Yenisiyle eskisinin yerini alacak yeni değer. |
| **replacementAttributeName** |İsteğe Bağlı |Dize |Değiştirme değeri için kullanılacak özniteliğin adı |
| **şablonlarını** |İsteğe Bağlı |Dize |**Şablon** değeri sağlandığında, şablon içinde **OldValue** aranacağı ve bunu **kaynak** değerle değiştirecek. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Çalışmayacaktır**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Açıklama:**<br> İfadeler kullanılarak tanımlanan benzersiz değer oluşturma kuralları olan en az iki bağımsız değişken gerektirir. İşlevi her kuralı değerlendirir ve hedef uygulamada/dizinde benzersizlik için oluşturulan değeri denetler. Bulunan ilk benzersiz değer döndürülecek bir değer olacaktır. Tüm değerler hedefte zaten mevcutsa, giriş biriktirme listesini alır ve bu nedenle denetim günlüklerine kaydedilir. Sağlanbağlanabilen bağımsız değişken sayısına üst sınır yoktur.


 - Bu, üst düzey bir işlevdir, iç içe geçirilemez.
 - Bu işlev, eşleşen önceliği olan özniteliklere uygulanamaz.   
 - Bu işlev yalnızca giriş oluşturmaları için kullanılmak üzere tasarlanmıştır. Bir özniteliğiyle birlikte kullanıldığında, **uygulamayı Uygula** özelliğini **yalnızca nesne oluşturma sırasında**olarak ayarlayın.
 - Bu işlev şu anda yalnızca "Workday for User sağlamasını Active Directory" için desteklenir. Diğer sağlama uygulamalarıyla birlikte kullanılamaz. 


**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **uniqueValueRule1 ... uniqueValueRuleN** |En az 2 gerekir, üst sınır yoktur |Dize | Değerlendirilecek benzersiz değer oluşturma kurallarının listesi. |


---
### <a name="singleapproleassignment"></a>Singleapprotaatama
**Çalışmayacaktır**<br> Singleapprotaatama ([Approtaatamalar])

**Açıklama:**<br> Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür. Approtaatamalar nesnesini tek bir rol adı dizesine dönüştürmek için bu işlev gereklidir. En iyi yöntem, tek seferde bir kullanıcıya yalnızca bir Approelatama atandığından emin olmak ve birden çok rol atanmamışsa döndürülen rol dizesinin tahmin edilebilir olamayacağını unutmayın. 

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **AppRoleAssignments** |Gerekli |Dize |**[Approtaatamalar]** nesnesi. |

---
### <a name="split"></a>Bölme
**Çalışmayacaktır**<br> Böl (kaynak, sınırlayıcı)

**Açıklama:**<br> Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |
| **ayırıcı** |Gerekli |Dize |Dizeyi ayırmak için kullanılacak karakteri belirtir (örneğin: ",") |

---
### <a name="stripspaces"></a>StripSpaces
**Çalışmayacaktır**<br> StripSpaces (kaynak)

**Açıklama:**<br> Kaynak dizeden tüm boşluk ("") karakterlerini kaldırır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |

---
### <a name="switch"></a>Anahtar
**Çalışmayacaktır**<br> Anahtar (kaynak, defaultValue, KEY1, değer1, key2, değer2,...)

**Açıklama:**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtara eşleşmezse, **DefaultValue**döndürür.  **Anahtar** ve **değer** parametrelerinin her zaman çiftler halinde gelmesi gerekir. İşlev her zaman çift sayıda parametre bekler. İşlev, yönetici gibi başvuru öznitelikleri için kullanılmamalıdır. 

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Güncelleştirilecek **kaynak** değeri. |
| **Değerinin** |İsteğe Bağlı |Dize |Kaynak herhangi bir anahtara eşleşmezse kullanılacak varsayılan değer. Boş dize ("") olabilir. |
| **anahtar** |Gerekli |Dize |**Kaynak** değeri Karşılaştırılacak **anahtar** . |
| **deeri** |Gerekli |Dize |Anahtarla eşleşen **kaynak** için değiştirme değeri. |

---
### <a name="tolower"></a>ToLower
**Çalışmayacaktır**<br> ToLower (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="toupper"></a>ToUpper
**Çalışmayacaktır**<br> ToUpper (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynaktaki** |Gerekli |Dize |Genellikle kaynak nesneden özniteliğin adı. |
| **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---
### <a name="word"></a>Word
**Çalışmayacaktır**<br> Sözcük (dize, WordNumber, sınırlayıcılar)

**Açıklama:**<br> Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük numarasını açıklayan parametrelere göre dize içinde içerilen bir sözcük döndürür. Sınırlayıcıdaki karakterlerden biri tarafından ayrılan dizedeki her karakter dizesi, sözcük olarak tanımlanır:

Sayı < 1 ise boş bir dize döndürür.
Dize null ise, boş bir dize döndürür.
Dize sayı olan sözcüklerden daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan herhangi bir sözcük içermiyorsa, boş bir dize döndürülür.

**Parametrelere**<br> 

| Name | Gerekli/yinelenen | Tür | Notlar |
| --- | --- | --- | --- |
| **Dize** |Gerekli |Çoklu değerli öznitelik |Bir sözcüğün döndürdüğü dize.|
| **WordNumber** |Gerekli | Tamsayı | Hangi sözcük sayısının dönmesi gerektiğini tanımlayan sayı|
| **ıcı** |Gerekli |Dize| Sözcükleri tanımlamak için kullanılması gereken sınırlayıcıları temsil eden bir dize|

**Örneğinde**<br>
Word ("hızlı kahverengi Fox", 3, "")                                                                                       
"Kahverengi" döndürür

Word ("this, String!&çok sayıda ayırıcı içeriyor", 3, ",! & #")                                                                       
"İçerir" sonucunu döndürür

---

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Bilinen etki alanı adını şeridi
Kullanıcı adı almak için kullanıcının e-postalarından bilinen bir etki alanı adını çıkarmanız gerekir. <br>
Örneğin, etki alanı "contoso.com" ise aşağıdaki ifadeyi kullanabilirsiniz:

**İfadesini** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): "john.doe@contoso.com"
* **Çıkış**: "John. tikan"

### <a name="append-constant-suffix-to-user-name"></a>Sabit son eki Kullanıcı adına Ekle
Salesforce korumalı alanı kullanıyorsanız, eşitlemeden önce tüm kullanıcı adlarınıza ek bir sonek eklemeniz gerekebilir.

**İfadesini** <br>
`Append([userPrincipalName], ".test")`

**Örnek giriş/çıkış:** <br>

* **Giriş**: (UserPrincipalName): "John.Doe@contoso.com"
* **Çıkış**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Birinci ve soyadı parçalarını birleştirerek kullanıcı diğer adı oluştur
Kullanıcının ilk adının ilk 3 harfini ve Kullanıcı adının ilk 5 harfini ayırarak bir kullanıcı diğer adı oluşturmanız gerekir.

**İfadesini** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Örnek giriş/çıkış:** <br>

* **Giriş** ("John"): "John"
* **Giriş** (soyad): "tikan"
* **Çıkış**: "cantikan"

### <a name="remove-diacritics-from-a-string"></a>Bir dizeden aksanları kaldırma
Vurgu işaretleri içeren karakterleri, vurgu işaretleri içermeyen denk karakterlerle değiştirmeniz gerekir.

**İfadesini** <br>
Normalizediacritika ([1})

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
Belirli bir biçimde bir SaaS uygulamasına tarihler göndermek istiyorsunuz. <br>
Örneğin, ServiceNow için tarihleri biçimlendirmek istiyorsunuz.

**İfadesini** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Örnek giriş/çıkış:**

* **Giriş** (extensionAttribute1): "20150123105347.1 z"
* **Çıkış**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenek kümesine göre bir değeri değiştirin

Kullanıcının saat dilimini Azure AD 'de depolanan durum koduna göre tanımlamanız gerekir. <br>
Durum kodu önceden tanımlanmış seçeneklerle eşleşmezse, "Avustralya/Sidney" varsayılan değerini kullanın.

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

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>UserPrincipalName (UPN) özniteliği için benzersiz değer oluştur
Kullanıcının adı, ikinci adı ve soyadı temelinde, UPN özniteliği için bir değer oluşturmanız ve değeri UPN özniteliğine atamadan önce hedef AD dizininde benzersizliği denetlemeniz gerekir.

**İfadesini** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com"),
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Örnek giriş/çıkış:**

* **Giriş** (preferredfirstname): "John"
* **Giriş** (preferredlastname): "Smith"
* **Çıkış**: "John.Smith@contoso.com" için UPN değeri John.Smith@contoso.com zaten dizinde yoksa
* **Çıkış**: "J.Smith@contoso.com" dizininde UPN değeri John.Smith@contoso.com zaten varsa
* **Çıkış**: YukarıdakiJo.Smith@contoso.comiki UPN değeri dizinde zaten mevcutsa ""

### <a name="flow-mail-value-if-not-null-otherwise-flow-userprincipalname"></a>NULL değilse Flow posta değeri, aksi takdirde Flow userPrincipalName
Varsa posta özniteliğini akışa almak istiyorsunuz. Aksi takdirde, bunun yerine userPrincipalName değerini akmasını istersiniz.

**İfadesini** <br>
`Coalesce([mail],[userPrincipalName])`

**Örnek giriş/çıkış:** <br>

* **Giriş** (posta): null
* **Giriş** (UserPrincipalName): "John.Doe@contoso.com"
* **Çıkış**: "John.Doe@contoso.com"

## <a name="related-articles"></a>İlgili Makaleler
* [SaaS uygulamalarına Kullanıcı sağlamasını/sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md)
* [Kullanıcı hazırlama için öznitelik eşlemelerini özelleştirme](../app-provisioning/customize-application-attributes.md)
* [Kullanıcı hazırlama için kapsam filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](../app-provisioning/use-scim-to-provision-users-and-groups.md)
* [Hesap Sağlama Bildirimleri](../app-provisioning/user-provisioning.md)
* [SaaS Uygulamalarını Tümleştirme Hakkında Öğreticiler Listesi](../saas-apps/tutorial-list.md)
