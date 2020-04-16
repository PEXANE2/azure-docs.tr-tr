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
ms.openlocfilehash: 20def8ca51f21d914e7090999e8284244c5f3ec7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416502"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Veri akışını nakışlamada koşullu bölme dönüşümü

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

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
