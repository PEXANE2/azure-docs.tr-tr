---
title: Azure AD Connect bulut sağlama ifadeleri ve işlev başvurusu
description: başvurunun
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d250377e15b957c10322dbba9ca587dd58944ad
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793545"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory öznitelik eşlemeleri için ifadeler yazma
Bulut sağlamayı yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemelerinin türlerinden biri bir ifade eşlemedir. 

İfade eşlemesi, bir betik benzeri ifade kullanarak öznitelikleri özelleştirmenizi sağlar.  Bu, şirket içi verileri yeni veya farklı bir değere dönüştürmenizi sağlar.  Örneğin, bu tek öznitelik bulut uygulamalarınızdan biri tarafından kullanıldığından, iki özniteliği tek bir öznitelikte birleştirmek isteyebilirsiniz.

Aşağıdaki belge, verileri dönüştürmek için kullanılan betiğe benzer ifadeleri kapsayacaktır.  Bu yalnızca işlemin bir parçasıdır.  Daha sonra bu ifadeyi kullanmanız ve kiracınıza bir Web isteğine yerleştirmeniz gerekir.  Bu konuda daha fazla bilgi için bkz. [dönüşümler](how-to-transformation.md)

## <a name="syntax-overview"></a>Sözdizimine genel bakış
Öznitelik eşlemeleri için Ifadeler söz dizimi Visual Basic for Applications (VBA) işlevlerinin bir rekidir.

* İfadenin tamamı, parantez içindeki bağımsız değişkenlerin ardında yer aldığı bir addan oluşan işlevler bakımından tanımlanmalıdır: <br>
  *Fonksiyonadı (`<<argument 1>>`,`<<argument N>>`)*
* İşlevleri birbirlerine iç içe yerleştirebilirsiniz. Örnek: <br> *FunctionOne (FunctionTwo (`<<argument1>>`))*
* İşlevlere üç farklı türde bağımsız değişken geçirebilirsiniz:
  
  1. Köşeli ayraçlar içine alınması gereken öznitelikler. Örneğin: [attributeName]
  2. Çift tırnak içine alınması gereken dize sabitleri. Örneğin: "Birleşik Devletler"
  3. Diğer Işlevler. Örneğin: FunctionOne (`<<argument1>>`, FunctionTwo (`<<argument2>>`))
