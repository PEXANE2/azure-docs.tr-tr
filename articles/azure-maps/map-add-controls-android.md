---
title: Android haritasına denetim ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps Android SDK bir haritaya yakınlaştırma denetimi, sıklık denetimi, döndürme denetimi ve stil Seçici ekleme.
author: rbrundritt
ms.author: richbrun
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 8224192ed0d13af2ff6ac60aac5aa928589ff01a
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055101"
---
# <a name="add-controls-to-a-map-android-sdk"></a>Haritaya denetim ekleme (Android SDK)

Bu makalede, haritaya UI denetimleri ekleme gösterilmektedir.

## <a name="add-zoom-control"></a>Yakınlaştırma denetimi Ekle

Bir yakınlaştırma denetimi, haritada ve dışarı yakınlaştırmak için düğmeler ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `ZoomControl` ve bir haritaya ekler.

```java
map.controls.add(new ZoomControl());
```

Aşağıdaki ekran görüntüsünde bir haritada yüklü yakınlaştırma denetimi yer alır.

![Harita denetimi Haritaya eklendi](media/map-add-controls-android/android-zoom-control.jpg)

## <a name="add-pitch-control"></a>Aralıklı Denetim Ekle

Bir aralıklı denetim, aralığı ufuk ile eşlemek için sıklık için düğmeler ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `PitchControl` ve bir haritaya ekler.

```java
//Construct a pitch control and add it to the map.
map.controls.add(new PitchControl());
```

Aşağıdaki ekran görüntüsünde, bir haritaya yüklenen bir aralıklı denetim bulunur.

![Eşleme denetimine eklenen aralıklı denetim](media/map-add-controls-android/android-pitch-control.jpg)

## <a name="add-compass-control"></a>Pusula denetimi Ekle

Pusula denetimi eşlemeyi döndürmek için bir düğme ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `CompassControl` ve bir haritaya ekler.

```java
//Construct a compass control and add it to the map.
map.controls.add(new CompassControl());
```

Aşağıdaki ekran görüntüsü, haritaya yüklenen bir pusula denetimidir.

![Eşlemeye eklenen pusula denetimi](media/map-add-controls-android/android-compass-control.jpg)

## <a name="add-traffic-control"></a>Trafik denetimi Ekle

Trafik denetimi, haritadaki trafik verilerinin görünürlüğünü değiştirmeye yönelik bir düğme ekler. Aşağıdaki kod örneği, sınıfının bir örneğini oluşturur `TrafficControl` ve bir haritaya ekler.

```java
//Construct a traffic control and add it to the map.
map.controls.add(new TrafficControl());
```

Aşağıdaki ekran görüntüsünde bir haritaya yüklenen trafik denetimi yer alır.

![Eşlemeye eklenen trafik denetimi](media/map-add-controls-android/android-traffic-control.jpg)

## <a name="a-map-with-all-controls"></a>Tüm denetimlerle bir eşleme

Birden çok denetim bir diziye yerleştirilebilir ve tek seferde haritaya eklenebilir ve geliştirmeyi basitleştirmek için haritanın aynı alanında konumlandırılmış olabilir. Aşağıdaki yaklaşım, bu yaklaşımı kullanarak haritaya standart gezinti denetimleri ekler.

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
