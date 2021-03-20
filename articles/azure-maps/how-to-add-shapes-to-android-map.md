---
title: Android Maps 'e çokgen katmanı ekleme | Microsoft Azure haritaları
description: Haritalara çokgenler veya daireler eklemeyi öğrenin. Geometrik şekilleri özelleştirmek ve bunların güncelleştirilmesini ve bakımını kolaylaştırmak için Azure Haritalar Android SDK kullanma konusuna bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 68d68424e71bcf60bf504ae174b84b9c361b8637
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102097270"
---
# <a name="add-a-polygon-layer-to-the-map-android-sdk"></a>Haritaya çokgen katmanı ekleme (Android SDK)

Bu makalede, `Polygon` `MultiPolygon` bir çokgen katmanı kullanarak haritada ve özellik geometrileri arasındaki alanların nasıl işleneceğini gösterir.

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="use-a-polygon-layer"></a>Çokgen katmanı kullanma

Bir çokgen katmanı bir veri kaynağına bağlıyken ve haritada yüklendiğinde, alanı `Polygon` ve `MultiPolygon` özellikleri işler. Bir çokgen oluşturmak için bir veri kaynağına ekleyin ve sınıfı kullanarak bir Çokgen katman ile oluşturun `PolygonLayer` .

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source, 
    fillColor("red"),
    fillOpacity(0.7f)
), "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a rectangular polygon.
source.add(
    Polygon.fromLngLats(
        Arrays.asList(
            Arrays.asList(
                Point.fromLngLat(-73.98235, 40.76799),
                Point.fromLngLat(-73.95785, 40.80044),
                Point.fromLngLat(-73.94928, 40.79680),
                Point.fromLngLat(-73.97317, 40.76437),
                Point.fromLngLat(-73.98235, 40.76799)
            )
        )
    )
)

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(
    PolygonLayer(
        source,
        fillColor("red"),
        fillOpacity(0.7f)
    ), "labels"
)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir çokgen katmanı kullanarak bir çokgen alanını işleyen Yukarıdaki kod gösterilmektedir.

![Fill alanı işlenmiş olan Çokgen](media/how-to-add-shapes-to-android-map/android-polygon-layer.png)

## <a name="use-a-polygon-and-line-layer-together"></a>Bir çokgen ve çizgi katmanını birlikte kullanma

Çokgenler ana hattını işlemek için çizgi katmanı kullanılır. Aşağıdaki kod örneği, önceki örnekte olduğu gibi bir çokgen oluşturur, ancak şimdi bir çizgi katmanı ekler. Bu çizgi katmanı, veri kaynağına bağlı ikinci bir katmandır.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a rectangular polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.98235, 40.76799),
            Point.fromLngLat(-73.95785, 40.80044),
            Point.fromLngLat(-73.94928, 40.79680),
            Point.fromLngLat(-73.97317, 40.76437),
            Point.fromLngLat(-73.98235, 40.76799)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
    fillColor("rgba(0, 200, 200, 0.5)")
), "labels");

//Create and add a line layer to render the outline of the polygon.
map.layers.add(new LineLayer(source,
    strokeColor("red"),
    strokeWidth(2f)
));
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a rectangular polygon.
source.add(
    Polygon.fromLngLats(
        Arrays.asList(
            Arrays.asList(
                Point.fromLngLat(-73.98235, 40.76799),
                Point.fromLngLat(-73.95785, 40.80044),
                Point.fromLngLat(-73.94928, 40.79680),
                Point.fromLngLat(-73.97317, 40.76437),
                Point.fromLngLat(-73.98235, 40.76799)
            )
        )
    )
)

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(
    PolygonLayer(
        source,
        fillColor("rgba(0, 200, 200, 0.5)")
    ), "labels"
)

//Create and add a line layer to render the outline of the polygon.
map.layers.add(
    LineLayer(
        source,
        strokeColor("red"),
        strokeWidth(2f)
    )
)
```

::: zone-end

Aşağıdaki ekran görüntüsünde, bir çizgi katmanı kullanılarak oluşturulan ana hattı ile bir çokgen işlenen Yukarıdaki kod gösterilmektedir.

![Kendi Fill alanı ve ana hattı işlenmiş Çokgen](media/how-to-add-shapes-to-android-map/android-polygon-and-line-layer.png)

> [!TIP]
> Bir çizgi katmanıyla bir çokgen ana hattını oluştururken, her bir nokta dizisinin aynı başlangıç ve bitiş noktasına sahip olması gibi çokgenler içindeki tüm halkaları kapatmayı unutmayın. Bu yapılmazsa, çizgi katmanı, çokgenin son noktasını ilk noktaya bağlayamayabilir.

## <a name="fill-a-polygon-with-a-pattern"></a>Çokgeni bir desenli doldur

Çokgeni bir renkle doldurmanın yanı sıra, çokgeni doldurmak için de bir resim kalıbı kullanabilirsiniz. Haritalar görüntüsü Sprite kaynaklarına bir resim kalıbı yükleyin ve ardından bu görüntüye `fillPattern` Çokgen katmanının seçeneğiyle başvurun.

::: zone pivot="programming-language-java-android"

```java
//Load an image pattern into the map image sprite.
map.images.add("fill-checker-red", R.drawable.fill_checker_red);

//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-50, -20),
            Point.fromLngLat(0, 40),
            Point.fromLngLat(50, -20),
            Point.fromLngLat(-50, -20)
        )
    )
));

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(new PolygonLayer(source,
        fillPattern("fill-checker-red"),
        fillOpacity(0.5f)
), "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Load an image pattern into the map image sprite.
map.images.add("fill-checker-red", R.drawable.fill_checker_red)

//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a polygon.
source.add(
    Polygon.fromLngLats(
        Arrays.asList(
            Arrays.asList(
                Point.fromLngLat(-50, -20),
                Point.fromLngLat(0, 40),
                Point.fromLngLat(50, -20),
                Point.fromLngLat(-50, -20)
            )
        )
    )
)

//Create and add a polygon layer to render the polygon on the map, below the label layer.
map.layers.add(
    PolygonLayer(
        source,
        fillPattern("fill-checker-red"),
        fillOpacity(0.5f)
    ), "labels"
)
```

::: zone-end

Bu örnekte, aşağıdaki görüntü uygulamanın çizilebilir klasörüne yüklenmiştir.

| ![Mor ok simgesi resmi](media/how-to-add-shapes-to-android-map/fill-checker-red.png)|
|:-----------------------------------------------------------------------:|
| fill_checker_red.png                                                    |

Aşağıda, haritada bir Fill desenli bir çokgen işleyen Yukarıdaki kodun ekran görüntüsü verilmiştir.

![Haritada işlenen bir Fill desenli Çokgen](media/how-to-add-shapes-to-android-map/android-polygon-pattern.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen kalıp katmanı ekleme](map-extruded-polygon-android.md)
