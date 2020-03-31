---
title: 'Azure AD Connect eşitleme: İşlevler Başvuru | Microsoft Dokümanlar'
description: Azure AD Connect eşitlemesinde bildirimsel sağlama ifadelerinin başvurusu.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5c3102480e316c634930c356ae02f769767b7d08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69900039"
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect eşitlemi: İşlevler Başvurusu
Azure AD Connect'te işlevler eşitleme sırasında bir öznitelik değerini işlemek için kullanılır.  
Fonksiyonların Sözdizimi aşağıdaki biçim kullanılarak ifade edilir:  
`<output type> FunctionName(<input type> <position name>, ..)`

Bir işlev aşırı yüklenir sa ve birden çok sözdizimi kabul ederse, geçerli tüm sözdizimiler listelenir.  
İşlevler güçlü bir şekilde dizilir ve geçirilen türün belgelenen türle eşleştiğini doğrular.  
Tür eşleşmiyorsa, bir hata atılır.

Türleri aşağıdaki sözdizimi ile ifade edilir:

* **bin** – İkili
* **bool** – Boolean
* **dt** – UTC Tarih/Saat
* **enum** – Bilinen sabitlerin numaralandırması
* **exp** – İfade, bir Boolean değerlendirmek için bekleniyor
* **mvbin** – Çok Değerli İkili
* **mvstr** – Çok Değerli Dize
* **mvref** – Çok Değerli Referans
* **num** – Sayısal
* **ref** – Referans
* **str** – Dize
* **var** – (neredeyse) başka bir tür bir varyantı
* **void** – bir değer döndürmez

**Mvbin,** **mvstr**ve **mvref** tipleri ile fonksiyonlar sadece çok değerli öznitelikleri üzerinde çalışabilir. Hem tek değerli hem de çok değerli özniteliklerüzerinde **bin**, **str**ve **ref** çalışması olan işlevler.

## <a name="functions-reference"></a>İşlevler Başvurusu

