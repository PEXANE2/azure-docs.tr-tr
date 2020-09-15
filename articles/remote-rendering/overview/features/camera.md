---
title: Kamera
description: Kamera ayarlarını ve kullanım örneklerini açıklar
author: christophermanthei
ms.author: chmant
ms.date: 03/07/2020
ms.topic: article
ms.openlocfilehash: 5ad39ad9f5e585029cff5d573a026702f259607e
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564917"
---
# <a name="camera"></a>Kamera

Yerel olarak ve uzaktan işlenmiş içeriklerin sorunsuz bir şekilde oluşturulabilir olduğundan emin olmak için, çeşitli kamera özelliklerinin sunucu ve istemci arasında eşitlenmiş durumda kalması gerekir. Çoğu özellikle kamera dönüştürme ve projeksiyon. Örneğin, yerel olarak işlenmiş içerik, en son uzak karenin birlikte işlendiği aynı kamera dönüşümünü ve projeksiyonu kullanmalıdır.

![Yerel ve uzak kamera](./media/camera.png)

Yukarıdaki görüntüde, yerel kamera sunucu tarafından gönderilen uzak çerçeveye göre taşınmıştır. Sonuç olarak, yerel içeriğin uzak kareyle aynı perspektiften oluşturulması gerekir ve son olarak, sonuçta elde edilen görüntü, [geç aşama yeniden projeksiyonundaki](late-stage-reprojection.md)ayrıntılı olarak açıklanan yerel kamera alanına yeniden yansıtılmıştır.

Uzak ve yerel işleme arasındaki gecikme, bu ayarların herhangi bir değişikliğinin gecikmeyle uygulandığı anlamına gelir. Bu nedenle, her yeni değer hemen aynı çerçeve üzerinde kullanılamaz.

Yakın ve en düzlem uzaklıkları Kamera ayarlarında ayarlanabilir. HoloLens 2 ' de, kalan dönüştürme ve projeksiyon verileri donanım tarafından tanımlanır. [Masaüstü benzetimi](../../concepts/graphics-bindings.md)kullanılırken, kendi işlevinin girişi aracılığıyla ayarlanır `Update` .

Değiştirilen değerler, ile işlenen ilk uzak çerçeve geldiğinde yerel olarak yerel olarak kullanılabilir. HoloLens 2 ' de, işleme için kullanılacak veriler başka bir holographic uygulamasında olduğu gibi *Holographicframe* tarafından sağlanır. [Masaüstü simülasyonu](../../concepts/graphics-bindings.md)üzerinde, işlevin çıktısı aracılığıyla alınır `Update` .

## <a name="camera-settings"></a>Kamera ayarları

Kamera ayarlarında aşağıdaki özellikler değiştirilebilir:

**Yakın ve uzak düzlem:**

Geçersiz Aralık ayarlanilemediğinizden emin olmak **için, yeniden, yeniden,** en fazla bir işlev salt **okunurdur ve aralığı** değiştirmek için ayrı bir **SetNearAndFarPlane** vardır. Bu veriler, çerçevenin sonundaki sunucuya gönderilir.

> [!IMPORTANT]
> Unity 'de, ana kameranın yakınında ve en dışı düzlemleri değiştirirken bu otomatik olarak işlenir.

**Enabledepth**:

Bazen hata ayıklama amacıyla uzak görüntünün derinlik arabelleği yazma işlemini devre dışı bırakmak yararlıdır. Derinliğin devre dışı bırakılması, sunucunun derinlik verileri göndermesini ve bu sayede bant genişliğini azaltmanızı da durdurur.

> [!TIP]
> Unity 'de, bu özelliği düzenleyici Kullanıcı arabiriminde değiştirmek için kullanılabilecek **Enabledepthcomponent** adlı bir hata ayıklama bileşeni sağlanır.

Kamera ayarlarının değiştirilmesi şu şekilde yapılabilir:

```cs
void ChangeCameraSetting(AzureSession session)
{
    CameraSettings settings = session.Actions.CameraSettings;

    settings.SetNearAndFarPlane(0.1f, 20.0f);
    settings.EnableDepth = false;
}
```

```cpp
void ChangeStageSpace(ApiHandle<AzureSession> session)
{
    ApiHandle<CameraSettings> settings = session->Actions()->GetCameraSettings();

    settings->SetNearAndFarPlane(0.1f, 20.0f);
    settings->SetEnableDepth(false);
}
```

## <a name="api-documentation"></a>API belgeleri

* [C# CameraSettings](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.camerasettings)
* [C++ CameraSettings](https://docs.microsoft.com/cpp/api/remote-rendering/camerasettings)
* [C# GraphicsBindingSimD3d11. Update işlevi](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.graphicsbindingsimd3d11.update)
* [C++ GraphicsBindingSimD3d11:: Update işlevi](https://docs.microsoft.com/cpp/api/remote-rendering/graphicsbindingsimd3d11#update)

## <a name="next-steps"></a>Sonraki adımlar

* [Grafik bağlama](../../concepts/graphics-bindings.md)
* [Geç aşama yeniden projeksiyonu](late-stage-reprojection.md)
