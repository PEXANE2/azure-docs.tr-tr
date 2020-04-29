---
title: Eşleme veri akışında koşullu bölünmüş dönüştürme
description: Azure Data Factory eşleme veri akışındaki koşullu bölünmüş dönüştürmeyi kullanarak verileri farklı akışlara bölme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: bd9241e526d7cf42f0697afb8635c085a08c80d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606476"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Eşleme veri akışında koşullu bölünmüş dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

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

Aşağıdaki örnek, gelen akışı `SplitByYear` `CleanData`alan adlı, koşullu bölünmüş bir dönüşümdir. Bu dönüşümde iki bölünmüş koşul `year < 1960` ve `year > 1980`. `disjoint`veriler ilk eşleşen koşula gittiğinden false. İlk koşulla eşleşen her satır çıkış akışına `moviesBefore1960`gider. İkinci koşulla eşleşen tüm kalan satırlar çıkış akışına `moviesAFter1980`gider. Diğer tüm satırlar varsayılan akış `AllOtherMovies`üzerinden akar.

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
