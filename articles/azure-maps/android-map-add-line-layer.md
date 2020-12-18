---
title: Android Maps 'e çizgi katmanı ekleme | Microsoft Azure haritaları
description: Haritalara satır eklemeyi öğrenin. Haritalara çizgi katmanları eklemek ve semboller ve renk degradeleriyle satırları özelleştirmek için Azure Haritalar Android SDK kullanan örneklere bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 12/08/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 3e68be79a4405af103512a9009187857a0d9af39
ms.sourcegitcommit: 66b0caafd915544f1c658c131eaf4695daba74c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97681747"
---
# <a name="add-a-line-layer-to-the-map-android-sdk"></a>Haritaya çizgi katmanı ekleme (Android SDK)

Bir çizgi katmanı `LineString` `MultiLineString` , haritada yollar veya rotalar olarak işlemek ve özellikleri oluşturmak için kullanılabilir. Bir çizgi katmanı, ve özelliklerinin anahatlarını işlemek için de kullanılabilir `Polygon` `MultiPolygon` . Veri kaynağı, işlenecek verileri sağlamak için bir çizgi katmanına bağlanır.

> [!TIP]
> Çizgi katmanları, varsayılan olarak çokgenler koordinatlarını ve bir veri kaynağındaki satırları de işler. Katmanı yalnızca LineString geometrisi özellikleri olacak şekilde sınırlamak için `filter` katmanın seçeneğini olarak ayarlayın `eq(geometryType(), "LineString")` . Çok LineString özellikleri de eklemek istiyorsanız `filter` katmanın seçeneğini olarak ayarlayın `any(eq(geometryType(), "LineString"), eq(geometryType(), "MultiLineString"))` .

## <a name="prerequisites"></a>Önkoşullar

[Hızlı başlangıç: Android uygulama belgesi oluşturma](quick-android-map.md) ' daki adımları tamamladığınızdan emin olun. Bu makaledeki kod blokları haritalar `onReady` olay işleyicisine eklenebilir.

## <a name="add-a-line-layer"></a>Çizgi katmanı ekleme

Aşağıdaki kod, bir satırın nasıl oluşturulacağını gösterir. Satırı bir veri kaynağına ekleyin, ardından sınıfı kullanarak bir çizgi katmanı ile işlenir `LineLayer` .

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a list of points.
List<Point> points = Arrays.asList(
    Point.fromLngLat(-73.972340, 40.743270),
    Point.fromLngLat(-74.004420, 40.756800));

//Create a LineString geometry and add it to the data source.
source.add(LineString.fromLngLats(points));

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor("blue"),
    strokeWidth(5f)
);

map.layers.add(layer);
```

Aşağıdaki ekran görüntüsünde, bir çizgi katmanında bir satırı işleyen Yukarıdaki kod gösterilmektedir.

![Çizgi katmanı kullanılarak işlenen bir satırla eşleme](media/android-map-add-line-layer/android-line-layer.png)

## <a name="data-drive-line-style"></a>Veri sürücüsü çizgi stili

Aşağıdaki kod iki satırlık Özellikler oluşturur ve her satıra bir özellik olarak bir hız sınırı değeri ekler. Çizgi katmanı, hız sınırı değerine göre satırları renkli bir veri sürücüsü stili ifade kullanır. Veri yolunda satır verilerinde yer paylaşımı olduğundan, aşağıdaki kod etiket katmanının altına çizgi katmanını ekler, böylece yol etiketleri de açıkça okunabilir.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a line feature.
Feature feature = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.131821, 47.704033),
        Point.fromLngLat(-122.099919, 47.703678))));

//Add a property to the feature.
feature.addNumberProperty("speedLimitMph", 35);

//Add the feature to the data source.
source.add(feature);

//Create a second line feature.
Feature feature2 = Feature.fromGeometry(
    LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.126662, 47.708265),
        Point.fromLngLat(-122.126877, 47.703980))));

//Add a property to the second feature.
feature2.addNumberProperty("speedLimitMph", 15);

//Add the second feature to the data source.
source.add(feature2);

//Create a line layer and add it to the map.
LineLayer layer = new LineLayer(source,
    strokeColor(
        interpolate(
            linear(),
            get("speedLimitMph"),
            stop(0, color(Color.GREEN)),
            stop(30, color(Color.YELLOW)),
            stop(60, color(Color.RED))
        )
    ),
    strokeWidth(5f)
);

map.layers.add(layer, "labels");
```

