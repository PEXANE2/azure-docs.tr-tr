---
title: OData tam metin arama işlevi başvurusu
titleSuffix: Azure Cognitive Search
description: OData tam metin arama işlevleri, Azure Bilişsel Arama sorgularında Search. IsMatch ve Search. ısmatchpuanlama.
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
ms.openlocfilehash: 78f9e4d8fa80fdf74bdb5cd79f4489d12696fcc2
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935798"
---
# <a name="odata-full-text-search-functions-in-azure-cognitive-search---searchismatch-and-searchismatchscoring"></a>Azure Bilişsel Arama OData tam metin arama işlevleri- `search.ismatch` ve `search.ismatchscoring`

Azure Bilişsel Arama, ve işlevleri aracılığıyla [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) bağlamında tam metin aramayı destekler `search.ismatch` `search.ismatchscoring` . Bu işlevler, tam metin aramasını `search` , yalnızca [Arama API](/rest/api/searchservice/search-documents)'sinin en üst düzey parametresini kullanarak mümkün olmayan şekilde kesin Boole filtrelemeleri ile birleştirmenize olanak tanır.

> [!NOTE]
> `search.ismatch`Ve `search.ismatchscoring` Işlevleri yalnızca [Arama API 'sindeki](/rest/api/searchservice/search-documents)filtrelerde desteklenir. Bunlar, [öneri](/rest/api/searchservice/suggestions) veya [otomatik tamamlama](/rest/api/searchservice/autocomplete) API 'lerinde desteklenmez.

## <a name="syntax"></a>Syntax

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) `search.ismatch` ve işlevlerinin dilbilgisini tanımlar `search.ismatchscoring` :

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

### <a name="searchismatch"></a>Search. IsMatch

`search.ismatch`İşlevi bir filtre ifadesinin parçası olarak bir tam metin arama sorgusu değerlendirir. Arama sorgusuyla eşleşen belgeler sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı yüklemeleri kullanılabilir:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Parametreler aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `search` | `Edm.String` | Arama sorgusu ( [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) Lucene sorgu sözdiziminde). |
| `searchFields` | `Edm.String` | Arama yapılacak aranabilir alanların virgülle ayrılmış listesi; dizindeki tüm aranabilir alanları varsayılan olarak belirler. [fielded search](query-lucene-syntax.md#bkmk_fields) `search` Parametresinde, alan araması kullanılırken, Lucene sorgusunda alan belirticileri bu parametrede belirtilen tüm alanları geçersiz kılar. |
| `queryType` | `Edm.String` | `'simple'` veya `'full'` ; Varsayılan olarak olur `'simple'` . Parametrede hangi sorgu dilinin kullanıldığını belirtir `search` . |
| `searchMode` | `Edm.String` | `'any'` ya `'all'` da varsayılan olarak olur `'any'` . `search`Belgeyi eşleşme olarak saymak için parametresindeki arama terimlerinin herhangi birinin veya tümünün eşleşmesi gerekip gerekmediğini gösterir. Parametresindeki [Lucene Boolean işleçleri](query-lucene-syntax.md#bkmk_boolean) kullanıldığında `search` , bu parametre üzerinden öncelikli olur. |

Yukarıdaki parametrelerin tümü, [Arama API 'sindeki karşılık gelen arama isteği parametrelerine](/rest/api/searchservice/search-documents)eşdeğerdir.

`search.ismatch`İşlevi, `Edm.Boolean` Boolean [mantıksal işleçlerini](search-query-odata-logical-operators.md)kullanarak diğer filtre alt ifadeleriyle onu oluşturmanıza olanak sağlayan türünde bir değer döndürür.

> [!NOTE]
> Azure Bilişsel Arama `search.ismatch` `search.ismatchscoring` lambda ifadelerinin kullanımını veya içini desteklemez. Bu, tam metin arama eşleşmelerini aynı nesne üzerinde kesin filtre eşleşmeleri ile ilişkilendirebileceği nesne koleksiyonları üzerine filtre yazmak mümkün olmadığı anlamına gelir. Bu sınırlama hakkında daha fazla bilgi ve örnek için bkz. [Azure bilişsel arama 'da koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md). Bu sınırlamanın neden varolduğu hakkında daha ayrıntılı bilgi için bkz. [Azure bilişsel arama 'da koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>arama. ısmatchpuanlama

İşlevi gibi işlev, `search.ismatchscoring` `search.ismatch` `true` parametre olarak geçirilmiş tam metin arama sorgusuyla eşleşen belgeler için döndürür. Aralarındaki fark, sorguyla eşleşen belgelerin ilgi puanının `search.ismatchscoring` Genel belge puanına katkıda bulunmasını sağlar, ancak bu durumda `search.ismatch` belge puanı değiştirilmez. Bu işlevin aşağıdaki aşırı yüklemeleri, bunlarla özdeş olan parametrelerle kullanılabilir `search.ismatch` :

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Hem `search.ismatch` hem de `search.ismatchscoring` işlevleri aynı filtre ifadesinde kullanılabilir.

## <a name="examples"></a>Örnekler

"Su ön" kelimesiyle belge bulun. Bu filtre sorgusu, ile bir [arama isteğiyle](/rest/api/searchservice/search-documents) özdeştir `search=waterfront` .

```odata-filter-expr
    search.ismatchscoring('waterfront')
```

"Hostel" sözcüğünü içeren belgeleri bulun ve 4 ' e eşit veya daha büyük derecelendirme veya "Motel" sözcüğünü ve derecesi 5 ' e eşit olan belgeleri bulun. Not, bu istek işlev olmadan ifade edilemedi `search.ismatchscoring` .

```odata-filter-expr
    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5
```

"Merkezlerini" sözcüğü olmadan belge bul.

```odata-filter-expr
    not search.ismatch('luxury')
```

"Okyanus görünümü" veya derecelendirme 5 ' e eşit olan belgeleri bulun. `search.ismatchscoring`Sorgu yalnızca alanlara ve ' a karşı yürütülür `HotelName` `Rooms/Description` .

Disbirleşimin yalnızca ikinci yan tümcesiyle eşleşen belgeler, 5 ' e eşit olan çok--oteller olarak döndürülür `Rating` . Bu belgelerin, ifadenin puanlanmış parçalarından hiçbiriyle eşleşmediğinden emin olmak için, puanın sıfıra eşit olarak döndürülmeleri gerekir.

```odata-filter-expr
    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5
```

"Otel" ve "Havaalanı" koşullarının, otel açıklamasına ait 5 sözcükten ve her odada en az bir kısmında izin verilmediği belgeleri bulun. Bu sorgu, [tam Lucene sorgu dilini](query-lucene-syntax.md)kullanır.

```odata-filter-expr
    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](/rest/api/searchservice/Search-Documents)