* Dize sabitleri için, dizede bir ters eğik çizgi (\) veya tırnak işareti (") gerekiyorsa, ters eğik çizgi (\) simgesiyle atlanmalıdır. Örneğin: "Şirket adı: \\" contoso\\""

## <a name="list-of-functions"></a>İşlevlerin listesi
| İşlevlerin listesi | Açıklama |
|-----|----|
|[Ýna](#append)|Bir kaynak dize değeri alır ve son eki bunun sonuna ekler.|
|[BitAnd](#bitand)|BitAnd işlevi, belirtilen bitleri bir değer üzerinde ayarlar.|
|[CBool](#cbool)|CBool işlevi, değerlendirilen ifadeye bağlı olarak bir Boole değeri döndürür|
|[ConvertFromBase64](#convertfrombase64)|ConvertFromBase64 işlevi, belirtilen Base64 kodlamalı değeri normal bir dizeye dönüştürür.|
|[ConvertToBase64](#converttobase64)|ConvertToBase64 işlevi bir dizeyi Unicode Base64 dizesine dönüştürür. |
|[ConvertToUTF8Hex](#converttoutf8hex)|ConvertToUTF8Hex işlevi bir dizeyi UTF8 onaltılık kodlanmış bir değere dönüştürür.|
|[Biriktirme](#count)|Count işlevi, birden çok değerli bir öznitelikteki öğelerin sayısını döndürür|
|[CStr](#cstr)|CStr işlevi bir dize veri türüne dönüştürür.|
|[Tarih Fromnum](#datefromnum)|DateFromNum işlevi, AD 'nin tarih biçimindeki bir değeri bir tarih saat türüne dönüştürür.|
|[DNComponent](#dncomponent)|DNComponent işlevi, soldan bir belirtilen DN bileşeninin değerini döndürür.|
|[Hatayla](#error)|Hata işlevi özel bir hata döndürmek için kullanılır.|
|[FormatDateTime](#formatdatetime) |Bir biçimden tarih dizesi alır ve onu farklı bir biçime dönüştürür.| 
|['INI](#guid)|İşlev GUID 'Si yeni bir rastgele GUID oluşturur.|           
|[Mayan](#iif)|IıF işlevi, belirli bir koşula göre olası bir değer kümesinden birini döndürür.|
|[InStr](#instr)|InStr işlevi bir dizedeki alt dizenin ilk oluşumunu bulur.|
|[IsNull](#isnull)|İfade null olarak değerlendirilirse, IsNull işlevi true döndürür.|
|[IsNullOrEmpty](#isnullorempty)|İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true değerini döndürür.|         
|[Olmasına](#ispresent)|İfade null olmayan ve boş olmayan bir dize olarak değerlendirilirse, ıssun işlevi true değerini döndürür.|    
|[Isstrıng](#isstring)|İfade bir dize türü olarak değerlendirilebiliyorsa, IsString işlevi true olarak değerlendirilir.|
|[Öğe](#item)|Item işlevi, birden çok değerli dize/öznitelikten bir öğe döndürür.|
|[Birleştir](#join) |JOIN (), birden çok **kaynak** dize değerini tek bir dizeye birleştirebildiğinden ve her bir değer bir **ayırıcı** dizeyle ayrılabilmesi dışında Append () ile benzerdir.| 
|[Tarafta](#left)|Left işlevi bir dizenin sol tarafında belirtilen sayıda karakteri döndürür.|
|[Orta](#mid) |Kaynak değerin alt dizesini döndürür. Alt dize, kaynak dizeden yalnızca bazı karakterleri içeren bir dizedir.|
|[Normalizediacritika](#normalizediacritics)|Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm aksanlı karakterlerle eşdeğer, aksanlı olmayan karakterler konur.|
|[Başlatılmadı](#not) |**Kaynağın**Boole değerini çevirir. **Kaynak** değer "*true*" ise, "*false*" döndürür. Aksi takdirde "*true*" döndürür.| 
|[RemoveDuplicates](#removeduplicates)|Removeyinelemelerini işlevi, birden çok değerli dizeyi alır ve her değerin benzersiz olduğundan emin olur.| 
|[Değiştirin](#replace) |Dize içindeki değerleri değiştirir. | 
|[SelectUniqueValue](#selectuniquevalue)|İfadeler kullanılarak tanımlanan benzersiz değer oluşturma kuralları olan en az iki bağımsız değişken gerektirir. İşlevi her kuralı değerlendirir ve hedef uygulamada/dizinde benzersizlik için oluşturulan değeri denetler.| 
|[Singleapprotaatama](#singleapproleassignment)|Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür.| 
|[Ayırmayı](#split)|Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.|
|[StringFromSID](#stringfromsid)|StringFromSid işlevi, bir güvenlik tanımlayıcısı içeren bir bayt dizisini dizeye dönüştürür.| 
|[StripSpaces](#stripspaces) |Kaynak dizeden tüm boşluk ("") karakterlerini kaldırır.| 
|[Değiştirebilirsiniz](#switch)|**Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. | 
|[ToLower](#tolower)|Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür.| 
|[ToUpper](#toupper)|Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür.|
|[Kırpma](#trim)|Trim işlevi bir dizeden baştaki ve sondaki boşlukları kaldırır.|
|[Sözcük](#word)|Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük numarasını açıklayan parametrelere göre dize içinde içerilen bir sözcük döndürür.|

---
### <a name="append"></a>Ekle
**Çalışmayacaktır**<br> Append (kaynak, sonek)

**Açıklaması**<br> Bir kaynak dize değeri alır ve son eki bunun sonuna ekler.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
   | **önekini** |Gereklidir |Dize |Kaynak değerin sonuna eklemek istediğiniz dize. |

---
### <a name="bitand"></a>BitAnd
**Açıklaması**  
BitAnd işlevi, belirtilen bitleri bir değer üzerinde ayarlar.

**Sözdizimi**  
`num BitAnd(num value1, num value2)`

* Değer1, değer2: bir arada olması gereken sayısal değerler

**Açıklamalarının**  
Bu işlev, her iki parametreyi de ikili gösterimine dönüştürür ve bir bit şu şekilde ayarlar:

* 0- *değer1* ve *değer2* içindeki karşılık gelen bitlerin biri veya her ikisi 0 ise
* 1-karşılık gelen bitlerin her ikisi de 1 ' dir.

Diğer bir deyişle, her iki parametrenin de karşılık gelen bitlerinin 1 olduğu durumlar dışında her durumda 0 döndürür.

**Örnek:**  
 
 `BitAnd(&HF, &HF7)`</br>
 Onaltılık "F" ve "F7" değerinin bu değeri değerlendirmesi nedeniyle 7 döndürür.

---

### <a name="cbool"></a>CBool
**Açıklaması**  
CBool işlevi, değerlendirilen ifadeye bağlı olarak bir Boole değeri döndürür

**Sözdizimi**  
`bool CBool(exp Expression)`

**Açıklamalarının**  
İfade sıfır olmayan bir değer olarak değerlendirilirse, CBool true değerini döndürür, aksi takdirde false döndürür.

**Örnek:**  
`CBool([attrib1] = [attrib2])`  

Her iki öznitelik de aynı değere sahip olduğunda true döndürür.

---
### <a name="convertfrombase64"></a>ConvertFromBase64
**Açıklaması**  
ConvertFromBase64 işlevi, belirtilen Base64 kodlamalı değeri normal bir dizeye dönüştürür.

**Sözdizimi**  
`str ConvertFromBase64(str source)`-kodlama için Unicode varsayılır  
`str ConvertFromBase64(str source, enum Encoding)`

* Kaynak: Base64 kodlamalı dize  
* Kodlama: Unicode, ASCII, UTF8

**Örnek**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Her iki örnek de "*Hello World!* " döndürür

---
### <a name="converttobase64"></a>ConvertToBase64
**Açıklaması**  
ConvertToBase64 işlevi bir dizeyi Unicode Base64 dizesine dönüştürür.  
Bir tamsayılar dizisinin değerini, Base-64 basamakları ile kodlanmış eşdeğer dize gösterimine dönüştürür.

**Sözdizimi**  
`str ConvertToBase64(str source)`

**Örnek:**  
`ConvertToBase64("Hello world!")`  
"Sablagwabebek Vacaadilevahiababkacea" döndürür

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Açıklaması**  
ConvertToUTF8Hex işlevi bir dizeyi UTF8 onaltılık kodlanmış bir değere dönüştürür.

**Sözdizimi**  
`str ConvertToUTF8Hex(str source)`

**Açıklamalarının**  
Bu işlevin çıkış biçimi Azure Active Directory tarafından, DN öznitelik biçimi olarak kullanılır.

**Örnek:**  
`ConvertToUTF8Hex("Hello world!")`  
48656C6C6F20776F726C6421 döndürür

---
### <a name="count"></a>Sayı
**Açıklaması**  
Count işlevi, birden çok değerli bir öznitelikteki öğelerin sayısını döndürür

**Sözdizimi**  
`num Count(mvstr attribute)`

---
### <a name="cstr"></a>CStr
**Açıklaması**  
CStr işlevi bir dize veri türüne dönüştürür.

**Sözdizimi**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* değer: sayısal bir değer, başvuru özniteliği veya Boole olabilir.

**Örnek:**  
`CStr([dn])`  
"CN = ali, DC = contoso, DC = com" döndürebilir

---
### <a name="datefromnum"></a>Tarih Fromnum
**Açıklaması**  
DateFromNum işlevi, AD 'nin tarih biçimindeki bir değeri bir tarih saat türüne dönüştürür.

**Sözdizimi**  
`dt DateFromNum(num value)`

**Örnek:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
2012-01-01 23:00:00 temsil eden bir tarih saat döndürür

---
### <a name="dncomponent"></a>DNComponent
**Açıklaması**  
DNComponent işlevi, soldan bir belirtilen DN bileşeninin değerini döndürür.

**Sözdizimi**  
`str DNComponent(ref dn, num ComponentNumber)`

* DN: yorumlamaya yönelik başvuru özniteliği
* ComponentNumber: DN 'nin döndürülecek olan bileşen

**Örnek:**  
`DNComponent(CRef([dn]),1)`  
DN "CN = ali, OU =..." ise, "ali döndürür

---
### <a name="error"></a>Hata
**Açıklaması**  
Hata işlevi özel bir hata döndürmek için kullanılır.

**Sözdizimi**  
`void Error(str ErrorMessage)`

**Örnek:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
AccountName özniteliği yoksa, nesne üzerinde bir hata oluşturur.

---
### <a name="formatdatetime"></a>formatDateTime
**Çalışmayacaktır**<br> FormatDateTime (kaynak, InPutFormat, outputFormat)

**Açıklaması**<br> Bir biçimden tarih dizesi alır ve onu farklı bir biçime dönüştürür.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
   | **InPutFormat** |Gereklidir |Dize |Kaynak değerinin biçimi bekleniyordu. Desteklenen biçimler için bkz. [https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
   | **outputFormat** |Gereklidir |Dize |Çıkış tarihinin biçimi. |

---
### <a name="guid"></a>Guid
**Açıklaması**  
İşlev GUID 'Si yeni bir rastgele GUID oluşturur

**Sözdizimi**  
`str Guid()`

---
### <a name="iif"></a>Mayan
**Açıklaması**  
IıF işlevi, belirli bir koşula göre olası bir değer kümesinden birini döndürür.

**Sözdizimi**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* koşul: doğru veya yanlış olarak değerlendirilebilen herhangi bir değer veya ifade.
* valueIfTrue: koşul true olarak değerlendirilirse döndürülen değer.
* valueIfFalse: Koşul false olarak değerlendirilirse döndürülen değer.

**Örnek:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Kullanıcı bir ınters ise, "t-" bir kullanıcının diğer adını döndürür. Bu, başka bir kullanıcının diğer adını olduğu gibi döndürür.

---
### <a name="instr"></a>InStr
**Açıklaması**  
InStr işlevi bir dizedeki alt dizenin ilk oluşumunu bulur

**Sözdizimi**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: Aranacak dize
* stringmatch: bulunan dize
* Başlat: alt dizeyi bulmak için başlangıç konumu
* Compare: vbTextCompare veya vbBinaryCompare

**Açıklamalarının**  
Alt dizenin bulunduğu konumu veya bulunmazsa 0 değerini döndürür.

**Örnek:**  
`InStr("The quick brown fox","quick")`  
Evalues 'a 5

`InStr("repEated","e",3,vbBinaryCompare)`  
7 olarak değerlendirilir

---
### <a name="isnull"></a>IsNull
**Açıklaması**  
İfade null olarak değerlendirilirse, IsNull işlevi true döndürür.

**Sözdizimi**  
`bool IsNull(var Expression)`

**Açıklamalarının**  
Bir öznitelik için, bir null özniteliğin yokluğuna göre ifade edilir.

**Örnek:**  
`IsNull([displayName])`  
Öznitelik CS veya MV içinde yoksa, true döndürür.

---
### <a name="isnullorempty"></a>IsNullOrEmpty
**Açıklaması**  
İfade null veya boş bir dize ise, IsNullOrEmpty işlevi true değerini döndürür.

**Sözdizimi**  
`bool IsNullOrEmpty(var Expression)`

**Açıklamalarının**  
Özniteliği için, öznitelik yoksa veya varsa ancak boş bir dize ise, bu true olarak değerlendirilir.  
Bu işlevin tersi ısun olarak adlandırılmıştır.

**Örnek:**  
`IsNullOrEmpty([displayName])`  
Öznitelik yoksa veya CS ya da MV içindeki boş bir dize ise true döndürür.

---
### <a name="ispresent"></a>Olmasına
**Açıklaması**  
İfade null olmayan ve boş olmayan bir dize olarak değerlendirilirse, ıssun işlevi true değerini döndürür.

**Sözdizimi**  
`bool IsPresent(var expression)`

**Açıklamalarının**  
Bu işlevin tersi IsNullOrEmpty olarak adlandırılmıştır.

**Örnek:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

---
### <a name="item"></a>Öğe
**Açıklaması**  
Item işlevi, birden çok değerli dize/öznitelikten bir öğe döndürür.

**Sözdizimi**  
`var Item(mvstr attribute, num index)`

* öznitelik: çok değerli öznitelik
* Dizin: Çoklu değerli dizedeki bir öğenin dizini.

**Açıklamalarının**  
İkinci işlev, dizini çok değerli öznitelikteki bir öğeye döndürdüğünden, öğe işlevi Contains işleviyle birlikte kullanışlıdır.

Dizin sınırların dışında olduğunda bir hata oluşturur.

**Örnek:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Birincil e-posta adresini döndürür.

---
### <a name="isstring"></a>Isstrıng
**Açıklaması**  
İfade bir dize türü olarak değerlendirilebiliyorsa, IsString işlevi true olarak değerlendirilir.

**Sözdizimi**  
`bool IsString(var expression)`

**Açıklamalarının**  
CStr () ifadesinin ayrıştırmak için başarılı olup olmadığını belirlemekte kullanılır.

---
### <a name="join"></a>Birleştir
**Çalışmayacaktır**<br> JOIN (separator, source1, SOURCE2,...)

**Açıklaması**<br> JOIN (), birden çok **kaynak** dize değerini tek bir dizeye birleştirebildiğinden ve her bir değer bir **ayırıcı** dizeyle ayrılabilmesi dışında Append () ile benzerdir.

Kaynak değerlerinden biri çok değerli bir öznitelik ise, bu öznitelikteki her değer, ayırıcı değeriyle ayırarak birlikte birleştirilir.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **ayırıcı** |Gereklidir |Dize |Tek bir dizede bitiştirildiği zaman kaynak değerlerini ayırmak için kullanılan dize. Hiçbir ayırıcı gerekmiyorsa "" olabilir. |
   | **source1 ... Kaynakcen** |Gerekli, değişken sayısı |Dize |Birlikte birleştirilecek dize değerleri. |

---
### <a name="left"></a>Tarafta
**Açıklaması**  
Left işlevi bir dizenin sol tarafında belirtilen sayıda karakteri döndürür.

**Sözdizimi**  
`str Left(str string, num NumChars)`

* dize: karakterlerin döndürdüğü dize
* NumChars: dizenin başından (solda) döndürülecek karakter sayısını tanımlayan sayı

**Açıklamalarının**  
Dizedeki ilk Numchar karakterlerini içeren bir dize:

* NumChars = 0 ise boş bir dize döndürür.
* NumChars 0 <, giriş dizesi döndürür.
* Dize null ise boş dize döndürün.

Dize Numchar 'lar içinde belirtilen sayıdan daha az karakter içeriyorsa, dize ile özdeş bir dize (yani, 1 parametresindeki tüm karakterleri içeren) döndürülür.

**Örnek:**  
`Left("John Doe", 3)`  
`Joh`döndürür.

---
### <a name="mid"></a>Orta
**Çalışmayacaktır**<br> Mid (kaynak, başlangıç, uzunluk)

**Açıklaması**<br> Kaynak değerin alt dizesini döndürür. Alt dize, kaynak dizeden yalnızca bazı karakterleri içeren bir dizedir.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Genellikle özniteliğin adı. |
   | **start** |Gereklidir |integer |Alt dizenin başlaması gereken **kaynak** dizedeki dizin. Dizedeki ilk karakter 1 dizinine sahip olacak, ikinci karakter dizin 2 ' ye sahip olur ve bu şekilde devam eder. |
   | **uzunluklu** |Gereklidir |integer |Alt dizenin uzunluğu. Uzunluk **kaynak** dizenin dışında biterse, işlev **Başlangıç** dizininden **kaynak** dizenin sonuna kadar alt dize döndürür. |

---
### <a name="normalizediacritics"></a>Normalizediacritika
**Çalışmayacaktır**<br> Normalizediacritika (kaynak)

**Açıklaması**<br> Bir dize bağımsız değişkeni gerektirir. Dizeyi döndürür, ancak tüm aksanlı karakterlerle eşdeğer, aksanlı olmayan karakterler konur. Genellikle, aksanlı karakter (vurgu işaretleri) içeren ilk adları ve soyadlarını, Kullanıcı asıl adları, SAM hesap adları ve e-posta adresleri gibi çeşitli Kullanıcı tanımlayıcılarında kullanılabilecek geçerli değerlere dönüştürmek için kullanılır.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize | Genellikle ad veya soyadı özniteliği. |

---
### <a name="not"></a>Not
**Çalışmayacaktır**<br> Not (kaynak)

**Açıklaması**<br> **Kaynağın**Boole değerini çevirir. **Kaynak** değer "*true*" ise, "*false*" döndürür. Aksi takdirde "*true*" döndürür.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Boole dizesi |Beklenen **kaynak** değerleri "true" veya "false" şeklindedir. |

---
### <a name="removeduplicates"></a>RemoveDuplicates
**Açıklaması**  
Removeyinelemelerini işlevi, birden çok değerli dizeyi alır ve her değerin benzersiz olduğundan emin olur.

**Sözdizimi**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Örnek:**  
`RemoveDuplicates([proxyAddresses])`  
Tüm yinelenen değerlerin kaldırıldığı bir ayıklanmış proxyAddress özniteliği döndürür.

---
### <a name="replace"></a>Değiştir
**Çalışmayacaktır**<br> Replace (kaynak, oldValue, regexGroupName, regexGroupName, replacementValue, replacementAttributeName, şablon)

**Açıklaması**<br>
Dize içindeki değerleri değiştirir. Belirtilen parametrelere göre farklı şekilde çalışır:

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

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Genellikle **kaynak** nesneden özniteliğin adı. |
   | **oldValue** |İsteğe Bağlı |Dize |**Kaynak** veya **şablonda**değiştirilmekte olan değer. |
   | **Regexmodel** |İsteğe Bağlı |Dize |**Kaynakta**değiştirilmekte olan değer için Regex stili. Ya da **Replacementpropertyname** kullanıldığında, **replacementpropertyname**öğesinden değer çıkarmak için bir model. |
   | **regexGroupName** |İsteğe Bağlı |Dize |**Regexmodel**içindeki grubun adı. Yalnızca **Replacementpropertyname** kullanıldığında, bu grubun değerini **replacementpropertyname**öğesinden **replacementvalue** olarak ayıklayacağız. |
   | **replacementValue** |İsteğe Bağlı |Dize |Yenisiyle eskisinin yerini alacak yeni değer. |
   | **replacementAttributeName** |İsteğe Bağlı |Dize |Değiştirme değeri için kullanılacak özniteliğin adı |
   | **şablonlarını** |İsteğe Bağlı |Dize |**Şablon** değeri sağlandığında, şablon içinde **OldValue** aranacağı ve bunu **kaynak** değerle değiştirecek. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Çalışmayacaktır**<br> SelectUniqueValue (uniqueValueRule1, uniqueValueRule2, uniqueValueRule3,...)

**Açıklaması**<br> İfadeler kullanılarak tanımlanan benzersiz değer oluşturma kuralları olan en az iki bağımsız değişken gerektirir. İşlevi her kuralı değerlendirir ve hedef uygulamada/dizinde benzersizlik için oluşturulan değeri denetler. Bulunan ilk benzersiz değer döndürülecek bir değer olacaktır. Tüm değerler hedefte zaten mevcutsa, giriş biriktirme listesini alır ve bu nedenle denetim günlüklerine kaydedilir. Sağlanbağlanabilen bağımsız değişken sayısına üst sınır yoktur.

> [!NOTE]
> - Bu, üst düzey bir işlevdir, iç içe geçirilemez.
> - Bu işlev, eşleşen önceliği olan özniteliklere uygulanamaz.  
> - Bu işlev yalnızca giriş oluşturmaları için kullanılmak üzere tasarlanmıştır. Bir özniteliğiyle birlikte kullanıldığında, **uygulamayı Uygula** özelliğini **yalnızca nesne oluşturma sırasında**olarak ayarlayın.
> - Bu işlev şu anda yalnızca "Workday for User sağlamasını Active Directory" için desteklenir. Diğer sağlama uygulamalarıyla birlikte kullanılamaz. 


**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **uniqueValueRule1 ... uniqueValueRuleN** |En az 2 gerekir, üst sınır yoktur |Dize | Değerlendirilecek benzersiz değer oluşturma kurallarının listesi. |


---
### <a name="singleapproleassignment"></a>Singleapprotaatama
**Çalışmayacaktır**<br> Singleapprotaatama ([Approtaatamalar])

**Açıklaması**<br> Belirli bir uygulama için kullanıcıya atanan tüm Approtaatamalar listesinden tek bir Approtaatama döndürür. Approtaatamalar nesnesini tek bir rol adı dizesine dönüştürmek için bu işlev gereklidir. En iyi yöntem, tek seferde bir kullanıcıya yalnızca bir Approelatama atandığından emin olmak ve birden çok rol atanmamışsa döndürülen rol dizesinin tahmin edilebilir olamayacağını unutmayın. 

**Parametrelere**<br> 

  | Adı | Gerekli/yinelenen | Tür | Notlar |
  |--- | --- | --- | --- |
  | **AppRoleAssignments** |Gereklidir |Dize |**[Approtaatamalar]** nesnesi. |

---
### <a name="split"></a>Bölme
**Çalışmayacaktır**<br> Böl (kaynak, sınırlayıcı)

**Açıklaması**<br> Belirtilen sınırlayıcı karakteri kullanarak bir dizeyi çok değerli bir diziye böler.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Güncelleştirilecek **kaynak** değeri. |
   | **ayırıcı** |Gereklidir |Dize |Dizeyi ayırmak için kullanılacak karakteri belirtir (örneğin: ",") |

---
### <a name="stringfromsid"></a>StringFromSid
**Açıklaması**  
StringFromSid işlevi, bir güvenlik tanımlayıcısı içeren bir bayt dizisini dizeye dönüştürür.

**Sözdizimi**  
`str StringFromSid(bin ObjectSID)`  

---
### <a name="stripspaces"></a>StripSpaces
**Çalışmayacaktır**<br> StripSpaces (kaynak)

**Açıklaması**<br> Kaynak dizeden tüm boşluk ("") karakterlerini kaldırır.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Güncelleştirilecek **kaynak** değeri. |

---
### <a name="switch"></a>Anahtar
**Çalışmayacaktır**<br> Anahtar (kaynak, defaultValue, KEY1, değer1, key2, değer2,...)

**Açıklaması**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtara eşleşmezse, **DefaultValue**döndürür.  **Anahtar** ve **değer** parametrelerinin her zaman çiftler halinde gelmesi gerekir. İşlev her zaman çift sayıda parametre bekler.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Güncelleştirilecek **kaynak** değeri. |
   | **Değerinin** |İsteğe Bağlı |Dize |Kaynak herhangi bir anahtara eşleşmezse kullanılacak varsayılan değer. Boş dize ("") olabilir. |
   | **anahtar** |Gereklidir |Dize |**Kaynak** değeri Karşılaştırılacak **anahtar** . |
   | **value** |Gereklidir |Dize |Anahtarla eşleşen **kaynak** için değiştirme değeri. |

---
### <a name="tolower"></a>ToLower
**Çalışmayacaktır**<br> ToLower (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

   | Adı | Gerekli/yinelenen | Tür | Notlar |
   | --- | --- | --- | --- |
   | **kaynaktaki** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı |
   | **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---

### <a name="toupper"></a>ToUpper
**Çalışmayacaktır**<br> ToUpper (kaynak, kültür)

**Açıklaması**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Belirtilen bir *kültür* bilgisi yoksa, sabit kültür kullanacaktır.

**Parametrelere**<br> 

  | Adı | Gerekli/yinelenen | Tür | Notlar |
  | --- | --- | --- | --- |
  | **kaynaktaki** |Gereklidir |Dize |Genellikle kaynak nesneden özniteliğin adı. |
  | **ayarı** |İsteğe Bağlı |Dize |RFC 4646 ' i temel alan kültür adı biçimi *languagecode2-Country/regioncode2*, burada *languagecode2* iki harfli dil kodu ve *Ülke/regioncode2* ise iki harfli alt kültür kodudur. Japonca (Japonya) için ja-JP ve Ingilizce (Birleşik Devletler) için en-US sayılabilir. İki harfli dil kodunun kullanılamadığı durumlarda ISO 639-2 ' den türetilen üç harfli bir kod kullanılır.|

---

### <a name="trim"></a>kırpma
**Açıklaması**  
Trim işlevi bir dizeden baştaki ve sondaki boşlukları kaldırır.

**Sözdizimi**  
`str Trim(str value)`  

**Örnek:**  
`Trim(" Test ")`  
"Test" döndürür.

`Trim([proxyAddresses])`  
ProxyAddress özniteliğinde her bir değer için öndeki ve sondaki boşlukları kaldırır.

---
### <a name="word"></a>Word
**Açıklaması**  
Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük numarasını açıklayan parametrelere göre dize içinde içerilen bir sözcük döndürür.

**Sözdizimi**  
`str Word(str string, num WordNumber, str delimiters)`

* String: bir sözcüğün döndürdüğü dize.
* WordNumber: hangi sözcük sayısının dönmesi gerektiğini belirleyen bir sayı.
* Sınırlayıcılar: sözcükleri tanımlamak için kullanılması gereken sınırlayıcıları temsil eden bir dize

**Açıklamalarının**  
Sınırlayıcıdaki karakterlerden biri tarafından ayrılan dizedeki her karakter dizesi, sözcük olarak tanımlanır:

* Sayı < 1 ise boş bir dize döndürür.
* Dize null ise, boş bir dize döndürür.

Dize sayı olan sözcüklerden daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan herhangi bir sözcük içermiyorsa, boş bir dize döndürülür.

**Örnek:**  
`Word("The quick brown fox",3," ")`  
"Kahverengi" döndürür

`Word("This,string!has&many separators",3,",!&#")`  
"Sahip" döndürmelidir

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
* **Çıkış**: John.Smith@contoso.com UPN değeri zaten dizinde yoksa "John.Smith@contoso.com"
* **Çıkış**: John.Smith@contoso.com UPN değeri zaten dizinde varsa "J.Smith@contoso.com"
* **Çıkış**: YUKARıDAKI iki UPN değeri dizinde zaten mevcutsa "Jo.Smith@contoso.com"


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
