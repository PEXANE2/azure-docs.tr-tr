---
title: Eşleme veri akışında filtre dönüşümü
description: Azure Data Factory eşleme veri akışındaki filtre dönüşümünü kullanarak satırları filtreleme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: 32a40f3d4da93318c6d2ec25295c89a7b53141e4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81606434"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Eşleme veri akışında filtre dönüşümü

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

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

Aşağıdaki örnek, gelen Stream `FilterBefore1960` `CleanData`'e ait adlı bir filtre dönüştürmesinin yer aldığı bir filtredir. Filtre koşulu ifadedir `year <= 1960`.

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
