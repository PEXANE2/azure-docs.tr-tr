---
title: Azure Data Factory eşleme veri akışında filtre dönüşümü
description: Azure Data Factory eşleme veri akışındaki filtre dönüşümünü kullanarak satırları filtreleme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/16/2019
ms.openlocfilehash: 77587d007d9bfcdc1461201f676c79880bd89d6b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73676169"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Eşleme veri akışında filtre dönüşümü

Filtre dönüştürmeleri bir koşula göre satır filtrelemesine izin verir. Çıkış akışı, filtreleme durumuyla eşleşen tüm satırları içerir. Filtre dönüşümü SQL 'deki WHERE yan tümcesine benzerdir.

## <a name="configuration"></a>Yapılandırma

Filtre koşulu için bir ifade girmek üzere veri akışı ifade oluşturucusunu kullanın. İfade oluşturucuyu açmak için mavi kutuya tıklayın. Filtre koşulu Boolean türünde olmalıdır. Bir ifade oluşturma hakkında daha fazla bilgi için bkz. [Expression Builder](concepts-data-flow-expression-builder.md) belgeleri.

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

Aşağıdaki örnek, gelen akış `CleanData`alan `FilterBefore1960` adlı koşullu bölünmüş dönüşümdir. Filtre koşulu `year <= 1960`ifadedir.

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Filtre dönüşümü](media/data-flow/filter1.png "Filtre dönüşümü")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında verilmiştir:

```
CleanData
    filter(
        year <= 1960
    ) ~> FilterBefore1960

```

## <a name="next-steps"></a>Sonraki adımlar

Sütunları [seçim dönüşümüne](data-flow-select.md) göre filtrele
