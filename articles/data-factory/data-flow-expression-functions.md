---
title: Haritalama veri akışında ifade işlevleri
description: Veri akışını eşlemede ifade işlevleri hakkında bilgi edinin.
author: kromerm
ms.author: makromer
manager: anandsub
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/15/2019
ms.openlocfilehash: d43650fc3dbd5fc1aabe676a4f2631e1655b25e5
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547950"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Veri akışını eşlemede veri dönüştürme ifadeleri 

## <a name="expression-functions"></a>İfade işlevleri

Veri Fabrikası'nda, veri dönüşümlerini yapılandırmak için eşleme veri akışı özelliğinin ifade dilini kullanın.
___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayının mutlak değeri.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
___
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir kosinüs ters değerini hesaplar* ``acos(1) -> 0.0``
___
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bir çift dize veya sayı ekler. Birkaç güne tarih ekler. Zaman damgasına süre ekler. Benzer türde bir diziyi diğerine ekler. + işleçle aynı* ``add(10, 20) -> 30``
* ``10 + 20 -> 30``
* ``add('ice', 'cream') -> 'icecream'``
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``
* ``add(toDate('2012-12-12'), 3) -> toDate('2012-12-15')``
* ``toDate('2012-12-12') + 3 -> toDate('2012-12-15')``
* ``[10, 20] + [30, 40] -> [10, 20, 30, 40]``
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``
___
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Bir tarih veya zaman damgasına gün ekleyin. Tarih için + işleçle aynı* ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')``
___
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral, [<i>&lt;value3&gt;</i> : string]) => datetime</b></code><br/><br/>
Bir tarih veya zaman damgasına aylar ekleyin. İsteğe bağlı olarak bir saat dilimi geçirebilirsiniz* ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')``
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> toTimestamp('2016-08-31 10:10:10')``
___
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal VE işleç.  && ile aynı* ``and(true, false) -> false``
* ``true && false -> false``
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Ters sinüs değerini hesaplar* ``asin(0) -> 0.0``
___
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Ters teğet değeri hesaplar* ``atan(0) -> 0.0``
___
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir düzlemin pozitif x ekseni ile koordinatlar tarafından verilen nokta arasındaki radyanlarda açıyı döndürür* ``atan2(0, 0) -> 0.0``
___
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
Akışta ada göre bir sütun değeri seçer. İsteğe bağlı bir akış adını ikinci bağımsız değişken olarak geçirebilirsiniz. Birden çok eşleşme varsa, ilk eşleşme döndürülür. Eşleşme yoksa NULL değeri döndürür. Döndürülen değer türü dönüştürme işlevlerinden biri tarafından dönüştürülür (TO_DATE, TO_STRING ...).  Tasarım zamanında bilinen sütun adları sadece adlarıyla ele alınmalıdır. Hesaplanan girişler desteklenmez, ancak parametre değiştirmelerini kullanabilirsiniz* ``toString(byName('parent'))``
* ``toLong(byName('income'))``
* ``toBoolean(byName('foster'))``
* ``toLong(byName($debtCol))``
* ``toString(byName('Bogus Column'))``
* ``toString(byName('Bogus Column', 'DeriveStream'))``
___
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Akıştaki göreli konumuna göre sütun değerini (1 tabanlı) seçer. Pozisyon sınırların dışındaysa, null değeri döndürür. Döndürülen değer, tür dönüştürme işlevlerinden biri tarafından dönüştürülür (TO_DATE, TO_STRING ...) Hesaplanan girişler desteklenmez, ancak parametre değiştirmelerini kullanabilirsiniz* ``toString(byPosition(1))``
* ``toDecimal(byPosition(2), 10, 2)``
* ``toBoolean(byName(4))``
* ``toString(byName($colName))``
* ``toString(byPosition(1234))``
___
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Alternatif koşullara göre bir değer ya da diğer geçerlidir. Giriş sayısı çift sayılsa, diğeri son koşul için NULL olarak varsayılan* ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``
* ``case(10 + 20 == 25, 'bojjus', 'do' < 'go', 'gunchus') -> 'gunchus'``
* ``isNull(case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus')) -> true``
* ``case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus', 'dumbo') -> 'dumbo'``
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının küp kökünü hesaplama* ``cbrt(8) -> 2.0``
___
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Numaradan küçük olmayan en küçük tümleç* ``ceil(-0.1) -> 0``
___
### <code>coalesce</code>
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Bir girdi kümesinden ilk geçersiz olmayan değeri verir. Tüm girişler aynı türde olmalıdır* ``coalesce(10, 20) -> 10``
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``
___
### <code>columnNames</code>
<code><b>columnNames(<i>&lt;value1&gt;</i> : string) => array</b></code><br/><br/>
Akış için tüm çıkış sütunlarını alır. İsteğe bağlı bir akış adını ikinci bağımsız değişken olarak geçirebilirsiniz.
* ``columnNames()``
* ``columnNames('DeriveStream')``
___
### <code>compare</code>
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
Aynı türden iki değeri karşılaştırır. Negatif tamsayı değeri1 < değeri2, 0 ise değer1 == değer2, pozitif değer ise değer1 > değeri2* ``(compare(12, 24) < 1) -> true``
* ``(compare('dumbo', 'dum') > 0) -> true``
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Değişken sayıda dizeyi biraraya getirir. Dizeleri ile + işleci ile aynı* ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'``
* ``'dataflow' + 'is' + 'awesome' -> 'dataflowisawesome'``
* ``isNull('sql' + null) -> true``
___
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Değişken sayıda dizeyi bir ayırıcıyla birleştirir. İlk parametre ayırıcı* ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'``
* ``isNull(concatWS(null, 'dataflow', 'is', 'awesome')) -> true``
* ``concatWS(' is ', 'dataflow', 'awesome') -> 'dataflow is awesome'``
___
### <code>contains</code>
<code><b>contains(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => boolean</b></code><br/><br/>
Sağlanan dizideki herhangi bir öğe, sağlanan yüklemde doğru olarak değerlendirirse doğru döndürür. İçerme, yüklem işlevindeki bir öğeye #item* ``contains([1, 2, 3, 4], #item == 3) -> true``
* ``contains([1, 2, 3, 4], #item > 5) -> false``
___
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir kosinüs değerini hesaplar* ``cos(10) -> -0.8390715290764524``
___
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir değerin hiperbolik kosinüshesaplar* ``cosh(0) -> 1.0``
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Yalnızca 0(256), 224, 256, 384, 512 değerlerinde olabilecek bit uzunluğu verilen farklı ilkel veri türlerinin sütun kümesinin CRC32 karmasını hesaplar. Bir satır için parmak izini hesaplamak için kullanılabilir* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L``
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Bu iş çalıştırmaya başladığında geçerli tarihi alır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. ["https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)
* ``currentDate() == toDate('2250-12-31') -> false``
* ``currentDate('PST')  == toDate('2250-12-31') -> false``
* ``currentDate('America/New_York')  == toDate('2250-12-31') -> false``
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
İş yerel saat dilimiyle çalışmabaşladığında geçerli zaman damgasını alır* ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false``
___
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Geçerli zaman damgasını UTC olarak alır. Geçerli saatinizin küme saat diliminizden farklı bir saat diliminde yorumlanmasını istiyorsanız, isteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Geçerli saat dilimi varsayılan olarak. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). UTC saatini UTC'den farklı bir saat dilimi kullanımına dönüştürmek için()* ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false``
* ``currentUTC() != toTimestamp('2050-12-12 19:18:12') -> true``
* ``fromUTC(currentUTC(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen ayın gününü alır* ``dayOfMonth(toDate('2018-06-08')) -> 8``
___
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen haftanın gününü alır. 1 - Pazar, 2 - Pazartesi ..., 7 - Cumartesi* ``dayOfWeek(toDate('2018-06-08')) -> 6``
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen yılın gününü alır* ``dayOfYear(toDate('2016-04-09')) -> 100``
___
### <code>days</code>
<code><b>days(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Gün sayısı için milisaniye cinsinden süre* ``days(2) -> 172800000L``
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Radyanları dereceye dönüştürür* ``degrees(3.141592653589793) -> 180``
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftini böler. / operatör le aynı* ``divide(20, 10) -> 2``
* ``20 / 10 -> 2``
___
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dize verilen dize ile sona erer seve denetler* ``endsWith('dumbo', 'mbo') -> true``
___
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleç. == işleci ile aynı* ``equals(12, 24) -> false``
* ``12 == 24 -> false``
* ``'bad' == 'bad' -> true``
* ``isNull('good' == toString(null)) -> true``
* ``isNull(null == null) -> true``
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
Karşılaştırma, işlecinin büyük/küçük harf yoksayma eşittir. <=> işleci ile aynı* ``'abc'<=>'Abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Bir sayının faktöriyelini hesaplama* ``factorial(5) -> 120``
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Her zaman yanlış bir değer döndürür. 'False' adlı bir sütun varsa sözdizimi(false()) işlevini kullanın* ``(10 + 20 > 30) -> false``
* ``(10 + 20 > 30) -> false()``
___
### <code>filter</code>
<code><b>filter(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => array</b></code><br/><br/>
Sağlanan yüklemle uymayan öğeleri diziden filtreler. Filtre, yüklem işlevindeki bir öğeye #item* ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]``
* ``filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') -> ['a', 'b']``
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Numaradan büyük olmayan en büyük tümseci döndürür* ``floor(-0.1) -> -1``
___
### <code>fromBase64</code>
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Verilen dizeyi base64'te kodlar* ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
UTC'nin zaman damgasına dönüştürür. Saat dilimini isteğe bağlı olarak 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Geçerli saat dilimi Varsayılan kullanılabilir biçimler için Java'nın SimpleDateFormat bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük işleç. > işleci yle aynı* ``greater(12, 24) -> false``
* ``('dumbo' > 'dum') -> true``
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük veya eşit işleç. >= işleç ile aynı* ``greaterOrEqual(12, 12) -> true``
* ``('dumbo' >= 'dum') -> true``
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Null değerleri atlayan girdi olarak değerler listesi arasındaki en büyük değeri döndürür. Tüm girişler null ise null döndürür* ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(10, toInteger(null), 20) -> 20``
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></code><br/><br/>
Akışta ada göre sütun değerini denetler. İsteğe bağlı bir akış adını ikinci bağımsız değişken olarak geçirebilirsiniz.  Tasarım zamanında bilinen sütun adları sadece adlarıyla ele alınmalıdır. Hesaplanan girişler desteklenmez, ancak parametre değiştirmelerini kullanabilirsiniz* ``hasColumn('parent')``
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Zaman damgasının saat değerini alır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Saat sayısı için milisaniye cinsinden süre* ``hours(2) -> 7200000L``
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Bir koşula göre bir değer veya diğer uygular. Diğer belirtilmemiş ise NULL olarak kabul edilir. Her iki değer uyumlu olmalıdır (sayısal, string...)* ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></code><br/><br/>
Değer NULL DeğİlSE DENETLER ve başka bir alternatif döndürür. İlk null olmayan değeri bulana kadar tüm girdileri sınar* ``iifNull(10, 20) -> 10``
* ``iifNull(null, 20, 40) -> 20``
* ``iifNull('bojjus', 'bo', 'dumbo') -> 'dumbo'``
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Bir öğenin dizide olup olmadığını denetler* ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Her sözcüğün ilk harfini büyük harfe dönüştürür. Sözcükler beyaz boşlukla ayrılmış olarak tanımlanır* ``initCap('cool iceCREAM') -> 'Cool Icecream'``
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Bir dize içindeki substring'in pozisyonunu(1 tabanlı) bulur. Bulunmazsa 0 döndürülür* ``instr('dumbo', 'mbo') -> 3``
* ``instr('microsoft', 'o') -> 5``
* ``instr('good', 'bad') -> 0``
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın siliniçin işaretli olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isDelete()``
* ``isDelete(1)``
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satır hata olarak işaretlenmiş olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isError()``
* ``isError(1)``
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satır ın yoksayılması için işaretli olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isIgnore()``
* ``isIgnore(1)``
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satır ekleme için işaretli olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isInsert()``
* ``isInsert(1)``
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Aramada satırın eşleşip eşleşmeyip eşleşmeyip eşleşmeyini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isMatch()``
* ``isMatch(1)``
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
DeğerIN NULL olup olmadığını denetler* ``isNull(NULL()) -> true``
* ``isNull('') -> false``
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
SatırGüncelleştirme için işaretli olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isUpdate()``
* ``isUpdate(1)``
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satır ekleme için işaretli olup olmadığını denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1* ``isUpsert()``
* ``isUpsert(1)``
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Verilen tarihin son tarihini alır* ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Karşılaştırma daha az veya eşit işleç. <= işleç ile aynı* ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Dizin 1'de karakter sayısıyla bir alt dize başlangıcı ayıklar. SUBSTRING(str, 1, n) ile aynı* ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Dize uzunluğunu verir* ``length('dumbo') -> 5``
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha az işleç. < işleci yle aynı* ``lesser(12, 24) -> true``
* ``('abcd' < 'abc') -> false``
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha az veya eşit işleç. <= işleç ile aynı* ``lesserOrEqual(12, 12) -> true``
* ``('dumbo' <= 'dum') -> false``
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
İki dize arasındaki levenshtein uzaklığı alır* ``levenshtein('boys', 'girls') -> 4``
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Desen kelimenin tam anlamıyla eşleşen bir dizedir. Özel durumlar aşağıdaki özel sembollerdir: _ girişteki herhangi bir karakterle eşleşir (. %, girişteki sıfır veya daha fazla karakterle eşleşir (posix normal ifadelerdeki .* ifadesine benzer).
Kaçış karakteri ''. Bir kaçış karakteri özel bir sembolden veya başka bir kaçış karakterinden önce yse, aşağıdaki karakter tam anlamıyla eşleşir. Başka bir karakter kaçmak için geçersizdir.
* ``like('icecream', 'ice%') -> true``
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Belirli bir konumdan başlayarak bir dize içindeki substring'in pozisyonunu(1 tabanlı) bulur. Pozisyon atlanırsa, dize başından itibaren kabul edilir. Bulunmazsa 0 döndürülür* ``locate('mbo', 'dumbo') -> 3``
* ``locate('o', 'microsoft', 6) -> 7``
* ``locate('bad', 'good') -> 0``
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Günlük değerini hesaplar. İsteğe bağlı bir taban, kullanılırsa başka bir euler numarası da sağlanabilir* ``log(100, 10) -> 2``
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Günlük değerini 10 temele göre hesaplar* ``log10(100) -> 2``
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bir dizeyi düşürür* ``lower('GunChus') -> 'gunchus'``
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Belirli bir uzunlukta olana kadar verilen dolgu tarafından dize sol pedleri. Dize uzunluğa eşit veya daha büyükse, ''lpad('dumbo', 8, '<>') -> * ``lpad('dumbo', 10, '-') -> '-----dumbo'`` 
* ``lpad('dumbo', 4, '-') -> 'dumb'`` 
* '<><dumbo'``
___
### <code>ltrim uzunluğuna kadar kesilir</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Sol, bir satır başkarakter dizesini kırpar. İkinci parametre belirtilmemişse, beyaz boşluğu kırpar. Aksi takdirde ikinci parametrede belirtilen herhangi bir karakteri kırpar* ``ltrim('  dumbo  ') -> 'dumbo  '``
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Verilen ifadeyi kullanarak dizinin her öğesini yeni bir öğeyle eşler. Harita, ifade işlevindeki bir öğeye #item* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/>
Verilen ifadeyi kullanarak dizinin her öğesini yeni bir öğeyle eşler. Harita, ifade işlevindeki bir öğeye #item olarak bir başvuru ve #index olarak öğe dizini için bir başvuru bekliyor* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerine ait sütun kümesinin MD5 özetini hesaplar ve 32 karakterli bir hex dizesini döndürür. Bir satır için parmak izini hesaplamak için kullanılabilir* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin milisaniye değerini alır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Milisaniye sayısı için milisaniye cinsinden süre* ``milliseconds(2) -> 2L``
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Numaraları çıkarır. Gün sayısı bir tarih çıkarma. Süreyi bir zaman damgasından çıkarın. Milisaniye cinsinden fark elde etmek için iki zaman damgası çıkarın. Aynı - operatör* ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Zaman damgasının dakika değerini alır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Dakika sayısı için milisaniye cinsinden süre* ``minutes(2) -> 120000L``
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Çift sayılar modülü. % işleç le aynı* ``mod(20, 8) -> 4``
* ``20 % 8 -> 4``
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih veya zaman damgasının ay değerini alır* ``month(toDate('2012-8-8')) -> 8``
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></code><br/><br/>
İki tarih arasındaki ay sayısını alır. Hesaplamayı tamamlayabilirsin. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftini çarpar. * işleç ile aynı* ``multiply(20, 10) -> 200``
* ``20 * 10 -> 200``
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayıyı inkar eder. Pozitif sayıları negatife çevirir ve tam tersi* ``negate(13) -> -13``
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Bir sonraki benzersiz sırayı döndürür. Sayı yalnızca bir bölüm içinde ardışık ve partitionId tarafından önceden belirlenmiş* ``nextSequence() == 12313112 -> false``
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
Aksanlı tek kodlu karakterleri ayırmak için dize değerini normalleştirme* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal olumsuzlama işleci* ``not(true) -> false``
* ``not(10 == 20) -> true``
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma işleci eşittir. != işleci ile aynı* ``12 != 24 -> true``
* ``'bojjus' != 'bo' + 'jjus' -> false``
___
### <code>notNull</code>
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Değer NULL değilse denetler* ``notNull(NULL()) -> false``
* ``notNull('') -> true``
___
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
NULL değeri verir. 'Null' adlı bir sütun varsa, sözdizimi(null()) işlevini kullanın. Kullanan herhangi bir işlem BIR NULL neden olur* ``isNull('dumbo' + null) -> true``
* ``isNull(10 * null) -> true``
* ``isNull('') -> false``
* ``isNull(10 + 20) -> false``
* ``isNull(10/0) -> true``
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal VEYA operatörüdür. ||* ``or(true, false) -> true``
* ``true || false -> true``
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Pozitif Sayı Çiftinin Modülü.
* ``pmod(-20, 8) -> 4``
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Giriş satırında geçerli bölüm id verir* ``partitionId()``
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir sayıyı diğerinin gücüne yükseltir* ``power(10, 2) -> 100``
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Bir dizideki öğeleri birikir. Azaltma #acc ve #item olarak ilk ifade işlevinde bir akümülatör ve bir öğe için bir başvuru bekliyor ve ikinci ifade işlevinde kullanılmak üzere #result olarak ortaya çıkan değeri bekliyor* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Belirli bir regex deseni için eşleşen bir alt dize ayıklayın. Son parametre eşleşme grubunu tanımlar ve atlanırsa varsayılan olarak 1'e ayarlanır. '''(geri<regex>teklif) bir dizeyi kaçmadan eşleştirmek için kullanın* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Dize verilen regex deseni eşleşip eşleşmeyin denetler. '''(geri<regex>teklif) bir dizeyi kaçmadan eşleştirmek için kullanın* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bir regex deseninin tüm oluşumlarını, bir dizeyi kaçmadan eşleştirmek için verilen string Use '(geri<regex>teklif) deki başka bir alt dizimle değiştirin* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Regex'e dayalı bir delimiter'e dayalı bir dizeyi böler ve bir dizi dize döndürür* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></code><br/><br/>
Verilen dizedeki başka bir alt dize ile bir alt dize tüm oluşumları değiştirin. Son parametre atlanırsa, boş dize için varsayılan* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``
* ``replace('doggie dog', 'dog', '') -> 'gie '``
* ``replace('doggie dog', 'dog') -> 'gie '``
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bir dizeyi tersine çevirir* ``reverse('gunchus') -> 'suhcnug'``
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Sağdan karakter sayısı ile bir alt dize ayıklar. SUBSTRING(str, LENGTH(str) - n, n) ile aynı* ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Dize verilen regex deseni yle eşleşip eşleşmeyin* ``rlike('200.50', `(\d+).(\d+)`) -> true``
* ``rlike('bogus', `M[0-9]+.*`) -> false``
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
İsteğe bağlı ölçek ve isteğe bağlı yuvarlama modu verilen bir sayıyı yuvarlar. Ölçek atlanırsa, varsayılan olarak 0'a ayarlanır.  Mod atlanırsa, varsayılan olarak ROUND_HALF_UP(5) olarak tanımlanır. Yuvarlama değerleri 1 - ROUND_UP 2 - ROUND_DOWN 3 - ROUND_CEILING 4 - ROUND_FLOOR 5 - ROUND_HALF_UP 6 - ROUND_HALF_DOWN 7 - ROUND_HALF_EVEN 8 - ROUND_UNNECESSARY* ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Sağ yastıkları belirli bir uzunlukta olana kadar verilen dolgu tarafından dize. Dize uzunluğa eşit veya daha büyükse, * ``rpad('dumbo', 10, '-') -> 'dumbo-----'`` 
* ``rpad('dumbo', 4, '-') -> 'dumb'`` 
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'`` 
___
### <code>rtrim</code>uzunluğa kadar kırpılır</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Sağ, bir dizi önde gelen karakteri kırpar. İkinci parametre belirtilmemişse, beyaz boşluğu kırpar. Aksi takdirde ikinci parametrede belirtilen herhangi bir karakteri kırpar* ``rtrim('  dumbo  ') -> '  dumbo'``
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin ikinci değerini alır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Yerel saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Saniye sayısı için milisaniye cinsinden süre* ``seconds(2) -> 2000L``
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerine ait sütun kümesinin SHA-1 özetini hesaplar ve 40 karakterli bir hex dizesini döndürür. Bir satır için parmak izini hesaplamak için kullanılabilir* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Sadece 0(256), 224, 256, 384, 512 değerlerinde olabilecek bit uzunluğu verilen farklı ilkel veri türlerinin sütun kümesinin SHA-2 özetini hesaplar. Bir satır için parmak izini hesaplamak için kullanılabilir* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Sinüs değerini hesaplar* ``sin(2) -> 0.9092974268256817``
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiperbolik sinüs değerini hesaplar* ``sinh(0) -> 0.0``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Bir konumdan bir dizinin alt kümesini ayıklar. Pozisyon 1 tabanlıdır. Uzunluk atlanırsa, dize sonuna varsayılan olarak* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/>
Sağlanan yüklem işlevini kullanarak diziyi sıralar. Sıralama, ifade işlevinde #item1 ve #item2 olarak art arda iki öğeye başvuru bekliyor* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Dize için soundex kodunu alır* ``soundex('genius') -> 'G520'``
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Bir dizeyi sınırlayıcıya göre böler ve bir dizi dize döndürür* ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının kare kökünü hesaplar* ``sqrt(9) -> 3``
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dize verilen dize ile başlar denetler* ``startsWith('dumbo', 'du') -> true``
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bir tarih veya zaman damgasından aylar çıkarın. Tarih için aynı - operatör* ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bir tarih veya zaman damgasından ay çıkarma* ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Bir konumdan belirli bir uzunluktaki bir alt dizeayıklar. Pozisyon 1 tabanlıdır. Uzunluk atlanırsa, dize sonuna varsayılan olarak* ``substring('Cat in the hat', 5, 2) -> 'in'``
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Teğet değeri hesaplar* ``tan(0) -> 0.0``
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiperbolik teğet değerini hesaplar* ``tanh(0) -> 0.0``
___
### <code>toBase64</code>
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Verilen dizeyi base64'te kodlar* ``toBase64('bojjus') -> 'Ym9qanVz'``
___
### <code>toBinary</code>
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></code><br/><br/>
Herhangi bir sayısal/tarih/zaman damgasını/dizeyi ikili gösterime dönüştürür* ``toBinary(3) -> [0x11]``
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
('t', 'true', 'y', 'yes', '1') değerini doğru ve ('f', 'false', 'n', 'no', '0') değeri başka bir değer için false ve NULL'a dönüştürür* ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``isNull(toBoolean('truthy')) -> true``
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
İsteğe bağlı giriş tarihi biçimini kullanarak giriş tarihi dizesini bugüne dönüştürür. Kullanılabilir biçimler için Java'nın SimpleDateFormat'ına bakın. Giriş tarihi biçimi atlanırsa, varsayılan biçim yyyy-[M]M-[d]d'dir. Kabul edilen biçimler :[ yyyy, yyyy-[M]M, yyyy-[M]M-[d]d, yyyy-[M]M-[d]d* ]* ``toDate('2012-8-18') -> toDate('2012-08-18')``
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi ondalık değere dönüştürür. Kesinlik ve ölçek belirtilmemişse, varsayılan olarak (10,2). Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. En-US, de, zh-CN gibi BCP47 dili şeklinde isteğe bağlı yerel biçim* ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi çift değere dönüştürür. Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. En-US, de, zh-CN gibi BCP47 dili şeklinde isteğe bağlı yerel biçim* ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi float değerine dönüştürür. Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. Herhangi bir çift truncates* ``toFloat(123.45) -> 123.45f``
* ``toFloat('123.45') -> 123.45f``
* ``toFloat('$123.45', '$###.00') -> 123.45f``
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir sayıdeğerine dönüştürür. Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. Herhangi bir uzun, float, çift truncates* ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi uzun bir değere dönüştürür. Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. Herhangi bir şamandıra, çift truncates* ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi kısa bir değere dönüştürür. Dönüştürme için isteğe bağlı Java ondalık biçimi kullanılabilir. Herhangi bir karşıcı, uzun, float, çift truncates* ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
İlkel bir veri türünü bir dize dönüştürür. Sayılar ve tarih için bir biçim belirtilebilir. Belirtilmemişse, sistem varsayılanı seçilir. Sayılar için Java ondalık biçimi kullanılır. Tüm olası tarih biçimleri için Java SimpleDateFormat'a bakın; varsayılan biçimi yyyy-MM-dd* ``toString(10) -> '10'``
* ``toString('engineer') -> 'engineer'``
* ``toString(123456.789, '##,###.##') -> '123,456.79'``
* ``toString(123.78, '000000.000') -> '000123.780'``
* ``toString(12345, '##0.#####E0') -> '12.345E3'``
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``
* ``isNull(toString(toDate('2018-12-31', 'MM/dd/yy'))) -> true``
* ``toString(4 == 20) -> 'false'``
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
İsteğe bağlı zaman damgası biçimi verilen bir dizeyi zaman damgasına dönüştürür. Tüm olası biçimler için Java SimpleDateFormat'a bakın. Zaman damgası atlanırsa varsayılan desen.yyyy-[M]M-[d]d hh:mm:ss[.f...] kullanılır. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Timestamp, mevcut biçimler için 999Refer Java'nın SimpleDateFormat değeriyle milisaniyeye kadar doğruluğu destekler. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Zaman damgasını UTC'ye dönüştürür. İsteğe bağlı bir saat dilimini 'GMT', 'PST', 'UTC', 'America/Cayman' şeklinde geçirebilirsiniz. Geçerli saat dilimi Varsayılan kullanılabilir biçimler için Java'nın SimpleDateFormat bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Bir karakter kümesini dizedeki başka bir karakter kümesiyle değiştirin. Karakterlerin 1-1 değiştirmesi vardır* ``translate('(bojjus)', '()', '[]') -> '[bojjus]'``
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Önde gelen ve sondaki karakterlerden oluşan bir dizisini kırpar. İkinci parametre belirtilmemişse, beyaz boşluğu kırpar. Aksi takdirde ikinci parametrede belirtilen herhangi bir karakteri kırpar* ``trim('  dumbo  ') -> 'dumbo'``
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``
___
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Her zaman gerçek bir değer döndürür. 'True' adlı bir sütun varsa sözdizimi(true()) işlevini kullanın* ``(10 + 20 == 30) -> true``
* ``(10 + 20 == 30) -> true()``
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Sütunun türüyle eşleşir. Yalnızca desen ifadelerinde kullanılabilir.sayı kısa, büyük, uzun, çift, float veya ondalık, integral eşleşmeleri kısa, büyüküstün, uzun, kesirli eşleşmeler çift, float, ondalık ve datetime eşleşmeleri tarih veya zaman damgası türüyle eşleşebilir* ``typeMatch(type, 'number')``
* ``typeMatch('date', 'datetime')``
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bir dize üst harf* ``upper('bojjus') -> 'BOJJUS'``
___
### <code>uuid</code>
<code><b>uuid() => string</b></code><br/><br/>
Oluşturulan UUID'yi döndürür* ``uuid()``
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen yılın haftasını alır* ``weekOfYear(toDate('2008-02-20')) -> 8``
___
### <code>weeks</code>
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Hafta sayısı için milisaniye cinsinden süre* ``weeks(2) -> 1209600000L``
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal XOR işleci. ^ işleci ile aynı* ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarihIn * ``year(toDate('2012-8-8')) -> 2012`` 
## yıl değerini alır Toplam işlevleri Aşağıdaki işlevler yalnızca toplam, pivot, unpivot ve pencere dönüşümlerinde kullanılabilir___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalamasını alır* ``avg(sales)``
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Bir ölçüte göre bir sütunun değerlerinin ortalamasını alır* ``avgIf(region == 'West', sales)``
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Değerlerin toplam sayısını alır. İsteğe bağlı sütun(lar) belirtilirse, sayımdaki NULL değerlerini yoksa* ``count(custId)``
* ``count(custId, custName)``
* ``count()``
* ``count(iif(isNull(custId), 1, NULL))``
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Bir sütun kümesinin farklı değerlerinin toplam sayısını alır* ``countDistinct(custId, custName)``
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Bir ölçüte göre değerlerin toplam sayısı alır. İsteğe bağlı sütun belirtilirse, sayımdaki NULL değerlerini yoksa* ``countIf(state == 'CA' && commission < 10000, name)``
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütun arasındaki popülasyon uyumuna sahip olur* ``covariancePopulation(sales, profit)``
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte göre, iki sütunun popülasyon uyumu* ``covariancePopulationIf(region == 'West', sales)``
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütunun örnek bir lâvesini alır* ``covarianceSample(sales, profit)``
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte göre, iki sütunun örnek bir arada* ``covarianceSampleIf(region == 'West', sales, profit)``
___
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Sütun grubunun ilk değerini alır. İkinci parametre yoksNulls atlanırsa, yanlış varsayılır* ``first(sales)``
* ``first(sales, false)``
___
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun kurtosis alır* ``kurtosis(sales)``
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir kritere göre, bir sütunun kurtoz* ``kurtosisIf(region == 'West', sales)``
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Sütun grubunun son değerini alır. İkinci parametre yoksNulls atlanırsa, yanlış varsayılır* ``last(sales)``
* ``last(sales, false)``
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bir sütunun maksimum değerini alır* ``max(sales)``
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun maksimum değerini alır* ``maxIf(region == 'West', sales)``
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalamasını alır. AVG ile aynı* ``mean(sales)``
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere göre bir sütunun değerleri ortalaması alır. avgIf ile aynı* ``meanIf(region == 'West', sales)``
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bir sütunun minimum değerini alır* ``min(sales)``
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun minimum değerini alır* ``minIf(region == 'West', sales)``
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun çarpıklığını alır* ``skewness(sales)``
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte göre, bir sütunun çarpıklığını alır* ``skewnessIf(region == 'West', sales)``
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun standart sapması alır* ``stdDev(sales)``
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun standart sapması alır* ``stddevIf(region == 'West', sales)``
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun popülasyon standart sapması alır* ``stddevPopulation(sales)``
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun popülasyon standardı sapması alır* ``stddevPopulationIf(region == 'West', sales)``
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun örnek standart sapması alır* ``stddevSample(sales)``
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun örnek standart sapması alır* ``stddevSampleIf(region == 'West', sales)``
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayısal bir sütunun toplam toplamını alır* ``sum(col)``
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayısal bir sütunun farklı değerlerinin toplam toplamını alır* ``sumDistinct(col)``
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere göre sayısal bir sütunun toplam toplamını alır. Koşul herhangi bir sütuna dayalı olabilir* ``sumDistinctIf(state == 'CA' && commission < 10000, sales)``
* ``sumDistinctIf(true, sales)``
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere göre sayısal bir sütunun toplam toplamını alır. Koşul herhangi bir sütuna dayalı olabilir* ``sumIf(state == 'CA' && commission < 10000, sales)``
* ``sumIf(true, sales)``
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun varyansını alır* ``variance(sales)``
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun varyansını alır* ``varianceIf(region == 'West', sales)``
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun popülasyon farkını alır* ``variancePopulation(sales)``
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütunun popülasyon varyansını alır* ``variancePopulationIf(region == 'West', sales)``
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun tarafsız varyansını alır* ``varianceSample(sales)``
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir ölçüte bağlı olarak, bir sütun * ``varianceSampleIf(region == 'West', sales)`` 
## Penceresi işlevlerinin tarafsız varyansını alır Aşağıdaki işlevler yalnızca pencere dönüşümlerinde kullanılabilir___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
CumeDist işlevi, bölümdeki tüm değerlere göre bir değerin konumunu hesaplar. Sonuç, pencere bölümündeki toplam satır sayısına bölünen bölümün sıralanmasında geçerli satırdan önce veya eşit satır sayısıdır. Sıralamadaki herhangi bir kravat değerleri aynı pozisyonda değerlendirilir.
* ``cumeDist()``
___
### <code>denseRank</code>
<code><b>denseRank() => integer</b></code><br/><br/>
Bir pencerenin sırasına göre belirtilen değerler grubundaki bir değerin sıralamasını maddeyle hesaplar. Sonuç, bölüm sıralanmasında geçerli satırdan önce veya eşit satır sayısı artıdır. Değerler sırayla boşluklar üretmez. Yoğun Sıralama, veriler sıralanmadığında ve değerlerde değişiklik aradığında bile çalışır* ``denseRank()``
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Geçerli satırdan önce değerlendirilen ilk parametrenin değerini alır. İkinci parametre geriye bakmak için satır sayısıdır ve varsayılan değer 1'dir. Çok sayıda satır yoksa, varsayılan değer belirtilmedikçe null değeri döndürülür* ``lag(amount, 2)``
* ``lag(amount, 2000, 100)``
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Geçerli satırdan sonra değerlendirilen ilk parametrenin değerini alır. İkinci parametre ileriye bakmak için satır sayısıdır ve varsayılan değer 1'dir. Çok sayıda satır yoksa, varsayılan değer belirtilmedikçe null değeri döndürülür* ``lead(amount, 2)``
* ``lead(amount, 2000, 100)``
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
NTile işlevi, her pencere bölümü için `n` satırları 1 ile en fazla `n`1 arasında değişen kovalara böler. Kova değerleri en fazla 1'e göre değişir. Bölümdeki satır sayısı kova sayısına eşit olarak bölünmezse, kalan değerler ilk kovadan başlayarak kova başına bir olarak dağıtılır. NTile işlevi tertiles, kuvarslar, deciles ve diğer ortak özet istatistikleri hesaplamak için yararlıdır. İşlev başlatma sırasında iki değişken hesaplar: Normal bir kovanın boyutuna bir ek satır eklenir. Her iki değişken de geçerli bölümün boyutuna dayanır. Hesaplama işlemi sırasında işlev geçerli satır numarasını, geçerli kova numarasını ve kovanın değişeceği satır numarasını (kovaEşik) izler. Geçerli satır numarası kova eşiğine ulaştığında, kova değeri bir artırılır ve eşik kova boyutuna göre artırılır (artı geçerli kova dolguluysa bir ekstra).
* ``nTile()``
* ``nTile(numOfBuckets)``
___
### <code>rank</code>
<code><b>rank() => integer</b></code><br/><br/>
Bir pencerenin sırasına göre belirtilen değerler grubundaki bir değerin sıralamasını maddeyle hesaplar. Sonuç, bölüm sıralanmasında geçerli satırdan önce veya eşit satır sayısı artıdır. Değerler sırayla boşluklar üretecektir. Sıralama, veriler sıralanmadığında ve değerlerde değişiklik aradığında bile çalışır* ``rank()``
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
1 ile başlayan bir penceredeki satırlar için sıralı satır numaralandırması atar* ``rowNumber()``

## <a name="next-steps"></a>Sonraki adımlar

[İfade Oluşturucu'nun nasıl kullanılacağını öğrenin.](concepts-data-flow-expression-builder.md)
