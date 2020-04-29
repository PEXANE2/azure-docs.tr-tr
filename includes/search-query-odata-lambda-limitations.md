---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: 998d0f1a84dc9cb2a07fb55286c1089787a263e1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80272628"
---
| Veri türü | İle Lambda ifadelerinde izin verilen özellikler`any` | İle Lambda ifadelerinde izin verilen özellikler`all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Ve dışındaki `search.ismatch` her şey`search.ismatchscoring` | Aynı |
| `Collection(Edm.String)` | Veya ile `eq` karşılaştırmalar`search.in` <br/><br/> Alt ifadeleri ile birleştirme`or` | Veya ile `ne` karşılaştırmalar`not search.in()` <br/><br/> Alt ifadeleri ile birleştirme`and` |
| `Collection(Edm.Boolean)` | Veya ile `eq` karşılaştırmalar`ne` | Aynı |
| `Collection(Edm.GeographyPoint)` | Veya `geo.distance` ile `lt` kullanma`le` <br/><br/> `geo.intersects` <br/><br/> Alt ifadeleri ile birleştirme`or` | Veya `geo.distance` ile `gt` kullanma`ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Alt ifadeleri ile birleştirme`and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | ,, `eq`, `ne`, `lt` `le`, `gt`Veya kullanarak karşılaştırmalar`ge` <br/><br/> Kullanarak karşılaştırmaları diğer alt ifadelerle birleştirme`or` <br/><br/> Kullanılarak diğer alt `ne` ifadelerle karşılaştırmalar hariç karşılaştırmaları birleştirme`and` <br/><br/> Ayırt edici normal formda `and` ve `or` birleşimleri kullanan ifadeler [(DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | ,, `eq`, `ne`, `lt` `le`, `gt`Veya kullanarak karşılaştırmalar`ge` <br/><br/> Kullanarak karşılaştırmaları diğer alt ifadelerle birleştirme`and` <br/><br/> Kullanılarak diğer alt `eq` ifadelerle karşılaştırmalar hariç karşılaştırmaları birleştirme`or` <br/><br/> `and` `or` [CONJUN, normal form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) içinde ve birleşimlerini kullanan ifadeler |
