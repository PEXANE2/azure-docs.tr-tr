---
title: Sorun giderme OData toplama filtreleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData toplama filtresi hatalarını çözmek için yaklaşımları öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
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
ms.openlocfilehash: e82fa00226c964d5ba774cdf06f5b0f3898bdc55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113086"
---
# <a name="troubleshooting-odata-collection-filters-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da OData toplama filtrelerini sorun giderme

Azure Bilişsel Arama'daki toplama alanlarına [filtre](query-odata-filter-orderby-syntax.md) uygulayabilmek için **lambda ifadeleri**ile [ `any` birlikte ve `all` işleçler](search-query-odata-collection-operators.md) kullanabilirsiniz. Lambda ifadesi, koleksiyonun her öğesine uygulanan bir alt filtredir.

Filtre ifadelerinin her özelliği lambda ifadesinin içinde bulunmaz. Hangi özelliklerin kullanılabildiği, filtrelemek istediğiniz toplama alanının veri türüne bağlı olarak değişir. Bu, bu bağlamda desteklenmeyen bir lambda ifadesinde bir özellik kullanmaya çalışırsanız, bu bir hataya neden olabilir. Koleksiyon alanları üzerinde karmaşık bir filtre yazmaya çalışırken bu tür hatalarla karşılaşıyorsanız, bu makale sorunu gidermenize yardımcı olur.

## <a name="common-collection-filter-errors"></a>Sık toplama filtresi hataları

Aşağıdaki tabloda, bir koleksiyon filtresini yürütmeye çalışırken karşılaşabileceğiniz hatalar listelenizdir. Bu hatalar, lambda ifadesi içinde desteklenmeyen filtre ifadeleri özelliğini kullandığınızda olur. Her hata, hatayı önlemek için filtrenizi nasıl yeniden yazabileceğinize dair bazı kılavuzlar verir. Tablo da bu hataönlemek için nasıl daha fazla bilgi sağlayan bu makalenin ilgili bölümüne bir bağlantı içerir.

