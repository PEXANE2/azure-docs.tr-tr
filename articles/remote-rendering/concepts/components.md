---
title: Bileşenler
description: Azure Uzaktan İşlem kapsamında bileşenlerin tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: conceptual
ms.openlocfilehash: cb8b38addef736914a8627971e57ea2b173293d6
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681902"
---
# <a name="components"></a>Bileşenler

Azure Uzaktan [İşleme, Varlık Bileşen Sistemi](https://en.wikipedia.org/wiki/Entity_component_system) modelini kullanır. [Varlıklar](entities.md) nesnelerin konumunu ve hiyerarşik bileşimini temsil ederken, bileşenler davranışı uygulamaktan sorumludur.

En sık kullanılan bileşen türleri, işleme ardışık bölümüne kafes ekleyen [kafes bileşenleridir.](meshes.md) Benzer şekilde, [ışık bileşenleri](../overview/features/lights.md) aydınlatma eklemek ve [düzlem bileşenleri açık](../overview/features/cut-planes.md) meshes kesmek için kullanılır.

Tüm bu bileşenler, referans noktası olarak bağlı oldukları varlığın dönüşümunu (konum, döndürme, ölçek) kullanır.

## <a name="working-with-components"></a>Bileşenlerle çalışma

Bileşenleri kolayca ekleyebilir, kaldırabilir ve işleyebilirsiniz:

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

Bir bileşen oluşturma zamanında bir varlığa eklenir. Daha sonra başka bir varlığa taşınamaz. Bileşenler, bileşenin `Component.Destroy()` sahibi varlığı yok edildiğinde veya otomatik olarak silinir.

Bir varlığa aynı anda her bileşen türünün yalnızca bir örneği eklenebilir.

## <a name="unity-specific"></a>Birliğe özel

Birletümleştirme, bileşenlerle etkileşim kurmak için ek uzatma işlevlerine sahiptir. Bkz. [Unity oyun nesneleri ve bileşenleri.](../how-tos/unity/objects-components.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Nesne sınırları](object-bounds.md)
* [Kafes](meshes.md)
