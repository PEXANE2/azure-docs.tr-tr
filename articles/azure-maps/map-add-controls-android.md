---
title: Android haritasına denetim ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps Android SDK bir haritaya yakınlaştırma denetimi, sıklık denetimi, döndürme denetimi ve stil Seçici ekleme.
author: rbrundritt
ms.author: richbrun
ms.date: 02/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 90d037fc02bdc1c4d6fe682386790561c890c1e6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102100228"
---
# <a name="add-controls-to-a-map-android-sdk"></a>Haritaya denetim ekleme (Android SDK)

Bu makalede, haritaya UI denetimleri ekleme gösterilmektedir.

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi Ekle

Bir yakınlaştırma denetimi, haritada ve dışarı yakınlaştırmak için düğmeler ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `ZoomControl` ve bir haritaya ekler.

::: zone pivot="programming-language-java-android"

```java
//Construct a zoom control and add it to the map.
map.controls.add(new ZoomControl());
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Construct a zoom control and add it to the map.
map.controls.add(ZoomControl())
```

::: zone-end

Aşağıdaki ekran görüntüsünde bir haritada yüklü yakınlaştırma denetimi yer alır.

![Harita denetimi Haritaya eklendi](media/map-add-controls-android/android-zoom-control.jpg)

## <a name="add-pitch-control"></a>Aralıklı Denetim Ekle

Bir aralıklı denetim, aralığı ufuk ile eşlemek için sıklık için düğmeler ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `PitchControl` ve bir haritaya ekler.

::: zone pivot="programming-language-java-android"

```java
//Construct a pitch control and add it to the map.
map.controls.add(new PitchControl());
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Construct a pitch control and add it to the map.
map.controls.add(PitchControl())
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir haritaya yüklenen bir aralıklı denetim bulunur.

![Eşleme denetimine eklenen aralıklı denetim](media/map-add-controls-android/android-pitch-control.jpg)

## <a name="add-compass-control"></a>Pusula denetimi Ekle

Pusula denetimi eşlemeyi döndürmek için bir düğme ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `CompassControl` ve bir haritaya ekler.

::: zone pivot="programming-language-java-android"

```java
//Construct a compass control and add it to the map.
map.controls.add(new CompassControl());
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Construct a compass control and add it to the map.
map.controls.add(CompassControl())
```

::: zone-end

Aşağıdaki ekran görüntüsü, haritaya yüklenen bir pusula denetimidir.

![Eşlemeye eklenen pusula denetimi](media/map-add-controls-android/android-compass-control.jpg)

## <a name="add-traffic-control"></a>Trafik denetimi Ekle

Trafik denetimi, haritadaki trafik verilerinin görünürlüğünü değiştirmeye yönelik bir düğme ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `TrafficControl` ve bir haritaya ekler.

::: zone pivot="programming-language-java-android"

```java
//Construct a traffic control and add it to the map.
map.controls.add(new TrafficControl());
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Construct a traffic control and add it to the map.
map.controls.add(TrafficControl())
```

::: zone-end

Aşağıdaki ekran görüntüsünde bir haritaya yüklenen trafik denetimi yer alır.

![Eşlemeye eklenen trafik denetimi](media/map-add-controls-android/android-traffic-control.jpg)

## <a name="a-map-with-all-controls"></a>Tüm denetimlerle bir eşleme

Birden çok denetim bir diziye yerleştirilebilir ve tek seferde haritaya eklenebilir ve geliştirmeyi basitleştirmek için haritanın aynı alanında konumlandırılmış olabilir. Aşağıdaki yaklaşım, bu yaklaşımı kullanarak haritaya standart gezinti denetimleri ekler.

::: zone pivot="programming-language-java-android"

```java
map.controls.add(
    new Control[]{
        new ZoomControl(),
        new CompassControl(),
        new PitchControl(),
        new TrafficControl()
    }
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
map.controls.add(
    arrayOf<Control>(
        ZoomControl(),
        CompassControl(),
        PitchControl(),
        TrafficControl()
    )
)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir haritada yüklü olan tüm denetimler gösterilmektedir. Haritaya eklendikleri sıranın görüneceği sıra olduğunu unutmayın.

![Haritaya eklenen tüm denetimler](media/map-add-controls-android/android-all-controls.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