| Hata iletisi | Durum | Daha fazla bilgi için bkz. |
| --- | --- | --- |
| 'ismatch' işlevinin aralık değişkeni 's'e bağlı parametreleri yoktur. Lambda ifadeleri ('any' veya 'tümü') içinde yalnızca bağlı alan referansları desteklenir. Lütfen filtrenizi değiştirin, böylece 'ismatch' işlevi lambda ifadesinin dışındadır ve yeniden deneyin. | Lambda ifadesini kullanma `search.ismatch` veya `search.ismatchscoring` içinde | [Karmaşık koleksiyonları filtreleme kuralları](#bkmk_complex) |
| Geçersiz lambda ifadesi. Bir lambda ifadesinde, toplama türü (Edm.String) bir alan üzerinde yineleyen bir ifadede tam tersinin beklendiği eşitlik veya eşitsizlik için bir test bulundu. 'Any' için lütfen 'x eq y' veya 'search.in(...)' formunun ifadelerini kullanın. 'All' için lütfen 'x ne y', 'not (x eq y)', "not search.in(...)" formlarının ifadelerini kullanın. | Tür alanında filtreleme`Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |
| Geçersiz lambda ifadesi. Karmaşık Boolean ifadesinin desteklenmeyen bir biçimini buldum. 'Herhangi' için, lütfen Disjunctive Normal Form olarak da bilinen 'AND'lerin OR'ları' ifadelerini kullanın. Örneğin: '(a ve b) veya (c ve d)' nerede a, b, c ve d karşılaştırma veya eşitlik alt ifadeleri vardır. 'Tüm' için, konjonktürel normal form olarak da bilinen 'ORs AND'leri' olan ifadeleri kullanın. Örneğin: '(a veya b) ve (c veya d)' nerede a, b, c ve d karşılaştırma veya eşitsizlik alt ifadeler vardır. Karşılaştırma ifadelerine örnekler: 'x gt 5', 'x le 2'. Eşitlik ifadesi örneği: 'x eq 5'. Eşitsizlik ifadesine örnek: 'x ne 5'. | Tür `Collection(Edm.DateTimeOffset)`, , , `Collection(Edm.Double)` `Collection(Edm.Int32)`veya alanlarda filtreleme`Collection(Edm.Int64)` | [Karşılaştırılabilir koleksiyonları filtreleme kuralları](#bkmk_comparables) |
| Geçersiz lambda ifadesi. Bir lambda ifadesinde geo.distance() veya geo.intersects() (Edm.GeographyPoint) türünden bir alan üzerinde titreşen desteklenmeyen bir kullanım bulundu. 'Any' için, 'lt' veya 'le' işleçlerini kullanarak geo.distance() ile karşılaştırdığınızdan ve geo.intersects() kullanımının inkar olmadığından emin olun. 'Tüm' için,'gt' veya 'ge' işleçlerini kullanarak geo.distance() ile karşılaştırdığınızdan ve geo.intersects() kullanımının inkâr olduğundan emin olun. | Tür alanında filtreleme`Collection(Edm.GeographyPoint)` | [GeographyPoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints) |
| Geçersiz lambda ifadesi. Karmaşık Boolean ifadeleri, koleksiyon türü (Edm.GeographyPoint) alanları üzerinde yineleyen lambda ifadelerinde desteklenmez. 'Any' için lütfen alt ifadeleri 'veya' ile birleştirin; 've' desteklenmez. 'All' için lütfen alt ifadelere 've' ile katılın; 'veya' desteklenmez. | Tür `Collection(Edm.String)` veya alanlarda filtreleme`Collection(Edm.GeographyPoint)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) <br/><br/> [GeographyPoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints) |
| Geçersiz lambda ifadesi. Bir karşılaştırma işleci ('lt', 'le', 'gt'veya 'ge') bulundu. Yalnızca eşitlik işleçleri, toplama türü alanları (Edm.String) üzerinde yineleyen lambda ifadelerinde izin verilir. 'Any' için lütfen 'x eq y' formunun ifadelerini kullanın. 'All' için lütfen 'x ne y' veya 'değil (x eq y)' formunun ifadelerini kullanın. | Tür alanında filtreleme`Collection(Edm.String)` | [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings) |

<a name="bkmk_examples"></a>

## <a name="how-to-write-valid-collection-filters"></a>Geçerli toplama filtreleri nasıl yazılır?

Geçerli toplama filtreleri yazma kuralları her veri türü için farklıdır. Aşağıdaki bölümler, hangi filtre özelliklerinin desteklendiği ve hangilerinin desteklenmiyor örneklerini göstererek kuralları açıklar:

