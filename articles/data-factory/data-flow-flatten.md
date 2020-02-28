---
title: Veri akışı düzleştirme dönüşümünü eşleme
description: Azure Data Factory eşleme veri akışı düzleştirme dönüştürmesi
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: 435d2469be8eb572caa02e381d84ae4e9ac32f4b
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674850"
---
# <a name="azure-data-factory-flatten-transformation"></a>Azure Data Factory dönüştürmeyi Düzleştir

Düzleştirme dönüştürmesi, hiyerarşik bir yapının içindeki dizi değerlerini yeni satırlara pivot olarak eklemek için kullanılabilir.

![Dönüştürme araç kutusu](media/data-flow/flatten5.png "Dönüştürme araç kutusu")

![Dönüştürmeyi düzleştirme 1](media/data-flow/flatten7.png "Dönüştürmeyi düzleştirme 1")

## <a name="unroll-by"></a>Alma ölçütü

İlk olarak, geri almak istediğiniz dizi sütununu seçin ve Pivot yapın.

![Dönüştürme ayarlarını Düzleştir](media/data-flow/flatten1.png "Dönüştürme ayarlarını Düzleştir")

## <a name="unroll-root"></a>Kökü al

Varsayılan olarak, ADF, yukarıda seçtiğiniz döngüleri dizisinde yapıyı düzleştirebilir. Ya da hiyerarşinin geri alınacağı farklı bir bölümünü seçebilirsiniz.

## <a name="input-columns"></a>Giriş sütunları

Son olarak, gelen alanlara ve topladığınız normalleştirilmiş sütuna göre yeni yapınızın projeksiyonunu seçin.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
foldDown(unroll(<unroll cols>),
    mapColumn(
        name,
        each(<array>(type == '<arrayDataType>')),
        each(<array>, match(true())),
        location
    )) ~> <transformationName>
```

### <a name="example"></a>Örnek

```
source(output(
        name as string,
        location as string,
        satellites as string[],
        goods as (trade as boolean, customers as string[], orders as (orderId as string, orderTotal as double, shipped as (orderItems as (itemName as string, itemQty as string)[]))[])
    ),
    allowSchemaDrift: true,
    validateSchema: false) ~> source2
source2 foldDown(unroll(goods.orders.shipped.orderItems),
    mapColumn(
        name,
        each(goods.orders, match(type == 'integer')),
        each(goods.orders.shipped.orderItems, match(true())),
        location
    )) ~> Flatten1
Flatten1 sink(allowSchemaDrift: true,
    validateSchema: false,
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> sink1
```    

## <a name="next-steps"></a>Sonraki adımlar

* Satırları sütunlara eklemek için [Pivot dönüşümünü](data-flow-pivot.md) kullanın.
* Sütunları satırlara pivot olarak eklemek için [UNPIVOT dönüşümünü](data-flow-unpivot.md) kullanın.