| Fonksiyonlar listesi |  |  |  |  |
| --- | --- | --- | --- | --- |
| **Sertifika** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[CertThumbprint](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Dönüştürme** | | | | |
| [Cbool](#cbool) |[Cdate](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConverttoBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [Cref](#cref) |[Cstr](#cstr) |[StringFromGuid](#stringfromguid) |[StringFromSid](#stringfromsid) | |
| **Tarih / Saat** | | | | |
| [Dateadd](#dateadd) |[TarihFromNum](#datefromnum) |[BiçimDateTime](#formatdatetime) |[Nwo](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Dizin** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Değerlendirme** | | | | |
| [IsBitSet](#isbitset) |[ısdate](#isdate) |[ısempty](#isempty) |[IsGuid](#isguid) | |
| [ısnull](#isnull) |[ısnullorempty](#isnullorempty) |[IsNumeric](#isnumeric) |[ıspresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Matematik** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Değerli** | | | | |
| [Contains](#contains) |[Sayısı](#count) |[Öğe](#item) |[ItemOrnull](#itemornull) | |
| [Katıl](#join) |[Removeduplicates](#removeduplicates) |[Bölme](#split) | | |
| **Program Akışı** | | | | |
| [Hata](#error) |[ııf](#iif) |[Seç](#select) |[Anahtarı](#switch) | |
| [Nerede](#where) |[Ile](#with) | | | |
| **Metin** | | | | |
| [Guıd](#guid) |[ınstr](#instr) |[Instrrev](#instrrev) |[Lcase](#lcase) | |
| [Sol](#left) |[Len](#len) |[Ltrim](#ltrim) |[Orta](#mid) | |
| [Padleft](#padleft) |[Padright](#padright) |[PCase](#pcase) |[Değiştirmek](#replace) | |
| [Yerine Chars](#replacechars) |[Doğru](#right) |[Rtrım](#rtrim) |[Döşeme](#trim) | |
| [Ucase](#ucase) |[Word](#word) | | | |

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
`BitAnd(&HF, &HF7)`  
Hexadecimal "F" VE "F7" bu değere değer biçilebildiği için 7 döndürür.

---
### <a name="bitor"></a>BitOr
**Açıklama:**  
BitOr işlevi bir değer üzerinde belirtilen bitleri ayarlar.

**Sözdizimi:**  
`num BitOr(num value1, num value2)`

* value1, value2: birlikte veya'ed olması gereken sayısal değerler

**Açıklamalar:**  
Bu işlev, her iki parametreyi de ikili gösterime dönüştürür ve maske ve bayraktaki karşılık gelen bitlerden biri veya her ikisi 1 ise biraz 1 ve karşılık gelen bitlerin her ikisi de 0 ise 0'a ayarlar. Başka bir deyişle, her iki parametrenin karşılık gelen bitlerinin 0 olduğu durumlar dışında her durumda 1 döndürür.

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
### <a name="cdate"></a>Cdate
**Açıklama:**  
CDate işlevi bir dizeden bir UTC DateTime döndürür. DateTime Eşitleme'de yerel bir öznitelik türü değildir, ancak bazı işlevler tarafından kullanılır.

**Sözdizimi:**  
`dt CDate(str value)`

* Değer: Tarih, saat ve isteğe bağlı saat dilimi içeren bir dize

**Açıklamalar:**  
Döndürülen dize her zaman UTC'dedir.

**Örnek:**  
`CDate([employeeStartTime])`  
Çalışanın başlangıç saatini temel alan bir DateTime döndürür

`CDate("2013-01-10 4:00 PM -8")`  
2013-01-11 12:00 temsil eden bir DateTime döndürür


---
### <a name="certextensionoids"></a>CertExtensionOids
**Açıklama:**  
Sertifika nesnesinin tüm kritik uzantılarının Oid değerlerini döndürür.

**Sözdizimi:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certformat"></a>CertFormat
**Açıklama:**  
Bu X.509v3 sertifikasının biçimini verir.

**Sözdizimi:**  
`str CertFormat(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certfriendlyname"></a>CertFriendlyName
**Açıklama:**  
Sertifika için ilişkili diğer adı döndürür.

**Sözdizimi:**  
`str CertFriendlyName(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certhashstring"></a>CertHashString
**Açıklama:**  
X.509v3 sertifikası için SHA1 karma değerini hexadecimal dize olarak döndürür.

**Sözdizimi:**  
`str CertHashString(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certissuer"></a>CertIssuer
**Açıklama:**  
X.509v3 sertifikasını veren sertifika yetkilisinin adını verir.

**Sözdizimi:**  
`str CertIssuer(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certissuerdn"></a>CertIssuerDN
**Açıklama:**  
Sertifikayı verenin ayırt edici adını verir.

**Sözdizimi:**  
`str CertIssuerDN(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certissueroid"></a>CertIssuerOid
**Açıklama:**  
Sertifikayı verenin Oid'ini verir.

**Sözdizimi:**  
`str CertIssuerOid(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Açıklama:**  
Bu X.509v3 sertifikasının anahtar algoritma bilgilerini dize olarak verir.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Açıklama:**  
X.509v3 sertifikası nın anahtar algoritma parametrelerini hexadecimal dize olarak döndürür.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certnameinfo"></a>CertNameInfo
**Açıklama:**  
Sertifikadan özne ve veren adlarını verir.

**Sözdizimi:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.
*   X509NameType: Konu için X509NameType değeri.
*   includeIssuerName: veren in adını eklemek için doğru; aksi takdirde, yanlış.

---
### <a name="certnotafter"></a>CertNotAfter
**Açıklama:**  
Sertifikanın artık geçerli olmadığı yerel saatteki tarihi döndürür.

**Sözdizimi:**  
`dt CertNotAfter(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certnotbefore"></a>CertNotBefore
**Açıklama:**  
Sertifikanın geçerli olduğu yerel saatteki tarihi döndürür.

**Sözdizimi:**  
`dt CertNotBefore(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Açıklama:**  
X.509v3 sertifikası için ortak anahtarın Oid'ini verir.

**Sözdizimi:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Açıklama:**  
X.509v3 sertifikası için ortak anahtar parametrelerinin Oid'ini verir.

**Sözdizimi:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certserialnumber"></a>CertSerialNumber
**Açıklama:**  
X.509v3 sertifikasının seri numarasını verir.

**Sözdizimi:**  
`str CertSerialNumber(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Açıklama:**  
Sertifika nın imzasını oluşturmak için kullanılan algoritmanın Oid'ini döndürür.

**Sözdizimi:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certsubject"></a>CertSubject
**Açıklama:**  
Özne ayırt edilen adı sertifikadan alır.

**Sözdizimi:**  
`str CertSubject(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Açıklama:**  
Bir sertifikadan özne ayırt edici adı döndürür.

**Sözdizimi:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Açıklama:**  
Bir sertifikadan özne adının Oid'ini verir.

**Sözdizimi:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certthumbprint"></a>CertThumbprint
**Açıklama:**  
Sertifikanın parmak izini verir.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="certversion"></a>CertVersion
**Açıklama:**  
Sertifikanın X.509 biçimli sürümünü döndürür.

**Sözdizimi:**  
`str CertThumbprint(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.

---
### <a name="cguid"></a>CGuid
**Açıklama:**  
CGuid işlevi, GUID'in dize temsilini ikili gösterimine dönüştürür.

**Sözdizimi:**  
`bin CGuid(str GUID)`

* Bu desende biçimlendirilmiş bir String: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

---
### <a name="contains"></a>Contains
**Açıklama:**  
İçerme işlevi çok değerli bir öznitelik içinde bir dize bulur

**Sözdizimi:**  
`num Contains (mvstring attribute, str search)`- Büyük/küçük harf duyarlı  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`- Büyük/küçük harf duyarlı

* öznitelik: aramak için çok değerli öznitelik.
* arama: öznitelik bulmak için dize.
* Casetype: CaseInsensitive veya CaseSensitive.

Dize nin bulunduğu çok değerli öznitelikteki dizini döndürür. Dize bulunmazsa 0 döndürülür.

**Açıklamalar:**  
Çok değerli dize öznitelikleri için, arama değerleri alt dizeleri bulur.  
Başvuru öznitelikleri için, aranan dize tam olarak bir eşleşme olarak kabul edilecek değeri eşleşmesi gerekir.

**Örnek:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
ProxyAddresss özniteliği birincil e-posta adresine sahipse (büyük harf "SMTP:" ile gösterilir), sonra proxyAddress özniteliğini döndürün, aksi takdirde bir hata döndürün.

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
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Açıklama:**  
ConvertFromUTF8Hex işlevi belirtilen UTF8 Hex kodlanmış değeri bir dize dönüştürür.

**Sözdizimi:**  
`str ConvertFromUTF8Hex(str source)`

* kaynak: UTF8 2 bayt kodlanmış sokma

**Açıklamalar:**  
Bu işlev ve ConvertFromBase64([],UTF8) arasındaki fark, sonuç DN özniteliği için dostudur.  
Bu biçim Azure Active Directory tarafından DN olarak kullanılır.

**Örnek:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
İade "*Merhaba dünya!*"

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
### <a name="cnum"></a>CNum
**Açıklama:**  
CNum işlevi bir dize alır ve sayısal bir veri türü döndürür.

**Sözdizimi:**  
`num CNum(str value)`

---
### <a name="cref"></a>Cref
**Açıklama:**  
Dizeyi bir başvuru özniteliğine dönüştürür

**Sözdizimi:**  
`ref CRef(str value)`

**Örnek:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

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
### <a name="dateadd"></a>DateAdd
**Açıklama:**  
Belirli bir zaman aralığının eklendiği tarihi içeren bir Tarih döndürür.

**Sözdizimi:**  
`dt DateAdd(str interval, num value, dt date)`

* aralık: Eklemek istediğiniz zaman aralığı olan string ifadesidir. Dize aşağıdaki değerlerden birine sahip olmalıdır:
  * yyyy Yıl
  * q Üç Aylık
  * m Ay
  * y Yılın Günü
  * d Gün
  * w Hafta içi
  * ww Hafta
  * h Saat
  * n Dakika
  * s İkinci
* değer: Eklemek istediğiniz birim sayısı. Bu olumlu (gelecekte tarih almak için) veya negatif (geçmişte tarih almak için) olabilir.
* tarih: Aralığın eklendiği tarihi temsil eden DateTime.

**Örnek:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
3 ay ekler ve "2001-04-01"i temsil eden bir DateTime döndürür.

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
### <a name="dncomponentrev"></a>DNComponentRev
**Açıklama:**  
DNComponentRev işlevi sağa (uçtan) giden belirli bir DN bileşeninin değerini döndürür.

**Sözdizimi:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* dn: yorumlamak için referans özniteliği
* ComponentNumber - DN'deki bileşenin döndürülecek
* Seçenekler: DC – "dc=" ile tüm bileşenleri yoksay

**Örnek:**  
Eğer dn "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" ise,  
`DNComponentRev(CRef([dn]),3)`  
`DNComponentRev(CRef([dn]),1,"DC")`  
Her ikisi de ABD dönmek.

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
### <a name="escapedncomponent"></a>EscapeDNComponent
**Açıklama:**  
EscapeDNComponent işlevi bir DN'nin bir bileşenini alır ve LDAP'de temsil edilebilsin diye ondan kaçar.

**Sözdizimi:**  
`str EscapeDNComponent(str value)`

**Örnek:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
DisplayName özniteliği LDAP'de kaçması gereken karakterlere sahip olsa bile nesnenin ldap dizininde oluşturulabilmesini sağlar.

---
### <a name="formatdatetime"></a>BiçimDateTime
**Açıklama:**  
FormatDateTime işlevi, DateTime'ı belirli bir biçime sahip bir dize biçimlendirmek için kullanılır

**Sözdizimi:**  
`str FormatDateTime(dt value, str format)`

* değer: DateTime biçiminde bir değer
* biçim: dönüştürülecek biçimi temsil eden bir dize.

**Açıklamalar:**  
Biçim için olası değerleri burada bulabilirsiniz: [FORMAT işlevi için özel tarih ve saat biçimleri.](https://docs.microsoft.com/dax/custom-date-and-time-formats-for-the-format-function)

**Örnek:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Sonuçlar "2007-12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"20140905081453.0Z" ile sonuçlanabilir

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
### <a name="instrrev"></a>Instrrev
**Açıklama:**  
InStrRev işlevi bir dize bir substring son oluşumunu bulur

**Sözdizimi:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: aranacak dize
* stringmatch: string bulunacak
* start: substring bulmak için başlangıç pozisyonu
* karşılaştırın: vbTextCompare veya vbBinaryKarşılaştır

**Açıklamalar:**  
Substring'in bulunduğu konumu veya yoksa 0'ı döndürür.

**Örnek:**  
`InStrRev("abbcdbbbef","bb")`  
İade 7

---
### <a name="isbitset"></a>IsBitSet
**Açıklama:**  
Bir bit ayarlanmış saveya değil mi, IsBitSet işlevi Testleri

**Sözdizimi:**  
`bool IsBitSet(num value, num flag)`

* değer: değerlendirilen sayısal bir değer.flag: değerlendirilmesi gereken bit olan sayısal değer

**Örnek:**  
`IsBitSet(&HF,4)`  
Bit "4" hexadecimal değer "F" olarak ayarlandığı için True döndürür

---
### <a name="isdate"></a>ısdate
**Açıklama:**  
İfade DateTime türü olarak değerlendirilebilirse, IsDate işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsDate(var Expression)`

**Açıklamalar:**  
CDate() başarılı olup olmadığını belirlemek için kullanılır.

---
### <a name="iscert"></a>IsCert
**Açıklama:**  
Ham veriler .NET X509Certificate2 sertifika nesnesine serihale edilebiliyorsa doğru döndürür.

**Sözdizimi:**  
`bool CertThumbprint(binary certificateRawData)`  
*   sertifikaRawData: X.509 sertifikasının bayt dizi gösterimi. Bayt dizisi ikili (DER) kodlanmış veya Base64 kodlanmış X.509 verisi olabilir.
---
### <a name="isempty"></a>IsEmpty
**Açıklama:**  
Öznitelik CS veya MV'de bulunursa ancak boş bir dize değerlendirirse, Boş boş işlevi True'yu değerlendirir.

**Sözdizimi:**  
`bool IsEmpty(var Expression)`

---
### <a name="isguid"></a>IsGuid
**Açıklama:**  
Dize bir GUID dönüştürülebilirse, O zaman IsGuid işlevi doğru olarak değerlendirilir.

**Sözdizimi:**  
`bool IsGuid(str GUID)`

**Açıklamalar:**  
Bu desenlerden birini izleyen bir kılavuz dize olarak tanımlanır: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx veya {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx}

CGuid() başarılı olabilir belirlemek için kullanılır.

**Örnek:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
StrAttribute bir GUID biçimi varsa, ikili gösterim döndürün, aksi takdirde bir Null döndürün.

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
### <a name="isnumeric"></a>IsNumeric
**Açıklama:**  
Umumeric işlevi, bir ifadenin bir sayı türü olarak değerlendirilip değerlendirilemeyeceğini belirten bir Boolean değeri döndürür.

**Sözdizimi:**  
`bool IsNumeric(var Expression)`

**Açıklamalar:**  
CNum() ifadesini ayrıştırmak için başarılı olup olmadığını belirlemek için kullanılır.

---
### <a name="isstring"></a>IsString
**Açıklama:**  
İfade bir dize türüne değerlendirilebilirse, IsString işlevi True olarak değerlendirilir.

**Sözdizimi:**  
`bool IsString(var expression)`

**Açıklamalar:**  
CStr() ifadesini ayrıştırmak için başarılı olup olmadığını belirlemek için kullanılır.

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
### <a name="itemornull"></a>ItemOrnull
**Açıklama:**  
ItemOrNull işlevi, çok değerli bir dize/öznitelikten bir öğeyi döndürür.

**Sözdizimi:**  
`var ItemOrNull(mvstr attribute, num index)`

* öznitelik: çok değerli öznitelik
* dizin: çok değerli dize bir öğeye dizin.

**Açıklamalar:**  
İkinci işlev dizini çok değerli öznitelikteki bir öğeye döndürdüğünden ItemOrNull işlevi İçer işleviile birlikte kullanışlıdır.

Dizin sınırların dışındaysa, null değeri döndürür.

---
### <a name="join"></a>Birleştir
**Açıklama:**  
Birleştirme işlevi çok değerli bir dize alır ve her öğe arasına belirtilen ayırıcı eklenmiş tek değerli bir dize döndürür.

**Sözdizimi:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* öznitelik: Birlilecek dizeleri içeren çok değerli öznitelik.
* delimiter: Döndürülen dizedeki alt dizeleri ayırmak için kullanılan herhangi bir dize. Atlanırsa, boşluk karakteri (" ") kullanılır. Delimiter sıfır uzunlukta bir dize ("") veya Hiçbir şey ise, listedeki tüm öğeler sınır dışı layıcılar ile birleştirilmiştir.

**Açıklamalar**  
Birleştirme ve Bölme işlevleri arasında eşitlik vardır. Birleştirme işlevi bir dizi dize alır ve tek bir dize döndürmek için bir sınırlayıcı dize kullanarak bunları birleştirir. Bölme işlevi bir dize alır ve bir dizi dize döndürmek için sınırlayıcıda ayırır. Ancak, önemli bir fark, Join herhangi bir sınırlayıcı dize ile dizeleri birleştirebilirsiniz, Split yalnızca tek bir karakter delimiter kullanarak dizeleri ayırabilirsiniz.

**Örnek:**  
`Join([proxyAddresses],",")`  
Geri dönebilir:SMTP:john.doe@contoso.comsmtp:jd@contoso.com" , "

---
### <a name="lcase"></a>Lcase
**Açıklama:**  
LCase işlevi, dizedeki tüm karakterleri küçük harfe dönüştürür.

**Sözdizimi:**  
`str LCase(str value)`

**Örnek:**  
`LCase("TeSt")`  
"Test"i döndürür.

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
"Joh"u döndürür.

---
### <a name="len"></a>Len
**Açıklama:**  
Len işlevi bir dizedeki karakter sayısını döndürür.

**Sözdizimi:**  
`num Len(str value)`

**Örnek:**  
`Len("John Doe")`  
İade 8

---
### <a name="ltrim"></a>Ltrim
**Açıklama:**  
LTrim işlevi, önde gelen beyaz boşlukları bir dizeden kaldırır.

**Sözdizimi:**  
`str LTrim(str value)`

**Örnek:**  
`LTrim(" Test ")`  
"Test" döndürür

---
### <a name="mid"></a>Orta
**Açıklama:**  
Orta işlev, dizedeki belirli bir konumdan belirli sayıda karakter döndürür.

**Sözdizimi:**  
`str Mid(str string, num start, num NumChars)`

* string: karakterleri döndürmek için dize
* başlangıç: karakterleri döndürmek için dizedeki başlangıç konumunu tanımlayan bir sayı
* NumChars: dizedeki konumdan dönecek karakter sayısını tanımlayan bir sayı

**Açıklamalar:**  
NumChars karakterlerini dizedeki pozisyondan başlayarak döndürün.  
NumChars karakterlerini içeren bir dize dizedeki konum başlangıcından başlar:

* numChars = 0 ise, boş dize döndürün.
* numChars 0'ı <, giriş dizelerini döndürün.
* Dize uzunluğu > başlarsa, giriş dizesini döndürün.
* =0 <başlarsa, giriş dizelerini döndürün.
* Dize null ise, boş dize döndürün.

Pozisyon başlangıcından itibaren dizede kalan numChar karakterleri yoksa, mümkün olduğunca çok karakter döndürülür.

**Örnek:**  
`Mid("John Doe", 3, 5)`  
"hn Do"u döndürür.

`Mid("John Doe", 6, 999)`  
"Doe" döndürür

---
### <a name="now"></a>Now
**Açıklama:**  
Şimdi işlevi, bilgisayarınızın sistem tarihi ne de saatine göre geçerli tarih ve saati belirten bir DateTime döndürür.

**Sözdizimi:**  
`dt Now()`

---
### <a name="numfromdate"></a>NumFromDate
**Açıklama:**  
NumFromDate işlevi AD'nin tarih biçiminde bir tarih döndürür.

**Sözdizimi:**  
`num NumFromDate(dt value)`

**Örnek:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
İade 12969932400000000

---
### <a name="padleft"></a>Padleft
**Açıklama:**  
PadLeft işlevi, sağlanan dolgu karakterini kullanarak bir dizeyi belirli bir uzunluğa kadar sol ayarı salar.

**Sözdizimi:**  
`str PadLeft(str string, num length, str padCharacter)`

* string: pad dize.
* uzunluk: İstenilen dize uzunluğunu temsil eden bir toplam.
* padKarakter: Pad karakteri olarak kullanılacak tek bir karakterden oluşan dize

**Açıklamalar:**

* Dize uzunluğu uzunluktan küçükse, padCharacter uzunluğa eşit bir uzunluğa sahip olana kadar dize başına (solda) tekrar tekrar eklenir.
* PadCharacter bir boşluk karakteri olabilir, ancak null bir değer olamaz.
* Dize uzunluğu uzunluğa eşit veya daha büyükse, dize değişmeden döndürülür.
* Dize uzunlamı uzunlamasına veya uzunluğa eşit olması durumunda, dizeyle aynı bir dize döndürülür.
* Dize uzunluğu uzunluktan daha azsa, istenen uzunlukta yeni bir dize padCharacter ile yastıklı dize içeren döndürülür.
* Dize null ise, işlev boş bir dize döndürür.

**Örnek:**  
`PadLeft("User", 10, "0")`  
"000000User" döndürür.

---
### <a name="padright"></a>Padright
**Açıklama:**  
PadRight işlevi, sağlanan dolgu karakterini kullanarak bir dizeyi belirli bir uzunluğa kadar doğru dizeler.

**Sözdizimi:**  
`str PadRight(str string, num length, str padCharacter)`

* string: pad dize.
* uzunluk: İstenilen dize uzunluğunu temsil eden bir toplam.
* padKarakter: Pad karakteri olarak kullanılacak tek bir karakterden oluşan dize

**Açıklamalar:**

* Dize uzunluğu uzunluktan küçükse, padCharacter uzunluğa eşit bir uzunluğa sahip olana kadar dizesonuna (sağa) tekrar tekrar eklenir.
* padCharacter bir boşluk karakteri olabilir, ancak null bir değer olamaz.
* Dize uzunluğu uzunluğa eşit veya daha büyükse, dize değişmeden döndürülür.
* Dize uzunlamı uzunlamasına veya uzunluğa eşit olması durumunda, dizeyle aynı bir dize döndürülür.
* Dize uzunluğu uzunluktan daha azsa, istenen uzunlukta yeni bir dize padCharacter ile yastıklı dize içeren döndürülür.
* Dize null ise, işlev boş bir dize döndürür.

**Örnek:**  
`PadRight("User", 10, "0")`  
"User000000" yazar.

---
### <a name="pcase"></a>PCase
**Açıklama:**  
PCase işlevi, bir dizedeki her alanın ilk karakterini büyük harfe dönüştürür ve diğer tüm karakterler küçük harfe dönüştürülür.

**Sözdizimi:**  
`String PCase(string)`

**Açıklamalar:**

* Bu işlev şu anda kısaltma gibi tamamen büyük bir sözcüğü dönüştürmek için uygun kasa sağlamaz.

**Örnek:**  
`PCase("TEsT")`  
"Test"i döndürür.

`PCase(LCase("TEST"))`  
"Test"i döndürür

---
### <a name="randomnum"></a>RandomNum
**Açıklama:**  
RandomNum işlevi, belirli bir aralık arasında rasgele bir sayı döndürür.

**Sözdizimi:**  
`num RandomNum(num start, num end)`

* başlangıç: oluşturmak için rasgele değerin alt sınırını tanımlayan bir sayı
* sonu: oluşturmak için rasgele değerin üst sınırını tanımlayan bir sayı

**Örnek:**  
`Random(100,999)`  
734'e dönebilir.

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
**Açıklama:**  
Değiştir işlevi, bir dizedeki tüm oluşumları başka bir dizeyle değiştirir.

**Sözdizimi:**  
`str Replace(str string, str OldValue, str NewValue)`

* string: Değerlerin yerine geçecek dize.
* OldValue: Aranacak ve değiştirilecek dize.
* NewValue: Değiştirilecek dize.

**Açıklamalar:**  
Fonksiyon aşağıdaki özel monikers tanır:

* \n – Yeni Hat
* \r – Taşıma İadesi
* \t – Sekme

**Örnek:**  
`Replace([address],"\r\n",", ")`  
CRLF'nin yerine virgül ve boşluk yer alır ve "One Microsoft Way, Redmond, WA, USA"

---
### <a name="replacechars"></a>Yerine Chars
**Açıklama:**  
ReplaceChars işlevi, ReplacePattern dizesinde bulunan karakterlerin tüm oluşumlarının yerini alır.

**Sözdizimi:**  
`str ReplaceChars(str string, str ReplacePattern)`

* string: Karakterlerin yerine geçecek dize.
* ReplacePattern: değiştirilecek karakterleriçeren bir sözlük içeren dize.

Biçim {source1},{target1},{source2}:{target2},{sourceN},{targetN}, kaynak karakter bulmak ve değiştirmek için dize hedef.

**Açıklamalar:**

* İşlev, tanımlanan kaynakların her oluşumunu alır ve bunları hedeflerle değiştirir.
* Kaynak tam olarak bir (unicode) karakter olmalıdır.
* Kaynak boş veya bir karakterden uzun olamaz (ayrışma hatası).
* Hedef, örneğin ö:oe, β:ss gibi birden çok karaktere sahip olabilir.
* Hedef, karakterin kaldırılması gerektiğini belirten boş olabilir.
* Kaynak büyük/küçük harf duyarlıdır ve tam bir eşleşme olmalıdır.
* , (virgül) ve : (kolon) ayrılmış karakterlerdir ve bu işlev kullanılarak değiştirilemez.
* ReplacePattern dizesindeki boşluklar ve diğer beyaz karakterler yoksayılır.

**Örnek:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Raksmorgas'ı döndürür

`ReplaceChars("O’Neil",%ReplaceString%)`  
"ONeil"i döndürür, tek kene kaldırılacak şekilde tanımlanır.

---
### <a name="right"></a>Sağ
**Açıklama:**  
Sağ işlevi, bir dizesağ (sonu) karakter belirli bir sayı döndürür.

**Sözdizimi:**  
`str Right(str string, num NumChars)`

* string: karakterleri döndürmek için dize
* NumChars: dize sonundan (sağ) dönmek için karakter sayısını tanımlayan bir sayı

**Açıklamalar:**  
NumChars karakterleri dize son konumundan döndürülür.

Dizedeki son numChars karakterlerini içeren bir dize:

* numChars = 0 ise, boş dize döndürün.
* numChars 0'ı <, giriş dizelerini döndürün.
* Dize null ise, boş dize döndürün.

Dize NumChars belirtilen sayıdan daha az karakter içeriyorsa, dize ile aynı bir dize döndürülür.

**Örnek:**  
`Right("John Doe", 3)`  
"Doe"u döndürür.

---
### <a name="rtrim"></a>Rtrım
**Açıklama:**  
RTrim işlevi, bir dizeden sondaki beyaz boşlukları kaldırır.

**Sözdizimi:**  
`str RTrim(str value)`

**Örnek:**  
`RTrim(" Test ")`  
"Test"i döndürür.

---
### <a name="select"></a>Şunu seçin:
**Açıklama:**  
Belirtilen işlevi temel alan çok değerli bir öznitelik (veya bir ifade çıktısı) tüm değerleri işleme.

**Sözdizimi:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* öğe: Çok değerli öznitelikteki bir öğeyi temsil eder
* öznitelik: çok değerli öznitelik
* ifade: değerler koleksiyonunu döndüren bir ifade
* koşul: öznitelikbir öğeyi işleyebilir herhangi bir işlev

**Örnekler:**  
`Select($item,[otherPhone],Replace($item,"-",""))`  
Tireler kaldırıldıktan sonra otherPhone'daki çok değerli öznitelikteki tüm değerleri döndürün.

---
### <a name="split"></a>Bölme
**Açıklama:**  
Bölme işlevi bir sınır dışı ile ayrılmış bir dize alır ve çok değerli bir dize yapar.

**Sözdizimi:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* değeri: ayırmak için bir delimiter karakteri ile dize.
* delimiter: delimiter olarak kullanılacak tek karakter.
* limit: dönebilecek maksimum değer sayısı.

**Örnek:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
ProxyAddress özniteliği için yararlı 2 öğeiçeren çok değerli bir dize döndürür.

---
### <a name="stringfromguid"></a>StringFromGuid
**Açıklama:**  
StringFromGuid işlevi ikili bir GUID alır ve bir dize dönüştürür

**Sözdizimi:**  
`str StringFromGuid(bin GUID)`

---
### <a name="stringfromsid"></a>StringFromSid
**Açıklama:**  
StringFromSid işlevi, güvenlik tanımlayıcısı içeren bir bayt dizisini bir dize dönüştürür.

**Sözdizimi:**  
`str StringFromSid(bin ObjectSID)`  

---
### <a name="switch"></a>Anahtar
**Açıklama:**  
Switch işlevi, değerlendirilen koşullara göre tek bir değer döndürmek için kullanılır.

**Sözdizimi:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* expr: Değerlendirmek istediğiniz varyant ifade.
* değer: Karşılık gelen ifade Doğruysa döndürülecek değer.

**Açıklamalar:**  
Switch işlevi bağımsız değişken listesi ifade ve değer çiftlerinden oluşur. İfadeler soldan sağa değerlendirilir ve True'ya değerlendirilecek ilk ifadeyle ilişkili değer döndürülür. Parçalar düzgün bir şekilde eşleştirilemezse, çalışma zamanı hatası oluşur.

Örneğin, expr1 True ise, Anahtar değeri döndürür1. Expr-1 Yanlış ise, ancak expr-2 True ise, Anahtar değeri-2 ve benzeri döndürür.

Anahtar, şu şekilde olursa bir Hiçbir Şey döndürür:

* İfadelerin hiçbiri Doğru değil.
* İlk True ifadesi nin Karşılık gelen bir değeri Null'dur.

Switch, yalnızca bir ifadesini döndürse bile tüm ifadeleri değerlendirir. Bu nedenle, istenmeyen yan etkileri için izlemelisiniz. Örneğin, herhangi bir ifadenin değerlendirilmesi bir bölmeyle sıfır hatayla sonuçlanırsa, bir hata oluşur.

Değer, özel bir dize döndürecek hata işlevi de olabilir.

**Örnek:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Bazı büyük şehirlerde konuşulan dili döndürür, aksi takdirde bir Hata döndürür.

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
### <a name="ucase"></a>Ucase
**Açıklama:**  
UCase işlevi bir dizedeki tüm karakterleri büyük harfe dönüştürür.

**Sözdizimi:**  
`str UCase(str string)`

**Örnek:**  
`UCase("TeSt")`  
"TEST"i döndürür.

---
### <a name="where"></a>Konum

**Açıklama:**  
Belirli bir koşula bağlı olarak çok değerli bir öznitelikten (veya ifade çıktısından) bir değer alt kümesini döndürür.

**Sözdizimi:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* öğe: Çok değerli öznitelikteki bir öğeyi temsil eder
* öznitelik: çok değerli öznitelik
* durum: doğru veya yanlış değerlendirilebilir herhangi bir ifade
* ifade: değerler koleksiyonunu döndüren bir ifade

**Örnek:**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Süresi dolmamış çok değerli öznitelik kullanıcı Sertifikası'ndaki sertifika değerlerini döndürün.

---
### <a name="with"></a>With
**Açıklama:**  
With işlevi, karmaşık ifadede bir veya daha fazla kez görünen bir alt ifadeyi temsil etmek için bir değişken kullanarak karmaşık bir ifadeyi basitleştirmenin bir yolunu sağlar.

**Sözdizimi:**
`With(var variable, exp subExpression, exp complexExpression)`  
* değişken: Alt ifadeyi temsil eder.
* subExpression: değişken tarafından temsil edilen alt ifade.
* complexExpression: Karmaşık bir ifade.

**Örnek:**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
İşlevsel olarak eşdeğerdir:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
KullanıcıSertifikası özyündeki yalnızca süresi dolmamış sertifika değerlerini döndürür.


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

## <a name="additional-resources"></a>Ek Kaynaklar
* [Beyan Edici İfadeleri Anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)
* [Azure AD Connect Eşitleme: Senkronizasyon seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
