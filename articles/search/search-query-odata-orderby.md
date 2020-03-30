---
title: OData sipariş-referans
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında sipariş kullanarak sözdizimi ve dil başvuru belgeleri.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113152"
---
# <a name="odata-orderby-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da Sözdizimini $orderby

 Azure Bilişsel Arama'da arama sonuçları için özel sıralama sırası uygulamak için [OData **$orderby** parametresini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz. Bu makalede, ayrıntılı olarak **$orderby** sözdizimini açıklanır. Arama sonuçlarını sunarken **$orderby** nasıl kullanılacağı hakkında daha genel bilgi için Azure [Bilişsel Arama'da arama sonuçlarıyla nasıl çalışacağınız](search-pagination-page-layout.md)hakkında bilgi edin.

## <a name="syntax"></a>Sözdizimi

**$orderby** parametresi, 32'ye kadar **sipariş yan tümcesi**olan virgülle ayrılmış bir listeyi kabul eder. Bir sipariş-by yan tümcesinin sözdizimi aşağıdaki EBNF[(Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)ile açıklanmıştır:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

Her yan tümcenin sıralama ölçütleri`asc` vardır ve `desc` bunu isteğe bağlı olarak bir sıralama yönü (artan veya azalan için) takip eder. Bir yön belirtmezseniz, varsayılan değer yükseliyor. Sıralama ölçütleri, bir `sortable` alanın yolu veya [`geo.distance`](search-query-odata-geo-spatial-functions.md) [`search.score`](search-query-odata-search-score-function.md) işlevleri n için bir çağrı olabilir.

Birden çok belge aynı sıralama `search.score` ölçütleri vardır ve işlev kullanılmazsa (örneğin, sayısal `Rating` bir alana göre sıralarsanız ve üç belgenin hepsi 4 dereceye sahipse), bağlar azalan sırada belge puanıyla kırılır. Belge puanları aynı olduğunda (örneğin, istekte tam metin arama sorgusu belirtilmemişse), bağlı belgelerin göreli sıralaması belirsizdir.

Birden çok sıralama ölçütleri belirtebilirsiniz. İfadelerin sırası son sıralama sırasını belirler. Örneğin, azalan puanı sıralamak için, ardından Derecelendirme, sözdizimi . `$orderby=search.score() desc,Rating desc`

**$orderby** için `geo.distance` sözdizimi **$filter**ile aynıdır. **$orderby** `geo.distance` kullanırken, uygulandığı alan türünde `Edm.GeographyPoint` olmalı ve aynı zamanda `sortable`olmalıdır.

`search.score` **$orderby** için sözdizimi `search.score()`. İşlev `search.score` herhangi bir parametre almaz.

## <a name="examples"></a>Örnekler

Yükselen otelleri taban ücrete göre sıralayın:

    $orderby=BaseRate asc

Derecelendirmeye göre azalan otelleri sıralayın, ardından taban hıza göre yükselir (artan ın varsayılan olduğunu unutmayın):

    $orderby=Rating desc,BaseRate

Otellerin derecelendirmeye göre alçalan sıralanın, ardından verilen koordinatlardan uzaklığa göre yükselin:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Otelleri azalan sırada search.score ve rating'e göre sıralayın ve ardından verilen koordinatlardan uzaklığa göre artan sırada sırayla sıralayın. Aynı alaka puanları ve derecelendirmeleri ile iki otel arasında, en yakın ilk listelenir:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'da arama sonuçlarıyla çalışma](search-pagination-page-layout.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
