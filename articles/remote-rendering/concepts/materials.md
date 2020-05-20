---
title: Malzemeler
description: İşleme malzemesi açıklaması ve malzeme özellikleri
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: conceptual
ms.openlocfilehash: 1a45dc39f5a0795684aba278391e04f6af3f7b55
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83659676"
---
# <a name="materials"></a>Malzemeler

Malzemeler, [kafeslerin](meshes.md) nasıl işleneceğini tanımlayan [paylaşılan kaynaklardır](../concepts/lifetime.md) . Malzemeler hangi [dokuların](textures.md) uygulanacağını, nesnelerin saydam hale yapılıp yapılmayacağını ve aydınlatma 'un nasıl hesaplanacağını belirtmek için kullanılır.

Malzemeler [model dönüştürme](../how-tos/conversion/model-conversion.md) sırasında otomatik olarak oluşturulur ve çalışma zamanında erişilebilir. Ayrıca, koddan özel malzemeler oluşturabilir ve var olanları değiştirebilirsiniz. Bu senaryo, birçok kafesde aynı malzemeleri paylaşmak istiyorsanız özellikle anlamlı hale gelir. Bir malzemenin değişiklikleri kendisine başvuran her bir kafesde görünür olduğundan, bu yöntem kolayca değişiklikler uygulamak için kullanılabilir.

> [!NOTE]
> Çekilen bir nesneyi vurgulama gibi bazı kullanım durumları, malzemeler değiştirilerek yapılabilir, ancak [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md)aracılığıyla çok daha kolay bir şekilde elde edilebilir.

## <a name="material-types"></a>Malzeme türleri

Azure uzaktan Işleme iki ayrı malzeme türüne sahiptir:

* [PBR malzemeleri](../overview/features/pbr-materials.md) , mümkün olduğunca fiziksel olarak doğru işlenmesi gereken yüzeyler için kullanılır. *Fiziksel tabanlı işleme* (PBR) kullanılarak bu malzemeler için gerçekçi aydınlatma hesaplanır. Bu malzeme türünden en iyi şekilde yararlanmak için, kabalık ve normal haritalar gibi yüksek kaliteli giriş verileri sağlamak önemlidir.

* [Renk malzemeleri](../overview/features/color-materials.md) , hiçbir ek aydınlatma istenmiyorsa durumlar için kullanılır. Bu malzemeler her zaman tamamen parlak ve daha kolay ayarlanır. Renk malzemeleri, hiç ışık olmaması gereken ya da [photogrammetri](https://en.wikipedia.org/wiki/Photogrammetry)aracılığıyla elde edilen modeller gibi statik aydınlatma içeren veriler için kullanılır.

## <a name="mesh-vs-meshcomponent-material-assignment"></a>Kafes ile MeshComponent malzeme ataması

[Kafeslerin](meshes.md) bir veya daha fazla alt kafesi vardır. Her alt ağ bir malzemeye başvurur. Ya doğrudan ağ üzerinde kullanmak için malzemeyi değiştirebilir veya bir [Meshcomponent](meshes.md#meshcomponent)üzerinde bir alt ağ için kullanılacak malzemeleri geçersiz kılabilirsiniz.

Bir malzemeyi doğrudan kafes kaynağında değiştirdiğinizde, bu değişiklik o kafesin tüm örneklerini etkiler. Ancak MeshComponent üzerinde değişiklik yapmak yalnızca bir kafes örneğini etkiler. Hangi yöntemin daha uygun olduğu, istenen davranışa bağlıdır, ancak bir MeshComponent 'un değiştirilmesi daha yaygın bir yaklaşımdır.

## <a name="material-de-duplication"></a>Malzeme Çoğaltma sırası

Dönüştürme sırasında aynı özelliklere ve dokularla birden çok malzeme otomatik olarak tek bir malzemeyle çoğaltılır. [Dönüştürme ayarlarında](../how-tos/conversion/configure-model-conversion.md)bu özelliği devre dışı bırakabilirsiniz, ancak en iyi performans için bu özelliği açık bırakmanızı öneririz.

## <a name="material-classes"></a>Malzeme sınıfları

API tarafından sunulan tüm malzemeler taban sınıftan türetilir `Material` . Bu tür aracılığıyla `Material.MaterialSubType` veya doğrudan atama yoluyla sorgulanabilir.

``` cs
void SetMaterialColorToGreen(Material material)
{
    if (material.MaterialSubType == MaterialType.Color)
    {
        ColorMaterial colorMaterial = material as ColorMaterial;
        colorMaterial.AlbedoColor = new Color4(0, 1, 0, 1);
        return;
    }

    PbrMaterial pbrMat = material as PbrMaterial;
    if( pbrMat!= null )
    {
        PbrMaterial pbrMaterial = material.PbrMaterial.Value;
        pbrMaterial.AlbedoColor = new Color4(0, 1, 0, 1);
        return;
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [PBR malzemeleri](../overview/features/pbr-materials.md)
* [Renk malzemeleri](../overview/features/color-materials.md)
