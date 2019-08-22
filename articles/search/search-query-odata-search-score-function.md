---
title: OData Search. Score işlev başvurusu-Azure Search
description: Azure Search sorgularında OData Search. Score işlevi.
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
ms.openlocfilehash: b6bf56a61ca685b306a15e474623336216ba531b
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647525"
---
# <a name="odata-searchscore-function-in-azure-search"></a>Azure Search `search.score` OData işlevi

[ **$OrderBy** parametresi](search-query-odata-orderby.md)olmadan Azure Search bir sorgu gönderdiğinizde, geri gelen sonuçlar ilgi puanı tarafından azalan düzende sıralanır. **$OrderBy**kullandığınızda bile, uygunluk puanı varsayılan olarak bu özellikleri bölmek için kullanılacaktır. Ancak, bazen bir ilk sıralama ölçütü olarak ilgi puanı ve diğer bazı kriterleri bağlama kesici olarak kullanmak faydalıdır. `search.score` İşlevi bunu yapmanıza olanak sağlar.

## <a name="syntax"></a>Sözdizimi

`search.score` **$OrderBy** 'deiçinsözdizimi.`search.score()` İşlev `search.score` hiçbir parametre almaz. **$OrderBy** parametresindeki diğer yan tümcelerde `asc` olduğu `desc` gibi, veya sıralama düzeni tanımlayıcısıyla birlikte kullanılabilir. Sıralama ölçütü listesinde herhangi bir yerde görünebilir.

## <a name="example"></a>Örnek

Oteller `search.score` ve ardından, aynı derecelendirmelere `rating`sahip iki otel arasında, en yakın bir değer olacak şekilde, ve daha sonra verilen koordinatlardan uzaklıktan artan düzende bir şekilde sıralayın.

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
