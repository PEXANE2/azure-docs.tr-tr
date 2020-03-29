---
title: Veri akışını nakışlamada koşullu bölme dönüşümü
description: Azure Veri Fabrikası eşleme veri akışındaki koşullu bölme dönüşümunu kullanarak verileri farklı akışlara bölme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: d7e2af6c98951e685192656b37226716e4340bfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930438"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Veri akışını nakışlamada koşullu bölme dönüşümü

Koşullu bölme dönüştürme, veri satırlarını eşleşen koşullara göre farklı akışlara yönlendirir. Koşullu bölme dönüşümü, programlama dilindeki CASE karar yapısına benzer. Dönüştürme ifadeleri değerlendirir ve sonuçlara göre, veri satırını belirtilen akışa yönlendirir.

## <a name="configuration"></a>Yapılandırma

Ayardaki **Bölme,** veri satırının ilk eşleşen akışa mı yoksa eşleştiğini her akışa mı aktığını belirler.

Bölme koşulu için bir ifade girmek için veri akışı ifade oluşturucusu kullanın. Yeni bir koşul eklemek için, varolan bir satırdaki artı simgesine tıklayın. Varsayılan akış, herhangi bir koşulla eşleşmeyen satırlar için de eklenebilir.

![koşullu bölünme](media/data-flow/conditionalsplit1.png "koşullu bölme seçenekleri")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, gelen akışı `SplitByYear` `CleanData`alan koşullu bir bölme dönüşümüdür. Bu dönüşümün iki `year < 1960` `year > 1980`bölünmüş koşulu vardır ve . `disjoint`veriler ilk eşleşen koşula gittiğinden yanlıştır. İlk koşulla eşleşen her satır `moviesBefore1960`çıkış akışına gider. İkinci koşulla eşleşen kalan tüm satırlar çıkış akışına `moviesAFter1980`gider. Diğer tüm satırlar varsayılan `AllOtherMovies`akış boyunca akar.

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![koşullu bölünme](media/data-flow/conditionalsplit1.png "koşullu bölme seçenekleri")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>Sonraki adımlar

Koşullu bölme ile kullanılan ortak veri akışı dönüşümleri [birleştirme dönüşümü,](data-flow-join.md) [arama dönüşümü](data-flow-lookup.md)ve [seçili dönüşüm](data-flow-select.md)
