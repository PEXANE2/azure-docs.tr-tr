---
title: OData search.in işlev başvurusu-Azure Search
description: Azure Search sorgularında OData search.in işlevi.
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
ms.openlocfilehash: 8bac0205fa2de8378abaa4d9e8ba8e05ea69192e
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647933"
---
# <a name="odata-searchin-function-in-azure-search"></a>Azure Search `search.in` OData işlevi

[OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) yaygın bir senaryo, her bir belgedeki tek bir alanın olası birçok değerden birine eşit olup olmadığını denetlemenize olanak tanır. Örneğin, bu, bir veya daha fazla asıl kimliği içeren bir alanı, sorguyu veren kullanıcıyı temsil eden bir asıl kimlik listesine göre denetleyerek, bazı uygulamalar [güvenlik kırpması](search-security-trimming-for-azure-search.md) uygular. Bunun gibi bir sorgu yazmanın bir yolu, [`eq`](search-query-odata-comparison-operators.md) ve [`or`](search-query-odata-logical-operators.md) işleçlerini kullanmaktır:

    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')

Ancak, `search.in` işlevini kullanarak bunu yazmanın daha kısa bir yolu vardır:

    group_ids/any(g: search.in(g, '123, 456, 789'))

> [!IMPORTANT]
> Daha kısa ve kolay okunması yanı sıra, kullanmak `search.in` da [performans avantajları](#bkmk_performance) sağlar ve filtreye dahil edilecek yüzlerce veya hatta binlerce değer olduğunda [filtrelerin belirli boyut sınırlamalarını](search-query-odata-filter.md#bkmk_limits) önler. Bu nedenle, daha karmaşık bir eşitlik ifadesi `search.in` birleşimi yerine kullanmanızı kesinlikle öneririz.

> [!NOTE]
> OData Standard sürüm 4,01, yakın zamanda Azure Search `search.in` işlevi gibi benzer davranışa sahip olan [ `in` işlecini](https://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230)kullanıma sunmuştur. Ancak Azure Search bu işleci desteklemez, bu nedenle, bunun yerine `search.in` işlevini kullanmanız gerekir.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) `search.in` işlevin dilbilgisini tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

İşlevi `search.in` , belirli bir dize alanı veya Aralık değişkeninin belirli bir değer listesinden birine eşit olup olmadığını sınar. Değişken ile listedeki her bir değer arasındaki eşitlik, `eq` işleçle aynı şekilde, büyük/küçük harf duyarlı bir biçimde belirlenir. Bu nedenle, gibi `search.in(myfield, 'a, b, c')` bir ifade ile `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`eşdeğerdir, ancak `search.in` bunun dışında daha iyi performans elde edilir.

`search.in` İşlevin iki aşırı yüklemesi vardır:

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

Parametreler aşağıdaki tabloda tanımlanmıştır:

| Parametre adı | Type | Açıklama |
| --- | --- | --- |
| `variable` | `Edm.String` | Bir dize alanı başvurusu (veya bir `search.in` `any` veya `all` ifadesi içinde kullanıldığı durumda bir dize koleksiyonu alanı üzerinde bir Aralık değişkeni). |
| `valueList` | `Edm.String` | `variable` Parametresiyle eşleştirilecek bir değer listesi içeren bir dize. `delimiters` Parametresi belirtilmemişse, varsayılan sınırlayıcılar boşluk ve virgüldür. |
| `delimiters` | `Edm.String` | `valueList` Parametre ayrıştırılırken her karakterin ayırıcı olarak kabul edildiği bir dize. Bu parametrenin varsayılan değeri, `' ,'` aralarında boşluk ve/veya virgüller içeren tüm değerlerin ayrılacağı anlamına gelir. Değerleriniz bu karakterleri içerdiğinden boşluklar ve virgüller dışında ayırıcılar kullanmanız gerekiyorsa, bu parametre gibi alternatif sınırlayıcılar `'|'` belirtebilirsiniz. |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Performansı`search.in`

Kullanırsanız `search.in`, ikinci parametre yüzlerce veya binlerce değerin listesini içerdiğinde alt ikinci yanıt süresini de bekleyebilir. İzin verilen en büyük istek boyutuyla sınırlı olsanız da, geçirebilmeniz `search.in`gereken öğe sayısı üzerinde açık bir sınır yoktur. Ancak, değer sayısı arttıkça gecikme artar.

## <a name="examples"></a>Örnekler

Adı ' Sea View Motel ' veya ' bütçe otel ' değerine eşit olan tüm oteller bulun. Tümcecikler varsayılan bir sınırlayıcı olan boşluklar içerir. Üçüncü dize parametresi olarak tek tırnak içinde alternatif bir sınırlayıcı belirtebilirsiniz:  

    search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Adı ' | ' ile ayrılmış ' Sea View Motel ' veya ' bütçe otel ' değerine eşit olan tüm oteller bulun:

    search.in(HotelName, 'Sea View motel|Budget hotel', '|')

' WiFi ' veya ' Tub ' etiketine sahip olan odaları içeren tüm oteller bulun:

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))

Etiketlerde ' ısıtılan tocekliler ' veya ' ince kurutucu dahil ' gibi bir koleksiyonda eşleşme bulun.

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

' Motel ' veya ' cabin' ' etiketi olmadan tüm otelleri bul:

    Tags/all(tag: not search.in(tag, 'motel, cabin'))

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
