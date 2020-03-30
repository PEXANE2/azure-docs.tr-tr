---
title: Azure AD Connect bulut sağlama ifadeleri ve işlev başvurusu
description: reference
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
ms.openlocfilehash: 51c14fd7f427c29c47521a7355309e62ab2254ca
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78298624"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Etkin Dizini'nde öznitelik eşlemeleri için ifadeler yazma
Bulut sağlamayı yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemetürtür türlerinden biri bir ifade eşlemidir. 

İfade eşlemi, komut dosyası benzeri bir ifade kullanarak öznitelikleri özelleştirmenize olanak tanır.  Bu, şirket içi verileri yeni veya farklı bir değere dönüştürmenize olanak tanır.  Örneğin, bu tek öznitelik bulut uygulamalarınızdan biri tarafından kullanıldığından, iki özniteliği tek bir öznitelikte birleştirmek isteyebilirsiniz.

Aşağıdaki belge, verileri dönüştürmek için kullanılan komut dosyası benzeri ifadeleri kapsayacaktır.  Bu işlemin yalnızca bir parçasıdır.  Daha sonra bu ifadeyi kullanmanız ve kiracınıza bir web isteğinde yerleştirmeniz gerekir.  Bu konuda daha fazla bilgi için [dönüşümler](how-to-transformation.md)

## <a name="syntax-overview"></a>Sözdizimine genel bakış
Öznitelik Eşlemeleri için İfadeler sözdizimi, Visual Basic for Applications (VBA) işlevlerini anımsatır.

* İfadenin tamamı, parantez içinde bağımsız değişkenlerin ardından gelen bir addan oluşan işlevler açısından tanımlanmalıdır: <br>
  *Fonksiyon`<<argument 1>>`Adı(`<<argument N>>`, )*
* İşlevleri birbirinuzun içine yerleştirmeniz gerekebilir. Örnek: <br> *FunctionOne(FunctionTwo(`<<argument1>>`))*
* Üç farklı türde bağımsız değişkeni işlevlere geçirebilirsiniz:
  
  1. Öznitelikleri, kare parantez içinde eklenmelidir. Örneğin: [öznitelikName]
  2. Çift tırnak içinde eklenmelidir string sabitleri. Örneğin: "Amerika Birleşik Devletleri"
  3. Diğer Fonksiyonlar. Örneğin: FunctionOne(`<<argument1>>`, FunctionTwo(`<<argument2>>`))