- [Dize koleksiyonlarını filtreleme kuralları](#bkmk_strings)
- [Boolean koleksiyonlarını filtreleme kuralları](#bkmk_bools)
- [GeographyPoint koleksiyonlarını filtreleme kuralları](#bkmk_geopoints)
- [Karşılaştırılabilir koleksiyonları filtreleme kuralları](#bkmk_comparables)
- [Karmaşık koleksiyonları filtreleme kuralları](#bkmk_complex)

<a name="bkmk_strings"></a>

## <a name="rules-for-filtering-string-collections"></a>Dize koleksiyonlarını filtreleme kuralları

Dize koleksiyonları için lambda ifadeleri içinde, kullanılabilecek tek `eq` karşılaştırma `ne`işleçleri ve .

> [!NOTE]
> Azure Bilişsel Arama, `lt` / `le` / `gt` / `ge` bir lambda ifadesinin içinde veya dışında olsun, dizeleri için operatörleri desteklemez.

Bir `any` vücut sadece eşitlik için test edebilirsiniz `all` ise bir vücut sadece eşitsizlik için test edebilirsiniz.

Ayrıca birden fazla `or` ifadeyi bir `any`' in `and` gövdesinde ve bir `all`. `search.in` Fonksiyon eşitlik denetimleri ile `or`birleştirerek eşdeğer olduğundan, aynı zamanda bir `any`vücutta izin verilir . Tersine, `not search.in` bir `all`vücutta izin verilir .

Örneğin, bu ifadelere izin verilir:

- `tags/any(t: t eq 'books')`
- `tags/any(t: search.in(t, 'books, games, toys'))`
- `tags/all(t: t ne 'books')`
- `tags/all(t: not (t eq 'books'))`
- `tags/all(t: not search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' or t eq 'games')`
- `tags/all(t: t ne 'books' and not (t eq 'games'))`

bu ifadelere izin verilmezken:

- `tags/any(t: t ne 'books')`
- `tags/any(t: not search.in(t, 'books, games, toys'))`
- `tags/all(t: t eq 'books')`
- `tags/all(t: search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' and t ne 'games')`
- `tags/all(t: t ne 'books' or not (t eq 'games'))`

<a name="bkmk_bools"></a>

## <a name="rules-for-filtering-boolean-collections"></a>Boolean koleksiyonlarını filtreleme kuralları

Türü `Edm.Boolean` yalnızca ve `eq` `ne` işleçleri destekler. Bu nedenle, her zaman tautologies veya çelişkilere yol açacak `and` / `or` beri aynı aralık değişkeni kontrol bu tür yan tümceleri birleştirerek izin vermek çok mantıklı değildir.

Boolean koleksiyonlarında izin verilen bazı filtreler örnekleri aşağıda verilmiştir:

- `flags/any(f: f)`
- `flags/all(f: f)`
- `flags/any(f: f eq true)`
- `flags/any(f: f ne true)`
- `flags/all(f: not f)`
- `flags/all(f: not (f eq true))`

String koleksiyonlarının aksine, Boolean koleksiyonlarının hangi işlecinin hangi lambda ifadesinde kullanabileceği konusunda bir sınırlaması yoktur. Her `eq` `ne` ikisi de ve vücutta `any` `all`kullanılabilir veya .

Boolean koleksiyonları için aşağıdaki ifadelere izin verilmez:

- `flags/any(f: f or not f)`
- `flags/any(f: f or f)`
- `flags/all(f: f and not f)`
- `flags/all(f: f and f eq true)`

<a name="bkmk_geopoints"></a>

## <a name="rules-for-filtering-geographypoint-collections"></a>GeographyPoint koleksiyonlarını filtreleme kuralları

Koleksiyondaki `Edm.GeographyPoint` tür değerleri doğrudan birbiriyle karşılaştırılamaz. Bunun yerine, bunlar parametreler `geo.distance` ve `geo.intersects` işlevler olarak kullanılmalıdır. `geo.distance` Sırayla `lt`fonksiyon karşılaştırma işleçlerinden biri kullanılarak bir mesafe `le` `gt`değeri `ge`ile karşılaştırıldığında olmalıdır , , veya . Bu kurallar, koleksiyona ekilmeyen Edm.GeographyPoint alanları için de geçerlidir.

Dize koleksiyonları `Edm.GeographyPoint` gibi, koleksiyonların da farklı lambda ifadelerinde coğrafi uzamsal işlevlerin nasıl kullanılabileceğini ve birleştirebileceğine dair bazı kuralları vardır:

- `geo.distance` İşlevle hangi karşılaştırma işleçlerini kullanabileceğiniz lambda ifadesinin türüne bağlıdır. Için `any`, yalnızca `lt` kullanabilirsiniz veya `le`. Için `all`, yalnızca `gt` kullanabilirsiniz veya `ge`. Içeren ifadeleri inkâr `geo.distance`edebilirsiniz, ancak karşılaştırma işleci değiştirmeniz `not (geo.distance(...) ge x)` `geo.distance(...) le x` gerekir `not (geo.distance(...) gt x)`(`geo.distance(...) lt x` olur ve olur).
- Bir `all`vücutta , `geo.intersects` fonksiyon inkar edilmelidir. Tersine, bir `any`vücutta , `geo.intersects` fonksiyon inkar edilmemelidir.
- Bir `any`, jeo-uzamsal ifadeler gövdesinde `or`kullanılarak kombine edilebilir. Bir `all`vücutta , bu tür ifadeler kullanılarak `and`kombine edilebilir.

Yukarıdaki sınırlamalar dize koleksiyonlarında eşitlik/eşitsizlik sınırlaması gibi benzer nedenlerle vardır. Bu nedenlere daha derinlemesine bakmak için [Azure Bilişsel Arama'daki OData toplama filtrelerini anlama](search-query-understand-collection-filters.md) bilginle bakın.

Koleksiyonlarda `Edm.GeographyPoint` izin verilen bazı filtreler örnekleri aşağıda verilmiştir:

- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: not (geo.distance(l, geography'POINT(-122 49)') ge 10) or geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') ge 10 and not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

Koleksiyonlar için `Edm.GeographyPoint` aşağıdaki ifadelere izin verilmez:

- `locations/any(l: l eq geography'POINT(-122 49)')`
- `locations/any(l: not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') gt 10)`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10 and geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') le 10 or not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

<a name="bkmk_comparables"></a>

## <a name="rules-for-filtering-comparable-collections"></a>Karşılaştırılabilir koleksiyonları filtreleme kuralları

Bu bölüm, aşağıdaki tüm veri türleri için geçerlidir:

- `Collection(Edm.DateTimeOffset)`
- `Collection(Edm.Double)`
- `Collection(Edm.Int32)`
- `Collection(Edm.Int64)`

Karşılaştırma işleçlerinin altısını da destekler `eq` `ne`ve `lt` `Edm.DateTimeOffset` `le`bunlar: `Edm.Int32` `ge`, , , `gt`, ve . Bu tür koleksiyonları üzerinde Lambda ifadeler bu işleçlerden herhangi birini kullanarak basit ifadeler içerebilir. Bu hem de `any` `all`geçerlidir . Örneğin, bu filtrelere izin verilir:

- `ratings/any(r: r ne 5)`
- `dates/any(d: d gt 2017-08-24T00:00:00Z)`
- `not margins/all(m: m eq 3.5)`

Ancak, bu tür karşılaştırma ifadelerinin lambda ifadesi içinde daha karmaşık ifadeler de nasıl birleştirilebildiği konusunda sınırlamalar vardır:

- Kurallar `any`için :
  - Basit eşitsizlik ifadeleri yararlı başka ifadeler ile birleştirilmez. Örneğin, bu ifadeye izin verilir:
    - `ratings/any(r: r ne 5)`

    ancak bu ifade şu değildir:
    - `ratings/any(r: r ne 5 and r gt 2)`

    ve bu ifadeye izin verilirken, koşullar çakıştığı için kullanışlı değildir:
    - `ratings/any(r: r ne 5 or r gt 7)`
  - Içeren basit karşılaştırma `eq` `lt`ifadeleri `le` `gt`, `ge` , , `and` / `or`, veya . Örnek:
    - `ratings/any(r: r gt 2 and r le 5)`
    - `ratings/any(r: r le 5 or r gt 7)`
  - Karşılaştırma ifadeleri ile `and` birlikte (bağlaçlar) `or`daha fazla kullanılarak kombine edilebilir. Bu form Boolean mantığında "[Disjunctive Normal Form](https://en.wikipedia.org/wiki/Disjunctive_normal_form)" (DNF) olarak bilinir. Örnek:
    - `ratings/any(r: (r gt 2 and r le 5) or (r gt 7 and r lt 10))`
- Kurallar `all`için :
  - Basit eşitlik ifadeleri yararlı başka ifadeler ile birleştirilmez. Örneğin, bu ifadeye izin verilir:
    - `ratings/all(r: r eq 5)`

    ancak bu ifade şu değildir:
    - `ratings/all(r: r eq 5 or r le 2)`

    ve bu ifadeye izin verilirken, koşullar çakıştığı için kullanışlı değildir:
    - `ratings/all(r: r eq 5 and r le 7)`
  - Içeren basit karşılaştırma `ne` `lt`ifadeleri `le` `gt`, `ge` , , `and` / `or`, veya . Örnek:
    - `ratings/all(r: r gt 2 and r le 5)`
    - `ratings/all(r: r le 5 or r gt 7)`
  - Karşılaştırma ifadeleri ile `or` birlikte (disjunctions) daha `and`fazla kullanılarak kombine edilebilir. Bu form Boolean mantığında "[Konjonktif Normal Form](https://en.wikipedia.org/wiki/Conjunctive_normal_form)" (CNF) olarak bilinir. Örnek:
    - `ratings/all(r: (r le 2 or gt 5) and (r lt 7 or r ge 10))`

<a name="bkmk_complex"></a>

## <a name="rules-for-filtering-complex-collections"></a>Karmaşık koleksiyonları filtreleme kuralları

Karmaşık koleksiyonlar üzerindeki Lambda ifadeleri, ilkel türkoleksiyonlarına göre lambda ifadelerinden çok daha esnek bir sözdizimini destekler. Sadece iki istisna dışında kullanabileceğiniz böyle bir lambda ifadesi içinde herhangi bir filtre yapısı kullanabilirsiniz.

İlk olarak, `search.ismatch` `search.ismatchscoring` işlevler ve lambda ifadeler içinde desteklenmez. Daha fazla bilgi için Azure [Bilişsel Arama'daki OData toplama filtrelerini anlama 'ya](search-query-understand-collection-filters.md)bakın.

İkinci olarak, aralık değişkenine *bağlı* olmayan alanlara *(serbest değişken*ler olarak adlandırılır) başvuru yada izin verilmez. Örneğin, aşağıdaki iki eşdeğer OData filtre ifadesini göz önünde bulundurun:

1. `stores/any(s: s/amenities/any(a: a eq 'parking')) and details/margin gt 0.5`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and details/margin gt 0.5))`

İlk ifadeye izin verilirken, ikinci form `details/margin` aralık değişkenine `s`bağlı olmadığından reddedilir.

Bu kural, dış kapsamda bağlı değişkenleri olan ifadeleri de genişletir. Bu tür değişkenler göründükleri kapsama göre ücretsizdir. Örneğin, ilk ifadeye izin verilirken, ikinci eşdeğer ifadeye izin verilmez, çünkü `s/name` aralık `a`değişkeninin kapsamına göre ücretsizdir:

1. `stores/any(s: s/amenities/any(a: a eq 'parking') and s/name ne 'Flagship')`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and s/name ne 'Flagship'))`

Lambda ifadeleri yalnızca bağlı değişkenler içeren filtreler oluşturmak her zaman mümkün olduğundan, bu sınırlama uygulamada bir sorun olmamalıdır.

## <a name="cheat-sheet-for-collection-filter-rules"></a>Toplama filtresi kuralları için hile sayfası

Aşağıdaki tablo, her toplama veri türü için geçerli filtreler oluşturma kurallarını özetler.

[!INCLUDE [Limitations on OData lambda expressions in Azure Cognitive Search](../../includes/search-query-odata-lambda-limitations.md)]

Her servis talebi için geçerli filtrelerin nasıl oluşturabildiğini örnekler için geçerli [koleksiyon filtrelerinin nasıl yazılmasına](#bkmk_examples)bakın.

Filtreleri sık sık yazarsanız ve kuralları ilk ilkelerden anlamak, bunları ezberlemekten daha fazla yardımcı olur, [bkz.](search-query-understand-collection-filters.md)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'da OData toplama filtrelerini anlama](search-query-understand-collection-filters.md)
- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
