---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: 998d0f1a84dc9cb2a07fb55286c1089787a263e1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "80272628"
---
| Veri türü | İle Lambda ifadelerinde izin verilen özellikler `any` | İle Lambda ifadelerinde izin verilen özellikler `all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Ve dışındaki her şey `search.ismatch``search.ismatchscoring` | Aynı |
| `Collection(Edm.String)` | Veya ile karşılaştırmalar `eq``search.in` <br/><br/> Alt ifadeleri ile birleştirme `or` | Veya ile karşılaştırmalar `ne``not search.in()` <br/><br/> Alt ifadeleri ile birleştirme `and` |
| `Collection(Edm.Boolean)` | Veya ile karşılaştırmalar `eq``ne` | Aynı |
| `Collection(Edm.GeographyPoint)` | `geo.distance`Veya ile `lt` kullanma`le` <br/><br/> `geo.intersects` <br/><br/> Alt ifadeleri ile birleştirme `or` | `geo.distance`Veya ile `gt` kullanma`ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Alt ifadeleri ile birleştirme `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | ,,,, `eq` `ne` `lt` `gt` `le` , Veya kullanarak karşılaştırmalar `ge` <br/><br/> Kullanarak karşılaştırmaları diğer alt ifadelerle birleştirme `or` <br/><br/> `ne`Kullanılarak diğer alt ifadelerle karşılaştırmalar hariç karşılaştırmaları birleştirme`and` <br/><br/> Ayırt edici `and` normal formda ve birleşimleri kullanan ifadeler `or` [(DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | ,,,, `eq` `ne` `lt` `gt` `le` , Veya kullanarak karşılaştırmalar `ge` <br/><br/> Kullanarak karşılaştırmaları diğer alt ifadelerle birleştirme `and` <br/><br/> `eq`Kullanılarak diğer alt ifadelerle karşılaştırmalar hariç karşılaştırmaları birleştirme`or` <br/><br/> `and` `or` [Conjun, normal form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) içinde ve birleşimlerini kullanan ifadeler |
