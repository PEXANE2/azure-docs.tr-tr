---
title: Malzemeler
description: Malzeme tanımıve malzeme özelliklerinin işlenmesi
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: conceptual
ms.openlocfilehash: 8551e17ddd71e76aca0c85b9768f564ae0e5f049
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681850"
---
# <a name="materials"></a>Malzemeler

Malzemeler, [meshes](meshes.md) nasıl işlenir tanımlayan [paylaşılan kaynaklardır.](../concepts/lifetime.md) Malzemeler, hangi [dokuların](textures.md) uygulanacağını, nesnelerin saydam olup olmadığını ve aydınlatmanın nasıl hesaplanacağını belirtmek için kullanılır.

Malzemeler [model dönüştürme](../how-tos/conversion/model-conversion.md) sırasında otomatik olarak oluşturulur ve çalışma zamanında erişilebilir. Ayrıca koddan özel malzemeler oluşturabilir ve varolanmalzemeleri değiştirebilirsiniz. Birçok meshes arasında aynı malzemeyi paylaşmak istiyorsanız, bu senaryo özellikle mantıklı. Bir malzemenin modifikasyonları ona başvuran her kafeste görülebildiğinden, bu yöntem değişiklikleri kolayca uygulamak için kullanılabilir.

> [!NOTE]
> Bazı kullanım örnekleri, örneğin seçilen bir nesneyi vurgulama malzemeleri değiştirerek yapılabilir, ancak [Hiyerarşik StateOverrideComponent](../overview/features/override-hierarchical-state.md)üzerinden elde çok daha kolay.

## <a name="material-types"></a>Malzeme tipleri

Azure Uzaktan İşleme'nin iki farklı malzeme türü vardır:

* [PBR malzemeleri,](../overview/features/pbr-materials.md) mümkün olduğunca fiziksel olarak doğru hale getirilmesi gereken yüzeyler için kullanılır. Gerçekçi aydınlatma *fiziksel tabanlı render* (PBR) kullanılarak bu malzemeler için hesaplanır. Bu malzeme türünden en iyi şekilde elde etmek için pürüzlülük ve normal haritalar gibi yüksek kaliteli giriş verileri sağlamak önemlidir.

* [Renk malzemeleri,](../overview/features/color-materials.md) ek aydınlatma istenmediği durumlar için kullanılır. Bu malzemeler her zaman tam parlak ve kurulumu daha kolaydır. Renk malzemeleri, hiç aydınlatma olmaması gereken veya [fotogrametri](https://en.wikipedia.org/wiki/Photogrammetry)yoluyla elde edilen modeller gibi statik aydınlatmayı zaten içeren veriler için kullanılır.

## <a name="mesh-vs-meshcomponent-material-assignment"></a>Mesh vs. MeshComponent malzeme ataması

[Meshes](meshes.md) bir veya daha fazla submeshes var. Her alt kafes bir malzemeye başvurur. Malzemeyi doğrudan kafeste kullanmak üzere değiştirebilir veya [MeshComponent'ta](meshes.md#meshcomponent)alt ağ için hangi malzemeyi kullanacağınızı geçersiz kılabilirsiniz.

Bir malzemeyi doğrudan kafes kaynağında değiştirdiğinizde, bu değişiklik bu kafesin tüm örneklerini etkiler. Ancak, MeshComponent üzerinde değiştirme, yalnızca bir kafes örneğini etkiler. Hangi yöntemin daha uygun olması istenen davranışa bağlıdır, ancak Bir Kafes Bileşenini değiştirmek daha yaygın bir yaklaşımdır.

## <a name="material-classes"></a>Malzeme sınıfları

API tarafından sağlanan tüm malzemeler taban `Material`sınıftan türetilmiştir. Onların türü aracılığıyla `Material.MaterialSubType` veya doğrudan döküm tarafından sorgulanabilir:

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
* [Renkli malzemeler](../overview/features/color-materials.md)
