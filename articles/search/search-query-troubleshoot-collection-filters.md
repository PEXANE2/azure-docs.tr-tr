---
title: OData koleksiyon filtreleri sorunlarını giderme-Azure Search
description: Azure Search sorgularında OData toplama filtresi hatalarında sorun giderme.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: fbd43cc13d3b7377668aad2fadc874ae47422ee1
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647946"
---
# <a name="troubleshooting-odata-collection-filters-in-azure-search"></a>Azure Search 'de OData koleksiyon filtreleri sorunlarını giderme

Azure Search koleksiyon alanlarını [filtrelemek](query-odata-filter-orderby-syntax.md) için, [ `any` ve `all` işleçlerini](search-query-odata-collection-operators.md) **lambda ifadeleriyle**birlikte kullanabilirsiniz. Lambda ifadesi, bir koleksiyonun her öğesine uygulanan bir alt filtredir.

Filtre ifadelerinin her özelliği bir lambda ifadesi içinde kullanılamaz. Kullanılabilir özellikler, filtrelemek istediğiniz koleksiyon alanının veri türüne göre farklılık gösterir. Bu, bu bağlamda desteklenmeyen bir lambda ifadesinde bir özelliği kullanmayı denerseniz hata oluşmasına neden olabilir. Koleksiyon alanları üzerinde karmaşık bir filtre yazmaya çalışırken bu tür hatalarla karşılaşdıysanız, bu makale sorunu gidermenize yardımcı olur.

## <a name="common-collection-filter-errors"></a>Ortak koleksiyon filtre hataları

Aşağıdaki tabloda, bir koleksiyon filtresini yürütmeye çalışırken karşılaşabileceğiniz hatalar listelenmektedir. Bu hatalar, bir lambda ifadesinin içinde desteklenmeyen bir filtre ifadeleri özelliği kullandığınızda meydana gelir. Her hata, hata oluşmasını önlemek için filtrenizi nasıl yeniden yazabileceğiniz hakkında rehberlik sağlar. Bu tabloda, bu hatanın nasıl önleneceğini hakkında daha fazla bilgi sağlayan bu makalenin ilgili bölümüne yönelik bir bağlantı da bulunur.

