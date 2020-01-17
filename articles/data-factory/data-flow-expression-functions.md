---
title: Eşleme veri akışındaki ifade işlevleri
description: Eşleme veri akışındaki ifade işlevleri hakkında bilgi edinin.
author: kromerm
ms.author: makromer
manager: anandsub
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/15/2019
ms.openlocfilehash: c85ce38f88ecf113a9d27e82b0be12bbfb0b3b47
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122670"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Eşleme veri akışındaki veri dönüştürme ifadeleri 

## <a name="expression-functions"></a>İfade işlevleri

Data Factory ' de, veri dönüşümlerini yapılandırmak için veri akışını eşleme özelliğinin ifade dilini kullanın.
___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayının mutlak değeri.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
___
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``acos(1) -> 0.0``
___
### <code>add</code>bir kosinüs ters değerini hesaplar 
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bir çift dize veya sayı ekler. Gün sayısı için bir tarih ekler. Zaman damgasına bir süre ekler. Benzer türdeki bir diziyi diğerine ekler. \+ İşleci ile aynı * ``add(10, 20) -> 30``
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
Tarih veya zaman damgasına gün ekleyin. Tarih * ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')``
___
### <code>addMonths</code>için + işleciyle aynı 
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral, [<i>&lt;value3&gt;</i> : string]) => datetime</b></code><br/><br/>
Tarih veya zaman damgasına aylar ekleyin. İsteğe bağlı olarak, 
___
### <code>and</code>
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> toTimestamp('2016-08-31 10:10:10')``* ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')``saat dilimi geçirebilirsiniz 
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal AND işleci. & & * ``and(true, false) -> false``
* ``true && false -> false``
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Ters Sinüs değeri * ``asin(0) -> 0.0``
___
### <code>atan</code>hesaplar 
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``atan(0) -> 0.0``
___
### <code>atan2</code>ters tanjant değerini hesaplar 
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Düzlemin pozitif x ekseni ve koordinat tarafından verilen nokta arasındaki radyan cinsinden açıyı döndürür 
<code><b>byName(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => any</b></code> * ``atan2(0, 0) -> 0.0``
___
### <code>byName</code><br/><br/>
Akıştaki ada göre bir sütun değeri seçer. İkinci bağımsız değişken olarak isteğe bağlı bir akış adı geçirebilirsiniz. Birden çok eşleşme varsa, ilk eşleşme döndürülür. Eğer eşleşme yoksa NULL değer döndürür. Döndürülen değerin tür dönüştürme işlevlerinden (TO_DATE, TO_STRING...) birine dönüştürülmesi gerekir.  Tasarım zamanında bilinen sütun adları yalnızca adına göre değinmelidir. Hesaplanan girişler desteklenmez, ancak 
* ``toLong(byName('income'))``
* ``toBoolean(byName('foster'))``
* ``toLong(byName($debtCol))``
* ``toString(byName('Bogus Column'))``
* ``toString(byName('Bogus Column', 'DeriveStream'))``
___
### <code>byPosition</code>* ``toString(byName('parent'))``parametre değiştirmeler kullanabilirsiniz 
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Akışta göreli konumuna göre (1 tabanlı) bir sütun değeri seçer. Konum sınırların dışında ise, NULL bir değer döndürür. Döndürülen değer tür dönüştürme işlevlerinden biri tarafından dönüştürülmüş tür olmalıdır (TO_DATE, TO_STRING...) Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler * ``toString(byPosition(1))``
* ``toDecimal(byPosition(2), 10, 2)``
* ``toBoolean(byName(4))``
* ``toString(byName($colName))``
* ``toString(byPosition(1234))``
___
### <code>case</code>de kullanabilirsiniz 
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Değişen koşullara göre bir değer veya diğeri geçerlidir. Giriş sayısı bile eşit ise, diğer son koşula göre varsayılan değer NULL olarak ayarlanır * ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``
* ``case(10 + 20 == 25, 'bojjus', 'do' < 'go', 'gunchus') -> 'gunchus'``
* ``isNull(case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus')) -> true``
* ``case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus', 'dumbo') -> 'dumbo'``
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının Küp kökünü * ``cbrt(8) -> 2.0``
___
### <code>ceil</code>hesaplama 
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
* ``ceil(-0.1) -> 0``
___
### <code>coalesce</code>sayıdan küçük olan en küçük tamsayıyı döndürür 
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Giriş kümesinden gelen ilk null değeri döndürür. Tüm girdilerin * ``coalesce(10, 20) -> 10``
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``
___
### <code>compare</code>aynı türde olması gerekir 
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
Aynı türdeki iki değeri karşılaştırır. Değer1 < değer2 ise negatif tamsayı döndürür, Değer1 = = değer2 ise pozitif değer, > değer2 * ``(compare(12, 24) < 1) -> true``
* ``(compare('dumbo', 'dum') > 0) -> true``
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Değişken sayıda dizeyi birlikte birleştirir. * ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'``
* ``'dataflow' + 'is' + 'awesome' -> 'dataflowisawesome'``
* ``isNull('sql' + null) -> true``
___
### <code>concatWS</code>dizimiyle + işleciyle aynı 
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Bir dizi dizeyi bir ayırıcıyla birlikte birleştirir. İlk parametre ayırıcı * ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'``
* ``isNull(concatWS(null, 'dataflow', 'is', 'awesome')) -> true``
* ``concatWS(' is ', 'dataflow', 'awesome') -> 'dataflow is awesome'``
___
### <code>contains</code>
<code><b>contains(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => boolean</b></code><br/><br/>
Belirtilen dizideki herhangi bir öğe, belirtilen koşulda doğru olarak değerlendiriliyorsa true değerini döndürür. Contains, #item * ``contains([1, 2, 3, 4], #item == 3) -> true``
* ``contains([1, 2, 3, 4], #item > 5) -> false``
___
### <code>cos</code>olarak koşul işlevindeki bir öğeye başvuru bekliyor 
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>

___
### <code>cosh</code>* ``cos(10) -> -0.8390715290764524``bir kosinüs değeri hesaplar 
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir değerin hiperbolik kosinüsünü hesaplar * ``cosh(0) -> 1.0``
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin CRC32 karmasını hesaplar. Bir satır parmak izini hesaplamak için 
___
### <code>currentDate</code>* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L``kullanılabilir 
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Bu işin çalışmaya başladığı geçerli tarihi alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``currentDate() == toDate('2250-12-31') -> false``
* ``currentDate('PST')  == toDate('2250-12-31') -> false``
* ``currentDate('America/New_York')  == toDate('2250-12-31') -> false``
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
İş * ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false``
___
### <code>currentUTC</code>yerel saat dilimi ile çalışmaya başladığında geçerli zaman damgasını alır 
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Geçerli zaman damgasını UTC olarak alır. Geçerli saatin, küme saat diliminizdeki farklı bir saat diliminde yorumlanması istiyorsanız, isteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Varsayılan olarak geçerli saat dilimine ayarlanır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.htmlTo UTC saatini farklı bir saat dilimine dönüştürmek, fromUTC () * ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false``
* ``currentUTC() != toTimestamp('2050-12-12 19:18:12') -> true``
* ``fromUTC(currentUTC(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>dayOfMonth</code>kullanın 
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih * ``dayOfMonth(toDate('2018-06-08')) -> 8``
___
### <code>dayOfWeek</code>verilen ayın gününü alır 
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen haftanın gününü alır. 1-Pazar, 2-Pazartesi..., 7-Cumartesi * ``dayOfWeek(toDate('2018-06-08')) -> 6``
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarih * ``dayOfYear(toDate('2016-04-09')) -> 100``
___
### <code>days</code>verilen yılın gününü alır 
<code><b>days(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Gün sayısı için milisaniye cinsinden süre * ``days(2) -> 172800000L``
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Radyana * ``degrees(3.141592653589793) -> 180``
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code> derece dönüştürür<br/><br/>
Sayı çiftini böler. 
* ``20 / 10 -> 2``
___
### <code>endsWith</code>* ``divide(20, 10) -> 2``/işleci ile aynı 
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin * ``endsWith('dumbo', 'mbo') -> true``
___
### <code>equals</code>belirtilen dize ile bitip bitmediğini denetler 
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleci. = = İşleci * ``equals(12, 24) -> false``
* ``12 == 24 -> false``
* ``'bad' == 'bad' -> true``
* ``isNull('good' == toString(null)) -> true``
* ``isNull(null == null) -> true``
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleci büyük/küçük harf yok sayılıyor. < = > İşleci * ``'abc'<=>'Abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Bir sayının faktöriyelini hesaplayın * ``factorial(5) -> 120``
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Her zaman yanlış bir değer döndürür. * ``(10 + 20 > 30) -> false``
* ``(10 + 20 > 30) -> false()``
___
### <code>filter</code>' false ' adlı bir sütun varsa işlev sözdizimini (false ()) kullanın 
<code><b>filter(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => array</b></code><br/><br/>
Öğeleri, belirtilen koşulu karşılamayan dizinin dışına filtreler. Filtre, koşul işlevindeki bir öğeye başvuru bekliyor #item * ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]``
* ``filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') -> ['a', 'b']``
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
* ``floor(-0.1) -> -1``
___
### <code>fromBase64</code>sayıdan büyük olan en büyük tamsayıyı döndürür 
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Base64 * ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``
___
### <code>fromUTC</code>verilen dizeyi kodlar 
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
UTC 'den zaman damgasına dönüştürür. İsteğe bağlı olarak, saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Kullanılabilir biçimler için geçerli timezoneRefer Java 'nın SimpleDateFormat öğesine varsayılan olarak ayarlanır. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük işleci. > İşleci ile aynı * ``greater(12, 24) -> false``
* ``('dumbo' > 'dum') -> true``
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük veya eşit işleç. > = İşleci * ``greaterOrEqual(12, 12) -> true``
* ``('dumbo' >= 'dum') -> true``
___
### <code>greatest</code>ile aynı 
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Giriş null değerlerini atlayarak değer listesi arasındaki en büyük değeri döndürür. Tüm girişler null ise null değerini döndürür * ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(10, toInteger(null), 20) -> 20``
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></code><br/><br/>
Akıştaki ada göre bir sütun değeri olup olmadığını denetler. İkinci bağımsız değişken olarak isteğe bağlı bir akış adı geçirebilirsiniz.  Tasarım zamanında bilinen sütun adları yalnızca adına göre değinmelidir. Hesaplanan girişler desteklenmez, ancak 
___
### <code>hour</code>* ``hasColumn('parent')``parametre değiştirmeler kullanabilirsiniz 
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir zaman damgasının saat değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>

___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code> * ``hours(2) -> 7200000L``saat sayısı için milisaniye cinsinden süre<br/><br/>
Bir koşula bağlı olarak bir değer veya diğerini uygular. Diğeri belirtilmemişse NULL kabul edilir. Her iki değer de uyumlu olmalıdır (sayısal, dize...) * ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></code><br/><br/>
Değerin NULL olup olmadığını denetler ve diğerini döndürür. * ``iifNull(10, 20) -> 10``
* ``iifNull(null, 20, 40) -> 20``
* ``iifNull('bojjus', 'bo', 'dumbo') -> 'dumbo'``
___
### <code>in</code>olan ilk null olmayan değeri bulana kadar tüm girişleri sınar 
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Bir öğenin dizi * ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
___
### <code>initCap</code>olup olmadığını denetler 
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Her sözcüğün ilk harfini büyük harfe dönüştürür. Sözcükler, boşluk * ``initCap('cool iceCREAM') -> 'Cool Icecream'``
___
### <code>instr</code>tarafından ayrılmış şekilde tanımlanır 
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. 0 
* ``instr('good', 'bad') -> 0``
___
### <code>isDelete</code>
* ``instr('microsoft', 'o') -> 5``* ``instr('dumbo', 'mbo') -> 3``bulunmazsa 0 döndürülür 
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın silme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isDelete()``
* ``isDelete(1)``
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın hata olarak işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isError()``
* ``isError(1)``
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın yok sayılacak şekilde işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isIgnore()``
* ``isIgnore(1)``
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın ekleme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isInsert()``
* ``isInsert(1)``
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Arama sırasında satırın eşleşip eşleşmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isMatch()``
* ``isMatch(1)``
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Değerin NULL olup olmadığını denetler * ``isNull(NULL()) -> true``
* ``isNull('') -> false``
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın güncelleştirme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isUpdate()``
* ``isUpdate(1)``
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın ekleme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini 1 veya 2 olmalıdır ve varsayılan değer 1 * ``isUpsert()``
* ``isUpsert(1)``
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Tarih * ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``
___
### <code>least</code>verilen ayın son tarihini alır 
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci * ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``
___
### <code>left</code>ile aynı 
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Dizin 1 ' den karakter sayısı ile başlayan bir alt dize ayıklar. SUBSTRING (Str, 1, n) * ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
___
### <code>length</code>ile aynı 
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
* ``length('dumbo') -> 5``
___
### <code>lesser</code>dize uzunluğunu döndürür 
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha az işleci. < İşleci ile aynı * ``lesser(12, 24) -> true``
* ``('abcd' < 'abc') -> false``
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci * ``lesserOrEqual(12, 12) -> true``
* ``('dumbo' <= 'dum') -> false``
___
### <code>levenshtein</code>ile aynı 
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
* ``levenshtein('boys', 'girls') -> 4``iki dize arasındaki mesafeyi alır 
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Bu, tam anlamıyla eşleşen bir dizedir. Özel durumlar aşağıdaki özel sembollerdir: _, girişte bir karakter ile eşleşir (şuna benzer. POSIX normal ifadelerinde)%, girişte sıfır veya daha fazla karakterle eşleşir (POSIX normal ifadelerinde. * ile benzerdir).
Kaçış karakteri ' '. Bir kaçış karakteri özel bir sembolden veya başka bir kaçış karakteriyle önceyse, aşağıdaki karakter tam anlamıyla eşleştirilir. Başka bir karakter kaçış geçersizdir.
* ``like('icecream', 'ice%') -> true``
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Belirli bir konumu Başlatan bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. Konum atlanırsa, dizenin başından hesaba göre değerlendirilir. 0 
* ``locate('bad', 'good') -> 0``
___
### <code>log</code>
* ``locate('o', 'microsoft', 6) -> 7``* ``locate('mbo', 'dumbo') -> 3``bulunmazsa 0 döndürülür 
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Günlük değerini hesaplar. İsteğe bağlı bir temel, * ``log(100, 10) -> 2``
___
### <code>log10</code>kullanılıyorsa bir Euler numarası sağlanabilir 
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Günlük değerini 10 temel * ``log10(100) -> 2``
___
### <code>lower</code>göre hesaplar 
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Küçük harf, bir dize * ``lower('GunChus') -> 'gunchus'``
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Belirli bir uzunluğa ulaşana kadar dizeyi sağlanan doldurmaya göre aşağı doğru doldurma. Dize uzunluğuna eşitse veya daha büyükse, * ``lpad('dumbo', 10, '-') -> '-----dumbo'``
* ``lpad('dumbo', 4, '-') -> 'dumb'``
* ' ' Lpad (' Dumbo ', 8, ' < > ')-> ' < ><dumbo'``
___
### <code>LTrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Sol karakterlerin bir dizesini sola kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, * ``ltrim('  dumbo  ') -> 'dumbo  '``
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``
___
### <code>map</code>ikinci parametresinde belirtilen tüm karakterleri kırpar 
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Dizideki her öğeyi, belirtilen ifadeyi kullanarak yeni bir öğeye eşler. Eşleme, #item * ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``
___
### <code>mapIndex</code>olarak ifade işlevindeki bir öğeye başvuru bekliyor 
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/>
Dizideki her öğeyi, belirtilen ifadeyi kullanarak yeni bir öğeye eşler. Map, #item ifade işlevindeki bir öğeye bir başvuru ve #index * ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``
___
### <code>md5</code>olarak öğe dizinine başvuru bekliyor 
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin MD5 özetini hesaplar ve 32 karakter onaltılı dize döndürür. Bir satır parmak izini hesaplamak için 
___
### <code>millisecond</code>* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``kullanılabilir 
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin milisaniyelik değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Milisaniye cinsinden * ``milliseconds(2) -> 2L``
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code> süresi (milisaniye)<br/><br/>
Sayıları çıkartır. Gün sayısından çıkar. Zaman damgasından alt katman süresi. Milisaniye cinsinden farkı almak için iki zaman damgasını alt katman. -İşleç * ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir zaman damgasının dakika değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
* ``minutes(2) -> 120000L``
___
### <code>mod</code>dakika sayısı için milisaniye cinsinden süre 
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftinin mod sayısı. % İşleci ile aynı 
* ``20 % 8 -> 4``
___
### <code>month</code>* ``mod(20, 8) -> 4``
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarihin veya zaman damgasının ay değerini alır * ``month(toDate('2012-8-8')) -> 8``
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></code><br/><br/>
İki tarih arasındaki ay sayısını alır. Hesaplamayı yuvarlayaseçebilirsiniz. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftini çarpar. \* İşleci ile aynı 
* ``20 * 10 -> 200``
___
### <code>negate</code>* ``multiply(20, 10) -> 200``
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayıyı geçersiz kılar. Pozitif sayıları negatif olarak döndürür ve tam tersi * ``negate(13) -> -13``
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Sonraki benzersiz diziyi döndürür. Numara yalnızca bir bölüm içinde ardışık ve PartitionID * ``nextSequence() == 12313112 -> false``
___
### <code>normalize</code>önekli 
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``
___
### <code>not</code>aksanlı Unicode karakterlerini ayırmak için dize değerini normalleştirin 
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal Değilleme İşleci * ``not(true) -> false``
* ``not(10 == 20) -> true``
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşit değildir işleci. ! = İşleci * ``12 != 24 -> true``
* ``'bojjus' != 'bo' + 'jjus' -> false``
___
### <code>notNull</code>ile aynı 
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Değerin NULL * ``notNull(NULL()) -> false``
* ``notNull('') -> true``
___
### <code>null</code>olup olmadığını denetler 
<code><b>null() => null</b></code><br/><br/>
NULL değeri döndürür. ' Null ' adlı bir sütun varsa işlev sözdizimini (null ()) kullanın. Tarafından kullanılan tüm işlemler NULL * ``isNull('dumbo' + null) -> true``
* ``isNull(10 * null) -> true``
* ``isNull('') -> false``
* ``isNull(10 + 20) -> false``
* ``isNull(10/0) -> true``
___
### <code>or</code>olur 
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal VEYA operatörüdür. Aynı | | * ``or(true, false) -> true``
* ``true || false -> true``
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftinin pozitif mod sayısı.
* ``pmod(-20, 8) -> 4``
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Giriş satırının * ``partitionId()``
___
### <code>power</code>bulunduğu geçerli bölüm kimliğini döndürür 
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir sayıyı başka bir * ``power(10, 2) -> 100``
___
### <code>reduce</code>kuvvetine yükseltir 
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Öğeleri bir dizide biriktirir. Azaltma, #acc ve #item gibi ilk ifade işlevindeki bir Biriktiricinin ve bir öğenin bir başvurusunu bekler ve * ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``
___
### <code>regexExtract</code>ikinci ifade işlevinde #result olarak sonuçta elde edilen değeri bekler 
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Verili bir Regex deseninin eşleşen alt dizesini ayıklayın. Son parametre, eşleşme grubunu tanımlar ve atlanırsa varsayılan olarak 1 ' e ayarlanır. Kaçış * ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``
___
### <code>regexMatch</code>olmadan bir dizeyle eşleştirmek için '<regex>' (arka tırnak işareti) kullanın 
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin verilen Regex düzeniyle eşleşip eşleşmediğini denetler. Kaçış * ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``
___
### <code>regexReplace</code>olmadan bir dizeyle eşleştirmek için '<regex>' (arka tırnak işareti) kullanın 
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bir Regex deseninin tüm oluşumlarını verilen dizedeki başka bir alt dizeyle Değiştir, kaçış * ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``
___
### <code>regexSplit</code>olmadan bir dizeyle eşleştirmek için '<regex>' (arka tırnak) kullanın 
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Bir dizeyi Regex temelinde bir sınırlayıcı temelinde böler ve 
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``
___
### <code>replace</code>* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``bir dize dizisi döndürür 
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></code><br/><br/>
Bir alt dizenin tüm tekrarlamalarını verilen dizedeki başka bir alt dize ile değiştirin. Son parametre atlanırsa, 
* ``replace('doggie dog', 'dog', '') -> 'gie '``
* ``replace('doggie dog', 'dog') -> 'gie '``
___
### <code>reverse</code>* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``dize boş değer olarak ayarlanır 
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bir dizeyi * ``reverse('gunchus') -> 'suhcnug'``
___
### <code>right</code>tersine çevirir 
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Sağdan karakter sayısıyla bir alt dize ayıklar. Alt DIZE (Str, LENGTH (str)-n, n) * ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``
___
### <code>rlike</code>ile aynı 
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin * ``rlike('200.50', `(\d+).(\d+)`) -> true``
* ``rlike('bogus', `M[0-9]+.*`) -> false``
___
### <code>round</code>belirtilen Regex düzeniyle eşleşip eşleşmediğini denetler 
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Bir sayıyı isteğe bağlı bir ölçek ve isteğe bağlı bir yuvarlama modu olarak yuvarlar. Ölçek atlanırsa, varsayılan olarak 0 olur.  Mod atlanırsa, varsayılan olarak ROUND_HALF_UP (5) olarak ayarlanır. Yuvarlama değerleri şunlardır 1-ROUND_UP 2-ROUND_DOWN 3-ROUND_CEILING 4-ROUND_FLOOR 5-ROUND_HALF_UP 6-ROUND_HALF_DOWN 7-ROUND_HALF_EVEN 8-ROUND_UNNECESSARY * ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Dizeyi, belirli bir uzunluğa ulaşana kadar sağlanan doldurmaya göre sağ Altlar. Dize uzunluğuna eşit veya daha büyük ise, 
* ``rpad('dumbo', 4, '-') -> 'dumb'``
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'``
___
### <code>rtrim</code>RTrim</code>* ``rpad('dumbo', 10, '-') -> 'dumbo-----'``uzunluğa atılır 
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Baştaki karakterlerden oluşan dizeyi sağ kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, * ``rtrim('  dumbo  ') -> '  dumbo'``
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``
___
### <code>second</code>ikinci parametresinde belirtilen tüm karakterleri kırpar 
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin ikinci değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Milisaniye cinsinden saniye sayısı * ``seconds(2) -> 2000L``
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin SHA-1 özetini hesaplar ve 40 karakter onaltılı dize döndürür. Bir satır parmak izini hesaplamak için 
___
### <code>sha2</code>* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``kullanılabilir 
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin SHA-2 özetini hesaplar. Bir satır parmak izini hesaplamak için 
___
### <code>sin</code>* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``kullanılabilir 
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sinüs değeri * ``sin(2) -> 0.9092974268256817``
___
### <code>sinh</code>hesaplar 
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>

___
### <code>slice</code>* ``sinh(0) -> 0.0``hiperbolik sinüs değerini hesaplar 
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Bir dizinin alt kümesini bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, 
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``
___
### <code>sort</code>* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``dize sonuna varsayılan olarak ayarlanır 
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/>
Belirtilen koşul işlevini kullanarak diziyi sıralar. Sıralama, ifade işlevindeki art arda iki öğeye başvuru bekliyor #item1 ve #item2 * ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
* ``soundex('genius') -> 'G520'``
___
### <code>split</code>dize için soundex kodunu alır 
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Bir dizeyi sınırlayıcıya göre böler ve dizeler dizisi döndürür * ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının kare kökünü 
___
### <code>startsWith</code>* ``sqrt(9) -> 3``hesaplar 
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin * ``startsWith('dumbo', 'du') -> true``
___
### <code>subDays</code>belirtilen dize ile başlayacağını denetler 
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bir tarih veya zaman damgasından ay çıkarın. Tarih * ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``
___
### <code>subMonths</code>için-işleci ile aynı 
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Tarih veya zaman damgasından ay çıkart * ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Belirli bir uzunluktaki alt dizeyi bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, 
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
___
### <code>tan</code>* ``substring('Cat in the hat', 5, 2) -> 'in'``dize sonuna varsayılan olarak ayarlanır 
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>

___
### <code>tanh</code>* ``tan(0) -> 0.0``bir teğet değeri hesaplar 
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>

___
### <code>toBase64</code>* ``tanh(0) -> 0.0``hiperbolik tanjant değerini hesaplar 
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Base64 * ``toBase64('bojjus') -> 'Ym9qanVz'``
___
### <code>toBinary</code>verilen dizeyi kodlar 
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></code><br/><br/>
Sayısal/Tarih/zaman damgası/dizeyi ikili gösterimine * ``toBinary(3) -> [0x11]``
___
### <code>toBoolean</code>dönüştürür 
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Değeri ('t ', ' true ', ' y ', ' Yes ', ' 1 ') true ve (' f ', ' false ', ' n ', ' No ', ' 0 ') değerini true ve NULL değerine, diğer herhangi bir değer * ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``isNull(toBoolean('truthy')) -> true``
___
### <code>toDate</code>dönüştürür 
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
İsteğe bağlı bir giriş tarihi biçimi kullanarak giriş tarihi dizesini tarihe dönüştürür. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. Giriş tarihi biçimi atlanırsa varsayılan biçim yyyy-[M] M-[d] d biçimindedir. Kabul edilen biçimler şunlardır: [yyyy, yyyy-[M] M, yyyy-[M] M-[d] d, yyyy-[M] M-[d] dT *] * ``toDate('2012-8-18') -> toDate('2012-08-18')``
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi ondalık bir değere dönüştürür. Duyarlık ve ölçek belirtilmemişse, varsayılan olarak ayarlanır (10, 2). Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN * ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``
___
### <code>toDouble</code>gibi isteğe bağlı bir yerel ayar biçimi 
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir Double değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN * ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``
___
### <code>toFloat</code>gibi isteğe bağlı bir yerel ayar biçimi 
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir float değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Herhangi bir çift * ``toFloat(123.45) -> 123.45f``
* ``toFloat('123.45') -> 123.45f``
* ``toFloat('$123.45', '$###.00') -> 123.45f``
___
### <code>toInteger</code>keser 
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir tamsayı değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Long, float, Double * ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``
___
### <code>toLong</code>keser 
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi uzun bir değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Herhangi bir float, Double * ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir kısa değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Herhangi bir tamsayı, uzun, kayan, Çift * ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``
___
### <code>toString</code>keser 
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
İlkel bir veri türünü dizeye dönüştürür. Sayı ve tarih için bir biçim belirtilebilir. Belirtilmezse, sistem varsayılanı seçilir. Sayılar için Java Decimal biçimi kullanılır. Olası tüm tarih biçimleri için Java SimpleDateFormat öğesine bakın; Varsayılan biçim yyyy-aa-gg * ``toString(10) -> '10'``
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
Bir dizeyi, isteğe bağlı bir zaman damgası biçimi verilen bir zaman damgasına dönüştürür. Tüm olası biçimler için Java SimpleDateFormat bölümüne bakın. Zaman damgası varsayılan düzende yoksayılırsa. yyyy-[M] M-[d] d hh: mm: SS [. f...] kullanılır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Zaman damgası, kullanılabilir biçimler için Java 'nın SimpleDateFormat değerini Ifade eden 999'A göre daha fazla milisaniyelik doğruluğu destekler. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Zaman damgasını UTC 'ye dönüştürür. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Kullanılabilir biçimler için geçerli timezoneRefer Java 'nın SimpleDateFormat öğesine varsayılan olarak ayarlanır. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Bir karakter kümesini dizedeki başka bir karakter kümesiyle değiştirin. Karakterler, 1 ile 1 arasında değiştirme * ``translate('(bojjus)', '()', '[]') -> '[bojjus]'``
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Baştaki ve sondaki karakterlerden oluşan dizeyi kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, * ``trim('  dumbo  ') -> 'dumbo'``
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``
___
### <code>true</code>ikinci parametresinde belirtilen tüm karakterleri kırpar 
<code><b>true() => boolean</b></code><br/><br/>
Her zaman doğru bir değer döndürür. ' True ' * ``(10 + 20 == 30) -> true``
* ``(10 + 20 == 30) -> true()``
___
### <code>typeMatch</code>adlı bir sütun varsa, işlev sözdizimini (true ()) kullanın 
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Sütunun türüyle eşleşir. Yalnızca desenli ifadelerde kullanılabilir. sayı Short, Integer, Long, Double, float veya Decimal ile eşleşir, tamsayısı Short, Integer, Long, kesir, Double, float, Decimal ve DateTime ile eşleşen tarih veya zaman damgası türü * ``typeMatch(type, 'number')``
* ``typeMatch('date', 'datetime')``
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Büyük/veya bir dize * ``upper('bojjus') -> 'BOJJUS'``
___
### <code>uuid</code>
<code><b>uuid() => string</b></code><br/><br/>
Oluşturulan UUID * ``uuid()``
___
### <code>weekOfYear</code>döndürür 
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarih * ``weekOfYear(toDate('2008-02-20')) -> 8``
___
### <code>weeks</code>verilen yılın haftasını alır 
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
Hafta sayısı için milisaniye cinsinden süre * ``weeks(2) -> 1209600000L``
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal XOR işleci. ^ İşleci * ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
___
### <code>year</code>ile aynı 
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih * ``year(toDate('2012-8-8')) -> 2012``tarihin yıl değerini alır 
## toplama işlevleri aşağıdaki işlevler yalnızca toplama, pivot, UNPIVOT ve pencere dönüştürmeleri ___
### <code>avg</code>kullanılabilir 
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalamasını alır * ``avg(sales)``
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere göre * ``avgIf(region == 'West', sales)``
___
### <code>count</code>bir sütunun ortalama değerlerini alır 
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Değerlerin toplam sayısını alır. İsteğe bağlı sütunlar belirtilmişse, Count * ``count(custId)``
* ``count(custId, custName)``
* ``count()``
* ``count(iif(isNull(custId), 1, NULL))``
___
### <code>countDistinct</code>NULL değerlerini yoksayar 
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
* ``countDistinct(custId, custName)``bir sütun kümesinin farklı değerlerinin toplam sayısını alır 
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Ölçütlere göre toplam değer sayısını alır. İsteğe bağlı sütun belirtilmişse, sayı * ``countIf(state == 'CA' && commission < 10000, name)``
___
### <code>covariancePopulation</code>NULL değerleri yoksayar 
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütun arasındaki popülasyon kovaryansını alır * ``covariancePopulation(sales, profit)``
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``covariancePopulationIf(region == 'West', sales)``
___
### <code>covarianceSample</code>iki sütunun popülasyon kovaryansını alır 
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütunun örnek kovaryansını alır * ``covarianceSample(sales, profit)``
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``covarianceSampleIf(region == 'West', sales, profit)``
___
### <code>first</code>iki sütunun örnek kovaryansını alır 
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bir sütun grubunun ilk değerini alır. IgnoreNulls ikinci parametresi atlanırsa, yanlış * ``first(sales)``
* ``first(sales, false)``
___
### <code>kurtosis</code>kabul edilir 
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun basıklığını alır * ``kurtosis(sales)``
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``kurtosisIf(region == 'West', sales)``
___
### <code>last</code>bir sütunun basıklığını alır 
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bir sütun grubunun son değerini alır. IgnoreNulls ikinci parametresi atlanırsa, yanlış * ``last(sales)``
* ``last(sales, false)``
___
### <code>max</code>kabul edilir 
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
* ``max(sales)``
___
### <code>maxIf</code>bir sütunun en büyük değerini alır 
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Ölçütlere göre * ``maxIf(region == 'West', sales)``
___
### <code>mean</code>bir sütunun en büyük değerini alır 
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalama değerini alır. AVG * ``mean(sales)``
___
### <code>meanIf</code>ile aynı 
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak bir sütunun değerlerinin ortalaması alınır. AvgIf * ``meanIf(region == 'West', sales)``
___
### <code>min</code>ile aynı 
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bir sütunun en küçük değerini alır * ``min(sales)``
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Ölçütlere göre * ``minIf(region == 'West', sales)``
___
### <code>skewness</code>bir sütunun en küçük değerini alır 
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun 
___
### <code>skewnessIf</code>* ``skewness(sales)``çarpıklığını alır 
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``skewnessIf(region == 'West', sales)``
___
### <code>stddev</code>bir sütunun çarpıklığını alır 
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``stdDev(sales)``
___
### <code>stddevIf</code>bir sütunun standart sapmasını alır 
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``stddevIf(region == 'West', sales)``
___
### <code>stddevPopulation</code>bir sütunun standart sapmasını alır 
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``stddevPopulation(sales)``
___
### <code>stddevPopulationIf</code>bir sütunun popülasyon standart sapmasını alır 
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``stddevPopulationIf(region == 'West', sales)``
___
### <code>stddevSample</code>bir sütunun popülasyon standart sapmasını alır 
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``stddevSample(sales)``
___
### <code>stddevSampleIf</code>bir sütunun örnek standart sapmasını alır 
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``stddevSampleIf(region == 'West', sales)``
___
### <code>sum</code>bir sütunun örnek standart sapmasını alır 
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
* ``sum(col)``
___
### <code>sumDistinct</code>sayısal bir sütunun toplam toplamını alır 
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayısal bir sütunun benzersiz değerlerinin toplam toplamını alır * ``sumDistinct(col)``
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul, * ``sumDistinctIf(state == 'CA' && commission < 10000, sales)``
* ``sumDistinctIf(true, sales)``
___
### <code>sumIf</code>herhangi bir sütunu temel alabilir 
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul, * ``sumIf(state == 'CA' && commission < 10000, sales)``
* ``sumIf(true, sales)``
___
### <code>variance</code>herhangi bir sütunu temel alabilir 
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun * ``variance(sales)``
___
### <code>varianceIf</code>farkını alır 
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``varianceIf(region == 'West', sales)``
___
### <code>variancePopulation</code>bir sütunun farkını alır 
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``variancePopulation(sales)``
___
### <code>variancePopulationIf</code>bir sütunun popülasyon varyansını alır 
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre * ``variancePopulationIf(region == 'West', sales)``
___
### <code>varianceSample</code>bir sütunun popülasyon varyansını alır 
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
* ``varianceSample(sales)``bir sütunun taraflı olmayan varyansını alır 
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere dayalı olarak, bir sütunun taraflı olmayan varyansını alır * ``varianceSampleIf(region == 'West', sales)``
## pencere işlevleri aşağıdaki işlevler yalnızca pencere dönüşümlerinde ___
### <code>cumeDist</code>kullanılabilir 
<code><b>cumeDist() => integer</b></code><br/><br/>
CumeDist işlevi, bir değerin bölüm içindeki tüm değerlere göre konumunu hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satıra göre veya buna eşit olan satır sayısıdır ve pencere bölümündeki toplam satır sayısı ile ayrılır. Sıralamada bulunan tüm bağlama değerleri aynı konuma göre değerlendirilir.
* ``cumeDist()``
___
### <code>denseRank</code>
<code><b>denseRank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk üretmeyecektir. Yoğun sıralama, veriler sıralanmasa bile ve * ``denseRank(salesQtr, salesAmt)``
___
### <code>lag</code>verilerde değişiklik ararken bile kullanılır 
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Geçerli satırdan önce değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, geri aranacak satır sayısıdır ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa, varsayılan bir değer belirtilmediği sürece null değeri döndürülür 
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code> * ``lag(amount, 2)``
* ``lag(amount, 2000, 100)``
___
### <code>lead</code><br/><br/>
Geçerli satırdan sonra değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, görüntülenecek satır sayısı ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa, varsayılan bir değer belirtilmediği sürece null değeri döndürülür 
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code> * ``lead(amount, 2)``
* ``lead(amount, 2000, 100)``
___
### <code>nTile</code><br/><br/>
NTile işlevi her pencere bölümü için satırları 1 ' den en çok `n`değişen `n` demetlere böler. Demet değerleri en fazla 1 farklı olacaktır. Bölümdeki satır sayısı demet sayısına eşit olarak bölünmezse, ilk sepete başlayarak, geri kalan değerler her demet için bir dağıtılır. NTile işlevi, terkutucukların, kubotların ve diğer yaygın Özet istatistiğin hesaplanması için yararlıdır. İşlev, başlatma sırasında iki değişkeni hesaplar: düzenli bir demet boyutunun boyutuna eklenmiş bir ek satır olacaktır. Her iki değişken de geçerli bölümün boyutunu temel alır. Hesaplama işlemi sırasında işlev geçerli satır numarasını, geçerli demet numarasını ve demet 'in değiştirileceği satır numarasını (bucketThreshold) izler. Geçerli satır numarası demet eşiğine ulaştığında, demet değeri bir artırılır ve eşik, demet boyutu (ve geçerli demet doldurulmuş ise bir ek) ile artırılır.
* ``nTile()``
* ``nTile(numOfBuckets)``
___
### <code>rank</code>
<code><b>rank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk oluşturur. Sıralama, veriler sıralanmasa bile ve * ``rank(salesQtr, salesAmt)``
___
### <code>rowNumber</code>verilerde değişiklik ararken kullanılır 
<code><b>rowNumber() => integer</b></code><br/><br/>
1 * ``rowNumber()`` başlayarak bir penceredeki satırlar için sıralı bir satır numaralandırması atar

## <a name="next-steps"></a>Sonraki adımlar

[Ifade oluşturucuyu nasıl kullanacağınızı öğrenin](concepts-data-flow-expression-builder.md).
