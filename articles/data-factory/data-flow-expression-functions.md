---
title: Azure Data Factory veri akışı eşleme özelliğindeki ifade işlevleri
description: Eşleme veri akışındaki ifade işlevleri hakkında bilgi edinin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 72c516f0a6e377cc16205917967482a29b4fdfbd
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "69036213"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Eşleme veri akışındaki veri dönüştürme ifadeleri 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="expression-functions"></a>İfade işlevleri

Data Factory ' de, veri dönüşümlerini yapılandırmak için veri akışını eşleme özelliğinin ifade dilini kullanın.

___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayı çiftinin pozitif mod sayısı.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
___
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Kosinüs ters değerini hesaplar* ``acos(1) -> 0.0``
___
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Bir çift dize veya sayı ekler. Gün sayısı için bir tarih ekler. Benzer türdeki bir diziyi diğerine ekler. \+ İşleciyle aynı* ``add(10, 20) -> 30``
* ``10 + 20 -> 30``
* ``add('ice', 'cream') -> 'icecream'``
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``
* ``add(toDate('2012-12-12'), 3) -> 2012-12-15 (date value)``
* ``toDate('2012-12-12') + 3 -> 2012-12-15 (date value)``
* ``[10, 20] + [30, 40] => [10, 20, 30, 40]``
___
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Tarih veya zaman damgasına gün ekleyin. Tarih için + işleciyle aynı* ``addDays(toDate('2016-08-08'), 1) -> 2016-08-09``
___
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Tarih veya zaman damgasına ay ekleme* ``addMonths(toDate('2016-08-31'), 1) -> 2016-09-30``
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> 2016-08-31 10:10:10``
___
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal AND işleci. & İle aynı &* ``and(true, false) -> false``
* ``true && false -> false``
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Ters Sinüs değerini hesaplar* ``asin(0) -> 0.0``
___
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Ters tanjant değerini hesaplar* ``atan(0) -> 0.0``
___
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Düzlemin pozitif x ekseni ile koordinatlar tarafından verilen nokta arasındaki radyan cinsinden açıyı döndürür* ``atan2(0, 0) -> 0.0``
___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalamasını alır* ``avg(sales) -> 7523420.234``
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak bir sütunun değerlerinin ortalaması alınır* ``avgIf(region == 'West', sales) -> 7523420.234``
___
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string) => any</b></code><br/><br/>
Akıştaki ada göre bir sütun değeri seçer. Birden çok eşleşme varsa, ilk eşleşme döndürülür. Eğer eşleşme yoksa NULL değer döndürür. Döndürülen değer tür dönüştürme işlevlerinden biri tarafından dönüştürülmüş olmalıdır (TO_DATE, TO_STRING...).  Tasarım zamanında bilinen sütun adları yalnızca adına göre değinmelidir. Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler kullanabilirsiniz* ``toString(byName('parent')) -> appa``
* ``toLong(byName('income')) -> 9000000000009``
* ``toBoolean(byName('foster')) -> false``
* ``toLong(byName($debtCol)) -> 123456890``
* ``birthDate -> 12/31/2050``
* ``toString(byName('Bogus Column')) -> NULL``
___
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Akışta göreli konumuna göre (1 tabanlı) bir sütun değeri seçer. Konum sınırların dışında ise, NULL bir değer döndürür. Döndürülen değer tür dönüştürme işlevlerinden biri tarafından dönüştürülmüş olmalıdır (TO_DATE, TO_STRING...) Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler kullanabilirsiniz* ``toString(byPosition(1)) -> amma``
* ``toDecimal(byPosition(2), 10, 2) -> 199990.99``
* ``toBoolean(byName(4)) -> false``
* ``toString(byName($colName)) -> family``
* ``toString(byPosition(1234)) -> NULL``
___
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Değişen koşullara göre bir değer veya diğeri geçerlidir. Giriş sayısı çiftse, diğeri son koşul için NULL olur* ``case(custType == 'Premium', 10, 4.5)``
* ``case(custType == 'Premium', price*0.95, custType == 'Elite',   price*0.9, price*2)``
* ``case(dayOfWeek(saleDate) == 1, 'Sunday', dayOfWeek(saleDate) == 6, 'Saturday')``
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının Küp kökünü hesapla* ``cbrt(8) -> 2.0``
___
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayıdan küçük olan en küçük tamsayıyı döndürür* ``ceil(-0.1) -> 0``
___
### <code>coalesce</code>
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Giriş kümesinden gelen ilk null değeri döndürür. Tüm girişler aynı türde olmalıdır* ``coalesce(10, 20) -> 10``
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``
___
### <code>compare</code>
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
Aynı türdeki iki değeri karşılaştırır. Değer1 < değer2 ise negatif tamsayı döndürür, 0 if Değer1 = = değer2 ise pozitif değer, Eğer değer1 > değer2* ``(compare(12, 24) < 1) -> true``
* ``(compare('dumbo', 'dum') > 0) -> true``
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Değişken sayıda dizeyi birlikte birleştirir. Dizelerle + işleciyle aynı* ``concat('Awesome', 'Cool', 'Product') -> 'AwesomeCoolProduct'``
* ``'Awesome' + 'Cool' + 'Product' -> 'AwesomeCoolProduct'``
* ``concat(addrLine1, ' ', addrLine2, ' ', city, ' ', state, ' ', zip)``
* ``addrLine1 + ' ' + addrLine2 + ' ' + city + ' ' + state + ' ' + zip``
___
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Bir dizi dizeyi bir ayırıcıyla birlikte birleştirir. İlk parametre ayırıcıdır* ``concatWS(' ', 'Awesome', 'Cool', 'Product') -> 'Awesome Cool Product'``
* ``concatWS(' ' , addrLine1, addrLine2, city, state, zip) ->``
* ``concatWS(',' , toString(order_total), toString(order_discount))``
___
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir kosinüs değeri hesaplar* ``cos(10) -> -0.83907152907``
___
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir değerin hiperbolik kosinüsünü hesaplar* ``cosh(0) -> 1.0``
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Değerlerin toplam sayısını alır. İsteğe bağlı sütunlar belirtilmişse, sayımla NULL değerleri yoksayar* ``count(custId) -> 100``
* ``count(custId, custName) -> 50``
* ``count() -> 125``
* ``count(iif(isNull(custId), 1, NULL)) -> 5``
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Bir sütun kümesinin farklı değerlerinin toplam sayısını alır* ``countDistinct(custId, custName) -> 60``
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Ölçütlere göre toplam değer sayısını alır. İsteğe bağlı sütun belirtilmişse, sayımla NULL değerleri yoksayar* ``countIf(state == 'CA' && commission < 10000, name) -> 100``
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütun arasındaki popülasyon kovaryansını alır* ``covariancePopulation(sales, profit) -> 122.12``
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre iki sütunun popülasyon kovaryansını alır* ``covariancePopulationIf(region == 'West', sales) -> 122.12``
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
İki sütunun örnek kovaryansını alır* ``covarianceSample(sales, profit) -> 122.12``
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, iki sütunun örnek kovaryansını alır* ``covarianceSampleIf(region == 'West', sales, profit) -> 122.12``
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin CRC32 karmasını hesaplar. Bir satır parmak izini hesaplamak için kullanılabilir* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689``
___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
CumeDist işlevi, bir değerin bölüm içindeki tüm değerlere göre konumunu hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satıra göre veya buna eşit olan satır sayısıdır ve pencere bölümündeki toplam satır sayısı ile ayrılır. Sıralamada bulunan tüm bağlama değerleri aynı konuma göre değerlendirilir.
* ``cumeDist() -> 1``
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Bu işin çalışmaya başladığı geçerli tarihi alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``currentDate() -> 12-12-2030``
* ``currentDate('PST') -> 12-31-2050``
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
İş yerel saat dilimiyle çalışmaya başladığında geçerli zaman damgasını alır* ``currentTimestamp() -> 12-12-2030T12:12:12``
___
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Geçerli zaman damgasını UTC olarak alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Varsayılan olarak geçerli saat dilimine ayarlanır* ``currentUTC() -> 12-12-2030T19:18:12``
* ``currentUTC('Asia/Seoul') -> 12-13-2030T11:18:12``
___
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarih verilen tarihin gününü alır* ``dayOfMonth(toDate('2018-06-08')) -> 08``
___
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen haftanın gününü alır. 1-Pazar, 2-Pazartesi..., 7-Cumartesi* ``dayOfWeek(toDate('2018-06-08')) -> 7``
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarih verilen yılın gününü alır* ``dayOfYear(toDate('2016-04-09')) -> 100``
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Radyana derece derece dönüştürür* ``degrees(3.141592653589793) -> 180``
___
### <code>denseRank</code>
<code><b>denseRank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk üretmeyecektir. Yoğun sıra, veriler sıralanmasa bile ve değerlerde değişiklik ararken bile çalışıyor* ``denseRank(salesQtr, salesAmt) -> 1``
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftini böler. /İşleciyle aynı* ``divide(20, 10) -> 2``
* ``20 / 10 -> 2``
___
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin belirtilen dize ile bitip bitmediğini denetler* ``endsWith('great', 'eat') -> true``
___
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleci. = = İşleci ile aynı* ``equals(12, 24) -> false``
* ``12==24 -> false``
* ``'bad'=='bad' -> true``
* ``'good'== NULL -> false``
* ``NULL===NULL -> false``
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
Karşılaştırma eşittir işleci büyük/küçük harf yok sayılıyor. < = > İşleci ile aynı* ``'abc'<==>'abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Bir sayının faktöriyelini hesapla* ``factorial(5) -> 120``
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Her zaman yanlış bir değer döndürür. ' False ' adlı bir sütun varsa işlev sözdizimini (false ()) kullanın* ``isDiscounted == false()``
* ``isDiscounted() == false``
___
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bir sütun grubunun ilk değerini alır. IgnoreNulls ikinci parametresi atlanırsa, false olarak kabul edilir* ``first(sales) -> 12233.23``
* ``first(sales, false) -> NULL``
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayıdan büyük olan en büyük tamsayıyı döndürür* ``floor(-0.1) -> -1``
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
UTC 'den zaman damgasına dönüştürür. İsteğe bağlı olarak, saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Varsayılan olarak geçerli saat dilimine ayarlanır* ``fromUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük işleci. > İşleçle aynı* ``greater(12, 24) -> false``
* ``'abcd' > 'abc' -> true``
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha büyük veya eşit işleç. > = İşleci ile aynı* ``greaterOrEqual(12, 12) -> false``
* ``'abcd' >= 'abc' -> true``
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Giriş olarak değerler listesi arasındaki en büyük değeri döndürür. Tüm girişler null ise null döndürür* ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2011'``
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir zaman damgasının saat değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``hour(toTimestamp('2009-07-30T12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30T12:58:59'), 'PST') -> 12``
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Bir koşula bağlı olarak bir değer veya diğerini uygular. Diğeri belirtilmemişse NULL kabul edilir. Her iki değer de uyumlu olmalıdır (sayısal, dize...)* ``iif(custType == 'Premium', 10, 4.5)``
* ``iif(amount > 100, 'High')``
* ``iif(dayOfWeek(saleDate) == 6, 'Weekend', 'Weekday')``
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Dizide bir öğe olup olmadığını denetler* ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Her sözcüğün ilk harfini büyük harfe dönüştürür. Sözcükler boşluk ile ayrılmış olarak tanımlanır* ``initCap('cool iceCREAM') -> 'Cool IceCREAM'``
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. 0 bulunamazsa döndürülür* ``instr('great', 'eat') -> 3``
* ``instr('microsoft', 'o') -> 7``
* ``instr('good', 'bad') -> 0``
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın silme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isDelete() -> true``
* ``isDelete(1) -> false``
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın hata olarak işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isError() -> true``
* ``isError(1) -> false``
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın yok sayılacak şekilde işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isIgnore() -> true``
* ``isIgnore(1) -> false``
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın ekleme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isInsert() -> true``
* ``isInsert(1) -> false``
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Arama sırasında satırın eşleşip eşleşmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isMatch() -> true``
* ``isMatch(1) -> false``
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Değerin NULL olup olmadığını denetler* ``isNull(NULL()) -> true``
* ``isNull('') -> false'``
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Satırın güncelleştirme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 ' dir* ``isUpdate() -> true``
* ``isUpdate(1) -> false``
___
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun basıklığını alır* ``kurtosis(sales) -> 122.12``
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun basıklığını alır* ``kurtosisIf(region == 'West', sales) -> 122.12``
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Geçerli satırdan önce değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, geri aranacak satır sayısıdır ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa varsayılan değer belirtilmediği sürece null değeri döndürülür* ``lag(amount, 2) -> 60``
* ``lag(amount, 2000, 100) -> 100``
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Bir sütun grubunun son değerini alır. IgnoreNulls ikinci parametresi atlanırsa, false olarak kabul edilir* ``last(sales) -> 523.12``
* ``last(sales, false) -> NULL``
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Tarih verilen ayın son tarihini alır* ``lastDayOfMonth(toDate('2009-01-12')) -> 2009-01-31``
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Geçerli satırdan sonra değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, görüntülenecek satır sayısı ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa varsayılan değer belirtilmediği sürece null değeri döndürülür* ``lead(amount, 2) -> 60``
* ``lead(amount, 2000, 100) -> 100``
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci ile aynı* ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2000'``
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Dizin 1 ' den karakter sayısı ile başlayan bir alt dize ayıklar. Alt DIZEYLE aynı (Str, 1, n)* ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Dizenin uzunluğunu döndürür* ``length('kiddo') -> 5``
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma daha az işleci. < İşleçle aynı* ``lesser(12 < 24) -> true``
* ``'abcd' < 'abc' -> false``
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci ile aynı* ``lesserOrEqual(12, 12) -> true``
* ``'abcd' <= 'abc' -> false``
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
İki dize arasındaki mesafeyi alır* ``levenshtein('boys', 'girls') -> 4``
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Bu, tam anlamıyla eşleşen bir dizedir. Özel durumlar aşağıdaki özel sembollerdir: _, girişte bir karakter ile eşleşir (şuna benzer. POSIX normal ifadelerinde)%, girişte sıfır veya daha fazla karakterle eşleşir (POSIX normal ifadelerinde. * ile benzerdir).
Kaçış karakteri ' '. Bir kaçış karakteri özel bir sembolden veya başka bir kaçış karakteriyle önceyse, aşağıdaki karakter tam anlamıyla eşleştirilir. Başka bir karakter kaçış geçersizdir.
* ``like('icecream', 'ice%') -> true``
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Belirli bir konumu Başlatan bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. Konum atlanırsa, dizenin başından hesaba göre değerlendirilir. 0 bulunamazsa döndürülür* ``locate('eat', 'great') -> 3``
* ``locate('o', 'microsoft', 6) -> 7``
* ``locate('bad', 'good') -> 0``
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Günlük değerini hesaplar. İsteğe bağlı bir taban, kullanıldıysa bir Euler numarası sağlanabilir* ``log(100, 10) -> 2``
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
10 tabanına göre günlük değerini hesaplar* ``log10(100) -> 2``
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Küçük harf, bir dize* ``lower('GunChus') -> 'gunchus'``
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Belirli bir uzunluğa ulaşana kadar dizeyi sağlanan doldurmaya göre aşağı doğru doldurma. Dize uzunluğuna eşit veya daha büyük ise, bu * ``lpad('great', 10, '-') -> '___--great'`` , bir-op 
* ``lpad('great', 4, '-') -> 'great'`` 
* ' ' Lpad (' harika ', 8, ' < > ')-> ' < ><great'``
___
### <code>LTrim olarak kabul edilir</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Sol karakterlerin bir dizesini sola kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, ikinci parametrede belirtilen karakterleri kırpar* ``ltrim('!--!wor!ld!', '-!') -> 'wor!ld!'``
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bir sütunun en büyük değerini alır* ``MAX(sales) -> 12312131.12``
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Ölçütlere göre, bir sütunun en büyük değerini alır* ``maxIf(region == 'West', sales) -> 99999.56``
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin MD5 özetini hesaplar ve 32 karakter onaltılı dize döndürür. Bir satır parmak izini hesaplamak için kullanılabilir* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'c1527622a922c83665e49835e46350fe'``
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sütunun değerlerinin ortalama değerini alır. AVG ile aynı* ``mean(sales) -> 7523420.234``
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak bir sütunun değerlerinin ortalaması alınır. AvgIf ile aynı* ``meanIf(region == 'West', sales) -> 7523420.234``
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin milisaniyelik değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Bir sütunun en küçük değerini alır* ``min(sales) -> 00.01``
* ``min(orderDate) -> 12/12/2000``
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Ölçütlere göre, bir sütunun en küçük değerini alır* ``minIf(region == 'West', sales) -> 00.01``
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayıları çıkartır. Gün sayısından çıkar. -İşleci ile aynı* ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> 2012-12-12 (date value)``
* ``toDate('2012-12-15') - 3 -> 2012-12-13 (date value)``
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir zaman damgasının dakika değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``minute(toTimestamp('2009-07-30T12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30T12:58:59', 'PST')) -> 58``
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftinin mod sayısı. % İşleci ile aynı* ``mod(20, 8) -> 4``
* ``20 % 8 -> 4``
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarih veya zaman damgasının ay değerini alır* ``month(toDate('2012-8-8')) -> 8``
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;time zone&gt;</i> : boolean], [<i>&lt;value4&gt;</i> : string]) => double</b></code><br/><br/>
İki tarih arasındaki ay sayısını alır ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde isteğe bağlı bir saat dilimi geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``monthsBetween(toDate('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftini çarpar. \* İşleciyle aynı* ``multiply(20, 10) -> 200``
* ``20 * 10 -> 200``
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
Ntile işlevi, her pencere bölümünün `n` satırlarını 1 ile en çok `n`arasında olacak demetlere böler. Demet değerleri en fazla 1 farklı olacaktır. Bölümdeki satır sayısı demet sayısına eşit olarak bölünmezse, ilk sepete başlayarak, geri kalan değerler her demet için bir dağıtılır. NTile işlevi, terkutucukların, kubotların ve diğer yaygın Özet istatistiğin hesaplanması için yararlıdır. İşlevi başlatma sırasında iki değişkeni hesaplar: Normal bir demet boyutunun, kendisine eklenmiş bir ek satırı olacaktır. Her iki değişken de geçerli bölümün boyutunu temel alır. Hesaplama işlemi sırasında işlev geçerli satır numarasını, geçerli demet numarasını ve demet 'in değiştirileceği satır numarasını (bucketThreshold) izler. Geçerli satır numarası demet eşiğine ulaştığında, demet değeri bir artırılır ve eşik, demet boyutu (ve geçerli demet doldurulmuş ise bir ek) ile artırılır.
* ``nTile() -> 1``
* ``nTile(numOfBuckets) -> 1``
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayıyı geçersiz kılar. Pozitif sayıları negatif ve tam tersi yönde döndürür* ``negate(13) -> -13``
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Sonraki benzersiz diziyi döndürür. Numara yalnızca bir bölüm içinde ardışık ve PartitionID tarafından önekli* ``nextSequence() -> 12313112``
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
Dize değerini, aksanlı Unicode karakterlerini ayırmak için normalleştirin* ``normalize('boys') -> 'boys'``
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal Değilleme İşleci* ``not(true) -> false``
* ``not(premium)``
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Karşılaştırma eşit değildir işleci. ! = İşleci ile aynı* ``12!=24 -> true``
* ``'abc'!='abc' -> false``
___
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
NULL değeri döndürür. ' Null ' adlı bir sütun varsa işlev sözdizimini (null ()) kullanın. Tarafından kullanılan tüm işlemler NULL ile sonuçlanır* ``custId = NULL (for derived field)``
* ``custId == NULL -> NULL``
* ``'nothing' + NULL -> NULL``
* ``10 * NULL -> NULL'``
* ``NULL == '' -> NULL'``
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal OR işleci. Aynı | |* ``or(true, false) -> true``
* ``true || false -> true``
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Sayı çiftinin pozitif mod sayısı.
* ``pmod(-20, 8) -> 4``
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Bir sayıyı diğerinin kuvvetine yükseltir* ``power(10, 2) -> 100``
___
### <code>rank</code>
<code><b>rank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk oluşturur. Sıralama, veriler sıralandığında ve değerlerde değişiklik ararken bile kullanılır* ``rank(salesQtr, salesAmt) -> 1``
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Verili bir Regex deseninin eşleşen alt dizesini ayıklayın. Son parametre, eşleşme grubunu tanımlar ve atlanırsa varsayılan olarak 1 ' e ayarlanır. Kaçış olmadan<regex>bir dizeyle eşleştirmek için ' ' (Back quote) kullanın* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin verilen Regex düzeniyle eşleşip eşleşmediğini denetler. Kaçış olmadan<regex>bir dizeyle eşleştirmek için ' ' (Back quote) kullanın* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bir Regex deseninin tüm yinelemelerini, belirtilen dizedeki başka bir alt dizeyle Değiştir kaçış olmadan bir<regex>dizeyle eşleştirmek için ' ' (arka quote) kullanın* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Bir dizeyi Regex temelinde bir sınırlayıcı temelinde böler ve dizeler dizisini döndürür* ``regexSplit('oneAtwoBthreeC', '[CAB]') -> ['one', 'two', 'three']``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[1] -> 'one'``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[0] -> NULL``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[20] -> NULL``
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Bir alt dizenin tüm oluşumlarını verilen dizedeki başka bir alt dizeyle Değiştir* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``
* ``replace('doggie dog', 'dog', '') -> 'gie'``
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Bir dizeyi tersine çevirir* ``reverse('gunchus') -> 'suhcnug'``
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Sağdan karakter sayısıyla bir alt dize ayıklar. Alt DIZEDEN aynı (Str, LENGTH (str)-n, n)* ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin verilen Regex düzeniyle eşleşip eşleşmediğini denetler* ``rlike('200.50', '(\d+).(\d+)') -> true``
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Bir sayıyı isteğe bağlı bir ölçek ve isteğe bağlı bir yuvarlama modu olarak yuvarlar. Ölçek atlanırsa, varsayılan olarak 0 olur.  Mod atlanırsa, varsayılan olarak ROUND_HALF_UP (5) olarak ayarlanır. Yuvarlama değerleri şunlardır 1-ROUND_UP 2-ROUND_DOWN 3-ROUND_CEILING 4-ROUND_FLOOR 5-ROUND_HALF_UP 6-ROUND_HALF_DOWN 7-ROUND_HALF_EVEN 8-ROUND_UNNECESSARY* ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
1 ile başlayan bir penceredeki satırlar için sıralı bir satır numaralandırması atar* ``rowNumber() -> 1``
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Dizeyi, belirli bir uzunluğa ulaşana kadar sağlanan doldurmaya göre sağ Altlar. Dize uzunluğuna eşit veya daha büyük ise * ``rpad('great', 10, '-') -> 'great___--'``, bu, işlem 
* ``rpad('great', 4, '-') -> 'great'`` 
* ``rpad('great', 8, '<>') -> 'great<><'`` 
___
### <code>rtrim</code>dışı bir RTrim olarak değerlendirilir</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Baştaki karakterlerden oluşan dizeyi sağ kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, ikinci parametrede belirtilen karakterleri kırpar* ``rtrim('!--!wor!ld!', '-!') -> '!--!wor!ld'``
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Bir tarihin ikinci değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır.
* ``second(toTimestamp('2009-07-30T12:58:59')) -> 59``
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin SHA-1 özetini hesaplar ve 40 karakter onaltılı dize döndürür. Bir satır parmak izini hesaplamak için kullanılabilir* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '63849fd2abb65fbc626c60b1f827bd05573f0cea'``
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin SHA-2 özetini hesaplar. Bir satır parmak izini hesaplamak için kullanılabilir* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'd3b2bff62c3a00e9370b1ac85e428e661a7df73959fa1a96ae136599e9ee20fd'``
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sinüs değeri hesaplar* ``sin(2) -> 0.90929742682``
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiperbolik sinüs değerini hesaplar* ``sinh(0) -> 0.0``
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun çarpıklığını alır* ``skewness(sales) -> 122.12``
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun çarpıklığını alır* ``skewnessIf(region == 'West', sales) -> 122.12``
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Bir dizinin alt kümesini bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, varsayılan dize sonuna ayarlanır* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``slice([10, 20, 30, 40], 2)[0] -> NULL``
* ``slice([10, 20, 30, 40], 2)[20] -> NULL``
* ``slice([10, 20, 30, 40], 8) -> []``
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Dize için soundex kodunu alır* ``soundex('genius') -> 'G520'``
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Bir dizeyi sınırlayıcıya göre böler ve dizeler dizisini döndürür* ``split('100,200,300', ',') -> ['100', '200', '300']``
* ``split('100,200,300', '|') -> ['100,200,300']``
* ``split('100, 200, 300', ', ') -> ['100', '200', '300']``
* ``split('100, 200, 300', ', ')[1] -> '100'``
* ``split('100, 200, 300', ', ')[0] -> NULL``
* ``split('100, 200, 300', ', ')[20] -> NULL``
* ``split('100200300', ',') -> ['100200300']``
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sayının kare kökünü hesaplar* ``sqrt(9) -> 3``
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Dizenin sağlanan dizeyle başlayacağını denetler* ``startsWith('great', 'gr') -> true``
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun standart sapmasını alır* ``stdDev(sales) -> 122.12``
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun standart sapmasını alır* ``stddevIf(region == 'West', sales) -> 122.12``
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun popülasyon standart sapmasını alır* ``stddevPopulation(sales) -> 122.12``
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun popülasyon standart sapmasını alır* ``stddevPopulationIf(region == 'West', sales) -> 122.12``
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun örnek standart sapmasını alır* ``stddevSample(sales) -> 122.12``
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun örnek standart sapmasını alır* ``stddevSampleIf(region == 'West', sales) -> 122.12``
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Bir tarihten itibaren ayı çıkar. Tarih için-işleci ile aynı* ``subDays(toDate('2016-08-08'), 1) -> 2016-08-09``
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Tarih veya zaman damgasından ayları çıkar* ``subMonths(toDate('2016-09-30'), 1) -> 2016-08-31``
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Belirli bir uzunluktaki alt dizeyi bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, varsayılan dize sonuna ayarlanır* ``substring('Cat in the hat', 5, 2) -> 'in'``
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayısal bir sütunun toplam toplamını alır* ``sum(col) -> value``
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Sayısal bir sütunun farklı değerlerinin toplam toplamını alır* ``sumDistinct(col) -> value``
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul herhangi bir sütunu temel alabilir* ``sumDistinctIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumDistinctIf(true, sales) -> SUM(sales)``
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul herhangi bir sütunu temel alabilir* ``sumIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumIf(true, sales) -> SUM(sales)``
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir teğet değeri hesaplar* ``tan(0) -> 0.0``
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiperbolik tanjant değerini hesaplar* ``tanh(0) -> 0.0``
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
('T ', ' true ', ' y ', ' Yes ', ' 1 ') değerini true ve (' f ', ' false ', ' n ', ' No ', ' 0 ') değerini false ve NULL değerine dönüştürür* ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``toBoolean('truthy') -> NULL``
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Bir dizeyi isteğe bağlı tarih biçimi verilen bir tarihe dönüştürür. Tüm olası biçimler için Java SimpleDateFormat bölümüne bakın. Tarih biçimi atlanırsa, aşağıdakilerin birleşimleri kabul edilir. [yyyy, yyyy-[M] M, yyyy-[M] M-[d] d, yyyy-[M] m-[d] d, yyyy-[m] m-[d] d, yyyy-[M] M-[d] dT *]* ``toDate('2012-8-8') -> 2012-8-8``
* ``toDate('12/12/2012', 'MM/dd/yyyy') -> 2012-12-12``
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi ondalık bir değere dönüştürür. Duyarlık ve ölçek belirtilmemişse, varsayılan olarak ayarlanır (10, 2). Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN gibi BCP47 dili biçiminde isteğe bağlı bir yerel ayar biçimi* ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir Double değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN gibi BCP47 dili biçiminde isteğe bağlı bir yerel ayar biçimi* ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir float değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Herhangi bir Double 'ı keser* ``toFloat(123.45) -> 123.45``
* ``toFloat('123.45') -> 123.45``
* ``toFloat('$123.45', '$###.00') -> 123.45``
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir tamsayı değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Long, float, Double ile keser* ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi uzun bir değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Tüm float, Double* ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Herhangi bir sayısal veya dizeyi bir kısa değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Tüm tamsayılar, Long, float, Double* ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
İlkel bir veri türünü dizeye dönüştürür. Sayı ve tarih için bir biçim belirtilebilir. Belirtilmezse, sistem varsayılanı seçilir. Sayılar için Java Decimal biçimi kullanılır. Olası tüm tarih biçimleri için Java SimpleDateFormat öğesine bakın; Varsayılan biçim yyyy-aa-gg şeklindedir* ``toString(10) -> '10'``
* ``toString('engineer') -> 'engineer'``
* ``toString(123456.789, '##,###.##') -> '123,456.79'``
* ``toString(123.78, '000000.000') -> '000123.780'``
* ``toString(12345, '##0.#####E0') -> '12.345E3'``
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``
* ``toString(toDate('2018-12-31'), 'MM/dd/yy') -> '12/31/18'``
* ``toString(4 == 20) -> 'false'``
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Bir dizeyi, isteğe bağlı bir zaman damgası biçimi verilen bir tarihe dönüştürür. Tüm olası biçimler için Java SimpleDateFormat bölümüne bakın. Zaman damgası atlanırsa yyyy-[M] M-[d] d hh: mm: SS [. f...] kullanılır* ``toTimestamp('2016-12-31 00:12:00') -> 2012-8-8T00:12:00``
* ``toTimestamp('2016/12/31T00:12:00', 'MM/dd/yyyyThh:mm:ss') -> 2012-12-12T00:12:00``
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Zaman damgasını UTC 'ye dönüştürür. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Varsayılan olarak geçerli saat dilimine ayarlanır* ``toUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Bir karakter kümesini dizedeki başka bir karakter kümesiyle değiştirin. Karakterler 1 ile 1 arasında değiştirme* ``translate('(Hello)', '()', '[]') -> '[Hello]'``
* ``translate('(Hello)', '()', '[') -> '[Hello'``
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Baştaki ve sondaki karakterlerden oluşan dizeyi kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi takdirde, ikinci parametrede belirtilen karakterleri kırpar* ``trim('!--!wor!ld!', '-!') -> 'wor!ld'``
___
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Her zaman doğru bir değer döndürür. ' True ' adlı bir sütun varsa işlev sözdizimini (true ()) kullanın* ``isDiscounted == true()``
* ``isDiscounted() == true``
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Sütunun türüyle eşleşir. Yalnızca desenli ifadelerde kullanılabilir. sayı Short, Integer, Long, Double, float veya Decimal ile eşleşir, tamsayısı Short, Integer, Long, kesirli eşleşir Double, float, Decimal ve DateTime ile tarih veya zaman damgası türüyle eşleşir* ``typeMatch(type, 'number') -> true``
* ``typeMatch('date', 'number') -> false``
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Büyük/büyük durumlar dize* ``upper('bojjus') -> 'BOJJUS'``
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun varyansını alır* ``variance(sales) -> 122.12``
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun farkını alır* ``varianceIf(region == 'West', sales) -> 122.12``
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun popülasyon varyansını alır* ``variancePopulation(sales) -> 122.12``
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere göre, bir sütunun popülasyon varyansını alır* ``variancePopulationIf(region == 'West', sales) -> 122.12``
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Bir sütunun taraflı olmayan varyansını alır* ``varianceSample(sales) -> 122.12``
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Ölçütlere dayalı olarak, bir sütunun taraflı olmayan varyansını alır* ``varianceSampleIf(region == 'West', sales) -> 122.12``
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Bir tarih verilen yılın haftasını alır* ``weekOfYear(toDate('2008-02-20')) -> 8``
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Mantıksal XOR işleci. ^ İşleci ile aynı* ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Tarihin yıl değerini alır* ``year(toDate('2012-8-8')) -> 2012``

## <a name="next-steps"></a>Sonraki adımlar

[Ifade oluşturucuyu nasıl kullanacağınızı öğrenin](concepts-data-flow-expression-builder.md).
