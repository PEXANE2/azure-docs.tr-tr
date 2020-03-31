---
title: OData diline genel bakış
titleSuffix: Azure Cognitive Search
description: Filtreler, Azure Bilişsel Arama sorguları için seçme ve sipariş için OData diline genel bakış.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/10/2020
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
ms.openlocfilehash: f3a1be435e297ab4a9ba7f8bfbd5f3ce3451d8a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77153885"
---
# <a name="odata-language-overview-for-filter-orderby-and-select-in-azure-cognitive-search"></a>Azure Bilişsel Arama `$filter` `$orderby`için `$select` ve Azure Bilişsel Arama'ya Genel Bakış

Azure Bilişsel Arama, **$filter,** **$orderby**ve **$select** ifadeler için OData ifade sözdiziminin bir alt kümesini destekler. Filtre ifadeleri sorgu ayrıştma, aramayı belirli alanlara zorlama veya dizin taramaları sırasında kullanılan eşleme ölçütleri ekleme sırasında değerlendirilir. Sipariş eki ifadeler, döndürülen belgeleri sıralamak için ayarlanmış bir sonuç üzerinde işlem sonrası bir adım olarak uygulanır. İfadeleri seçin sonuç kümesine hangi belge alanlarının dahil edildiğini belirler. Başvuru alanları için sözdiziminde bazı çakışma olmasına rağmen, bu ifadelerin sözdizimi, **arama** parametresinde kullanılan [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) sorgu sözdiziminden farklıdır.

Bu makalede, filtreler, sıralı ve select ifadelerde kullanılan OData ifade dilinin genel bir özetini sağlar. Dil en temel unsurlardan başlayıp üzerlerinde inşa edilen "aşağıdan yukarıya" sunulur. Her parametre için üst düzey sözdizimi ayrı bir makalede açıklanmıştır:

- [$filter sözdizimi](search-query-odata-filter.md)
- [$orderby sözdizimi](search-query-odata-orderby.md)
- [$select sözdizimi](search-query-odata-select.md)

OData ifadeleri basitten son derece karmaşıka kadar değişir, ancak hepsi ortak öğeleri paylaşır. Azure Bilişsel Arama'daki OData ifadesinin en temel bölümleri şunlardır:

- Dizininizin belirli alanlarına başvuran **alan yolları.**
- **Sabitler,** belirli bir veri türünün gerçek değerleridir.

> [!NOTE]
> Azure Bilişsel Arama'daki terminoloji birkaç yönden [OData standardından](https://www.odata.org/documentation/) farklıdır. Azure Bilişsel Arama'da **alan** dediğimiz şeye OData'da özellik denir ve benzer şekilde **alan yoluna** karşı **özellik yolu**için de adlandırılır. **property** Azure Bilişsel Arama'da **belgeleri** içeren bir **dizin,** odata'da daha genel olarak **varlıklar**içeren bir **varlık kümesi** olarak adlandırılır. Azure Bilişsel Arama terminolojisi bu başvuru boyunca kullanılır.

## <a name="field-paths"></a>Saha yolları

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)alan yollarının gramerini tanımlar.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
field_path ::= identifier('/'identifier)*

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#field_path)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Bir alan yolu kesikler ile ayrılmış bir veya daha fazla **tanımlayıcıdan** oluşur. Her tanımlayıcı, ascii harfi veya alt rakamıile başlaması gereken ve yalnızca ASCII harfleri, rakamları veya alt çiziyi içeren bir karakter dizisidir. Harfler üst veya küçük harf olabilir.

Tanımlayıcı, bir alanın adına veya bir filtredeki [koleksiyon ifadesi](search-query-odata-collection-operators.md) `any` `all`bağlamında bir **aralık değişkenine** başvurabilir. Aralık değişkeni, koleksiyonun geçerli öğesini temsil eden bir döngü değişkeni gibidir. Karmaşık koleksiyonlar için, bu değişken bir nesneyi temsil eder, bu nedenle değişkenin alt alanlarına başvurmak için alan yollarını kullanabilirsiniz. Bu, birçok programlama dilinde nokta gösterimi benzer.

Alan yolları örnekleri aşağıdaki tabloda gösterilmiştir:

