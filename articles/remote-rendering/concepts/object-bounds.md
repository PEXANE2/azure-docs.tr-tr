---
title: Nesne sınırları
description: Uzamsal nesne sınırlarının nasıl sorgulanabileceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 1d2dfdb203b05f2f6b7de740718d7407bd88066c
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681720"
---
# <a name="object-bounds"></a>Nesne sınırları

Nesne sınırları, bir [varlığın](entities.md) ve çocuklarının kapsadığı birimi temsil eder. Azure Uzaktan İşleme'de nesne sınırları her zaman *eksen hizalı sınırlayıcı kutular* (AABB) olarak verilir. Nesne sınırları *yerel alanda* veya *dünya alanında*olabilir. Her iki şekilde de, her zaman eksen hizalı, hangi ölçüde ve hacim yerel ve dünya alanı gösterimi arasında farklı olabilir anlamına gelir.

## <a name="querying-object-bounds"></a>Nesne sınırlarını sorgulama

Bir [kafesin](meshes.md) yerel AABB'si doğrudan kafes kaynağından sorgulanabilir. Bu sınırlar, varlığın dönüşümü kullanılarak bir varlığın yerel uzayına veya dünya alanına dönüştürülebilir.

Tüm nesne hiyerarşisinin sınırlarını bu şekilde hesaplamak mümkündür, ancak bu hiyerarşide geçiş yapmak, her kafesiçin sınırları sorgulamak ve bunları el ile birleştirmek gerekir. Bu operasyon hem sıkıcı hem de verimsiz.

Daha iyi bir `QueryLocalBoundsAsync` yol `QueryWorldBoundsAsync` aramak veya bir varlık üzerinde. Hesaplama daha sonra sunucuya boşaltılır ve en az gecikmeyle döndürülür.

``` cs
private BoundsQueryAsync _boundsQuery = null;

public void GetBounds(Entity entity)
{
    _boundsQuery = entity.QueryWorldBoundsAsync();
    _boundsQuery.Completed += (BoundsQueryAsync bounds) =>
    {
        if (bounds.IsRanToCompletion)
        {
            Double3 aabbMin = bounds.Result.min;
            Double3 aabbMax = bounds.Result.max;
            // ...
        }
    };
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Uzamsal sorgular](../overview/features/spatial-queries.md)
