---
title: Gökyüzü yansımaları
description: Sky yansımaları için ortam eşlemelerini ayarlamayı açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: be3dc2b113cb21c2dfb54a29e7f426e0d925c6d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83759124"
---
# <a name="sky-reflections"></a>Gökyüzü yansımaları

Azure uzaktan Işlemede, nesneleri gerçekçi bir şekilde hafif bir şekilde açmaya yönelik bir gök dokusu kullanılır. Genişletilmiş gerçeklik uygulamaları için bu doku, nesnelerin ikna edici görünmesini sağlamak için gerçek dünyaya benzer olmalıdır. Bu makalede, gök dokusunun nasıl değiştirileceği açıklanır.

> [!NOTE]
> Gök dokusunu de *ortam Haritası*olarak adlandırılır. Bu terimler birbirinin yerine kullanılır.

## <a name="object-lighting"></a>Nesne aydınlatma

Azure uzaktan Işleme, gerçekçi aydınlatma hesaplamaları için *fiziksel tabanlı işleme* (PBR) kullanır. Sahneye [hafif kaynaklar](lights.md) ekleyebilseniz de, iyi bir gök dokusunun kullanılması en büyük etkiye sahiptir.

Aşağıdaki görüntüler, farklı yüzeylerin yalnızca gök dokuyla aydınlatma sonuçlarını gösterir:

| Kablık  | 0                                        | 0,25                                          | 0,5                                          | 0,75                                          | 1                                          |
|:----------:|:----------------------------------------:|:---------------------------------------------:|:--------------------------------------------:|:---------------------------------------------:|:------------------------------------------:|
| Metal olmayan  | ![Dielectric0](media/dielectric-0.png)   | ![Yeşilpointpark](media/dielectric-0.25.png)  | ![Yeşilpointpark](media/dielectric-0.5.png)  | ![Yeşilpointpark](media/dielectric-0.75.png)  | ![Yeşilpointpark](media/dielectric-1.png)  |
| Metal      | ![Yeşilpointpark](media/metallic-0.png)  | ![Yeşilpointpark](media/metallic-0.25.png)    | ![Yeşilpointpark](media/metallic-0.5.png)    | ![Yeşilpointpark](media/metallic-0.75.png)    | ![Yeşilpointpark](media/metallic-1.png)    |

Aydınlatma modeli hakkında daha fazla bilgi için bkz. [malzemeler](../../concepts/materials.md) bölümü.

> [!IMPORTANT]
> Azure uzaktan Işleme, yalnızca aydınlatma modelleri için gök dokusunu kullanır. Genişletmüş olan gerçeklik uygulamalarının gerçek bir arka plana sahip olduğu için bir arka plan olarak Çarpıt olarak işlenmiyor.

## <a name="changing-the-sky-texture"></a>Gök dokusunu değiştirme

Ortam eşlemesini değiştirmek için, tek yapmanız gereken [bir doku yükler](../../concepts/textures.md) ve oturum şu şekilde değişir `SkyReflectionSettings` :

