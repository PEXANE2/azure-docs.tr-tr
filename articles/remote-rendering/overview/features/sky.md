---
title: Gökyüzü yansımaları
description: Gökyüzü yansımaları için ortam haritalarının nasıl ayarlanır
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: 7316df7bcf78e3a154510e69116c288b2b293d4c
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680615"
---
# <a name="sky-reflections"></a>Gökyüzü yansımaları

Azure Uzaktan İşleme'de gökyüzü dokusu nesneleri gerçekçi bir şekilde aydınlatmak için kullanılır. Artırılmış gerçeklik uygulamaları için, bu doku nesneleri ikna edici görünmesi için, gerçek dünya çevrenize benzemeye karar vermelidir. Bu makalede, gökyüzü dokusunun nasıl değiştirilen açıklanmıştır.

> [!NOTE]
> Gökyüzü dokusu da bir *çevre haritası*olarak adlandırılır. Bu terimler birbirinin yerine kullanılır.

## <a name="object-lighting"></a>Nesne aydınlatması

Azure Uzaktan İşlem, gerçekçi aydınlatma hesaplamaları için *fiziksel tabanlı görüntüleme* (PBR) kullanır. Sahnenize [ışık kaynakları](lights.md) ekleyebiliyor olabilirsiniz, ancak iyi bir gökyüzü dokusu kullanarak en büyük etkiye sahiptir.

Aşağıdaki resimler, yalnızca bir gökyüzü dokusu ile farklı yüzeyleraydınlatma sonuçlarını göstermektedir:

| Pürüz  | 0                                        | 0,25                                          | 0,5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| Metal Olmayan  | ![Dielektrik0](media/dielectric-0.png)   | ![GreenPointPark](media/dielectric-0.25.png)  | ![GreenPointPark](media/dielectric-0.5.png)  | ![GreenPointPark](media/dielectric-0.75.png)  | ![GreenPointPark](media/dielectric-1.png)  |
| Metal      | ![GreenPointPark](media/metallic-0.png)  | ![GreenPointPark](media/metallic-0.25.png)    | ![GreenPointPark](media/metallic-0.5.png)    | ![GreenPointPark](media/metallic-0.75.png)    | ![GreenPointPark](media/metallic-1.png)    |

Aydınlatma modeli hakkında daha fazla bilgi için [malzeme](../../concepts/materials.md) bölümüne bakın.

> [!IMPORTANT]
> Azure Uzaktan İşlem, gökyüzü dokusunu yalnızca aydınlatma modelleri için kullanır. Artırılmış Gerçeklik uygulamaları zaten uygun bir arka plana sahip olduğundan, bir arka plan olarak gökyüzü işlemez - gerçek dünya.

## <a name="changing-the-sky-texture"></a>Gökyüzü dokusunu değiştirme

Ortam haritasını değiştirmek için tek yapmanız gereken [bir doku yüklemek](../../concepts/textures.md) `SkyReflectionSettings`ve oturumun:

``` cs
LoadTextureAsync _skyTextureLoad = null;
void ChangeEnvironmentMap(AzureSession session)
{
    _skyTextureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams("builtin://VeniceSunset", TextureType.CubeMap));

    _skyTextureLoad.Completed += (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                try
                {
                    session.Actions.SkyReflectionSettings.SkyReflectionTexture = res.Result;
                }
                catch (RRException exception)
                {
                    System.Console.WriteLine($"Setting sky reflection failed: {exception.Message}");
                }
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
        };
}
```

Yerleşik bir `LoadTextureFromSASAsync` doku yüklendiğinden, varyantın yukarıda kullanıldığını unutmayın. [Bağlantılı blob depolarından](../../how-tos/create-an-account.md#link-storage-accounts)yükleme durumunda, `LoadTextureAsync` varyantı kullanın.

## <a name="sky-texture-types"></a>Gökyüzü doku türleri

Hem küp *[eşlelerini](https://en.wikipedia.org/wiki/Cube_mapping)* hem de *2B dokuları* ortam haritaları olarak kullanabilirsiniz.

Tüm dokular desteklenen doku [biçiminde](../../concepts/textures.md#supported-texture-formats)olmalıdır. Gökyüzü dokuları için mipmaps sağlamanız gerekmez.

### <a name="cube-environment-maps"></a>Küp ortam haritaları

Başvuru için, burada unwrapped küp haritası:

![Paketlenmemiş bir küp haritası](media/Cubemap-example.png)

Küp `AzureSession.Actions.LoadTextureAsync` /  `LoadTextureFromSASAsync` `TextureType.CubeMap` eşlemi dokularını yüklemek için kullanın.

### <a name="sphere-environment-maps"></a>Küre ortamı haritaları

Bir ortam haritası olarak 2B doku kullanırken, görüntü [küresel koordinat alanında](https://en.wikipedia.org/wiki/Spherical_coordinate_system)olmalıdır.

![Küresel koordinatlarda bir gökyüzü görüntüsü](media/spheremap-example.png)

Küresel `AzureSession.Actions.LoadTextureAsync` `TextureType.Texture2D` ortam haritalarını yüklemek için kullanın.

## <a name="built-in-environment-maps"></a>Yerleşik ortam haritaları

Azure Uzaktan İşleme, her zaman kullanılabilen birkaç yerleşik ortam eşlem sağlar. Tüm yerleşik ortam haritaları küp eşlemlerdir.

|Tanımlayıcı                         | Açıklama                                              | Illüstrasyon                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Şerit ışıkları, parlak iç taban aydınlatma çeşitleri    | ![Otodükodükkanı](media/autoshop.png)
|builtin://BoilerRoom               | Parlak iç mekan ışık ayarı, birden fazla pencere Lambası      | ![Kazan Odası](media/boiler-room.png)
|builtin://ColorfulStudio           | Orta Hafif iç mekan ayarında değişen renkli ışıklar  | ![ColorfulStudio](media/colorful-studio.png)
|builtin://Hangar                   | Orta derecede parlak ortam salonu ışığı                     | ![Küçük Hangar](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Açık-koyu kontrastlı loş iç mekan ayarı              | ![IndustrialPipeAndValve](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Gündüz ortam odası ışığı, parlak pencere alanı ışığı     | ![Lebombo](media/lebombo.png)
|builtin://SataraNight              | Karanlık gece gökyüzü ve zemin birçok çevreleyen ışıklar ile   | ![SataraGece](media/satara-night.png)
|builtin://SunnyVondelpark          | Parlak güneş ışığı ve gölge kontrastı                      | ![SunnyVondelpark](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Orta yer aydınlatması ile açık gökyüzü ışığı            | ![Syferfontein](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Orta derecede değişen güneş ve gölge                         | ![TearsOfSteelBridge](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Akşam gün batımı ışık alacakaranlık yaklaşıyor                    | ![VenedikGünbatımı](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Parlak, gür-yeşil ve beyaz ışık tonları, soluk zemin | ![WhippleCreekBölgeselPark](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Parlak ortam zemin ışığı ile gündüz                 | ![Kış Nehri](media/winter-river.png)
|builtin://DefaultSky               | TearsOfSteelBridge ile aynı                               | ![Varsayılan Sky](media/tears-of-steel-bridge.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Işıklar](../../overview/features/lights.md)
* [Malzemeler](../../concepts/materials.md)
* [Doku](../../concepts/textures.md)
* [TexConv komut satırı aracı](../../resources/tools/tex-conv.md)
