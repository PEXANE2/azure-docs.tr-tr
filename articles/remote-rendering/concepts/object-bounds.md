---
title: Nesne sınırları
description: Uzamsal nesne sınırlarının nasıl sorgulanabilecek açıklanmaktadır
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: bd2b1026f69b5ec5814124a92d6a78d7f0225143
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613815"
---
# <a name="object-bounds"></a>Nesne sınırları

Nesne sınırları, bir [varlığın](entities.md) ve alt öğelerinin kaplayacağı birimi temsil eder. Azure uzaktan Işlemede, nesne sınırları her zaman *eksen hizalanmış sınırlayıcı kutular* (AABB) olarak verilir. Nesne sınırları, *yerel alanda* veya *Dünya*alanında olabilir. Her iki durumda da, her zaman eksen hizalı olur, bu da kapsamlar ve birim yerel ve dünya alanı gösterimi arasında farklılık gösterebilir.

## <a name="querying-object-bounds"></a>Nesne sınırlarını sorgulama

Bir [kafesin](meshes.md) yerel AABB ' si doğrudan ağ kaynağından sorgulanabilir. Bu sınırlar, varlığın dönüşümünü kullanarak bir varlığın yerel alanına veya dünya alanına dönüştürülebilir.

Bu şekilde tüm nesne hiyerarşisinin sınırlarını hesaplamak mümkündür, ancak hiyerarşide geçiş yapmak, her bir kafesin sınırlarını sorgulamak ve bunları el ile birleştirmek gerekir. Bu işlem hem sıkıcı hem de verimsiz.

Daha iyi bir yol, `QueryLocalBoundsAsync` bir varlığı çağırmak ya da `QueryWorldBoundsAsync` bir varlığı kullanmaktır. Hesaplama daha sonra sunucuda boşaltılır ve en az gecikmeyle döndürülür.

```cs
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

```cpp
void GetBounds(ApiHandle<Entity> entity)
{
    ApiHandle<BoundsQueryAsync> boundsQuery = *entity->QueryWorldBoundsAsync();
    boundsQuery->Completed([](ApiHandle<BoundsQueryAsync> bounds)
    {
        if (bounds->GetIsRanToCompletion())
        {
            Double3 aabbMin = bounds->GetResult().min;
            Double3 aabbMax = bounds->GetResult().max;
            // ...
        }
    });
}
```

## <a name="api-documentation"></a>API belgeleri

* [C# Entity. QueryLocalBoundsAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.entity.querylocalboundsasync)
* [C++ varlığı:: QueryLocalBoundsAsync](https://docs.microsoft.com/cpp/api/remote-rendering/entity#querylocalboundsasync)

## <a name="next-steps"></a>Sonraki adımlar

* [Uzamsal sorgular](../overview/features/spatial-queries.md)