| Alan yolu | Açıklama |
| --- | --- |
| `HotelName` | Dizinin üst düzey bir alanını ifade eder |
| `Address/City` | Dizindeki `City` karmaşık bir alanın alt alanını ifade eder; `Address` bu örnekte tür `Edm.ComplexType` |
| `Rooms/Type` | Dizindeki `Type` karmaşık bir toplama alanının alt alanını ifade eder; `Rooms` bu örnekte tür `Collection(Edm.ComplexType)` |
| `Stores/Address/Country` | Dizindeki `Country` karmaşık bir toplama `Address` alanının alt alanının alt alanını ifade eder; `Stores` `Collection(Edm.ComplexType)` türü `Address` ve bu örnekte türüdür `Edm.ComplexType` |
| `room/Type` | `room` Aralık değişkeninin `Type` alt alanını ifade eder, örneğin filtre ifadesinde`Rooms/any(room: room/Type eq 'deluxe')` |
| `store/Address/Country` | Aralık değişkeninin `Country` `Address` alt alanının alt alanını ifade eder, örneğin filtre ifadesinde `store``Stores/any(store: store/Address/Country eq 'Canada')` |

Alan yolunun anlamı içeriğe bağlı olarak değişir. Filtrelerde, alan yolu geçerli belgedeki tek bir alanın *örneğinin* değerini ifade eder. Diğer bağlamlarda, **$orderby**gibi, **$select,** veya [tam Lucene sözdizimi alanlı arama](query-lucene-syntax.md#bkmk_fields), bir alan yolu alanın kendisi anlamına gelir. Bu fark, filtrelerde alan yollarını kullanma şekliniz için bazı sonuçlar doğurur.

Alan yolunu `Address/City`düşünün. Bir filtrede, bu geçerli belge için "San Francisco" gibi tek bir şehir anlamına gelir. Buna karşılık, `Rooms/Type` birçok `Type` oda için alt alan anlamına gelir (ilk oda için "standart" gibi, ikinci oda için "deluxe" ve benzeri). Alt `Rooms/Type` alanın `Type`tek bir *örneğine* atıfta bulunmadığından, doğrudan bir filtrede kullanılamaz. Bunun yerine, oda türüne filtre uygulayacak şekilde, aralık değişkenine sahip bir [lambda ifadesi](search-query-odata-collection-operators.md) kullanırsınız:

    Rooms/any(room: room/Type eq 'deluxe')

Bu örnekte, aralık `room` değişkeni `room/Type` alan yolunda görünür. Bu şekilde, `room/Type` geçerli belgedeki geçerli odanın türünü ifade eder. Bu `Type` alt alanın tek bir örneğidir, bu nedenle doğrudan filtrede kullanılabilir.

### <a name="using-field-paths"></a>Alan yollarını kullanma

Alan yolları [Azure Bilişsel Arama REST API'lerinin](https://docs.microsoft.com/rest/api/searchservice/)birçok parametresinde kullanılır. Aşağıdaki tabloda kullanılabilecekleri tüm yerler ve bunların kullanımıyla ilgili kısıtlamalar listelenemeniz gerekir:

| API | Parametre adı | Kısıtlamalar |
| --- | --- | --- |
| [Dizin Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [Güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) | `suggesters/sourceFields` | None |
| [Dizin Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [Güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) | `scoringProfiles/text/weights` | Yalnızca **aranabilir alanlara başvurabilir** |
| [Dizin Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) veya [Güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) | `scoringProfiles/functions/fieldName` | Yalnızca filtrelenebilir alanlara **başvurabilir** |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `search`ne `queryType` zaman`full` | Yalnızca **aranabilir alanlara başvurabilir** |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `facet` | Yalnızca **facetable** alanları için başvurabilir |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `highlight` | Yalnızca **aranabilir alanlara başvurabilir** |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `searchFields` | Yalnızca **aranabilir alanlara başvurabilir** |
| [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions) ve [Otomatik Tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `searchFields` | Yalnızca [önericinin](index-add-suggesters.md) parçası olan alanlara başvurabilir |
| [Ara](https://docs.microsoft.com/rest/api/searchservice/search-documents), [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions)ve [Otomatik Tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `$filter` | Yalnızca filtrelenebilir alanlara **başvurabilir** |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) ve [Önerme](https://docs.microsoft.com/rest/api/searchservice/suggestions) | `$orderby` | Yalnızca **sıralanabilir** alanlara başvurabilir |
| [Arama](https://docs.microsoft.com/rest/api/searchservice/search-documents), [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions)ve [Arama](https://docs.microsoft.com/rest/api/searchservice/lookup-document) | `$select` | Yalnızca **alınabilir** alanlara başvurabilir |

## <a name="constants"></a>Sabitler

OData'daki sabitler, belirli bir [Varlık Veri Modeli](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) (EDM) türünün gerçek değerleridir. Azure Bilişsel Arama'da desteklenen türlerin listesi için [desteklenen veri türlerine](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) bakın. Koleksiyon türlerinin sabitleri desteklenmez.

Aşağıdaki tablo, Azure Bilişsel Arama tarafından desteklenen veri türlerinin her biri için sabit örneklerini gösterir:

| Veri türü | Örnek sabitler |
| --- | --- |
| `Edm.Boolean` | `true`, `false` |
| `Edm.DateTimeOffset` | `2019-05-06T12:30:05.451Z` |
| `Edm.Double` | `3.14159`, `-1.2e7`, `NaN`, `INF`, `-INF` |
| `Edm.GeographyPoint` | `geography'POINT(-122.131577 47.678581)'` |
| `Edm.GeographyPolygon` | `geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'` |
| `Edm.Int32` | `123`, `-456` |
| `Edm.Int64` | `283032927235` |
| `Edm.String` | `'hello'` |

### <a name="escaping-special-characters-in-string-constants"></a>Dize sabitlerinde özel karakterlerden kaçış

OData'daki dize sabitleri tek tırnak la sınırlandırılır. Tek tırnak işaretleri içerebilecek bir dize sabiti içeren bir sorgu oluşturmanız gerekiyorsa, bunları iki katına çıkararak közlediğiniz tırnak işaretlerinden kaçabilirsiniz.

Örneğin, "Alice'in arabası" gibi biçimlendirilmemiş bir apostrophe içeren bir tümcecik `'Alice''s car'`OData'da dize sabiti olarak temsil edilir.

> [!IMPORTANT]
> Filtreleri programlı bir şekilde yaparken, kullanıcı girişlerinden gelen dize sabitlerinden kaçmayı unutmamak önemlidir. Bu [enjeksiyon saldırıları](https://wikipedia.org/wiki/SQL_injection)olasılığını azaltmak için , özellikle [güvenlik kırpma](search-security-trimming-for-azure-search.md)uygulamak için filtreleri kullanırken.

### <a name="constants-syntax"></a>Sabitler sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)yukarıdaki tabloda gösterilen sabitlerin çoğu için dilbilgisi tanımlar. Coğrafi uzamsal türler için [dilbilgisi, Azure Bilişsel Arama'da OData coğrafi uzamsal işlevlerinde](search-query-odata-geo-spatial-functions.md)bulunabilir.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#constant)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

## <a name="building-expressions-from-field-paths-and-constants"></a>Alan yollarından ve sabitlerden ifadeler oluşturma

Alan yolları ve sabitler, OData ifadesinin en temel parçasıdır, ancak zaten tam ifadelerdir. Aslında, Azure Bilişsel **Arama'daki $select** parametresi, alan yollarının virgülle ayrılmış bir listesinden başka bir şey değildir ve **$orderby** **$select**çok daha karmaşık değildir. Dizininizde bir tür `Edm.Boolean` alanı varsa, bu alanın yolundan başka bir şey olmayan bir filtre bile yazabilirsiniz. Sabitler `true` ve `false` aynı şekilde geçerli filtreler vardır.

Ancak, çoğu zaman birden fazla alan ve sabit bakın daha karmaşık ifadeler gerekir. Bu ifadeler parametreye bağlı olarak farklı şekillerde oluşturulur.

Aşağıdaki EBNF[(Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form) **$filter**, **$orderby**ve **$select** parametreleri için dilbilgisi tanımlar. Bunlar, alan yollarına ve sabitlere atıfta bulunan daha basit ifadelerden oluşturulur:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#filter_expression)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

**$orderby** ve **$select** parametreleri hem virgülle ayrılmış basit ifadeler listeleridir. **$filter** parametresi, daha basit alt ifadelerden oluşan bir Boolean ifadesidir. Bu alt [ `and`ifadeler, `or` `not` ](search-query-odata-logical-operators.md) [ `eq`, `lt` `gt`](search-query-odata-comparison-operators.md), ve , , , , , gibi karşılaştırma işleçleri gibi mantıksal işleçler ve toplama işleçleri [ `any` `all`gibi ](search-query-odata-collection-operators.md)ve .

**$filter**, **$orderby**ve **$select** parametreleri aşağıdaki makalelerde daha ayrıntılı olarak incelenir:

- [Azure Bilişsel Arama'da Sözdizimini $filter Veri](search-query-odata-filter.md)
- [Azure Bilişsel Arama'da Sözdizimini $orderby](search-query-odata-orderby.md)
- [Azure Bilişsel Arama'da OData $select sözdizimi](search-query-odata-select.md)

## <a name="see-also"></a>Ayrıca bkz.  

- [Azure Bilişsel Arama'da yönlü gezinme](search-faceted-navigation.md)
- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Lucene sorgu söz dizimi](query-lucene-syntax.md)
- [Azure Bilişsel Arama'da basit sorgu sözdizimi](query-simple-syntax.md)
