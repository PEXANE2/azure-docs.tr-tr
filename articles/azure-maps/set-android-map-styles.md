---
title: Android haritalar 'da harita stili ayarlama | Microsoft Azure haritaları
description: Bir haritanın stilini ayarlamanın iki yolunu öğrenin. Stili ayarlamak için Düzen dosyasında ya da etkinlik sınıfında Azure Maps Android SDK kullanma konusuna bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 1cce355c8ffbcd4704bd32b0e4d1739c77c2b623
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97678478"
---
# <a name="set-map-style-android-sdk"></a>Harita stilini ayarla (Android SDK)

Bu makalede, Azure Haritalar Android SDK kullanarak harita stillerini ayarlamak için kullanabileceğiniz iki yol gösterilmektedir. Azure haritalar 'ın aralarından seçim yapabileceğiniz altı farklı harita stili vardır. Desteklenen harita stilleri hakkında daha fazla bilgi için bkz. [Azure Maps 'de desteklenen harita stilleri](supported-map-styles.md).

## <a name="prerequisites"></a>Ön koşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun.

## <a name="set-map-style-in-the-layout"></a>Düzende harita stili ayarla

Harita denetimi eklerken, etkinlik sınıfınızın düzen dosyasında bir harita stili ayarlayabilirsiniz. Aşağıdaki kod merkez konumunu, yakınlaştırma düzeyini ve harita stilini ayarlar.

```XML
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/mapcontrol"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_centerLat="47.602806"
    app:mapcontrol_centerLng="-122.329330"
    app:mapcontrol_zoom="12"
    app:mapcontrol_style="grayscale_dark"
    />
```

Aşağıdaki ekran görüntüsünde, gri tonlamalı koyu stille bir yol haritası görüntüleyen Yukarıdaki kod gösterilmektedir.

![Gri tonlamalı koyu yol haritası stiliyle eşle](media/set-android-map-styles/android-grayscale-dark.png)

## <a name="set-map-style-in-code"></a>Koddaki harita stilini ayarla

Harita stili, eşleme yöntemi kullanılarak kodda program aracılığıyla ayarlanabilir `setStyle` . Aşağıdaki kod, haritalar `setCamera` yöntemini ve harita stilini kullanarak merkez konumunu ve yakınlaştırma düzeyini ayarlar `SATELLITE_ROAD_LABELS` .

```java
mapControl.onReady(map -> {

    //Set the camera of the map.
    map.setCamera(center(Point.fromLngLat(-122.33, 47.64)), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE_ROAD_LABELS));
});
```

Aşağıdaki ekran görüntüsünde, uydu yol etiketleri stiliyle bir eşlemeyi görüntüleyen Yukarıdaki kod gösterilmektedir.

![Uydu Road Etiketler stiliyle eşle](media/set-android-map-styles/android-satellite-road-labels.png)

## <a name="setting-the-map-camera"></a>Harita kamerayı ayarlama

Harita Kamerası haritanın hangi kısmının haritada görüntülendiğini denetler. Kamera, programlama yoluyla kodda bir düzende olabilir. Kodda ayarlanırken haritanın konumunu ayarlamak için iki ana yöntem vardır; Merkezi ve yakınlaştırmayı kullanarak ya da bir sınırlayıcı kutuya geçirerek. Aşağıdaki kod, ve kullanırken tüm isteğe bağlı kamera seçeneklerinin nasıl ayarlanacağını gösterir `center` `zoom` .

```java
//Set the camera of the map using center and zoom.
map.setCamera(
    center(Point.fromLngLat(-122.33, 47.64)), 

    //The zoom level. Typically a value between 0 and 22.
    zoom(14),

    //The amount of tilt in degrees the map where 0 is looking straight down.
    pitch(45),

    //Direction the top of the map is pointing in degrees. 0 = North, 90 = East, 180 = South, 270 = West
    bearing(90),

    //The minimum zoom level the map will zoom-out to when animating from one location to another on the map.
    minZoom(10),
    
    //The maximium zoom level the map will zoom-in to when animating from one location to another on the map.
    maxZoom(14)
);
```

Genellikle Haritayı bir veri kümesinin üzerine odaklamanız tercih edilir. Bir sınırlayıcı kutu, yöntemi kullanılarak özelliklerden hesaplanabilir `MapMath.fromData` ve `bounds` harita Kamerası seçeneğine geçirilebilir. Bir sınırlayıcı kutusu temelinde bir harita görünümü ayarlarken, genellikle `padding` balon veya sembol olarak işlenen noktaların piksel boyutu için bir değer belirtmek yararlı olur. Aşağıdaki kod, kameranın konumunu ayarlamak için bir sınırlama kutusu kullanırken tüm isteğe bağlı kamera seçeneklerinin nasıl ayarlanacağını gösterir.

```java
//Set the camera of the map using a bounding box.
map.setCamera(
    //The area to focus the map on.
    bounds(BoundingBox.fromLngLats(
        //West
        -122.4594,

        //South
        47.4333,
        
        //East
        -122.21866,
        
        //North
        47.75758
    )),

    //Amount of pixel buffer around the bounding box to provide extra space around the bounding box.
    padding(20),

    //The maximium zoom level the map will zoom-in to when animating from one location to another on the map.
    maxZoom(14)
);
```

Bir sınırlayıcı kutunun en boy oranının haritanın en boy oranıyla aynı olamayacağını unutmayın. bu nedenle, haritada genellikle tam sınırlayıcı kutu alanı gösterilir, ancak genellikle en fazla dikey veya yatay olur.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)
