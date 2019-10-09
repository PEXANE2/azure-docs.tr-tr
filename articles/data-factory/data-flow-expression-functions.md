---
title: Azure Data Factory veri akışı eşleme özelliğindeki ifade işlevleri
description: Eşleme veri akışındaki ifade işlevleri hakkında bilgi edinin.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: c062a75516a1b865c1ff6c35f00d4fbf7c4881c6
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029382"
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
Bir kosinüs ters değerini hesaplar * ``acos(1) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Bir çift dize veya sayı ekler. Gün sayısı için bir tarih ekler. Zaman damgasına bir süre ekler. Benzer türdeki bir diziyi diğerine ekler. \+ İşleci ile aynı * ``add(10, 20) -> 30`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9<br/><br/>
Tarih veya zaman damgasına gün ekleyin. Tarih için + işleciyle aynı * ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')`` @ no__t-1 @ no__t-2<br/><br/>
Tarih veya zaman damgasına aylar ekleyin. İsteğe bağlı olarak bir saat dilimi geçirebilirsiniz * ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Mantıksal AND işleci. & & @No__t-0 @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Ters Sinüs değerini hesaplar * ``asin(0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Ters tanjant değerini hesaplar * ``atan(0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Düzlemin pozitif x ekseni ile koordinat tarafından verilen nokta arasındaki radyan cinsinden açıyı döndürür * ``atan2(0, 0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun değerlerinin ortalamasını alır * ``avg(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre bir sütunun değerlerinin ortalamasını alır * ``avgIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Akıştaki ada göre bir sütun değeri seçer. İkinci bağımsız değişken olarak isteğe bağlı bir akış adı geçirebilirsiniz. Birden çok eşleşme varsa, ilk eşleşme döndürülür. Eğer eşleşme yoksa NULL değer döndürür. Döndürülen değer tür dönüştürme işlevlerinden biri tarafından dönüştürülmüş olmalıdır (TO_DATE, TO_STRING...).  Tasarım zamanında bilinen sütun adları yalnızca adına göre değinmelidir. Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler kullanabilirsiniz * ``toString(byName('parent'))`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7<br/><br/>
Akışta göreli konumuna göre (1 tabanlı) bir sütun değeri seçer. Konum sınırların dışında ise, NULL bir değer döndürür. Döndürülen değer tür dönüştürme işlevlerinden biri tarafından dönüştürülmüş olmalıdır (TO_DATE, TO_STRING...) Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler kullanabilirsiniz * ``toString(byPosition(1))`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6<br/><br/>
Değişen koşullara göre bir değer veya diğeri geçerlidir. Giriş sayısı çift ise, diğeri son koşul için varsayılan olarak NULL olarak ayarlanır * ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Bir sayının Küp kökünü hesapla * ``cbrt(8) -> 2.0`` @ no__t-1 @ no__t-2<br/><br/>
Sayıdan küçük olmayan en küçük tamsayıyı döndürür * ``ceil(-0.1) -> 0`` @ no__t-1 @ no__t-2<br/><br/>
Giriş kümesinden gelen ilk null değeri döndürür. Tüm girişler aynı türde olmalıdır * ``coalesce(10, 20) -> 10`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Aynı türdeki iki değeri karşılaştırır. Değer1 < değer2 ise negatif tamsayı döndürür, Değer1 = = değer2 ise pozitif değer > değer2 * ``(compare(12, 24) < 1) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Değişken sayıda dizeyi birlikte birleştirir. Dizeleri olan + işleciyle aynı * ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir dizi dizeyi bir ayırıcıyla birlikte birleştirir. İlk parametre ayırıcı * ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Belirtilen dizideki herhangi bir öğe, belirtilen koşulda doğru olarak değerlendiriliyorsa true değerini döndürür. Contains, koşul işlevindeki bir öğeye bir başvuru bekliyor #item * ``contains([1, 2, 3, 4], #item == 3) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir kosinüs değeri hesaplar * ``cos(10) -> -0.8390715290764524`` @ no__t-1 @ no__t-2<br/><br/>
Değerin hiperbolik kosinüsünü hesaplar * ``cosh(0) -> 1.0`` @ no__t-1 @ no__t-2<br/><br/>
Değerlerin toplam sayısını alır. İsteğe bağlı sütun (ler) belirtilmişse, sayımla NULL değerleri yoksayar * ``count(custId)`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Bir sütun kümesinin farklı değerlerinin toplam sayısını alır * ``countDistinct(custId, custName)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre toplam değer sayısını alır. İsteğe bağlı sütun belirtilmişse, sayımla NULL değerleri yoksayar * ``countIf(state == 'CA' && commission < 10000, name)`` @ no__t-1 @ no__t-2<br/><br/>
İki sütun arasındaki popülasyon kovaryansını alır * ``covariancePopulation(sales, profit)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, iki sütunun popülasyon kovaryansını alır * ``covariancePopulationIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
İki sütunun örnek kovaryansını alır * ``covarianceSample(sales, profit)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, iki sütunun örnek kovaryansını alır * ``covarianceSampleIf(region == 'West', sales, profit)`` @ no__t-1 @ no__t-2<br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin CRC32 karmasını hesaplar. Bir satır için parmak izini hesaplamak için kullanılabilir * ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L`` @ no__t-1 @ no__t-2<br/><br/>
CumeDist işlevi, bir değerin bölüm içindeki tüm değerlere göre konumunu hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satıra göre veya buna eşit olan satır sayısıdır ve pencere bölümündeki toplam satır sayısı ile ayrılır. Sıralamada bulunan tüm bağlama değerleri aynı konuma göre değerlendirilir.
* ``cumeDist()``
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Bu işin çalışmaya başladığı geçerli tarihi alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``currentDate() == toDate('2250-12-31') -> false`` @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
İş yerel saat dilimiyle çalışmaya başladığında geçerli zaman damgasını alır * ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false`` @ no__t-1 @ no__t-2<br/><br/>
Geçerli zaman damgasını UTC olarak alır. Geçerli saatin, küme saat diliminizdeki farklı bir saat diliminde yorumlanması istiyorsanız, isteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Varsayılan olarak geçerli saat dilimine ayarlanır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.htmlTo UTC saatini farklı bir saat dilimine Dönüştür fromUTC () * ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false`` @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Ayın gününü alır * ``dayOfMonth(toDate('2018-06-08')) -> 8`` @ no__t-1 @ no__t-2<br/><br/>
Bir tarih verilen haftanın gününü alır. 1-Pazar, 2-Pazartesi..., 7-Cumartesi * ``dayOfWeek(toDate('2018-06-08')) -> 6`` @ no__t-1 @ no__t-2<br/><br/>
Yıl verilen tarihin gününü alır * ``dayOfYear(toDate('2016-04-09')) -> 100`` @ no__t-1 @ no__t-2<br/><br/>
Gün sayısı için milisaniye cinsinden süre * ``days(2) -> 172800000L`` @ no__t-1 @ no__t-2<br/><br/>
Radyana derece derece dönüştürür * ``degrees(3.141592653589793) -> 180`` @ no__t-1 @ no__t-2<br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk üretmeyecektir. Yoğun sıralama, veriler sıralanmasa bile ve değerlerde değişiklik ararken bile geçerlidir * ``denseRank(salesQtr, salesAmt)`` @ no__t-1 @ no__t-2<br/><br/>
Sayı çiftini böler. /İşleci ile aynı * ``divide(20, 10) -> 2`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Dizenin belirtilen dize ile bitip bitmediğini denetler * ``endsWith('dumbo', 'mbo') -> true`` @ no__t-1 @ no__t-2<br/><br/>
Karşılaştırma eşittir işleci. Aynı = = işleci * ``equals(12, 24) -> false`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6<br/><br/>
Karşılaştırma eşittir işleci büyük/küçük harf yok sayılıyor. < = > İşleci * ``'abc'<=>'Abc' -> true`` @ no__t-1 @ no__t-2 @ no__t-3 ile aynı<br/><br/>
Bir sayının faktöriyelini hesaplar * ``factorial(5) -> 120`` @ no__t-1 @ no__t-2<br/><br/>
Her zaman yanlış bir değer döndürür. ' False ' adlı bir sütun varsa (false ()) işlev sözdizimini kullanın * ``(10 + 20 > 30) -> false`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Öğeleri, belirtilen koşulu karşılamayan dizinin dışına filtreler. Filtre, koşul işlevindeki bir öğeye başvuru bekliyor #item * ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir sütun grubunun ilk değerini alır. IgnoreNulls ikinci parametresi atlanırsa, false olarak kabul edilir * ``first(sales)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Sayıdan büyük olmayan en büyük tamsayıyı döndürür * ``floor(-0.1) -> -1`` @ no__t-1 @ no__t-2<br/><br/>
UTC 'den zaman damgasına dönüştürür. İsteğe bağlı olarak, saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Kullanılabilir biçimler için geçerli timezoneRefer Java 'nın SimpleDateFormat öğesine varsayılan olarak ayarlanır. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Karşılaştırma daha büyük işleci. > İşleçle aynı * ``greater(12, 24) -> false`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Karşılaştırma daha büyük veya eşit işleç. > = İşleci ile aynı * ``greaterOrEqual(12, 12) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Giriş null değerlerini atlayarak değer listesi arasındaki en büyük değeri döndürür. Tüm girişler null ise null döndürür * ``greatest(10, 30, 15, 20) -> 30`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Akıştaki ada göre bir sütun değeri olup olmadığını denetler. İkinci bağımsız değişken olarak isteğe bağlı bir akış adı geçirebilirsiniz.  Tasarım zamanında bilinen sütun adları yalnızca adına göre değinmelidir. Hesaplanan girişler desteklenmez, ancak parametre değiştirmeler kullanabilirsiniz * ``hasColumn('parent')`` @ no__t-1 @ no__t-2<br/><br/>
Bir zaman damgasının saat değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Saat sayısı için milisaniye cinsinden süre * ``hours(2) -> 7200000L`` @ no__t-1 @ no__t-2<br/><br/>
Dizideki her öğeyi, belirtilen ifadeyi kullanarak yeni bir öğeye eşler. Eşleme, ifade işlevindeki bir öğeye #item ve #index olarak öğe dizinine bir başvuru bekliyor * ``iMap([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]`` @ no__t-1 @ no__t-2<br/><br/>
Bir koşula bağlı olarak bir değer veya diğerini uygular. Diğeri belirtilmemişse NULL kabul edilir. Her iki değer de uyumlu olmalıdır (sayısal, dize...) * ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Değerin NULL olup olmadığını denetler ve diğerini döndürür. Null olmayan ilk değeri bulana kadar tüm girişleri sınar * ``iifNull(10, 20) -> 10`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir öğenin dizide olup olmadığını denetler * ``in([10, 20, 30], 10) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Her sözcüğün ilk harfini büyük harfe dönüştürür. Sözcükler boşluk ile ayrılmış olarak tanımlanır * ``initCap('cool iceCREAM') -> 'Cool Icecream'`` @ no__t-1 @ no__t-2<br/><br/>
Bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. no__t, bulunamazsa döndürülür * ``instr('dumbo', 'mbo') -> 3`` @-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Satırın silme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isDelete()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Satırın hata olarak işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isError()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Satırın yok sayılacak şekilde işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isIgnore()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Satırın ekleme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isInsert()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Arama sırasında satırın eşleşip eşleşmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isMatch()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Değerin NULL olup olmadığını denetler * ``isNull(NULL()) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Satırın güncelleştirme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isUpdate()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Satırın ekleme için işaretlenip işaretlenmediğini denetler. Birden fazla giriş akışı alan dönüşümler için akışın (1 tabanlı) dizinini geçirebilirsiniz. Akış dizini için varsayılan değer 1 * ``isUpsert()`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir sütunun basıklığını alır * ``kurtosis(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere dayalı olarak, bir sütunun basıklığını alır * ``kurtosisIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Geçerli satırdan önce değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, geri aranacak satır sayısıdır ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa varsayılan değer belirtilmediği sürece null değeri döndürülür * ``lag(amount, 2)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir sütun grubunun son değerini alır. IgnoreNulls ikinci parametresi atlanırsa, false olarak kabul edilir * ``last(sales)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Tarih verilen tarihin son tarihini alır * ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')`` @ no__t-1 @ no__t-2<br/><br/>
Geçerli satırdan sonra değerlendirilen n satıra ilk parametre değerini alır. İkinci parametre, görüntülenecek satır sayısı ve varsayılan değer 1 ' dir. Çok sayıda satır yoksa varsayılan değer belirtilmediği sürece null değeri döndürülür * ``lead(amount, 2)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci ile aynı * ``least(10, 30, 15, 20) -> 10`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Dizin 1 ' den karakter sayısı ile başlayan bir alt dize ayıklar. Alt DIZE ile aynı (Str, 1, n) * ``left('bojjus', 2) -> 'bo'`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Dizenin uzunluğunu döndürür * ``length('dumbo') -> 5`` @ no__t-1 @ no__t-2<br/><br/>
Karşılaştırma daha az işleci. < İşleçle aynı * ``lesser(12, 24) -> true`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Karşılaştırma küçüktür veya eşittir işleci. < = İşleci ile aynı * ``lesserOrEqual(12, 12) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
İki dize arasındaki mesafeyi alır * ``levenshtein('boys', 'girls') -> 4`` @ no__t-1 @ no__t-2<br/><br/>
Bu, tam anlamıyla eşleşen bir dizedir. Özel durumlar aşağıdaki özel sembollerdir: _, girişte bir karakter ile eşleşir (şuna benzer. POSIX normal ifadelerinde)%, girişte sıfır veya daha fazla karakterle eşleşir (POSIX normal ifadelerinde. * ile benzerdir).
Kaçış karakteri ' '. Bir kaçış karakteri özel bir sembolden veya başka bir kaçış karakteriyle önceyse, aşağıdaki karakter tam anlamıyla eşleştirilir. Başka bir karakter kaçış geçersizdir.
* ``like('icecream', 'ice%') -> true``
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Belirli bir konumu Başlatan bir dize içindeki alt dizenin konumunu (1 tabanlı) bulur. Konum atlanırsa, dizenin başından hesaba göre değerlendirilir. no__t, bulunamazsa döndürülür * ``locate('mbo', 'dumbo') -> 3`` @-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Günlük değerini hesaplar. @No__t-0 @ no__t-1 @ no__t-2 kullanılırsa, isteğe bağlı bir temel, başka bir Euler numarası sağlanabilir<br/><br/>
10 tabanına göre günlük değerini hesaplar * ``log10(100) -> 2`` @ no__t-1 @ no__t-2<br/><br/>
Küçük harf, bir dize * ``lower('GunChus') -> 'gunchus'`` @ no__t-1 @ no__t-2<br/><br/>
Belirli bir uzunluğa ulaşana kadar dizeyi sağlanan doldurmaya göre aşağı doğru doldurma. Dize uzunluğuna eşit veya daha büyük ise, bu değer uzunluğa göre kırpılır * ``lpad('dumbo', 10, '-') -> '-----dumbo'`` @ no__t-1 @ no__t-2 ' ' Lpad (' Dumbo ', 8, ' < > ')-> ' < > <dumbo'``
___
### <code>ltrim @ no__t-4 @ no__t-5<br/><br/>
Sol karakterlerin bir dizesini sola kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi durumda ikinci parametrede belirtilen karakterleri kırpar * ``ltrim('  dumbo  ') -> 'dumbo  '`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Dizideki her öğeyi, belirtilen ifadeyi kullanarak yeni bir öğeye eşler. Eşleme, ifade işlevindeki bir öğeye bir başvuru bekliyor #item * ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir sütunun en büyük değerini alır * ``max(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun en büyük değerini alır * ``maxIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin MD5 özetini hesaplar ve 32 karakter onaltılı dize döndürür. Bir satır için parmak izini hesaplamak için kullanılabilir * ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun değerlerinin ortalama değerini alır. AVG @no__t ile aynı-0 @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere dayalı olarak bir sütunun değerlerinin ortalaması alınır. AvgIf ile aynı * ``meanIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir tarihin milisaniyelik değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871`` @ no__t-2 @ no__t-3<br/><br/>
Milisaniye cinsinden milisaniye cinsinden süre * ``seconds(2) -> 2L`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun en küçük değerini alır * ``min(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun en küçük değerini alır * ``minIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Sayıları çıkartır. Gün sayısından çıkar. Zaman damgasından alt katman süresi. Milisaniye cinsinden farkı almak için iki zaman damgasını alt katman. -İşleci * ``minus(20, 10) -> 10`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7<br/><br/>
Bir zaman damgasının dakika değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Dakika sayısı için milisaniye cinsinden süre * ``minutes(2) -> 120000L`` @ no__t-1 @ no__t-2<br/><br/>
Sayı çiftinin mod sayısı. % Operator @no__t ile aynı-0 @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Tarih veya zaman damgasının ay değerini alır * ``month(toDate('2012-8-8')) -> 8`` @ no__t-1 @ no__t-2<br/><br/>
İki tarih arasındaki ay sayısını alır. Hesaplamayı yuvarlayaseçebilirsiniz. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677`` @ no__t-2 @ no__t-3<br/><br/>
Sayı çiftini çarpar. \* İşleci ile aynı * ``multiply(20, 10) -> 200`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
NTile işlevi her pencere bölümü için satırları, 1 ' den en `n` ' e kadar olan `n` demetlerine böler. Demet değerleri en fazla 1 farklı olacaktır. Bölümdeki satır sayısı demet sayısına eşit olarak bölünmezse, ilk sepete başlayarak, geri kalan değerler her demet için bir dağıtılır. NTile işlevi, terkutucukların, kubotların ve diğer yaygın Özet istatistiğin hesaplanması için yararlıdır. İşlev, başlatma sırasında iki değişkeni hesaplar: düzenli bir demet boyutunun boyutuna eklenmiş bir ek satır olacaktır. Her iki değişken de geçerli bölümün boyutunu temel alır. Hesaplama işlemi sırasında işlev geçerli satır numarasını, geçerli demet numarasını ve demet 'in değiştirileceği satır numarasını (bucketThreshold) izler. Geçerli satır numarası demet eşiğine ulaştığında, demet değeri bir artırılır ve eşik, demet boyutu (ve geçerli demet doldurulmuş ise bir ek) ile artırılır.
* ``nTile()``
* ``nTile(numOfBuckets)``
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Bir sayıyı geçersiz kılar. Pozitif sayıları negatif ve tam tersi @no__t döndürür-0 @ no__t-1 @ no__t-2<br/><br/>
Sonraki benzersiz diziyi döndürür. Numara yalnızca bir bölüm içinde ardışık ve PartitionID * ``nextSequence() == 12313112 -> false`` @ no__t-1 @ no__t-2 ' dir<br/><br/>
Dize değerini, aksanlı Unicode karakterlerini ayırmak için normalleştirin * ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'`` @ no__t-1 @ no__t-2<br/><br/>
Mantıksal Değilleme İşleci * ``not(true) -> false`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Karşılaştırma eşit değildir işleci. ! = İşleci * ``12 != 24 -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
NULL değeri döndürür. ' Null ' adlı bir sütun varsa işlev sözdizimini (null ()) kullanın. Tarafından kullanılan tüm işlemler NULL @no__t ile sonuçlanır-0 @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6<br/><br/>
Mantıksal OR işleci. Aynı | | * ``or(true, false) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Sayı çiftinin pozitif mod sayısı.
* ``pmod(-20, 8) -> 4``
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Giriş satırının geçerli bölüm kimliğini döndürür * ``partitionId()`` @ no__t-1 @ no__t-2<br/><br/>
Bir sayıyı diğerinin kuvvetine yükseltir * ``power(10, 2) -> 100`` @ no__t-1 @ no__t-2<br/><br/>
Bir değer grubundaki bir değerin derecesini hesaplar. Sonuç, Bölüm sıralamasına göre geçerli satırdan önceki veya eşit olan satır sayısıdır. Değerler dizide boşluk oluşturur. Sıralama, veriler sıralanana sırada ve değerlerde değişiklik ararken bile geçerlidir * ``rank(salesQtr, salesAmt)`` @ no__t-1 @ no__t-2<br/><br/>
Öğeleri bir dizide biriktirir. Azaltma, #acc ve #item gibi ilk ifade işlevindeki bir Biriktiricinin ve bir öğenin bir başvurusunu bekliyor ve ikinci ifade işlevinde kullanılacak #result olarak sonuçta elde edilen değeri bekliyor * ``reduce([1, 2, 3, 4], 0, #acc + #item, #result) -> 10`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Verili bir Regex deseninin eşleşen alt dizesini ayıklayın. Son parametre, eşleşme grubunu tanımlar ve atlanırsa varsayılan olarak 1 ' e ayarlanır. Kaçış olmadan bir dizeyle eşleştirmek için ' <regex> ' (arka tırnak) kullanın * ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Dizenin verilen Regex düzeniyle eşleşip eşleşmediğini denetler. Kaçış olmadan bir dizeyle eşleştirmek için ' <regex> ' (arka tırnak) kullanın * ``regexMatch('200.50', '(\\d+).(\\d+)') -> true`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir Regex deseninin tüm yinelemelerini, belirtilen dizedeki başka bir alt dize ile değiştirin. kaçış olmadan bir dizeyle eşleştirmek için ' <regex> ' (arka tırnak) kullanın * ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir dizeyi, Regex temelinde bir sınırlayıcı temelinde böler ve dizeler dizisi döndürür * ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Bir alt dizenin tüm tekrarlamalarını verilen dizedeki başka bir alt dize ile değiştirin. Son parametre atlanırsa, varsayılan olarak boş dize olur * ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Dizeyi tersine çevirir * ``reverse('gunchus') -> 'suhcnug'`` @ no__t-1 @ no__t-2<br/><br/>
Sağdan karakter sayısıyla bir alt dize ayıklar. SUBSTRING ile aynı (Str, LENGTH (str)-n, n) * ``right('bojjus', 2) -> 'us'`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Dizenin belirtilen Regex düzeniyle eşleşip eşleşmediğini denetler * ``rlike('200.50', `(\d+).(\d+)`) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir sayıyı isteğe bağlı bir ölçek ve isteğe bağlı bir yuvarlama modu olarak yuvarlar. Ölçek atlanırsa, varsayılan olarak 0 olur.  Mod atlanırsa, varsayılan olarak ROUND_HALF_UP (5) olarak ayarlanır. Yuvarlama değerleri şunlardır 1-ROUND_UP 2-ROUND_DOWN 3-ROUND_CEILING 4-ROUND_FLOOR 5-ROUND_HALF_UP 6-ROUND_HALF_DOWN 7-ROUND_HALF_EVEN 8-ROUND_UNNECESSARY * ``round(100.123) -> 100.0`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
1 * ``rowNumber()`` @ no__t-1 @ no__t-2 ' den başlayarak bir penceredeki satırlar için sıralı bir satır numaralandırması atar<br/><br/>
Dizeyi, belirli bir uzunluğa ulaşana kadar sağlanan doldurmaya göre sağ Altlar. Dize uzunluğuna eşit veya daha büyük ise, bu değer uzunluğa atılır * ``rpad('dumbo', 10, '-') -> 'dumbo-----'`` @ no__t-1 @ no__t-2 @ no__t-3rtrim @ no__t-4 @ no__t-5<br/><br/>
Baştaki karakterlerden oluşan dizeyi sağ kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi durumda ikinci parametrede belirtilen karakterleri kırpar * ``rtrim('  dumbo  ') -> '  dumbo'`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir tarihin ikinci değerini alır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Yerel Saat dilimi varsayılan olarak kullanılır. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``second(toTimestamp('2009-07-30 12:58:59')) -> 59`` @ no__t-2 @ no__t-3<br/><br/>
Milisaniye cinsinden saniye sayısı için süre * ``seconds(2) -> 2000L`` @ no__t-1 @ no__t-2<br/><br/>
Farklı ilkel veri türlerinin sütun kümesinin SHA-1 özetini hesaplar ve 40 karakter onaltılı dize döndürür. Bir satır için parmak izini hesaplamak için kullanılabilir * ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'`` @ no__t-1 @ no__t-2<br/><br/>
Yalnızca 0 (256), 224, 256, 384, 512 değerlerinin yer aldığı bir bit uzunluğu verilen, farklı ilkel veri türlerinin sütun kümesinin SHA-2 özetini hesaplar. Bir satır için parmak izini hesaplamak için kullanılabilir * ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'`` @ no__t-1 @ no__t-2<br/><br/>
Bir sinüs değeri hesaplar * ``sin(2) -> 0.9092974268256817`` @ no__t-1 @ no__t-2<br/><br/>
Bir hiperbolik sinüs değeri hesaplar * ``sinh(0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun çarpıklığını alır * ``skewness(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun çarpıklığını alır * ``skewnessIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir dizinin alt kümesini bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, varsayılan dize sonuna ayarlanır * ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7<br/><br/>
Belirtilen koşul işlevini kullanarak diziyi sıralar. Sıralama, #item1 ve #item2 * ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]`` @ no__t-1 @ no__t-2 @ no__t-3 olarak ifade işlevindeki birbirini izleyen iki öğeye bir başvuru bekliyor<br/><br/>
Dize için soundex kodunu alır * ``soundex('genius') -> 'G520'`` @ no__t-1 @ no__t-2<br/><br/>
Bir dizeyi sınırlayıcıya göre böler ve dizeler dizisi döndürür * ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8<br/><br/>
Bir sayının kare kökünü hesaplar * ``sqrt(9) -> 3`` @ no__t-1 @ no__t-2<br/><br/>
Dizenin sağlanan dizeyle @no__t olup olmadığını denetler.-0 @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun standart sapmasını alır * ``stdDev(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun standart sapmasını alır * ``stddevIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun popülasyon standart sapmasını alır * ``stddevPopulation(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun popülasyon standart sapmasını alır * ``stddevPopulationIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun örnek standart sapmasını alır * ``stddevSample(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun örnek standart sapmasını alır * ``stddevSampleIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir tarih veya zaman damgasından ay çıkarın. Tarih için-işleci ile aynı * ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')`` @ no__t-1 @ no__t-2<br/><br/>
Tarih veya zaman damgasından ayları çıkar * ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')`` @ no__t-1 @ no__t-2<br/><br/>
Belirli bir uzunluktaki alt dizeyi bir konumdan ayıklar. Konum 1 tabanlıdır. Uzunluk atlanırsa, varsayılan olarak dize sonuna ayarlanır * ``substring('Cat in the hat', 5, 2) -> 'in'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5<br/><br/>
Sayısal sütunun toplam toplamını alır * ``sum(col)`` @ no__t-1 @ no__t-2<br/><br/>
Sayısal bir sütunun farklı değerlerinin toplam toplamını alır * ``sumDistinct(col)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul herhangi bir sütunu temel alabilir * ``sumDistinctIf(state == 'CA' && commission < 10000, sales)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Ölçütlere dayalı olarak sayısal bir sütunun toplam toplamı alınır. Koşul herhangi bir sütunu temel alabilir * ``sumIf(state == 'CA' && commission < 10000, sales)`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Bir teğet değeri hesaplar * ``tan(0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Bir hiperbolik tanjant değeri hesaplar * ``tanh(0) -> 0.0`` @ no__t-1 @ no__t-2<br/><br/>
Herhangi bir sayısal/Tarih/zaman damgası/dizeyi ikili gösterimine dönüştürür * ``toBinary(3) -> [0x11]`` @ no__t-1 @ no__t-2<br/><br/>
('T ', ' true ', ' y ', ' Yes ', ' 1 ') değerini true ve (' f ', ' false ', ' n ', ' No ', ' 0 ') doğru ve NULL değerine dönüştürür * ``toBoolean('true') -> true`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
İsteğe bağlı bir giriş tarihi biçimi kullanarak giriş tarihi dizesini tarihe dönüştürür. Kullanılabilir biçimler için Java 'nın SimpleDateFormat bölümüne bakın. Giriş tarihi biçimi atlanırsa varsayılan biçim yyyy-[M] M-[d] d biçimindedir. Kabul edilen biçimler şunlardır: [yyyy, yyyy-[M] M, yyyy-[M] M-[d] d, yyyy-[M] M-[d] dT *] * ``toDate('2012-8-18') -> toDate('2012-08-18')`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Herhangi bir sayısal veya dizeyi ondalık bir değere dönüştürür. Duyarlık ve ölçek belirtilmemişse, varsayılan olarak ayarlanır (10, 2). Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN * ``toDecimal(123.45) -> 123.45`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 gibi BCP47 dili biçiminde isteğe bağlı bir yerel ayar biçimi<br/><br/>
Herhangi bir sayısal veya dizeyi bir Double değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. En-US, de, zh-CN * ``toDouble(123.45) -> 123.45`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 gibi BCP47 dili biçiminde isteğe bağlı bir yerel ayar biçimi<br/><br/>
Herhangi bir sayısal veya dizeyi bir float değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Herhangi bir Double @no__t keser-0 @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Herhangi bir sayısal veya dizeyi bir tamsayı değerine dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Long, float, Double * ``toInteger(123) -> 123`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 ' ü keser<br/><br/>
Herhangi bir sayısal veya dizeyi uzun bir değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Tüm float, Double * ``toLong(123) -> 123`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Herhangi bir sayısal veya dizeyi bir kısa değere dönüştürür. Dönüştürme için isteğe bağlı bir Java ondalık biçimi kullanılabilir. Tüm tamsayıyı, uzun, float, Double * ``toShort(123) -> 123`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 ' ü keser<br/><br/>
İlkel bir veri türünü dizeye dönüştürür. Sayı ve tarih için bir biçim belirtilebilir. Belirtilmezse, sistem varsayılanı seçilir. Sayılar için Java Decimal biçimi kullanılır. Olası tüm tarih biçimleri için Java SimpleDateFormat öğesine bakın; Varsayılan biçim yyyy-aa-gg * ``toString(10) -> '10'`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6 @ no__t-7 @ no__t-8 @ no__t-9<br/><br/>
Bir dizeyi, isteğe bağlı bir zaman damgası biçimi verilen bir zaman damgasına dönüştürür. Tüm olası biçimler için Java SimpleDateFormat bölümüne bakın. Zaman damgası varsayılan düzende yoksayılırsa. yyyy-[M] M-[d] d hh: mm: SS [. f...] kullanılır. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Zaman damgası, kullanılabilir biçimler için Java 'nın SimpleDateFormat değerini Ifade eden 999'A göre daha fazla milisaniyelik doğruluğu destekler. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')`` @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6<br/><br/>
Zaman damgasını UTC 'ye dönüştürür. İsteğe bağlı bir saat dilimini ' GMT ', ' PST ', ' UTC ', ' Amerika/Cayman ' biçiminde geçirebilirsiniz. Kullanılabilir biçimler için geçerli timezoneRefer Java 'nın SimpleDateFormat öğesine varsayılan olarak ayarlanır. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false`` @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir karakter kümesini dizedeki başka bir karakter kümesiyle değiştirin. Karakterler 1 ile 1 arasında değiştirme * ``translate('(bojjus)', '()', '[]') -> '[bojjus]'`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Baştaki ve sondaki karakterlerden oluşan dizeyi kırpar. İkinci parametre belirtilmemişse boşluğu kırpar. Aksi durumda ikinci parametrede belirtilen karakterleri kırpar * ``trim('  dumbo  ') -> 'dumbo'`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Her zaman doğru bir değer döndürür. ' True ' adlı bir sütun varsa (true ()) işlev sözdizimini kullanın * ``(10 + 20 == 30) -> true`` @ no__t-1 @ no__t-2 @ no__t-3<br/><br/>
Sütunun türüyle eşleşir. Yalnızca desenli ifadelerde kullanılabilir. sayı Short, Integer, Long, Double, float veya Decimal ile eşleşir, tamsayısı Short, Integer, Long, kesir, Double, float, Decimal ve DateTime ile eşleşen tarih veya zaman damgası türü * ``typeMatch(type, 'number')`` @ no__t-1 @ no__t-2 @ no__ t-3<br/><br/>
Büyük/veya bir dize * ``upper('bojjus') -> 'BOJJUS'`` @ no__t-1 @ no__t-2<br/><br/>
Oluşturulan UUID 'yi döndürür * ``uuid()`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun varyansını alır * ``variance(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun varyansını alır * ``varianceIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun popülasyon varyansını alır * ``variancePopulation(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun popülasyon varyansını alır * ``variancePopulationIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Bir sütunun taraflı olmayan varyansını alır * ``varianceSample(sales)`` @ no__t-1 @ no__t-2<br/><br/>
Ölçütlere göre, bir sütunun taraflı olmayan varyansını alır * ``varianceSampleIf(region == 'West', sales)`` @ no__t-1 @ no__t-2<br/><br/>
Yıl verilen tarihin haftasını alır * ``weekOfYear(toDate('2008-02-20')) -> 8`` @ no__t-1 @ no__t-2<br/><br/>
Hafta sayısı için milisaniye cinsinden süre * ``weeks(2) -> 1209600000L`` @ no__t-1 @ no__t-2<br/><br/>
Mantıksal XOR işleci. ^ İşleci * ``xor(true, false) -> true`` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4<br/><br/>
Bir tarihin yıl değerini alır * ``year(toDate('2012-8-8')) -> 2012``

## <a name="next-steps"></a>Sonraki adımlar

[Ifade oluşturucuyu nasıl kullanacağınızı öğrenin](concepts-data-flow-expression-builder.md).
