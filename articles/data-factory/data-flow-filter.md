---
title: Veri akışını eşlemede filtre dönüşümü
description: Azure Veri Fabrikası eşleme veri akışında filtre dönüşümlerini kullanarak satırları filtreleyin
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: 84b935d1646ea4f13d5926fc68797fd8efd03dea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930366"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede filtre dönüşümü

Filtre dönüşümleri, bir koşula göre satır filtreleme sağlar. Çıktı akışı, filtreleme koşuluyla eşleşen tüm satırları içerir. Filtre dönüştürme, SQL'deki WHERE yan tümcesine benzer.

## <a name="configuration"></a>Yapılandırma

Filtre koşulu için bir ifade girmek için veri akışı ifadesi oluşturucuyu kullanın. İfade oluşturucuyu açmak için mavi kutuya tıklayın. Filtre koşulu tip boolean olmalıdır. İfade oluşturma hakkında daha fazla bilgi için [ifade oluşturucu](concepts-data-flow-expression-builder.md) belgelerine bakın.

![Filtre dönüşümü](media/data-flow/filter1.png "Filtre dönüşümü")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
    filter(
        <conditionalExpression>
    ) ~> <filterTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, gelen akışı `FilterBefore1960` `CleanData`alan bir filtre dönüştürme. Filtre koşulu ifadedir. `year <= 1960`

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Filtre dönüşümü](media/data-flow/filter1.png "Filtre dönüşümü")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
CleanData
    filter(
        year <= 1960
    ) ~> FilterBefore1960

```

## <a name="next-steps"></a>Sonraki adımlar

Seç ile dönüştürme yle sütunları [filtreleyin](data-flow-select.md)
