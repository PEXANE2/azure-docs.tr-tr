---
title: Azure AD'de Öznitelik Eşlemeleri için İfadeler Yazma
description: Azure Active Directory'de SaaS uygulama nesnelerinin otomatik olarak sağlanması sırasında öznitelik değerlerini kabul edilebilir bir biçime dönüştürmek için ifade eşlemelerini nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: cc41a18063202bfefb9ddf7238de17fc691984af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77612147"
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Azure Active Directory’de Öznitelik Eşlemeleri için İfadeler Yazma
Bir SaaS uygulamasına sağlama yı yapılandırdığınızda, belirtebileceğiniz öznitelik eşlemetürtürtürtür. Bunlar için, kullanıcılarınızın verilerini SaaS uygulaması için daha kabul edilebilir biçimlere dönüştürmenize olanak tanıyan komut dosyası na benzer bir ifade yazmanız gerekir.

## <a name="syntax-overview"></a>Sözdizimine Genel Bakış
Öznitelik Eşlemeleri için İfadeler sözdizimi, Visual Basic for Applications (VBA) işlevlerini anımsatır.

* İfadenin tamamı, parantez içinde bağımsız değişkenlerin ardından gelen bir addan oluşan işlevler açısından tanımlanmalıdır: <br>
  *Fonksiyon`<<argument 1>>`Adı(`<<argument N>>`, )*
* İşlevleri birbirinuzun içine yerleştirmeniz gerekebilir. Örnek: <br> *FunctionOne(FunctionTwo(`<<argument1>>`))*
* Üç farklı türde bağımsız değişkeni işlevlere geçirebilirsiniz:
  
  1. Öznitelikleri, kare parantez içinde eklenmelidir. Örneğin: [öznitelikName]
  2. Çift tırnak içinde eklenmelidir string sabitleri. Örneğin: "Amerika Birleşik Devletleri"
  3. Diğer Fonksiyonlar. Örneğin: FunctionOne(`<<argument1>>`, FunctionTwo(`<<argument2>>`))
