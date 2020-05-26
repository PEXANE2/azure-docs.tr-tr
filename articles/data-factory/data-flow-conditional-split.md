---
title: Eşleme veri akışında koşullu bölünmüş dönüştürme
description: Azure Data Factory eşleme veri akışındaki koşullu bölünmüş dönüştürmeyi kullanarak verileri farklı akışlara bölme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/21/2020
ms.openlocfilehash: eece6f97e82f3800d4f59ac1849b34c2a1e4635b
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800079"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Eşleme veri akışında koşullu bölünmüş dönüştürme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Koşullu bölünmüş dönüştürme, veri satırlarını eşleşen koşullara göre farklı akışlara yönlendirir. Koşullu bölünmüş dönüştürme, programlama dilindeki bir CASE karar yapısına benzerdir. Dönüştürme ifadeleri değerlendirir ve sonuçlara dayanarak, veri satırını belirtilen akışa yönlendirir.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4wKCX]

## <a name="configuration"></a>Yapılandırma

**Bölme açık** ayarı, verilerin satırının ilk eşleşen akışa mı yoksa eşleşen her akışa mı akacağını belirler.

Bölünmüş koşul için bir ifade girmek üzere veri akışı ifade oluşturucusunu kullanın. Yeni bir koşul eklemek için, varolan bir satırdaki artı simgesine tıklayın. Herhangi bir koşulla eşleşmeyen satırlar için varsayılan akış eklenebilir.

![Koşullu bölme](media/data-flow/conditionalsplit1.png "Koşullu bölünmüş seçenekler")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Söz dizimi

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

Aşağıdaki örnek, gelen akışı alan adlı, koşullu bölünmüş bir dönüşümdir `SplitByYear` `CleanData` . Bu dönüşümde iki bölünmüş koşul `year < 1960` ve `year > 1980` . `disjoint`veriler ilk eşleşen koşula gittiğinden false. İlk koşulla eşleşen her satır çıkış akışına gider `moviesBefore1960` . İkinci koşulla eşleşen tüm kalan satırlar çıkış akışına gider `moviesAFter1980` . Diğer tüm satırlar varsayılan akış üzerinden akar `AllOtherMovies` .

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
