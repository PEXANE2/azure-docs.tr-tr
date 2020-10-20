---
title: Ağ yapıları
description: Azure uzaktan Işleme kapsamındaki kafeslerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 628331677a84f6cf1dd8fa1df1e7f6c1a2fa09e1
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92202812"
---
# <a name="meshes"></a>Ağ yapıları

## <a name="mesh-resource"></a>Kafes kaynağı

Kafesler yalnızca [model dönüştürme](../how-tos/conversion/model-conversion.md)ile oluşturulabilecek sabit bir [paylaşılan kaynaktır](../concepts/lifetime.md). Kafesler, [raycast sorguları](../overview/features/spatial-queries.md)için bir veya daha fazla *alt kafes* ve bir fizik gösterimiyle birlikte bulunur. Her alt ağ, varsayılan olarak işlenmesi gereken bir [malzemeye](materials.md) başvurur. 3B alana bir ağ yerleştirmek için bir [varlığa](entities.md)bir [meshcomponent](#meshcomponent) ekleyin.

### <a name="mesh-resource-properties"></a>Kafes kaynağı özellikleri

`Mesh`Sınıf özellikleri şunlardır:

* **Malzemeler:** Bir dizi malzeme. Her malzeme farklı bir alt ağ tarafından kullanılır. Dizide birden çok girdi aynı [malzemeye](materials.md)başvurabilir. Bu veriler çalışma zamanında değiştirilemez.

* **Sınırlar:** Kafes köşelerine ait yerel alan ekseni hizalanmış bir sınırlayıcı kutusu (AABB).

## <a name="meshcomponent"></a>MeshComponent

`MeshComponent`Sınıf, bir kafes kaynağı örneğini yerleştirmek için kullanılır. Her bir MeshComponent tek bir kafes 'e başvurur. Her bir alt ağı işlemek için kullanılan malzemeleri geçersiz kılabilir.

### <a name="meshcomponent-properties"></a>MeshComponent özellikleri

* **Ağ:** Bu bileşen tarafından kullanılan kafes kaynağı.

* **Malzemeler:** Kafes bileşeninin kendisinde belirtilen malzeme dizisi. Dizi, her zaman kafes kaynağındaki *malzemeler* dizisiyle aynı uzunluğa sahip olacaktır. Bu dizide varsayılan olarak geçersiz kılınamayan malzemeler, bu dizide *null* olarak ayarlanır.

* **Usedmalzemeleri:** Her alt ağ için gerçekten kullanılan malzemeler dizisi. Null olmayan değerler için, *malzemeler* dizisindeki verilerle aynı olacaktır. Aksi takdirde, kafes örneğindeki *malzemeler* dizisinden değeri içerir.

### <a name="sharing-of-meshes"></a>Kafeslerin paylaşılması

Bir `Mesh` kaynak, birden fazla kafes bileşeni örneği arasında paylaşılabilir. Ayrıca, `Mesh` bir ağ bileşenine atanan kaynak, programlama yoluyla dilediğiniz zaman değiştirilebilir. Aşağıdaki kod, bir ağı nasıl klonlanacağını göstermektedir:

```cs
Entity CloneEntityWithModel(RemoteManager manager, Entity sourceEntity)
{
    MeshComponent meshComp = sourceEntity.FindComponentOfType<MeshComponent>();
    if (meshComp != null)
    {
        Entity newEntity = manager.CreateEntity();
        MeshComponent newMeshComp = manager.CreateComponent(ObjectType.MeshComponent, newEntity) as MeshComponent;
        newMeshComp.Mesh = meshComp.Mesh; // share the mesh
        return newEntity;
    }
    return null;
}
```

```cpp
ApiHandle<Entity> CloneEntityWithModel(ApiHandle<RemoteManager> manager, ApiHandle<Entity> sourceEntity)
{
    if (ApiHandle<MeshComponent> meshComp = sourceEntity->FindComponentOfType<MeshComponent>())
    {
        ApiHandle<Entity> newEntity = *manager->CreateEntity();
        ApiHandle<MeshComponent> newMeshComp = manager->CreateComponent(ObjectType::MeshComponent, newEntity)->as<RemoteRendering::MeshComponent>();
        newMeshComp->SetMesh(meshComp->GetMesh()); // share the mesh
        return newEntity;
    }
    return nullptr;
}
```

## <a name="api-documentation"></a>API belgeleri

* [C# kafes sınıfı](/dotnet/api/microsoft.azure.remoterendering.mesh)
* [C# MeshComponent sınıfı](/dotnet/api/microsoft.azure.remoterendering.meshcomponent)
* [C++ kafes sınıfı](/cpp/api/remote-rendering/mesh)
* [C++ MeshComponent sınıfı](/cpp/api/remote-rendering/meshcomponent)


## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)