```cs
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

```cpp
void ChangeEnvironmentMap(ApiHandle<AzureSession> session)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::CubeMap;
    params.TextureUrl = "builtin://VeniceSunset";
    ApiHandle<LoadTextureAsync> skyTextureLoad = *session->Actions()->LoadTextureFromSASAsync(params);

    skyTextureLoad->Completed([&](ApiHandle<LoadTextureAsync> res)
    {
        if (res->IsRanToCompletion())
        {
            ApiHandle<SkyReflectionSettings> settings = *session->Actions()->SkyReflectionSettings();
            settings->SkyReflectionTexture(*res->Result());
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}

```

`LoadTextureFromSASAsync`Yerleşik bir doku yüklendiği için değişkenin yukarıda kullanıldığını unutmayın. [Bağlı BLOB depolama](../../how-tos/create-an-account.md#link-storage-accounts)alanından yükleme durumunda, `LoadTextureAsync` türevini kullanın.

## <a name="sky-texture-types"></a>Sky doku türleri

Yalnızca *[cubemaps](https://en.wikipedia.org/wiki/Cube_mapping)* ve *2B dokuları* ortam haritaları olarak kullanabilirsiniz.

Tüm dokuların [desteklenen bir doku biçiminde](../../concepts/textures.md#supported-texture-formats)olması gerekir. Gök dokuları için msunucudan mfor haritaları sağlamanız gerekmez.

### <a name="cube-environment-maps"></a>Küp ortam haritaları

Başvuru için sarmalanmamış bir cubemap aşağıda verilmiştir:

![Sarmalanmamış bir küp harita](media/Cubemap-example.png)

' İ kullanarak `AzureSession.Actions.LoadTextureAsync` /  `LoadTextureFromSASAsync` `TextureType.CubeMap` küp harita dokuları yükleyin.

### <a name="sphere-environment-maps"></a>Sphere ortam haritaları

Bir 2B dokusunu ortam haritası olarak kullanırken, görüntünün [küresel koordinat](https://en.wikipedia.org/wiki/Spherical_coordinate_system)alanında olması gerekir.

![Küresel koordinatlardaki gök resmi](media/spheremap-example.png)

`AzureSession.Actions.LoadTextureAsync` `TextureType.Texture2D` Küresel ortam haritalarını yüklemek için ile kullanın.

## <a name="built-in-environment-maps"></a>Yerleşik ortam haritaları

Azure uzaktan Işleme, her zaman kullanılabilir olan birkaç yerleşik ortam haritası sağlar. Tüm yerleşik ortam haritaları cubemaps.

|Tanımlayıcı                         | Açıklama                                              | Göstermektedir                                                      |
|-----------------------------------|:---------------------------------------------------------|:-----------------------------------------------------------------:|
|builtin://Autoshop                 | Çeşitli Stripe ışıkları, parlak kapılı taban aydınlatma    | ![Oto Mağazası](media/autoshop.png)
|builtin://BoilerRoom               | Parlak kapılı ışık ayarı, birden çok pencere ışıkları      | ![BoilerRoom](media/boiler-room.png)
|builtin://ColorfulStudio           | Orta hafif inkapılı varyingly renkli ışıklar  | ![ColorfulStudio](media/colorful-studio.png)
|builtin://Hangar                   | Orta parlak çevresel salonu ışığı                     | ![SmallHangar](media/hangar.png)
|builtin://IndustrialPipeAndValve   | Açık Koyu karşıtlıklı karartma ınkapı ayarı              | ![IndustrialPipeAndValve](media/industrial-pipe-and-valve.png)
|builtin://Lebombo                  | Gündüz ortam odası ışığı, parlak pencere alanı ışığı     | ![Lebombo](media/lebombo.png)
|builtin://SataraNight              | Çok sayıda ışığı içeren koyu gece gök ve zemin   | ![SataraNight](media/satara-night.png)
|builtin://SunnyVondelpark          | Parlak güneş ve gölge karşıtlığı                      | ![SunnyVondelpark](media/sunny-vondelpark.png)
|builtin://Syferfontein             | Orta zemin aydınlatma ile gök ışığı temizle            | ![Syferfontein](media/syferfontein.png)
|builtin://TearsOfSteelBridge       | Orta derecede değişen güneş ve gölge                         | ![Tearsofsteelköprüsü](media/tears-of-steel-bridge.png)
|builtin://VeniceSunset             | Akşam günlü ışığı, alacak                    | ![VeniceSunset](media/venice-sunset.png)
|builtin://WhippleCreekRegionalPark | Parlak, Lush-yeşil ve beyaz açık ton, soluk zemin | ![WhippleCreekRegionalPark](media/whipple-creek-regional-park.png)
|builtin://WinterRiver              | Parlak çevresel zemin ışığı ile gündüz                 | ![WinterRiver](media/winter-river.png)
|builtin://DefaultSky               | TearsOfSteelBridge ile aynı                               | ![DefaultSky](media/tears-of-steel-bridge.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Işıklar](../../overview/features/lights.md)
* [Malzemeler](../../concepts/materials.md)
* [Dokular](../../concepts/textures.md)
* [TexConv komut satırı aracı](../../resources/tools/tex-conv.md)