| Hata iletisi | Olanını | Daha fazla bilgi için bkz. |
| --- | --- | --- |
| ' IsMatch ' işlevinin, Aralık değişkeninin ' ' öğesine göre hiçbir parametresi yok. Lambda ifadelerinde yalnızca ilişkili alan başvuruları desteklenir (' any ' veya ' All '). Lütfen filtrenizi ' IsMatch ' işlevinin lambda ifadesinin dışında olması için değiştirin ve yeniden deneyin. | Lambda `search.ismatch` ifadesi `search.ismatchscoring` içinde veya içinde kullanma | [Karmaşık koleksiyonları filtrelemeye yönelik kurallar](#bkmk_complex) |
| Geçersiz lambda ifadesi. Koleksiyon türü (EDM. String) alanı üzerinde yinelenen bir lambda ifadesinde ters beklenildiği eşitlik veya eşitsizlik için bir test bulundu. ' Any ' için lütfen ' x EQ y ' veya ' search.in (...) ' biçimindeki ifadeleri kullanın. ' All ' için lütfen ' x ne y ', ' Not (x EQ y) ' ya da ' Not search.in (...) ' biçimindeki ifadeleri kullanın. | Türünde bir alanda filtreleme`Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |
| Geçersiz lambda ifadesi. Desteklenmeyen bir karmaşık Boole ifadesi formu bulundu. ' Any ' için lütfen, ayırt edici normal form olarak da bilinen ' and lerin ' ORs ' ifadesi kullanın. Örneğin: a, b, c ve d ' nin karşılaştırma ya da eşitlik alt ifadeleri olduğu ' (a ve b) veya (c ve d) '. ' All ' için lütfen ' and of ORs ' olan ifadeleri kullanın, Ayrıca, ayırt edici normal form olarak da bilinir. Örneğin: a, b, c ve d, burada a, b, c ve d, karşılaştırma veya eşitsizlik alt ifadeleridir. Karşılaştırma ifadesi örnekleri: ' x gt 5 ', ' x Le 2 '. Eşitlik ifadesi örneği: ' x EQ 5 '. Eşitsizlik ifadesi örneği: ' x ne 5 '. | ,, Veya türündeki `Collection(Edm.DateTimeOffset)` `Collection(Edm.Double)` `Collection(Edm.Int32)`alanlar üzerinde filtreleme`Collection(Edm.Int64)` | [Karşılaştırılabilir koleksiyonları filtrelemeye yönelik kurallar](#bkmk_comparables) |
| Geçersiz lambda ifadesi. Koleksiyon türünde (EDM. Geographyıpoint) bir alan üzerinde yinelenen bir lambda ifadesinde, desteklenmeyen coğrafi. uzaklık () veya coğrafi. kesişme () kullanımı bulundu. ' Any ' için, ' lt ' veya ' le ' işleçlerini kullanarak coğrafi. distance () öğesini karşılaştırdığınızdan emin olun ve coğrafi. kesişme () kullanımının herhangi bir kullanımında olmadığından emin olun. ' All ' için, ' gt ' veya ' ge ' işleçlerini kullanarak coğrafi. distance () ' i karşılaştırdığınızdan emin olun ve coğrafi. kesişme () kullanımının tüm kullanımlarda olduğundan emin olun. | Türünde bir alanda filtreleme`Collection(Edm.GeographyPoint)` | [Geographyıpoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints) |
| Geçersiz lambda ifadesi. Koleksiyon türü (EDM. Geographi Point) alanları üzerinde yinelen Lambda ifadelerinde karmaşık Boole ifadeleri desteklenmez. ' Any ' için lütfen ' or ' ile alt ifadeleri birleştirin; ' ve ' desteklenmez. ' All ' için lütfen ' ve ' ile alt ifadelere katın; ' veya ' desteklenmez. | Veya türündeki `Collection(Edm.String)` alanlarda filtreleme`Collection(Edm.GeographyPoint)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) <br/><br/> [Geographyıpoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints) |
| Geçersiz lambda ifadesi. Bir karşılaştırma işleci bulundu (' lt ', ' le ', ' gt ' veya ' ge '). Yalnızca eşitlik işleçlerinde, koleksiyon türü (EDM. String) alanları üzerinde yineleme yapan Lambda ifadelerinde izin verilir. ' Any ' için lütfen ' x EQ y ' biçiminde ifadeler kullanın. ' All ' için lütfen ' x ne y ' veya ' Not (x EQ y) ' biçimindeki ifadeleri kullanın. | Türünde bir alanda filtreleme`Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |

<a name="bkmk_examples"></a>

## <a name="how-to-write-valid-collection-filters"></a>Geçerli koleksiyon filtreleri yazma

Geçerli koleksiyon filtreleri yazma kuralları her veri türü için farklıdır. Aşağıdaki bölümlerde, hangi filtre özelliklerinin desteklendiğini ve hangilerinin desteklenmediğini gösteren örnekler gösterilmektedir:

- [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings)
- [Boole koleksiyonlarını filtreleme kuralları](#bkmk_bools)
- [Geographyıpoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints)
- [Karşılaştırılabilir koleksiyonları filtrelemeye yönelik kurallar](#bkmk_comparables)
- [Karmaşık koleksiyonları filtrelemeye yönelik kurallar](#bkmk_complex)

<a name="bkmk_strings"></a>

## <a name="rules-for-filtering-string-collections"></a>Dize koleksiyonlarını filtreleme kuralları

Dize koleksiyonları için lambda ifadeleri içinde, yalnızca kullanılabilen karşılaştırma işleçleri ve `eq` `ne`' dir.

> [!NOTE]
> Azure Search, lambda ifadesinin içinde `lt` veya dışında dizeler için `gt` `le` / / işleçleri`ge` desteklemez / .

Gövdesi `any` yalnızca eşitsizlik için test `all` edilebilir iken yalnızca eşitlik için test edebilir.

Ayrıca, birden çok `or` ifadeyi bir `any`, ve `and` `all`gövdesinin içinde ile birleştirmek mümkündür. İşlev eşitlik denetimlerini birleştirme ile `or`eşdeğer olduğundan, ' ın `any`gövdesinde de izin verilir. `search.in` Buna karşılık `not search.in` , bir `all`' ın gövdesinde izin verilir.

Örneğin, bu ifadelere izin verilir:

- `tags/any(t: t eq 'books')`
- `tags/any(t: search.in(t, 'books, games, toys'))`
- `tags/all(t: t ne 'books')`
- `tags/all(t: not (t eq 'books'))`
- `tags/all(t: not search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' or t eq 'games')`
- `tags/all(t: t ne 'books' and not (t eq 'games'))`

Bu ifadelere izin verilmiyor:

- `tags/any(t: t ne 'books')`
- `tags/any(t: not search.in(t, 'books, games, toys'))`
- `tags/all(t: t eq 'books')`
- `tags/all(t: search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' and t ne 'games')`
- `tags/all(t: t ne 'books' or not (t eq 'games'))`

<a name="bkmk_bools"></a>

## <a name="rules-for-filtering-boolean-collections"></a>Boole koleksiyonlarını filtreleme kuralları

Tür `Edm.Boolean` yalnızcave`ne`işleçlerinidestekler. `eq` Bu nedenle, bu tür yan tümceleri, ile `and` / `or` aynı Aralık değişkenini denetleyen bir araya getirmek için, her zaman tautolotions veya Çelişmeler ' e yol açacağından çok mantıklı değildir.

Aşağıda, Boole koleksiyonlarında izin verilen filtrelerin bazı örnekleri verilmiştir:

- `flags/any(f: f)`
- `flags/all(f: f)`
- `flags/any(f: f eq true)`
- `flags/any(f: f ne true)`
- `flags/all(f: not f)`
- `flags/all(f: not (f eq true))`

Dize koleksiyonlarının aksine, Boole koleksiyonlarının, hangi tür lambda ifadesinde kullanılabilecek bir sınırı yoktur. Her ikisi `eq` de `any` veya gövdesinde`all`kullanılabilir. `ne`

Boole koleksiyonları için aşağıdakiler gibi ifadelere izin verilmez:

- `flags/any(f: f or not f)`
- `flags/any(f: f or f)`
- `flags/all(f: f and not f)`
- `flags/all(f: f and f eq true)`

<a name="bkmk_geopoints"></a>

## <a name="rules-for-filtering-geographypoint-collections"></a>Geographyıpoint koleksiyonlarını filtreleme kuralları

Bir koleksiyondaki tür `Edm.GeographyPoint` değerleri birbirleriyle doğrudan karşılaştırılamıyor. Bunun yerine, `geo.distance` ve `geo.intersects` işlevlerinin parametreleri olarak kullanılması gerekir. `lt` `ge`İşlevin sırasıyla ,`gt`karşılaştırma işleçlerinden biri (,, veya) kullanılarak bir uzaklık değeriyle karşılaştırılması gerekir. `le` `geo.distance` Bu kurallar, koleksiyon olmayan Edm. Geographyıpoint alanları için de geçerlidir.

Dize koleksiyonlarına benzer şekilde `Edm.GeographyPoint` , koleksiyonlar, coğrafi uzamsal işlevlerin farklı türlerde Lambda ifadelerinde nasıl kullanılabileceğini ve birleştirildiğini gösteren bazı kurallara sahiptir:

- `geo.distance` İşleviyle kullanabileceğiniz karşılaştırma işleçleri lambda ifadesinin türüne bağlıdır. İçin `any`yalnızca `lt` veya`le`kullanabilirsiniz. İçin `all`yalnızca `gt` veya`ge`kullanabilirsiniz. İfadeleri `geo.distance`içeren ifade oluşturabilirsiniz, ancak karşılaştırma işlecini değiştirmeniz gerekir `not (geo.distance(...) ge x)` (`geo.distance(...) lt x` ve `geo.distance(...) le x` olur `not (geo.distance(...) gt x)`).
- Bir `all`öğesinin gövdesinde `geo.intersects` , işlev, iç içe olmalıdır. Buna karşılık, bir `any` `geo.intersects` , gövdesinde, işlevin katlanmamalıdır.
- Bir `any`gövdesinde, coğrafi uzamsal ifadeler kullanılarak `or`birleştirilebilir. `all`' In gövdesinde, bu tür ifadeler kullanılarak `and`birleştirilebilir.

Yukarıdaki sınırlamalar, dize koleksiyonlarında eşitlik/eşitsizlik sınırlaması gibi benzer nedenlerle mevcuttur. Bu nedenlerden daha ayrıntılı bir bakış için bkz. [Azure Search OData koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md) .

Aşağıda, izin verilen `Edm.GeographyPoint` koleksiyonlara yönelik filtrelerin bazı örnekleri verilmiştir:

- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: not (geo.distance(l, geography'POINT(-122 49)') ge 10) or geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') ge 10 and not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

Koleksiyonlar için `Edm.GeographyPoint` aşağıdakiler gibi ifadelere izin verilmez:

- `locations/any(l: l eq geography'POINT(-122 49)')`
- `locations/any(l: not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') gt 10)`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10 and geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') le 10 or not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

<a name="bkmk_comparables"></a>

## <a name="rules-for-filtering-comparable-collections"></a>Karşılaştırılabilir koleksiyonları filtrelemeye yönelik kurallar

Bu bölüm aşağıdaki tüm veri türleri için geçerlidir:

- `Collection(Edm.DateTimeOffset)`
- `Collection(Edm.Double)`
- `Collection(Edm.Int32)`
- `Collection(Edm.Int64)`

`Edm.Int32` Ve gibitürler`eq`:, ,`lt`,, ve`gt`. `ne` `Edm.DateTimeOffset` `le` `ge` Bu tür koleksiyonlar üzerinde lambda ifadeleri, bu işleçlerden herhangi birini kullanan basit ifadeler içerebilir. Bu hem hem de `any` `all`için geçerlidir. Örneğin, bu filtrelere izin verilir:

- `ratings/any(r: r ne 5)`
- `dates/any(d: d gt 2017-08-24T00:00:00Z)`
- `not margins/all(m: m eq 3.5)`

Ancak, bu tür karşılaştırma ifadelerinin lambda ifadesinin içinde daha karmaşık ifadelerle nasıl birleştirilebilme konusunda sınırlamalar vardır:

- `any`Kurallar:
  - Basit eşitsizlik ifadeleri diğer ifadelerle tamamen birleştirilemez. Örneğin, bu ifadeye izin verilir:
    - `ratings/any(r: r ne 5)`

    Ancak bu ifade şu değildir:
    - `ratings/any(r: r ne 5 and r gt 2)`

    Bu ifadeye izin verildiğinde, koşullar örtüştiğinden yararlı değildir:
    - `ratings/any(r: r ne 5 or r gt 7)`
  - ,,, Veya `eq` `gt` `lt` içerenbasit`le`Karşılaştırma ifadeleri ile `and` birleştirilebilir./ `ge` `or` Örneğin:
    - `ratings/any(r: r gt 2 and r le 5)`
    - `ratings/any(r: r le 5 or r gt 7)`
  - İle `and` birleştirilen Karşılaştırma ifadeleri (yarışmalar) kullanılarak `or`daha da birleştirilebilir. Bu form, "ayırt edici[normal form](https://en.wikipedia.org/wiki/Disjunctive_normal_form)" (DNF) olarak Boole mantığındaki bilinmektedir. Örneğin:
    - `ratings/any(r: (r gt 2 and r le 5) or (r gt 7 and r lt 10))`
- `all`Kurallar:
  - Basit eşitlik ifadeleri diğer ifadelerle tamamen birleştirilemez. Örneğin, bu ifadeye izin verilir:
    - `ratings/all(r: r eq 5)`

    Ancak bu ifade şu değildir:
    - `ratings/all(r: r eq 5 or r le 2)`

    Bu ifadeye izin verildiğinde, koşullar örtüştiğinden yararlı değildir:
    - `ratings/all(r: r eq 5 and r le 7)`
  - ,,, Veya `ne` `gt` `lt` içerenbasit`le`Karşılaştırma ifadeleri ile `and` birleştirilebilir./ `ge` `or` Örneğin:
    - `ratings/all(r: r gt 2 and r le 5)`
    - `ratings/all(r: r le 5 or r gt 7)`
  - İle `or` birleştirilen Karşılaştırma ifadeleri (ayırt edici), kullanılarak `and`daha fazla birleştirilebilir. Bu form, Boole mantığındaki "[Conjunnormal form](https://en.wikipedia.org/wiki/Conjunctive_normal_form)" (CNF) olarak bilinir. Örneğin:
    - `ratings/all(r: (r le 2 or gt 5) and (r lt 7 or r ge 10))`

<a name="bkmk_complex"></a>

## <a name="rules-for-filtering-complex-collections"></a>Karmaşık koleksiyonları filtrelemeye yönelik kurallar

Karmaşık koleksiyonlar üzerinde lambda ifadeleri, ilkel türler koleksiyonları üzerinde lambda ifadelerinden çok daha esnek bir sözdizimi destekler. Bu tür bir lambda ifadesinin içinde, yalnızca iki özel durum ile kullanabileceğiniz herhangi bir filtre yapısını kullanabilirsiniz.

Birincisi, işlevler `search.ismatch` ve `search.ismatchscoring` Lambda ifadelerinde desteklenmez. Daha fazla bilgi için bkz. [Azure Search OData koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).

İkinci olarak, Aralık değişkenine (yani, bilinen *ücretsiz değişkenler*) *bağlanmamış* alanlara başvurmak için izin verilmez. Örneğin, aşağıdaki iki denk OData filtre ifadesini göz önünde bulundurun:

1. `stores/any(s: s/amenities/any(a: a eq 'parking')) and details/margin gt 0.5`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and details/margin gt 0.5))`

Aralık değişkenine `details/margin` `s`bağlanmadığı için ikinci form reddedildiği için ilk ifadeye izin verilir.

Bu kural ayrıca bir dış kapsamda değişken bağlanan ifadelere da genişletilir. Bu tür değişkenler göründükleri kapsama göre ücretsizdir. Örneğin, ilk ifadeye izin verilir, ancak ikinci denk ifadeye izin verilmez, çünkü `s/name` Aralık değişkeninin `a`kapsamına göre ücretsizdir:

1. `stores/any(s: s/amenities/any(a: a eq 'parking') and s/name ne 'Flagship')`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and s/name ne 'Flagship'))`

Lambda ifadeleri yalnızca ilgili değişkenleri içermesi gibi filtrelerin oluşturulması her zaman mümkün olduğundan, bu sınırlama uygulamada bir sorun olması gerekmez.

## <a name="cheat-sheet-for-collection-filter-rules"></a>Koleksiyon filtresi kuralları için bir sayfa sayfası

Aşağıdaki tabloda, her koleksiyon veri türü için geçerli Filtreler oluşturma kuralları özetlenmektedir.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Her durum için geçerli filtrelerin nasıl oluşturulacağı hakkında örnekler için bkz. [geçerli koleksiyon filtreleri yazma](#bkmk_examples).

Filtreleri sıklıkla yazarsanız ve ilk ilkeden kuralları anlamak, bunları çok az hale getirmenize yardımcı olabilir, bkz. [Azure Search OData koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search 'de OData koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md)
- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
