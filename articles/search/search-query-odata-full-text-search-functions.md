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
ms.openlocfilehash: 06eb29f2f3245d3f4fd047fb86b2b57fb1f0989e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "72793359"
---
# <a name="odata-full-text-search-functions-in-azure-cognitive-search---searchismatch-and-searchismatchscoring"></a>Azure Bilişsel Arama OData tam metin arama işlevleri- `search.ismatch` ve`search.ismatchscoring`

Azure Bilişsel Arama, `search.ismatch` ve `search.ismatchscoring` işlevleri aracılığıyla [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) bağlamında tam metin aramayı destekler. Bu işlevler, tam metin aramasını, yalnızca `search` [Arama API](https://docs.microsoft.com/rest/api/searchservice/search-documents)'sinin en üst düzey parametresini kullanarak mümkün olmayan şekilde kesin Boole filtrelemeleri ile birleştirmenize olanak tanır.

> [!NOTE]
> Ve işlevleri yalnızca [Arama API 'sindeki](https://docs.microsoft.com/rest/api/searchservice/search-documents)filtrelerde desteklenir. `search.ismatchscoring` `search.ismatch` Bunlar, [öneri](https://docs.microsoft.com/rest/api/searchservice/suggestions) veya [otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) API 'lerinde desteklenmez.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) `search.ismatch` ve `search.ismatchscoring` işlevlerinin dilbilgisini tanımlar:

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

`search.ismatch` İşlevi bir filtre ifadesinin parçası olarak bir tam metin arama sorgusu değerlendirir. Arama sorgusuyla eşleşen belgeler sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı yüklemeleri kullanılabilir:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Parametreler aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `search` | `Edm.String` | Arama sorgusu ( [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) Lucene sorgu sözdiziminde). |
| `searchFields` | `Edm.String` | Arama yapılacak aranabilir alanların virgülle ayrılmış listesi; dizindeki tüm aranabilir alanları varsayılan olarak belirler. Parametresinde, alan araması kullanılırken, Lucene sorgusunda alan belirticileri bu parametrede belirtilen tüm alanları geçersiz kılar. [fielded search](query-lucene-syntax.md#bkmk_fields) `search` |
| `queryType` | `Edm.String` | `'simple'`ya `'full'`da; Varsayılan olarak `'simple'`olur. `search` Parametrede hangi sorgu dilinin kullanıldığını belirtir. |
| `searchMode` | `Edm.String` | `'any'`ya `'all'`da varsayılan olarak `'any'`olur. Belgeyi eşleşme olarak saymak için `search` parametresindeki arama terimlerinin herhangi birinin veya tümünün eşleşmesi gerekip gerekmediğini gösterir. Parametresindeki [Lucene Boolean işleçleri](query-lucene-syntax.md#bkmk_boolean) kullanıldığında, bu parametre üzerinden öncelikli olur. `search` |

Yukarıdaki parametrelerin tümü, [Arama API 'sindeki karşılık gelen arama isteği parametrelerine](https://docs.microsoft.com/rest/api/searchservice/search-documents)eşdeğerdir.

`search.ismatch` Işlevi, Boolean [mantıksal işleçlerini](search-query-odata-logical-operators.md)kullanarak diğer filtre alt ifadeleriyle onu oluşturmanıza olanak sağlayan türünde `Edm.Boolean`bir değer döndürür.

> [!NOTE]
> Azure Bilişsel Arama lambda ifadelerinin kullanımını `search.ismatch` veya `search.ismatchscoring` içini desteklemez. Bu, tam metin arama eşleşmelerini aynı nesne üzerinde kesin filtre eşleşmeleri ile ilişkilendirebileceği nesne koleksiyonları üzerine filtre yazmak mümkün olmadığı anlamına gelir. Bu sınırlama hakkında daha fazla bilgi ve örnek için bkz. [Azure bilişsel arama 'da koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md). Bu sınırlamanın neden varolduğu hakkında daha ayrıntılı bilgi için bkz. [Azure bilişsel arama 'da koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>arama. ısmatchpuanlama

İşlevi gibi işlev, parametre olarak geçirilmiş tam metin arama sorgusuyla eşleşen belgeler için döndürür `true` `search.ismatchscoring` `search.ismatch` Aralarındaki fark, `search.ismatchscoring` sorguyla eşleşen belgelerin ilgi puanının Genel belge puanına katkıda bulunmasını sağlar, ancak bu durumda `search.ismatch`belge puanı değiştirilmez. Bu işlevin aşağıdaki aşırı yüklemeleri, bunlarla özdeş olan parametrelerle kullanılabilir `search.ismatch`:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Hem `search.ismatch` hem `search.ismatchscoring` de işlevleri aynı filtre ifadesinde kullanılabilir.

## <a name="examples"></a>Örnekler

"Su ön" kelimesiyle belge bulun. Bu filtre sorgusu, ile `search=waterfront`bir [arama isteğiyle](https://docs.microsoft.com/rest/api/searchservice/search-documents) özdeştir.

    search.ismatchscoring('waterfront')

"Hostel" sözcüğünü içeren belgeleri bulun ve 4 ' e eşit veya daha büyük derecelendirme veya "Motel" sözcüğünü ve derecesi 5 ' e eşit olan belgeleri bulun. Not, bu istek `search.ismatchscoring` işlev olmadan ifade edilemedi.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

"Merkezlerini" sözcüğü olmadan belge bul.

    not search.ismatch('luxury')

"Okyanus görünümü" veya derecelendirme 5 ' e eşit olan belgeleri bulun. `search.ismatchscoring` Sorgu yalnızca alanlara `HotelName` ve ' `Rooms/Description`a karşı yürütülür.

Disbirleşimin yalnızca ikinci yan tümcesiyle eşleşen belgeler, 5 ' e eşit olan `Rating` çok--oteller olarak döndürülür. Bu belgelerin, ifadenin puanlanmış parçalarından hiçbiriyle eşleşmediğinden emin olmak için, puanın sıfıra eşit olarak döndürülmeleri gerekir.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

"Otel" ve "Havaalanı" koşullarının, otel açıklamasına ait 5 sözcükten ve her odada en az bir kısmında izin verilmediği belgeleri bulun. Bu sorgu, [tam Lucene sorgu dilini](query-lucene-syntax.md)kullanır.

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
