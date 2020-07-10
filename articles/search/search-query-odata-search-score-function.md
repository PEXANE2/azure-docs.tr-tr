---
title: OData Search. Score işlev başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında Search. Score işlevinin kullanılmasına yönelik söz dizimi ve başvuru belgeleri.
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
ms.openlocfilehash: c31304228d9629b0df7f7511ecca2616b4891ee7
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206958"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>`search.score`Azure bilişsel arama 'de OData işlevi

Azure Bilişsel Arama [ **$OrderBy** parametresi](search-query-odata-orderby.md)olmadan bir sorgu gönderdiğinizde, geri gelen sonuçlar ilgi puanı tarafından azalan düzende sıralanır. **$OrderBy**kullandığınızda bile, uygunluk puanı varsayılan olarak bu özellikleri bölmek için kullanılacaktır. Ancak, bazen bir ilk sıralama ölçütü olarak ilgi puanı ve diğer bazı kriterleri bağlama kesici olarak kullanmak faydalıdır. `search.score`İşlevi bunu yapmanıza olanak sağlar.

## <a name="syntax"></a>Sözdizimi

`search.score` **$OrderBy** ' de için sözdizimi `search.score()` . İşlev `search.score` hiçbir parametre almaz. `asc` `desc` **$OrderBy** parametresindeki diğer yan tümcelerde olduğu gibi, veya sıralama düzeni tanımlayıcısıyla birlikte kullanılabilir. Sıralama ölçütü listesinde herhangi bir yerde görünebilir.

## <a name="example"></a>Örnek

Oteller `search.score` ve `rating` ardından, aynı derecelendirmelere sahip iki otel arasında, en yakın bir değer olacak şekilde, ve daha sonra verilen koordinatlardan uzaklıktan artan düzende bir şekilde sıralayın.

```odata-filter-expr
    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama EST API 'SI &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
