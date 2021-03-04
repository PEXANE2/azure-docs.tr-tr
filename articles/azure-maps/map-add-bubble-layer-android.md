---
title: Android Maps 'e kabarcık katmanı ekleme | Microsoft Azure haritaları
description: Haritalar üzerinde noktaları sabit boyutlarla daire olarak nasıl işleyebileceğinizi öğrenin. Bu amaçla kabarcık katmanları eklemek ve özelleştirmek için bkz. Azure Maps Android SDK nasıl kullanılır.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: f3c175f30c5c0e6206f4fee274e0f3f000e55a74
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102100177"
---
# <a name="add-a-bubble-layer-to-a-map-android-sdk"></a>Haritaya kabarcık katmanı ekleme (Android SDK)

Bu makalede, bir veri kaynağındaki nokta verilerinin haritada kabarcık katmanı olarak nasıl oluşturulduğu gösterilir. Kabarcık katmanları, bir sabit piksel yarıçapı ile haritada daire olarak işleme noktaları.

> [!TIP]
> Kabarcık katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özellikleri olacak şekilde sınırlamak için `filter` katmanın seçeneğini olarak ayarlayın `eq(geometryType(), "Point")` . MultiPoint özelliklerini de dahil etmek istiyorsanız `filter` katmanın seçeneğini olarak ayarlayın `any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint"))` .

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="add-a-bubble-layer"></a>Baloncuk katmanı ekleme

Aşağıdaki kod, bir dizi noktayı bir veri kaynağına yükler. Ardından, veri noktalarını bir kabarcık katmanına bağlar. Kabarcık katmanı, her kabarcığun yarıçapını beş piksel ve bir de beyaz dolgulu bir renk oluşturur. Ve, mavi renkli bir kontur rengi ve altı piksellik bir kontur kalınlığı.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create point locations.
Point[] points = new Point[] {
    Point.fromLngLat(-73.985708, 40.75773),
    Point.fromLngLat(-73.985600, 40.76542),
    Point.fromLngLat(-73.985550, 40.77900),
    Point.fromLngLat(-73.975550, 40.74859),
    Point.fromLngLat(-73.968900, 40.78859)
};

//Add multiple points to the data source.
source.add(points);

//Create a bubble layer to render the filled in area of the circle, and add it to the map.
BubbleLayer layer = new BubbleLayer(source, 
    bubbleRadius(5f),
    bubbleColor("white"),
    bubbleStrokeColor("#4288f7"),
    bubbleStrokeWidth(6f)
);

map.layers.add(layer);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create point locations.
val points: Array<Point> = arrayOf<Point>(
    Point.fromLngLat(-73.985708, 40.75773),
    Point.fromLngLat(-73.985600, 40.76542),
    Point.fromLngLat(-73.985550, 40.77900),
    Point.fromLngLat(-73.975550, 40.74859),
    Point.fromLngLat(-73.968900, 40.78859)
)

//Add multiple points to the data source.
source.add(points)

//Create a bubble layer to render the filled in area of the circle, and add it to the map.
val layer = BubbleLayer(
    source,
    bubbleRadius(5f),
    bubbleColor("white"),
    bubbleStrokeColor("#4288f7"),
    bubbleStrokeWidth(6f)
)

map.layers.add(layer)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, üstteki kodun bir kabarcık katmanında noktaları işlediğini gösterilmektedir.

![Kabarcık katmanı kullanılarak işlenen noktalarıyla eşleme](media/map-add-bubble-layer-android/android-bubble-layer.png)

## <a name="show-labels-with-a-bubble-layer"></a>Kabarcık katmanı içeren etiketleri göster

Bu kod, haritada bir noktayı işlemek için balon katmanını nasıl kullanacağınızı gösterir. Ve bir etiketi işlemek için bir sembol katmanını kullanma. Sembol katmanının simgesini gizlemek için `iconImage` seçeneğini olarak ayarlayın `"none"` .

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Add a data point to the map.
source.add(Point.fromLngLat(-122.336641,47.627631));

//Add a bubble layer.
map.layers.add(new BubbleLayer(source,
    bubbleRadius(5f),
    bubbleColor("white"),
    bubbleStrokeColor("#4288f7"),
    bubbleStrokeWidth(6f)
));

//Add a symbol layer to display text, hide the icon image.
map.layers.add(new SymbolLayer(source,
    //Hide the icon image.
    iconImage("none"),
    textField("Museum of History & Industry (MOHAI)"),
    textColor("#005995"),
    textOffset(new Float[]{0f, -2.2f})
));
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Add a data point to the map.
source.add(Point.fromLngLat(-122.336641, 47.627631))

//Add a bubble layer.
map.layers.add(
    BubbleLayer(
        source,
        bubbleRadius(5f),
        bubbleColor("white"),
        bubbleStrokeColor("#4288f7"),
        bubbleStrokeWidth(6f)
    )
)

//Add a symbol layer to display text, hide the icon image.
map.layers.add(
    SymbolLayer(
        source,  //Hide the icon image.
        iconImage("none"),
        textField("Museum of History & Industry (MOHAI)"),
        textColor("#005995"),
        textOffset(arrayOf(0f, -2.2f))
    )
)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir kabarcık katmanındaki bir noktayı ve bir sembol katmanını içeren noktanın metin etiketini izleyen kod görüntülenir.

![Kabarcık katmanı ve sembol katmanı ile metin etiketi kullanılarak işlenen nokta ile eşleme](media/map-add-bubble-layer-android/android-bubble-symbol-layer.png)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Özellik bilgilerini görüntüleme](display-feature-information-android.md)