* Dize sabitleri için, dizedeki bir ters eğik çizgi ( \ ) veya tırnak işaretine ( " ) ihtiyacınız varsa, ters eğik çizgi ( \ ) simgesiyle birlikte kaçmış olmalıdır. Örneğin: "Şirket adı: \\"Contoso\\""

## <a name="list-of-functions"></a>İşlevler Listesi
[Ek](#append) &nbsp; &nbsp; &nbsp; &nbsp; [BitAnd](#bitand) &nbsp; &nbsp; [Count](#count) [CBool](#cbool) &nbsp; &nbsp; [CStr](#cstr) [Coalesce](#coalesce) &nbsp; &nbsp; &nbsp; [ConvertToBase64](#converttobase64) &nbsp; [ConvertToUTF8Hex](#converttoutf8hex) &nbsp; CBool &nbsp; &nbsp; Coalesce &nbsp; &nbsp; ConvertToBase64 &nbsp; ConvertToUTF8Hex Count CStr &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [Guid](#guid) &nbsp; [IIF](#iif) &nbsp; [InStr](#instr) &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; [DateFromNum](#datefromnum) &nbsp; &nbsp; &nbsp; [IsNullOrEmpty](#isnullorempty) [IsNull](#isnull) DateFromNum &nbsp; &nbsp; &nbsp; FormatDateTime &nbsp; &nbsp; Guid &nbsp; &nbsp; IIF &nbsp; InStr &nbsp; IsNullIsNullOrEmpty &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [IsPresent](#ispresent) &nbsp; &nbsp; &nbsp; [IsString](#isstring) &nbsp; &nbsp; [Item](#item) &nbsp; [Mid](#mid) [Left](#left) [Join](#join) [Not](#not) [NormalizeDiacritics](#normalizediacritics) IsString &nbsp; &nbsp; Öğesi &nbsp; Join &nbsp; Sol &nbsp; Orta &nbsp; NormalizeDiacritics &nbsp; Değil &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [Replace](#replace) &nbsp; &nbsp; [RemoveDuplicates](#removeduplicates) &nbsp; &nbsp; [SelectUniqueValue](#selectuniquevalue) &nbsp; [Split](#split) RemoveDuplicates &nbsp; &nbsp; [SingleAppRoleAssignment](#singleapproleassignment) SelectUniqueValue&nbsp; SingleAppRoleAssignment&nbsp; Split değiştirin&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [ Stripspaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [ToLower](#tolower) [Switch](#switch) &nbsp; [Word](#word) [ToUpper](#toupper) Tolower&nbsp; Toupper&nbsp; Word'e geçiş&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

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
**Işlev:**<br> BitAnd(value1, value2)

**Açıklama:**<br> Bu işlev her iki parametreyi de ikili gösterime dönüştürür ve aşağıdakilere göre biraz ayarlar:

0 - değer1 ve value2'deki karşılık gelen bitlerden biri veya her ikisi 0 ise                                                  
1 - karşılık gelen bitlerin her ikisi de 1 ise.                                    

Başka bir deyişle, her iki parametrenin karşılık gelen bitleri 1 dışında tüm durumlarda 0 döndürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **değer1** |Gerekli |sayı |Değerle birlikte olması gereken sayısal değer2|
| **değeri2** |Gerekli |sayı |Değerle ve'ed olması gereken sayısal değer1|

**Örnek:**<br>
BitAnd(&HF, &HF7)                                                                                
11110111 VE 00000111 = 00000111 1 11 BitAnd 7 döndürür, 00000111 ikili değeri

---
### <a name="cbool"></a>Cbool
**Işlev:**<br> CBool(İfade)

**Açıklama:**<br> CBool değerlendirilen ifadeye dayalı bir boolean döndürür. İfade sıfır olmayan bir değere göre değerlendirirse, CBool True döndürür, aksi takdirde False döndürür...

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Ifa -de** |Gerekli | ifade | Geçerli ifadeler |

**Örnek:**<br>
CBool([öznitelik1] = [öznitelik2])                                                                    
Her iki öznitelik de aynı değere sahipse True döndürür.

---
### <a name="coalesce"></a>Coalesce
**Işlev:**<br> Coalesce(kaynak1, kaynak2, ..., defaultValue)

**Açıklama:**<br> NULL olmayan ilk kaynak değerini döndürür. Tüm bağımsız değişkenler NULL ise ve varsayılan Değer varsa, varsayılan Değer döndürülür. Tüm bağımsız değişkenler NULL ise ve varsayılanDeğer yoksa, Coalesce NULL döndürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **kaynak1 ... kaynakN** | Gerekli | Dize |Gerekli, değişken sayıda kez. Genellikle kaynak nesneden öznitelik adı. |
| **defaultDeğer** | İsteğe bağlı | Dize | Tüm kaynak değerleri NULL olduğunda kullanılacak varsayılan değer. Boş dize ("") olabilir.

---
### <a name="converttobase64"></a>ConverttoBase64
**Işlev:**<br> ConvertToBase64(kaynak)

**Açıklama:**<br> ConvertToBase64 işlevi bir dizeyi Unicode base64 dizesine dönüştürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |Taban 64'e dönüştürülecek dize|

**Örnek:**<br>
ConvertToBase64("Merhaba dünya!")                                                                                                        
"SABlAGwAbABvACAAdwBvAHIAbABkACEA"

---
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Işlev:**<br> ConvertToUTF8Hex(kaynak)

**Açıklama:**<br> ConvertToUTF8Hex işlevi bir dizeyi UTF8 Hex kodlanmış değere dönüştürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |UTF8 Hex'e dönüştürülecek dize|

**Örnek:**<br>
ConvertToUTF8Hex("Merhaba dünya!")                                                                                                         
İade 48656C6C6F20776F726C6421

---
### <a name="count"></a>Sayı
**Işlev:**<br> Sayım(öznitelik)

**Açıklama:**<br> Count işlevi çok değerli bir öznitelikteki eleman sayısını döndürür

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Öznitelik** |Gerekli |özniteliği |Öğelerin sayılmasını sağlayacak çok değerli öznitelik|

---
### <a name="cstr"></a>Cstr
**Işlev:**<br> CStr(değer)

**Açıklama:**<br> CStr işlevi bir değeri dize veri türüne dönüştürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | sayısal, referans veya boolean | Sayısal bir değer, başvuru özniteliği veya Boolean olabilir. |

**Örnek:**<br>
CStr([dn])                                                            
"cn=Joe,dc=contoso,dc=com" döndürür

---
### <a name="datefromnum"></a>TarihFromNum
**Işlev:**<br> DateFromNum(değer)

**Açıklama:**<br> DateFromNum işlevi, AD'nin tarih biçimindeki bir değeri DateTime türüne dönüştürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | Tarih | DateTime türüne dönüştürülecek AD Tarihi |

**Örnek:**<br>
TarihFromNum([lastLogonTimestamp])                                                                                                   
TarihFromNum(1296993240000000000)                                                            
2012-01-01 23:00:00'ı temsil eden bir DateTime döndürür

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
**Işlev:**<br> Kılavuz()

**Açıklama:**<br> Guid işlevi yeni bir rasgele GUID oluşturur

---
### <a name="iif"></a>ııf
**Işlev:**<br> IIF(durum,valueIfTrue,valueIfFalse)

**Açıklama:**<br> IIF işlevi, belirtilen durumu temel alan olası değerler kümesinden birini döndürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Durum** |Gerekli |Değişken veya İfade |Doğru veya yanlış olarak değerlendirilebilecek herhangi bir değer veya ifade. |
| **valueIfTrue** |Gerekli |Değişken veya String | Koşul doğru değerlendirirse, döndürülen değer. |
| **valueIfFalse** |Gerekli |Değişken veya String |Koşul false olarak değerlendirilirse, döndürülen değer.|

**Örnek:**<br>
IIF([ülke]="ABD",[ülke],[departman])

---
### <a name="instr"></a>ınstr
**Işlev:**<br> InStr(value1,value2,start,compareType)

**Açıklama:**<br> InStr işlevi bir dize bir alt dize ilk oluşumunu bulur

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **değer1** |Gerekli |Dize |Aranacak dize |
| **değeri2** |Gerekli |Dize |Bulunacak dize |
| **Başlatmak** |İsteğe bağlı |Tamsayı |Substring bulmak için başlangıç pozisyonu|
| **compareType** |İsteğe bağlı |Sabit Listesi |vbTextCompare veya vbBinaryKarşılaştır olabilir |

**Örnek:**<br>
InStr("Hızlı kahverengi tilki"," hızlı")                                                                             
Evalues için 5

InStr("repEated","e",3,vbBinaryKarşılaştır)                                                                                  
7'ye göre değerlendirir

---
### <a name="isnull"></a>ısnull
**Işlev:**<br> Isnull(İfade)

**Açıklama:**<br> İfade Null'a göre değerlendirilirse, IsNull işlevi doğru döndürür. Bir öznitelik için, bir Null öznitelik yokluğu ile ifade edilir.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Ifa -de** |Gerekli |ifade |Değerlendirilecek ifade |

**Örnek:**<br>
IsNull([displayName])                                                                                                
Öznitelik yoksa True döndürür

---
### <a name="isnullorempty"></a>ısnullorempty
**Işlev:**<br> Isnullorempty(ifade)

**Açıklama:**<br> İfade null veya boş bir dize ise, O zaman IsNullOrEmpty işlevi doğru döndürür. Bir öznitelik için, öznitelik yoksa veya yoksa ancak boş bir dize yse, bu True'ya değerlenir.
Bu işlevin tersi IsPresent olarak adlandırılır.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Ifa -de** |Gerekli |ifade |Değerlendirilecek ifade |

**Örnek:**<br>
IsNullOrEmpty([displayName])                                               
Öznitelik yoksa veya boş bir dizeyse True döndürür

---
### <a name="ispresent"></a>ıspresent
**Işlev:**<br> IsPresent(İfade)

**Açıklama:**<br> İfade Null olmayan ve boş olmayan bir dize değerlendirirse, IsPresent işlevi doğru döndürür. Bu işlevin tersi IsNullOrEmpty olarak adlandırılır.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Ifa -de** |Gerekli |ifade |Değerlendirilecek ifade |

**Örnek:**<br>
Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])

