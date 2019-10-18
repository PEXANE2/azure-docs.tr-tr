---
title: Azure Data Factory eşleme veri akışında koşullu bölünmüş dönüşüm | Microsoft Docs
description: Azure Data Factory eşleme veri akışındaki koşullu bölünmüş dönüştürmeyi kullanarak verileri farklı akışlara bölme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: 2d794714f27340e8886843988b6c075dd8d3366e
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72527430"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Eşleme veri akışında koşullu bölünmüş dönüştürme

Koşullu bölünmüş dönüştürme, veri satırlarını eşleşen koşullara göre farklı akışlara yönlendirir. Koşullu bölünmüş dönüştürme, programlama dilindeki bir CASE karar yapısına benzerdir. Dönüştürme ifadeleri değerlendirir ve sonuçlara dayanarak, veri satırını belirtilen akışa yönlendirir.

## <a name="configuration"></a>Yapılandırma

**Bölme açık** ayarı, verilerin satırının ilk eşleşen akışa mı yoksa eşleşen her akışa mı akacağını belirler.

Bölünmüş koşul için bir ifade girmek üzere veri akışı ifade oluşturucusunu kullanın. Yeni bir koşul eklemek için, varolan bir satırdaki artı simgesine tıklayın. Herhangi bir koşulla eşleşmeyen satırlar için varsayılan akış eklenebilir.

![Koşullu bölme](media/data-flow/conditionalsplit1.png "Koşullu bölünmüş seçenekler")

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

Aşağıdaki örnek, gelen akış `CleanData` alan `SplitByYear` adlı koşullu bölünmüş dönüşümdir. Bu dönüşüme `year < 1960` ve `year > 1980` iki bölme koşulu vardır. veriler ilk eşleşen koşula ulaştığı için `disjoint` false. İlk koşulla eşleşen her satır, çıkış akışına `moviesBefore1960` gider. İkinci koşulla eşleşen tüm kalan satırlar çıkış akışı `moviesAFter1980` gider. Varsayılan akış `AllOtherMovies` tüm diğer satırlar akar.

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Koşullu bölme](media/data-flow/conditionalsplit1.png "Koşullu bölünmüş seçenekler")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında verilmiştir:

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>Sonraki adımlar

Koşullu bölme ile kullanılan ortak veri akışı dönüştürmeleri, [JOIN dönüşümü](data-flow-join.md), [arama dönüşümü](data-flow-lookup.md)ve [seçim dönüşümünüzün](data-flow-select.md)
