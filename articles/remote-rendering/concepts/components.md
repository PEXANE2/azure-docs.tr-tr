---
title: Bileşenler
description: Azure uzaktan Işleme kapsamındaki bileşenlerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.openlocfilehash: 42efc1395fee28a3854420f15c2ad08b301cc1f7
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758750"
---
# <a name="components"></a>Bileşenler

Azure uzaktan Işleme, [varlık bileşeni sistem](https://en.wikipedia.org/wiki/Entity_component_system) modelini kullanır. [Varlıklar](entities.md) , nesnelerin konumunu ve hiyerarşik birleşimini temsil ederken, davranış uygulamaktan bileşenler sorumludur.

En sık kullanılan bileşen türleri, işleme işlem hattına kafesler ekleyen [kafes bileşenleridir](meshes.md). Benzer şekilde, açık kafesler eklemek için aydınlatma ve [kesilen düzlem bileşenleri](../overview/features/cut-planes.md) eklemek için de [hafif bileşenler](../overview/features/lights.md) kullanılır.

Tüm bu bileşenler, bağlı oldukları varlığın başvuru noktası olarak dönüşümünü (konum, döndürme, ölçek) kullanır.

## <a name="working-with-components"></a>Bileşenlerle çalışma

Programlı olarak bileşenleri kolayca ekleyebilir, kaldırabilir ve yönetebilirsiniz:

```cs
// create a point light component
AzureSession session = GetCurrentlyConnectedSession();
PointLightComponent lightComponent = session.Actions.CreateComponent(ObjectType.PointLightComponent, ownerEntity) as PointLightComponent;

lightComponent.Color = new Color4Ub(255, 150, 20, 255);
lightComponent.Intensity = 11;

// ...

// destroy the component
lightComponent.Destroy();
lightComponent = null;
```

```cpp
// create a point light component
ApiHandle<AzureSession> session = GetCurrentlyConnectedSession();

ApiHandle<PointLightComponent> lightComponent = session->Actions()->CreateComponent(ObjectType::PointLightComponent, ownerEntity)->as<PointLightComponent>();

// ...

// destroy the component
lightComponent->Destroy();
lightComponent = nullptr;
```


Bir bileşen, oluşturma zamanında bir varlığa eklenir. Daha sonra başka bir varlığa taşınamaz. Bileşenler `Component.Destroy()` , bileşenin sahip varlığı yok edildiğinde açıkça veya otomatik olarak silinir.

Her bir bileşen türünün yalnızca bir örneği bir varlığa bir seferde eklenebilir.

## <a name="unity-specific"></a>Unity 'ye özgü

Unity tümleştirmesinde bileşenlerle etkileşim kurmak için ek uzantı işlevleri bulunur. Bkz. [Unity oyun nesneleri ve bileşenleri](../how-tos/unity/objects-components.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Nesne sınırları](object-bounds.md)
* [Ağ yapıları](meshes.md)