* Dize sabitleri için, dizedeki bir ters eğik çizgi ( \ ) veya tırnak işaretine ( " ) ihtiyacınız varsa, ters eğik çizgi ( \ ) simgesiyle birlikte kaçmış olmalıdır. Örneğin: "Şirket adı: \\"Contoso\\""

## <a name="list-of-functions"></a>Fonksiyonlar listesi
| Fonksiyonlar listesi | Açıklama |
|-----|----|
|[Ekle](#append)|Bir kaynak dize değeri alır ve sonuna kadar soneki ekler.|
|[BitAnd](#bitand)|BitAnd işlevi bir değer üzerinde belirtilen bitleri ayarlar.|
|[Cbool](#cbool)|CBool fonksiyonu değerlendirilen ifadeye dayalı bir Boolean döndürür|
|[ConvertFromBase64](#convertfrombase64)|ConvertFromBase64 işlevi belirtilen base64 kodlanmış değeri normal bir dize dönüştürür.|
|[ConverttoBase64](#converttobase64)|ConvertToBase64 işlevi bir dizeyi Unicode base64 dizesine dönüştürür. |
|[ConvertToUTF8Hex](#converttoutf8hex)|ConvertToUTF8Hex işlevi bir dizeyi UTF8 Hex kodlanmış değere dönüştürür.|
|[Sayısı](#count)|Count işlevi çok değerli bir öznitelikteki eleman sayısını döndürür|
|[Cstr](#cstr)|CStr işlevi dize veri türüne dönüşür.|
|[TarihFromNum](#datefromnum)|DateFromNum işlevi, AD'nin tarih biçimindeki bir değeri DateTime türüne dönüştürür.|
|[DNComponent](#dncomponent)|DNComponent işlevi soldan giden belirli bir DN bileşeninin değerini döndürür.|
|[Hata](#error)|Hata işlevi özel bir hata döndürmek için kullanılır.|
|[BiçimDateTime](#formatdatetime) |Bir tarih dizesini tek bir biçimden alır ve farklı bir biçime dönüştürür.| 
|[Guıd](#guid)|Guid işlevi yeni bir rasgele GUID oluşturur.|           
|[ııf](#iif)|IIF işlevi, belirtilen durumu temel alan olası değerler kümesinden birini döndürür.|
|[ınstr](#instr)|InStr işlevi, bir dizedeki bir alt dizenin ilk oluşumunu bulur.|
|[ısnull](#isnull)|İfade Null'a göre değerlendirilirse, IsNull işlevi doğru döndürür.|
|[ısnullorempty](#isnullorempty)|İfade null veya boş bir dize ise, O zaman IsNullOrEmpty işlevi doğru döndürür.|         
|[ıspresent](#ispresent)|İfade Null olmayan ve boş olmayan bir dize değerlendirirse, IsPresent işlevi doğru döndürür.|    
|[IsString](#isstring)|İfade bir dize türüne değerlendirilebilirse, IsString işlevi True olarak değerlendirilir.|
|[Öğe](#item)|Öğe işlevi, çok değerli bir dize/öznitelikten bir öğeyi döndürür.|
|[Katıl](#join) |Join() Append() ile benzer, ancak birden çok **kaynak** dize değerlerini tek bir dizede birleştirebiliyor ve her değer bir **ayırıcı** dize ile ayrılacak.| 
|[Sol](#left)|Sol işlev, dizesin solundan belirli sayıda karakter döndürür.|
|[Orta](#mid) |Kaynak değerinin bir alt dizesini döndürür. Alt dize, kaynak dizedeki karakterlerin yalnızca bazılarını içeren bir dizedir.|
|[NormalizeDiacritics](#normalizediacritics)|Bir dize bağımsız değişkeni gerektirir. Dize döndürür, ancak herhangi bir akdekritik karakter eşdeğer non-akdekritik karakterler ile değiştirilir.|
|[Değil](#not) |**Kaynağın**boolean değerini çevirir. **Kaynak** değeri "*True*" ise, "*False*" döndürür. Aksi takdirde, "*True*" döndürür.| 
|[Removeduplicates](#removeduplicates)|RemoveDuplicates işlevi çok değerli bir dize alır ve her değerin benzersiz olduğundan emin olun.| 
|[Değiştirmek](#replace) |Dize içindeki değerleri değiştirir. | 
|[SelectUniqueValue](#selectuniquevalue)|İfadeler kullanılarak tanımlanan benzersiz değer oluşturma kuralları olan en az iki bağımsız değişken gerektirir. İşlev her kuralı değerlendirir ve hedef uygulama/dizinde benzersizlik için oluşturulan değeri denetler.| 
|[SingleAppRoleAssignment](#singleapproleassignment)|Belirli bir uygulama için bir kullanıcıya atanan tüm uygulamaRol Atamaları listesinden tek bir uygulamaRoleAssignment döndürür.| 
|[Bölme](#split)|Belirtilen sınır dışı layıcı karakterini kullanarak dizeyi çok değerli bir diziye böler.|
|[StringFromSID](#stringfromsid)|StringFromSid işlevi, güvenlik tanımlayıcısı içeren bir bayt dizisini bir dize dönüştürür.| 
|[Şerit Alanları](#stripspaces) |Kaynak dizedeki tüm boşluk (" ") karakterlerini kaldırır.| 
|[Anahtarı](#switch)|**Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. | 
|[Tolower](#tolower)|Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür.| 
|[Toupper](#toupper)|Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür.|
|[Döşeme](#trim)|Kırpma işlevi, bir dizeden önde gelen ve sondaki beyaz boşlukları kaldırır.|
|[Word](#word)|Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük sayısını açıklayan parametrelere dayalı olarak dize içinde bulunan bir sözcüğü döndürür.|

---
### <a name="append"></a>Ekle
**Işlev:**<br> Ek(kaynak, sonek)

**Açıklama:**<br> Bir kaynak dize değeri alır ve sonuna kadar soneki ekler.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden öznitelik adı. |
   | **Soneki** |Gerekli |Dize |Kaynak değerinin sonuna eklemek istediğiniz dize. |

---
### <a name="bitand"></a>BitAnd
**Açıklama:**  
BitAnd işlevi bir değer üzerinde belirtilen bitleri ayarlar.

**Sözdizimi:**  
`num BitAnd(num value1, num value2)`

* value1, value2: birlikte ve'ed olmalıdır sayısal değerler

**Açıklamalar:**  
Bu işlev her iki parametreyi de ikili gösterime dönüştürür ve aşağıdakilere göre biraz ayarlar:

* 0 - *değer1* ve *value2'deki* karşılık gelen bitlerden biri veya her ikisi 0 ise
* 1 - karşılık gelen bitlerin her ikisi de 1 ise.

Başka bir deyişle, her iki parametrenin karşılık gelen bitleri 1 dışında tüm durumlarda 0 döndürür.

**Örnek:**  
 
 `BitAnd(&HF, &HF7)`</br>
 Hexadecimal "F" VE "F7" bu değere değer biçilebildiği için 7 döndürür.

---

### <a name="cbool"></a>Cbool
**Açıklama:**  
CBool fonksiyonu değerlendirilen ifadeye dayalı bir Boolean döndürür

**Sözdizimi:**  
`bool CBool(exp Expression)`

**Açıklamalar:**  
İfade sıfır olmayan bir değere göre değerlendirirse, CBool True döndürür, aksi takdirde False döndürür.

**Örnek:**  
`CBool([attrib1] = [attrib2])`  

Her iki öznitelik de aynı değere sahipse True döndürür.

---
### <a name="convertfrombase64"></a>ConvertFromBase64
**Açıklama:**  
ConvertFromBase64 işlevi belirtilen base64 kodlanmış değeri normal bir dize dönüştürür.

**Sözdizimi:**  
`str ConvertFromBase64(str source)`- kodlama için Unicode varsayar  
`str ConvertFromBase64(str source, enum Encoding)`

* kaynak: Base64 kodlanmış dize  
* Kodlama: Unicode, ASCII, UTF8

**Örnek**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Her iki örnek de "*Merhaba dünya!*"

---
### <a name="converttobase64"></a>ConverttoBase64
**Açıklama:**  
ConvertToBase64 işlevi bir dizeyi Unicode base64 dizesine dönüştürür.  
Bir dizi tümsayenin değerini, taban-64 basamaklarla kodlanmış eşdeğer dize gösterimine dönüştürür.

**Sözdizimi:**  
`str ConvertToBase64(str source)`

**Örnek:**  
`ConvertToBase64("Hello world!")`  
"SABlAGwAbABvACAAdwBvAHIAbABkACEA"

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Açıklama:**  
ConvertToUTF8Hex işlevi bir dizeyi UTF8 Hex kodlanmış değere dönüştürür.

**Sözdizimi:**  
`str ConvertToUTF8Hex(str source)`

**Açıklamalar:**  
Bu işlevin çıktı biçimi, Azure Active Directory tarafından DN öznitelik biçimi olarak kullanılır.

**Örnek:**  
`ConvertToUTF8Hex("Hello world!")`  
İade 48656C6C6F20776F726C6421

---
### <a name="count"></a>Sayı
**Açıklama:**  
Count işlevi çok değerli bir öznitelikteki eleman sayısını döndürür

**Sözdizimi:**  
`num Count(mvstr attribute)`

---
### <a name="cstr"></a>Cstr
**Açıklama:**  
CStr işlevi dize veri türüne dönüşür.

**Sözdizimi:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* değer: Sayısal bir değer, referans özniteliği veya Boolean olabilir.

**Örnek:**  
`CStr([dn])`  
"cn=Joe,dc=contoso,dc=com" dönebilir

---
### <a name="datefromnum"></a>TarihFromNum
**Açıklama:**  
DateFromNum işlevi, AD'nin tarih biçimindeki bir değeri DateTime türüne dönüştürür.

**Sözdizimi:**  
`dt DateFromNum(num value)`

**Örnek:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
2012-01-01 23:00:00'ı temsil eden bir DateTime döndürür

---
### <a name="dncomponent"></a>DNComponent
**Açıklama:**  
DNComponent işlevi soldan giden belirli bir DN bileşeninin değerini döndürür.

**Sözdizimi:**  
`str DNComponent(ref dn, num ComponentNumber)`

* dn: yorumlamak için referans özniteliği
* ComponentNumber: DN'deki bileşenin döndürülecek

**Örnek:**  
`DNComponent(CRef([dn]),1)`  
Eğer dn "cn=Joe,ou=...," joe döndürür

---
### <a name="error"></a>Hata
**Açıklama:**  
Hata işlevi özel bir hata döndürmek için kullanılır.

**Sözdizimi:**  
`void Error(str ErrorMessage)`

**Örnek:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Öznitelik hesabı Adı yoksa, nesneye bir hata atın.

---
### <a name="formatdatetime"></a>BiçimDateTime
**Işlev:**<br> FormatDateTime(kaynak, inputFormat, outputFormat)

**Açıklama:**<br> Bir tarih dizesini tek bir biçimden alır ve farklı bir biçime dönüştürür.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden öznitelik adı. |
   | **inputFormat** |Gerekli |Dize |Kaynak değerinin beklenen biçimi. Desteklenen biçimler için [https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx)bkz. |
   | **outputFormat** |Gerekli |Dize |Çıkış tarihinin biçimi. |

---
### <a name="guid"></a>Guid
**Açıklama:**  
Guid işlevi yeni bir rasgele GUID oluşturur

**Sözdizimi:**  
`str Guid()`

---
### <a name="iif"></a>ııf
**Açıklama:**  
IIF işlevi, belirtilen durumu temel alan olası değerler kümesinden birini döndürür.

**Sözdizimi:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* durum: doğru veya yanlış olarak değerlendirilebilecek herhangi bir değer veya ifade.
* valueIfTrue: Koşul doğru değerlendirirse, döndürülen değer.
* valueIfFalse: Koşul false olarak değerlendirilirse, döndürülen değer.

**Örnek:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Kullanıcı bir stajyerse, bir kullanıcının takma adını başına "t-" eklenmiştir, aksi takdirde kullanıcının diğer adını olduğu gibi döndürür.

---
### <a name="instr"></a>ınstr
**Açıklama:**  
InStr işlevi bir dize bir alt dize ilk oluşumunu bulur

**Sözdizimi:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: aranacak dize
* stringmatch: string bulunacak
* start: substring bulmak için başlangıç pozisyonu
* karşılaştırın: vbTextCompare veya vbBinaryKarşılaştır

**Açıklamalar:**  
Substring'in bulunduğu konumu veya yoksa 0'ı döndürür.

**Örnek:**  
`InStr("The quick brown fox","quick")`  
Evalues için 5

`InStr("repEated","e",3,vbBinaryCompare)`  
7'ye göre değerlendirir

---
### <a name="isnull"></a>ısnull
**Açıklama:**  
İfade Null'a göre değerlendirilirse, IsNull işlevi doğru döndürür.

**Sözdizimi:**  
`bool IsNull(var Expression)`

**Açıklamalar:**  
Bir öznitelik için, bir Null öznitelik yokluğu ile ifade edilir.

**Örnek:**  
`IsNull([displayName])`  
Özellik CS veya MV'de yoksa True döndürür.

---
### <a name="isnullorempty"></a>ısnullorempty
**Açıklama:**  
İfade null veya boş bir dize ise, O zaman IsNullOrEmpty işlevi doğru döndürür.

**Sözdizimi:**  
`bool IsNullOrEmpty(var Expression)`

**Açıklamalar:**  
Bir öznitelik için, öznitelik yoksa veya yoksa ancak boş bir dize yse, bu True'ya değerlenir.  
Bu işlevin tersi IsPresent olarak adlandırılır.

**Örnek:**  
`IsNullOrEmpty([displayName])`  
Öznitelik yoksa veya CS veya MV'de boş bir dizeyse True döndürür.

---
### <a name="ispresent"></a>ıspresent
**Açıklama:**  
İfade Null olmayan ve boş olmayan bir dize değerlendirirse, IsPresent işlevi doğru döndürür.

**Sözdizimi:**  
`bool IsPresent(var expression)`

**Açıklamalar:**  
Bu işlevin tersi IsNullOrEmpty olarak adlandırılır.

**Örnek:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

---
### <a name="item"></a>Öğe
**Açıklama:**  
Öğe işlevi, çok değerli bir dize/öznitelikten bir öğeyi döndürür.

**Sözdizimi:**  
`var Item(mvstr attribute, num index)`

* öznitelik: çok değerli öznitelik
* dizin: çok değerli dize bir öğeye dizin.

**Açıklamalar:**  
İkinci işlev dizini çok değerli öznitelikteki bir öğeye döndürdüğünden, Öğe işlevi İçer işleviile birlikte kullanışlıdır.

Dizin sınırların dışındaysa hata atar.

**Örnek:**  
`Mid(Item([proxyAddresses],Contains([proxyAddresses], "SMTP:")),6)`  
Birincil e-posta adresini döndürür.

---
### <a name="isstring"></a>IsString
**Açıklama:**  
İfade bir dize türüne değerlendirilebilirse, IsString işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsString(var expression)`

**Açıklamalar:**  
CStr() ifadesini ayrıştırmak için başarılı olup olmadığını belirlemek için kullanılır.

---
### <a name="join"></a>Birleştir
**Işlev:**<br> Birleştirme(ayırıcı, kaynak1, kaynak2, ...)

**Açıklama:**<br> Join() Append() ile benzer, ancak birden çok **kaynak** dize değerlerini tek bir dizede birleştirebiliyor ve her değer bir **ayırıcı** dize ile ayrılacak.

Kaynak değerlerinden biri çok değerli bir öznitelikse, bu öznitelikteki her değer birleştirilir ve ayırıcı değerle ayrılır.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Ayırıcı** |Gerekli |Dize |Bir dize içine sıkıştırılmış zaman kaynak değerlerini ayırmak için kullanılan dize. Ayırıcı yoksa "" olabilir. |
   | **kaynak1 ... kaynakN** |Gerekli, değişken sayı |Dize |Dize değerleri biraraya getirilecek. |

---
### <a name="left"></a>Sol
**Açıklama:**  
Sol işlev, dizesin solundan belirli sayıda karakter döndürür.

**Sözdizimi:**  
`str Left(str string, num NumChars)`

* string: karakterleri döndürmek için dize
* NumChars: dize başından (sol) dönmek için karakter sayısını tanımlayan bir sayı

**Açıklamalar:**  
Dizedeki ilk numChars karakterlerini içeren bir dize:

* numChars = 0 ise, boş dize döndürün.
* numChars 0'ı <, giriş dizelerini döndürün.
* Dize null ise, boş dize döndürün.

Dize numChars belirtilen sayıdan daha az karakter içeriyorsa, dize ile aynı bir dize (yani parametre 1'deki tüm karakterleri içeren) döndürülür.

**Örnek:**  
`Left("John Doe", 3)`  
`Joh` döndürür.

---
### <a name="mid"></a>Orta
**Işlev:**<br> Orta (kaynak, başlangıç, uzunluk)

**Açıklama:**<br> Kaynak değerinin bir alt dizesini döndürür. Alt dize, kaynak dizedeki karakterlerin yalnızca bazılarını içeren bir dizedir.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |Genellikle öznitelik adı. |
   | **Başlatmak** |Gerekli |integer |Alt dize başlatılması gereken **kaynak** dize dizin. Dizedeki ilk karakter 1 dizin, ikinci karakter dizin 2 ve benzeri olacaktır. |
   | **Uzun -luğu** |Gerekli |integer |Alt dizenin uzunluğu. Uzunluk **kaynak** dizesin dışında biterse, işlev **başlangıç** dizininden **kaynak** dize sonuna kadar substring döndürür. |

---
### <a name="normalizediacritics"></a>NormalizeDiacritics
**Işlev:**<br> NormalizeDiacritics(kaynak)

**Açıklama:**<br> Bir dize bağımsız değişkeni gerektirir. Dize döndürür, ancak herhangi bir akdekritik karakter eşdeğer non-akdekritik karakterler ile değiştirilir. Genellikle, aksan karakterleri (vurgu işaretleri) içeren ad ve soyadları, kullanıcı anapara adları, SAM hesap adları ve e-posta adresleri gibi çeşitli kullanıcı tanımlayıcılarında kullanılabilecek yasal değerlere dönüştürmek için kullanılır.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize | Genellikle bir ad veya soyadı özniteliği. |

---
### <a name="not"></a>Not
**Işlev:**<br> Değil(kaynak)

**Açıklama:**<br> **Kaynağın**boolean değerini çevirir. **Kaynak** değeri "*True*" ise, "*False*" döndürür. Aksi takdirde, "*True*" döndürür.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Boolean String |Beklenen **kaynak** değerleri "True" veya "False" şeklindedir. |

---
### <a name="removeduplicates"></a>Removeduplicates
**Açıklama:**  
RemoveDuplicates işlevi çok değerli bir dize alır ve her değerin benzersiz olduğundan emin olun.

**Sözdizimi:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Örnek:**  
`RemoveDuplicates([proxyAddresses])`  
Tüm yinelenen değerlerin kaldırıldığı bir dezenfekte proxyAddress özniteliği döndürür.

---
### <a name="replace"></a>Değiştir
**Işlev:**<br> Değiştir(kaynak, oldValue, regexPattern, regexGroupName, replaceValue, replacementAttributeName, şablon)

**Açıklama:**<br>
Dize içindeki değerleri değiştirir. Sağlanan parametrelere bağlı olarak farklı çalışır:

* **OldValue** ve **replacementValue** sağlandığında:
  
  * **Kaynaktaki** **oldValue'ın** tüm oluşumlarını **replaceValue** ile değiştirir
* **oldValue** ve **şablon** sağlandığında:
  
  * **Şablondaki** **eski Değer'in** tüm oluşumlarını **kaynak** değeriyle değiştirir
* **RegexPattern** ve **replacementValue** sağlandığında:

  * İşlev **kaynak** dize **regexPattern** uygular ve **değiştirmeDeğer** için dize oluşturmak için regex grup adlarını kullanabilirsiniz
* **RegexPattern**, **regexGroupName**, **replacementValue** sağlandığında:
  
  * İşlev **kaynak** **dize regexPattern** uygular ve **değiştirmeValue** ile **regexGroupName** eşleşen tüm değerleri değiştirir
* **RegexPattern**, **regexGroupName**, **replacementAttributeName** sağlandığında:
  
  * **Kaynağın** değeri yoksa, **kaynak** döndürülür
  * **Kaynak** bir değere sahipse, işlev **regexPattern'i** **kaynak** dizesine uygular ve **regexGroupName** ile eşleşen tüm değerleri **değiştirmeAttributeName** ile ilişkili değerle değiştirir

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |Genellikle **kaynak** nesneden öznitelik adı. |
   | **Oldvalue** |İsteğe bağlı |Dize |**Kaynak** veya **şablonda**değiştirilecek değer. |
   | **regexPattern** |İsteğe bağlı |Dize |**Kaynakta**değiştirilecek değer için Regex deseni. Veya, **değiştirmePropertyName** kullanıldığında, **deseni değiştirmePropertyName**değer ayıklamak için . |
   | **regexGroupName** |İsteğe bağlı |Dize |**RegexPattern**içindeki grubun adı. Sadece **değiştirmePropertyName** kullanıldığında, biz **değiştirmeValue** olarak bu grubun değerini ayıklamak **gerekirPropertyName**. |
   | **değiştirmeDeğer** |İsteğe bağlı |Dize |Eskisini değiştirmek için yeni bir değer. |
   | **değiştirmeAttributeName** |İsteğe bağlı |Dize |Değiştirme değeri için kullanılacak özniteliğin adı |
   | **şablon** |İsteğe bağlı |Dize |**Şablon** değeri sağlandığında, şablonun içindeki **oldValue'ı** arar ve **kaynak** değeriyle değiştiririz. |

---
### <a name="selectuniquevalue"></a>SelectUniqueValue
**Işlev:**<br> SelectUniqueValue(uniqueValueRule1, uniqueValueRule2, uniqueValueRule3, ...)

**Açıklama:**<br> İfadeler kullanılarak tanımlanan benzersiz değer oluşturma kuralları olan en az iki bağımsız değişken gerektirir. İşlev her kuralı değerlendirir ve hedef uygulama/dizinde benzersizlik için oluşturulan değeri denetler. Bulunan ilk benzersiz değer döndürülen değer olacaktır. Tüm değerler hedefte zaten varsa, giriş emanet alır ve nedeni denetim günlüklerinde günlüğe kaydedilir. Sağlanabilir bağımsız değişken sayısına bir üst sınır yoktur.

> [!NOTE]
> - Bu üst düzey bir işlevdir, iç içe geçemez.
> - Bu işlev, eşleşen bir önceliğe sahip özniteliklere uygulanamaz.  
> - Bu işlev yalnızca giriş oluşturmaları için kullanılmak üzere dir. Bir öznitelik ile kullanırken, **Nesne oluşturma sırasında Yalnızca** **Eşleme Uygula** özelliğini ayarlayın.
> - Bu işlev şu anda yalnızca "Active Directory User Provisioning için Çalışma Günü" için desteklenir. Diğer sağlama uygulamaları ile kullanılamaz. 


**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **uniqueValueRule1 ... uniqueValueRuleN** |En az 2 gereklidir, üst sınır yoktur |Dize | Değerlendirilecek benzersiz değer oluşturma kurallarılistesi. |


---
### <a name="singleapproleassignment"></a>SingleAppRoleAssignment
**Işlev:**<br> SingleAppRoleAssignment([appRoleAssignments])

**Açıklama:**<br> Belirli bir uygulama için bir kullanıcıya atanan tüm uygulamaRol Atamaları listesinden tek bir uygulamaRoleAssignment döndürür. Bu işlev, appRoleAssignments nesnesini tek bir rol adı dizesine dönüştürmek için gereklidir. En iyi uygulamanın, aynı anda bir kullanıcıya yalnızca bir uygulamaRoleAssignment atanmasını sağlamak olduğunu ve birden çok role atandığında döndürülen rol dizesi öngörülebilir olmayabilir. 

**Parametre:**<br> 

  | Adı | Gerekli/ Yineleme | Tür | Notlar |
  |--- | --- | --- | --- |
  | **[appRoleAssignments]** |Gerekli |Dize |**[appRoleAssignments] nesnesi.** |

---
### <a name="split"></a>Bölme
**Işlev:**<br> Bölme (kaynak, sınır layıcı)

**Açıklama:**<br> Belirtilen sınır dışı layıcı karakterini kullanarak dizeyi çok değerli bir diziye böler.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |**kaynak** değeri güncelleştirin. |
   | **Sınırlayıcı** |Gerekli |Dize |Dizeyi bölmek için kullanılacak karakteri belirtir (örnek: ",") |

---
### <a name="stringfromsid"></a>StringFromSid
**Açıklama:**  
StringFromSid işlevi, güvenlik tanımlayıcısı içeren bir bayt dizisini bir dize dönüştürür.

**Sözdizimi:**  
`str StringFromSid(bin ObjectSID)`  

---
### <a name="stripspaces"></a>Şerit Alanları
**Işlev:**<br> StripSpaces(kaynak)

**Açıklama:**<br> Kaynak dizedeki tüm boşluk (" ") karakterlerini kaldırır.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |**kaynak** değeri güncelleştirin. |

---
### <a name="switch"></a>Anahtar
**Işlev:**<br> Anahtar(kaynak, defaultValue, key1, value1, key2, value2, ...)

**Açıklama:**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtarla eşleşmiyorsa, **varsayılan Değer**döndürür.  **Anahtar** ve **değer** parametreleri her zaman çiftler halinde gelmelidir. İşlev her zaman eşit sayıda parametre bekler.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |**Denetlenen kaynak** değeri. |
   | **defaultDeğer** |İsteğe bağlı |Dize |Kaynak anahtarla eşleşmediğinde kullanılacak varsayılan değer. Boş dize ("") olabilir. |
   | **anahtar** |Gerekli |Dize |**Kaynak** değerini karşılaştırmak için **anahtar.** |
   | **value** |Gerekli |Dize |Anahtarla eşleşen **kaynak** için değiştirme değeri. |

---
### <a name="tolower"></a>Tolower
**Işlev:**<br> ToLower (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak küçük harfe dönüştürür. Kültür *bilgisi* belirtilmemişse, değişmez kültürü kullanır.

**Parametre:**<br> 

   | Adı | Gerekli/ Yineleme | Tür | Notlar |
   | --- | --- | --- | --- |
   | **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden öznitelik adı |
   | **kültür** |İsteğe bağlı |Dize |RFC 4646'yı temel alan kültür adının biçimi, languagecode2'nin iki harfli dil kodu, *ülke/bölgekodu2'nin* ise iki harfli alt kültür kodu olduğu *languagecode2-country/regioncode2'dir.* *languagecode2* Japonca (Japonya) için ja-JP ve İngilizce (Amerika Birleşik Devletleri) için en-US örnekleri verilebilir. İki harfli bir dil kodunun kullanılamadığı durumlarda, ISO 639-2'den türetilen üç harfli bir kod kullanılır.|

---

### <a name="toupper"></a>Toupper
**Işlev:**<br> ToUpper (kaynak, kültür)

**Açıklama:**<br> Bir *kaynak* dize değeri alır ve belirtilen kültür kurallarını kullanarak büyük harfe dönüştürür. Kültür *bilgisi* belirtilmemişse, değişmez kültürü kullanır.

**Parametre:**<br> 

  | Adı | Gerekli/ Yineleme | Tür | Notlar |
  | --- | --- | --- | --- |
  | **Kaynak** |Gerekli |Dize |Genellikle kaynak nesneden öznitelik adı. |
  | **kültür** |İsteğe bağlı |Dize |RFC 4646'yı temel alan kültür adının biçimi, languagecode2'nin iki harfli dil kodu, *ülke/bölgekodu2'nin* ise iki harfli alt kültür kodu olduğu *languagecode2-country/regioncode2'dir.* *languagecode2* Japonca (Japonya) için ja-JP ve İngilizce (Amerika Birleşik Devletleri) için en-US örnekleri verilebilir. İki harfli bir dil kodunun kullanılamadığı durumlarda, ISO 639-2'den türetilen üç harfli bir kod kullanılır.|

---

### <a name="trim"></a>Trim
**Açıklama:**  
Kırpma işlevi, bir dizeden önde gelen ve sondaki beyaz boşlukları kaldırır.

**Sözdizimi:**  
`str Trim(str value)`  

**Örnek:**  
`Trim(" Test ")`  
"Test"i döndürür.

`Trim([proxyAddresses])`  
ProxyAddress özniteliğindeki her değer için satır aralığı ve sondaki boşlukları kaldırır.

---
### <a name="word"></a>Word
**Açıklama:**  
Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük sayısını açıklayan parametrelere dayalı olarak dize içinde bulunan bir sözcüğü döndürür.

**Sözdizimi:**  
`str Word(str string, num WordNumber, str delimiters)`

* string: bir kelime dönmek için dize.
* WordNumber: hangi sözcük numarasının döndürülmesi gerektiğini tanımlayan bir sayı.
* delimiters: kelimeleri tanımlamak için kullanılması gereken delimiter(ler) temsil eden bir dize

**Açıklamalar:**  
Delimiters karakterlerden biri tarafından ayrılmış dize karakter her dize sözcükler olarak tanımlanır:

* 1 < sayısı varsa, boş dize döndürür.
* Dize null ise, boş dize döndürür.

Dize sayı sözcükten daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan sözcükleri içermiyorsa, boş bir dize döndürülür.

**Örnek:**  
`Word("The quick brown fox",3," ")`  
"Kahverengi" döndürür

`Word("This,string!has&many separators",3,",!&#")`  
"has" dönecek

## <a name="examples"></a>Örnekler
### <a name="strip-known-domain-name"></a>Bilinen etki alanı adını şeritle
Bir kullanıcı adı edinmek için bilinen bir etki alanı adını kullanıcının e-postasından çıkarmanız gerekir. <br>
Örneğin, etki alanı "contoso.com" ise, aşağıdaki ifadeyi kullanabilirsiniz:

**Ifa -de:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**Örnek giriş / çıktı:** <br>

* **GIRIŞ** (posta):john.doe@contoso.com" "
* **ÇIKIŞ**: "john.doe"

### <a name="append-constant-suffix-to-user-name"></a>Kullanıcı adına sabit sonek ekle
Salesforce Sandbox kullanıyorsanız, eşitlemeden önce tüm kullanıcı adlarınıza ek bir son eklenmeniz gerekebilir.

**Ifa -de:** <br>
`Append([userPrincipalName], ".test")`

**Örnek giriş/çıktı:** <br>

* **GIRIŞ**: (userPrincipalName):John.Doe@contoso.com" "
* **ÇıKTı**:John.Doe@contoso.com.test" "

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Ad ve soyad parçaları yla biraraya gelirken kullanıcı takma adı oluşturma
Kullanıcının adının ilk 3 harfini ve kullanıcının soyadının ilk 5 harfini alarak bir kullanıcı takma adı oluşturmanız gerekir.

**Ifa -de:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Örnek giriş/çıktı:** <br>

* **INPUT** (givenName): "John"
* **INPUT** (soyadı): "Doe"
* **ÇIKTI**: "JohDoe"

### <a name="remove-diacritics-from-a-string"></a>Aksit leri dizeden kaldırma
Vurgu işaretleri içeren karakterleri, vurgu işaretleri içermeyen eşdeğer karakterlerle değiştirmeniz gerekir.

**Ifa -de:** <br>
NormalizeDiacritics([givenName])

**Örnek giriş/çıktı:** <br>

* **INPUT** (givenName): "Zoë"
* **ÇIKIŞ**: "Zoe"

### <a name="split-a-string-into-a-multi-valued-array"></a>Dizeyi çok değerli bir diziye bölme
Virgülle sınırlı bir dize listesi almanız ve bunları Salesforce'un PermissionSets özniteliği gibi çok değerli bir özniteliğe takılabilir bir diziye bölmeniz gerekir. Bu örnekte, Azure AD'de uzantıAttribute5'te bir izin kümeleri listesi doldurulmuştur.

**Ifa -de:** <br>
Bölme([extensionAttribute5], ",")

**Örnek giriş/çıktı:** <br>

* **INPUT** (extensionAttribute5): "PermissionSetOne, PermisionSetTwo"
* **ÇIKIŞ**: ["PermissionSetOne", "PermissionSetTwo"]

### <a name="output-date-as-a-string-in-a-certain-format"></a>Belirli bir biçimde dize olarak çıkış tarihi
Tarihleri belirli bir biçimde Bir SaaS uygulamasına göndermek istiyorsunuz. <br>
Örneğin, ServiceNow için tarihleri biçimlendirmek istiyorsunuz.

**Ifa -de:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Örnek giriş/çıktı:**

* **INPUT** (extensionAttribute1): "20150123105347.1Z"
* **ÇIKTI**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Önceden tanımlanmış seçenekler kümesine dayalı bir değer değiştirme

Azure AD'de depolanan durum koduna göre kullanıcının saat dilimini tanımlamanız gerekir. <br>
Eyalet kodu önceden tanımlanmış seçeneklerden herhangi biri ile eşleşmiyorsa, "Avustralya/Sydney" varsayılan değerini kullanın.

**Ifa -de:** <br>
`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Örnek giriş/çıktı:**

* **INPUT** (durum): "QLD"
* **ÇIKIŞ**: "Avustralya/Brisbane"

### <a name="replace-characters-using-a-regular-expression"></a>Normal bir ifade kullanarak karakterleri değiştirme
Normal bir ifade değeriyle eşleşen karakterleri bulmanız ve bunları kaldırmanız gerekir.

**Ifa -de:** <br>

Değiştir([mailNickname], " "[a-zA-Z_]*", "", " , )

**Örnek giriş/çıktı:**

* **INPUT** (mailNickname: "john_doe72"
* **ÇıKTı**: "72"

### <a name="convert-generated-userprincipalname-upn-value-to-lower-case"></a>Oluşturulan userPrincipalName (UPN) değerini küçük harfe dönüştürme
Aşağıdaki örnekte, UPN değeri PreferredFirstName ve PreferredLastName kaynak alanları nın biraraya getirilmesiyle oluşturulur ve ToLower işlevi tüm karakterleri küçük harfe dönüştürmek için oluşturulan dize üzerinde çalışır. 

`ToLower(Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"))`

**Örnek giriş/çıktı:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **ÇıKTı**:john.smith@contoso.com" "

### <a name="generate-unique-value-for-userprincipalname-upn-attribute"></a>userPrincipalName (UPN) özniteliği için benzersiz bir değer oluşturma
Kullanıcının adı, göbek adı ve soyadına bağlı olarak, UPN özniteliği için bir değer oluşturmanız ve değeri UPN özniteliğine atamadan önce hedef AD dizinindeki benzersizliğini denetlemeniz gerekir.

**Ifa -de:** <br>

    SelectUniqueValue( 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  [PreferredFirstName], [PreferredLastName]))), "contoso.com"), 
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 1), [PreferredLastName]))), "contoso.com"),
        Join("@", NormalizeDiacritics(StripSpaces(Join(".",  Mid([PreferredFirstName], 1, 2), [PreferredLastName]))), "contoso.com")
    )

**Örnek giriş/çıktı:**

* **INPUT** (PreferredFirstName): "John"
* **INPUT** (PreferredLastName): "Smith"
* **OUTPUT**:John.Smith@contoso.com" " " John.Smith@contoso.com dizinde UPN değeri yoksa
* **OUTPUT**:J.Smith@contoso.com" " " John.Smith@contoso.com dizinde upn değeri zaten varsa
* **OUTPUT**:Jo.Smith@contoso.com" " yukarıdaki iki UPN değeri zaten dizinde varsa


## <a name="next-steps"></a>Sonraki adımlar 

- [Sağlama nedir?](what-is-provisioning.md)
- [Azure AD Connect bulut sağlama nedir?](what-is-cloud-provisioning.md)
