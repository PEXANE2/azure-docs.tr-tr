---
title: Sorgu hızlandırma SQL dil başvurusu (önizleme)
titleSuffix: Azure Storage
description: Sorgu hızlandırma sql sözdizimini nasıl kullanacağınızı öğrenin.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.reviewer: ereilebr
ms.openlocfilehash: cea5fb507225f063e2d48c56fae254e123a8f72b
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81772125"
---
# <a name="query-acceleration-sql-language-reference-preview"></a>Sorgu hızlandırma SQL dil başvurusu (önizleme)

Sorgu hızlandırma, blob içeriği üzerinde sorguları ifade etmek için ANSI SQL benzeri bir dili destekler.  Sorgu hızlandırma SQL lehçesi, desteklenen veri türleri, işleçler, vb sınırlı bir dizi ile ANSI SQL bir alt kümesidir, ama aynı zamanda JSON gibi hiyerarşik yarı yapılandırılmış veri biçimleri üzerinde sorguları desteklemek için ANSI SQL genişletir. 

> [!NOTE]
> Sorgu hızlandırma özelliği genel önizlemededir ve Kanada Orta ve Fransa Orta bölgelerinde kullanılabilir. Sınırlamaları gözden geçirmek için [Bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın. Önizlemeye kaydolmak için [bu forma](https://aka.ms/adls/qa-preview-signup)bakın. 

## <a name="select-syntax"></a>SELECT Sözdizimi

Sorgu ivmesi tarafından desteklenen tek SQL deyimi SELECT deyimidir. Bu örnek, ifadenin doğru döndürdüğü her satırı döndürür.

```sql
SELECT * FROM table [WHERE expression] [LIMIT limit]
```

CSV biçimli veriler *table* için tablo `BlobStorage`.  Bu, sorgunun REST çağrısında belirtilen blob'a karşı çalışacağı anlamına gelir.
JSON biçimlendirilmiş veriler için *tablo* bir "tablo tanımlayıcısı" dır.   Bu makalenin [Tablo Tanımlayıcıları](#table-descriptors) bölümüne bakın.

Aşağıdaki örnekte, WHERE *ifadesinin* doğru döndüğü her satır için, bu deyim, projeksiyon ifadelerinin her birini değerlendirmekten yapılan yeni bir satır döndürür.


```sql
SELECT expression [, expression …] FROM table [WHERE expression] [LIMIT limit]
```

Aşağıdaki örnek, *ifadenin* doğru döndüğü satırların her biri üzerinde bir toplu hesaplama (örneğin: belirli bir sütunun ortalama değeri) döndürür. 

```sql
SELECT aggregate_expression FROM table [WHERE expression] [LIMIT limit]
```

Aşağıdaki örnek, CSV biçimli bir blob'u bölmek için uygun uzaklıkları döndürür.  Bu makalenin [Sys.Split](#sys-split) bölümüne bakın.

```sql
SELECT sys.split(split_size)FROM BlobStorage
```

<a id="data-types" />

## <a name="data-types"></a>Veri Türleri

|Veri Türü|Açıklama|
|---------|-------------------------------------------|
|INT      |64 bit imzalı biranda.                     |
|Float    |64 bit ("çift duyarlıklı") kayan nokta.|
|Dize   |Değişken uzunlukta Unicode dizesi.            |
|Zaman damgası|Zamanda bir nokta.                           |
|Boolean  |True veya false.                             |

CSV biçimli verilerden değerler okunurken, tüm değerler dize olarak okunur.  String değerleri CAST ifadeleri kullanılarak diğer türlere dönüştürülebilir.  Değerler, içeriğe bağlı olarak dolaylı olarak diğer türlere dökülebilir. daha fazla bilgi için [Bkz. Veri türü önceliği (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-precedence-transact-sql?view=sql-server-2017).

## <a name="expressions"></a>İfadeler

### <a name="referencing-fields"></a>Başvuru alanları

JSON biçimli veriler veya üstbilgi satırı olan CSV biçimli veriler için alanlar ada göre başvurulabilir.  Alan adları alıntılanabilir veya tırnak içinde kalınabilir. Alıntı lanan alan adları çift tırnak karakterleri (") ile çevrilidir, boşluklar içerebilir ve büyük/küçük harf duyarlıdır.  Tırnak içinde verilmemiş alan adları büyük/küçük harf duyarsızdır ve özel karakter içermeyebilir.

CSV biçimli verilerde, alanlar alt (_) karakterle önceden belirlenmiş olan ordinal tarafından da başvurulabilir.  Örneğin, ilk alan _1 veya onbirinci alan _11 olarak başvurulabilir.  Alanları ordinal'a göre başvurmak, üstbilgi satırı içermeyen CSV biçimli veriler için yararlıdır ve bu durumda belirli bir alana başvurmanın tek yolu ordinal'dir.

### <a name="operators"></a>İşleçler

Aşağıdaki standart SQL işleçleri desteklenir:

``=``, ``!=``, ``<>``, ``<``, ``<=``, ``>``, ``>=``, ``+``, ``-``, ``/``, ``*``, ``%``, ``AND``, ``OR``, ``NOT``, ``CAST``, ``BETWEEN``, ``IN``, ``NULLIF``, ``COALESCE``

Bir işlecinin solundaki ve sağındaki veri türleri farklıysa, burada belirtilen kurallara göre otomatik dönüştürme gerçekleştirilir: [Veri türü önceliği (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-precedence-transact-sql?view=sql-server-2017).

Sorgu hızlandırma SQL dili, bu makalede tartışılan veri türlerinin yalnızca çok küçük bir alt kümesini destekler.  Bu makalenin [Veri Türleri](#data-types) bölümüne bakın.

### <a name="casts"></a>Çevirir

Sorgu ivmesi SQL dili, buradaki kurallara göre CAST işlecisini destekler: [Veri türü dönüştürme (Database Engine)](https://docs.microsoft.com/sql/t-sql/data-types/data-type-conversion-database-engine?view=sql-server-2017).  

Sorgu hızlandırma SQL dili, bu makalede tartışılan veri türlerinin yalnızca küçük bir alt kümesini destekler.  Bu makalenin [Veri Türleri](#data-types) bölümüne bakın.

### <a name="string-functions"></a>Dize işlevleri

Sorgu hızlandırma SQL dili aşağıdaki standart SQL dize işlevlerini destekler:

``LIKE``, ``CHAR_LENGTH``, ``CHARACTER_LENGTH``, ``LOWER``, ``UPPER``, ``SUBSTRING``, ``TRIM``, ``LEADING``, ``TRAILING``.

Aşağıda birkaç örnek verilmiştir:

|İşlev|Örnek|Sonuç|
|---|---|---|
|CHARACTER_LENGTH|``SELECT CHARACTER_LENGTH('abcdefg') from BlobStorage`` |``7``|
|CHAR_LENGTH|``SELECT CHAR_LENGTH(_1) from BlobStorage``|``1``|
|LOWER|``SELECT LOWER('AbCdEfG') from BlobStorage``|``abcdefg``|
|UPPER|``SELECT UPPER('AbCdEfG') from BlobStorage``|``ABCDEFG``|
|SUBSTRING|``SUBSTRING('123456789', 1, 5)``|``23456``|
|TRIM|``TRIM(BOTH '123' FROM '1112211Microsoft22211122')``|``Microsoft``|

[LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver15) işlevi bir desen aramanıza yardımcı olur. Aşağıda, veri dizesini ``abc,abd,cd\ntest,test2,test3\na_bc,xc%d^e,gh[i ``aramak için [LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver15) işlevini kullanan birkaç örnek verilmiştir.

|Sorgu|Örnek|
|--|--|
|``SELECT _1, _2, _3 from BlobStorage where _2 LIKE 'a%'``|``abc,abd,cd\n``|
|``SELECT * from BlobStorage where _1 LIKE 'a[bcd]c``|``abc,abd,cd\n``|
|``SELECT _1 from BlobStorage where _2 LIKE '[^xyz]%'``|``abc\ntest\n``|
|``SELECT * from BlobStorage where _1 LIKE 'a_``|``abc,abd,cd\n``|
|``SELECT _2,_3 from BlobStorage where _3 LIKE '[g-h]_![[a-j]' Escape '!'``|``xc%d^e,gh[i\n``|

### <a name="date-functions"></a>Tarih işlevleri

Aşağıdaki standart SQL tarih işlevleri desteklenir:

``DATE_ADD``, ``DATE_DIFF``, ``EXTRACT``, ``TO_STRING``, ``TO_TIMESTAMP``.

Şu anda [standart IS08601](https://www.w3.org/TR/NOTE-datetime)tüm tarih biçimlerini dönüştürmek. 

#### <a name="date_add-function"></a>DATE_ADD fonksiyonu

Sorgu hızlandırma SQL dili ``DATE_ADD`` işlevi için yıl, ay, gün, saat, dakika, ikinci destekler.

Örnekler:

```sql
DATE_ADD(datepart, quantity, timestamp)
DATE_ADD('minute', 1, CAST('2017-01-02T03:04:05.006Z' AS TIMESTAMP)
```

#### <a name="date_diff-function"></a>DATE_DIFF fonksiyonu

Sorgu hızlandırma SQL dili ``DATE_DIFF`` işlevi için yıl, ay, gün, saat, dakika, ikinci destekler.

```sql
DATE_DIFF(datepart, timestamp, timestamp)
DATE_DIFF('hour','2018-11-09T00:00+05:30','2018-11-09T01:00:23-08:00') 
```

#### <a name="extract-function"></a>EXTRACT fonksiyonu

``DATE_ADD`` İşlev için desteklenen tarih parçası dışındaki EXTRACT için sorgu hızlandırma SQL dili timezone_hour destekler ve tarih parçası olarak timezone_minute.

Örnekler:

```sql
EXTRACT(datepart FROM timestampstring)
EXTRACT(YEAR FROM '2010-01-01T')
```

#### <a name="to_string-function"></a>TO_STRING fonksiyonu

Örnekler:

```sql
TO_STRING(TimeStamp , format)
TO_STRING(CAST('1969-07-20T20:18Z' AS TIMESTAMP),  'MMMM d, y')
```

Bu tablo, ``TO_STRING`` işlevin çıktı biçimini belirtmek için kullanabileceğiniz dizeleri açıklar.

|Biçim dizesi    |Çıktı                               |
|-----------------|-------------------------------------|
|yy               |Yıl 2 haneli formatta - 1999 '99' olarak|
|y                |4 haneli formatta yıl               |
|yyyy             |4 haneli formatta yıl               |
|M                |Yılın Ayı – 1                    |
|MM               |Sıfır yastıklı Ay – 01               |
|AAA              |Abbr, ne kadar. yıl ayı -OCAK            |
|MMMM             |Tam ay – Mayıs                      |
|d                |Ayın günü (1-31)                  |
|Ekle               |Ayın sıfır yastıklı günü (01-31)     |
|a                |veya PM                             |
|h                |Günün saati (1-12)                   |
|hh               |Sıfır yastıklı Saat od gün (01-12)     |
|H                |Günün saati (0-23)                   |
|HH               |Günün Sıfır Yastıklı Saat (00-23)      |
|m                |Dakika saat (0-59)                |
|mm               |Sıfır yastıklı dakika (00-59)           |
|s                |Dakika Nın İkinci (0-59)             |
|ss               |Sıfır yastıklı Saniye (00-59)          |
|S                |Saniye Kesir (0.1-0.9)        |
|SS               |SaniyeLerin Kesir (0.01-0.99)      |
|SSS              |SaniyeLerin Kesir (0.001-0.999)    |
|X                |Saatlerde Ofset                      |
|XX veya XXXX       |Saat ve dakika mahsup (+0430)  |
|XXX veya XXXXX     |Saat ve dakika içinde ofset (-07:00) |
|x                |Saat içinde ofset (7)                  |
|xx veya xxxx       |Saat dakika ofset (+0530)    |
|Xxx veya xxxxx     |Saat dakika ve saat ofset (+05:30)   |

#### <a name="to_timestamp-function"></a>TO_TIMESTAMP fonksiyonu

Yalnızca IS08601 biçimleri desteklenir.

Örnekler:

```sql
TO_TIMESTAMP(string)
TO_TIMESTAMP('2007T')
```

> [!NOTE]
> Ayrıca sistem zamanı ``UTCNOW`` almak için işlevi kullanabilirsiniz.


## <a name="aggregate-expressions"></a>Toplu İfadeler

SELECT deyimi bir veya daha fazla projeksiyon deyimi veya tek bir toplu ifade içerebilir.  Aşağıdaki toplu ifadeler desteklenir:

|İfadeler|Açıklama|
|--|--|
|[SAYIN(\*)](https://docs.microsoft.com/sql/t-sql/functions/count-transact-sql?view=sql-server-ver15)    |Yüklem ifadesiyle eşleşen kayıt sayısını döndürür.|
|[COUNT(ifade)](https://docs.microsoft.com/sql/t-sql/functions/count-transact-sql?view=sql-server-ver15)    |İfadenin null olmayan kayıt sayısını verir.|
|[ORTALAMA(ifade)](https://docs.microsoft.com/sql/t-sql/functions/avg-transact-sql?view=sql-server-ver15)    |İfadenin null olmayan değerlerinin ortalamasını verir.|
|[MIN(ifade)](https://docs.microsoft.com/sql/t-sql/functions/min-transact-sql?view=sql-server-ver15)    |İfadenin minimum null olmayan değerini verir.|
|[MAX(ifade](https://docs.microsoft.com/sql/t-sql/functions/max-transact-sql?view=sql-server-ver15))    |İfadenin en fazla null olmayan değerini verir.|
|[SUM(ifade)](https://docs.microsoft.com/sql/t-sql/functions/sum-transact-sql?view=sql-server-ver15)    |İfadenin tüm null olmayan değerlerinin toplamını verir.|

### <a name="missing"></a>Eksik

İşleç, ``IS MISSING`` sorgu hızlandırma SQL dilinin desteklediği tek standart dışı dır.  JSON verileri için, belirli bir giriş kaydından bir alan ``IS MISSING`` eksikse, ifade alanı Boolean değeri doğru olarak değerlendirilir.

<a id="table-descriptors" />

## <a name="table-descriptors"></a>Tablo Tanımlayıcıları

CSV verileri için tablo adı `BlobStorage`her zaman.  Örneğin:

```sql
SELECT * FROM BlobStorage
```

JSON verileri için ek seçenekler mevcuttur:

```sql
SELECT * FROM BlobStorage[*].path
```

Bu, JSON verilerinin alt kümeleri üzerinde sorguyapılmasına izin verir.

JSON sorguları için, FROM yan tümcesinin bir bölümünde ki yoldan bahsedebilirsiniz. Bu yollar JSON verilerinin alt kümesini ayrıştırmaya yardımcı olur. Bu yollar JSON Dizi ve Nesne değerlerine başvurulabilir.

Bunu daha ayrıntılı olarak anlamak için bir örnek alalım.

Bu bizim örnek verilerimiz:

```json
{
  "id": 1,
  "name": "mouse",
  "price": 12.5,
  "tags": [
    "wireless",
    "accessory"
  ],
  "dimensions": {
    "length": 3,
    "width": 2,
    "height": 2
  },
  "weight": 0.2,
  "warehouses": [
    {
      "latitude": 41.8,
      "longitude": -87.6
    }
  ]
}
```

Yukarıdaki verilerden yalnızca `warehouses` JSON nesnesi ile ilgilenebilirsiniz. Nesne `warehouses` bir JSON dizi türüdür, bu nedenle FROM yan tümcesinde bu söz edebilirsiniz. Örnek sorgunuz buna benzer bir şey olabilir.

```sql
SELECT latitude FROM BlobStorage[*].warehouses[*]
```

Sorgu tüm alanları alır, ancak yalnızca enlem seçer.

Yalnızca JSON nesne `dimensions` değerine erişmek istiyorsanız, sorgunuzdaki nesneye başvuru yapmak için kullanabilirsiniz. Örneğin:

```sql
SELECT length FROM BlobStorage[*].dimensions
```

Bu, nesnenin `dimensions` üyelerine erişiminizi de sınırlar. JSON alanlarının diğer üyelerine ve JSON nesnelerinin iç değerlerine erişmek istiyorsanız, aşağıdaki örnekte gösterildiği gibi bir sorgu kullanabilirsiniz:

```sql
SELECT weight,warehouses[0].longitude,id,tags[1] FROM BlobStorage[*]
```

> [!NOTE]
> BlobStorage ve BlobStorage[]\*her ikisi de tüm nesneyi ifade eder. Ancak, FROM yan tümcesinde bir yolunuz varsa, BlobStorage[].path\*

<a id="sys-split" />

## <a name="syssplit"></a>Sys.Split

Bu, yalnızca CSV biçimli veriler için kullanılabilen SELECT deyiminin özel bir biçimidir.

```sql
SELECT sys.split(split_size)FROM BlobStorage
```

CSV veri kayıtlarını toplu olarak indirmek ve sonra işlemek istediğiniz durumlarda bu deyimi kullanın. Bu şekilde, tüm kayıtları tek seferde indirmek zorunda kalmak yerine kayıtları paralel olarak işleyebilirsiniz. Bu bildirim CSV dosyasındaki kayıtları döndürmez. Bunun yerine, toplu iş boyutları bir koleksiyon döndürür. Daha sonra veri kayıtları toplu almak için her toplu iş boyutunu kullanabilirsiniz. 

Her toplu iş içermesini istediğiniz bayt sayısını belirtmek için *split_size* parametresini kullanın. Örneğin, bir seferde yalnızca 10 MB veri işlemek istiyorsanız, deyimi şu şekilde `SELECT sys.split(10485760)FROM BlobStorage` görünür: 10 MB 10.485.760 bayt adedine eşit olduğundan. Her toplu iş, bu 10 MB'a sığabileceği kadar çok kayıt içerir. 

Çoğu durumda, her toplu iş boyutu belirttiğiniz sayıdan biraz daha yüksek olacaktır. Bunun nedeni, bir toplu iş partisinin kısmi bir kayıt içerememesidir. Bir toplu iş teki son kayıt eşiğizin bitiminden önce başlarsa, toplu iş tam kaydı içerebilmek için daha büyük olur. Son toplu iş boyutu büyük olasılıkla belirttiğiniz boyuttan daha küçük olacaktır.

>[!NOTE]
> split_size en az 10 MB (10485760) olmalıdır.

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Veri Gölü Depolama sorgu hızlandırma (önizleme)](data-lake-storage-query-acceleration.md)
- [Azure Veri Gölü Depolama sorgu hızlandırma (önizleme) kullanarak verileri filtreleme](data-lake-storage-query-acceleration-how-to.md)

