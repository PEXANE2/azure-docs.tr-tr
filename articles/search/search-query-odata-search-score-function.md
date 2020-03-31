---
title: OData search.score fonksiyon başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında search.score işlevini kullanmak için sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: 2439d4f03184f8dbb85b229b3908dff95013b4bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113129"
---
# <a name="odata-searchscore-function-in-azure-cognitive-search"></a>Azure `search.score` Bilişsel Arama'da OData işlevi

[ **$orderby** parametresi](search-query-odata-orderby.md)olmadan Azure Bilişsel Arama'ya bir sorgu gönderdiğinde, geri gelen sonuçlar alaka düzeyi puanına göre azalan sırada sıralanır. **$orderby**kullandığınızda bile, alaka düzeyi puanı varsayılan olarak bağları kırmak için kullanılır. Ancak, bazen alaka düzeyi puanının başlangıç sıralama ölçütleri ve diğer bazı ölçütleri de tie-breaker olarak kullanılması yararlıdır. İşlev `search.score` bunu yapmanızı sağlar.

## <a name="syntax"></a>Sözdizimi

`search.score` **$orderby** için sözdizimi `search.score()`. İşlev `search.score` herhangi bir parametre almaz. `asc` **$orderby** parametredeki diğer `desc` tümceler gibi, sıralama sırası belirteci yle birlikte kullanılabilir. Sıralama ölçütleri listesinde herhangi bir yerde görünebilir.

## <a name="example"></a>Örnek

Otelleri azalan sıraya göre `search.score` `rating`sıralayın ve sonra da verilen koordinatlardan uzaklığa göre artan sırada sıralayın, böylece aynı derecelendirmeye sahip iki otel arasında en yakın ıstabir ilk sırada listelenir:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama EST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
