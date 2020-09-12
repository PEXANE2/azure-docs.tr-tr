---
title: Bileşenler
description: Azure uzaktan Işleme kapsamındaki bileşenlerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: a488e2499b92b290ad2b55120c3c70a18d45d426
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613941"
---
# <a name="components"></a>Bileşenler

Azure uzaktan Işleme, [varlık bileşeni sistem](https://en.wikipedia.org/wiki/Entity_component_system) modelini kullanır. [Varlıklar](entities.md) , nesnelerin konumunu ve hiyerarşik birleşimini temsil ederken, davranış uygulamaktan bileşenler sorumludur.

En sık kullanılan bileşen türleri [:::no-loc text="mesh components":::](meshes.md) , işleme işlem hattına kafesler ekler. Benzer şekilde, açık kafesler eklemek için aydınlatma ve [kesilen düzlem bileşenleri](../overview/features/cut-planes.md) eklemek için de [hafif bileşenler](../overview/features/lights.md) kullanılır.

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

## <a name="api-documentation"></a>API belgeleri

* [C# ComponentBase](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.componentbase)
* [C# RemoteManager. CreateComponent ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.remotemanager.createcomponent)
* [C# Entity. Findbileşen Entoftype ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.entity.findcomponentoftype)
* [C++ ComponentBase](https://docs.microsoft.com/cpp/api/remote-rendering/componentbase)
* [C++ RemoteManager:: CreateComponent ()](https://docs.microsoft.com/cpp/api/remote-rendering/remotemanager#createcomponent)
* [C++ varlığı:: Findbileşen Entoftype ()](https://docs.microsoft.com/cpp/api/remote-rendering/entity#findcomponentoftype)

## <a name="next-steps"></a>Sonraki adımlar

* [Nesne sınırları](object-bounds.md)
* [Ağ yapıları](meshes.md)
