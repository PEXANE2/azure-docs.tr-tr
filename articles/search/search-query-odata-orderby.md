---
title: OData sırası başvuruya göre
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında sıralama ölçütü kullanmaya yönelik sözdizimi ve dil başvurusu belgeleri.
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
ms.openlocfilehash: 99ec639b88f3334530243242aadfa0ab52a40df0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74113152"
---
# <a name="odata-orderby-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de OData $orderby söz dizimi

 Azure Bilişsel Arama arama sonuçları için özel bir sıralama düzeni uygulamak üzere [OData **$OrderBy** parametresini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz. Bu makalede **$OrderBy** sözdizimi ayrıntılı olarak açıklanmaktadır. Arama sonuçlarını sunarken **$OrderBy** kullanma hakkında daha fazla genel bilgi için bkz. [Azure bilişsel arama arama sonuçlarıyla çalışma](search-pagination-page-layout.md).

## <a name="syntax"></a>Sözdizimi

**$OrderBy** parametresi, en fazla 32 **sıra ölçütü yan tümceleri**olan virgülle ayrılmış bir liste kabul eder. Order by yan tümcesinin sözdizimi aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) tarafından açıklanmıştır:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

Her yan tümce sıralama ölçütlerine sahiptir ve isteğe bağlı olarak bir sıralama yönü`asc` (artan veya `desc` azalan için) izler. Bir yön belirtmezseniz, varsayılan değer artan olur. Sıralama ölçütü, bir `sortable` alanın yolu veya ya da [`geo.distance`](search-query-odata-geo-spatial-functions.md) [`search.score`](search-query-odata-search-score-function.md) işlevlerine yapılan bir çağrı olabilir.

Birden çok belge aynı sıralama ölçütlerine sahip ise ve `search.score` işlev kullanılmazsa (örneğin, bir sayısal `Rating` alana göre sıralarsanız ve hepsi 4 derecelendirmesine sahipseniz), bu siteler belge puanına göre azalan sırada bozulur. Belge puanları aynı olduğunda (örneğin, istekte tam metin arama sorgusu belirtilmediğinde), bağlı belgelerin göreli sıralaması belirsiz olur.

Birden çok sıralama ölçütü belirtebilirsiniz. İfadelerin sırası, son sıralama düzenini belirler. Örneğin, derecelendirmeye göre azalan sırada sıralama yapmak için sözdizimi olur `$orderby=search.score() desc,Rating desc`.

`geo.distance` **$OrderBy** ' deki sözdizimi **$Filter**' deki ile aynıdır. $Orderby kullanılırken `geo.distance` , **$orderby**geçerli olduğu alan türünde `Edm.GeographyPoint` olmalıdır ve ayrıca olmalıdır `sortable`.

$Orderby ' de `search.score` için **$orderby** sözdizimi `search.score()`. İşlev `search.score` herhangi bir parametre almaz.

## <a name="examples"></a>Örnekler

Oteller taban oranına göre artan şekilde sıralayın:

    $orderby=BaseRate asc

Otelleri derecelendirmeye göre azalan şekilde sıralayın, ardından taban oranına göre artan şekilde (artan varsayılan değer olduğunu unutmayın):

    $orderby=Rating desc,BaseRate

Otelleri derecelendirmeye göre azalan şekilde sıralayın, sonra da verilen koordinatlardan uzaklıktan yükselen:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Oteller, arama. puan ve derecelendirme, ardından verilen koordinatlardan uzaklıktan artan sırada sıralayın. Aynı ilgi puanları ve derecelendirmelere sahip iki otel arasında en yakın olanı ilk olarak listelenmiştir:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama arama sonuçlarıyla çalışma](search-pagination-page-layout.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
