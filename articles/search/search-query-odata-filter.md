---
title: OData filtresi başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında filtre ifadeleri oluşturmak için kullanılan OData dili başvurusu ve tam sözdizimi.
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
ms.openlocfilehash: b966e9cfa3ef40666dbbd62135f8f964e5eb2023
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282893"
---
# <a name="odata-filter-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da Sözdizimini $filter Veri

Azure Bilişsel Arama, tam metin arama terimlerinin yanı sıra bir arama sorgusuna ek ölçütler uygulamak için [OData filtre ifadelerini](query-odata-filter-orderby-syntax.md) kullanır. Bu makalede, filtrelerin sözdizimini ayrıntılı olarak açıklanır. Filtrelerin ne olduğu ve belirli sorgu senaryolarını gerçekleştirmek için bunların nasıl kullanılacağı hakkında daha genel bilgi için [Azure Bilişsel Arama'daki Filtreler'e](search-filters.md)bakın.

## <a name="syntax"></a>Sözdizimi

OData dilindeki bir filtre, aşağıdaki EBNF[(Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)tarafından gösterildiği gibi, sırayla çeşitli ifade türlerinden biri olabilecek bir Boolean ifadesidir:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#boolean_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Boolean ifadelerinin türleri şunlardır:

- Toplama filtresi ifadeleri `any` `all`kullanarak veya . Bunlar toplama alanlarına filtre ölçütleri uygular. Daha fazla bilgi için [Azure Bilişsel Arama'daki OData toplama operatörlerine](search-query-odata-collection-operators.md)bakın.
- İşleçleri `and`kullanarak diğer Boolean ifadelerini `or`birleştiren `not`mantıksal ifadeler , ve. Daha fazla bilgi için [Azure Bilişsel Arama'daki OData mantıksal işleçleri'ne](search-query-odata-logical-operators.md)bakın.
- Alanları veya aralık `eq`değişkenlerini, işleçler , , `ne` `gt`, `lt` `ge`, ve `le`. Daha fazla bilgi için [Azure Bilişsel Arama'daki OData karşılaştırma işleçleri'ne](search-query-odata-comparison-operators.md)bakın. Karşılaştırma ifadeleri, işlevi kullanarak coğrafi uzamsal koordinatlar `geo.distance` arasındaki mesafeleri karşılaştırmak için de kullanılır. Daha fazla bilgi için Azure [Bilişsel Arama'da OData coğrafi uzamsal işlevlerine](search-query-odata-geo-spatial-functions.md)bakın.
- Boolean edebi `true` ve. `false` Bu sabitler bazen filtreleri programlı olarak oluştururken yararlı olabilir, ancak bunun dışında uygulamada kullanılma yatkınlığı yoktur.
- Boolean işlevlerine yapılan aramalar:
  - `geo.intersects`, belirli bir noktanın belirli bir çokgen içinde olup olmadığını test eden. Daha fazla bilgi için Azure [Bilişsel Arama'da OData coğrafi uzamsal işlevlerine](search-query-odata-geo-spatial-functions.md)bakın.
  - `search.in`, bir alan veya aralık değişkenini değerler listesindeki her değerle karşılaştıran. Daha fazla bilgi için [Azure Bilişsel Arama'da `search.in` OData işlevine](search-query-odata-search-in-function.md)bakın.
  - `search.ismatch`ve `search.ismatchscoring`tam metin arama işlemlerini filtre bağlamında yürüten. Daha fazla bilgi için [Azure Bilişsel Arama'da OData tam metin arama işlevlerine](search-query-odata-full-text-search-functions.md)bakın.
- Alan yolları veya tür `Edm.Boolean`aralık değişkenleri . Örneğin, dizininizin bir Boolean alanı `IsEnabled` varsa ve tüm belgeleri bu `true`alanın olduğu yerde döndürmek istiyorsanız, filtre ifadeniz sadece ad `IsEnabled`olabilir.
- Parantez içinde boolean ifadeler. Parantez kullanmak, bir filtredeki işlemlerin sırasını açıkça belirlemeye yardımcı olabilir. OData işleçlerinin varsayılan önceliği hakkında daha fazla bilgi için bir sonraki bölüme bakın.

### <a name="operator-precedence-in-filters"></a>Filtrelerde operatör önceliği

Alt ifadelerinin etrafında parantez olmayan bir filtre ifadesi yazarsanız, Azure Bilişsel Arama bu ifadeyi bir dizi işleç önceliği kuralına göre değerlendirir. Bu kurallar, alt ifadeleri birleştirmek için hangi işleçlerin kullanıldığına dayanır. Aşağıdaki tabloda işleç grupları en yüksekten en düşük önceliğe doğru sıralanır:

| Grup | Operatör(ler) |
| --- | --- |
| Mantıksal işleçler | `not` |
| Karşılaştırma işleçleri | `eq`, `ne`, `gt`, `lt`, `ge`, `le` |
| Mantıksal işleçler | `and` |
| Mantıksal işleçler | `or` |

Yukarıdaki tabloda daha yüksek olan bir işleç, diğer işleçlere göre operandlarına "daha sıkı bağlanır". Örneğin, `and` karşılaştırma işleçleri `or`her ikisinden de daha yüksek önceliğe sahip olduğundan, aşağıdaki iki ifade eşdeğerdir:

    Rating gt 0 and Rating lt 3 or Rating gt 7 and Rating lt 10
    ((Rating gt 0) and (Rating lt 3)) or ((Rating gt 7) and (Rating lt 10))

Operatör, `not` karşılaştırma işleçlerinden bile daha yüksek olan en yüksek önceliğe sahiptir. Bu yüzden böyle bir filtre yazmaya çalışırsanız:

    not Rating gt 5

Bu hata iletisi alırsınız:

    Invalid expression: A unary operator with an incompatible type was detected. Found operand type 'Edm.Int32' for operator kind 'Not'.

Bu hata, işleç, karşılaştırma `Rating` ifadesinin tamamıyla `Edm.Int32`değil, türünde olan alanla ilişkili olduğundan hata olur. Düzeltme parantez `not` içinde operand koymaktır:

    not (Rating gt 5)

<a name="bkmk_limits"></a>

### <a name="filter-size-limitations"></a>Filtre boyutu sınırlamaları

Azure Bilişsel Arama'ya gönderebileceğiniz filtre ifadelerinin boyutu ve karmaşıklığının sınırları vardır. Sınırlar kabaca filtre ifadenizdeki yan tümce sayısına dayanır. İyi bir kılavuz, yüzlerce yan tümceniz varsa, sınırı aşma riskiyle karşı karşı nıza gelmiş olmasıdır. Uygulamanızı sınırsız boyutta filtreler oluşturacak şekilde tasarlamanızı öneririz.

> [!TIP]
> İşlev çağrısı tek bir yan tümce olarak saydığından, işlevin uzun eşitlik karşılaştırmaları yerine kullanılması filtre yan tümcesi sınırını önlemeye yardımcı olabilir. [ `search.in` ](search-query-odata-search-in-function.md)

## <a name="examples"></a>Örnekler

4'te veya üzerinde puan alan taban fiyatı 200 TL'den az olan en az bir odalı tüm otelleri bulun:

    $filter=Rooms/any(room: room/BaseRate lt 200.0) and Rating ge 4

2010 yılından beri yenilenen "Sea View Motel" dışındaki tüm otelleri bulun:

    $filter=HotelName ne 'Sea View Motel' and LastRenovationDate ge 2010-01-01T00:00:00Z

2010 veya daha sonra yenilenen tüm otelleri bulun. Datetime literal, Pasifik Standart Saati için saat dilimi bilgilerini içerir:  

    $filter=LastRenovationDate ge 2010-01-01T00:00:00-08:00

Otoparkı olan ve tüm odaların sigara içilmeyen olduğu tüm otelleri bulun:

    $filter=ParkingIncluded and Rooms/all(room: not room/SmokingAllowed)

 \-VEYA -  

    $filter=ParkingIncluded eq true and Rooms/all(room: room/SmokingAllowed eq false)

Lüks olan veya otopark içeren ve 5 puana sahip tüm otelleri bulun:  

    $filter=(Category eq 'Luxury' or ParkingIncluded eq true) and Rating eq 5

En az bir odada "wifi" etiketine sahip tüm otelleri bulun (her odanın bir `Collection(Edm.String)` tarlada saklanan etiketleri vardır):  

    $filter=Rooms/any(room: room/Tags/any(tag: tag eq 'wifi'))

Herhangi bir oda ile tüm oteller bulun:  

    $filter=Rooms/any()

Odası olmayan tüm otelleri bulun:

    $filter=not Rooms/any()

Belirli bir referans noktası (türü bir alan `Location` `Edm.GeographyPoint`nerede): 10 kilometre içinde tüm oteller bulun:

    $filter=geo.distance(Location, geography'POINT(-122.131577 47.678581)') le 10

Çokgen olarak tanımlanan belirli bir görünüm portu `Location` içindeki tüm otelleri bulun (Edm.GeographyPoint türünde bir alan dır). Çokgen kapatılmalıdır, yani ilk ve son nokta kümeleri aynı olmalıdır. Ayrıca, [noktalar saat yönünün tersine sırayla listelenmelidir.](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1)

    $filter=geo.intersects(Location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

"Açıklama" alanının boş olduğu tüm otelleri bulun. Alan hiç ayarlanmazsa veya açıkça null olarak ayarlanmışsa geçersiz olur:  

    $filter=Description eq null

'Sea View motel' veya 'Budget hotel' adlarına eşit olan tüm otelleri bulun). Bu tümcecikler boşluklar içerir ve boşluk varsayılan bir sınır layıcıdır. Üçüncü dize parametresi olarak tek tırnak içinde alternatif bir sınırlayıcı belirtebilirsiniz:  

    $filter=search.in(HotelName, 'Sea View motel,Budget hotel', ',')

'Sea View motel' veya 'Budget hotel' ile ayrılmış ada sahip tüm otelleri bulun:  

    $filter=search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Tüm odaların 'wifi' veya 'küvet' etiketine sahip olduğu tüm otelleri bulun:

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub'))

Etiketlerdeki 'ısıtmalı havlu rafları' veya 'saç kurutma makinesi dahil' gibi koleksiyondaki ifadelerde bir eşleşme bulun.

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

"Rıhtım" sözcüğünün yer aldığı belgeleri bulun. Bu filtre [search request](https://docs.microsoft.com/rest/api/searchservice/search-documents) `search=waterfront`sorgusu, '.

    $filter=search.ismatchscoring('waterfront')

"Hostel" sözcüğünün ve 4'ün üzerinde veya eşit olan belgeleri veya "motel" sözcüğü ne kadar yüksek hem de 5'e eşit olan belgeleri bulun. Tam metin aramaile filtre `search.ismatchscoring` işlemlerini birleştirdiği `or`için bu istek işlev olmadan ifade edilemezdi.

    $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

"Lüks" kelimesi olmadan belgeleri bulun.

    $filter=not search.ismatch('luxury')

"Okyanus görünümü" veya 5'e eşit derecelendirmesi olan belgeleri bulun. Sorgu `search.ismatchscoring` yalnızca alanlara `HotelName` karşı `Description`yürütülür ve . Disjunction'ın yalnızca ikinci maddesine uyan belgeler de iade `Rating` edilecek - 5'e eşit oteller. Bu belgeler, ifadenin puanlı kısımlarıyla eşleşmediğini açıkça belirtmek için sıfıra eşit puanla iade edilecektir.

    $filter=search.ismatchscoring('"ocean view"', 'Description,HotelName') or Rating eq 5

"Otel" ve "havaalanı" terimlerinin tanımda en fazla beş kelime ayrı kaldığı ve tüm odaların sigara içilmediği otelleri bulun. Bu sorgu [tam Lucene sorgu dilini](query-lucene-syntax.md)kullanır.

    $filter=search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and not Rooms/any(room: room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
