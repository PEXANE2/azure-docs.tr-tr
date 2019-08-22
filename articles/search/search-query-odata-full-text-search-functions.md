---
title: OData tam metin arama işlevi başvurusu-Azure Search
description: OData tam metin arama işlevleri, arama. IsMatch ve Search. ısmatchpuanlama, Azure Search sorgularda.
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
ms.openlocfilehash: c3b28c8799b09ddfe008df8539709c5a704ac6b4
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648008"
---
# <a name="odata-full-text-search-functions-in-azure-search---searchismatch-and-searchismatchscoring"></a>Azure Search `search.ismatch` ve içindeki OData tam metin arama işlevleri`search.ismatchscoring`

Azure Search, `search.ismatch` ve `search.ismatchscoring` işlevleri aracılığıyla [OData filtre ifadeleri](query-odata-filter-orderby-syntax.md) bağlamında tam metin aramayı destekler. Bu işlevler, tam metin aramasını, yalnızca `search` [Arama API](https://docs.microsoft.com/rest/api/searchservice/search-documents)'sinin en üst düzey parametresini kullanarak mümkün olmayan şekilde kesin Boole filtrelemeleri ile birleştirmenize olanak tanır.

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
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

### <a name="searchismatch"></a>Search. IsMatch

`search.ismatch` İşlevi bir filtre ifadesinin parçası olarak bir tam metin arama sorgusu değerlendirir. Arama sorgusuyla eşleşen belgeler sonuç kümesinde döndürülür. Bu işlevin aşağıdaki aşırı yüklemeleri kullanılabilir:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Parametreler aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Type | Açıklama |
| --- | --- | --- |
| `search` | `Edm.String` | Arama sorgusu ( [basit](query-simple-syntax.md) veya [tam](query-lucene-syntax.md) Lucene sorgu sözdiziminde). |
| `searchFields` | `Edm.String` | Arama yapılacak aranabilir alanların virgülle ayrılmış listesi; dizindeki tüm aranabilir alanları varsayılan olarak belirler. `search` Parametresinde, alan [araması](query-lucene-syntax.md#bkmk_fields) kullanılırken, Lucene sorgusunda alan belirticileri bu parametrede belirtilen tüm alanları geçersiz kılar. |
| `queryType` | `Edm.String` | `'simple'`veya `'full'`; varsayılan olarak `'simple'`olur. `search` Parametrede hangi sorgu dilinin kullanıldığını belirtir. |
| `searchMode` | `Edm.String` | `'any'`ya `'all'`da varsayılan olarak `'any'`olur. Belgeyi eşleşme olarak saymak için `search` parametresindeki arama terimlerinin herhangi birinin veya tümünün eşleşmesi gerekip gerekmediğini gösterir. `search` Parametresindeki [Lucene Boolean işleçleri](query-lucene-syntax.md#bkmk_boolean) kullanıldığında, bu parametre üzerinden öncelikli olur. |

Yukarıdaki parametrelerin tümü, [Arama API 'sindeki karşılık gelen arama isteği parametrelerine](https://docs.microsoft.com/rest/api/searchservice/search-documents)eşdeğerdir.

İşlevi, Boolean [mantıksal işleçlerini](search-query-odata-logical-operators.md)kullanarak diğer `Edm.Boolean`filtre alt ifadeleriyle onu oluşturmanıza olanak sağlayan türünde bir değer döndürür. `search.ismatch`

> [!NOTE]
> Azure Search lambda ifadelerinin kullanımını `search.ismatch` veya `search.ismatchscoring` içini desteklemez. Bu, tam metin arama eşleşmelerini aynı nesne üzerinde kesin filtre eşleşmeleri ile ilişkilendirebileceği nesne koleksiyonları üzerine filtre yazmak mümkün olmadığı anlamına gelir. Bu sınırlama ve örnekler hakkında daha fazla bilgi için bkz. [Azure Search koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md). Bu sınırlamanın neden olduğu hakkında daha ayrıntılı bilgi için bkz. [Azure Search koleksiyon filtrelerini anlama](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>arama. ısmatchpuanlama

İşlevi gibi işlev, parametre olarak geçirilmiş tam metin arama sorgusuyla eşleşen belgeler için döndürür `true`. `search.ismatchscoring` `search.ismatch` Aralarındaki fark, `search.ismatchscoring` sorguyla eşleşen belgelerin ilgi puanının Genel belge puanına katkıda bulunmasını sağlar, ancak bu `search.ismatch`durumda belge puanı değiştirilmez. Bu işlevin aşağıdaki aşırı yüklemeleri, bunlarla özdeş `search.ismatch`olan parametrelerle kullanılabilir:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

`search.ismatch` Hem hem `search.ismatchscoring` de işlevleri aynı filtre ifadesinde kullanılabilir.

## <a name="examples"></a>Örnekler

"Su ön" kelimesiyle belge bulun. Bu filtre sorgusu, ile `search=waterfront`bir [arama isteğiyle](https://docs.microsoft.com/rest/api/searchservice/search-documents) özdeştir.

    search.ismatchscoring('waterfront')

"Hostel" sözcüğünü içeren belgeleri bulun ve 4 ' e eşit veya daha büyük derecelendirme veya "Motel" sözcüğünü ve derecesi 5 ' e eşit olan belgeleri bulun. Not, bu istek `search.ismatchscoring` işlev olmadan ifade edilemedi.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

"Merkezlerini" sözcüğü olmadan belge bul.

    not search.ismatch('luxury')

"Okyanus görünümü" veya derecelendirme 5 ' e eşit olan belgeleri bulun. Sorgu yalnızca alanlara `HotelName` ve ' `Rooms/Description`a karşı yürütülür. `search.ismatchscoring`

Disbirleşimin yalnızca ikinci yan tümcesiyle eşleşen belgeler, 5 ' e eşit olan `Rating` çok--oteller olarak döndürülür. Bu belgelerin, ifadenin puanlanmış parçalarından hiçbiriyle eşleşmediğinden emin olmak için, puanın sıfıra eşit olarak döndürülmeleri gerekir.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

"Otel" ve "Havaalanı" koşullarının, otel açıklamasına ait 5 sözcükten ve her odada en az bir kısmında izin verilmediği belgeleri bulun. Bu sorgu, [tam Lucene sorgu dilini](query-lucene-syntax.md)kullanır.

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
