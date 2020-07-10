---
title: OData search.in işlev başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında search.in işlevinin kullanılmasına yönelik sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: 1748a334c024401d845145947ecd55519f61e5e3
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206929"
---
# <a name="odata-searchin-function-in-azure-cognitive-search"></a>`search.in`Azure bilişsel arama 'de OData işlevi

[OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) yaygın bir senaryo, her bir belgedeki tek bir alanın olası birçok değerden birine eşit olup olmadığını denetlemenize olanak tanır. Örneğin, bu, bir veya daha fazla asıl kimliği içeren bir alanı, sorguyu veren kullanıcıyı temsil eden bir asıl kimlik listesine göre denetleyerek, bazı uygulamalar [güvenlik kırpması](search-security-trimming-for-azure-search.md) uygular. Bunun gibi bir sorgu yazmanın bir yolu, [`eq`](search-query-odata-comparison-operators.md) ve [`or`](search-query-odata-logical-operators.md) işleçlerini kullanmaktır:

```odata-filter-expr
    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')
```

Ancak, işlevini kullanarak bunu yazmanın daha kısa bir yolu vardır `search.in` :

```odata-filter-expr
    group_ids/any(g: search.in(g, '123, 456, 789'))
```

> [!IMPORTANT]
> Daha kısa ve kolay okunması yanı sıra, kullanmak `search.in` da [performans avantajları](#bkmk_performance) sağlar ve filtreye dahil edilecek yüzlerce veya hatta binlerce değer olduğunda [filtrelerin belirli boyut sınırlamalarını](search-query-odata-filter.md#bkmk_limits) önler. Bu nedenle, `search.in` daha karmaşık bir eşitlik ifadesi birleşimi yerine kullanmanızı kesinlikle öneririz.

> [!NOTE]
> OData standardının 4,01 sürümü yakın zamanda, Azure Bilişsel Arama işlevi gibi benzer davranışa sahip olan [ `in` işlecini](https://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230)kullanıma sunmuştur `search.in` . Ancak, Azure Bilişsel Arama bu işleci desteklemez, bu nedenle `search.in` bunun yerine işlevini kullanmanız gerekir.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) işlevin dilbilgisini tanımlar `search.in` :

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

`search.in`İşlevi, belirli bir dize alanı veya Aralık değişkeninin belirli bir değer listesinden birine eşit olup olmadığını sınar. Değişken ile listedeki her bir değer arasındaki eşitlik, işleçle aynı şekilde, büyük/küçük harf duyarlı bir biçimde belirlenir `eq` . Bu nedenle, gibi bir ifade `search.in(myfield, 'a, b, c')` ile eşdeğerdir `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'` , ancak bunun dışında `search.in` daha iyi performans elde edilir.

İşlevin iki aşırı yüklemesi vardır `search.in` :

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

Parametreler aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Tür | Açıklama |
| --- | --- | --- |
| `variable` | `Edm.String` | Bir dize alanı başvurusu (veya bir veya ifadesi içinde kullanıldığı durumda bir dize koleksiyonu alanı üzerinde bir Aralık değişkeni `search.in` `any` `all` ). |
| `valueList` | `Edm.String` | Parametresiyle eşleştirilecek bir değer listesi içeren bir dize `variable` . `delimiters`Parametresi belirtilmemişse, varsayılan sınırlayıcılar boşluk ve virgüldür. |
| `delimiters` | `Edm.String` | Parametre ayrıştırılırken her karakterin ayırıcı olarak kabul edildiği bir dize `valueList` . Bu parametrenin varsayılan değeri, aralarında `' ,'` boşluk ve/veya virgüller içeren tüm değerlerin ayrılacağı anlamına gelir. Değerleriniz bu karakterleri içerdiğinden boşluklar ve virgüller dışında ayırıcılar kullanmanız gerekiyorsa, bu parametre gibi alternatif sınırlayıcılar belirtebilirsiniz `'|'` . |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Performansı`search.in`

Kullanırsanız `search.in` , ikinci parametre yüzlerce veya binlerce değerin listesini içerdiğinde alt ikinci yanıt süresini de bekleyebilir. İzin `search.in` verilen en büyük istek boyutuyla sınırlı olsanız da, geçirebilmeniz gereken öğe sayısı üzerinde açık bir sınır yoktur. Ancak, değer sayısı arttıkça gecikme artar.

## <a name="examples"></a>Örnekler

Adı ' Sea View Motel ' veya ' bütçe otel ' değerine eşit olan tüm oteller bulun. Tümcecikler varsayılan bir sınırlayıcı olan boşluklar içerir. Üçüncü dize parametresi olarak tek tırnak içinde alternatif bir sınırlayıcı belirtebilirsiniz:  

```odata-filter-expr
    search.in(HotelName, 'Sea View motel,Budget hotel', ',')
```

Adı ' | ' ile ayrılmış ' Sea View Motel ' veya ' bütçe otel ' değerine eşit olan tüm oteller bulun:

```odata-filter-expr
    search.in(HotelName, 'Sea View motel|Budget hotel', '|')
```

' WiFi ' veya ' Tub ' etiketine sahip olan odaları içeren tüm oteller bulun:

```odata-filter-expr
    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))
```

Etiketlerde ' ısıtılan tocekliler ' veya ' ince kurutucu dahil ' gibi bir koleksiyonda eşleşme bulun.

```odata-filter-expr
    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))
```

' Motel ' veya ' cabin' ' etiketi olmadan tüm otelleri bul:

```odata-filter-expr
    Tags/all(tag: not search.in(tag, 'motel, cabin'))
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