---
### <a name="isstring"></a>IsString
**Işlev:**<br> IsString(İfade)

**Açıklama:**<br> İfade bir dize türüne değerlendirilebilirse, IsString işlevi True olarak değerlendirilir.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Ifa -de** |Gerekli |ifade |Değerlendirilecek ifade |

---
### <a name="item"></a>Öğe
**Işlev:**<br> Öğe(öznitelik, dizin)

**Açıklama:**<br> Öğe işlevi, çok değerli bir dize/öznitelikten bir öğeyi döndürür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Öznitelik** |Gerekli |Öznitelik |Aranacak çok değerli öznitelik |
| **Dizin** |Gerekli |Tamsayı | Çok değerli dizedeki bir öğeye dizin|

**Örnek:**<br>
Öğe([proxyAdresleri], 1)

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
**Işlev:**<br> Sol (String,NumChars)

**Açıklama:**<br> Sol işlev, dizesin solundan belirli sayıda karakter döndürür. numChars = 0 ise, boş dize döndürün.
numChars 0'ı <, giriş dizelerini döndürün.
Dize null ise, boş dize döndürün.
Dize numChars belirtilen sayıdan daha az karakter içeriyorsa, dize ile aynı bir dize (yani parametre 1'deki tüm karakterleri içeren) döndürülür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Dize** |Gerekli |Öznitelik | Karakterleri döndürecek dize |
| **NumChars** |Gerekli |Tamsayı | Dize başından (solda) döndürülecek karakter sayısını tanımlayan bir sayı|

**Örnek:**<br>
Sol("John Doe", 3)                                                            
"Joh" döndürür

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
### <a name="numfromdate"></a>NumFromDate
**Işlev:**<br> NumFromDate(değer)

**Açıklama:**<br> NumFromDate işlevi, DateTime değerini [accountExpires](https://docs.microsoft.com/windows/win32/adschema/a-accountexpires)gibi öznitelikleri ayarlamak için gereken Active Directory biçimine dönüştürür. İş Günü ve Başarı Faktörleri gibi bulut İk uygulamalarından alınan DateTime değerlerini eşdeğer AD gösterimlerine dönüştürmek için bu işlevi kullanın. 

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **value** |Gerekli | Dize | Desteklenen biçimde tarih saati dizesi. Desteklenen biçimler için https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspxbkz. |

**Örnek:**<br>
* İş günü örneği <br>
  *2020-12-31-08:00* biçiminde ki İş Günü'nden AD'deki *accountExpires* alanına bağlı *ContractEndDate* özniteliğini eşlendirmek istediğinizi varsayarsak, bu işlevi nasıl kullanabilirsiniz ve saat dilimi mahsulünü bulunduğunuz yerle eşleşecek şekilde değiştirebilirsiniz. 
  `NumFromDate(Join("", FormatDateTime([ContractEndDate], "yyyy-MM-ddzzz", "yyyy-MM-dd"), "T23:59:59-08:00"))`

* SuccessFactors örnek <br>
  Ad'da *accountExpires* alanına *M/d/yyyy hh:mm:ss tt* biçiminde olan SuccessFactors'ten öznitelik *son Tarihini* haritalamak istediğinizi varsayarsak, bu işlevi nasıl kullanabilirsiniz ve saat dilimi mahsulü nüanslarını bulunduğunuz yerle eşleşecek şekilde değiştirebilirsiniz.
  `NumFromDate(Join("",FormatDateTime([endDate],"M/d/yyyy hh:mm:ss tt","yyyy-MM-dd"),"T23:59:59-08:00"))`


---
### <a name="removeduplicates"></a>Removeduplicates
**Işlev:**<br> RemoveDuplicates(öznitelik)

**Açıklama:**<br> RemoveDuplicates işlevi çok değerli bir dize alır ve her değerin benzersiz olduğundan emin olun.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Öznitelik** |Gerekli |Çok değerli Öznitelik |Yinelenenleri kaldıracak çok değerli öznitelik|

**Örnek:**<br>
RemoveDuplicates([proxyAdresleri])                                                                                                       
Tüm yinelenen değerlerin kaldırıldığı bir sanitized proxyAddress özniteliği döndürür

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
| --- | --- | --- | --- |
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

**Açıklama:**<br> **Kaynak** değeri bir **anahtarla**eşleştiğinde, bu **anahtar**için **değer** döndürür. **Kaynak** değeri herhangi bir anahtarla eşleşmiyorsa, **varsayılan Değer**döndürür.  **Anahtar** ve **değer** parametreleri her zaman çiftler halinde gelmelidir. İşlev her zaman eşit sayıda parametre bekler. İşlev, yönetici gibi başvurusal öznitelikler için kullanılmamalıdır. 

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Kaynak** |Gerekli |Dize |**Güncelleştirilen kaynak** değeri. |
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
### <a name="word"></a>Word
**Işlev:**<br> Word(String,WordNumber,Delimiters)

**Açıklama:**<br> Word işlevi, kullanılacak sınırlayıcıları ve döndürülecek sözcük sayısını açıklayan parametrelere dayalı olarak dize içinde bulunan bir sözcüğü döndürür. Delimiters karakterlerden biri tarafından ayrılmış dize karakter her dize sözcükler olarak tanımlanır:

1 < sayısı varsa, boş dize döndürür.
Dize null ise, boş dize döndürür.
Dize sayı sözcükten daha az sözcük içeriyorsa veya dize sınırlayıcılar tarafından tanımlanan sözcükleri içermiyorsa, boş bir dize döndürülür.

**Parametre:**<br> 

| Adı | Gerekli/ Yineleme | Tür | Notlar |
| --- | --- | --- | --- |
| **Dize** |Gerekli |Çok değerli Öznitelik |Bir kelimeyi döndürmek için dize.|
| **Kelime Sayısı** |Gerekli | Tamsayı | Hangi sözcük numarasının döndürülmesi gerektiğini belirleme sayısı|
| **Sınırlayıcı** |Gerekli |Dize| Sözcükleri tanımlamak için kullanılması gereken delimiter(ler)i temsil eden bir dize|

**Örnek:**<br>
Word("Hızlı kahverengi tilki",3," ")                                                                                       
"Kahverengi" döndürür

Word("Bu, string!&çok ayırıcı",3,",,&#")                                                                       
"has" döndürür

---

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

### <a name="flow-mail-value-if-not-null-otherwise-flow-userprincipalname"></a>NULL değilse akış posta değeri, aksi takdirde akış userPrincipalName
Varsa posta özniteliğini akışyapmak istersiniz. Değilse, bunun yerine userPrincipalName değerini akış istiyorum.

**Ifa -de:** <br>
`Coalesce([mail],[userPrincipalName])`

**Örnek giriş/çıktı:** <br>

* **GIRIŞ** (posta): NULL
* **INPUT** (userPrincipalName):John.Doe@contoso.com" "
* **ÇıKTı**:John.Doe@contoso.com" "

## <a name="related-articles"></a>İlgili Makaleler
* [SaaS Uygulamalarına Kullanıcı Sağlama/Sağlama sağlama](../app-provisioning/user-provisioning.md)
* [Kullanıcı Sağlama için Öznitelik Eşlemeleri Özelleştirme](../app-provisioning/customize-application-attributes.md)
* [Kullanıcı Sağlama için Kapsam Filtreleri](define-conditional-rules-for-provisioning-user-accounts.md)
* [Kullanıcıların ve grupların Azure Active Directory'den uygulamalara otomatik olarak hazırlanmasını etkinleştirmek için SCIM'yi kullanma](../app-provisioning/use-scim-to-provision-users-and-groups.md)
* [Hesap Sağlama Bildirimleri](../app-provisioning/user-provisioning.md)
* [SaaS Uygulamalarını Tümleştirme Hakkında Öğreticiler Listesi](../saas-apps/tutorial-list.md)
