---
title: Nesne sınırları
description: Uzamsal nesne sınırlarının nasıl sorgulanabilecek açıklanmaktadır
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 1d2dfdb203b05f2f6b7de740718d7407bd88066c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681720"
---
# <a name="object-bounds"></a>Nesne sınırları

Nesne sınırları, bir [varlığın](entities.md) ve alt öğelerinin kaplayacağı birimi temsil eder. Azure uzaktan Işlemede, nesne sınırları her zaman *eksen hizalanmış sınırlayıcı kutular* (AABB) olarak verilir. Nesne sınırları, *yerel alanda* veya *Dünya*alanında olabilir. Her iki durumda da, her zaman eksen hizalı olur, bu da kapsamlar ve birim yerel ve dünya alanı gösterimi arasında farklılık gösterebilir.

## <a name="querying-object-bounds"></a>Nesne sınırlarını sorgulama

Bir [kafesin](meshes.md) yerel AABB ' si doğrudan ağ kaynağından sorgulanabilir. Bu sınırlar, varlığın dönüşümünü kullanarak bir varlığın yerel alanına veya dünya alanına dönüştürülebilir.

Bu şekilde tüm nesne hiyerarşisinin sınırlarını hesaplamak mümkündür, ancak hiyerarşide geçiş yapmak, her bir kafesin sınırlarını sorgulamak ve bunları el ile birleştirmek gerekir. Bu işlem hem sıkıcı hem de verimsiz.

Daha iyi bir yol, bir `QueryLocalBoundsAsync` varlığı `QueryWorldBoundsAsync` çağırmak ya da bir varlığı kullanmaktır. Hesaplama daha sonra sunucuda boşaltılır ve en az gecikmeyle döndürülür.

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