Aşağıdaki ekran görüntüsünde, çizgi özelliklerinde bir özelliği temel alan bir veri temelli stil ifadesinden alınan renklerle birlikte bir çizgi katmanındaki iki satırı işleyen Yukarıdaki kod gösterilmektedir.

![Bir çizgi katmanında işlenen veri sürücüsü biçimlendirilmiş satırlar ile eşleme](media/android-map-add-line-layer/android-line-layer-data-drive-style.png)

## <a name="add-symbols-along-a-line"></a>Çizgi üzerinde semboller ekleme

Bu örnek, haritada bir çizgi üzerinde ok simgelerinin nasıl ekleneceğini gösterir. Bir sembol katmanını kullanırken `symbolPlacement` seçeneğini olarak ayarlayın `SymbolPlacement.LINE` . Bu seçenek, simgeleri satır üzerinde işler ve simgeleri döndürür (0 derece = sağ).

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Load a image of an arrow into the map image sprite and call it "arrow-icon".
map.images.add("arrow-icon", R.drawable.purple-arrow-right);

//Create and add a line to the data source.
source.add(LineString.fromLngLats(Arrays.asList(
        Point.fromLngLat(-122.18822, 47.63208),
        Point.fromLngLat(-122.18204, 47.63196),
        Point.fromLngLat(-122.17243, 47.62976),
        Point.fromLngLat(-122.16419, 47.63023),
        Point.fromLngLat(-122.15852, 47.62942),
        Point.fromLngLat(-122.15183, 47.62988),
        Point.fromLngLat(-122.14256, 47.63451),
        Point.fromLngLat(-122.13483, 47.64041),
        Point.fromLngLat(-122.13466, 47.64422),
        Point.fromLngLat(-122.13844, 47.65440),
        Point.fromLngLat(-122.13277, 47.66515),
        Point.fromLngLat(-122.12779, 47.66712),
        Point.fromLngLat(-122.11595, 47.66712),
        Point.fromLngLat(-122.11063, 47.66735),
        Point.fromLngLat(-122.10668, 47.67035),
        Point.fromLngLat(-122.10565, 47.67498))));

//Create a line layer and add it to the map.
map.layers.add(new LineLayer(source,
    strokeColor("DarkOrchid"),
    strokeWidth(5f)
));

//Create a symbol layer and add it to the map.
map.layers.add(new SymbolLayer(source,
    //Space symbols out along line.
    symbolPlacement(SymbolPlacement.LINE),

    //Spread the symbols out 100 pixels apart.
    symbolSpacing(100f),
    
    //Use the arrow icon as the symbol.
    iconImage("arrow-icon"),

    //Allow icons to overlap so that they aren't hidden if they collide with other map elements.
    iconAllowOverlap(true),

    //Center the symbol icon.
    iconAnchor(AnchorType.CENTER),

    //Scale the icon size.
    iconSize(0.8f)
));
```

Bu örnekte, aşağıdaki görüntü uygulamanın çizilebilir klasörüne yüklenmiştir.

| ![Mor ok simgesi resmi](media/android-map-add-line-layer/purple-arrow-right.png)|
|:-----------------------------------------------------------------------:|
|                                                  |

Aşağıdaki ekran görüntüsünde, üzerinde ok simgeleri görüntülenirken bir satır işleyen Yukarıdaki kod gösterilmektedir.

![Bir çizgi katmanında işlenen oklarla veri sürücüsü stillendirilmiş çizgiler ile eşleme](media/android-map-add-line-layer/android-symbols-along-line-path.png)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
