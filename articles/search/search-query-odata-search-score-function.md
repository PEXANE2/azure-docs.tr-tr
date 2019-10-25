---
title: OData Search. Score işlev başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData Search. Score işlevi.
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
ms.openlocfilehash: 500ac4f3a44d54e367ddc4ee5efc9514d603cab6
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793263"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>Azure Bilişsel Arama OData `search.score` işlevi

Azure Bilişsel Arama [ **$OrderBy** parametresi](search-query-odata-orderby.md)olmadan bir sorgu gönderdiğinizde, geri gelen sonuçlar ilgi puanı tarafından azalan düzende sıralanır. **$OrderBy**kullandığınızda bile, uygunluk puanı varsayılan olarak bu özellikleri bölmek için kullanılacaktır. Ancak, bazen bir ilk sıralama ölçütü olarak ilgi puanı ve diğer bazı kriterleri bağlama kesici olarak kullanmak faydalıdır. `search.score` işlevi bunu yapmanıza olanak sağlar.

## <a name="syntax"></a>Sözdizimi

**$Orderby** `search.score` söz dizimi `search.score()`. `search.score` işlevi herhangi bir parametre almaz. **$OrderBy** parametresindeki diğer yan tümcelerde olduğu gibi, `asc` veya `desc` sıralama düzeni belirticisi ile kullanılabilir. Sıralama ölçütü listesinde herhangi bir yerde görünebilir.

## <a name="example"></a>Örnek

Oteller `search.score` ve `rating`göre azalan sırada sıralayın, sonra da aynı derecelendirmelere sahip iki otel arasında, en yakın bir tane olmak üzere verilen koordinatlardan uzaklıktan artan sırada sıralama yapın:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure bilişsel arama EST API 'sine arama&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
