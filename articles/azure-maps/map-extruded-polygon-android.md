---
title: Android haritasına Çokgen katman ekleme | Microsoft Azure haritaları
description: Microsoft Azure Maps Android SDK Çokgen kalıp ekleme.
author: rbrundritt
ms.author: richbrun
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: b62c2540dc8cf2c7d4f67d465b2d464cbc0c3091
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055053"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map-android-sdk"></a>Haritaya Çokgen kalıp ekleme (Android SDK)

Bu makalede, Çokgen katman alanlarının `Polygon` ve `MultiPolygon` özellik geometrileri tarafından yükseltilmiş şekiller olarak işlenip nasıl kullanılacağı gösterilir.

## <a name="use-a-polygon-extrusion-layer"></a>Çokgen katman kullan

Çokgen kalıp katmanını bir veri kaynağına bağlayın. Daha sonra, bu dosyayı haritaya yükledi. Çokgen tabakası katmanı, bir `Polygon` ve `MultiPolygon` özelliklerinin ve özellikleri yükseltilmiş şekiller olarak işler. `height` `base` Çokgen katman katmanının ve özellikleri, yükseltme şeklinin zemin ve yüksekliğinden **Ölçü** cinsinden temel mesafeyi tanımlar. Aşağıdaki kod, bir çokgen oluşturmayı, bir veri kaynağına eklemeyi ve Çokgen katman sınıfını kullanarak nasıl işleneceğini gösterir.

> [!Note]
> `base`Çokgen katman katmanında tanımlanan değer, değerinden küçük veya buna eşit olmalıdır `height` .

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.95838379859924, 40.80027995478159),
            Point.fromLngLat(-73.98154735565186, 40.76845986171129),
            Point.fromLngLat(-73.98124694824219, 40.767761062136955),
            Point.fromLngLat(-73.97361874580382, 40.76461637311633),
            Point.fromLngLat(-73.97306084632874, 40.76512830937617),
            Point.fromLngLat(-73.97259950637817, 40.76490890860481),
            Point.fromLngLat(-73.9494466781616, 40.79658450499243),
            Point.fromLngLat(-73.94966125488281, 40.79708807289436),
            Point.fromLngLat(-73.95781517028809, 40.80052360358227),
            Point.fromLngLat(-73.95838379859924, 40.80027995478159)
        )
    )
));

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillColor("#fc0303"),
    fillOpacity(0.7f),
    height(500f)
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

Aşağıdaki ekran görüntüsünde, çokgen bir katman kullanarak dikey olarak uzatılmış bir çokgen olan yukarıdaki kod gösterilmektedir.

![Çokgen katman kullanarak dikey olarak uzatılmış çokgen ile eşleme](media/map-extruded-polygon-android/polygon-extrusion-layer.jpg)

## <a name="add-data-driven-polygons"></a>Veri odaklı çokgenler ekleme

Bir choroptath eşlemesi, Çokgen katman kullanılarak oluşturulabilir. , `height` `fillColor` `Polygon` Ve özellik geometrileri içindeki istatistiksel değişkenin ölçüsünün ve yer aldığı katman özelliklerini ayarlayın `MultiPolygon` . Aşağıdaki kod örneği,, popülasyon yoğunluğunun duruma göre ölçümüne göre Birleşik Devletler yükseltilmiş bir choroptath haritasını gösterir.

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Import the geojson data and add it to the data source.
Utils.importData("https://azuremapscodesamples.azurewebsites.net/Common/data/geojson/US_States_Population_Density.json", this,  (String result) -> {
    //Parse the data as a GeoJSON Feature Collection.
    FeatureCollection fc = FeatureCollection.fromJson(result);

    //Add the feature collection to the data source.
    source.add(fc);
});

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillOpacity(0.7f),
    fillColor(
        step(
            get("density"),
            literal("rgba(0, 255, 128, 1)"),
            stop(10, "rgba(9, 224, 118, 1)"),
            stop(20, "rgba(11, 191, 103, 1)"),
            stop(50, "rgba(247, 227, 5, 1)"),
            stop(100, "rgba(247, 199, 7, 1)"),
            stop(200, "rgba(247, 130, 5, 1)"),
            stop(500, "rgba(247, 94, 5, 1)"),
            stop(1000, "rgba(247, 37, 5, 1)")
        )
    ),
    height(
        interpolate(
            linear(),
            get("density"),
            stop(0, 100),
            stop(1200, 960000)
        )
    )
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

Aşağıdaki ekran görüntüsünde, ABD durumlarının bir choroptath haritasını, popülasyon yoğunluğu temelinde yükseltilmiş çokgenler olarak dikey ve genişletilmiş bir şekilde gösterir.

![ABD Devletleri 'nin tek bir haritası, popülasyon yoğunluğu temelinde yükseltilmiş çokgenler olarak renklendirilir ve dikey olarak uzatılır](media/map-extruded-polygon-android/android-extruded-choropleth.jpg)

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)
