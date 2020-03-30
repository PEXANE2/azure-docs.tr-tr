---
title: OData tam metin arama fonksiyonu başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData tam metin arama işlevleri, search.ismatch ve search.ismatchscoring.
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
ms.openlocfilehash: 06eb29f2f3245d3f4fd047fb86b2b57fb1f0989e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793359"
---
# <a name="odata-full-text-search-functions-in-azure-cognitive-search---searchismatch-and-searchismatchscoring"></a>Azure Bilişsel Arama'da OData tam `search.ismatch` metin arama işlevleri - ve`search.ismatchscoring`

Azure Bilişsel Arama, [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) bağlamında tam metin `search.ismatch` `search.ismatchscoring` aramayı destekler ve işlevler aracılığıyla. Bu işlevler, tam metin aramaile katı Boolean filtrelemesini yalnızca Arama `search` [API'sının](https://docs.microsoft.com/rest/api/searchservice/search-documents)üst düzey parametresini kullanarak mümkün olmayan şekillerde birleştirmenize olanak sağlar.

> [!NOTE]
> Ve `search.ismatch` `search.ismatchscoring` işlevler yalnızca [Arama API'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents)filtrelerde desteklenir. [Öner](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [Otomatik Tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) API'lerinde desteklenmez.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)ve `search.ismatch` `search.ismatchscoring` işlevlerin dilbilgisini tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

### <a name="searchismatch"></a>search.ismatch

İşlev, `search.ismatch` tam metin arama sorgusunu filtre ifadesinin bir parçası olarak değerlendirir. Arama sorgusuyla eşleşen belgeler sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı yükleri kullanılabilir:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Parametreler aşağıdaki tabloda tanımlanır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `search` | `Edm.String` | Arama sorgusu [(basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) Lucene sorgu sözdiziminde). |
| `searchFields` | `Edm.String` | Aranabilir alanların virgülle ayrılmış listesi aranır; dizinteki tüm aranabilir alanlar için varsayılan. Parametrede [alanlı arama](query-lucene-syntax.md#bkmk_fields) kullanırken, Lucene sorgusundaki alan belirteciler bu parametrede belirtilen alanları geçersiz kılar. `search` |
| `queryType` | `Edm.String` | `'simple'`veya `'full'`; varsayılan olarak `'simple'`. Parametrede hangi sorgu dilinin `search` kullanıldığını belirtir. |
| `searchMode` | `Edm.String` | `'any'`veya `'all'`, varsayılan `'any'`. Belgeyi eş olarak saymak için `search` parametredeki arama terimlerinden herhangi birinin veya tamamının eşleşip eşleşmemesi gerektiğini gösterir. `search` Parametredeki [Lucene Boolean işleçlerini](query-lucene-syntax.md#bkmk_boolean) kullanırken, bu parametreden önce gelir. |

Yukarıdaki parametrelerin [tümü, Arama API'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents)karşılık gelen arama isteği parametrelerine eşittir.

İşlev, `search.ismatch` Boolean `Edm.Boolean` [mantıksal işleçlerini](search-query-odata-logical-operators.md)kullanarak diğer filtre alt ifadeleri ile oluşturmak için izin veren bir tür değeri döndürür.

> [!NOTE]
> Azure Bilişsel Arama, `search.ismatch` lambda ifadelerinin kullanılmasını veya `search.ismatchscoring` içinde kullanılmasını desteklemez. Bu, tam metin arama eşlerini aynı nesneüzerindeki katı filtre eşleşmeleriyle ilişkilendirebilen nesnelerin koleksiyonları na filtre yazmanın mümkün olmadığı anlamına gelir. Bu sınırlama ve örnekler hakkında daha fazla bilgi için [Azure Bilişsel Arama'daki Sorun Giderme toplama filtrelerine](search-query-troubleshoot-collection-filters.md)bakın. Bu sınırlamanın neden var olduğu hakkında daha ayrıntılı bilgi için Azure [Bilişsel Arama'da koleksiyon filtrelerini anlama 'ya](search-query-understand-collection-filters.md)bakın.


### <a name="searchismatchscoring"></a>search.ismatchscoring

İşlev `search.ismatchscoring` gibi `search.ismatch` işlev, `true` parametre olarak geçirilen tam metin arama sorgusuyla eşleşen belgeler için döndürür. Aralarındaki fark, `search.ismatchscoring` sorguyla eşleşen belgelerin alaka düzeyi puanının genel belge puanına `search.ismatch`katkıda bulunması, belge puanının değiştirilmemesi dir. Bu fonksiyonun aşağıdaki aşırı yükleri, aşağıdakiparametrelerle `search.ismatch`aynıdır:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Hem `search.ismatch` işlevler hem de `search.ismatchscoring` işlevler aynı filtre ifadesinde kullanılabilir.

## <a name="examples"></a>Örnekler

"Rıhtım" sözcüğünün yer aldığı belgeleri bulun. Bu filtre [search request](https://docs.microsoft.com/rest/api/searchservice/search-documents) `search=waterfront`sorgusu, '.

    search.ismatchscoring('waterfront')

"Hostel" sözcüğünün ve 4'ün üzerinde veya eşit olan belgeleri veya "motel" sözcüğü ne kadar yüksek hem de 5'e eşit olan belgeleri bulun. Not, bu istek `search.ismatchscoring` işlevi olmadan ifade edilemedi.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

"Lüks" kelimesi olmadan belgeleri bulun.

    not search.ismatch('luxury')

"Okyanus görünümü" veya 5'e eşit derecelendirmesi olan belgeleri bulun. Sorgu `search.ismatchscoring` yalnızca alanlara `HotelName` karşı `Rooms/Description`yürütülür ve .

Disjunction'ın yalnızca ikinci maddesine uyan belgeler de iade `Rating` edilecek - 5'e eşit oteller. Bu belgelerin ifadenin puanlanan kısımlarıyla eşleşmediğini belirtmek için, sıfıra eşit puanla döndürülürler.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

Otelin açıklamasında "otel" ve "havaalanı" terimlerinin birbirinden 5 kelime içinde olduğu ve en azından bazı odalarda sigara içilmemesi ne rendelendiği belgeleri bulun. Bu sorgu [tam Lucene sorgu dilini](query-lucene-syntax.md)kullanır.

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
