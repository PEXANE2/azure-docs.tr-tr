---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: 998d0f1a84dc9cb2a07fb55286c1089787a263e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80272628"
---
| Veri türü | Lambda ifadelerinde izin verilen özellikler`any` | Lambda ifadelerinde izin verilen özellikler`all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Her `search.ismatch` şey dışında ve`search.ismatchscoring` | Aynı |
| `Collection(Edm.String)` | Karşılaştırmalar `eq` ile veya`search.in` <br/><br/> Alt ifadeleri ile birleştirme`or` | Karşılaştırmalar `ne` ile veya`not search.in()` <br/><br/> Alt ifadeleri ile birleştirme`and` |
| `Collection(Edm.Boolean)` | Karşılaştırmalar `eq` ile veya`ne` | Aynı |
| `Collection(Edm.GeographyPoint)` | Kullanarak `geo.distance` `lt` veya`le` <br/><br/> `geo.intersects` <br/><br/> Alt ifadeleri ile birleştirme`or` | Kullanarak `geo.distance` `gt` veya`ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Alt ifadeleri ile birleştirme`and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | Karşılaştırmalar `eq`kullanarak `ne` `lt`, `gt` `le`, , , veya`ge` <br/><br/> Karşılaştırmaları diğer alt ifadelerle birleştirme`or` <br/><br/> Kullanarak diğer alt `ne` ifadeler dışında karşılaştırmalar birleştirme`and` <br/><br/> `and` [Disjunctive Normal Form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) kombinasyonları `or` ve ifadeler kullanarak ifadeler | Karşılaştırmalar `eq`kullanarak `ne` `lt`, `gt` `le`, , , veya`ge` <br/><br/> Karşılaştırmaları diğer alt ifadelerle birleştirme`and` <br/><br/> Kullanarak diğer alt `eq` ifadeler dışında karşılaştırmalar birleştirme`or` <br/><br/> `and` [Konjonktif Normal Form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) kombinasyonlarını `or` kullanan ifadeler |
